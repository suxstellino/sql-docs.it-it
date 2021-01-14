---
description: Spostare i database di sistema
title: Spostare i database di sistema | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- moving system databases
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- tempdb database [SQL Server], moving
- system databases [SQL Server], moving
- scheduled disk maintenace [SQL Server]
- moving databases
- msdb database [SQL Server], moving
- moving database files
- relocating database files
- planned database relocations [SQL Server]
- master database [SQL Server], moving
- model database [SQL Server], moving
- Resource database [SQL Server]
- databases [SQL Server], moving
ms.assetid: 72bb62ee-9602-4f71-be51-c466c1670878
author: stevestein
ms.author: sstein
ms.openlocfilehash: c24a98684e87eb94a3cd9e100f203509789b7a0f
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031110"
---
# <a name="move-system-databases"></a>Spostare i database di sistema
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene descritta la procedura per lo spostamento dei database di sistema in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lo spostamento dei database di sistema può risultare utile nelle situazioni seguenti:  
  
-   Recupero da errore. Ad esempio, il database è in modalità sospetta oppure viene chiuso a causa di un errore hardware.  
  
-   Rilocazione pianificata.  
  
-   Rilocazione per una manutenzione pianificata del disco.  
  
 Le procedure seguenti consentono di spostare i file di database all'interno della stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per spostare un database in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in un altro server, usare l'operazione di [backup e ripristino](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) .  

 Le procedure descritte in questo argomento richiedono il nome logico dei file di database. Per ottenere il nome, eseguire una query sulla colonna name della vista del catalogo [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) .  
  
> [!IMPORTANT]  
>  Se si sposta un database di sistema e successivamente si ricompila il database master, è necessario spostare nuovamente il database di sistema, in quanto l'operazione di ricompilazione ha come conseguenza l'installazione di tutti i database di sistema nei rispettivi percorsi predefiniti.  

> [!IMPORTANT]  
>  Dopo lo spostamento dei file, l'account del servizio [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] deve avere l'autorizzazione per accedere ai file nel nuovo percorso della cartella di file. Per altre informazioni, vedere [Configurare le autorizzazioni del file system per l'accesso al motore di database](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md).
    
  
##  <a name="planned-relocation-and-scheduled-disk-maintenance-procedure"></a><a name="Planned"></a> Procedura di rilocazione pianificata e manutenzione pianificata del disco  
 Per spostare un file di dati o di log del database di sistema nell'ambito di un'operazione di rilocazione pianificata o di manutenzione pianificata, attenersi alla procedura seguente. Questa procedura è valida per tutti i database di sistema ad eccezione dei database master e Resource.  
  
1.  Per ogni file che si desidera spostare, eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
2.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o arrestare il sistema per eseguire la manutenzione. Per altre informazioni, vedere [Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Spostare il file o i file nella nuova posizione e verificare che l'account del servizio [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] abbia ancora dell'autorizzazione per accedervi.

4.  Riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il server. Per altre informazioni, vedere [Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
5.  Verificare la modifica ai file eseguendo la query riportata di seguito.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
 Se il database msdb viene spostato e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata per [Posta elettronica database](../../relational-databases/database-mail/database-mail.md), completare i passaggi aggiuntivi seguenti.  
  
1.  Verificare che [!INCLUDE[ssSB](../../includes/sssb-md.md)] sia abilitato per il database msdb eseguendo la query seguente.  
  
    ```  
    SELECT is_broker_enabled   
    FROM sys.databases  
    WHERE name = N'msdb';  
    ```  
  
     Per altre informazioni su come abilitare [!INCLUDE[ssSB](../../includes/sssb-md.md)], vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
2.  Verificare il funzionamento di Posta elettronica database inviando un messaggio di prova.  
  
##  <a name="failure-recovery-procedure"></a><a name="Failure"></a> Procedura di recupero da errore  
 Se è necessario spostare un file a causa di un errore hardware, eseguire la procedura seguente per rilocare il file in una nuova posizione. Questa procedura è valida per tutti i database di sistema ad eccezione dei database master e Resource.  
  
> [!IMPORTANT]  
>  Se non è possibile avviare il database, ovvero se il database è in modalità sospetta o in stato non recuperato, il file può essere spostato solo dai membri del ruolo predefinito sysadmin.  
  
1.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se avviata.  
  
2.  Avviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità di recupero del solo database master digitando uno dei comandi seguenti al prompt dei comandi. I parametri specificati in questi comandi fanno distinzione tra maiuscole e minuscole. I comandi hanno esito negativo se i parametri non vengono specificati come illustrato.  
  
    -   Per l'istanza predefinita (MSSQLSERVER), eseguire il comando seguente:  
  
        ```  
        NET START MSSQLSERVER /f /T3608
        ```  
  
    -   Per un'istanza denominata, eseguire il comando riportato di seguito:  
  
        ```  
        NET START MSSQL$instancename /f /T3608
        ```  
  
     Per altre informazioni, vedere [Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Per ogni file da spostare, usare i comandi **sqlcmd** oppure [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
     Per altre informazioni su come usare l'utilità **sqlcmd** , vedere [Usare l'utilità sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Uscire dall'utilità **sqlcmd** o [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eseguire, ad esempio, `NET STOP MSSQLSERVER`.  
  
6.  Spostare il file o i file nella nuova posizione.  
  
7.  Riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eseguire, ad esempio, `NET START MSSQLSERVER`.  
  
8.  Verificare la modifica ai file eseguendo la query riportata di seguito.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
##  <a name="moving-the-master-database"></a><a name="master"></a> Spostamento del database master  
 Per spostare il database master, effettuare le operazioni seguenti.  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, **Microsoft SQL Server**, **Strumenti di configurazione** e quindi fare clic su **Gestione configurazione SQL Server**.  
  
2.  Nel nodo **Servizi di SQL Server** fare clic con il pulsante destro del mouse sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio **SQL Server (MSSQLSERVER)** e scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà (** _nome_istanza_ **) di SQL Server** fare clic sulla scheda **Parametri di avvio** .  
  
4.  Nella casella **Parametri esistenti** selezionare il parametro -d. Nella casella **Specificare un parametro di avvio** impostare il parametro sul nuovo percorso del file di *dati* master. Per salvare le modifiche, fare clic su **Aggiorna** .
  
5.  Nella casella **Parametri esistenti** selezionare il parametro -l. Nella casella **Specificare un parametro di avvio** impostare il parametro sul nuovo percorso del file di *log* master. Per salvare le modifiche, fare clic su **Aggiorna** .

     Il valore del parametro per il file di dati deve seguire il parametro `-d` e il valore per il file di log deve seguire il parametro `-l` . L'esempio seguente illustra i valori dei parametri per il percorso predefinito del file di dati master.  
  
     `-dC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\master.mdf`  
  
     `-lC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\mastlog.ldf`  
  
     Se la rilocazione pianificata del file di dati master è `E:\SQLData`, i valori dei parametri verranno modificati nel modo seguente:  
  
     `-dE:\SQLData\master.mdf`  
  
     `-lE:\SQLData\mastlog.ldf`  

6.  Fare clic su **OK** per salvare in modo permanente le modifiche e chiudere la finestra di dialogo **Proprietà (** _nome istanza_ **)** di SQL Server.

7.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] facendo clic con il pulsante destro del mouse sul nome dell'istanza e scegliendo **Arresta**.  
  
8.  Spostare i file master.mdf e mastlog.ldf nel nuovo percorso.  
  
9.  Riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
10. Verificare la modifica dei file per il database master eseguendo la query seguente.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID('master');  
    GO  
    ```  

11. A questo punto SQL Server dovrebbe essere eseguito normalmente. Microsoft consiglia comunque di modificare anche la voce del Registro di sistema in `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\instance_ID\Setup`, dove *instance_ID* è simile a `MSSQL13.MSSQLSERVER`. In tale hive impostare il valore `SQLDataRoot` sul nuovo percorso. Se non si aggiorna il Registro di sistema, l'applicazione di patch e l'aggiornamento possono avere esito negativo.

  
##  <a name="moving-the-resource-database"></a><a name="Resource"></a> Spostamento del database delle risorse  
 Il percorso del database Resource è \<*drive*>:\Programmi\Microsoft SQL Server\MSSQL\<version>.\<*instance_name*>\MSSQL\Binn\\. Il database non può essere spostato.  
  
##  <a name="follow-up-after-moving-all-system-databases"></a><a name="Follow"></a> Completamento: Dopo lo spostamento di tutti i database di sistema  
 Se tutti i database di sistema sono stati spostati in un nuovo volume o unità oppure in un altro server con una lettera di unità diversa, effettuare gli aggiornamenti riportati di seguito.  
  
-   Modificare il percorso del log di SQL Server Agent Se non si aggiorna questo percorso, non sarà possibile avviare SQL Server Agent.  
  
-   Modificare il percorso predefinito del database. La creazione di un nuovo database potrebbe non venir completata correttamente se la lettera di unità e il percorso specificati come posizione predefinita non esistono.  
  
#### <a name="change-the-sql-server-agent-log-path"></a>Modificare il percorso del log di SQL Server Agent  
  
1.  In Esplora oggetti di SQL Server Management Studio espandere **SQL Server Agent**.  
  
2.  Fare clic con il pulsante destro del mouse su **Log degli errori** e scegliere **Configura**.  
  
3.  Nella finestra di dialogo **Configura log degli errori di SQL Server Agent** specificare il nuovo percorso del file SQLAGENT.OUT. Il percorso predefinito è C:\Programmi\Microsoft SQL Server\MSSQL\<version>.<nome_istanza>\MSSQL\Log\\.  
  
#### <a name="change-the-database-default-location"></a>Modificare il percorso predefinito del database  
  
1.  In Esplora oggetti di SQL Server Management Studio fare clic con il pulsante destro del mouse sul server SQL Server e scegliere **Proprietà**.  
  
2.  Nella finestra di dialogo **Proprietà server** selezionare **Impostazioni database**.  
  
3.  In **Percorsi predefiniti database** selezionare il nuovo percorso sia per i file di dati sia per quelli di log.  
  
4.  Per completare la modifica, avviare e arrestare il servizio SQL Server.  
  
##  <a name="examples"></a><a name="Examples"></a> Esempi  
  
### <a name="a-moving-the-tempdb-database"></a>R. Spostamento del database tempdb  
 Nell'esempio seguente i file dei dati e di log del database `tempdb` vengono spostati in un nuovo percorso nell'ambito di una rilocazione pianificata.  
  
> [!NOTE]  
>  Poiché tempdb viene ricreato a ogni avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , non è necessario spostare fisicamente i file di dati e di log. I file vengono creati nella nuova posizione quando il servizio viene riavviato nel passaggio 3. Fino al riavvio del servizio, il database tempdb continuerà a utilizzare i file di dati e di log nella posizione esistente.  
  
1.  Determinare i nomi dei file logici del database `tempdb` e la relativa posizione corrente sul disco.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    GO  
    ```  
  
2.  Modificare il percorso di ogni file tramite `ALTER DATABASE`.  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = templog, FILENAME = 'F:\SQLLog\templog.ldf');  
    GO  
    ```  
  
3.  Arrestare e riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Verificare la modifica ai file.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    ```  
  
5.  Eliminare i file `tempdb.mdf` e `templog.ldf` dal percorso originale.  
  
## <a name="see-also"></a>Vedere anche  
 [Database Resource](../../relational-databases/databases/resource-database.md)   
 [Database tempdb](../../relational-databases/databases/tempdb-database.md)   
 [Database master](../../relational-databases/databases/master-database.md)   
 [Database msdb](../../relational-databases/databases/msdb-database.md)   
 [Database model](../../relational-databases/databases/model-database.md)   
 [Spostare database utente](../../relational-databases/databases/move-user-databases.md)   
 [Spostare file del database](../../relational-databases/databases/move-database-files.md)   
 [Avviare, arrestare, sospendere, riprendere, riavviare il motore di database, SQL Server Agent o SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Ricompilare database di sistema](../../relational-databases/databases/rebuild-system-databases.md)  
  
