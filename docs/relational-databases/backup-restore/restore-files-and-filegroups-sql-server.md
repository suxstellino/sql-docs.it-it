---
title: Ripristinare file e filegroup (SQL Server) | Microsoft Docs
description: In questo articolo viene illustrato come ripristinare file e filegroup in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restorefilesandfilegrps.general.f1
- sql13.swb.bselectfilegrpsfiles.f1
- sql13.swb.restorefilesandfilegrps.options.f1
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], restoring files and filegroups
- restoring [SQL Server], files
ms.assetid: 72603b21-3065-4b56-8b01-11b707911b05
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4a79c574e576b157d5dd4c07c8e725e02d8d74e7
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076919"
---
# <a name="restore-files-and-filegroups-sql-server"></a>Ripristino di file e filegroup (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene descritto come ripristinare file e filegroup in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
-   [Sicurezza](#Security)  
  
-   **Ripristino di file e filegroup utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   L'amministratore di sistema che esegue il ripristino di file e di filegroup deve essere l'unico utente attualmente collegato al database da ripristinare.  
  
-   Non è possibile utilizzare RESTORE in una transazione esplicita o implicita.  
  
-   Con il modello di recupero con registrazione minima, il file deve appartenere a un filegroup di sola lettura.  
  
-   In base al modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, prima di poter ripristinare file è necessario eseguire il backup del log delle transazioni attivo, noto come parte finale del log. Per altre informazioni, vedere [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
-   Per ripristinare un database crittografato, è necessario poter accedere alla chiave asimmetrica o al certificato utilizzato per crittografare il database. Non è possibile effettuare l'operazione di ripristino del database senza almeno uno di questi due elementi. Di conseguenza, il certificato utilizzato per crittografare la chiave di crittografia del database deve essere conservato fino a quando il backup è necessario. Per altre informazioni, vedere [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Se il database da ripristinare non esiste, per eseguire un'operazione RESTORE l'utente deve disporre delle autorizzazioni CREATE DATABASE. Se il database esiste, le autorizzazioni per l'istruzione RESTORE vengono assegnate per impostazione predefinita ai membri dei ruoli predefiniti del server **sysadmin** e **dbcreator** e al proprietario (**dbo**) del database. Per l'opzione FROM DATABASE_SNAPSHOT, il database esiste sempre.  
  
 Le autorizzazioni per l'istruzione RESTORE vengono assegnate ai ruoli in cui le informazioni sull'appartenenza sono sempre disponibili per il server. Poiché è possibile controllare l'appartenenza ai ruoli predefiniti del database solo quando il database è accessibile e non è danneggiato, condizioni che non risultano sempre vere quando si esegue un'operazione RESTORE, i membri del ruolo predefinito del database **db_owner** non dispongono delle autorizzazioni per l'istruzione RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-restore-files-and-filegroups"></a>Per ripristinare file e filegroup  
  
1.  Dopo aver eseguito la connessione all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], in Esplora oggetti fare clic sul nome del server per espandere l'albero del server.  
  
2.  Espandere **Database**. A seconda del database, selezionare un database utente oppure espandere **Database di sistema** e selezionare un database di sistema.  
  
3.  Fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi fare clic su **Ripristina**.  
  
4.  Fare clic su **File e filegroup**. Verrà visualizzata la finestra di dialogo **Ripristina file e filegroup** .  
  
5.  Nella pagina **Generale** nella casella di riepilogo **Database di destinazione** , immettere il database da ripristinare. È possibile immettere un nuovo database oppure sceglierne uno esistente dall'elenco a discesa. Nell'elenco sono inclusi tutti i database presenti nel server, ad eccezione dei database di sistema **master** e **tempdb**.  
  
6.  Per specificare l'origine e il percorso dei set di backup da ripristinare, fare clic su una delle opzioni seguenti:  
  
    -   **Database di origine**  
  
         Immettere il nome di un database nella casella di riepilogo. Nell'elenco sono inclusi solo i database di cui è stato eseguito il backup in base alla cronologia di backup di **msdb** .  
  
    -   **Dispositivo di origine**  
  
         Fare clic sul pulsante Sfoglia Nella finestra di dialogo **Seleziona dispositivi di backup** selezionare uno dei tipi di dispositivo elencati nella casella di riepilogo **Tipo di supporti di backup** . Per selezionare uno o più dispositivi per la casella di riepilogo **Supporti di backup** , fare clic su **Aggiungi**.  
  
         Dopo avere aggiunto i dispositivi desiderati nella casella di riepilogo **Dispositivi di backup** , fare clic su **OK** per tornare alla pagina **Generale** .  
  
7.  Nella griglia **Selezionare i set di backup da ripristinare** selezionare i set di backup che si desidera ripristinare. In questa griglia vengono visualizzati i backup disponibili per il percorso specificato. Per impostazione predefinita, viene suggerito un piano di recupero. Per ignorare il piano di recupero suggerito, è possibile modificare le impostazioni selezionate nella griglia. I backup che dipendono da un backup deselezionato vengono automaticamente deselezionati.  
  
    |Intestazione della colonna|Valori|  
    |-----------------|------------|  
    |**Restore**|Le caselle di controllo selezionate indicano i set di backup da ripristinare.|  
    |**Nome**|Nome del set di backup.|  
    |**Tipo di file**|Specifica il tipo di dati nel backup: **dati**, **registro** o **dati FILESTREAM**. I dati contenuti nelle tabelle sono nei file **Dati** . I dati del log delle transazioni sono nei file **Log** . I dati BLOB (Binary Large Object, oggetto binario di grandi dimensioni) archiviati nel file system si trovano nei file **Dati FILESTREAM** .|  
    |**Tipo**|Tipo di operazione di backup eseguita: **Completo**, **Differenziale** o **Log delle transazioni**.|  
    |**Server**|Nome dell'istanza del Motore di database che ha eseguito l'operazione di backup.|  
    |**Nome file logico**|Nome logico del file.|  
    |**Database**|Nome del database interessato dall'operazione di backup.|  
    |**Data inizio**|Data e ora di inizio dell'operazione di backup, visualizzate in base alle impostazioni internazionali del client.|  
    |**Data fine**|Data e ora di fine dell'operazione di backup, visualizzate in base alle impostazioni internazionali del client.|  
    |**Dimensione**|Dimensioni in byte del set di backup.|  
    |**Nome utente**|Nome dell'utente che ha eseguito l'operazione di backup.|  
  
8.  Per visualizzare o selezionare le opzioni avanzate, fare clic su **Opzioni** nel riquadro **Selezione pagina**.  
  
9. Nel pannello **Opzioni di ripristino** è possibile scegliere una qualsiasi delle opzioni seguenti, in base alla specifica situazione.  
  
     **Ripristina come filegroup**  
     Indica che un intero filegroup è in fase di ripristino.  
  
     **Sovrascrivi il database esistente**  
     Specifica che l'operazione di ripristino deve sovrascrivere eventuali database esistenti e i file correlati, anche se esiste già un database o un file con lo stesso nome.  
  
     La selezione di questa opzione equivale all'utilizzo dell'opzione REPLACE in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
     **Chiedi conferma prima del ripristino di ogni backup**  
     Viene richiesta la conferma dell'utente prima del ripristino di ogni set di backup.  
  
     Questa opzione risulta particolarmente utile quando è necessario scambiare i nastri di set di supporti diversi, ad esempio quando il server dispone di un solo dispositivo nastro.  
  
     **Limita accesso al database ripristinato**  
     Consente di rendere disponibile il database ripristinato solo per i membri di **db_owner**, **dbcreator** o **sysadmin**.  
  
     La selezione di questa opzione equivale all'utilizzo dell'opzione RESTRICTED_USER in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
10. È possibile ripristinare il database in un nuovo percorso specificando una nuova destinazione di ripristino per ogni file nella griglia **Ripristina file di database come** .  
  
    |Intestazione della colonna|Valori|  
    |-----------------|------------|  
    |**Nome file originale**|Percorso completo di un file di backup di origine.|  
    |**Tipo di file**|Specifica il tipo di dati nel backup: **dati**, **registro** o **dati FILESTREAM**. I dati contenuti nelle tabelle sono nei file **Dati** . I dati del log delle transazioni sono nei file **Log** . I dati BLOB (Binary Large Object, oggetto binario di grandi dimensioni) archiviati nel file system si trovano nei file **Dati FILESTREAM** .|  
    |**Ripristina come**|Percorso completo del file di database da ripristinare. Per specificare un nuovo file di ripristino, fare clic nella casella di testo e modificare il percorso e il nome del file suggeriti. La modifica del percorso o del nome file nella colonna **Ripristina come** equivale all'utilizzo dell'opzione MOVE in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
  
11. Il pannello **Stato di recupero** determina lo stato del database dopo l'operazione di ripristino.  

  **Lascia il database pronto per l'utilizzo eseguendo il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi non possono essere ripristinati. (RESTORE WITH RECOVERY)**  
  Esegue il recupero del database. Questo è il comportamento predefinito. Selezionare questa opzione solo se si stanno ripristinando tutti i backup necessari. Questa opzione equivale all'opzione WITH RECOVERY in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
  **Lascia il database non operativo e non eseguire il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi possono essere ripristinati. (RESTORE WITH NORECOVERY)**  
  Il database viene lasciato nello stato di ripristino. Per recuperare il database sarà necessario eseguire un altro ripristino utilizzando l'opzione RESTORE WITH RECOVERY descritta in precedenza. Questa opzione equivale all'opzione WITH NORECOVERY in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
  Selezionando questa opzione, l'opzione **Mantieni le impostazioni di replica** non sarà disponibile.  
  
  **Lascia il database in modalità sola lettura. Esegui il rollback delle transazioni di cui non è stato eseguito il commit e salva l'operazione di rollback in un file in modo che gli effetti del recupero possano essere annullati. (RESTORE WITH STANDBY)**  
  Il database viene lasciato nello stato di standby. Questa opzione equivale all'opzione WITH STANDBY in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
  Se si seleziona questa opzione è necessario specificare un file standby.  
  
  **File di rollback**  
  Specificare un nome per il file standby nella casella di testo **File di rollback** . Questa opzione è necessaria se il database viene lasciato in modalità sola lettura (RESTORE WITH STANDBY).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-restore-files-and-filegroups"></a>Per ripristinare file e filegroup  
  
1.  Eseguire l'istruzione RESTORE DATABASE per ripristinare il backup di file e filegroup, specificando:  
  
    -   Nome del database da ripristinare.  
  
    -   Dispositivo di backup da cui verrà ripristinato il backup completo del database.  
  
    -   Clausola FILE per ogni file da ripristinare.  
  
    -   Clausola FILEGROUP per ogni filegroup da ripristinare.  
  
    -   Clausola NORECOVERY. Se i file non sono stati modificati dopo la creazione del backup, specificare la clausola RECOVERY.  
  
2.  Se i file sono stati modificati dopo la creazione del backup, eseguire l'istruzione RESTORE LOG per applicare il backup del log delle transazioni, specificando:  
  
    -   Nome del database a cui verrà applicato il log delle transazioni.  
  
    -   Dispositivo di backup da cui verrà ripristinato il backup del log delle transazioni.  
  
    -   Clausola NORECOVERY se è disponibile un altro backup del log delle transazioni successivo a quello corrente. In caso contrario, specificare la clausola RECOVERY.  
  
         Nei backup del log delle transazioni, se applicati, deve essere incluso il periodo di tempo intercorso dall'ultimo backup di file e filegroup fino alla fine del log, a meno che non vengano ripristinati TUTTI i file di database.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio vengono ripristinati i file e i filegroup per il database `MyDatabase` . Per ripristinare il database all'ora corrente, verranno applicati due log delle transazioni.  
  
```sql  
USE master;  
GO  
-- Restore the files and filesgroups for MyDatabase.  
RESTORE DATABASE MyDatabase  
   FILE = 'MyDatabase_data_1',  
   FILEGROUP = 'new_customers',  
   FILE = 'MyDatabase_data_2',  
   FILEGROUP = 'first_qtr_sales'  
   FROM MyDatabase_1  
   WITH NORECOVERY;  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyDatabase  
   FROM MyDatabase_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyDatabase  
   FROM MyDatabase_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [Backup di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Creare un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)   
 [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
