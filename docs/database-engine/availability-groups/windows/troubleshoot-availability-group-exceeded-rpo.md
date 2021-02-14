---
title: Il gruppo di disponibilità ha superato la soglia RPO
description: Problemi comuni e soluzioni per i casi in cui il gruppo di disponibilità Always On supera l'obiettivo del punto di ripristino (RPO)
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 38de1841-9c99-435a-998d-df81c7ca0f1e
author: rothja
ms.author: jroth
ms.openlocfilehash: f70696592cae8f486e7ff7b52e417bb67edfa8d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337925"
---
# <a name="troubleshoot-availability-group-exceeded-rpo"></a>Risoluzione dei problemi: Il gruppo di disponibilità ha superato la soglia RPO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Dopo aver eseguito un failover manuale forzato su un gruppo di disponibilità a una replica secondaria con commit asincrono, è possibile che la perdita di dati sia maggiore dell'obiettivo punto di ripristino (RPO, Recovery Point Objective). Oppure, quando si calcola la perdita di dati potenziale di una replica secondaria con commit asincrono tramite il metodo in [Monitor Performance for Always On Availability Groups](monitor-performance-for-always-on-availability-groups.md) (Monitorare le prestazioni per i gruppi di disponibilità Always On), è possibile scoprire che supera l'obiettivo RPO.  
  
 Una replica secondaria con commit sincrono garantisce la totale assenza di perdite di dati, ma la perdita di dati potenziale di una replica secondaria con commit asincrono dipende dalla dimensione di log ancora in attesa di finalizzazione nella replica secondaria.  
  
 Le sezioni seguenti descrivono le cause comuni della potenzialità elevata delle perdite di dati delle repliche secondarie con commit asincrono, presupponendo che nell'istanza del server non sia presente un problema di prestazioni a livello di sistema non correlato ai gruppi di disponibilità.  
  
1.  [Una latenza elevata o una velocità effettiva bassa della rete causa un accumulo di log nella replica primaria](#BKMK_LATENCY)  
  
2.  [Un collo di bottiglia a livello di I/O del disco rallenta la finalizzazione dei log nella replica secondaria](#BKMK_IO_BOTTLENECK)  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> Una latenza elevata o una velocità effettiva bassa della rete causa un accumulo di log nella replica primaria  
 La causa più comune del superamento dell'obiettivo RPO da parte dei database è l'impossibilità di inviarli alla replica secondaria con una rapidità sufficiente.  
  
### <a name="explanation"></a>Spiegazione  
 La replica primaria attiva il controllo di flusso per l'invio dei log quando ha superato il numero massimo consentito di messaggi non riconosciuti inviati alla replica secondaria. Finché alcuni di questi messaggi non vengono confermati, non possono essere inviati altri blocchi di log alla replica secondaria. Poiché è possibile evitare la perdita di dati solo dopo che questi ultimi sono stati finalizzati nella replica secondaria, l'accumulo di messaggi di log non inviati aumenta le possibilità di perdita di dati.  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 Un numero elevato di ripetizioni di invii di messaggi alla replica secondaria può indicare una latenza di rete elevata e un alto livello di disturbo sulla rete. È anche possibile confrontare il valore DMV **log_send_rate** con l'oggetto prestazioni Log Bytes Flushed/sec. Se i log sono vengono scaricati su disco più velocemente di quanto vengano inviati, la perdita di dati potenziale può aumentare in modo indefinito.  
  
 È utile controllare i due oggetti prestazioni `SQL Server:Availability Replica > Flow Control Time (ms/sec)` e `SQL Server:Availability Replica > Flow Control/sec`. Moltiplicando questi due valori si ottiene quanto dell'ultimo secondo è stato impiegato nell'attesa della cancellazione del controllo di flusso. Maggiore il tempo di attesa del controllo di flusso, minore la velocità di invio.  
  
 Le metriche seguenti sono utili per diagnosticare la velocità effettiva e la latenza della rete. Per valutare la latenza l'utilizzo della rete, è possibile usare altri strumenti di Windows, ad esempio **ping.exe** e [Network Monitor](https://www.microsoft.com/p/network-monitor-pro-free-edition/9n8gdvj32gp7).  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_queue_size`  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_rate`  
  
-   Contatore delle prestazioni `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Contatore delle prestazioni `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Contatore delle prestazioni `SQL Server:Availability Replica > Resent Messages/sec`  

Per risolvere questo problema, provare ad aggiornare la larghezza di banda di rete o a rimuovere o ridurre traffico di rete non necessario.  


##  <a name="disk-io-bottleneck-slows-down-log-hardening-on-the-secondary-replica"></a><a name="BKMK_IO_BOTTLENECK"></a> Un collo di bottiglia a livello di I/O del disco rallenta la finalizzazione dei log nella replica secondaria  
 A seconda della distribuzione dei file di database, la finalizzazione dei log può rallentare a causa di una contesa I/O con il carico di lavoro di creazione di report.  
  
### <a name="explanation"></a>Spiegazione  
 La perdita di dati viene evitata non appena il blocco del log viene finalizzato nel file di log. Di conseguenza, è fondamentale isolare il file di log dal file di dati. Se il file di log e il file di dati sono entrambi mappati allo stesso disco rigido, un carico di lavoro di creazione di report con una frequenza intensiva di operazioni di lettura sul file di dati utilizzerà le stesse risorse di I/O necessarie per l'operazione di finalizzazione del log. Una finalizzazione del log lenta può tradursi in una conferma lenta alla replica primaria, che può causare un'attivazione eccessiva del controllo di flusso e lunghi tempi di attesa di quest'ultimo.  
  
### <a name="diagnosis-and-resolution"></a>Diagnosi e risoluzione  
 Dopo aver verificato che la rete non abbia un problema di latenza elevata o di bassa velocità effettiva, è necessario analizzare la replica secondaria per individuare eventuali contese di I/O. Le query in [SQL Server: Riduzione dell'I/O del disco](/previous-versions/technet-magazine/jj643251(v=msdn.10)) sono utili per identificare le contese. Per comodità, di seguito sono riportati alcuni esempi tratti da questo articolo.  
  
 Lo script seguente consente di visualizzare il numero di operazioni di lettura e scrittura in ogni file di dati e di log per ogni database di disponibilità in esecuzione in un'istanza di SQL Server. L'elenco è ordinato per tempo di blocco I/O medio in millisecondi. Si noti che le cifre rappresentano il totale dall'ultimo avvio dell'istanza del server. È pertanto necessario rilevare la differenza tra due misure dopo un certo periodo di tempo.  
  
```sql  
SELECT DB_NAME(database_id) AS   
   [Database Name] ,   
   file_id ,   
   io_stall_read_ms ,   
   num_of_reads ,   
   CAST(io_stall_read_ms / ( 1.0 + num_of_reads ) AS NUMERIC(10, 1)) AS [avg_read_stall_ms] ,   
   io_stall_write_ms ,   
   num_of_writes ,  
   CAST(io_stall_write_ms / ( 1.0 + num_of_writes ) AS NUMERIC(10, 1)) AS [avg_write_stall_ms] ,   
   io_stall_read_ms + io_stall_write_ms AS [io_stalls] ,   
   num_of_reads + num_of_writes AS [total_io] ,   
   CAST(( io_stall_read_ms + io_stall_write_ms ) / ( 1.0 + num_of_reads  
+ num_of_writes) AS NUMERIC(10,1)) AS [avg_io_stall_ms]  
FROM sys.dm_io_virtual_file_stats(NULL, NULL)  
WHERE DB_NAME(database_id) IN (SELECT DISTINCT database_name FROM sys.dm_hadr_database_replica_cluster_states)  
ORDER BY avg_io_stall_ms DESC;  
```  
  
 La query successiva fornisce uno snapshot (non cumulativo) temporizzato di richieste I/O in sospeso nel sistema.  
  
```sql  
SELECT DB_NAME(mf.database_id) AS [Database] ,   
   mf.physical_name ,  
   r.io_pending ,   
   r.io_pending_ms_ticks ,   
   r.io_type ,   
   fs.num_of_reads ,   
   fs.num_of_writes  
FROM sys.dm_io_pending_io_requests AS r   
INNER JOIN sys.dm_io_virtual_file_stats(NULL, NULL) AS fs ON r.io_handle = fs.file_handle   
INNER JOIN sys.master_files AS mf ON fs.database_id = mf.database_id  
AND fs.file_id = mf.file_id  
ORDER BY r.io_pending , r.io_pending_ms_ticks DESC;  
```  
  
 Per identificare una contesa I/O, è possibile confrontare il modo in cui le operazioni I/O rispettivamente di lettura e di scrittura corrispondono le une alle altre.  
  
 Di seguito sono riportati alcuni altri contatori delle prestazioni in grado di semplificare la diagnosi dei colli di bottiglia I/O:  
  
-   **Disco fisici: tutti i contatori**  
  
-   **Disco fisico: Avg. secondi/trasf. disco**  
  
-   **SQL Server: Database > Tempo di attesa scaricamento log**  
  
-   **SQL Server: Database > Attese scaricamento log/sec**  
  
-   **SQL Server: Database > Letture disco del pool di log/sec**  
  
 Se si identifica un collo di bottiglia I/O e il file di log e il file di dati si trovano nello stesso disco rigido, la prima cosa da fare è posizionarli in dischi separati. Questa procedura consigliata evita che il carico di lavoro di creazione di report interferisca con il percorso di trasferimento del log dalla replica primaria al buffer dei log e con la finalizzazione della transazione nella replica secondaria.  
  
## <a name="next-steps"></a>Passaggi successivi  
 [Troubleshooting performance problems in SQL Server (applies to SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100)) (Risoluzione dei problemi di prestazioni in SQL Server (si applica a SQL Server 2012))  
  
