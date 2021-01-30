---
description: backupset (Transact-SQL)
title: backupset (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2020
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupset
- backupset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupset system table
- backup media [SQL Server], backupset system table
- backup sets [SQL Server]
ms.assetid: 6ff79bbf-4acf-4f75-926f-38637ca8a943
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe3235515a86a6fbe7ca378d3c162b31c8214b82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183379"
---
# <a name="backupset-transact-sql"></a>backupset (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Contiene una riga per ogni set di backup. Un *set di backup* contiene il backup di una singola operazione di backup riuscita. Le istruzioni RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY e RESTORE VERIFYONLY operano in un singolo set di backup all'interno del set di supporti nei dispositivi di backup specificati.  
  
 Questa tabella è archiviata nel database **msdb** .  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Numero di identificazione univoco del set di backup. Identità, chiave primaria.|  
|**backup_set_uuid**|**uniqueidentifier**|Numero di identificazione univoco del set di backup.|  
|**media_set_id**|**int**|Numero di identificazione univoco del set di supporti che include il set di backup. Fa riferimento a **BackupMediaSet (media_set_id)**.|  
|**first_family_number**|**tinyint**|Numero del gruppo di supporti in cui inizia il set di backup. Può essere NULL.|  
|**first_media_number**|**smallint**|Numero del supporto in cui inizia il set di backup. Può essere NULL.|  
|**last_family_number**|**tinyint**|Numero del gruppo di supporti in cui termina il set di backup. Può essere NULL.|  
|**last_media_number**|**smallint**|Numero del supporto in cui termina il set di backup. Può essere NULL.|  
|**catalog_family_number**|**tinyint**|Numero del gruppo di supporti che include l'inizio della directory del set di backup. Può essere NULL.|  
|**catalog_media_number**|**smallint**|Numero del supporto che include l'inizio della directory del set di backup. Può essere NULL.|  
|**position**|**int**|Posizione del set di backup utilizzata nell'operazione di ripristino per individuare il set e i file di backup appropriati. Può essere NULL. Per ulteriori informazioni, vedere FILE in [BACKUP &#40;&#41;Transact-SQL ](../../t-sql/statements/backup-transact-sql.md).|  
|**expiration_date**|**datetime**|Data e ora di scadenza del set di backup. Può essere NULL.|  
|**software_vendor_id**|**int**|Numero di identificazione del produttore del software con cui viene scritta l'intestazione supporto di backup. Può essere NULL.|  
|**nome**|**nvarchar(128)**|Nome del set di backup. Può essere NULL.|  
|**description**|**nvarchar(255)**|Descrizione del set di backup. Può essere NULL.|  
|**user_name**|**nvarchar(128)**|Nome dell'utente che esegue l'operazione di backup. Può essere NULL.|  
|**software_major_version**|**tinyint**|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]numero di versione principale. Può essere NULL.|  
|**software_minor_version**|**tinyint**|Numero di versione secondario di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Può essere NULL.|  
|**software_build_version**|**smallint**|Numero di build di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Può essere NULL.|  
|**time_zone**|**smallint**|Differenza tra l'ora locale (in cui viene eseguita l'operazione di backup) e l'ora UTC (Coordinated Universal Time) in intervalli di 15 minuti usando le informazioni sul fuso orario al momento dell'avvio dell'operazione di backup. I possibili valori sono compresi tra -48 e +48 inclusi. Il valore 127 indica che la differenza è sconosciuta. Ad esempio, -20 indica l'ora della costa orientale degli Stati Uniti, ovvero 5 ore in meno rispetto all'ora di Greenwich. Può essere NULL.|  
|**mtf_minor_version**|**tinyint**|Numero secondario della versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format. Può essere NULL.|  
|**first_lsn**|**numeric(25,0)**|Numero di sequenza del file di log del primo record, ovvero del record di log meno recente nel set di backup. Può essere NULL.|  
|**last_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) del record di log successivo dopo il set di backup. Può essere NULL.|  
|**checkpoint_lsn**|**numeric(25,0)**|Numero di sequenza del file di log del record di log da cui deve essere avviata l'operazione di rollforward. Può essere NULL.|  
|**database_backup_lsn**|**numeric(25,0)**|Numero di sequenza del file di log dell'operazione più recente di backup completo del database. Può essere NULL.<br /><br /> **database_backup_lsn** è l'inizio del checkpoint che viene attivato all'avvio del backup. Questo LSN coincide con **first_lsn** se il backup viene effettuato quando il database è inattivo e non è configurata alcuna replica.|  
|**database_creation_date**|**datetime**|Data e ora in cui è stato creato il database. Può essere NULL.|  
|**backup_start_date**|**datetime**|Data e ora in cui è stata avviata l'operazione di backup. Può essere NULL.|  
|**backup_finish_date**|**datetime**|Data e ora in cui è terminata l'operazione di backup. Può essere NULL.|  
|**type**|**char(1)**|Tipo di backup. I possibili valori sono i seguenti:<br /><br /> D = Database<br /><br /> I = Database differenziale<br /><br /> L = Log<br /><br /> F = File o filegroup<br /><br /> G =File differenziale<br /><br /> P = Parziale<br /><br /> Q = Parziale differenziale<br /><br /> Può essere NULL.|  
|**sort_order**|**smallint**|Tipo di ordinamento del server che esegue l'operazione di backup. Può essere NULL. Per ulteriori informazioni sugli ordinamenti e sulle regole di confronto, vedere [regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**code_page**|**smallint**|Tabella codici del server che esegue l'operazione di backup. Può essere NULL. Per ulteriori informazioni sulle tabelle codici, vedere [regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**compatibility_level**|**tinyint**|Impostazione del livello di compatibilità per il database. I possibili valori sono i seguenti:<br /><br /> 90 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 100 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 110 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 120 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> Può essere NULL.<br /><br /> Per informazioni sui livelli di compatibilità supportati, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**database_version**|**int**|Numero di versione del database. Può essere NULL.|  
|**backup_size**|**numeric(20,0)**|Dimensioni in byte del set di backup. Può essere NULL. Per i backup VSS, backup_size è un valore stimato.|  
|**database_name**|**nvarchar(128)**|Nome del database su cui viene eseguita l'operazione di backup. Può essere NULL.|  
|**server_name**|**nvarchar(128)**|Nome del server che esegue l'operazione di backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Può essere NULL.|  
|**machine_name**|**nvarchar(128)**|Nome del computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Può essere NULL.|  
|**flags**|**int**|In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la colonna **Flags** è stata deprecata e viene sostituita con le colonne di bit seguenti:<br /><br /> **has_bulk_logged_data** <br /> **is_snapshot** <br /> **is_readonly** <br /> **is_single_user** <br /> **has_backup_checksums** <br /> **is_damaged** <br /> **begins_log_chain** <br /> **has_incomplete_metadata** <br /> **is_force_offline** <br /> **is_copy_only**<br /><br /> Può essere NULL.<br /><br /> Nei set di backup di versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i flag disponibili sono i seguenti:<br />1 = Il backup contiene dati a registrazione minima. <br />2 = È stata utilizzata l'opzione WITH SNAPSHOT. <br />4 = Al momento del backup il database era in modalità sola lettura.<br />8 = Al momento del backup il database era in modalità utente singolo.|  
|**unicode_locale**|**int**|Impostazioni locali Unicode. Può essere NULL.|  
|**unicode_compare_style**|**int**|Stile di confronto Unicode. Può essere NULL.|  
|**nome_regole_di_confronto**|**nvarchar(128)**|Nome delle regole di confronto. Può essere NULL.|  
|**Is_password_protected**|**bit**|Indica se il set di backup<br /><br /> è protetto con password:<br /><br /> 0 = non protetto<br /><br /> 1 = protetto|  
|**recovery_model**|**nvarchar(60)**|Modello di recupero per il database:<br /><br /> FULL<br /><br /> BULK-LOGGED<br /><br /> SEMPLICE|  
|**has_bulk_logged_data**|**bit**|1 = Il backup contiene dati con registrazione minima delle operazioni bulk.|  
|**is_snapshot**|**bit**|1 = Il backup è stato eseguito utilizzando l'opzione SNAPSHOT.|  
|**is_readonly**|**bit**|1 = Al momento del backup il database era in modalità sola lettura.|  
|**is_single_user**|**bit**|1 = Al momento del backup il database era in modalità utente singolo.|  
|**has_backup_checksums**|**bit**|1 = Il backup contiene valori di checksum del backup.|  
|**is_damaged**|**bit**|1 = Durante la creazione del backup sono stati rilevati danni al database. È stato richiesto di continuare l'operazione di backup nonostante gli errori.|  
|**begins_log_chain**|**bit**|1 = Il primo di una catena continua di backup di log. Una catena di log inizia con il primo backup del log eseguito dopo la creazione del database oppure quando si passa dal modello di recupero con registrazione semplice al modello di recupero con registrazione completa o al modello di recupero con registrazione minima delle operazioni bulk.|  
|**has_incomplete_metadata**|**bit**|1 = Backup della parte finale del log con metadati incompleti. Per altre informazioni, vedere [Backup della parte finale del log &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).|  
|**is_force_offline**|**bit**|1 = Per il database è stata impostata la modalità offline mediante l'utilizzo dell'opzione NORECOVERY durante la creazione del backup.|  
|**is_copy_only**|**bit**|1 = Backup di sola copia. Per altre informazioni, vedere [Backup di sola copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).|  
|**first_recovery_fork_guid**|**uniqueidentifier**|ID del fork di recupero iniziale. Corrisponde a **FirstRecoveryForkID** di RESTORE HEADERONLY.<br /><br /> Per i backup dei dati, **first_recovery_fork_guid** è uguale a **last_recovery_fork_guid**.|  
|**last_recovery_fork_guid**|**uniqueidentifier**|ID del fork di recupero finale. Corrisponde a **RecoveryForkID** di RESTORE HEADERONLY.<br /><br /> Per i backup dei dati, **first_recovery_fork_guid** è uguale a **last_recovery_fork_guid**.|  
|**fork_point_lsn**|**numeric(25,0)**|Se **first_recovery_fork_guid** non è uguale a **last_recovery_fork_guid**, si tratta del numero di sequenza del file di log del punto di fork. Negli altri casi il valore è NULL.|  
|**database_guid**|**uniqueidentifier**|ID univoco per il database. Corrisponde a **BindingId** di RESTORE HEADERONLY. Quando il database viene ripristinato, viene assegnato un nuovo valore.|  
|**family_guid**|**uniqueidentifier**|ID univoco del database originale al momento della creazione. Questo valore rimane invariato quando il database viene ripristinato, anche in caso di modifica del nome.|  
|**differential_base_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) di base per i backup differenziali. Per un backup differenziale basato su un solo backup; le modifiche con LSN maggiori o uguali a **differential_base_lsn** sono incluse nel backup differenziale.<br /><br /> Per un valore differenziale basato su più backup, il valore è NULL e l'LSN di base deve essere determinato a livello di file (vedere [backupfile &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/backupfile-transact-sql.md)).<br /><br /> Per i tipi di backup non differenziali, il valore è sempre NULL.|  
|**differential_base_guid**|**uniqueidentifier**|Per un backup differenziale basato su un solo backup, il valore è l'identificatore univoco della base differenziale.<br /><br /> Per i backup differenziali basati su più backup, il valore è NULL e la base differenziale deve essere determinata a livello di file.<br /><br /> Per tipi di backup non differenziali, il valore è NULL.|  
|**compressed_backup_size**|**Numerico (20, 0)**|Numero totale di byte del backup archiviato nel disco.<br /><br /> Per calcolare il rapporto di compressione, utilizzare **compressed_backup_size** e **backup_size**.<br /><br /> Durante un aggiornamento di **msdb** , questo valore è impostato su null. che indica un backup non compresso.|  
|**key_algorithm**|**nvarchar(32)**|Algoritmo utilizzato per crittografare il backup. Il valore NO_Encryption indica che il backup non è stato crittografato.|  
|**encryptor_thumbprint**|**varbinary(20)**|L'identificazione digitale del componente di crittografia che può essere utilizzato per trovare il certificato o la chiave asimmetrica nel database. Nel caso in cui il backup non è stato crittografato, questo valore è NULL.|  
|**encryptor_type**|**nvarchar(32)**|Tipo di componente di crittografia usato: certificato o chiave asimmetrica. . Nel caso in cui il backup non è stato crittografato, questo valore è NULL.|  
  
## <a name="remarks"></a>Commenti
- RESTOre VERIFYONLY FROM *backup_device* with LOADHISTORY popola la colonna della tabella **BackupMediaSet** con i valori appropriati dell'intestazione del set di supporti.  
- Per ridurre il numero di righe in questa tabella e in altre tabelle di backup e di cronologia, eseguire la [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) stored procedure.  
- Per SQL Istanza gestita, la tabella backupset Mostra solo la cronologia di backup per i [backup di sola copia](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)avviati dall'utente. La tabella backupset non Mostra la cronologia di backup per i backup automatici eseguiti dal servizio. 
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di backup e ripristino &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [Possibili errori relativi ai supporti durante il backup e il ripristino &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Modelli di recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Tabelle di backup e ripristino &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)  
  
  
