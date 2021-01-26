---
title: Ripristinare i file in una nuova posizione (SQL Server) | Microsoft Docs
description: Questo articolo illustra come ripristinare i file in una nuova posizione in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring files [SQL Server], how-to topics
- restoring databases [SQL Server], moving
- restoring files [SQL Server], steps
- file restores [SQL Server], how-to topics
- filegroups [SQL Server], restoring
- restoring filegroups [SQL Server]
ms.assetid: b4f4791d-646e-4632-9980-baae9cb1aade
author: cawrites
ms.author: chadam
ms.openlocfilehash: d1b7d7b90d4893e4031477780cf96c2162f509a6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764797"
---
# <a name="restore-files-to-a-new-location-sql-server"></a>Ripristino dei file in una nuova posizione (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene descritto come ripristinare i file in un nuovo percorso in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per ripristinare i file in una nuova posizione utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   L'amministratore di sistema che esegue il ripristino dei file deve essere l'unico utente collegato al database da ripristinare.  
  
-   Non è possibile utilizzare RESTORE in una transazione esplicita o implicita.  
  
-   In base al modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, prima di poter ripristinare file è necessario eseguire il backup del log delle transazioni attivo, noto come parte finale del log. Per altre informazioni, vedere [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
-   Per ripristinare un database crittografato, è necessario poter accedere alla chiave asimmetrica o al certificato utilizzato per crittografare il database. Non è possibile effettuare l'operazione di ripristino del database senza almeno uno di questi due elementi. Di conseguenza, il certificato utilizzato per crittografare la chiave di crittografia del database deve essere conservato fino a quando il backup è necessario. Per altre informazioni, vedere [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Se il database da ripristinare non esiste, per eseguire un'operazione RESTORE l'utente deve disporre delle autorizzazioni CREATE DATABASE. Se il database esiste, le autorizzazioni per l'istruzione RESTORE vengono assegnate per impostazione predefinita ai membri dei ruoli predefiniti del server **sysadmin** e **dbcreator** e al proprietario (**dbo**) del database. Per l'opzione FROM DATABASE_SNAPSHOT, il database esiste sempre.  
  
 Le autorizzazioni per l'istruzione RESTORE vengono assegnate ai ruoli in cui le informazioni sull'appartenenza sono sempre disponibili per il server. Poiché è possibile controllare l'appartenenza ai ruoli predefiniti del database solo quando il database è accessibile e non è danneggiato, condizioni che non risultano sempre vere quando si esegue un'operazione RESTORE, i membri del ruolo predefinito del database **db_owner** non dispongono delle autorizzazioni per l'istruzione RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-restore-files-to-a-new-location"></a>Per ripristinare i file in una nuova posizione  
  
1.  In **Esplora oggetti** connettersi a un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], espandere tale istanza, quindi espandere **Database**.  
  
2.  Fare clic con il pulsante destro del mouse sul database specifico, scegliere **Attività** e scegliere **Ripristina** e fare clic su **File e filegroup**.  
  
3.  Nella pagina **Generale** nella casella di riepilogo **Database di destinazione** , immettere il database da ripristinare. È possibile immettere un nuovo database oppure sceglierne uno esistente dall'elenco a discesa. Nell'elenco sono inclusi tutti i database presenti nel server, ad eccezione dei database di sistema **master** e **tempdb**.  
  
4.  Per specificare l'origine e il percorso dei set di backup da ripristinare, fare clic su una delle opzioni seguenti:  
  
    -   **Database di origine**  
  
         Immettere il nome di un database nella casella di riepilogo. Nell'elenco sono inclusi solo i database di cui è stato eseguito il backup in base alla cronologia di backup di **msdb** .  
  
    -   **Dispositivo di origine**  
  
         Fare clic sul pulsante Sfoglia Nella finestra di dialogo **Seleziona dispositivi di backup** selezionare uno dei tipi di dispositivo elencati nella casella di riepilogo **Tipo di supporti di backup** . Per selezionare uno o più dispositivi per la casella di riepilogo **Supporti di backup** , fare clic su **Aggiungi**.  
  
         Dopo avere aggiunto i dispositivi desiderati nella casella di riepilogo **Dispositivi di backup** , fare clic su **OK** per tornare alla pagina **Generale** .  
  
5.  Nella griglia **Selezionare i set di backup da ripristinare** selezionare i set di backup che si desidera ripristinare. In questa griglia vengono visualizzati i backup disponibili per il percorso specificato. Per impostazione predefinita, viene suggerito un piano di recupero. Per ignorare il piano di recupero suggerito, è possibile modificare le impostazioni selezionate nella griglia. I backup che dipendono da un backup deselezionato vengono automaticamente deselezionati.  
  
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
  
6.  Nel riquadro **Selezione pagina** fare clic sulla pagina **Opzioni** .  
  
7.  Nella griglia **Ripristina file di database come** , specificare un nuovo percorso per il file o i file da spostare.  
  
    |Intestazione della colonna|Valori|  
    |-----------------|------------|  
    |**Nome file originale**|Percorso completo di un file di backup di origine.|  
    |**Tipo di file**|Specifica il tipo di dati nel backup: **dati**, **registro** o **dati FILESTREAM**. I dati contenuti nelle tabelle sono nei file **Dati** . I dati del log delle transazioni sono nei file **Log** . I dati BLOB (Binary Large Object, oggetto binario di grandi dimensioni) archiviati nel file system si trovano nei file **Dati FILESTREAM** .|  
    |**Ripristina come**|Percorso completo del file di database da ripristinare. Per specificare un nuovo file di ripristino, fare clic nella casella di testo e modificare il percorso e il nome del file suggeriti. La modifica del percorso o del nome file nella colonna **Ripristina come** equivale all'utilizzo dell'opzione MOVE in un'istruzione RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-restore-files-to-a-new-location"></a>Per ripristinare i file in una nuova posizione  
  
1.  Eseguire facoltativamente l'istruzione RESTORE FILELISTONLY per stabilire il numero e i nomi dei file nel backup completo del database.  
  
2.  Eseguire l'istruzione RESTORE DATABASE per ripristinare il backup completo del database, specificando:  
  
    -   Nome del database da ripristinare.  
  
    -   Dispositivo di backup da cui verrà ripristinato il backup completo del database.  
  
    -   Clausola MOVE per ogni file da ripristinare in una nuova posizione.  
  
    -   Clausola NORECOVERY.  
  
3.  Se i file sono stati modificati dopo la creazione del backup, eseguire l'istruzione RESTORE LOG per applicare il backup del log delle transazioni, specificando:  
  
    -   Nome del database a cui verrà applicato il log delle transazioni.  
  
    -   Dispositivo di backup da cui verrà ripristinato il backup del log delle transazioni.  
  
    -   Clausola NORECOVERY se è disponibile un altro backup del log delle transazioni successivo a quello corrente. In caso contrario, specificare la clausola RECOVERY.  
  
         I backup dei log delle transazioni, se utilizzati, devono coprire il periodo intercorso dall'ultimo backup dei file e dei filegroup.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 Questo esempio illustra come ripristinare in nuove posizioni nell'unità D due file del database `MyNwind` originariamente memorizzati nell'unità C. Verranno anche applicati due log delle transazioni per ripristinare il database all'ora corrente. L'istruzione `RESTORE FILELISTONLY` viene utilizzata per determinare il numero e i nomi logici e fisici dei file del database da ripristinare.  
  
```sql  
USE master;  
GO  
-- First determine the number and names of the files in the backup.  
RESTORE FILELISTONLY  
   FROM MyNwind_1;  
-- Restore the files for MyNwind.  
RESTORE DATABASE MyNwind  
   FROM MyNwind_1  
   WITH NORECOVERY,  
   MOVE 'MyNwind_data_1' TO 'D:\MyData\MyNwind_data_1.mdf',   
   MOVE 'MyNwind_data_2' TO 'D:\MyData\MyNwind_data_2.ndf';  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Copiare database tramite backup e ripristino](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [Ripristino di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
  
