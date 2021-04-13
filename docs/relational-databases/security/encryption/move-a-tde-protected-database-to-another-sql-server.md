---
title: Spostare un database protetto da TDE in un'altra istanza di SQL Server
description: Questo argomento illustra come proteggere un database tramite TDE (Transparent Data Encryption) e spostare il database in un'altra istanza di SQL Server usando SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption, moving
- TDE, moving a database
ms.assetid: fb420903-df54-4016-bab6-49e6dfbdedc7
author: shohamMSFT
ms.author: shohamd
ms.openlocfilehash: d4024c5f1833eb7013e67cfa082d6924c868d0eb
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298830"
---
# <a name="move-a-tde-protected-database-to-another-sql-server"></a>Spostare un database protetto da TDE in un'altra istanza di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come proteggere un database tramite TDE (Transparent Data Encryption) e spostare il database in un'altra istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. TDE esegue la crittografia e la decrittografia delle operazioni di I/O di file di dati e log in tempo reale. La crittografia usa una chiave di crittografia del database (DEK) che viene archiviata nel record di avvio del database per la disponibilità durante il ripristino. La chiave DEK è una chiave simmetrica protetta tramite un certificato archiviato nel database **master** del server o una chiave asimmetrica protetta da un modulo EKM.   
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Quando si sposta il database protetto con TDE, è necessario spostare anche la chiave asimmetrica o il certificato usato per aprire la chiave di decrittografia. La chiave asimmetrica o il certificato deve essere installato nel database **master** del server di destinazione in modo che in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sia possibile accedere ai file del database. Per altre informazioni sulla crittografia trasparente del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
-   Ai fini del recupero del certificato, è necessario mantenere copie sia del file del certificato sia del file della chiave privata. La password per la chiave primaria non deve essere uguale a quella della chiave master del database.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] archivia i file creati qui in **C:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA** . I nomi e i percorsi dei file possono essere diversi.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
  
-   È necessaria l'autorizzazione **CONTROL DATABASE** per il database **master** per creare la chiave master del database.  
  
-   È necessaria l'autorizzazione **CREATE CERTIFICATE** per il database **master** per creare il certificato che consente di proteggere la chiave di decrittografia.  
  
-   Sono necessarie l'autorizzazione **CONTROL DATABASE** per il database crittografato e l'autorizzazione **VIEW DEFINITION** per la chiave asimmetrica o il certificato usato per crittografare la chiave di crittografia del database.  
  
##  <a name="to-create-a-database-protected-by-transparent-data-encryption"></a><a name="SSMSProcedure"></a> Per creare un database protetto con TDE  

Nelle procedure riportate di seguito viene illustrato come creare un database protetto da Transparent Data Encryption utilizzando SQL Server Management Studio e utilizzando Transact-SQL.
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSCreate"></a> Utilizzo di SQL Server Management Studio  
  
1.  Creare una chiave master del database e il certificato nel database **master** . Per altre informazioni, vedere **Uso di Transact-SQL** di seguito.  
  
2.  Creare un backup del certificato del server nel database **master** . Per altre informazioni, vedere **Uso di Transact-SQL** di seguito.  
  
3.  In Esplora oggetti fare clic con il pulsante destro del mouse sulla cartella **Database** e selezionare **Nuovo database**.  
  
4.  Nella finestra di dialogo **Nuovo database** digitare il nome del nuovo database nella casella **Nome database** .  
  
5.  Nella casella **Proprietario** digitare il nome del proprietario del nuovo database. In alternativa, fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Seleziona proprietario database**. Per altre informazioni sulla creazione di un nuovo database, vedere [Create a Database](../../../relational-databases/databases/create-a-database.md).  
  
6.  In Esplora oggetti fare clic sul segno più per espandere la cartella **Database** .  
  
7.  Fare clic con il pulsante destro del mouse sul database creato, scegliere **Attività** e quindi fare clic su **Gestione crittografia del database**.  
  
     Nella finestra di dialogo **Gestione crittografia del database** sono disponibili le opzioni indicate di seguito.  
  
     **Algoritmo di crittografia**  
     Visualizza o imposta l'algoritmo da usare per la crittografia del database. L'algoritmo predefinito è **AES128** . Il campo non può essere vuoto. Per altre informazioni sugli algoritmi di crittografia, vedere [Choose an Encryption Algorithm](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
     **Usa certificato server**  
     Imposta la sicurezza della crittografia mediante un certificato. Selezionarne uno dall'elenco. Se non si ha l'autorizzazione **VIEW DEFINITION** per i certificati del server, l'elenco sarà vuoto. Se viene selezionato un metodo certificato di crittografia, il valore non può essere vuoto. Per altre informazioni sui certificati, vedere [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
     **Usa chiave asimmetrica server**  
     Imposta la sicurezza della crittografia mediante una chiave asimmetrica. Vengono visualizzate solo le chiavi asimmetriche disponibili. Solo una chiave asimmetrica protetta da un modulo EKM può crittografare un database tramite Transparent Data Encryption.  
  
     **Attiva crittografia del database**  
     Modifica il database per abilitare (se selezionata) o disabilitare la funzionalità TDE (se deselezionata).  
  
8.  Al termine, fare clic su **OK**.  

###  <a name="using-transact-sql"></a><a name="TsqlCreate"></a> Uso di Transact-SQL  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```sql  
    -- Create a database master key and a certificate in the master database.  
    USE master ;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    CREATE CERTIFICATE TestSQLServerCert   
    WITH SUBJECT = 'Certificate to protect TDE key'  
    GO  
    -- Create a backup of the server certificate in the master database.  
    -- The following code stores the backup of the certificate and the private key file in the default data location for this instance of SQL Server   
    -- (C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA).  
  
    BACKUP CERTIFICATE TestSQLServerCert   
    TO FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Create a database to be protected by TDE.  
    CREATE DATABASE CustRecords ;  
    GO  
    -- Switch to the new database.  
    -- Create a database encryption key, that is protected by the server certificate in the master database.   
    -- Alter the new database to encrypt the database using TDE.  
    USE CustRecords;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM = AES_128  
    ENCRYPTION BY SERVER CERTIFICATE TestSQLServerCert;  
    GO  
    ALTER DATABASE CustRecords  
    SET ENCRYPTION ON;  
    GO  
    ```  
  
 Per altre informazioni, vedere:  
  
-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-certificate-transact-sql.md)  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../../t-sql/statements/create-database-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql.md)  
  
##  <a name="to-move-a-database-protected-by-transparent-data-encryption"></a><a name="TsqlProcedure"></a> Per spostare un database protetto con TDE 

Nelle procedure riportate di seguito viene illustrato come spostare un database protetto da Transparent Data Encryption utilizzando SQL Server Management Studio e utilizzando Transact-SQL.
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSMove"></a> Utilizzo di SQL Server Management Studio  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse sul database crittografato in precedenza, scegliere **Attività** e fare clic su **Scollega...** .  
  
     Nella finestra di dialogo **Scollega database** sono disponibili le opzioni seguenti.  
  
     **Database da scollegare**  
     Consente di visualizzare i database da scollegare.  
  
     **Nome database**  
     Consente di visualizzare il nome del database da scollegare.  
  
     **Interrompi connessioni**  
     Consente di interrompere le connessioni al database specificato.  
  
    > [!NOTE]  
    >  Non è possibile scollegare un database con connessioni attive.  
  
     **Aggiorna statistiche**  
     Per impostazione predefinita, con l'operazione di scollegamento è possibile mantenere eventuali statistiche di ottimizzazione non aggiornate prima di scollegare il database. Per aggiornare le statistiche di ottimizzazione esistenti, fare clic su questa casella di controllo.  
  
     **Mantieni cataloghi full-text**  
     Per impostazione predefinita, con l'operazione di scollegamento è possibile mantenere eventuali cataloghi full-text associati al database. Per rimuoverli, deselezionare la casella di controllo **Mantieni cataloghi full-text** . Questa opzione è visualizzata solo quando si aggiorna un database da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
     **Status**  
     Consente di visualizzare uno degli stati seguenti: **Pronto** o **Non pronto**.  
  
     **Messaggio**  
     Nella colonna **Messaggio** possono essere visualizzate informazioni sul database simili alle seguenti:  
  
    -   Quando un database è coinvolto nella replica, lo **Stato** è **Non pronto** e nella colonna **Messaggio** viene visualizzato **Database replicato**.  
  
    -   Quando un database ha una o più connessioni attive, lo **Stato** è **Non pronto** e nella colonna **Messaggio** viene visualizzato _\<number\_of\_active\_connections\>_ **Connessioni attive**, ad esempio: **Connessioni attive: 1**. Prima di poter scollegare il database è necessario disconnettere tutte le connessioni attive selezionando **Interrompi connessioni**.  
  
     Per ottenere ulteriori informazioni su un messaggio, fare clic sul testo del collegamento ipertestuale per aprire Monitoraggio attività.  
  
2.  Fare clic su **OK**.  
  
3.  Usando Esplora risorse spostare o copiare i file del database dal server di origine nello stesso percorso nel server di destinazione.  
  
4.  Usando Esplora risorse spostare o copiare il backup dei file del certificato del server e della chiave privata dal server di origine nello stesso percorso nel server di destinazione.  
  
5.  Creare una chiave master del database nell'istanza di destinazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere **Uso di Transact-SQL** di seguito.  
  
6.  Ricreare il certificato del server usando il file di backup del certificato del server originale. Per altre informazioni, vedere **Uso di Transact-SQL** di seguito.  
  
7.  In Esplora oggetti in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], fare clic con il pulsante destro del mouse sulla cartella **Database** e selezionare **Collega...** .  
  
8.  Nella finestra di dialogo **Collega database** , in **Database da collegare** fare clic su **Aggiungi**.  
  
9. Nella finestra di dialogo **Individua file di database -** _nome\_server_ selezionare il file di database da collegare al nuovo server e fare clic su **OK**.  
  
     Nella finestra di dialogo **Collega database** sono disponibili le opzioni seguenti.  
  
     **Database da collegare**  
     Consente di visualizzare informazioni sui database selezionati.  
  
     \<no column header>  
     Consente di visualizzare un'icona che indica lo stato dell'operazione di collegamento. Le icone possibili sono illustrate di seguito nella descrizione di **Stato** .  
  
     **Percorso file MDF**  
     Consente di visualizzare il percorso e il nome del file MDF selezionato.  
  
     **Nome database**  
     Consente di visualizzare il nome del database.  
  
     **Collega come**  
     Facoltativamente, è possibile specificare un nome diverso per il database da collegare.  
  
     **Proprietario**  
     Consente di visualizzare un elenco a discesa di possibili proprietari del database in cui è possibile selezionare un proprietario diverso.  
  
     **Status**  
     Consente di visualizzare lo stato del base in base alla tabella seguente.  
  
    |Icona|Testo Stato|Descrizione|  
    |----------|-----------------|-----------------|  
    |(Nessuna icona)|(Nessun testo)|L'operazione di collegamento non è stata avviata o può essere sospesa per questo oggetto. È il valore predefinito all'apertura della finestra di dialogo.|  
    |Triangolo verde che punta a destra|In corso|L'operazione di collegamento è stata avviata ma non ancora completata.|  
    |Segno di spunta verde|Operazione completata|L'oggetto è stato collegato.|  
    |Cerchio rosso con croce bianca|Errore|Si è verificato un errore durante l'operazione. Il collegamento non è stato completato.|  
    |Cerchio con due quadranti neri a destra e a sinistra e due quadranti bianchi in alto e in basso|Arrestato|L'operazione di collegamento non è stata completata perché l'utente ne ha arrestato l'esecuzione.|  
    |Cerchio con freccia curva che punta in senso antiorario.|È stato eseguito il rollback|L'operazione di collegamento è stata completata ma ne è stato eseguito il rollback a causa di un errore durante il collegamento di un altro oggetto.|  
  
     **Messaggio**  
     Non viene visualizzato alcun messaggio oppure viene visualizzato il collegamento ipertestuale "Impossibile trovare il file".  
  
     **Aggiungere**  
     Consente di individuare i file principali del database necessari. Se l'utente seleziona un file con estensione mdf, le informazioni appropriate vengono inserite automaticamente nei rispettivi campi della griglia **Database da collegare** .  
  
     **Rimuovi**  
     Consente di rimuovere il file selezionato dalla griglia **Database da collegare** .  
  
     **"** _<nome_database>_ **" dettagli database**  
     Consente di visualizzare i nomi dei file da collegare. Per verificare o modificare il percorso di un file, fare clic sul pulsante **Sfoglia** ( **...** ).  
  
    > [!NOTE]  
    >  Se il file non esiste, nella colonna **Messaggio** verrà visualizzato il testo "File non trovato". Se non rilevato, un file di log può trovarsi in un'altra directory o essere stato eliminato. È necessario aggiornare il percorso del file nella griglia **Dettagli database** in modo che indichi la posizione corretta oppure rimuovere il file di log dalla griglia. Se non viene rilevato un file di dati con estensione ndf, è necessario aggiornare il percorso nella griglia in modo che indichi la posizione corretta.  
  
     **Nome file originale**  
     Consente di visualizzare il nome del file collegato appartenente al database.  
  
     **Tipo di file**  
     Indica il tipo di file, ovvero **Dati** o **Log**.  
  
     **Percorso file corrente**  
     Consente di visualizzare il percorso del file di database selezionato. Il percorso può essere modificato manualmente.  
  
     **Messaggio**  
     Non viene visualizzato alcun messaggio oppure viene visualizzato il collegamento ipertestuale **Impossibile trovare il file**.  
  
###  <a name="using-transact-sql"></a><a name="TsqlMove"></a> Uso di Transact-SQL  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```sql  
    -- Detach the TDE protected database from the source server.   
    USE master ;  
    GO  
    EXEC master.dbo.sp_detach_db @dbname = N'CustRecords';  
    GO  
    -- Move or copy the database files from the source server to the same location on the destination server.   
    -- Move or copy the backup of the server certificate and the private key file from the source server to the same location on the destination server.   
    -- Create a database master key on the destination instance of SQL Server.   
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    -- Recreate the server certificate by using the original server certificate backup file.   
    -- The password must be the same as the password that was used when the backup was created.  
  
    CREATE CERTIFICATE TestSQLServerCert   
    FROM FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        DECRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Attach the database that is being moved.   
    -- The path of the database files must be the location where you have stored the database files.  
    CREATE DATABASE [CustRecords] ON   
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords.mdf' ),  
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords_log.LDF' )  
    FOR ATTACH ;  
    GO  
    ```  
  
 Per altre informazioni, vedere:  
  
-   [sp_detach_db &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../../t-sql/statements/create-database-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  
