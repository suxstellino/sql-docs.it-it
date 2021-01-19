---
title: Backup di snapshot di file per i file di database in Azure | Microsoft Docs
description: Il backup di snapshot di file di SQL Server usa gli snapshot di Azure per offrire backup veloci e ripristini più veloci per i file di database archiviati mediante il servizio di archiviazione BLOB di Azure.
ms.custom: ''
ms.date: 05/23/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 17a81fcd-8dbd-458d-a9c7-2b5209062f45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 63e2f722305c2af448be7d49868eef72d1a9fb8d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172693"
---
# <a name="file-snapshot-backups-for-database-files-in-azure"></a>Backup di snapshot di file per i file di database in Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il backup di snapshot di file usa gli snapshot di Azure per offrire backup quasi istantanei e ripristini più veloci per i file di database archiviati usando il servizio di archiviazione BLOB di Azure. Questa funzionalità consente di semplificare i criteri di backup e ripristino. Per una dimostrazione dal vivo, vedere la [demo del ripristino temporizzato](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo). Per altre informazioni sull'archiviazione dei file di database con il servizio di archiviazione BLOB di Azure, vedere [File di dati di SQL Server in Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md).  
  
 ![diagramma dell'architettura per il backup di snapshot](../../relational-databases/backup-restore/media/snapshotbackups.PNG "diagramma dell'architettura per il backup di snapshot")  
  
 **Scaricare**  
  
-   Per scaricare [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], passare a  **[Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   Se si ha un account di Azure,  fare clic **[qui](https://azure.microsoft.com/services/virtual-machines/sql-server/)** per creare rapidamente una macchina virtuale in cui è già installato [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
## <a name="using-azure-snapshots-to-back-up-database-files-stored-in-azure"></a>Uso degli snapshot di Azure per eseguire il backup dei file di database archiviati in Azure  
  
### <a name="what-is-a-ssnoversion-file-snapshot-backup"></a>Che cos'è un backup di snapshot di file [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Un backup di snapshot di file è costituito da un set di snapshot di Azure dei BLOB che contengono i file di database e da un file di backup che contiene puntatori a tali snapshot di file. Ogni snapshot di file viene archiviato nel contenitore con il BLOB di base. È possibile specificare che il file di backup stesso venga scritto su URL, disco o nastro. È consigliabile eseguire il backup su URL. Per altre informazioni sul backup, vedere [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Per altre informazioni sul backup nell'URL, vedere [Backup di SQL Server nell'URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
 ![architettura della funzionalità relativa agli snapshot](../../relational-databases/backup-restore/media/snapshotbackups-flat.png "architettura della funzionalità relativa agli snapshot")  
  
 Se si elimina il BLOB il set di backup non sarà più valido e non è possibile eliminare un BLOB che contiene i file di snapshot (a meno che non si sia scelto espressamente di eliminare un BLOB con tutti i relativi snapshot di file). Inoltre, l'eliminazione di un database o di un file di dati non elimina il BLOB di base o uno qualsiasi dei relativi snapshot di file e l'eliminazione del file di backup non elimina alcuno degli snapshot di file nel set di backup. Per eliminare un set di backup di snapshot di file, usare la stored procedure di sistema **sys.sp_delete_backup** .  
  
 **Backup di database completo:** L'esecuzione di un backup di database completo usando il backup di snapshot di file crea uno snapshot di Azure di ogni file di dati e di log che costituisce il database, stabilisce la catena di backup del log delle transazioni e scrive la posizione degli snapshot di file nel file di backup.  
  
 **Backup del log delle transazioni:** L'esecuzione di un backup del log delle transazioni con il backup di snapshot di file crea uno snapshot di file di ogni file di database (non solo del log delle transazioni), registra le informazioni sul percorso dello snapshot di file nel file di backup e tronca il file di log delle transazioni.  
  
> [!IMPORTANT]  
>  Dopo il backup completo iniziale che è necessario per stabilire la catena dei backup del log delle transazioni (che può essere un backup di snapshot di file), è sufficiente eseguire i backup del log delle transazioni perché ogni set di backup di snapshot di file del log delle transazioni contiene snapshot di file di tutti i file di database e può essere usato per eseguire un ripristino del database o un ripristino del log. Dopo il backup del database completo iniziale, non sono necessari altri backup completi o differenziali perché il servizio di archiviazione BLOB di Azure gestisce le differenze tra ogni snapshot di file e lo stato corrente del BLOB di base per ogni file di database.  
  
> [!NOTE]  
>  Per un'esercitazione sull'uso di SQL Server 2016 con il servizio di archiviazione BLOB di Microsoft Azure, vedere [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
### <a name="restore-using-file-snapshot-backups"></a>Eseguire il ripristino con i backup di snapshot di file  
 Ogni set di backup di snapshot di file contiene uno snapshot di file di ogni file di database, quindi un processo di ripristino richiede al massimo due set di backup di snapshot di file adiacenti. Questo vale indipendentemente dal fatto che il set di backup provenga da un backup di database completo o da un backup del log. Si tratta di un processo molto diverso rispetto al ripristino eseguito con i file di backup di flusso tradizionale. Con il backup di flusso tradizionale il processo di ripristino richiede l'uso di un'intera catena dei set di backup: il backup completo, un backup differenziale e uno o più backup del log delle transazioni. La parte di recupero del processo di ripristino rimane invariata indipendentemente dal fatto che il ripristino usi un backup di snapshot di file o un set di backup di flusso.  
  
 **All'ora di qualsiasi set di backup:** Per eseguire un'operazione RESTORE DATABASE per ripristinare un database all'ora di un set di backup di snapshot di file specifico, è necessario solo il set di backup specifico, oltre ai BLOB di base stessi. Considerato che è possibile usare un set di backup di snapshot di file del log delle transazioni per eseguire un'operazione RESTORE DATABASE, in genere si usa un set di backup del log delle transazioni per eseguire questo tipo di operazione e solo raramente si usa un set di backup di database completo. Alla fine di questo argomento viene fornito un esempio che illustra questa tecnica.  
  
 **A un punto nel tempo che intercorre tra due set di backup di snapshot di file:** Per eseguire un'operazione RESTORE DATABASE per ripristinare un database a uno specifico punto nel tempo che intercorre tra l'ora di due set di backup del log delle transazioni adiacenti, sono necessari solo due set di backup del log delle transazioni (prima e dopo il punto nel tempo per cui si vuole ripristinare il database). A tale scopo, eseguire un'operazione RESTORE DATABASE WITH NORECOVERY usando il set di backup di snapshot di file del log transazionale del precedente punto nel tempo ed eseguire un'operazione RESTORE LOG WITH RECOVERY usando il set di backup di snapshot di file del log delle transazioni del successivo punto nel tempo e usando l'argomento STOPAT per specificare il punto nel tempo in cui interrompere il recupero dal backup del log delle transazioni. Alla fine di questo argomento viene fornito un esempio che illustra questa tecnica. Per una dimostrazione dal vivo, vedere la [demo del ripristino temporizzato](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo).  
  
### <a name="file-backup-set-maintenance"></a>Manutenzione di set di backup di file  
 **Eliminazione di un set di backup di snapshot di file:** Non è possibile sovrascrivere un set di backup di snapshot di file impostato usando l'argomento FORMAT. L'argomento FORMAT non è consentito per evitare di lasciare orfani snapshot di file che erano stati creati con il backup di snapshot di file originale. Per eliminare un set di backup di snapshot di file, usare la stored procedure di sistema **sys.sp_delete_backup** . Questa stored procedure elimina il file di backup e gli snapshot di file che costituiscono il set di backup. L'uso di un altro metodo per eliminare un set di backup di snapshot di file può eliminare il file di backup senza eliminare gli snapshot di file nel set di backup.  
  
 **Eliminazione di snapshot di file di backup orfani:** Potrebbero essere stati lasciati snapshot di file orfani se il file di backup è stato eliminato senza usare la stored procedure di sistema **sys.sp_delete_backup** o se un database o un file di database è stato eliminato mentre i BLOB che contenevano il database o il file di database contenevano snapshot di file di backup a essi associati. Per identificare gli snapshot di file che potrebbero essere orfani, usare la funzione di sistema **sys.fn_db_backup_file_snapshots** per elencare tutti gli snapshot di file dei file di database. Per identificare gli snapshot di file che fanno parte di un set di backup di snapshot di file specifico, usare la stored procedure di sistema RESTORE FILELISTONLY. È quindi possibile usare la stored procedure di sistema **sys.sp_delete_backup_file_snapshot** per eliminare un singolo snapshot di file di backup rimasto orfano. Alla fine di questo argomento vengono forniti esempi di uso di questa funzione di sistema e di queste stored procedure di sistema. Per altre informazioni, vedere [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md), [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md), [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) e [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
### <a name="considerations-and-limitations"></a>Considerazioni e limitazioni  
 **Archiviazione Premium:** Quando si usa l'archiviazione Premium, si applicano le limitazioni seguenti:  
  
-   Il file di backup stesso non può essere archiviato usando l'archiviazione Premium.  
  
-   La frequenza dei backup non può essere inferiore a 10 minuti.  
  
-   Il numero massimo di snapshot che è possibile archiviare è 100.  
  
-   RESTORE WITH MOVE è obbligatorio.  
  
-   Per altre informazioni sull'archiviazione Premium, vedere [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](/azure/virtual-machines/disks-types) (Archiviazione Premium: Archiviazione ad alte prestazioni per i carichi di lavoro delle macchine virtuali di Azure)  
  
 **Account di archiviazione singolo:** I BLOB degli snapshot di file e di destinazione devono usare lo stesso account di archiviazione.  
  
 **Modello di recupero con registrazione minima delle operazioni bulk:** Quando si usa la modalità di recupero con registrazione minima delle operazioni bulk e si lavora con un backup del log delle transazioni contenente transazioni con registrazione minima, non è possibile eseguire un ripristino del log (compreso il recupero temporizzato) usando il backup del log delle transazioni. Eseguire invece un ripristino del database all'ora del set di backup di snapshot di file. Questa limitazione è identica alla limitazione con il backup di flusso.  
  
 **Ripristino in linea:** Quando si usa il backup di snapshot di file non è possibile eseguire un ripristino in linea. Per altre informazioni sul ripristino in linea, vedere [Ripristino in linea &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
 **Fatturazione:** Quando si usa il backup di snapshot di file di SQL Server, le modifiche dei dati saranno soggette a costi aggiuntivi. Per altre informazioni, vedere [Informazioni sull'incremento dei costi dovuto agli snapshot](/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges).  
  
 **Archiviazione:** Se si vuole archiviare un backup di snapshot di file, è possibile usare l'archivio BLOB o il backup di flusso. Per archiviare nell'archivio BLOB, copiare gli snapshot nel set di backup di snapshot di file in BLOB separati. Per archiviare il backup di flusso, ripristinare il backup di snapshot di file come un nuovo database e quindi eseguire un normale backup di flusso con la compressione e/o la crittografia e archiviarlo per il tempo desiderato, indipendentemente dai BLOB di base.  
  
> [!IMPORTANT]  
>  La gestione di più backup di snapshot di file comporta solo un minimo overhead delle prestazioni. La gestione di un numero eccessivo di backup di snapshot di file, invece, può avere un impatto sulle prestazioni di I/O del database. È consigliabile gestire solo i backup di snapshot di file necessari per supportare l'obiettivo del punto di recupero.  
  
## <a name="backing-up-the-database-and-log-using-a-file-snapshot-backup"></a>Backup del database e del log usando un backup di snapshot di file  
 L'esempio seguente usa il backup di snapshot di file per eseguire il backup del database di esempio AdventureWorks2016 su URL.  
  
```  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2016  
   SET RECOVERY FULL;  
GO  
-- Back up the full AdventureWorks2016 database.  
BACKUP DATABASE AdventureWorks2016   
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
WITH FILE_SNAPSHOT;  
GO  
-- Back up the AdventureWorks2016 log using a time stamp in the backup file name.  
DECLARE @Log_Filename AS VARCHAR (300);  
SET @Log_Filename = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_Log_'+   
REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
BACKUP LOG AdventureWorks2016  
 TO URL = @Log_Filename WITH FILE_SNAPSHOT;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup"></a>Ripristino da un backup di snapshot di file [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 L'esempio eseguente mostra come eseguire il ripristino del database AdventureWorks2016 usando un set di backup di snapshot di file del log delle transazioni e illustra un'operazione di recupero. Si noti che è possibile ripristinare un database da un singolo set di backup di snapshot di file del log transazionale.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH RECOVERY, REPLACE;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup-to-a-point-in-time"></a>Ripristino temporizzato da un backup di snapshot di file [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 L'esempio seguente mostra come eseguire il ripristino di AdventureWorks2016 allo stato in cui si trova in un punto nel tempo specificato usando due set di backup di snapshot di file del log delle transazioni e illustra un'operazione di recupero.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH NORECOVERY,REPLACE;  
GO   
  
RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
WITH RECOVERY,STOPAT = 'May 18, 2015 5:35 PM';  
GO  
```  
  
## <a name="deleting-a-database-file-snapshot-backup-set"></a>Eliminazione di un set di backup di snapshot di file di database  
 Per eliminare un set di backup di snapshot di file, usare la stored procedure di sistema **sys.sp_delete_backup** . Specificare il nome del database in modo che il sistema verifichi che il set di backup di snapshot di file specificato sia effettivamente un backup per il database specificato. Se non si specifica alcun nome di database, il set di backup specificato con i relativi snapshot di file verrà eliminato senza tale convalida. Per altre informazioni, vedere [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md).  
  
> [!WARNING]  
>  Se si prova a eliminare un set di backup di snapshot di file usando un altro metodo, ad esempio il portale di gestione di Microsoft Azure o il visualizzatore di archiviazione di Azure in SQL Server Management Studio, gli snapshot di file nel set di backup non verranno eliminati. Questi strumenti elimineranno il file di backup stesso che contiene i puntatori agli snapshot di file nel set di backup di snapshot di file. Per identificare gli snapshot di file di backup che rimangono dopo aver eliminato un file di backup in modo errato, usare la funzione di sistema **sys.fn_db_backup_file_snapshots** e quindi la stored procedure di sistema **sys.sp_delete_backup_file_snapshot** per eliminare un singolo snapshot di file di backup.  
  
 L'esempio seguente mostra come eliminare il set di backup di snapshot di file specificato, inclusi i file di backup e gli snapshot di file che costituiscono il set di backup specificato.  
  
```  
sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak', 'adventureworks2016' ;  
GO  
  
```  
  
## <a name="viewing-database-backup-file-snapshots"></a>Visualizzazione di snapshot di file di backup del database  
 Per visualizzare snapshot di file di un BLOB di base per ogni file di database, usare la funzione di sistema **sys.fn_db_backup_file_snapshots** , che consente di visualizzare tutti gli snapshot di file di backup di ogni BLOB di base per un database archiviato usando il servizio di archiviazione BLOB di Azure. Un caso d'utilizzo primario per questa funzione prevede l'identificazione degli snapshot di file di backup di un database che rimangono quando il file di backup per un set di backup di snapshot di file viene eliminato usando un meccanismo diverso dalla stored procedure di sistema **sys.sp_delete_backup** . Per determinare gli snapshot di file di backup che fanno parte di set di backup intatti e quelli che non ne fanno parte, usare la stored procedure di sistema **RESTORE FILELISTONLY**  per elencare gli snapshot di file che appartengono a ogni file di backup. Per altre informazioni, vedere [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) e [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
 L'esempio seguente restituisce l'elenco di tutti gli snapshot di file di backup per il database specificato.  
  
```  
--Either specify the database name or set the database context  
USE AdventureWorks2016  
select * from sys.fn_db_backup_file_snapshots (null) ;  
GO  
select * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016') ;  
GO  
  
```  
  
## <a name="deleting-an-individual-database-backup-file-snapshot"></a>Eliminazione di un singolo snapshot di file di backup di database  
 Per eliminare un singolo snapshot di file di backup di un BLOB di base di un database, usare la stored procedure di sistema **sys.sp_delete_backup_file_snapshot** . Un caso d'utilizzo primario per questa stored procedure di sistema prevede l'eliminazione dei file di snapshot di file orfani che rimangono dopo aver eliminato un file di backup con un metodo diverso dalla stored procedure di sistema **sys.sp_delete_backup** . Per altre informazioni, vedere [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md).  
  
> [!WARNING]  
>  L'eliminazione di un singolo snapshot di file che fa parte di un set di backup di snapshot di file invalida il set di backup.  
  
 L'esempio seguente mostra come eliminare lo snapshot di file di backup specificato. L'URL per il backup specificato viene ottenuto usando la funzione di sistema **sys.fn_db_backup_file_snapshots** .  
  
```  
sys.sp_delete_backup_file_snapshot N'adventureworks2016', N'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016Data.mdf?snapshot=2015-05-29T21:31:31.6502195Z';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
