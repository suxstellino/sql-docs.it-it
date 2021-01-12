---
description: cdc.change_tables (Transact-SQL)
title: cdc.change_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc.change_tables
- cdc.change_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.change_tables
ms.assetid: 3525a5f5-8d8b-46a8-b334-4b7cd9fb7c21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28da42d6fbf03a01dc1c0719bd62a6270cb6525e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091509"
---
# <a name="cdcchange_tables-transact-sql"></a>cdc.change_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni tabella delle modifiche del database. Una tabella delle modiche viene creata quando l'acquisizione dei dati delle modifiche è abilitata in una tabella di origine. È consigliabile non eseguire una query direttamente sulle tabelle di sistema. Eseguire invece il [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) stored procedure.  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID della tabella delle modifiche. Valore univoco all'interno di un database.|  
|**version**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], questa colonna restituisce sempre un valore 0.|  
|**source_object_id**|**int**|ID della tabella di origine abilitata per l'acquisizione dei dati delle modifiche.|  
|**capture_instance**|**sysname**|Nome dell'istanza di acquisizione utilizzata per denominare gli oggetti per il rilevamento specifici dell'istanza. Per impostazione predefinita, il nome deriva dal nome dello schema di origine più il nome della tabella di origine nel formato *schemaname_sourcename*.|  
|**start_lsn**|**binary(10)**|Numero di sequenza del file di log (LSN) che rappresenta l'endpoint inferiore quando si esegue una query sui dati delle modifiche nella tabelle delle modifiche.<br /><br /> NULL = l'endpoint inferiore non è stato stabilito.|  
|**end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] questa colonna restituisce sempre un valore NULL.|  
|**supports_net_changes**|**bit**|Il supporto per l'esecuzione di una query sulle modifiche totali è abilitato per la tabella delle modifiche.|  
|**has_drop_pending**|**bit**|Il processo di acquisizione ha ricevuto la notifica che la tabella di origine è stata eliminata.|  
|**role_name**|**sysname**|Nome del ruolo del database utilizzato per controllare l'accesso ai dati delle modifiche.<br /><br /> NULL = non è utilizzato un ruolo.|  
|**index_name**|**sysname**|Nome dell'indice utilizzato per identificare in modo univoco le righe nella tabella di origine. **index_name** è il nome dell'indice di chiave primaria della tabella di origine o il nome di un indice univoco specificato quando Change Data Capture è stato abilitato nella tabella di origine.<br /><br /> NULL = la tabella di origine non ha avuto una chiave primaria quando l'acquisizione dei dati delle modifiche è stata abilitata e un indice univoco non è stato specificato quando l'acquisizione dei dati delle modifiche è stata abilitata.<br /><br /> Nota: Se Change Data Capture è abilitato in una tabella in cui esiste una chiave primaria, la funzionalità Change Data Capture usa l'indice indipendentemente dall'abilitazione o meno delle modifiche nette. Dopo l'abilitazione di Change Data Capture, sulla chiave primaria non è consentita alcuna modifica. Se nella tabella non è presente una chiave primaria, è ancora possibile abilitare la funzionalità Change Data Capture, ma solo se le modifiche totali sono impostate su false. Dopo l'abilitazione di Change Data Capture, è possibile creare una chiave primaria. È inoltre possibile modificare la chiave primaria perché la funzionalità Change Data Capture non la utilizza.|  
|**filegroup_name**|**sysname**|Nome del database filegroup contenente la tabella delle modifiche specificata.<br /><br /> NULL = la tabella delle modifiche si trova nel filegroup predefinito del database.|  
|**create_date**|**datetime**|Data in cui la tabella di origine è stata abilitata.|  
|**partition_switch**|**bit**|Indica se il comando **Switch Partition** di **ALTER TABLE** può essere eseguito su una tabella abilitata per Change Data Capture. 0 indica che il cambio della partizione viene bloccato. Tramite le tabelle non partizionate viene restituito sempre 1.|  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
