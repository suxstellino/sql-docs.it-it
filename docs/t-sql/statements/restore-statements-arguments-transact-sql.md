---
description: 'Istruzioni RESTORE: argomenti (Transact-SQL)'
title: Argomenti dell'istruzione RESTORE (Transact-SQL)
ms.custom: ''
ms.date: 08/08/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE statement, arguments
- RESTORE statement
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 39057d8708fba5df5610b3ffd481849771266683
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813075"
---
# <a name="restore-statements---arguments-transact-sql"></a>Istruzioni RESTORE: argomenti (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo articolo illustra gli argomenti descritti nelle sezioni "Sintassi" dell'istruzione RESTORE {DATABASE|LOG} e del set di istruzioni ausiliarie associato: RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY, RESTORE REWINDONLY e RESTORE VERIFYONLY. La maggior parte degli argomenti sono supportati solo da un subset di queste sei istruzioni. Informazioni dettagliate sono contenute nella descrizione dell'argomento.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
 Per informazioni sulla sintassi, vedere gli argomenti seguenti:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 DATABASE  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica il database di destinazione. Se viene specificato un elenco di file e filegroup, vengono ripristinati solo i file e filegroup dell'elenco.  
  
 Per un database impostato per l'utilizzo del modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nella maggior parte dei casi è necessario eseguire il backup della parte finale del log prima di ripristinare il database. Se si esegue il ripristino di un database senza aver prima eseguito il backup della parte finale del log si verificherà un errore, a meno che nell'istruzione RESTORE DATABASE non sia inclusa la clausola WITH REPLACE o WITH STOPAT, che deve consentire di specificare l'ora o la transazione verificatasi al termine del backup dei dati. Per altre informazioni sui backup della parte finale del log, vedere [Backup della parte finale del log &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 LOG  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica che è necessario applicare un backup del log delle transazioni al database. I log delle transazioni devono essere applicati in ordine sequenziale. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito un controllo del log delle transazioni di cui è stato eseguito il backup per verificare che le transazioni vengano caricate nel database corretto e nella sequenza corretta. Per applicare più log delle transazioni, utilizzare l'opzione NORECOVERY in tutte le operazioni di ripristino, ad eccezione dell'ultima.  
  
> [!NOTE]  
>  In genere, l'ultimo log ripristinato è il backup della parte finale del log. Il termine *backup della parte finale del log* indica un backup del log creato appena prima del ripristino di un database, in genere in seguito a un errore del database. La creazione di un backup della parte finale del log per il database potenzialmente danneggiato consente di evitare perdite di dati, grazie all'acquisizione della sezione del log per cui non è ancora disponibile una copia di backup, ovvero la parte finale del log. Per altre informazioni, vedere [Backup della parte finale del log &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 Per altre informazioni, vedere [Applicazione dei backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
 { _database\_name_ |  **@** _database\_name\_var_}  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Database in cui viene ripristinato il log o il database completo. Se indicato in forma di variabile ( **@** _database\_name\_var_), questo nome può essere specificato come costante stringa ( **@** _database\_name\_var_ = *database*\_*name*) oppure come variabile di tipo stringa di caratteri, ad eccezione del tipo di dati **ntext** o **text**.  
  
 \<file_or_filegroup_or_page> [ **,** ...*n* ]  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica il nome di un file, una pagina o un filegroup logico da includere in un'istruzione RESTORE DATABASE o RESTORE LOG. È possibile specificare un elenco di file o filegroup.  
  
 In un database che usa il modello di recupero con registrazione minima, le opzioni FILE e FILEGROUP sono consentite solo se i file o i filegroup di destinazione sono di sola lettura o se si tratta di un ripristino PARTIAL, il cui risultato è un [filegroup inattivo](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md).  
  
 Per i database impostati per l'utilizzo del modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, dopo l'utilizzo dell'istruzione RESTORE DATABASE per il ripristino di uno o più file, filegroup e/o pagine, è in genere necessario applicare il log delle transazioni ai file contenenti i dati ripristinati. L'applicazione del log consente di rendere tali file consistenti con il resto del database. Esistono le eccezioni seguenti:  
  
-   Se i file in corso di ripristino erano di sola lettura prima dell'ultimo backup, non è necessario applicare il log delle transazioni e l'istruzione RESTORE segnala questa situazione.  
  
-   Se il backup contiene il filegroup primario e viene eseguito un ripristino parziale. In questo caso, l'applicazione del log non è necessaria perché il log viene ripristinato automaticamente dal set di backup.  
  
FILE **=** { *logical_file_name_in_backup*|  **@** _logical\_file\_name\_in\_backup\_var_}  
 Specifica il nome di un file da includere nell'operazione di ripristino del database.  
  
FILEGROUP **=** { *logical_filegroup_name* |  **@** _logical\_filegroup\_name\_var_ }  
 Specifica il nome di un filegroup da includere nell'operazione di ripristino del database.  
  
 **Nota** L'argomento FILEGROUP è consentito nel modello di recupero con registrazione minima solo se il filegroup specificato è di sola lettura e se viene eseguito un ripristino parziale, ovvero se si usa la clausola WITH PARTIAL. Tutti i filegroup di lettura/scrittura non ripristinati vengono contrassegnati come inattivi e non possono pertanto essere ripristinati nel database risultante  
  
READ_WRITE_FILEGROUPS  
 Seleziona tutti i filegroup di lettura/scrittura. Questa opzione risulta particolarmente utile quando sono presenti filegroup di sola lettura che si desidera ripristinare dopo i filegroup di lettura/scrittura.  
  
PAGE = **'** _file_ **:** _page* [ **,** ...*n* ] **'**  
 Specifica un elenco di una o più pagine per un ripristino della pagina, supportato solo nei database che utilizzano i modelli di recupero con registrazione completa o con registrazione minima delle operazioni bulk. Sono disponibili i valori seguenti:  
  
PAGE  
 Indica un elenco di uno o più file e pagine.  
  
 *file*  
 ID del file contenente una pagina specifica da ripristinare.  
  
 *page*  
 ID della pagina da ripristinare nel file.  
  
 *n*  
 Segnaposto che indica la possibilità di specificare più pagine.  
  
 In un singolo file di una sequenza di ripristino è possibile ripristinare al massimo 1000 pagine. Tuttavia, se il numero di pagine danneggiate in un file è consistente, è consigliabile valutare la possibilità di ripristinare l'intero file anziché le singole pagine.  
  
> [!NOTE]  
>  Le operazioni di recupero della pagina non vengono mai eseguite.  
  
 Per altre informazioni sul ripristino della pagina, vedere [Ripristino di pagine &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).  
  
 [ **,** ...*n* ]  
 Segnaposto che indica la possibilità di specificare più file, filegroup e pagine in un elenco delimitato da virgole. Il numero di file e filegroup che è possibile specificare è illimitato.  
  
FROM { \<backup_device> [ **,** ...*n* ]| \<database_snapshot> } In genere, specifica i dispositivi di backup da cui eseguire il ripristino del backup. In alternativa, in un'istruzione RESTORE DATABASE la clausola FROM può specificare il nome di uno snapshot del database in cui eseguire il ripristino del database e in questo caso la clausola WITH non è consentita.  
  
 Se la clausola FROM viene omessa, non viene eseguito il ripristino del backup, ma il recupero del database. Ciò consente di recuperare un database ripristinato con l'opzione NORECOVERY oppure di passare a un server di standby. Se si omette la clausola FROM, è necessario specificare l'opzione NORECOVERY, RECOVERY o STANDBY nella clausola WITH.  
  
 \<backup_device> [ **,** ...*n* ] Specifica i dispositivi di backup logici o fisici da utilizzare per l'operazione di ripristino.  
  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md), [RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 \<backup_device>::= Specifica il dispositivo di backup logico o fisico da usare per l'operazione di backup, come indicato di seguito:  
  
 { _logical\_backup\_device\_name_ |  **@** _logical\_backup\_device\_name\_var_ }  
 Nome logico, conforme alle regole per gli identificatori, del o dei dispositivi di backup creati tramite **sp_addumpdevice** da cui viene ripristinato il database. Se indicato in forma di variabile ( **@** _logical\_backup\_device\_name\_var_), il nome del dispositivo di backup può essere specificato come costante stringa ( **@** _logical\_backup\_device\_name\_var_ = _logical\_backup\_device\_name_) o come variabile con tipo di dati stringa di caratteri, ad eccezione dei tipi di dati **ntext** o **text**.  
  
 {DISK | TAPE } **=** { **'** _physical\_backup\_device\_name_ **'**  |  **@** _physical\_backup\_device\_name\_var_ }  
 Consente di ripristinare i backup dal dispositivo disco o nastro specificato. I tipi di dispositivo disco e nastro devono essere specificati con il nome effettivo (ad esempio percorso completo e nome di file) del dispositivo: `DISK ='Z:\SQLServerBackups\AdventureWorks.bak'` o `TAPE ='\\\\.\TAPE0'`. Se indicato come variabile ( **@** _physical\_backup\_device\_name\_var_), il nome del dispositivo può essere specificato come costante stringa ( **@** _physical\_backup\_device\_name\_var_ = '*physical_backup_device_name*') oppure come variabile con tipo di dati stringa di caratteri, ad eccezione dei tipi di dati **ntext** o **text**.  
  
 Se si utilizza un server di rete avente un nome UNC (che deve contenere il nome del server), specificare un dispositivo disco. Per altre informazioni su come usare i nomi UNC, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 Per poter eseguire un'operazione RESTORE, l'account utilizzato per l'esecuzione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve disporre dell'autorizzazione di accesso READ per il computer remoto o il server di rete.  
  
 *n*  
 Segnaposto che indica la possibilità di specificare fino a 64 dispositivi di backup in un elenco delimitato da virgole.  
  
 Per una sequenza di ripristino, la necessità o meno di utilizzare lo stesso numero di dispositivi di backup utilizzato per la creazione del set di supporti cui appartengono i backup dipende dal tipo di ripristino, ovvero se si tratta di un ripristino in linea o non in linea.  
  
-   Il ripristino offline consente il ripristino di un backup utilizzando un numero minore di dispositivi di backup rispetto a quelli utilizzati per creare il backup.  
  
-   Il ripristino online richiede tutti i dispositivi di backup del backup. Non è possibile eseguire il ripristino con un numero inferiore di dispositivi.  
  
 Si consideri, ad esempio, il caso del backup di un database eseguito su quattro unità nastro connesse al server. Per un ripristino in linea è necessario che le quattro unità siano connesse al server, mentre per un ripristino non in linea è possibile eseguire l'operazione anche se nel computer sono disponibili meno di quattro unità.  
  
> [!NOTE]  
>  Per eseguire il ripristino di un backup da un set di supporti con mirroring, è possibile specificare un solo mirror per ogni gruppo di supporti. In presenza di errori, tuttavia, la disponibilità degli altri mirror può consentire una risoluzione rapida di alcuni problemi di ripristino. È possibile sostituire un volume di un supporto danneggiato con il volume corrispondente da un altro mirror. Per i ripristini non in linea, è possibile eseguire il ripristino da un numero minore di dispositivi che di gruppi di supporti, ma ogni gruppo viene elaborato una sola volta.  
  
\<database_snapshot>::=  
**Supportato da:**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
DATABASE_SNAPSHOT **=** _database\_snapshot\_name_  
 Ripristina il database usando lo snapshot del database specificato da *database_snapshot_name*. L'opzione DATABASE_SNAPSHOT è disponibile solo per un ripristino di database completo. In un'operazione di ripristino di questo tipo, lo snapshot del database sostituisce un backup completo del database.  
  
 Per eseguire un'operazione di ripristino è necessario che lo snapshot del database specificato sia l'unico esistente per il database. Durante l'operazione, sia lo snapshot del database che il database di destinazione vengono contrassegnati come `In restore`. Per altre informazioni, vedere la sezione "Osservazioni" in [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="with-options"></a>Opzioni WITH  
 Specifica le opzioni da utilizzare per un'operazione di ripristino. Per informazioni di riepilogo sull'utilizzo delle singole opzioni in ogni istruzione, vedere "Riepilogo del supporto delle opzioni WITH" di seguito in questo argomento.  
  
> [!NOTE]  
>  Le opzioni WITH sono organizzate nello stesso ordine usato nella sezione "Sintassi" in [RESTORE {DATABASE|LOG}](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 PARTIAL  
 **Supportato da:**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica un'operazione di ripristino parziale che consente di ripristinare solo il filegroup primario e i filegroup secondari specificati. L'opzione PARTIAL consente di selezionare il filegroup primario in modo implicito. Non è necessario specificare FILEGROUP = 'PRIMARY'. Per ripristinare un filegroup secondario, è necessario specificare in modo esplicito il filegroup tramite l'opzione FILE o FILEGROUP.  
  
 L'opzione PARTIAL non è consentita per istruzioni RESTORE LOG.  
  
 L'opzione PARTIAL consente di avviare la fase iniziale di un ripristino a fasi che consente il ripristino dei filegroup rimanenti in un momento successivo. Per altre informazioni, vedere [Ripristini a fasi &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
 [ **RECOVERY** | NORECOVERY | STANDBY ]  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 **RECOVERY**  
 Imposta l'operazione di ripristino in modo che venga eseguito il rollback di tutte le transazioni di cui non è stato eseguito il commit. Dopo il processo di recupero, il database è pronto per essere utilizzato. Se non si specifica NORECOVERY, RECOVERY o STANDBY, il valore predefinito è RECOVERY.  
  
 Se si prevede di dover eseguire operazioni RESTORE successive (RESTORE LOG o RESTORE DATABASE da un backup differenziale), è invece necessario specificare NORECOVERY o STANDBY.  
  
 Per il ripristino di set di backup da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], potrebbe essere necessario un aggiornamento del database. Tale aggiornamento viene eseguito automaticamente quando si specifica l'opzione WITH RECOVERY. Per altre informazioni, vedere [Applicazione dei backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
> [!NOTE]  
>  Se si omette la clausola FROM, è necessario specificare l'opzione NORECOVERY, RECOVERY o STANDBY nella clausola WITH.  
  
 NORECOVERY  
 Imposta l'operazione di ripristino in modo che non venga eseguito il rollback delle transazioni di cui non è stato eseguito il commit. Se si prevede di dover applicare un altro log delle transazioni in seguito, specificare l'opzione NORECOVERY o STANDBY. Se non si specifica NORECOVERY, RECOVERY o STANDBY, il valore predefinito è RECOVERY. Durante un'operazione di ripristino non in linea con l'opzione NORECOVERY, il database non è utilizzabile.  
  
 Per il ripristino di un backup di database e di uno o più log delle transazioni oppure in tutti i casi in cui sono necessarie più istruzioni RESTORE (ad esempio per il ripristino di un backup di database completo seguito da un backup differenziale del database), è necessario specificare l'opzione WITH NORECOVERY per tutte le istruzioni RESTORE ad eccezione dell'ultima. È consigliabile utilizzare l'opzione WITH NORECOVERY per TUTTE le istruzioni in una sequenza di ripristino in più passaggi fino al raggiungimento del punto di recupero desiderato, quindi utilizzare un'istruzione RESTORE WITH RECOVERY separata solo per il recupero.  
  
 Se si utilizza l'opzione NORECOVERY per un'operazione di ripristino di un file o filegroup, il database viene mantenuto forzatamente nello stato di ripristino dopo l'operazione di ripristino. Ciò risulta utile in una delle situazioni seguenti:  
  
-   Viene eseguito uno script di ripristino e il log viene sempre applicato.  
  
-   Viene utilizzata una sequenza di operazioni di ripristino di file e si desidera che il database non venga utilizzato tra due operazioni di ripristino.  
  
 In alcuni casi, l'istruzione RESTORE WITH NORECOVERY esegue il rollforward del set di rollforward fino al punto di renderlo consistente con il database. In questi casi il rollback non viene eseguito e i dati rimangono offline, come previsto con questa opzione. [!INCLUDE[ssDE](../../includes/ssde-md.md)], tuttavia, genera un messaggio informativo indicante che il set di rollforward è pronto per essere recuperato utilizzando l'opzione RECOVERY.  
  
STANDBY **=** _standby\_file\_name_  
 Specifica il nome del file standby che consente di eseguire il rollback degli effetti del recupero. L'opzione STANDBY è consentita per il ripristino non in linea, inclusi ripristini parziali. Non è invece consentita per il ripristino in linea. Se si tenta di specificare l'opzione STANDBY per un'operazione di ripristino in linea, l'operazione avrà esito negativo. L'opzione STANDBY non è inoltre consentita quando è necessario l'aggiornamento del database.  
  
 Il file standby viene utilizzato per memorizzare una pre-immagine "copy-on-write" per le pagine modificate durante la fase di rollback di un'istruzione RESTORE WITH STANDBY. Il file standby consente di attivare un database per l'accesso in sola lettura tra due operazioni di ripristino del log delle transazioni e può essere utilizzato in server warm standby o in situazioni di recupero speciali in cui risulta utile controllare il database tra operazioni di ripristino del log successive. Dopo il completamento di un'operazione RESTORE WITH STANDBY, il file di rollback viene eliminato automaticamente dalla successiva operazione RESTORE. Se il file standby viene eliminato manualmente prima della successiva operazione RESTORE, sarà necessario ripetere il ripristino dell'intero database. Mentre il database è in stato STANDBY, è consigliabile gestire questo file standby con la stessa cautela utilizzata per qualsiasi altro file del database. A differenza degli altri file di database, questo file viene mantenuto aperto da [!INCLUDE[ssDE](../../includes/ssde-md.md)] solo durante le operazioni di ripristino attive.  
  
 Il parametro *standby_file_name* specifica un file standby il cui percorso viene archiviato nel log del database. Se il nome specificato è già utilizzato da un file esistente, quest'ultimo viene sovrascritto. In caso contrario, il file viene creato da [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Le dimensioni richieste per un determinato file standby dipendono dal volume delle azioni di rollback risultanti dalle transazioni di cui non è stato eseguito il commit, rilevate durante l'operazione di ripristino.  
  
> [!IMPORTANT]  
>  Se lo spazio disponibile nell'unità contenente il file standby specificato diventa insufficiente, l'operazione di ripristino viene arrestata.  
  
 Per informazioni sulle differenze tra le opzioni RECOVERY e NORECOVERY, vedere la sezione "Osservazioni" nell'argomento relativo all'istruzione [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
LOADHISTORY  
 **Supportato da:**  [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Specifica che l'operazione di ripristino carica le informazioni nelle tabelle della cronologia di **msdb**. Per il singolo set di backup in corso di verifica, l'opzione LOADHISTORY consente di caricare le informazioni sui backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] archiviati nel set di supporti nelle tabelle della cronologia di backup e di ripristino del database **msdb**. Per altre informazioni sulle tabelle della cronologia, vedere [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md).  
  
#### <a name="general_with_options--n-"></a>\<general_WITH_options> [ ,...n ]  
 Le opzioni WITH generali sono tutte supportate nelle istruzioni RESTORE DATABASE e RESTORE LOG. Alcune di queste opzioni sono supportate anche da una o più istruzioni ausiliarie, come indicato di seguito.  
  
##### <a name="restore-operation-options"></a>Opzioni relative all'operazione di ripristino  
 Queste opzioni influiscono sul comportamento dell'operazione di ripristino.  
  
MOVE **'** _logical\_file\_name\_in\_backup_ **'** TO **'** _operating\_system\_file\_name_ **'** [ ...*n* ]  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Specifica che il file di dati o di log il cui nome logico è specificato da *logical_file_name_in_backup* deve essere spostato ripristinandolo nel percorso specificato da *operating_system_file_name*. Il nome di file logico di un file di dati o di log in un set di backup corrisponde al relativo nome logico nel database al momento della creazione del set di backup.  
  
*n* è un segnaposto che indica la possibilità di specificare istruzioni MOVE aggiuntive. Specificare un'istruzione MOVE per ogni file logico che si desidera ripristinare dal set di backup in un nuovo percorso. Per impostazione predefinita, il file *logical_file_name_in_backup* viene ripristinato nel percorso originale.  
  
> [!NOTE]  
>  Per ottenere un elenco dei file logici del set di backup, utilizzare l'istruzione RESTORE FILELISTONLY.  
  
 Se si utilizza l'istruzione RESTORE per spostare un database nello stesso server o copiarlo in un server diverso, potrebbe essere necessario utilizzare l'opzione MOVE per spostare i file di database ed evitare eventuali collisioni con i file esistenti.  
  
 In combinazione con l'istruzione RESTORE LOG, l'opzione MOVE può essere utilizzata solo per lo spostamento di file aggiunti durante l'intervallo coperto dal log in corso di ripristino. Ad esempio, se il backup del log contiene un'operazione di aggiunta per il file `file23`, questo file può essere spostato utilizzando l'opzione MOVE nell'istruzione RESTORE LOG.  
  
 Se usata con il backup dello snapshot di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'opzione MOVE può essere usata solo per rilocare i file in un BLOB di Azure all'interno dello stesso account di archiviazione del BLOB originale. L'opzione MOVE non può essere usata per ripristinare il backup dello snapshot in un file locale o in un account di archiviazione diverso.  
  
 Se si utilizza l'istruzione RESTORE VERIFYONLY per spostare un database nello stesso server o copiarlo in un server diverso, potrebbe essere necessario utilizzare l'opzione MOVE per verificare che lo spazio disponibile nella destinazione sia sufficiente e per individuare potenziali collisioni con i file esistenti.  
  
 Per altre informazioni, vedere [Copiare database tramite backup e ripristino](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
CREDENTIAL  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 e versioni successive
  
 Usato solo quando si ripristina un backup dal servizio Archiviazione BLOB di Microsoft Azure.  
  
> [!NOTE]  
>  Con [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 fino a [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], durante il ripristino da URL è possibile eseguire il ripristino solo da un singolo dispositivo. Per eseguire il ripristino da più dispositivi durante il ripristino da URL, è necessario usare [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] o versione successiva ed è necessario usare i token di firma di accesso condiviso (SAS). Per altre informazioni, vedere [Abilitare il backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md) e [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Semplificazione della creazione di credenziali SQL con token di firma di accesso condiviso (SAS) in Archiviazione di Azure con Powershell).  
  
 REPLACE  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve creare il database specificato e i file correlati anche se esiste già un database avente lo stesso nome. In questo caso, il database esistente viene eliminato. Se non si specifica l'opzione REPLACE, viene eseguito un controllo di sicurezza. Questa operazione impedisce la sovrascrittura accidentale di un database diverso. Tale controllo assicura che l'istruzione RESTORE DATABASE non ripristini il database nel server corrente se:  
  
-   Il database specificato nell'istruzione RESTORE esiste già nel server e  
  
-   Il nome del database è diverso da quello del database registrato nel set di backup  
  
 L'opzione REPLACE consente inoltre di sovrascrivere i file esistenti di cui non è possibile verificare l'appartenenza al database da ripristinare. L'istruzione RESTORE in genere non sovrascrive i file esistenti. L'opzione WITH REPLACE può essere utilizzata nello stesso modo per l'opzione RESTORE LOG.  
  
 Se si utilizza l'opzione REPLACE, è possibile ignorare il requisito che richiede la creazione del backup della parte finale del log prima di eseguire il ripristino del database.  
  
 Per informazioni sull'impatto dell'uso dell'opzione REPLACE, vedere [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
RESTART  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica che in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere riavviata un'operazione di ripristino interrotta. L'operazione di ripristino viene riavviata dal punto in cui è stata interrotta.  
  
RESTRICTED_USER  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Limita l'accesso al database ripristinato ai membri dei ruoli **db_owner**, **dbcreator** e **sysadmin**.  RESTRICTED_USER sostituisce l'opzione DBO_ONLY. In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] l'opzione DBO_ONLY non è più supportata.  
  
 Utilizzare sempre l'opzione RECOVERY.  
  
##### <a name="backup-set-options"></a>Opzioni relative al set di backup  
 Queste opzioni riguardano il set di backup contenente il backup da ripristinare.  
  
FILE **=** { *backup_set_file_number* |  **@** _backup\_set\_file\_number_ }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Identifica il set di backup da ripristinare. Il valore *1* per **backup_set_file_number** indica il primo set di backup nel supporto di backup, mentre il valore *2* per **backup_set_file_number** indica il secondo set di backup. È possibile ottenere il valore *backup_set_file_number* di un backup usando l'istruzione [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
 Se l'argomento non viene specificato, il valore predefinito è **1**, con l'eccezione di RESTORE HEADERONLY. In questo caso vengono infatti elaborati tutti i set di backup nel set di supporti. Per ulteriori informazioni, vedere "Specifica di un set di backup" di seguito in questo argomento.  
  
> [!IMPORTANT]  
>  Questa opzione FILE non è correlata all'opzione FILE usata per specificare un file di database, FILE **=** { *logical_file_name_in_backup* |  **@** _logical\_file\_name\_in\_backup\_var_ }.  
  
 PASSWORD  **=** { *password* |  **@** _password\_variable_ }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Specifica la password per il set di backup. La password di un set di backup è una stringa di caratteri.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Se durante la creazione del set di backup è stata impostata una password, sarà necessario specificare tale password per eseguire qualsiasi operazione di ripristino dal set di backup. Viene considerata un errore l'impostazione della password errata o la specifica di una password per un set di backup per il quale non è stata impostata.  
  
> [!IMPORTANT]  
>  Il livello di protezione del set di supporti garantito da questa password è ridotto. Per ulteriori informazioni, vedere la sezione Autorizzazioni per l'istruzione specifica.  
  
##### <a name="media-set-options"></a>Opzioni relative ai set di supporti  
 Queste opzioni vengono applicate all'intero set di supporti.  
  
 MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_}  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Specifica il nome del supporto. Se specificato, il nome deve corrispondere al nome del supporto nei volumi di backup. In caso contrario, l'operazione di ripristino viene interrotta. Se non viene specificato alcun nome di supporto nell'istruzione RESTORE, non viene eseguita la ricerca di un nome di supporto corrispondente nei volumi di backup.  
  
> [!IMPORTANT]  
>  Un utilizzo coerente dei nomi dei supporti nelle operazioni di backup e ripristino rappresenta un ulteriore controllo di sicurezza dei supporti selezionati per l'operazione di ripristino.  
  
 MEDIAPASSWORD **=** { *mediapassword* |  **@** _mediapassword\_variable_ }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Specifica la password per il set di supporti. La password di un set di supporti è una stringa di caratteri.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Se durante la formattazione del set di supporti è stata impostata una password, è necessario specificare tale password per accedere a qualsiasi set di backup nel set di supporti. Viene considerata un errore l'impostazione della password errata o la specifica di una password per un set di supporti per il quale non è stata impostata.  
  
> [!IMPORTANT]  
>  Il livello di protezione del set di supporti garantito da questa password è ridotto. Per ulteriori informazioni, vedere la sezione "Autorizzazioni" per l'istruzione specifica.  
  
 BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica le dimensioni fisiche del blocco, in byte. Le dimensioni supportate sono 512, 1024, 2048, 4096, 8192, 16384, 32768 e 65536 (64 KB) byte. Il valore predefinito è 65536 per i dispositivi nastro e 512 negli altri casi. Questa opzione non è in genere necessaria, poiché le dimensioni del blocco appropriate per il dispositivo vengono selezionate automaticamente. L'impostazione esplicita delle dimensioni del blocco ha la priorità sulla selezione automatica.  
  
 Se si esegue il ripristino di un backup da un CD-ROM, specificare BLOCKSIZE=2048.  
  
> [!NOTE]  
>  Questa opzione influisce in genere sulle prestazioni solo durante la lettura da dispositivi nastro.  
  
##### <a name="data-transfer-options"></a>Opzioni di trasferimento dei dati  
 Queste opzioni consentono di ottimizzare il trasferimento dei dati dal dispositivo di backup.  
  
 BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica il numero totale di buffer di I/O da utilizzare per l'operazione di ripristino. È possibile specificare qualsiasi numero intero positivo. Un numero elevato di buffer può tuttavia causare errori di memoria insufficiente dovuti a spazio degli indirizzi virtuali non adeguato nel processo Sqlservr.exe.  
  
 Lo spazio totale usato dai buffer viene determinato da: _buffercount_* *\** _maxtransfersize_.  
  
 MAXTRANSFERSIZE **=** { _maxtransfersize_ |  **@** _maxtransfersize\_variable_ }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Specifica le dimensioni massime, espresse in byte, per il trasferimento tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il supporto di backup. I valori possibili sono i multipli di 65536 byte (64 KB) fino a 4194304 byte (4 MB).  
> [!NOTE]  
>  Se il database ha configurato FILESTREAM o include gruppi di file OLTP in memoria, al momento del ripristino il valore di `MAXTRANSFERSIZE` deve essere maggiore o uguale a quello usato durante la creazione del backup.  
  
##### <a name="error-management-options"></a>Opzioni relative alla gestione degli errori  
 Queste opzioni consentono di specificare se i checksum del backup sono abilitati per l'operazione di ripristino e se l'operazione verrà arrestata in caso di errore.    
  
 { CHECKSUM | NO_CHECKSUM }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Per impostazione predefinita, vengono verificati i valori di checksum, se presenti, e l'operazione procede senza verifiche se non sono presenti.  
  
 CHECKSUM  
 Specifica che è richiesta la verifica dei valori di checksum del backup. Se per il backup non sono disponibili valori di checksum, l'operazione di ripristino viene interrotta con un messaggio che indica che i checksum non sono presenti.  
  
> [!NOTE]  
>  I valori di checksum per la pagina sono relativi alle operazioni di backup solo se vengono utilizzati i valori di checksum del backup.  
  
 Per impostazione predefinita, se viene individuato un valore di checksum non valido, l'istruzione RESTORE segnala un errore checksum e viene arrestata. Tuttavia, se si specifica CONTINUE_AFTER_ERROR, l'operazione RESTORE continua dopo la restituzione di un errore checksum e del numero di pagina contenente il valore di checksum non valido, nel caso la condizione di errore lo permetta.  
  
 Per altre informazioni sull'uso di checksum di backup, vedere [Possibili errori relativi ai supporti durante il backup e il ripristino &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
 NO_CHECKSUM  
 Disabilita in modo esplicito la convalida dei valori di checksum durante l'operazione di ripristino.  
  
 { **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 STOP_ON_ERROR  
 Specifica che l'operazione di ripristino deve essere arrestata dopo il rilevamento del primo errore. Questo è il funzionamento predefinito dell'istruzione RESTORE, con l'eccezione dell'argomento VERIFYONLY per il quale l'impostazione predefinita è CONTINUE_AFTER_ERROR.  
  
 CONTINUE_AFTER_ERROR  
 Specifica che l'operazione di ripristino deve continuare dopo il rilevamento di un errore.  
  
 Se un backup contiene pagine danneggiate, è consigliabile ripetere l'operazione di ripristino usando un backup alternativo senza errori, ad esempio una copia di backup creata prima del danneggiamento delle pagine. Come ultima risorsa, tuttavia, è possibile ripristinare un backup danneggiato utilizzando l'opzione CONTINUE_AFTER_ERROR dell'istruzione di ripristino e tentare di recuperare i dati.  
  
##### <a name="filestream-options"></a>Opzioni FILESTREAM  
 FILESTREAM ( DIRECTORY_NAME =*directory_name* )  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive
  
 Nome di directory compatibile con Windows. Il nome deve essere univoco in tutti i nomi di directory FILESTREAM a livello di database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il confronto di univocità supporta la distinzione tra maiuscole e minuscole, indipendentemente dalle impostazioni delle regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##### <a name="monitoring-options"></a>Opzioni di monitoraggio  
 Queste opzioni consentono di monitorare il trasferimento dei dati dal dispositivo di backup.  
  
 STATS [ **=** _percentage_ ]  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Visualizza un messaggio ogni volta che viene completata la percentuale specificata. Consente di tenere traccia dello stato di avanzamento dell'operazione. Se si omette *percentage*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] visualizza un messaggio al completamento di ogni 10% circa dell'operazione.  
  
 L'opzione STATS segnala la percentuale di completamento in base alla soglia specificata per l'intervallo successivo. Si tratta approssimativamente della percentuale specificata. Con l'impostazione STATS=10, ad esempio, [!INCLUDE[ssDE](../../includes/ssde-md.md)] visualizza un messaggio in corrispondenza di tale intervallo e l'opzione potrebbe visualizzare il 43% anziché il 40% esatto. Per i set di backup di grandi dimensioni ciò non rappresenta un problema, in quanto la percentuale di completamento aumenta molto lentamente tra le varie chiamate di I/O completate.  
  
##### <a name="tape-options"></a>Opzioni relative ai dispositivi nastro  
 Queste opzioni vengono utilizzate solo per i dispositivi nastro. Se non si utilizza un dispositivo nastro, queste opzioni vengono ignorate.  
  
 { **REWIND** | NOREWIND }  
 Queste opzioni vengono utilizzate solo per i dispositivi nastro. Se non si utilizza un dispositivo nastro, queste opzioni vengono ignorate.  
  
 REWIND  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Specifica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve rilasciare e riavvolgere il nastro. REWIND è l'opzione predefinita.  
  
 NOREWIND  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Se si specifica l'opzione NOREWIND in qualsiasi altra istruzione di ripristino verrà generato un errore.  
  
 Specifica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve mantenere il nastro aperto dopo l'operazione di backup. È possibile utilizzare questa opzione per migliorare le prestazioni durante l'esecuzione di più operazioni di backup sullo stesso nastro.  
  
 L'opzione NOREWIND implica l'utilizzo di NOUNLOAD e queste opzioni non sono compatibili all'interno di una singola istruzione RESTORE.  
  
> [!NOTE]  
>  Se si utilizza l'opzione NOREWIND, l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene la proprietà dell'unità nastro fino a quando un'istruzione BACKUP o RESTORE eseguita nello stesso processo non utilizza l'opzione REWIND o UNLOAD oppure fino alla chiusura dell'istanza del server. Ciò impedisce ad altri processi di accedere al nastro. Per informazioni sulla visualizzazione dell'elenco dei nastri aperti e sulla chiusura di un nastro aperto, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 { **UNLOAD** | NOUNLOAD }  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md), [RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md) e [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Queste opzioni vengono utilizzate solo per i dispositivi nastro. Se non si utilizza un dispositivo nastro, queste opzioni vengono ignorate.  
  
> [!NOTE]  
>  UNLOAD/NOUNLOAD è un'impostazione di sessione che rimane valida per l'intera durata della sessione o finché non viene reimpostata tramite la specifica di un'impostazione alternativa.  
  
 UNLOAD  
 Specifica che il nastro viene riavvolto e scaricato automaticamente al termine del backup. L'opzione UNLOAD è l'impostazione predefinita all'avvio di una sessione.  
  
 NOUNLOAD  
 Specifica che dopo l'operazione RESTORE il nastro rimane caricato sull'unità nastro.  
  
#### <a name="replication_with_option"></a><replication_WITH_option>  
 Questa opzione è pertinente solo se è stata eseguita la replica del database al momento della creazione del backup.  
  
 KEEP_REPLICATION  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
Quando si configura la replica per l'uso del log shipping, è consigliabile usare l'opzione KEEP_REPLICATION. Questa opzione impedisce la rimozione delle impostazioni di replica quando un backup del database o del log viene ripristinato in un server warm standby e il database viene recuperato. Non è consentito utilizzare questa opzione per il ripristino di un backup con l'opzione NORECOVERY. Per garantire il corretto funzionamento della replica dopo il ripristino:  
  
-   I database **msdb** e **master** nel server warm standby devono essere sincronizzati con i database **msdb** e **master** nel server primario.  
  
-   È necessario rinominare il server warm standby per utilizzare lo stesso nome del server primario.  
  
#### <a name="change_data_capture_with_option"></a><change_data_capture_WITH_option>  
 Questa opzione è pertinente solo se il database è stato abilitato per Change Data Capture al momento della creazione del backup.  
  
 KEEP_CDC  
 **Supportato da:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 L'opzione KEEP_CDC deve essere utilizzata per impedire la rimozione delle impostazioni di Change Data Capture quando un backup del database o del log viene ripristinato in un altro server e il database viene recuperato. Non è consentito utilizzare questa opzione per il ripristino di un backup con l'opzione NORECOVERY.  
  
 Se si ripristina il database con l'opzione KEEP_CDC, non vengono creati processi di Change Data Capture. Per estrarre le modifiche dal log dopo aver ripristinato il database, ricreare il processo di acquisizione e il processo di pulizia per il database ripristinato. Per informazioni, vedere [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).  
  
 Per informazioni sull'uso di Change Data Capture con il mirroring del database, vedere [Change Data Capture e altre funzionalità di SQL Server](../../relational-databases/track-changes/change-data-capture-and-other-sql-server-features.md).  
  
#### \<service_broker_WITH_options>  
 Attiva o disattiva il recapito dei messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] o imposta un nuovo identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Questa opzione è pertinente solo se [!INCLUDE[ssSB](../../includes/sssb-md.md)] è stato abilitato (attivato) per il database al momento della creazione del backup.  
  
 { ENABLE_BROKER  | ERROR_BROKER_CONVERSATIONS  | NEW_BROKER }  
 **Supportato da:**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 ENABLE_BROKER  
 Specifica che il recapito dei messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] deve essere abilitato al termine del ripristino in modo che i messaggi possano essere inviati immediatamente. Per impostazione predefinita, durante un'operazione di ripristino il recapito dei messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è disabilitato. Nel database viene mantenuto l'identificatore di Service Broker esistente.  
  
 ERROR_BROKER_CONVERSATIONS  
 Termina tutte le conversazioni e restituisce un errore che indica che il database è collegato o ripristinato. Questo consente alle applicazioni di eseguire operazioni regolari di pulizia per le conversazioni esistenti. Il recapito dei messaggi di Service Broker viene disabilitato fino al termine dell'operazione e quindi viene riabilitato. Nel database viene mantenuto l'identificatore di Service Broker esistente.  
  
 NEW_BROKER  
 Specifica che al database deve essere assegnato un nuovo identificatore di Service Broker. Poiché il database viene considerato una nuova istanza di Service Broker, tutte le conversazioni esistenti nel database vengono rimosse immediatamente senza generare messaggi di fine dialogo. Tutte le route che fanno riferimento all'identificatore di Service Broker precedente devono essere ricreate con il nuovo identificatore.  
  
#### \<point_in_time_WITH_options>  
 **Supportato da:**  [RESTORE {DATABASE|LOG}](../../t-sql/statements/restore-statements-transact-sql.md) e solo per i modelli di recupero con registrazione minima delle operazioni bulk o con registrazione completa.  
  
 Per ripristinare un database fino a uno specifico punto nel tempo o fino a una specifica transazione, indicare il punto di recupero di destinazione in una clausola STOPAT, STOPATMARK o STOPBEFOREMARK. Un'ora o una transazione specifica viene sempre ripristinata da un backup del log. In ogni istruzione RESTORE LOG della sequenza di ripristino, è necessario specificare l'ora o la transazione di destinazione in una clausola STOPAT, STOPATMARK o STOPBEFOREMARK identica.  
  
 Come prerequisito per un ripristino temporizzato, è necessario innanzitutto ripristinare un backup completo del database il cui endpoint sia precedente rispetto al punto di recupero di destinazione. Per consentire l'identificazione del backup del database da ripristinare, è possibile specificare la clausola WITH STOPAT, STOPATMARK o STOPBEFOREMARK in un'istruzione RESTORE DATABASE per generare un errore se un backup dei dati è troppo recente rispetto alla data e ora specifica di destinazione. Il backup completo dei dati viene però sempre ripristinato, anche se contiene la data e ora specifica di destinazione.  
  
> [!NOTE]  
>  Le opzioni WITH di temporizzazione RESTORE_DATABASE e RESTORE_LOG sono simili, ma solo RESTORE LOG supporta l'argomento *mark_name*.  
  
 { STOPAT | STOPATMARK | STOPBEFOREMARK }   
 
 STOPAT **=** { **'** _datetime_ **'**  |  **@** _datetime\_var* }  
 Specifica che il database deve essere ripristinato nello stesso stato in cui si trovava alla data e all'ora specificate dal parametro *datetime* o **@** _datetime\_var_. Per informazioni sull'indicazione della data e dell'ora, vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Se per STOPAT si usa una variabile, il tipo di dati di tale variabile deve essere **varchar**, **char**, **smalldatetime** o **datetime**. Al database vengono applicati solo i record del log delle transazioni scritti prima della data e ora specificate.  
  
> [!NOTE]  
>  Se il punto nel tempo specificato per STOPAT è successivo all'ultimo backup del log, il database rimane in uno stato non recuperato, come se l'istruzione RESTORE LOG fosse stata eseguita con NORECOVERY.  
  
 Per altre informazioni, vedere [Ripristino di un database di SQL Server fino a un punto specifico all'interno di un backup &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
 STOPATMARK **=** { **'** _mark\_name_ **'**  |  **'** lsn:_lsn\_number_ **'** } [ AFTER **'** _datetime_ **'** ]  
 Specifica il recupero fino a un punto di recupero specificato. La transazione specificata viene inclusa nel recupero. Il commit della transazione viene eseguito solo se è stato eseguito al momento della generazione effettiva della transazione.  
  
 Sia RESTORE DATABASE che RESTORE LOG supportano il parametro *lsn_number*. che specifica un numero di sequenza del file di log (LSN).  
  
 Il parametro *mark_name* è supportato solo dall'istruzione RESTORE LOG. Questo parametro identifica un contrassegno di transazione nel backup del log.  
  
 Se l'opzione AFTER *datetime* viene omessa in un'istruzione RESTORE LOG, il recupero si interrompe in corrispondenza del primo contrassegno con il nome specificato. Se l'opzione AFTER *datetime* viene specificata, il recupero si interrompe in corrispondenza del primo contrassegno avente il nome specificato, nella data e all'ora indicate in *datetime* o in un momento successivo.  
  
> [!NOTE]  
>  Se il contrassegno, l'LSN o il punto nel tempo specificato è successivo all'ultimo backup del log, il database rimane in uno stato non recuperato, come se l'istruzione RESTORE LOG fosse stata eseguita con NORECOVERY.  
  
 Per altre informazioni, vedere [Usare transazioni contrassegnate per recuperare coerentemente i database correlati &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) e [Recupero fino a un numero di sequenza del file di log &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md).  
  
 STOPBEFOREMARK **=** { **'** _mark\_name_ **'**  |  **'** lsn:_lsn\_number_ **'** } [ AFTER **'** _datetime_ **'** ]  
 Specifica il recupero fino a un punto di recupero specificato. La transazione specificata non viene inclusa nel recupero. Il rollback della transazione viene eseguito quando si utilizza WITH RECOVERY.  
  
 Sia RESTORE DATABASE che RESTORE LOG supportano il parametro *lsn_number*. che specifica un numero di sequenza del file di log (LSN).  
  
 Il parametro *mark_name* è supportato solo dall'istruzione RESTORE LOG. Questo parametro identifica un contrassegno di transazione nel backup del log.  
  
 Se l'opzione AFTER *datetime* viene omessa in un'istruzione RESTORE LOG, il recupero si interrompe immediatamente prima del primo contrassegno con il nome specificato. Se AFTER *datetime* viene specificato, il recupero si interrompe immediatamente prima del primo contrassegno avente il nome specificato, nella data e all'ora indicate in *datetime* o in un momento successivo.  
  
> [!IMPORTANT]  
>  Se una sequenza di ripristino parziale esclude qualsiasi filegroup FILESTREAM, il ripristino temporizzato non è supportato. È possibile forzare la continuazione della sequenza di ripristino. Tuttavia i filegroup FILESTREAM omessi dall'istruzione RESTORE non potranno più essere ripristinati. Per forzare un ripristino temporizzato, specificare l'opzione CONTINUE_AFTER_ERROR insieme all'opzione STOPAT, STOPATMARK o STOPBEFOREMARK. Se si specifica CONTINUE_AFTER_ERROR, la sequenza di ripristino parziale ha esito positivo e il filegroup FILESTREAM non può più essere recuperato.  
  
## <a name="result-sets"></a>Set di risultati  
 Per informazioni sui set di risultati, vedere gli argomenti seguenti:  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
## <a name="remarks"></a>Osservazioni  
 Per ulteriori osservazioni, vedere gli argomenti seguenti:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
## <a name="specifying-a-backup-set"></a>Specifica di un set di backup  
 Un *set di backup* contiene il backup di una singola operazione di backup riuscita. Le istruzioni RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY e RESTORE VERIFYONLY operano in un singolo set di backup all'interno del set di supporti nei dispositivi di backup specificati. È consigliabile specificare il backup necessario all'interno del set di supporti. È possibile ottenere il valore *backup_set_file_number* di un backup usando l'istruzione [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
 L'opzione per specificare il set di backup da ripristinare è:  
  
 FILE **=** { *backup_set_file_number* |  **@** _backup\_set\_file\_number_ }  
  
 Dove *backup_set_file_number* indica la posizione del backup nel set di supporti. Un valore *backup_set_file_number* pari a 1 (FILE = 1) indica il primo set di backup nel supporto di backup, un valore di *backup_set_file_number* pari a 2 (FILE = 2) indica il secondo set di backup, e così via.  
  
 Il comportamento di questa opzione varia a seconda dell'istruzione, come descritto nella tabella seguente:  
  
|.|Comportamento dell'opzione FILE di un set di backup|  
|---------------|-----------------------------------------|  
|RESTORE|Il numero di file predefinito del set di backup è 1. In un'istruzione RESTORE è consentita una sola opzione FILE di un set di backup. È importante specificare i set di backup nell'ordine corretto.|  
|RESTORE FILELISTONLY|Il numero di file predefinito del set di backup è 1.|  
|RESTORE HEADERONLY|Per impostazione predefinita, vengono elaborati tutti i set di backup nel set di supporti. Il set di risultati dell'istruzione RESTORE HEADERONLY restituisce informazioni su ogni set di backup, inclusa la relativa **Posizione** nel set di supporti. Per restituire informazioni su un set di backup specifico, usare il numero di posizione corrispondente come valore di *backup_set_file_number* nell'opzione FILE.<br /><br /> Nota: Per i supporti a nastro, RESTORE HEADER elabora solo i set di backup contenuti nel nastro caricato.|  
|RESTORE VERIFYONLY|Il valore predefinito di *backup_set_file_number* è 1.|  
  
> [!NOTE]  
>  L'opzione FILE per l'indicazione di un set di backup non è correlata all'opzione FILE usata per specificare un file di database, FILE **=** { *logical_file_name_in_backup* |  **@** _logical\_file\_name\_in\_backup\_var_ }.  
  
## <a name="summary-of-support-for-with-options"></a>Riepilogo del supporto delle opzioni WITH  
 Le opzioni WITH seguenti sono supportate solo dall'istruzione RESTORE: BLOCKSIZE, BUFFERCOUNT, MAXTRANSFERSIZE, PARTIAL, KEEP_REPLICATION, { RECOVERY | NORECOVERY | STANDBY }, REPLACE, RESTART, RESTRICTED_USER e { STOPAT | STOPATMARK | STOPBEFOREMARK }  
  
> [!NOTE]  
>  L'opzione PARTIAL è supportata solo dall'istruzione RESTORE DATABASE.  
  
 Nella tabella seguente sono elencate le opzioni WITH utilizzate da una o più istruzioni e vengono indicate le istruzioni supportate da ciascuna opzione. Un segno di spunta (√) indica che l'opzione è supportata, mentre un trattino (—) indica che l'opzione non è supportata.  
  
|Opzione WITH|RESTORE|RESTORE FILELISTONLY|RESTORE HEADERONLY|RESTORE LABELONLY|RESTORE REWINDONLY|RESTORE VERIFYONLY|  
|-----------------|-------------|--------------------------|------------------------|-----------------------|------------------------|------------------------|  
|{ CHECKSUM<br /><br /> &#124; NO_CHECKSUM }|√|√|√|√|-|√|  
|{ CONTINUE_AFTER_ERROR<br /><br /> &#124; STOP_ON_ERROR }|√|√|√|√|-|√|  
|FILE<sup>1</sup>|√|√|√|-|-|√|  
|LOADHISTORY|-|-|-|-|-|√|  
|MEDIANAME|√|√|√|√|-|√|  
|MEDIAPASSWORD|√|√|√|√|-|√|  
|MOVE|√|-|-|-|-|√|  
|PASSWORD|√|√|√|-|-|√|  
|{ REWIND &#124; NOREWIND }|√|Solo REWIND|Solo REWIND|Solo REWIND|-|√|  
|STATS (Statistiche)|√|-|-|-|-|√|  
|{ UNLOAD &#124; NOUNLOAD }|√|√|√|√|√|√|  
  
 <sup>1</sup> FILE **=** _backup\_set\_file\_number_, che è diversa da {FILE | FILEGROUP}.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per informazioni sulle autorizzazioni, vedere gli argomenti seguenti:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
## <a name="examples"></a>Esempi  
 Per gli esempi, vedere gli argomenti seguenti:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)  
