---
description: Replication System Stored Procedures Concepts
title: Concetti di base relativi alle stored procedure del sistema di replica | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server replication], programming
- programming [SQL Server replication], system stored procedures
- programming interfaces [SQL Server replication]
- system stored procedures [SQL Server replication]
- replication [SQL Server], how-to topics
ms.assetid: 816d2bda-ed72-43ec-aa4d-7ee3dc25fd8a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eb082b29411b136edca7ed14f06effa354a5f0e6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643100"
---
# <a name="replication-system-stored-procedures-concepts"></a>Replication System Stored Procedures Concepts
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'accesso a livello di codice a tutte le funzionalità configurabili dall'utente in una topologia di replica viene fornito da stored procedure di sistema. Mentre le stored procedure possono essere eseguite singolarmente utilizzando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o l'utilità della riga di comando sqlcmd, può rivelarsi vantaggioso scrivere file script [!INCLUDE[tsql](../../../includes/tsql-md.md)] che consentono di eseguire una sequenza logica di attività di replica.  
  
 L'esecuzione di attività di replica tramite script offre i vantaggi seguenti:  
  
-   Consente di conservare una copia permanente dei passaggi utilizzati per distribuire la topologia di replica.  
  
-   Consente di utilizzare un unico script per configurare più sottoscrittori.  
  
-   Consente di formare in breve tempo nuovi amministratori di database permettendo loro di valutare, comprendere e modificare il codice, nonché di risolvere i problemi a livello del codice.  
  
    > [!IMPORTANT]  
    >  È possibile che gli script siano all'origine di vulnerabilità nella sicurezza, in quanto possono richiamare funzioni di sistema all'insaputa dell'utente e senza richiederne l'intervento, nonché contenere credenziali di sicurezza in testo normale. Prima di utilizzarli, verificare gli script in modo da individuare possibili problemi di sicurezza.  
  
## <a name="creating-replication-scripts"></a>Creazione di script di replica  
 Dal punto di vista della replica, uno script è una serie di una o più istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] in cui ogni istruzione esegue una stored procedure di replica. Gli script sono file di testo, spesso con l'estensione di file sql, che possono essere eseguiti utilizzando l'utilità sqlcmd. Quando un file script viene eseguito, l'utilità esegue le istruzioni SQL archiviate nel file. Allo stesso modo, uno script può essere archiviato come oggetto query in un progetto di [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 Per creare script di replica è possibile procedere nei modi seguenti:  
  
-   Creare lo script in modo manuale.  
  
-   Utilizzare le caratteristiche di generazione degli script fornite nelle procedure guidate per la replica oppure  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   Utilizzare RMO (Replication Management Objects) per generare lo script a livello di codice per la creazione di un oggetto RMO.  
  
 Quando si creano manualmente script di replica, tenere presente le considerazioni seguenti:  
  
-   Gli script [!INCLUDE[tsql](../../../includes/tsql-md.md)] possono contenere uno o più batch. Il comando GO indica la fine di un batch. Uno script [!INCLUDE[tsql](../../../includes/tsql-md.md)] che non include alcun comando GO viene eseguito come batch singolo.  
  
-   Quando si eseguono più stored procedure di replica in un solo batch, tutte le procedure successive alla prima procedura nel batch devono essere precedute dalla parola chiave EXECUTE.  
  
-   Tutte le stored procedure in un batch devono essere compilate prima che un batch venga eseguito. Tuttavia, dopo aver compilato il batch e creato un piano di esecuzione, potrebbe o meno verificarsi un errore di run-time.  
  
-   Quando si creano script per la configurazione della replica, è necessario utilizzare l'autenticazione di Windows per evitare di archiviare le credenziali di sicurezza nel file script. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
## <a name="sample-replication-script"></a>Script di replica di esempio  
 Lo script seguente può essere eseguito per configurare la pubblicazione e la distribuzione in un server.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
 È quindi possibile salvare questo script localmente come `instdistpub.sql` in modo da poterlo eseguire più volte in base alle esigenze.  
  
 Lo script precedente include variabili di scripting **sqlcmd** usate in molti degli esempi di codice di replica presenti nella documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le variabili di scripting vengono definite mediante la sintassi `$(MyVariable)`. I valori per le variabili possono essere passati a uno script dalla riga di comando o in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Per ulteriori informazioni, vedere la sezione successiva in questo argomento, "Esecuzione di script di replica".  
  
## <a name="executing-replication-scripts"></a>Esecuzione di script di replica  
 Dopo averlo creato, uno script di replica può essere eseguito in uno dei modi seguenti:  
  
### <a name="creating-a-sql-query-file-in-sql-server-management-studio"></a>Creazione di un file query SQL in SQL Server Management Studio  
 Un file script di replica di [!INCLUDE[tsql](../../../includes/tsql-md.md)] può essere creato come file query SQL in un progetto di [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Dopo aver scritto lo script, è possibile stabilire una connessione al database per questo file query ed eseguire lo script. Per altre informazioni su come creare script di [!INCLUDE[tsql](../../../includes/tsql-md.md)] con [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], vedere [Editor di query e di testo &#40;SQL Server Management Studio&#41;](../../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
 Per usare uno script che include variabili di scripting, è necessario che [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] sia in esecuzione in modalità **sqlcmd**. In modalità **sqlcmd**, l'editor di query accetta la sintassi aggiuntiva specifica di **sqlcmd**, ad esempio il comando `:setvar` usato per assegnare un valore a una variabile. Per altre informazioni sulla modalità **sqlcmd**, vedere [Modificare script SQLCMD con l'editor di query](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md). Nello script seguente il comando `:setvar` viene usato per fornire un valore per la variabile `$(DistPubServer)`.  
  
```  
:setvar DistPubServer N'MyPublisherAndDistributor';  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
--  
-- Additional code goes here  
--  
```  
  
### <a name="using-the-sqlcmd-utility-from-the-command-line"></a>Utilizzo dell'utilità sqlcmd dalla riga di comando  
 L'esempio seguente mostra come usare la riga di comando per eseguire il file di script `instdistpub.sql` con l'[utilità sqlcmd](../../../tools/sqlcmd-utility.md):  
  
```  
sqlcmd.exe -E -S sqlserverinstance -i C:\instdistpub.sql -o C:\output.log -v DistPubServer="N'MyDistributorAndPublisher'"  
```  
  
 In questo esempio, l'opzione `-E` indica che viene utilizzata l'autenticazione di Windows per la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Quando si utilizza l'autenticazione di Windows, non è necessario archiviare un nome utente e una password nel file script. Il nome e il percorso del file script vengono specificati dall'opzione `-i` e il nome del file di output viene specificato dall'opzione `-o`. Quando viene utilizzata questa opzione, l'output da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene scritto in questo file anziché indirizzato alla console. L'utilità `sqlcmd` consente di passare variabili di scripting a uno script di [!INCLUDE[tsql](../../../includes/tsql-md.md)] durante l'esecuzione utilizzando l'opzione `-v`. In questo esempio, `sqlcmd` sostituisce ogni istanza di `$(DistPubServer)` nello script con il valore `N'MyDistributorAndPublisher'` prima dell'esecuzione.  
  
> [!NOTE]  
>  L'opzione `-X` disabilita le variabili di scripting.  
  
### <a name="automating-tasks-in-a-batch-file"></a>Automatizzazione di attività in un file batch  
 L'utilizzo di un file batch consente di automatizzare le attività di amministrazione della replica, le attività di sincronizzazione della replica e altre attività nello stesso file batch. Il file batch seguente usa l'utilità **sqlcmd** per eliminare e ricreare il database di sottoscrizione e aggiungere una sottoscrizione pull di tipo merge. Il file richiama quindi l'agente di merge per sincronizzare la nuova sottoscrizione:  
  
```  
REM ----------------------Script to synchronize merge subscription ----------------------  
REM -- Creates subscription database and   
REM -- synchronizes the subscription to MergeSalesPerson.  
REM -- Current computer acts as both Publisher and Subscriber.  
REM -------------------------------------------------------------------------------------  
  
SET Publisher=%computername%  
SET Subscriber=%computername%  
SET PubDb=AdventureWorks  
SET SubDb=AdventureWorksReplica  
SET PubName=AdvWorksSalesOrdersMerge  
  
REM -- Drop and recreate the subscription database at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE master IF EXISTS (SELECT * FROM sysdatabases WHERE name='%SubDb%' ) DROP DATABASE %SubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE master CREATE DATABASE %SubDb%"  
  
REM -- Add a pull subscription at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb% EXEC sp_addmergepullsubscription @publisher = %Publisher%, @publication = %PubName%, @publisher_db = %PubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb%  EXEC sp_addmergepullsubscription_agent @publisher = %Publisher%, @publisher_db = %PubDb%, @publication = %PubName%, @subscriber = %Subscriber%, @subscriber_db = %SubDb%, @distributor = %Publisher%"  
  
REM -- This batch file starts the merge agent at the Subscriber to   
REM -- synchronize a pull subscription to a merge publication.  
REM -- The following must be supplied on one line.  
"\Program Files\Microsoft SQL Server\130\COM\REPLMERG.EXE"  -Publisher  %Publisher% -Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB  %PubDb% -SubscriberDB %SubDb% -Publication %PubName% -PublisherSecurityMode 1 -OutputVerboseLevel 1  -Output  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1 -Validate 3  
  
```  
  
## <a name="scripting-common-replication-tasks"></a>Generazione di script per attività di replica comuni  
 Di seguito vengono illustrate alcune delle attività di replica più comuni per le quali è possibile generare script utilizzando stored procedure di sistema:  
  
-   Configurazione della pubblicazione e della distribuzione.  
  
-   Modifica delle proprietà del server di pubblicazione e del server di distribuzione.  
  
-   Disabilitazione della pubblicazione e della distribuzione.  
  
-   Creazione di pubblicazioni e definizione di articoli.  
  
-   Eliminazione di pubblicazioni e articoli.  
  
-   Creazione di una sottoscrizione pull.  
  
-   Modifica di una sottoscrizione pull.  
  
-   Eliminazione di una sottoscrizione pull.  
  
-   Creazione di una sottoscrizione push.  
  
-   Modifica di una sottoscrizione push.  
  
-   Eliminazione di una sottoscrizione push.  
  
-   Sincronizzazione di una sottoscrizione pull.  
  
## <a name="see-also"></a>Vedere anche  
 [Concetti di base relativi alla programmazione della replica](../../../relational-databases/replication/concepts/replication-programming-concepts.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Creazione di script di replica](../../../relational-databases/replication/scripting-replication.md)  
  
