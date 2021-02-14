---
title: Creare un gruppo di disponibilità con Transact-SQL (T-SQL)
description: Usare Transact-SQL per creare e configurare un gruppo di disponibilità nelle istanze di SQL Server 2019 (15.x) in cui è abilitata la funzionalità dei gruppi di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 8b0a6301-8b79-4415-b608-b40876f30066
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3bb28baa0f3cf8cfe6c414719d3ae1c377a8915f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343332"
---
# <a name="create-an-always-on-availability-group-using-transact-sql-t-sql"></a>Creare un gruppo di disponibilità Always On con Transact-SQL (T-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come utilizzare [!INCLUDE[tsql](../../../includes/tsql-md.md)] per creare e configurare un gruppo di disponibilità su istanze di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] nelle quali è abilitata la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Tramite un *gruppo di disponibilità* vengono definiti un set di database utente di cui verrà eseguito il failover come unità singola e un set di partner di failover, noti come *repliche di disponibilità*, che supportano il failover.  
  
> [!NOTE]  
>  Per un'introduzione ai gruppi di disponibilità, vedere [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
> [!NOTE]  
>  In alternativa all'utilizzo di [!INCLUDE[tsql](../../../includes/tsql-md.md)], è possibile utilizzare la procedura guidata Crea gruppo di disponibilità o i cmdlet di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Per altre informazioni, vedere [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), [Usare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md), o [Creare un gruppo di disponibilità &#40; SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md).  

  
## <a name="prerequisites-restrictions-and-recommendations"></a><a name="PrerequisitesRestrictions"></a> Prerequisiti, restrizioni e raccomandazioni  
  
-   Prima di creare un gruppo di disponibilità, verificare che le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospitano repliche di disponibilità si trovino in un nodo del Clustering di failover di Windows Server (Windows Server Failover Clustering, WSFC) diverso all'interno dello stesso cluster di failover WSFC. Inoltre, verificare che ciascuna delle istanze del server soddisfi tutti gli altri prerequisiti [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Per altre informazioni, si consiglia di leggere [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.  
  
##  <a name="using-transact-sql-to-create-and-configure-an-availability-group"></a><a name="TsqlProcedure"></a> Utilizzo di Transact-SQL per creare e configurare un gruppo di disponibilità 

###  <a name="summary-of-tasks-and-corresponding-transact-sql-statements"></a><a name="SummaryTsqlStatements"></a> Riepilogo delle attività e istruzioni Transact-SQL corrispondenti  
 Nella tabella seguente sono elencate le attività di base necessarie per la creazione e la configurazione di un gruppo di disponibilità e vengono indicate le istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] da utilizzare per queste attività. È necessario eseguire le attività [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nell'ordine con cui sono elencate nella tabella.  
  
|Attività|Istruzione/i Transact-SQL|Posizione in cui eseguire l'attività **&#42;**|  
|----------|----------------------------------|---------------------------------|  
|Creare un endpoint del mirroring del database (una volta per ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] )|[CREATE ENDPOINT](../../../t-sql/statements/create-endpoint-transact-sql.md) *nomeEndpoint* ... FOR DATABASE_MIRRORING|Eseguire in ogni istanza del server in cui non è presente l'endpoint del mirroring del database.|  
|Creare un gruppo di disponibilità|[CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)|Eseguire nell'istanza del server che dovrà ospitare la replica primaria iniziale.|  
|Creare un join della replica secondaria al gruppo di disponibilità|[ALTER AVAILABILITY GROUP](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md) *nome_gruppo* JOIN|Eseguire in ogni istanza del server in cui è ospitata una replica secondaria.|  
|Preparare il database secondario|[BACKUP](../../../t-sql/statements/backup-transact-sql.md) e [RESTORE](../../../t-sql/statements/restore-statements-transact-sql.md).|Creare i backup nell'istanza del server in cui è ospitata la replica primaria.<br /><br /> Ripristinare i backup in ogni istanza del server che ospita una replica secondaria, utilizzando RESTORE WITH NORECOVERY.|  
|Avviare la sincronizzazione dei dati creando un join di ogni database secondario al gruppo di disponibilità|[ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) *nome_database* SET HADR AVAILABILITY GROUP = *nome_gruppo*|Eseguire in ogni istanza del server in cui è ospitata una replica secondaria.|  
  
 \* Per eseguire un'attività specifica, connettersi alle istanze del server indicate.   
 
### <a name="using-transact-sql"></a>Uso di Transact-SQL 
> [!NOTE]  
>  Per una procedura di configurazione di esempio contenente esempi di codice di ognuna di queste istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)], vedere [Esempio: Configurazione di un gruppo di disponibilità in cui viene usata l'autenticazione di Windows](#ExampleConfigAGWinAuth).  
  
1.  Connettersi all'istanza del server che dovrà ospitare la replica primaria.  
  
2.  Creare il gruppo di disponibilità usando l'istruzione [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
3.  Creare un join della nuova replica secondaria al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
4.  Per ogni database nel gruppo di disponibilità, creare un database secondario ripristinando i backup recenti del database primario, usando RESTORE WITH NORECOVERY. Per altre informazioni, vedere [Esempio: Configurazione di un gruppo di disponibilità in cui viene usata l'autenticazione di Windows (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md), a partire dal passaggio per il ripristino del backup di database.  
  
5.  Creare un join di ogni nuovo database secondario al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="example-configuring-an-availability-group-that-uses-windows-authentication"></a><a name="ExampleConfigAGWinAuth"></a> Esempio: Configurazione di un gruppo di disponibilità che usa l'autenticazione di Windows  
 In questo esempio viene creata una procedura di configurazione [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] di esempio in cui viene utilizzato [!INCLUDE[tsql](../../../includes/tsql-md.md)] per configurare endpoint del mirroring del database in cui viene utilizzata l'autenticazione di Windows, nonché per creare e configurare un gruppo di disponibilità e i relativi database secondari.  
  
 In questo esempio sono incluse le sezioni seguenti:  
  
-   [Prerequisiti per l'utilizzo della procedura di configurazione di esempio](#PrerequisitesForExample)  
  
-   [Procedura di configurazione di esempio](#SampleProcedure)  
  
-   [Esempio di codice completo per la procedura di configurazione di esempio](#CompleteCodeExample)  
  
###  <a name="prerequisites-for-using-the-sample-configuration-procedure"></a><a name="PrerequisitesForExample"></a> Prerequisiti per l'utilizzo della procedura di configurazione di esempio  
 Questa procedura di esempio prevede i requisiti seguenti:  
  
-   Le istanze del server devono supportare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Per altre informazioni, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Devono essere presenti due database di esempio, *MyDb1* e *MyDb2*, nell'istanza del server che ospiterà la replica primaria. Gli esempi di codice seguenti consentono di creare e configurare questi due database, nonché di creare un backup completo di ognuno di essi. Eseguire questi esempi di codice nell'istanza del server in cui si desidera creare il gruppo di disponibilità di esempio. Questa istanza del server ospiterà la replica primaria iniziale del gruppo di disponibilità di esempio.  
  
    1.  L'esempio [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente consente di creare questi database e di modificarli in modo da utilizzare il modello di recupero con registrazione completa:  
  
        ```sql  
        -- Create sample databases:  
        CREATE DATABASE MyDb1;  
        GO  
        ALTER DATABASE MyDb1 SET RECOVERY FULL;  
        GO  
  
        CREATE DATABASE MyDb2;  
        GO  
        ALTER DATABASE MyDb2 SET RECOVERY FULL;  
        GO  
        ```  
  
    2.  Nell'esempio di codice seguente viene creato un backup completo del database di *MyDb1* e *MyDb2*. In questo esempio di codice si usa una condivisione di backup fittizia, \\\\*FILESERVER*\\*SQLbackups*.  
  
        ```sql  
        -- Backup sample databases:  
        BACKUP DATABASE MyDb1   
        TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
            WITH FORMAT;  
        GO  
  
        BACKUP DATABASE MyDb2   
        TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
            WITH FORMAT;  
        GO  
        ```  
  
 [&#91;TopOfExample&#93;](#ExampleConfigAGWinAuth)  
  
###  <a name="sample-configuration-procedure"></a><a name="SampleProcedure"></a> Procedura di configurazione di esempio  
 In questa configurazione di esempio sarà creata la replica di disponibilità in due istanze del server autonome i cui account del servizio vengono eseguiti in domini differenti, ma trusted,`DOMAIN1` e `DOMAIN2`.  
  
 Nella tabella seguente sono riepilogati i valori utilizzati in questa configurazione di esempio.  
  
|Ruolo iniziale|Sistema|Istanza host di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|------------------|------------|---------------------------------------------|  
|Primaria|`COMPUTER01`|`AgHostInstance`|  
|Secondari|`COMPUTER02`|Istanza predefinita|  
  
1.  Creare un endpoint del mirroring del database denominato *dbm_endpoint* nell'istanza del server in cui si intende creare il gruppo di disponibilità. Si tratta di un'istanza denominata `AgHostInstance` in `COMPUTER01`. In questo endpoint si usa la porta 7022. Si noti che la replica primaria sarà ospitata nell'istanza del server in cui si crea il gruppo di disponibilità.  
  
    ```sql  
    -- Create endpoint on server instance that hosts the primary replica:  
    CREATE ENDPOINT dbm_endpoint  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL);  
    GO  
    ```  
  
2.  Creare un endpoint *dbm_endpoint* nell'istanza del server in cui sarà ospitata la replica secondaria. Si tratta dell'istanza del server predefinita in `COMPUTER02`. In questo endpoint si utilizza la porta 5022.  
  
    ```sql  
    -- Create endpoint on server instance that hosts the secondary replica:   
    CREATE ENDPOINT dbm_endpoint  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=5022)   
        FOR DATABASE_MIRRORING (ROLE=ALL);  
    GO  
    ```  
  
3.  > [!NOTE]  
    >  Se gli account del servizio delle istanze del server in cui dovranno essere ospitate le repliche di disponibilità sono eseguiti con lo stesso account di dominio, questo passaggio non è necessario. Ignorarlo e passare direttamente al successivo.  
  
     Se gli account del servizio delle istanze del server vengono eseguiti con utenti di dominio diversi, in ogni istanza del server creare un account di accesso per l'altra istanza del server e concedere a questo account l'autorizzazione per l'accesso all'endpoint del mirroring del database locale.  
  
     Nell'esempio di codice seguente vengono illustrate le istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] per la creazione di un account di accesso e la concessione dell'autorizzazione in un endpoint. L'account di dominio dell'istanza del server remoto è rappresentato come *nome_dominio*\\*nome_utente*.  
  
    ```sql  
    -- If necessary, create a login for the service account, domain_name\user_name  
    -- of the server instance that will host the other replica:  
    USE master;  
    GO  
    CREATE LOGIN [domain_name\user_name] FROM WINDOWS;  
    GO  
    -- And Grant this login connect permissions on the endpoint:  
    GRANT CONNECT ON ENDPOINT::dbm_endpoint   
       TO [domain_name\user_name];  
    GO  
    ```  
  
4.  Nell'istanza del server in cui si trovano i database utente creare il gruppo di disponibilità.  
  
     Nell'esempio di codice seguente si crea un gruppo di disponibilità denominato *MyAG* nell'istanza del server in cui sono stati creati i database di esempio, *MyDb1* e *MyDb2*. Si specifica innanzitutto l'istanza del server locale, `AgHostInstance`, su *COMPUTER01* . Questa istanza ospiterà la replica primaria iniziale. Si specifica un'istanza del server remota, l'istanza del server predefinita in *COMPUTER02*, in cui viene ospitata una replica secondaria. Entrambe le repliche di disponibilità sono configurate per utilizzare la modalità con commit asincrono con failover manuale. Per le repliche con commit asincrono il failover manuale indica un failover forzato con possibile perdita di dati.  
  
    ```sql
    -- Create the availability group, MyAG:   
    CREATE AVAILABILITY GROUP MyAG   
       FOR   
          DATABASE MyDB1, MyDB2   
       REPLICA ON   
          'COMPUTER01\AgHostInstance' WITH   
             (  
             ENDPOINT_URL = 'TCP://COMPUTER01.Adventure-Works.com:7022',   
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             ),  
          'COMPUTER02' WITH   
             (  
             ENDPOINT_URL = 'TCP://COMPUTER02.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );   
    GO  
    ```  
  
     Per altri esempi di codice [!INCLUDE[tsql](../../../includes/tsql-md.md)] per la creazione di un gruppo di disponibilità, vedere [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md).  
  
5.  Nell'istanza del server in cui viene ospitata la replica secondaria creare un join della replica secondaria al gruppo di disponibilità.  
  
     Nell'esempio di codice seguente viene creato un join della replica secondaria in `COMPUTER02` al gruppo di disponibilità `MyAG` .  
  
    ```sql 
    -- On the server instance that hosts the secondary replica,   
    -- join the secondary replica to the availability group:  
    ALTER AVAILABILITY GROUP MyAG JOIN;  
    GO  
    ```  
  
6.  Nell'istanza del server che ospita la replica secondaria creare i database secondari.  
  
     L'esempio di codice seguente crea i database secondari *MyDb1* e *MyDb2* ripristinando i backup dei database tramite RESTORE WITH NORECOVERY.  
  
    ```sql 
    -- On the server instance that hosts the secondary replica,   
    -- Restore database backups using the WITH NORECOVERY option:  
    RESTORE DATABASE MyDb1   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH NORECOVERY;  
    GO  
  
    RESTORE DATABASE MyDb2   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITH NORECOVERY;  
    GO 
    ```  
  
7.  Nell'istanza del server in cui viene ospitata la replica primaria eseguire il backup del log delle transazioni in ognuno dei database primari.  
  
    > [!IMPORTANT]  
    >  Quando si configura un gruppo di disponibilità reale, prima di eseguire questo backup del log è consigliabile sospendere le attività di backup del log per i database primari fino a quando non è stato creato un join dei database secondari corrispondenti al gruppo di disponibilità.  
  
     Nell'esempio di codice seguente viene creato un backup del log delle transazioni in MyDb1 e MyDb2.  
  
    ```sql  
    -- On the server instance that hosts the primary replica,   
    -- Backup the transaction log on each primary database:  
    BACKUP LOG MyDb1   
    TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH NOFORMAT;  
    GO  
  
    BACKUP LOG MyDb2   
    TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITHNOFORMAT;  
    GO
    ```  
  
    > [!TIP]  
    >  In genere, è necessario eseguire un backup del log in ogni database primario, quindi ripristinare tale backup nel database secondario corrispondente utilizzando WITH NORECOVERY. Questo backup del log potrebbe tuttavia non essere necessario se il database è stato appena creato e non è ancora stato eseguito alcun backup del log oppure se il modello di recupero è stato appena modificato da SIMPLE a FULL.  
  
8.  Nell'istanza del server che ospita la replica secondaria applicare i backup del log ai database secondari.  
  
     L'esempio di codice seguente applica backup ai database secondari *MyDb1* e *MyDb2* ripristinando i backup dei database tramite RESTORE WITH NORECOVERY.  
  
    > [!IMPORTANT]  
    >  Quando si prepara un database secondario reale, è necessario applicare ogni backup del log eseguito dopo il backup del database da cui è stato creato il database secondario, a partire da quello meno recente e utilizzando sempre RESTORE WITH NORECOVERY. Naturalmente, se si ripristinano sia il backup completo del database che il backup differenziale, è necessario applicare solo i backup del log eseguiti dopo il backup differenziale.  
  
    ```sql  
    -- Restore the transaction log on each secondary database,  
    -- using the WITH NORECOVERY option:  
    RESTORE LOG MyDb1   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH FILE=1, NORECOVERY;  
    GO  
    RESTORE LOG MyDb2   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITH FILE=1, NORECOVERY;  
    GO  
    ```  
  
9. Nell'istanza del server che ospita la replica secondaria creare un join dei nuovi database secondari al gruppo di disponibilità.  
  
     L'esempio di codice seguente crea i join dei database secondari *MyDb1* e *MyDb2* al gruppo di disponibilità *MyAG* .  
  
    ```sql  
    -- On the server instance that hosts the secondary replica,   
    -- join each secondary database to the availability group:  
    ALTER DATABASE MyDb1 SET HADR AVAILABILITY GROUP = MyAG;  
    GO  
  
    ALTER DATABASE MyDb2 SET HADR AVAILABILITY GROUP = MyAG;  
    GO  
    ```  
  
###  <a name="complete-code-example-for-sample-configuration-procedure"></a><a name="CompleteCodeExample"></a> Esempio di codice completo per la procedura di configurazione di esempio  
 Nell'esempio seguente vengono uniti gli esempi di codice di tutti i passaggi della procedura di configurazione di esempio. Nella tabella seguente sono riepilogati i valori segnaposto utilizzati nell'esempio di codice. Per ulteriori informazioni sui passaggi di questo esempio di codice, vedere [Prerequisiti per l'utilizzo della procedura di configurazione di esempio](#PrerequisitesForExample) e [Procedura di configurazione di esempio](#SampleProcedure), precedentemente in questo argomento.  
  
|Segnaposto|Descrizione|  
|-----------------|-----------------|  
|\\\\*FILESERVER*\\*SQLbackups*|Condivisione di backup fittizia.|  
|\\\\*FILESERVER*\\*SQLbackups\MyDb1.bak*|File di backup per MyDb1.|  
|\\\\*FILESERVER*\\*SQLbackups\MyDb2.bak*|File di backup per MyDb2.|  
|*7022*|Numero di porta assegnato a ogni endpoint del mirroring del database.|  
|*COMPUTER01\AgHostInstance*|Istanza del server che ospita la replica primaria iniziale.|  
|*COMPUTER02*|Istanza del server in cui viene ospitata la replica secondaria iniziale. Si tratta dell'istanza del server predefinita in `COMPUTER02`.|  
|*dbm_endpoint*|Nome specificato per ogni endpoint del mirroring del database.|  
|*MyAG*|Nome del gruppo di disponibilità di esempio.|  
|*MyDb1*|Nome del primo database di esempio.|  
|*MyDb2*|Nome del secondo database di esempio.|  
|*DOMAIN1\user1*|Account del servizio dell'istanza del server che dovrà ospitare la replica primaria iniziale.|  
|*DOMAIN2\user2*|Account del servizio dell'istanza del server in cui dovrà essere ospitata la replica secondaria iniziale.|  
|TCP://*COMPUTER01.Adventure-Works.com*:*7022*|URL dell'endpoint dell'istanza AgHostInstance di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in COMPUTER01.|  
|TCP://*COMPUTER02.Adventure-Works.com*:*5022*|URL dell'endpoint dell'istanza predefinita di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in COMPUTER02.|  
  
> [!NOTE]  
>  Per altri esempi di codice [!INCLUDE[tsql](../../../includes/tsql-md.md)] per la creazione di un gruppo di disponibilità, vedere [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md).  
  
```sql  
-- on the server instance that will host the primary replica,   
-- create sample databases:  
CREATE DATABASE MyDb1;  
GO  
ALTER DATABASE MyDb1 SET RECOVERY FULL;  
GO  
  
CREATE DATABASE MyDb2;  
GO  
ALTER DATABASE MyDb2 SET RECOVERY FULL;  
GO  
  
-- Backup sample databases:  
BACKUP DATABASE MyDb1   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH FORMAT;  
GO  
  
BACKUP DATABASE MyDb2   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH FORMAT;  
GO  
  
-- Create the endpoint on the server instance that will host the primary replica:  
CREATE ENDPOINT dbm_endpoint  
    STATE=STARTED   
    AS TCP (LISTENER_PORT=7022)   
    FOR DATABASE_MIRRORING (ROLE=ALL);  
GO  
  
-- Create the endpoint on the server instance that will host the secondary replica:   
CREATE ENDPOINT dbm_endpoint  
    STATE=STARTED   
    AS TCP (LISTENER_PORT=7022)   
    FOR DATABASE_MIRRORING (ROLE=ALL);  
GO  
  
-- If both service accounts run under the same domain account, skip this step. Otherwise,   
-- On the server instance that will host the primary replica,   
-- create a login for the service account   
-- of the server instance that will host the secondary replica, DOMAIN2\user2,   
-- and grant this login connect permissions on the endpoint:  
USE master;  
GO  
CREATE LOGIN [DOMAIN2\user2] FROM WINDOWS;  
GO  
GRANT CONNECT ON ENDPOINT::dbm_endpoint   
   TO [DOMAIN2\user2];  
GO  
  
-- If both service accounts run under the same domain account, skip this step. Otherwise,   
-- On the server instance that will host the secondary replica,  
-- create a login for the service account   
-- of the server instance that will host the primary replica, DOMAIN1\user1,   
-- and grant this login connect permissions on the endpoint:  
USE master;  
GO  
  
CREATE LOGIN [DOMAIN1\user1] FROM WINDOWS;  
GO  
GRANT CONNECT ON ENDPOINT::dbm_endpoint   
   TO [DOMAIN1\user1];  
GO  
  
-- On the server instance that will host the primary replica,   
-- create the availability group, MyAG:  
CREATE AVAILABILITY GROUP MyAG   
   FOR   
      DATABASE MyDB1, MyDB2   
   REPLICA ON   
      'COMPUTER01\AgHostInstance' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.Adventure-Works.com:7022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),  
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.Adventure-Works.com:7022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         );   
GO  
  
-- On the server instance that hosts the secondary replica,   
-- join the secondary replica to the availability group:  
ALTER AVAILABILITY GROUP MyAG JOIN;  
GO  
  
-- Restore database backups onto this server instance, using RESTORE WITH NORECOVERY:  
RESTORE DATABASE MyDb1   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH NORECOVERY;  
GO  
  
RESTORE DATABASE MyDb2   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH NORECOVERY;  
GO  
  
-- Back up the transaction log on each primary database:  
BACKUP LOG MyDb1   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH NOFORMAT;  
GO  
  
BACKUP LOG MyDb2   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITHNOFORMAT  
GO  
  
-- Restore the transaction log on each secondary database,  
-- using the WITH NORECOVERY option:  
RESTORE LOG MyDb1   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH FILE=1, NORECOVERY;  
GO  
RESTORE LOG MyDb2   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH FILE=1, NORECOVERY;  
GO  
  
-- On the server instance that hosts the secondary replica,   
-- join each secondary database to the availability group:  
ALTER DATABASE MyDb1 SET HADR AVAILABILITY GROUP = MyAG;  
GO  
  
ALTER DATABASE MyDb2 SET HADR AVAILABILITY GROUP = MyAG;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare le proprietà della replica e del gruppo di disponibilità**  
  
-   [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurare i criteri di failover flessibili per controllare le condizioni per il failover automatico &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Modificare il periodo di timeout della sessione per una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Per completare la configurazione del gruppo di disponibilità**  
  
-   [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Modalità alternative di creazione di un gruppo di disponibilità**  
  
-   [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Creare un gruppo di disponibilità &#40;PowerShell SQL Server&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **Per abilitare i gruppi di disponibilità AlwaysOn**  
  
-   [Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Per configurare un endpoint del mirroring del database**  
  
-   [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Per risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn**  
  
-   [Risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   **Blog:**  
  
     [Serie di informazioni su Always On - HADRON: Uso del pool di lavoro per database abilitati HADRON](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server Always On Team Blogs (Blog di SQL Server Always On): blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
     [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
-   **Video:**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1: Introducing the Next Generation High Availability Solution](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302) (Serie Always On in Microsoft SQL Server nome in codice "Denali", parte 1: Introduzione alla soluzione a disponibilità elevata di nuova generazione)  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2: Building a Mission-Critical High Availability Solution Using Always On](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404) (Serie Always On in Microsoft SQL Server nome in codice "Denali", parte 2: Creazione di una soluzione cruciale a disponibilità elevata con Always On)  
  
-   **White paper:**  
  
     [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Vedere anche  
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
