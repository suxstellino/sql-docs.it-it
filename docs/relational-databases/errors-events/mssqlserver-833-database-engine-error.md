---
description: MSSQLSERVER_833
title: MSSQLSERVER_833 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 833 (Database Engine error)
ms.assetid: 14129cc4-be80-4772-9e3f-0e5da4d0696b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d562e3ffcde77c706d0cf56a92f188b895eeb457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197877"
---
# <a name="mssqlserver_833"></a>MSSQLSERVER_833
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sql-asdbmi.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|833|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|BUF_LONG_IO|  
|Testo del messaggio|Rilevate %d occorrenze di richieste di I/O che impiegano più di %d secondi per essere completate nel file [%ls] nel database `[%ls] (%d)`.  L'handle di file del sistema operativo è 0x%p.  Offset dell'ultimo I/O lungo: %#016I64x.|  
  
## <a name="explanation"></a>Spiegazione  
Questo messaggio indica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha eseguito una richiesta di lettura o scrittura dal disco e che la durata dell'operazione è stata superiore a 15 secondi. Questo errore viene segnalato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e indica un problema relativo al sottosistema di I/O. Si potrebbero notare anche altri sintomi associati a questo messaggio: tempi di attesa elevati per PAGEIOLATCH, avvisi o errori nel registro eventi di sistema, indicazioni di problemi di latenza del disco nei contatori di monitoraggio del sistema. Monitorare [sys.dm_io_virtual_file_stats](../system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) e scegliere il livello di archiviazione e il numero di operazioni di I/O al secondo appropriati per la velocità effettiva di archiviazione. 
  
### <a name="possible-causes"></a>Possibili cause  
Il problema può essere causato da problemi di prestazioni del sistema operativo, errori hardware, errori del firmware, problemi del driver del dispositivo, dall'intervento del driver di filtro nel processo di I/O o dal percorso di archiviazione dei file di database. SQL Server registra la data e l'ora di inizio di una richiesta di I/O e registra l'ora di completamento dell'I/O. Se la differenza è di 15 secondi o più, viene rilevata questa condizione. Ciò significa anche che SQL Server non è la causa di una condizione di I/O ritardato descritta e segnalata da questo messaggio. Questa condizione è nota come "I/O in stallo". La maggior parte delle richieste del disco si verificano entro la velocità tipica del disco. Questa velocità tipica del disco è spesso nota come "tempo di ricerca del disco". Il tempo di ricerca del disco per la maggior parte dei dischi standard è pari a 10 millisecondi o meno. Di conseguenza, 15 secondi è un tempo molto lungo di attesa che il percorso di I/O di sistema torni a SQL Server. 
  
## <a name="user-action"></a>Azione dell'utente  
Per risolvere il problema che ha causato questo errore, individuare nel registro eventi di sistema i messaggi di errore correlati all'hardware. Esaminare inoltre eventuali log specifici dei componenti hardware se disponibili. È consigliabile usare i metodi e le tecniche necessari per determinare la causa del ritardo nel sistema operativo, con i driver o con l'hardware di I/O. La risoluzione di questo problema potrebbe comportare l'aggiornamento di tutti i driver di dispositivo e il firmware o l'esecuzione di altre operazioni di diagnostica associate al sistema del disco. 
  
Utilizzare Performance Monitor per esaminare i contatori seguenti:  
  
-   **Media secondi/trasf. disco**  
  
-   **Lunghezza media coda del disco**  
  
-   **Lunghezza corrente coda del disco**  
  
Il valore della durata media, ad esempio, di **Disco sec/trasferimento** in un computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in genere inferiore a 15 millisecondi. L'aumento del valore di **Media secondi/trasf. disco** indica che il sottosistema di I/O non è in grado di soddisfare in modo ottimale le richieste di I/O.

È anche possibile usare funzionalità come la [registrazione ETW di Storport](/archive/blogs/ntdebugging/storport-etw-logging-to-measure-requests-made-to-a-disk-unit) per misurare la latenza delle richieste effettuate a un'unità disco. Un altro kit di risoluzione dei problemi di I/O su disco simile è disponibile come profilo predefinito di [Windows Performance Recorder](/windows-hardware/test/wpt/introduction-to-wpr).
  
> [!NOTE]  
> L'accesso al disco può essere rallentato da un programma antivirus. Per aumentare la velocità di accesso, escludere dalle analisi virus attive i file di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificati nel messaggio di errore. È possibile usare l'[utilità della riga di comando fltmc.exe](/windows-hardware/drivers/ifs/development-and-testing-tools#fltmcexe-control-program) per eseguire una query su tutti i driver di filtro installati nel sistema e per comprendere le funzioni eseguite sul percorso di archiviazione dei file di database. 
  
Per altre informazioni sugli errori di I/O, vedere [Microsoft SQL Server I/O Basics, Chapter 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)) (Nozioni di base sull'I/O in Microsoft SQL Server, capitolo 2) e l'articolo della Knowledge Base all'indirizzo [https://support.microsoft.com/kb/897284/en-us](https://support.microsoft.com/kb/897284/en-us).  
