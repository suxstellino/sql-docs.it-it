---
title: 'Risoluzione dei problemi: Il gruppo di disponibilità ha superato la soglia RTO (SQL Server) | Microsoft Docs'
description: Informazioni su come risolvere i problemi relativi a un failover in un gruppo di disponibilità Always On quando il failover richiede più tempo dell'obiettivo del tempo di ripristino in SQL Server.
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: e83e4ef8-92f0-406f-bd0b-dc48dc210517
author: rothja
ms.author: jroth
ms.openlocfilehash: 5847162edb0d962826931624f4d14a3d78468066
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641877"
---
# <a name="troubleshoot-availability-group-exceeded-rto"></a>Risoluzione dei problemi: Il gruppo di disponibilità ha superato la soglia RTO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Dopo un failover automatico o un failover manuale pianificato senza perdita di dati in un gruppo di disponibilità, è possibile che il tempo di failover superi l'obiettivo tempo di ripristino (RTO, Recovery Time Objective). Oppure, quando si stima il tempo di failover di una replica secondaria con commit asincrono (ad esempio un partner di failover automatico) tramite il metodo in [Monitor Performance for Always On Availability Groups](monitor-performance-for-always-on-availability-groups.md) (Monitorare le prestazioni per i gruppi di disponibilità Always On), è possibile scoprire che supera l'obiettivo RTO.  
  
 Se il failover automatico non è ancora completato, vedere [Risoluzione dei problemi di failover automatico in ambienti SQL Server 2012 AlwaysOn](https://support.microsoft.com/kb/2833707).  
  
 Le sezioni seguenti descrivono le cause comuni del superamento dell'obiettivo RTO da parte del tempo di failover.  
  
1.  [Un carico di lavoro di creazione di report blocca l'esecuzione del thread di rollforward](#BKMK_REDOBLOCK)  
  
2.  [Il thread di rollforward è in ritardo a causa di una contesa di risorse](#BKMK_CONTENTION)  
  
##  <a name="reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> Un carico di lavoro di creazione di report blocca l'esecuzione del thread di rollforward  
 L'esecuzione di modifiche DDL (Data Definition Language) del thread di rollforward nella replica secondaria viene bloccata da una query di sola lettura con esecuzione prolungata.  
  
### <a name="explanation"></a>Spiegazione  
 Nella replica secondaria, le query di sola lettura acquisiscono blocchi di stabilità dello schema (`Sch-S`). Questi blocchi `Sch-S` sono in grado di impedire al thread di rollforward di acquisire blocchi di modifica dello schema (`Sch-M`) e di apportare eventuali modifiche DDL. Un thread di rollforward bloccato non può applicare record di log finché non viene sbloccato. Dopo lo sblocco, può continuare ad aggiornarsi rispetto alla fine del log e consentire l'esecuzione del processo di rollback e di failover successivo.  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 Quando il thread di rollforward è bloccato, viene generato un evento esteso denominato `sqlserver.lock_redo_blocked`. È anche possibile eseguire una query su DMV sys.dm_exec_request nella replica secondaria per scoprire quale sessione stia bloccando il thread di rollforward, per poter eseguire un'azione correttiva. La query seguente restituisce l'ID di sessione della query di sola lettura che sta bloccando il thread di rollforward.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 È possibile lasciar finire il carico di lavoro di creazione di report, e a questo punto il thread di rollforward viene sbloccato, oppure è possibile sbloccare il thread di rollforward immediatamente eseguendo il comando [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) sull'ID di sessione che causa il blocco.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_CONTENTION"></a> Il thread di rollforward è in ritardo a causa di una contesa di risorse  
 Un carico di lavoro di creazione di report di grandi dimensioni nella replica secondaria ha rallentato le prestazioni della replica secondaria stessa e il thread di rollforward è in ritardo.  
  
### <a name="explanation"></a>Spiegazione  
 Quando si applicano record di log alla replica secondaria, il thread di rollforward legge tali record dal disco dei log e quindi per ogni record di log accede alle pagine di dati corrispondenti per applicarlo. Se una pagina non è ancora nel pool di buffer, l'accesso a questa può avvenire tramite binding I/O (accesso al disco fisico). Se è presente un carico di lavoro di creazione di report che esegue il binding I/O, tale carico di lavoro si contende le risorse I/O con il thread di rollforward, rallentando quest'ultimo.  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 È possibile usare la query DMV seguente per vedere quanto è in ritardo il thread di rollforward, misurando il divario tra `last_redone_lsn` e `last_received_lsn`.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Se il thread di rollforward è effettivamente in ritardo, è necessario ricercare la causa radice della riduzione del livello delle prestazioni nella replica secondaria. Se è presente una contesa per le risorse I/O con il carico di lavoro di creazione di report, è possibile usare [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) per controllare i cicli della CPU usati dal carico di lavoro di creazione di report e controllare indirettamente, in una certa misura, i cicli di I/O eseguiti. Se ad esempio il carico di lavoro di creazione di report consuma il 10% della CPU ma il carico di lavoro esegue il binding I/O, è possibile usare Resource Governor per limitare l'utilizzo di risorse della CPU al 5%, limitando il carico di lavoro di lettura e riducendo al minimo l'impatto sull'I/O.  
  
## <a name="next-steps"></a>Passaggi successivi  
 [Troubleshooting performance problems in SQL Server (applies to SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100)) (Risoluzione dei problemi di prestazioni in SQL Server (si applica a SQL Server 2012))  
  
