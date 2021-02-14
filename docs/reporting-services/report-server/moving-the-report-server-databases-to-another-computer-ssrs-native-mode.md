---
title: Spostare database del server di report in un altro computer (modalità nativa) | Microsoft Docs
description: È possibile spostare i database del server di report usati in un'installazione del motore di database di SQL Server in un'istanza di un computer diverso.
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 44a9854d-e333-44f6-bdc7-8837b9f34416
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 58d7a46df8eb6580961d54e02f3ee6b0c57ee32f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059326"
---
# <a name="moving-report-server-databases-to-another-computer-ssrs-native-mode"></a>Spostamento di database del server di report in un altro computer (modalità nativa SSRS)

  È possibile spostare i database del server di report usati in un'installazione del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di [!INCLUDE[ssDE](../../includes/ssde-md.md)] in un'istanza di un computer diverso. I database reportserver e reportservertempdb devono essere spostati o copiati insieme. Per un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sono necessari entrambi i database. Il database reportservertempdb deve essere correlato tramite il nome al database reportserver primario che si sta spostando.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  Modalità nativa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Lo spostamento di un database non influisce sulle operazioni pianificate attualmente definite per gli elementi del server di report.  
  
-   Le pianificazioni vengono ricreate la prima volta che si riavvia il servizio del server di report.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] In Agent i processi usati per attivare una pianificazione verranno ricreati nella istanza di database. Non è necessario spostare i processi nel nuovo computer, ma è necessario eliminare quelli che non verranno più utilizzati.  
  
-   Le sottoscrizioni, gli snapshot e i report memorizzati nella cache vengono mantenuti nel database spostato. Se uno snapshot non include i dati aggiornati dopo lo spostamento del database, deselezionare le opzioni relative allo snapshot e selezionare **Applica** per salvare le modifiche, creare di nuovo la pianificazione e selezionare ancora una volta **Applica** per salvare le modifiche.  
  
-   Il report temporaneo e i dati della sessione utente archiviati nel database reportservertempdb vengono mantenuti quando si sposta il database.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili diversi modi per spostare i database, tra cui backup e ripristino, collegamento e scollegamento e copia. Non tutti gli approcci sono appropriati per spostare un database esistente in una nuova istanza del server. L'approccio da utilizzare per spostare il database del server di report dipende dai requisiti di disponibilità del sistema. Il modo più semplice per spostare i database del server di report consiste nel collegarli e scollegarli. Questo approccio richiede tuttavia di portare in modalità offline il server di report mentre lo si scollega. Il backup e il ripristino rappresentano un'opzione migliore se si desidera ridurre al minimo le interruzioni del servizio, tuttavia per eseguire queste operazioni è necessario usare i comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] . La copia del database, in particolare l'utilizzo della procedura Copia guidata database, non è consigliabile in quanto non consente di mantenere le impostazioni delle autorizzazioni nel database.  
  
> [!IMPORTANT]  
>  È consigliabile eseguire la procedura descritta in questo articolo quando lo spostamento del database del server di report è l'unica modifica che si intende apportare all'installazione esistente. Per la migrazione di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] completa, vale a dire lo spostamento del database e la modifica dell'identità del servizio Windows ReportServer usato dal database, è necessario riconfigurare le informazioni di connessione e reimpostare la chiave di crittografia.  
  
## <a name="detaching-and-attaching-the-report-server-databases"></a>Scollegamento e collegamento dei database del server di report  
 Se il server di report può essere portato in modalità offline, è possibile scollegare i database per spostarli nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da utilizzare. In questo modo, è possibile mantenere le autorizzazioni presenti nei database. Se si usa un database SQL Server, è necessario spostarlo in un'altra istanza di SQL Server. Dopo avere spostato i database, è necessario riconfigurare la connessione del server di report al database del server di report. Se si sta eseguendo una distribuzione con scalabilità orizzontale, è necessario riconfigurare la connessione al database del server di report per ogni server di report della distribuzione.  
  
 Per spostare i database, eseguire la procedura seguente:  
  
1.  Eseguire il backup delle chiavi di crittografia per il database del server di report da spostare. Per eseguire questa operazione, è possibile usare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
2.  Arrestare il servizio del server di report. Per eseguire questa operazione, è possibile utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
3.  Avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e stabilire una connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita i database del server di report.  
  
4.  Fare clic con il pulsante destro del mouse sul database del server di report, scegliere Attività, quindi **Scollega**. Ripetere il passaggio per il database temporaneo del server di report.  
  
5.  Copiare o spostare i file con estensione mdf e ldf nella cartella Dati dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da utilizzare. Poiché si stanno spostando due database, verificare di spostare o copiare tutti e quattro i file.  
  
6.  In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]stabilire una connessione alla nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospiterà i database del server di report.  
  
7.  Fare clic con il pulsante destro del mouse sul nodo Database, quindi scegliere **Collega**.  
  
8.  Fare clic su **Aggiungi** per selezionare i file con estensione mdf e ldf del database del server di report che si desidera collegare. Ripetere il passaggio per il database temporaneo del server di report.  
  
9. Dopo avere collegato i database, verificare che **RSExecRole** sia un ruolo del database nel database del server di report e nel database temporaneo. Il ruolo **RSExecRole** deve disporre delle autorizzazioni di selezione, inserimento, aggiornamento, eliminazione e riferimento nelle tabelle del database del server di report e delle autorizzazioni di esecuzione nelle stored procedure. Per altre informazioni, vedere [Creare RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
10. Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi al server di report.  
  
11. Nella pagina Database selezionare la nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , quindi fare clic su **Connetti**.  
  
12. Selezionare il database del server di report appena spostato, quindi fare clic su **Applica**.  
  
13. Nella pagina Chiavi di crittografia fare clic su Ripristina. Specificare il file che contiene la copia di backup delle chiavi e la password per sbloccare il file.  
  
14. Riavviare il servizio del server di report.  
  
## <a name="backing-up-and-restoring-the-report-server-databases"></a>Backup e ripristino dei database del server di report  
 Se il server di report non può essere portato in modalità offline, è possibile rilocare i database del server di report tramite backup e ripristino. Per eseguire queste due operazioni, è necessario usare le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] . Dopo aver ripristinato i database, è necessario configurare il server di report per utilizzare il database nella nuova istanza del server. Per ulteriori informazioni, vedere le istruzioni alla fine di questo argomento.  
  
### <a name="using-backup-and-copy_only-to-backup-the-report-server-databases"></a>Utilizzo di BACKUP e COPY_ONLY per eseguire il backup dei database del server di report  
 Quando si esegue il backup dei database, impostare l'argomento COPY_ONLY. Accertarsi di eseguire il backup di entrambi i database e dei file di log.  
  
```  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServer  
   SET RECOVERY FULL  
  
-- If the ReportServerData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerData.bak'  
  
-- Create a logical backup device, ReportServerLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerLog.bak'  
  
-- Back up the full ReportServer database.  
BACKUP DATABASE ReportServer  
   TO ReportServerData  
   WITH COPY_ONLY  
  
-- Back up the ReportServer log.  
BACKUP LOG ReportServer  
   TO ReportServerLog  
   WITH COPY_ONLY  
  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServerTempdb  
   SET RECOVERY FULL  
  
-- If the ReportServerTempDBData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBData',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBData.bak'  
  
-- Create a logical backup device, ReportServerTempDBLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBLog.bak'  
  
-- Back up the full ReportServerTempDB database.  
BACKUP DATABASE ReportServerTempDB  
   TO ReportServerTempDBData  
   WITH COPY_ONLY  
  
-- Back up the ReportServerTempDB log.  
BACKUP LOG ReportServerTempDB  
   TO ReportServerTempDBLog  
   WITH COPY_ONLY  
```  
  
### <a name="using-restore-and-move-to-relocate-the-report-server-databases"></a>Utilizzo di RESTORE e MOVE per spostare i database del server di report  
 Quando si ripristinano i database, accertarsi di includere l'argomento MOVE per poter specificare un percorso. Utilizzare l'argomento NORECOVERY per eseguire il ripristino iniziale. In questo modo, il database viene mantenuto in uno stato RESTORING, consentendo di analizzare i backup dei log per determinare quali ripristinare. Nel passaggio finale l'operazione RESTORE viene ripetuta con l'argomento RECOVERY.  
  
 Per l'argomento MOVE viene utilizzato il nome logico del file di dati. Per individuare il nome logico, eseguire l'istruzione `RESTORE FILELISTONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Negli esempi seguenti viene incluso l'argomento FILE in modo che sia possibile specificare la posizione del file di log da ripristinare. Per individuare la posizione del file, eseguire l'istruzione `RESTORE HEADERONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Durante il ripristino del database e dei file di log, eseguire ogni operazione RESTORE separatamente.  
  
```  
-- Restore the report server database and move to new instance folder   
RESTORE DATABASE ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore the report server log file to new instance folder   
RESTORE LOG ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore and move the report server temporary database  
RESTORE DATABASE ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Restore the temporary database log file to new instance folder   
RESTORE LOG ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServer  
   WITH RECOVERY  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServerTempDB  
   WITH RECOVERY  
GO  
```  
  
### <a name="how-to-configure-the-report-server-database-connection"></a>Come configurare la connessione al database del server di report  
  
1.  Avviare Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi al server di report.  
  
2.  Nella pagina Database fare clic su **Cambia database**. Fare clic su **Avanti**.  
  
3.  Fare clic su **Scegli un database del server di report esistente**. Fare clic su **Avanti**.  
  
4.  Selezionare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita attualmente il database del server di report, quindi fare clic su **Test connessione**. Fare clic su **Avanti**.  
  
5.  In Nome database selezionare il database del server di report da utilizzare. Fare clic su **Avanti**.  
  
6.  In Credenziali specificare le credenziali che il server di report utilizzerà per la connessione al database relativo. Fare clic su **Avanti**.  
  
7.  Fare clic su **Avanti** , quindi su **Fine**.  
  
> [!NOTE]  
>  Per un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è necessario che nell'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sia incluso il ruolo **RSExecRole** . Quando si imposta la connessione al database del server di report tramite lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vengono eseguite le operazioni di creazione dei ruoli, registrazione dell'account di accesso e assegnazione di ruoli. Se per configurare la connessione si utilizzano approcci alternativi, in particolare l'utilità della riga di comando rsconfig.exe, il server di report non si troverà in uno stato attivo. Per renderlo disponibile il server di report, potrebbe essere necessario scrivere codice WMI. Per altre informazioni, vedere [Accedere al provider WMI per Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  

## <a name="next-steps"></a>Passaggi successivi

[Creare RSExecRole](../../reporting-services/security/create-the-rsexecrole.md)   
[Avviare e arrestare il servizio del server di report](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
[Configurare una connessione del database del server di report](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[Configurare l'account di esecuzione automatica](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)   
[Gestione configurazione del server di report](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[Utilità rsconfig](../../reporting-services/tools/rsconfig-utility-ssrs.md)   
[Configurare e gestire le chiavi di crittografia](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)   
[Database del server di report](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
