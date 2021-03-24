---
title: Copiare database tramite backup e ripristino | Microsoft Docs
description: In SQL Server è possibile creare un nuovo database ripristinando un backup di un database utente creato tramite alcune delle versioni precedenti.
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], back up and restore
- restoring databases [SQL Server], previous SQL Server versions
- database restores [SQL Server], copying databases
- restoring databases [SQL Server], onto another server instance
- restoring [SQL Server], onto another server instance
- backing up databases [SQL Server], copying databases
- database backups [SQL Server], copying databases
ms.assetid: b93e9701-72a0-408e-958c-dc196872c040
author: stevestein
ms.author: sstein
ms.openlocfilehash: 6be7ab989d06f807f33342bbe7e9978a23f809ad
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890671"
---
# <a name="copy-databases-with-backup-and-restore"></a>Copiare database tramite backup e ripristino
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] è possibile creare un nuovo database ripristinando un backup di un database utente creato tramite [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versione successiva. Tuttavia, i backup di **master**, **model** e **msdb** creati con una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non possono essere ripristinati da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Inoltre, i backup di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] non possono essere ripristinati da una qualsiasi versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]
> SQL Server 2016 usa un percorso predefinito diverso rispetto alle versioni precedenti. Pertanto, per ripristinare i backup di un database creato nel percorso predefinito di versioni precedenti, è necessario utilizzare l'opzione MOVE. Per informazioni sul nuovo percorso predefinito, vedere [Percorsi dei file per le istanze predefinite e denominate di SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md). Per altre informazioni sullo spostamento dei file di database, vedere "Spostamento dei file di database" di seguito in questo argomento.  
  
## <a name="general-steps-for-using-backup-and-restore-to-copy-a-database"></a>Procedura generale per l'uso di operazioni di backup e ripristino per copiare un database  
 Quando si usa un'operazione di backup e ripristino per copiare un database in un'altra istanza di SQL Server, i computer di origine e di destinazione possono usare qualsiasi piattaforma sulla quale viene eseguito SQL Server.  
  
 Di seguito sono indicati i passaggi fondamentali:  
  
1.  Effettuare il backup del database di origine che può risiedere in un'istanza di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versioni successive. Il computer in cui è in esecuzione questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è il **computer di origine**.  
  
2.  Nel computer in cui si vuole copiare il database ( **computer di destinazione**), connettersi all'istanza di SQL Server in cui si intende ripristinare il database. Se necessario, nell'istanza del server di **destinazione** creare gli stessi dispositivi di backup usati per il backup dei database di **origine** .  
  
3.  Ripristinare il backup del database di **origine** nel computer di **destinazione** . Il ripristino del database determina la creazione automatica di tutti i file del database.  
  
Considerazioni aggiuntive che possono avere effetto su questo processo:
  
## <a name="before-you-restore-database-files"></a>Prima di ripristinare i file di database  
 Il ripristino di un database determina la creazione automatica dei file di database necessari al database in fase di ripristino. Per impostazione predefinita, i file creati da SQL Server durante il processo di ripristino usano gli stessi nomi e percorsi dei file di backup del database originale nel computer di origine.  
  
 Facoltativamente, quando si ripristina il database, è possibile specificare il mapping dei dispositivi, i nomi dei file oppure il percorso per il database in fase di ripristino. 
 
 Tale operazione potrebbe essere necessaria nelle situazioni seguenti:  
  
-   La struttura di directory o il mapping delle unità utilizzato dal database nel computer originale non è più disponibile nell'altro computer. Si supponga, ad esempio, che nel backup sia incluso un file che, per impostazione predefinita, verrebbe ripristinato all'unità E, ma tale unità non è disponibile nel computer di destinazione.  
  
-   Lo spazio nel percorso di destinazione non è sufficiente.  
  
-   Si sta riutilizzando un nome del database già presente nella destinazione di ripristino e il nome di qualsiasi file contenuto nel database è uguale al file di database del set di backup, pertanto si verifica una delle situazioni indicate di seguito.  
  
    -   Se è possibile sovrascrivere il file di database esistente, verrà sovrascritto (ciò non influisce su un file appartenente a un nome di database diverso).  
  
    -   Se non è possibile sovrascrivere il file esistente, si verifica un errore di ripristino.  
  
 Per evitare errori e conseguenze impreviste, prima dell'operazione di ripristino è possibile usare la tabella della cronologia [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md) per trovare i file di database e di log nel backup che si intende ripristinare.  
  
## <a name="moving-the-database-files"></a>Spostamento dei file di database  
 Se non è possibile ripristinare i file di backup del database nel computer di destinazione, sarà necessario spostarli in un nuovo percorso man mano che vengono ripristinati. Ad esempio:  
  
-   Si supponga di voler ripristinare un database da backup creati nella posizione predefinita di versioni precedenti.  
  
-   Per problemi di spazio, potrebbe essere necessario ripristinare alcuni file di database nel backup in un'altra unità disco. Questa eventualità può verificarsi frequentemente, dato che nella maggior parte dei casi i computer di un'organizzazione non hanno lo stesso numero o le stesse dimensioni di unità disco o configurazioni software identiche.  
  
-   Può essere necessario creare una copia di un database esistente sullo stesso computer, a scopo di prova. In questo caso i file del database originale esistono già, quindi è necessario specificare nomi di file diversi quando viene creata la copia del database durante l'operazione di ripristino.  
  
 Per altre informazioni, vedere "Per ripristinare file e filegroup in una nuova posizione" di seguito in questo argomento.  
  
## <a name="changing-the-database-name"></a>Modifica del nome del database  
 Il nome del database può essere modificato al momento del ripristino nel computer di destinazione. Non è necessario ripristinare il database e quindi modificare il nome manualmente. Ad esempio, può risultare necessario cambiare il nome del database da **Sales** a **SalesCopy** per indicare che si tratta di una copia.  
  
 Il nome del database specificato esplicitamente al momento del ripristino viene usato automaticamente come nuovo nome del database. Poiché il nome del database non esiste, viene creato un database con il nuovo nome tramite i file presenti nel backup.  
  
## <a name="when-upgrading-a-database-by-using-restore"></a>Quando si aggiorna un database usando il ripristino  
 Nel ripristino dei backup da una versione precedente può essere utile sapere in anticipo se il percorso (unità e directory) di ogni catalogo full-text di un backup sia esistente sul computer di destinazione. Per un elenco dei nomi logici e fisici (percorso e nome file) di ogni file in un backup, inclusi i file di catalogo, usare un'istruzione RESTORE FILELISTONLY FROM *<dispositivo_backup>* . Per altre informazioni, vedere [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
 Se lo stesso percorso non esiste sul computer di destinazione, sono disponibili due alternative:  
  
-   Creare il mapping di unità/directory equivalente sul computer di destinazione.  
  
-   Spostare i file di catalogo in una nuova posizione durante l'operazione di ripristino, utilizzando la clausola WITH MOVE nell'istruzione RESTORE DATABASE. Per altre informazioni, vedere [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Per informazioni sulle opzioni alternative per l'aggiornamento degli indici full-text, vedere [Aggiornare la ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="database-ownership"></a>Proprietà dei database  
 Quando un database viene ripristinato in un altro computer, l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o l'utente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows che inizia l'operazione di ripristino diventa automaticamente il proprietario del nuovo database. Al momento del ripristino, l'amministratore di sistema o il nuovo proprietario del database possono modificare il proprietario del database. Per evitare ripristini non autorizzati di un database, impostare password per i supporti o i set di backup.  
  
## <a name="managing-metadata-when-restoring-to-another-server-instance"></a>Gestione dei metadati durante il ripristino in un'altra istanza del server  
 Quando si ripristina un database in un'altra istanza del server per offrire a utenti e applicazioni un sistema più coerente, può essere necessario ricreare alcuni o tutti i metadati del database, ad esempio account di accesso e processi, nell'altra istanza del server. Per altre informazioni, vedere [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
 **Visualizzare i file di dati e i file di log in un set di backup**  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
 **Ripristinare file e filegroup in una nuova posizione**  
  
-   [Ripristino dei file in una nuova posizione &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
 **Ripristinare file e filegroup sovrascrivendo file esistenti**  
  
-   [Ripristinare file e filegroup sovrascrivendo file esistenti &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
 **Ripristinare un database con un nuovo nome**  
  
-   [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
 **Riavviare un'operazione di ripristino interrotta**  
  
-   [Riavviare un'operazione di ripristino interrotta &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
 **Cambiare il proprietario del database**  
  
-   [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)  
  
 **Copiare un database mediante SMO (SQL Server Management Objects)**  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadFileList%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.RelocateFiles%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReplaceDatabase%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore>  
  
## <a name="see-also"></a>Vedere anche  
 [Copia di database in altri server](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [Percorsi dei file per le istanze predefinite e denominate di SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)   
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
