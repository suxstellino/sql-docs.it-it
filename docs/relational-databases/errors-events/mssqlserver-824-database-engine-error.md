---
description: MSSQLSERVER_824
title: MSSQLSERVER_824 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 824 (Database Engine error)
ms.assetid: 2aa22246-2712-4fdb-9744-36e7e6f3175e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 031a478a281538b096d0900ad80fa34741182269
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180307"
---
# <a name="mssqlserver_824"></a>MSSQLSERVER_824
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|824|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|B_HARDSSERR|  
|Testo del messaggio|SQL Server ha rilevato un errore di I/O logico legato alla consistenza. Errore %1!s! durante un'operazione di %S_MSG della pagina %S_PGID nel database con ID %d all'offset %#016I64x nel file '%ls'.  Per informazioni più dettagliate, vedere i messaggi aggiuntivi nel log degli errori di SQL Server e nel registro eventi di sistema.|  
  
## <a name="symptom"></a>Sintomo  


È possibile che venga visualizzato il messaggio di errore seguente nel log degli errori di SQL Server o nel registro eventi dell'applicazione di Windows se una verifica coerenza logica ha esito negativo dopo la lettura o la scrittura in una pagina del database:
 
``` 
2009-11-02 15:46:42.90 spid51      Error: 824, Severity: 24, State: 2.
2009-11-02 15:46:42.90 spid51      SQL Server detected a logical consistency-based I/O error: incorrect pageid (expected 1:43686; actual 0:0). It occurred during a read of page (1:43686) in database ID 23 at offset 0x0000001554c000 in file 'H:\MSSQL.SQL2008\MSSQL\DATA\my_db.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```
 
Se questo messaggio viene visualizzato in un'applicazione durante la query o la modifica dei dati, il messaggio di errore viene restituito all'applicazione e la connessione di database viene terminata. 
  
## <a name="cause"></a>Causa
Questo errore indica che, sebbene Windows segnali che la pagina è stata correttamente letta dal disco, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha individuato un errore nella pagina. Questo errore è simile all'errore 823, con la sola differenza che Windows non ha rilevato l'errore e indica in genere un problema relativo al sottosistema di I/O, ad esempio un'unità disco danneggiata, problemi relativi al firmware del disco, un driver di dispositivo difettoso e così via. Per altre informazioni sugli errori di I/O, vedere il capitolo 2 della pagina relativa alle [nozioni fondamentali sull'I/O in Microsoft SQL Server](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).  

SQL Server usa le API di Windows, ad esempio ReadFile, WriteFile, ReadFileScatter, WriteFileGather, per eseguire operazioni di I/O. Dopo l'esecuzione di queste operazioni di I/O, SQL Server controlla se a queste chiamate API sono associate condizioni di errore. Se queste chiamate API hanno esito negativo con un errore del sistema operativo, SQL Server restituisce l'errore 823. In alcune situazioni la chiamata API di Windows ha esito positivo, ma è possibile che per i dati trasferiti dall'operazione di I/O si sia verificato un problema di coerenza logica. Questi problemi di coerenza logica vengono segnalati tramite l'errore 824.
 
L'errore 824 contiene le informazioni seguenti:

- Il file di database su cui viene eseguita l'operazione di I/O
- L'offset con il file in cui è stata tentata l'operazione di I/O
- Database a cui appartiene il file
- Numero di pagina coinvolto nell'operazione di I/O
- Se l'operazione è stata di lettura o di scrittura
- Dettagli sulla verifica coerenza logica non riuscita (il tipo di controllo, il valore effettivo e il valore previsto usati per questa verifica)
 
Queste verifiche coerenza logica sono controlli di integrità aggiuntivi eseguiti da SQL Server per assicurarsi che alcuni aspetti chiave dei dati coinvolti nel trasferimento dei dati siano stati mantenuti durante l'operazione di I/O. I controlli includono checksum, pagina incompleta, trasferimento breve, ID pagina non valido, lettura non aggiornata, errore controllo pagina. La natura dei controlli eseguiti varia a seconda delle diverse opzioni di configurazione a livello di database e di server. 
 
Il messaggio di errore 824 indica in genere che si è verificato un problema con il sistema di archiviazione sottostante o con l'hardware oppure con un driver che si trova nel percorso della richiesta di I/O. Questo errore può verificarsi se sono presenti incoerenze nel file system o se il file di database è danneggiato.

## <a name="resolution"></a>Risoluzione  

Se si verifica l'errore 824, è possibile provare le risoluzioni seguenti: 

- Esaminare la tabella [suspect_pages](../backup-restore/manage-the-suspect-pages-table-sql-server.md) in msdb per controllare se questo problema si sta verificando anche in altre pagine (nello stesso database o in database diversi).
- Verificare la coerenza dei database presenti nello stesso volume (come quello riportato nel messaggio 824) usando il comando DBCC CHECKDB. Se vengono rilevate incoerenze dal comando DBCC CHECKDB, usare le linee guida disponibili nell'articolo della Knowledge Base [Risoluzione degli errori di coerenza del database segnalati da DBCC CHECKDB](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check).
- Se nel database in cui si verificano questi errori 824 non è attivata l'opzione di database PAGE_VERIFY CHECKSUM, attivarla immediatamente. Gli errori 824 possono verificarsi per motivi diversi da un errore di checksum, ma CHECKSUM costituisce l'opzione migliore per verificare la coerenza della pagina dopo che è stata scritta su disco.
- Esaminare i registri eventi di Windows per individuare eventuali errori o messaggi segnalati dal sistema operativo oppure da un dispositivo di archiviazione o un driver di dispositivo. Se in qualche modo sono correlati a questo errore, risolvere prima tali errori. Ad esempio, oltre al messaggio 824, è possibile notare un evento come "Il driver ha rilevato un errore del controller in \Device\Harddisk4\DR4" segnalato dall'origine disco nel registro eventi. In tal caso, è necessario valutare se il file è presente nel dispositivo e quindi correggere gli errori del disco rilevati.
- Usare l'utilità [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) per verificare se questi errori 824 possono essere riprodotti al di fuori delle normali richieste di I/O di SQL Server. SQLIOSim viene fornita con SQL Server 2008, quindi non è necessario un download separato in questa versione o in quelle successive.
- Collaborare con il fornitore dell'hardware o il produttore del dispositivo per verificare quanto segue:
   - I dispositivi hardware e la configurazione sono conformi ai [requisiti di I/O di SQL Server](https://support.microsoft.com/help/967576/microsoft-sql-server-database-engine-input-output-requirements).
   - I driver di dispositivo e altri componenti software di supporto di tutti i dispositivi nel percorso di I/O sono aggiornati.
- Se il fornitore dell'hardware o il produttore del dispositivo ha fornito utilità di diagnostica, usarle per valutare l'integrità del sistema di I/O.
- Valutare se sono presenti driver di filtro nel percorso di queste richieste di I/O in cui si verificano problemi.
   - Controllare se sono presenti aggiornamenti per questi driver di filtro
   - I driver di filtro possono essere rimossi o disabilitati per verificare se il problema che causa l'errore 824 è stato risolto
- Se il problema non è correlato all'hardware ed è disponibile un backup valido noto, ripristinare il database dal backup.  

## <a name="see-also"></a>Vedere anche  
[Gestione della tabella suspect_pages &#40;SQL Server&#41;](~/relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
