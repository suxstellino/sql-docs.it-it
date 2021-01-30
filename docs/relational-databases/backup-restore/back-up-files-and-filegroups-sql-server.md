---
title: Backup di file e filegroup | Microsoft Docs
description: Questo articolo descrive come eseguire il backup di file e filegroup in SQL Server usando SQL Server Management Studio, Transact-SQL o PowerShell.
ms.custom: ''
ms.date: 08/03/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up filegroups [SQL Server]
- file backups [SQL Server], how-to topics
- backing up files [SQL Server]
- backups [SQL Server], creating
- filegroups [SQL Server], backing up
ms.assetid: a0d3a567-7d8b-4cfe-a505-d197b9a51f70
author: cawrites
ms.author: chadam
ms.openlocfilehash: 592cfb8ede44ac5f5314276cf0ce8be0874a785c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076939"
---
# <a name="back-up-files-and-filegroups"></a>Backup di file e filegroup
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come eseguire il backup di file e filegroup in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o PowerShell. Quando a causa delle dimensioni del database e dei requisiti relativi alle prestazioni non è consigliabile eseguire un backup completo del database, è possibile creare invece un backup del file. Un *backup del file* contiene tutti i dati inclusi in uno o più file o filegroup.
  
Per altre informazioni sul backup dei file, vedere [Backup completi del file &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md) e [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
- Non è possibile utilizzare l'istruzione BACKUP in una transazione esplicita o implicita.  
  
- In base al modello di recupero con registrazione minima, è necessario creare una copia di backup contenente tutti i file di lettura/scrittura, al fine di garantire che il database possa essere ripristinato fino a un punto nel tempo coerente. Anziché specificare singolarmente ogni file o filegroup di lettura/scrittura, utilizzare l'opzione READ_WRITE_FILEGROUPS. Questa opzione consente di eseguire il backup di tutti i filegroup di lettura/scrittura del database. Un backup creato con l'opzione READ_WRITE_FILEGROUPS è noto come *backup parziale*. Vedere [Backup parziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/partial-backups-sql-server.md).  
  
Per altre informazioni sulle limitazioni e restrizioni, vedere [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni
  
Per impostazione predefinita, per ogni operazione di backup eseguita in modo corretto viene aggiunta una voce al log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e al registro eventi di sistema. Se il backup del log viene eseguito di frequente, questi messaggi possono aumentare rapidamente, provocando la creazione di log degli errori di dimensioni elevate e rendendo difficile l'individuazione di altri messaggi. In questo caso è possibile eliminare tali voci di log usando il flag di traccia 3226 se nessuno degli script dipende da esse. Vedere [Flag di traccia &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  

###  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

Le autorizzazioni `BACKUP DATABASE` e `BACKUP LOG` vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** e dei ruoli predefiniti del database **db_owner** e **db_backupoperator**.  
  
 Eventuali problemi correlati alla proprietà e alle autorizzazioni sul file fisico del dispositivo di backup possono interferire con l'operazione di backup. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia possibile leggere e scrivere sul dispositivo e che l'account utilizzato per eseguire il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponga delle autorizzazioni di scrittura. Le autorizzazioni di accesso ai file, tuttavia, non vengono controllate dalla stored procedure [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)che aggiunge una voce per un dispositivo di backup nelle tabelle di sistema. Di conseguenza, i problemi relativi all'accesso e alla proprietà del file fisico del dispositivo di backup potrebbero emergere solo in fase di accesso alla risorsa fisica durante un tentativo di backup o ripristino.

## <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio   
  
1. Dopo aver effettuato la connessione all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], in Esplora oggetti fare clic sul nome del server per espanderne l'albero.  
  
1. Espandere **Database** e, a seconda del database, selezionare un database utente o espandere **Database di sistema** e selezionare un database di sistema.  
  
1. Fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi fare clic su **Backup**. Verrà visualizzata la finestra di dialogo **Backup database** .  
  
1. Verificare il nome del database nell'elenco **Database** . È possibile selezionare facoltativamente un database diverso nell'elenco.  
  
1. Nell'elenco **Tipo di backup** selezionare **Completo** o **Differenziale**.  
  
1. Selezionare l'opzione **File e filegroup** in **Componente di cui eseguire il backup**.  
  
1. Selezionare i file e i filegroup dei quali si desidera eseguire il backup nella finestra di dialogo **Seleziona file e filegroup** . È possibile selezionare uno o più file singoli oppure selezionare la casella di controllo relativa a un filegroup per selezionare automaticamente tutti i file appartenenti al filegroup.  
  
1. Accettare il nome predefinito del set di backup indicato nella casella di testo **Nome** oppure immettere un nome diverso per il set di backup.  
  
1. Immettere una descrizione del set di backup nella casella di testo **Descrizione** (facoltativo).  
  
1. Specificare la scadenza del set di backup:  
  
    - Per impostare la scadenza del set di backup dopo un numero di giorni specifico, fare clic su **Dopo** (opzione predefinita) e immettere il numero di giorni dopo la creazione del set trascorsi i quali il set scadrà. È possibile impostare un valore compreso nell'intervallo da 0 a 99999 giorni. L'impostazione del valore 0 giorni indica che il set di backup non ha scadenza.  
  
         Il valore predefinito viene impostato nell'opzione **Periodo di memorizzazione predefinito supporti di backup (giorni)** della finestra di dialogo **Proprietà server** (pagina **Impostazioni database** ). Per accedere a questa opzione, fare clic con il pulsante destro del mouse sul nome del server in Esplora oggetti, scegliere Proprietà e quindi selezionare la pagina **Impostazioni database** .  
  
    - Per impostare una data di scadenza specifica per il set di backup, fare clic su **Il** e immettere la data di scadenza del set.  
  
1. Fare clic su **Disco** o su **Nastro** per selezionare il tipo di destinazione del backup. Per selezionare i percorsi per un massimo di 64 unità disco o nastro contenenti un singolo set di supporti, fare clic su **Aggiungi**. I percorsi selezionati vengono visualizzati nell'elenco **Backup su** .  
  
    > [!NOTE]
    > Per rimuovere una destinazione di backup, selezionarla e fare clic su **Rimuovi**. Per visualizzare il contenuto di una destinazione di backup, selezionarla e fare clic su **Contenuto**.  
  
1. Per visualizzare o selezionare le opzioni avanzate, fare clic su **Opzioni** nel riquadro **Selezione pagina** .  
  
1. Selezionare un'opzione **Sovrascrivi supporti** facendo clic su una delle opzioni seguenti:  
  
    - **Esegui backup nel set di supporti esistente**  
  
         Per questa opzione, fare clic su **Accoda al set di backup esistente** o **Sovrascrivi tutti i set di backup esistenti**. 
         
         Per informazioni sul backup in un set di supporti esistente, vedere [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
         - Selezionare l'opzione **Controlla nome set di supporti e scadenza set di backup** per impostare la verifica della data e dell'ora di scadenza del set di supporti e del set di backup durante l'operazione di backup (facoltativo).  
  
         - Immettere un nome nella casella di testo **Nome set di supporti** (facoltativo). Se non si specifica un nome, verrà creato un set di supporti con nome vuoto. Se si specifica il nome di un set di supporti, viene eseguito un controllo sul supporto (nastro o disco) per verificare che il nome effettivo corrisponda al nome qui immesso.  
  
         Se non si specifica il nome del set di supporti e si seleziona la casella di controllo per il controllo del nome, in caso di esito positivo anche il nome dei supporti nei supporti risulterà vuoto.  
  
    - **Esegui backup in un nuovo set di supporti e cancella tutti i set di backup esistenti**  
  
         Per questa opzione, immettere un nome nella casella di testo **Nome nuovo set di supporti** e, facoltativamente, aggiungere una descrizione per il set di supporti nella casella di testo **Descrizione nuovo set di supporti** .
         
         Per altre informazioni sulla creazione di un nuovo set di supporti, vedere [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
1. Nella sezione **Affidabilità** selezionare (facoltativo):  
  
    - **Verifica backup al termine**.  
  
    - **Esegui checksum prima della scrittura nei supporti** e **Continua in caso di errori checksum** (facoltativo).
    
         Per altre informazioni sui checksum, vedere [Possibili errori relativi ai supporti durante il backup e il ripristino &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
1. Se si esegue il backup su un'unità nastro, come specificato nella sezione **Destinazione** della pagina **Generale** , l'opzione **Scarica nastro al termine del backup** sarà attiva. Selezionando questa opzione viene abilitata anche l'opzione **Riavvolgi il nastro prima di scaricarlo** .  
  
    > [!NOTE]
    > Le opzioni presenti nella sezione **Log delle transazioni** sono attive solo in caso di backup di un log delle transazioni, come specificato nella sezione **Tipo backup** nella pagina **Generale** .  
  
1. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] e nelle versioni più recenti è supportata la [compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md). Per impostazione predefinita, la compressione di un backup dipende dal valore dell'opzione di configurazione del server **Valore predefinito di compressione backup**. Tuttavia, indipendentemente dall'impostazione predefinita a livello di server corrente, è possibile comprimere un backup selezionando **Comprimi backup** ed è possibile impedire la compressione selezionando **Non comprimere il backup**.  
  
     Per visualizzare l'impostazione predefinita di compressione dei backup, vedere [Visualizzare o configurare l'opzione di configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  

## <a name="using-transact-sql"></a>Uso di Transact-SQL
  
Per creare un backup del file o del filegroup, usare un'istruzione [BACKUP DATABASE <file_or_filegroup>](../../t-sql/statements/backup-transact-sql.md). In questa istruzione è necessario specificare almeno gli elementi seguenti:  
  
  - Nome del database.  
  
  - Una clausola FILE o FILEGROUP per ogni file o filegroup, rispettivamente.  
  
  - Il dispositivo di backup in cui verrà scritto il backup completo.  
  
La sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] di base per un backup del file è la seguente:  
  
  BACKUP DATABASE *database*  
  
  { FILE _=_ *nome_file_logico* | FILEGROUP _=_ *nome_filegroup_logico* } [ **,** ...*f* ]  
  
  TO *dispositivo_backup* [ **,** ...*n* ]  
  
  [ WITH *con_opzioni* [ **,** ...*o* ] ];  
  
|Opzione|Descrizione|  
|------------|-----------------|  
|*database*|Nome del database di cui viene eseguito il backup del log delle transazioni oppure il backup parziale o completo del database.|  
|FILE _=_ *nome_file_logico*|Specifica il nome logico di un file da includere nel backup del file.|  
|FILEGROUP _=_ *nome_filegroup_logico*|Specifica il nome logico di un filegroup da includere nel backup del file. Con il modello di recupero con registrazione minima, il backup dei filegroup è consentito solo per i filegroup di sola lettura.|  
|[ **,** ...*f* ]|Segnaposto che indica che è possibile specificare più file e filegroup. Il numero di file o filegroup che possono essere specificati è illimitato.|  
|*backup_device* [ **,** ...*n* ]|Specifica un elenco di dispositivi di backup da 1 a 64 da utilizzare per l'operazione di backup. È possibile specificare un dispositivo di backup fisico oppure un dispositivo di backup logico corrispondente se è già stata definito. Per specificare un dispositivo di backup fisico, utilizzare l'opzione DISK o TAPE:<br /><br /> { DISK &#124; TAPE } _=_ *nome_dispositivo_backup_fisico*<br /><br /> Per altre informazioni, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|  
|WITH *con_opzioni* [ **,** ...*o* ]|Facoltativamente, specifica una o più opzioni aggiuntive, ad esempio DIFFERENTIAL. un backup differenziale del file richiede come base un backup completo del file. <br /><br />Per altre informazioni, vedere [Creare un backup differenziale del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md).|  
  
Se si utilizza il modello di recupero con registrazione completa, è inoltre necessario eseguire un backup del log delle transazioni. Per utilizzare un set completo di backup del file completi per il ripristino di un database, è inoltre necessario disporre di backup dei log relativi a tutti i backup del file, dall'inizio del primo backup del file.

Per altre informazioni, vedere [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
###  <a name="examples"></a><a name="TsqlExample"></a> Esempi
Negli esempi seguenti viene eseguito il backup di uno o più file dei filegroup secondari del database `Sales` . Questo database utilizza il modello di recupero con registrazione completa e contiene i filegroup secondari seguenti:  
  
- Un filegroup denominato `SalesGroup1` che include i file `SGrp1Fi1` e `SGrp1Fi2`.  
  
- Un filegroup denominato `SalesGroup2` che include i file `SGrp2Fi1` e `SGrp2Fi2`.  
  
#### <a name="a-create-a-file-backup-of-two-files"></a>R. Creare un backup di due file  
Nell'esempio seguente viene creato un backup differenziale del file solo per il file `SGrp1Fi2` del filegroup `SalesGroup1` e per il file `SGrp2Fi2` del filegroup `SalesGroup2` .  
  
```sql  
--Backup the files in the SalesGroup1 secondary filegroup.  
BACKUP DATABASE Sales  
   FILE = 'SGrp1Fi2',   
   FILE = 'SGrp2Fi2'   
   TO DISK = 'G:\SQL Server Backups\Sales\SalesGroup1.bck';  
GO  
```  
  
#### <a name="b-create-a-full-file-backup-of-the-secondary-filegroups"></a>B. Creare un backup completo dei filegroup secondari  
Nell'esempio seguente viene creato un backup completo di ogni file presente in entrambi i filegroup secondari.  
  
```sql  
--Back up the files in SalesGroup1.  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'C:\MySQLServer\Backups\Sales\SalesFiles.bck';  
GO  
```  
  
#### <a name="c-create-a-differential-file-backup-of-the-secondary-filegroups"></a>C. Creare un backup differenziale dei filegroup secondari  
Nell'esempio seguente viene creato un backup differenziale di ogni file presente in entrambi i filegroup secondari.  
  
```sql  
--Back up the files in SalesGroup1.  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'C:\MySQLServer\Backups\Sales\SalesFiles.bck'  
   WITH   
      DIFFERENTIAL;  
GO  
```  
  
## <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell

Impostare e usare il [provider SQL Server PowerShell](../../powershell/sql-server-powershell-provider.md).
  
Usare il cmdlet **Backup-SqlDatabase** e specificare **Files** per il valore del parametro **-BackupAction** . Inoltre, specificare uno dei parametri seguenti:  
  
- Per eseguire il backup di un determinato file, specificare il parametro _-DatabaseFile_*String* , dove *String* rappresenta uno o più file di database di cui eseguire il backup.  
  
- Per eseguire il backup di tutti i file di un determinato filegroup, specificare il parametro _-DatabaseFileGroup_*String* , dove *String* rappresenta uno o più filegroup di database di cui eseguire il backup.  
  
Nell'esempio seguente viene creato un backup completo di ogni file presente nei filegroup secondari 'FileGroup1' e 'FileGroup2' nel database `<myDatabase>` . I backup vengono creati nel percorso di backup predefinito dell'istanza del server `Computer\Instance`.  

```powershell
Backup-SqlDatabase -ServerInstance Computer\Instance -Database <myDatabase> -BackupAction Files -DatabaseFileGroup "FileGroup1","FileGroup2" 
```
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Informazioni sulla cronologia e sull'intestazione del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [Eseguire il backup di database &#40;pagina Generale&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [Eseguire il backup di database &#40;pagina Opzioni di backup&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [Backup completi del file &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Ripristini di file &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Ripristini di file &#40;modello di recupero con registrazione minima&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)