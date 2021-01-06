---
title: Le modifiche non sono visibili nella replica secondaria di un gruppo di disponibilità
description: Informazioni su come determinare il motivo per cui le modifiche apportate alla replica primaria non vengono rispecchiate nella replica secondaria per un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: c602fd39-db93-4717-8f3a-5a98b940f9cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8bc95e935d030c792fa4a239fa722367d1f2f1db
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641867"
---
# <a name="determine-why-changes-from-primary-replica-are-not-reflected-on-secondary-replica-for-an-always-on-availability-group"></a>Determinare il motivo per cui le modifiche apportate alla replica primaria non vengono eseguite nella replica secondaria per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  L'applicazione client completa con esito positivo un aggiornamento per la replica primaria, ma l'esecuzione di query sulla replica secondaria rivela che qui la modifica non è stata eseguita. Questo caso presume che lo stato di sincronizzazione della disponibilità sia integro. Nella maggior parte dei casi, questo comportamento si risolve automaticamente dopo pochi minuti.  
  
 Se dopo pochi minuti le modifiche non sono ancora presenti nella replica secondaria, può esserci un collo di bottiglia nel flusso di lavoro di sincronizzazione. La posizione del collo di bottiglia dipende dall'impostazione del commit della replica secondaria (sincrono o asincrono).  
  
 **Commit sincrono**  
  
 Ogni aggiornamento riuscito della replica primaria è già stato sincronizzato nella replica secondaria o i record di log sono stati già scaricati per la finalizzazione nella replica secondaria. Il collo di bottiglia deve quindi trovarsi nel processo di rollforward che si verifica dopo che il log è stato scaricato nella replica secondaria.  
  
 Dopo che la fase di rollforward è stata eseguita, tutti i carichi di lavoro di lettura nella replica secondaria sono carichi di lavoro di isolamento dello snapshot:  
  
  -   Transazioni a esecuzione prolungata nella replica primaria  
  
  -   Fase di rollforward nella replica secondaria  


**Commit asincrono**  
 
 Poiché il commit asincrono conferma una transazione non appena viene scaricata nel disco locale, il collo di bottiglia può trovarsi in qualsiasi punto dopo tale operazione:  
 
  -   Transazioni a esecuzione prolungata nella replica primaria  
  
  -   Latenza di rete o velocità effettiva  
  
  -   Finalizzazione log nella replica secondaria  
  
  -   Fase di rollforward nella replica secondaria  


Le sezioni seguenti descrivono le cause comuni della mancata copia nella replica secondaria delle modifiche alla replica primaria per le query di sola lettura.  


##  <a name="long-running-active-transactions"></a><a name="BKMK_OLDTRANS"></a> Transazioni attive con esecuzione prolungata  
 Una transazione con esecuzione prolungata nella replica primaria impedisce la lettura nella replica secondaria degli aggiornamenti.  
  
### <a name="explanation"></a>Spiegazione  
 Tutti i carichi di lavoro di lettura nella replica secondaria sono query di isolamento dello snapshot. Nell'isolamento dello snapshot, i client di sola lettura vedono il database di disponibilità nella replica secondaria in corrispondenza del punto di inizio della transazione attiva meno recente nel log sottoposto a rollforward. Se il commit di una transazione non viene eseguito per ore, la transazione aperta impedisce alle query di sola lettura di vedere tutti i nuovi aggiornamenti.  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 Nella replica primaria, usare [DBCC OPENTRAN &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-opentran-transact-sql.md) per visualizzare le transazioni attive meno recenti e verificare se è possibile eseguirne il rollback. Dopo il rollback delle transazioni attive meno recenti e la sincronizzazione di queste con la replica secondaria, i carichi di lavoro di lettura nella replica secondaria possono vedere gli aggiornamenti nel database di disponibilità fino all'inizio della transazione attiva meno recente di quel momento.  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> Una latenza elevata o una velocità effettiva bassa della rete causa un accumulo di log nella replica primaria  
 Una latenza elevata o un velocità effettiva bassa della rete può impedire un invio sufficientemente rapido dei log alla replica secondaria.  
  
### <a name="explanation"></a>Spiegazione  
 La replica primaria attiva il controllo di flusso per l'invio dei log quando ha superato il numero massimo consentito di messaggi non riconosciuti inviati alla replica secondaria. Finché alcuni di questi messaggi non vengono confermati, non possono essere inviati altri blocchi di log alla replica secondaria. Questa situazione può avere come conseguenza più grave una perdita di dati tale da compromettere l'obiettivo punto di ripristino (RPO, Recovery Point Objective).  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 Un valore DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) elevato può indicare che i registri vengono trattenuti nella replica primaria. Dividendo questo valore per [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md), è possibile ottenere subito una stima approssimativa di quanto velocemente può avvenire il recupero dei dati nella replica secondaria.  
  
 È anche utile controllare i due oggetti prestazioni [SQL Server: Replica di disponibilità > Ora controllo di flusso (ms/sec)](~/relational-databases/performance-monitor/sql-server-availability-replica.md) e [SQL Server: Replica di disponibilità > Controllo di flusso/sec](~/relational-databases/performance-monitor/sql-server-availability-replica.md). Moltiplicando questi due valori si ottiene quanto dell'ultimo secondo è stato impiegato nell'attesa della cancellazione del controllo di flusso. Maggiore il tempo di attesa del controllo di flusso, minore la velocità di invio.  
  
 Di seguito è riportato un elenco di metriche utili per diagnosticare la velocità effettiva e la latenza di rete. Per valutare l'utilizzo della rete, è possibile usare altri strumenti di Windows, ad esempio **ping.exe**.  
  
-   DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   DMV [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   Contatore delle prestazioni `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Contatore delle prestazioni `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Resent Messages/sec`  
  
 Per risolvere questo problema, provare ad aggiornare la larghezza di banda di rete o a rimuovere o ridurre traffico di rete non necessario.  
  
##  <a name="another-reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> Un altro carico di lavoro di creazione di report blocca l'esecuzione del thread di rollforward  
 L'esecuzione di modifiche DDL (Data Definition Language) del thread di rollforward nella replica secondaria viene bloccata da una query di sola lettura con esecuzione prolungata. Per poter rendere disponibili altri aggiornamenti al carico di lavoro di lettura, il thread di rollforward deve essere sbloccato.  
  
### <a name="explanation"></a>Spiegazione  
 Nella replica secondaria, le query di sola lettura acquisiscono blocchi di stabilità dello schema (`Sch-S`). Questi blocchi `Sch-S` sono in grado di impedire al thread di rollforward di acquisire blocchi di modifica dello schema (`Sch-M`) e di apportare eventuali modifiche DDL. Un thread di rollforward bloccato non può applicare record di log finché non viene sbloccato.  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 Quando il thread di rollforward è bloccato, viene generato un evento esteso denominato `sqlserver.lock_redo_blocked`. È anche possibile eseguire una query su DMV sys.dm_exec_request nella replica secondaria per scoprire quale sessione stia bloccando il thread di rollforward, per poter eseguire un'azione correttiva. La query seguente restituisce l'ID di sessione del carico di lavoro di creazione di report che sta bloccando il thread di rollforward.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 È possibile lasciar finire il carico di lavoro di creazione di report, e a questo punto il thread di rollforward viene sbloccato, oppure è possibile sbloccare il thread di rollforward immediatamente eseguendo il comando [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) sull'ID di sessione che causa il blocco.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_REDOBEHIND"></a> Il thread di rollforward è in ritardo a causa di una contesa di risorse  
 Un carico di lavoro di creazione di report di grandi dimensioni nella replica secondaria ha rallentato le prestazioni della replica secondaria stessa e il thread di rollforward è in ritardo.  
  
### <a name="explanation"></a>Spiegazione  
 Quando si applicano record di log alla replica secondaria, il thread di rollforward legge tali record dal disco dei log e quindi per ogni record di log accede alle pagine di dati corrispondenti per applicarlo. Se una pagina non è ancora nel pool di buffer, l'accesso a questa può avvenire tramite binding I/O (accesso al disco fisico). Se è presente un carico di lavoro di creazione di report che esegue il binding I/O, tale carico di lavoro si contende le risorse I/O con il thread di rollforward, rallentando quest'ultimo. Questa situazione non solo impedisce ad altri carichi di lavoro di creazione di report di vedere i dati aggiornati, ma influisce anche sull'obiettivo RTO (Recovery Time Objective, obiettivo tempo di recupero).  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 È possibile usare la query DMV seguente per vedere quanto è in ritardo il thread di rollforward, misurando il divario tra `last_redone_lsn` e `last_received_lsn`.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Se il thread di rollforward è effettivamente in ritardo, è necessario ricercare la causa radice della riduzione del livello delle prestazioni nella replica secondaria. Se è presente una contesa per le risorse I/O con il carico di lavoro di creazione di report, è possibile usare [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) per controllare i cicli della CPU usati dal carico di lavoro di creazione di report e controllare indirettamente, in una certa misura, i cicli di I/O eseguiti. Se ad esempio il carico di lavoro di creazione di report consuma il 10% della CPU ma il carico di lavoro esegue il binding I/O, è possibile usare Resource Governor per limitare l'utilizzo di risorse della CPU al 5%, limitando il carico di lavoro di lettura e riducendo al minimo l'impatto sull'I/O.  
  
## <a name="next-steps"></a>Passaggi successivi  
 [Troubleshooting performance problems in SQL Server 2008](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100)) (Risoluzione dei problemi relativi alle prestazioni in SQL Server 2008)
  
