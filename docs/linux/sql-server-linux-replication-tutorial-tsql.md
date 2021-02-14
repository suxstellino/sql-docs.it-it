---
title: 'Esercitazione: Configurare la replica (T-SQL)'
description: Configurare la replica snapshot di SQL Server in Linux con due istanze di SQL Server tramite Transact-SQL (T-SQL).
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 825fec3eeb0abbfacd4bb5f1311e13b6f99189e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346465"
---
# <a name="configure-replication-with-t-sql"></a>Configurare la replica con T-SQL

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)] 

In questa esercitazione viene configurata la replica snapshot di SQL Server in Linux con due istanze di SQL Server tramite Transact-SQL. Il server di pubblicazione e il server di distribuzione si troveranno nella stessa istanza e il sottoscrittore si troverà in un'istanza separata.

> [!div class="checklist"]
> * Abilitare agenti di replica di SQL Server in Linux
> * Creare un database di esempio
> * Configurare la cartella snapshot per l'accesso degli agenti di SQL Server
> * Configurare il server di distribuzione
> * Configurare il server di pubblicazione
> * Configurare la pubblicazione e gli articoli
> * Configurare il sottoscrittore 
> * Eseguire i processi di replica

Tutte le configurazioni di replica possono essere configurate con [stored procedure di replica](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

## <a name="prerequisites"></a>Prerequisiti
Per completare questa esercitazione è necessario quanto segue:

- Due istanze di SQL Server con la versione più recente di SQL Server in Linux
- Uno strumento per eseguire query T-SQL per la configurazione della replica, ad esempio SQLCMD o SSMS

   Vedere [Usare SSMS per gestire SQL Server in Linux](./sql-server-linux-manage-ssms.md).

   >[!NOTE]
   >[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) e versioni successive supportano la replica di SQL Server per le istanze di SQL Server in Linux.

## <a name="detailed-steps"></a>Procedura dettagliata

1. Abilitare gli agenti di replica di SQL Server in Linux. Nel terminale di entrambi i computer host eseguire i comandi seguenti. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   sudo systemctl restart mssql-server
   ```

1. Creare il database e la tabella di esempio. Nel server di pubblicazione creare una tabella e un database di esempio che fungeranno da articoli per una pubblicazione.

   ```sql
   CREATE DATABASE Sales
   GO
   USE [SALES]
   GO 
   CREATE TABLE CUSTOMER([CustomerID] [int] NOT NULL, [SalesAmount] [decimal] NOT NULL)
   GO 
   INSERT INTO CUSTOMER (CustomerID, SalesAmount) VALUES (1,100),(2,200),(3,300)
   ```

   Nell'altra istanza di SQL Server, il sottoscrittore, creare il database per la ricezione degli articoli.

   ```sql
   CREATE DATABASE Sales
   GO
   ```

1. Creare la cartella snapshot per la lettura/scrittura da parte degli agenti di SQL Server nel server di distribuzione, creare la cartella snapshot e concedere l'accesso all'utente "mssql" 

   ```bash
   sudo mkdir /var/opt/mssql/data/ReplData/
   sudo chown mssql /var/opt/mssql/data/ReplData/
   sudo chgrp mssql /var/opt/mssql/data/ReplData/
   ```

1. Configurare il server di distribuzione. In questo esempio il server di pubblicazione fungerà anche da server di distribuzione. Eseguire i comandi seguenti nel server di pubblicazione per configurare l'istanza anche per la distribuzione.

   ```sql
   DECLARE @distributor AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @distributor = N'<distributor instance name>'--in this example, it will be the name of the publisher
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 
   
   use master
   exec sp_adddistributor @distributor = @distributor -- this should be the hostname

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   exec sp_adddistributiondb @database = N'distribution', @log_file_size = 2, @deletebatchsize_xact = 5000, @deletebatchsize_cmd = 2000, @security_mode = 0, @login = @distributorlogin, @password = @distributorpassword
   GO

   DECLARE @snapshotdirectory AS nvarchar(500)
   SET @snapshotdirectory = N'/var/opt/mssql/data/ReplData/'

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   use [distribution] 
   if (not exists (select * from sysobjects where name = 'UIProperties' and type = 'U ')) 
          create table UIProperties(id int) 
   if (exists (select * from ::fn_listextendedproperty('SnapshotFolder', 'user', 'dbo', 'table', 'UIProperties', null, null))) 
          EXEC sp_updateextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties' 
   else 
         EXEC sp_addextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties'
   GO
   ```

1. Configurare il server di pubblicazione. Eseguire i comandi T-SQL seguenti nel server di pubblicazione.

   ```sql
   DECLARE @publisher AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @publisher = N'<instance name>' 
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 

   -- Adding the distribution publishers
   exec sp_adddistpublisher @publisher = @publisher, 
   @distribution_db = N'distribution', 
   @security_mode = 0, 
   @login = @distributorlogin, 
   @password = @distributorpassword, 
   @working_directory = N'/var/opt/mssql/data/ReplData', 
   @trusted = N'false', 
   @thirdparty_flag = 0, 
   @publisher_type = N'MSSQLSERVER'
   GO
   ```

1. Configurare il processo di pubblicazione. Eseguire i comandi T-SQL seguenti nel server di pubblicazione.

   ```sql
   DECLARE @replicationdb AS sysname
   DECLARE @publisherlogin AS sysname
   DECLARE @publisherpassword AS sysname
   SET @replicationdb = N'Sales'
   SET @publisherlogin = N'<Publisher login>'
   SET @publisherpassword = N'<Publisher Password>'

   use [Sales]
   exec sp_replicationdboption @dbname = N'Sales', @optname = N'publish', @value = N'true'
   
   -- Add the snapshot publication
   exec sp_addpublication 
   @publication = N'SnapshotRepl', 
   @description = N'Snapshot publication of database ''Sales'' from Publisher ''<PUBLISHER HOSTNAME>''.',
   @retention = 0, 
   @allow_push = N'true', 
   @repl_freq = N'snapshot', 
   @status = N'active', 
   @independent_agent = N'true'

   exec sp_addpublication_snapshot @publication = N'SnapshotRepl', 
   @frequency_type = 1, 
   @frequency_interval = 1, 
   @frequency_relative_interval = 1, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 8, 
   @frequency_subday_interval = 1, 
   @active_start_time_of_day = 0,
   @active_end_time_of_day = 235959, 
   @active_start_date = 0, 
   @active_end_date = 0, 
   @publisher_security_mode = 0, 
   @publisher_login = @publisherlogin, 
   @publisher_password = @publisherpassword
   ```

1. Creare gli articoli dalla tabella Sales. Eseguire i comandi T-SQL seguenti nel server di pubblicazione.

   ```sql
   use [Sales]
   exec sp_addarticle 
   @publication = N'SnapshotRepl', 
   @article = N'customer', 
   @source_owner = N'dbo', 
   @source_object = N'customer', 
   @type = N'logbased', 
   @description = null, 
   @creation_script = null, 
   @pre_creation_cmd = N'drop', 
   @schema_option = 0x000000000803509D,
   @identityrangemanagementoption = N'manual', 
   @destination_table = N'customer', 
   @destination_owner = N'dbo', 
   @vertical_partition = N'false'
   ```

1. Configurare la sottoscrizione. Eseguire i comandi T-SQL seguenti nel server di pubblicazione.

   ```sql
   DECLARE @subscriber AS sysname
   DECLARE @subscriber_db AS sysname
   DECLARE @subscriberLogin AS sysname
   DECLARE @subscriberPassword AS sysname
   SET @subscriber = N'<Instance Name>' -- for example, MSSQLSERVER
   SET @subscriber_db = N'Sales'
   SET @subscriberLogin = N'<Subscriber Login>'
   SET @subscriberPassword = N'<Subscriber Password>'

   use [Sales]
   exec sp_addsubscription 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @destination_db = @subscriber_db, 
   @subscription_type = N'Push', 
   @sync_type = N'automatic', 
   @article = N'all', 
   @update_mode = N'read only', 
   @subscriber_type = 0

   exec sp_addpushsubscription_agent 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @subscriber_db = @subscriber_db, 
   @subscriber_security_mode = 0, 
   @subscriber_login = @subscriberLogin,
   @subscriber_password = @subscriberPassword,
   @frequency_type = 1,
   @frequency_interval = 0, 
   @frequency_relative_interval = 0, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 0, 
   @frequency_subday_interval = 0, 
   @active_start_time_of_day = 0, 
   @active_end_time_of_day = 0, 
   @active_start_date = 0, 
   @active_end_date = 19950101
   GO
   ```

1. Eseguire i processi dell'agente di replica. Eseguire la query seguente per ottenere un elenco di processi:

   ```sql
   SELECT name, date_modified FROM msdb.dbo.sysjobs order by date_modified desc
   ```

   Eseguire il processo di replica snapshot per generare lo snapshot:

   ```sql
   USE msdb;   
   --generate snapshot of publications, for example
   EXEC dbo.sp_start_job N'PUBLISHER-PUBLICATION-SnapshotRepl-1'
   GO
   ```

   Eseguire il processo di replica snapshot per generare lo snapshot:

   ```sql
   USE msdb;
   --distribute the publication to subscriber, for example
   EXEC dbo.sp_start_job N'DISTRIBUTOR-PUBLICATION-SnapshotRepl-SUBSCRIBER'
   GO
   ```

1. Connettere il sottoscrittore ed eseguire query sui dati replicati.    

   Nel sottoscrittore verificare che la replica funzioni eseguendo la query seguente:

   ```sql
   SELECT * from [Sales].[dbo].[CUSTOMER]
   ```

In questa esercitazione è stata configurata la replica snapshot di SQL Server in Linux con due istanze di SQL Server tramite Transact-SQL.

> [!div class="checklist"]
> * Abilitare agenti di replica di SQL Server in Linux
> * Creare un database di esempio
> * Configurare la cartella snapshot per l'accesso degli agenti di SQL Server
> * Configurare il server di distribuzione
> * Configurare il server di pubblicazione
> * Configurare la pubblicazione e gli articoli
> * Configurare il sottoscrittore 
> * Eseguire i processi di replica

## <a name="see-also"></a>Vedere anche

Per informazioni dettagliate sulla replica, vedere la [documentazione della replica di SQL Server](../relational-databases/replication/sql-server-replication.md).

## <a name="next-steps"></a>Passaggi successivi

[Concetti: replica di SQL Server in Linux](sql-server-linux-replication.md)

[Stored procedure per la replica](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).
