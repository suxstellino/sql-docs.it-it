---
title: Disponibilità elevata - database OLTP in memoria
description: I database di SQL Server con tabelle ottimizzate per la memoria, con o senza stored procedure compilate native, sono completamente supportati con i gruppi di disponibilità Always On.
ms.custom: seo-dt-2019
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2113a916-3b1e-496c-8650-7f495e492510
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b275387efa5cc44b012cccef82fb3284e594abb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170543"
---
# <a name="high-availability-support-for-in-memory-oltp-databases"></a>Supporto della disponibilità elevata per i database OLTP in memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  I database che contengono tabelle ottimizzate per la memoria, con o senza stored procedure compilate native, sono completamente supportati con i gruppi di disponibilità AlwaysOn.  Non c'è alcuna differenza di configurazione e supporto per i database contenenti oggetti [!INCLUDE[hek_2](../../includes/hek-2-md.md)] rispetto a quelli che non li contengono.  

 Le modifiche alle tabelle ottimizzate per la memoria nella replica primaria vengono applicate alle tabelle nella replica secondaria durante la fase di rollforward. Questo approccio consente un failover rapido nella replica secondaria poiché i dati si trovano già in memoria. Le tabelle sono disponibili per le query di lettura nelle repliche secondarie che sono state configurate per l'accesso in lettura.  

  
## <a name="always-on-availability-groups-and-in-memory-oltp-databases"></a>Gruppi di disponibilità AlwaysOn e database OLTP in memoria  
 La configurazione dei database con componenti [!INCLUDE[hek_2](../../includes/hek-2-md.md)] offre i vantaggi seguenti:  
  
-   **Un'esperienza completamente integrata**   
    È possibile configurare i database contenenti tabelle ottimizzate per la memoria usando la stessa procedura guidata con lo stesso livello di supporto sia per le repliche secondarie asincrone sia per quelle sincrone. Inoltre, il monitoraggio dello stato viene fornito tramite il noto dashboard AlwaysOn in SQL Server Management Studio.  
  
-   **Tempo di failover confrontabile**   
    Le repliche secondarie mantengono lo stato in memoria delle tabelle durevoli ottimizzate per la memoria. In caso di failover automatico o forzato, il tempo di failover al nuovo database primario è paragonabile a quello del failover a tabelle basate su disco, in quanto non è necessario il ripristino. Le tabelle con ottimizzazione per la memoria create come SCHEMA_ONLY sono supportate in questa configurazione. Tuttavia, le modifiche a queste tabelle non vengono registrate e non esisteranno quindi dati in queste tabelle nella replica secondaria.  
  
-   **Secondario leggibile**   
    È possibile accedere ed eseguire query su tabelle ottimizzate per la memoria nella replica secondaria se questa è stata configurata per l'accesso in lettura. In [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] la sincronizzazione del timestamp di lettura sulla replica secondaria con il timestamp di lettura sulla replica primaria è molto veloce, il che significa che le modifiche apportate alla replica primaria diventano visibili rapidamente nella replica secondaria. Questo comportamento di sincronizzazione rapida è diverso rispetto a OLTP in memoria di [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

### <a name="considerations"></a>Considerazioni

- SQL Server 2019 ha introdotto la fase di rollforward parallela per i database di gruppi di disponibilità ottimizzati per la memoria. In SQL Server 2016 e 2017 le tabelle basate su disco non usano la fase di rollforward parallela se un database in un gruppo di disponibilità è anche ottimizzato per la memoria. 
  
## <a name="failover-clustering-instance-fci-and-in-memory-oltp-databases"></a>Istanza di clustering di failover e database OLTP in memoria  
 Per ottenere la disponibilità elevata in una configurazione di archiviazione condivisa, è possibile impostare un'istanza del cluster di failover con database che usano tabelle ottimizzate per la memoria. Per l'impostazione di un'istanza di clustering di failover, considerare i fattori seguenti:  
  
-   **Obiettivo tempo di ripristino**   
    Il tempo di failover è probabilmente maggiore perché le tabelle ottimizzate per la memoria devono essere caricate in memoria prima che il database venga reso disponibile.  
  
-   **Tabelle SCHEMA_ONLY**   
    Tenere presente che le tabelle SCHEMA_ONLY saranno vuote e senza righe dopo il failover. Si tratta di un comportamento previsto definito dall'applicazione. Questo comportamento corrisponde esattamente a ciò che succede quando si riavvia un database [!INCLUDE[hek_2](../../includes/hek-2-md.md)] con una o più tabelle SCHEMA_ONLY.  
  
## <a name="support-for-transaction-replication-in-in-memory-oltp"></a>Supporto per la replica transazionale in OLTP In memoria  
 Le tabelle con funzione di sottoscrittori di replica transazionale, esclusa la replica transazionale peer-to-peer, possono essere configurate come tabelle ottimizzate per la memoria. Le altre configurazioni di replica non sono compatibili con le tabelle ottimizzate per la memoria.  Per altre informazioni, vedere [Replica in sottoscrittori di tabelle con ottimizzazione per la memoria](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Gruppi di disponibilità Always On (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Repliche secondarie attive: Repliche secondarie leggibili (Gruppi di disponibilità AlwaysOn)](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Replica in sottoscrittori di tabelle con ottimizzazione per la memoria](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)  
  
  
