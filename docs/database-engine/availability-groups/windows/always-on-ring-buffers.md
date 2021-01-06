---
title: Buffer circolari per le informazioni sull'integrità dei gruppi di disponibilità
description: Si possono ottenere alcune informazioni di diagnostica sui gruppi di disponibilità Always On usando i buffer circolari di SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 47bb7a1a-c0a5-473c-a7db-d9f4bf3ee650
author: rothja
ms.author: jroth
ms.openlocfilehash: 1feb461ad05167b4669da2ee53c17342bba98195
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641438"
---
# <a name="use-ring-buffers-to-obtain-health-information-about-always-on-availability-groups"></a>Usare buffer circolari per ottenere informazioni sull'integrità dei gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Alcune informazioni diagnostiche sui gruppi di disponibilità Always On possono essere ottenute dai buffer circolari di SQL Server o dalla vista a gestione dinamica (DMV) sys.dm_os_ring_buffers. I buffer circolari vengono creati all'avvio di SQL Server e registrano gli avvisi all'interno del sistema SQL Server per la diagnostica interna. Nonostante non siano supportati, è comunque possibile estrarre informazioni utili da essi durante le attività risoluzione dei problemi. I buffer circolari rappresentano un'altra origine di dati diagnostici a cui ricorrere quando SQL Server si blocca o si arresta in modo anomalo.  
  
 La query Transact-SQL (T-SQL) seguente recupera tutti i record degli eventi dai buffer circolari dei gruppi di disponibilità.  
  
```sql  
SELECT * FROM sys.dm_os_ring_buffers WHERE ring_buffer_type LIKE '%HADR%'  
```  
  
 Per rendere i dati più gestibili, filtrare i dati in base alla data e al tipo di buffer circolare. La query seguente recupera i record dal buffer circolare specificato che si è verificato oggi.  
  
```sql  
DECLARE @runtime datetime  
SET @runtime = GETDATE()  
SELECT CONVERT (varchar(30), @runtime, 121) as data_collection_runtime,   
DATEADD (ms, -1 * (inf.ms_ticks - ring.[timestamp]), GETDATE()) AS ring_buffer_record_time,   
ring.[timestamp] AS record_timestamp, inf.ms_ticks AS cur_timestamp, ring.*   
FROM sys.dm_os_ring_buffers ring  
CROSS JOIN sys.dm_os_sys_info inf where ring_buffer_type='<RING_BUFFER_TYPE>'  
```  
  
 La colonna Record di ogni record contiene i dati di diagnostica in formato XML. I dati XML variano a seconda dei tipi di buffer circolare. Per altre informazioni sui tipi di buffer circolari, vedere [Tipi di buffer circolari dei gruppi di disponibilità](#BKMK_RingBufferTypes). Per rendere più leggibili i dati XML, è necessario personalizzare la query T-SQL in modo che estragga gli elementi XML desiderati. Ad esempio, la query seguente recupera tutti gli eventi dal buffer circolare RING_BUFFER_HADRDBMGR_API e formatta i dati XML creando colonne di tabella separate.  
  
```sql  
WITH hadr(ts, type, record) AS  
(  
  SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
  FROM sys.dm_os_ring_buffers WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API'  
)  
SELECT   
  ts,  
  type,  
  record.value('(./Record/@id)[1]','bigint') AS [Record ID],  
  record.value('(./Record/@time)[1]','bigint') AS [Time],  
  record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [DBID],  
  record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
  record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
  record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
  record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
##  <a name="availability-groups-ring-buffer-types"></a><a name="BKMK_RingBufferTypes"></a> Tipi di buffer circolari dei gruppi di disponibilità  
 In sys.dm_os_ring_buffers esistono quattro buffer circolari dei gruppi di disponibilità. La tabella seguente descrive i tipi di buffer circolari e propone un esempio del contenuto della colonna Record per ogni tipo di buffer circolare.  
  
 **RING_BUFFER_HADRDBMGR_API**  
  
 Transizioni di stato del record che sono avvenute o che stanno avvenendo. Quando si esaminano le transizioni di stato prestare particolare attenzione ai valori objectType.  
  
```xml  
<Record id="11" type="RING_BUFFER_HADRDBMGR_STATE" time="860243">  
  <HadrDbMgrState>  
    <objectType>HadrUsers</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>1</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_STATE**  
  
 Registra le chiamate interne di funzione o al metodo effettuate dall'attività Always On. Possono essere visualizzate informazioni come sospensioni, riprese o modifiche dei ruoli, inclusi i punti di ingresso e uscita.  
  
```xml  
<Record id="45" type="RING_BUFFER_HADRDBMGR_STATE" time="1723487912">  
  <HadrDbMgrState>  
    <dbId>5</dbId>  
    <objectType>HadrDbMgr</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>2</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_COMMIT**  
  
```xml  
<Record id="0" type="RING_BUFFER_HADRDBMGR_COMMIT" time="1723475368">  
  <HadrDbMgrCommitPolicy>  
    <dbId>5</dbId>  
    <replicaId>883a18f5-97d5-450f-8f8f-9983a4fa5299</replicaId>  
    <dbHardenPolicy>KillAll</dbHardenPolicy>  
    <dbSyncConfig>0x0</dbSyncConfig>  
    <syncPartnerCount>0</syncPartnerCount>  
    <minSyncPartnerConfig>0</minSyncPartnerConfig>  
    <partnerHardenPolicy>KillAll</partnerHardenPolicy>  
    <partnerSyncConfig>0x0</partnerSyncConfig>  
    <logBlock>0x0000000000000000</logBlock>  
    <leaseExpired>Y</leaseExpired>  
    <partnerChange>N</partnerChange>  
    <role>2</role>  
  </HadrDbMgrCommitPolicy>  
</Record>  
```  
  
 **RING_BUFFER_HADR_TRANSPORT_STATE**  
  
```xml  
<Record id="3" type="RING_BUFFER_HADR_TRANSPORT_STATE" time="1723485399">  
  <HadrTransportState>  
    <agId>08264B79-D10B-412F-B38D-CA07B08E9BD8</agId>  
    <localArId>883A18F5-97D5-450F-8F8F-9983A4FA5299</localArId>  
    <targetArId>628D6349-72DD-4D18-A6E1-1272645660BA</targetArId>  
    <currentState>HadrSession_Configuring</currentState>  
    <targetState>HadrSession_Connected</targetState>  
    <legalTransition>Y</legalTransition>  
  </HadrTransportState>  
</Record>  
```  
  
## <a name="parse-xml-data-from-a-ring-buffer"></a>Analizzare i dati XML da un buffer circolare  
 È possibile analizzare il campo Record del buffer circolare se si sta controllando con una query che contiene il [Metodo value&#40;&#41; &#40;Tipo di dati xml&#41;](~/t-sql/xml/value-method-xml-data-type.md) . Per utilizzare questo metodo, è necessario applicare prima [CAST](~/t-sql/functions/cast-and-convert-transact-sql.md) alla colonna Record del buffer circolare in XML. Ad esempio, la query seguente dimostra come analizzare RING_BUFFER_HADRDBMGR_API in formato leggibile utilizzando questo metodo.  
  
```sql 
WITH hadr(ts, type, record) AS  
   (SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
FROM sys.dm_os_ring_buffers   
WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API')  
SELECT ts,  
type,  
record.value('(./Record/@id)[1]','bigint') AS [Record id],  
record.value('(./Record/@time)[1]','bigint') AS [Time],  
record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [dbid],  
record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
  
