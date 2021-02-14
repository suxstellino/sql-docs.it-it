---
title: Creare ed eseguire processi per SQL Server in Linux
description: Informazioni su come creare un processo di SQL Server Agent in Linux usando Transact-SQL e SQL Server Management Studio (SSMS).
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1d93d95e-9c89-4274-9b3f-fa2608ec2792
ms.openlocfilehash: bc52f0a2052d0b9abd1700e92af3a30c0e02bad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346446"
---
# <a name="create-and-run-sql-server-agent-jobs-on-linux"></a>Creare ed eseguire processi di SQL Server Agent in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

I processi di SQL Server vengono usati per eseguire regolarmente la stessa sequenza di comandi nel database SQL Server. Questa esercitazione fornisce un esempio di come creare un processo di SQL Server Agent in Linux usando sia Transact-SQL che SQL Server Management Studio (SSMS).

> [!div class="checklist"]
> * Installare SQL Server Agent in Linux
> * Creare un nuovo processo per eseguire backup giornalieri del database
> * Pianificare ed eseguire il processo
> * Eseguire gli stessi passaggi in SSMS (facoltativo)

Per problemi noti con SQL Server Agent in Linux, vedere le [note sulla versione](sql-server-linux-release-notes.md).

## <a name="prerequisites"></a>Prerequisites

Per completare l'esercitazione, è necessario soddisfare i prerequisiti seguenti:

* Di seguito vengono indicati i prerequisiti del computer Linux:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) o [Ubuntu](quickstart-install-connect-ubuntu.md)) con gli strumenti da riga di comando.

I prerequisiti seguenti sono facoltativi:

* Computer Windows con SSMS:
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) per i passaggi facoltativi in SSMS.

## <a name="enable-sql-server-agent"></a>Abilitare SQL Server Agent

Per usare SQL Server Agent in Linux, è necessario abilitare prima SQL Server Agent in un computer in cui è già installato SQL Server.

1. Per abilitare SQL Server Agent, seguire questa procedura.
  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
  ```

1. Riavviare SQL Server con il comando seguente:
  ```bash
  sudo systemctl restart mssql-server
  ```

> [!NOTE]
> A partire da SQL Server 2017 CU4, SQL Server Agent è incluso nel pacchetto **mssql-server** ed è disabilitato per impostazione predefinita. Per la configurazione di Agent precedente alla versione CU4, vedere [Installare SQL Server Agent in Linux](sql-server-linux-setup-sql-agent.md).

## <a name="create-a-sample-database"></a>Creare un database di esempio

Seguire questa procedura per creare un database di esempio denominato **SampleDB**. Questo database viene usato per il processo di backup giornaliero.

1. Nel computer Linux aprire una sessione del terminale Bash.

1. Usare **sqlcmd** per eseguire un comando **CREATE DATABASE** di Transact-SQL.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'CREATE DATABASE SampleDB'
   ```

1. Verificare che il database sia stato creato elencando i database nel server.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'SELECT Name FROM sys.Databases'
   ```

## <a name="create-a-job-with-transact-sql"></a>Creare un processo con Transact-SQL

La procedura seguente crea un processo di SQL Server Agent in Linux con i comandi di Transact-SQL. Il processo esegue un backup giornaliero del database di esempio **SampleDB**.

> [!TIP]
> È possibile usare qualsiasi client T-SQL per eseguire questi comandi. In Linux, ad esempio, è possibile usare [sqlcmd](sql-server-linux-setup-tools.md) o [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md). Da un server Windows remoto è anche possibile eseguire query in SQL Server Management Studio (SSMS) o usare l'interfaccia utente per la gestione dei processi, descritta nella sezione successiva.

1. Usare [sp_add_job](../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md) per creare un processo denominato `Daily SampleDB Backup`.

   ```sql
   -- Adds a new job executed by the SQLServerAgent service
   -- called 'Daily SampleDB Backup'
   USE msdb ;
   GO
   EXEC dbo.sp_add_job
      @job_name = N'Daily SampleDB Backup' ;
   GO
   ```

1. Chiamare [sp_add_jobstep](../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) per creare un passaggio del processo, che crea un backup del database `SampleDB`.

   ```sql
   -- Adds a step (operation) to the job
   EXEC sp_add_jobstep
      @job_name = N'Daily SampleDB Backup',
      @step_name = N'Backup database',
      @subsystem = N'TSQL',
      @command = N'BACKUP DATABASE SampleDB TO DISK = \
         N''/var/opt/mssql/data/SampleDB.bak'' WITH NOFORMAT, NOINIT, \
         NAME = ''SampleDB-full'', SKIP, NOREWIND, NOUNLOAD, STATS = 10',
      @retry_attempts = 5,
      @retry_interval = 5 ;
   GO
   ```

1. Creare quindi una pianificazione giornaliera per il processo con [sp_add_schedule](../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md).

   ```sql
   -- Creates a schedule called 'Daily'
   EXEC dbo.sp_add_schedule
      @schedule_name = N'Daily SampleDB',
      @freq_type = 4,
      @freq_interval = 1,
      @active_start_time = 233000 ;
   USE msdb ;
   GO
   ```

1. Associare la pianificazione al processo con [sp_attach_schedule](../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md).

   ```sql
   -- Sets the 'Daily' schedule to the 'Daily SampleDB Backup' Job
   EXEC sp_attach_schedule
      @job_name = N'Daily SampleDB Backup',
      @schedule_name = N'Daily SampleDB';
   GO
   ```

1. Usare [sp_add_jobserver](../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) per assegnare il processo a un server di destinazione. In questo esempio la destinazione è il server locale.

   ```sql
   EXEC dbo.sp_add_jobserver
      @job_name = N'Daily SampleDB Backup',
      @server_name = N'(LOCAL)';
   GO
   ```
1. Avviare il processo con [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).

   ```sql
   EXEC dbo.sp_start_job N' Daily SampleDB Backup' ;
   GO
   ```

## <a name="create-a-job-with-ssms"></a>Creare un processo con SSMS

È anche possibile creare e gestire i processi in modalità remota usando SQL Server Management Studio (SSMS) in Windows.

1. Avviare SSMS in Windows e connettersi all'istanza di SQL Server in Linux. Per altre informazioni, vedere [Gestire SQL Server in Linux con SSMS](sql-server-linux-manage-ssms.md).

1. Verificare di aver creato un database di esempio denominato **SampleDB**.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-0.png" alt="Create a SampleDB database" style="width: 550px;"/>

1. Verificare che SQL Agent sia stato [installato](sql-server-linux-setup-sql-agent.md) e configurato correttamente. Cercare il segno più accanto a SQL Server Agent in Esplora oggetti. Se SQL Server Agent non è abilitato, provare a riavviare il servizio **mssql-server** in Linux.

   ![Verificare che SQL Server Agent sia stato installato](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-1.png)

1. Creare un nuovo processo.

   ![Creare un nuovo processo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-2.png)

1. Assegnare un nome al processo e creare il passaggio del processo.

   ![Creare un passaggio del processo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-3.png)

1. Specificare il sottosistema che si vuole usare e l'azione che deve essere eseguita dal passaggio del processo.

   ![Sottosistema del processo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-4.png)

   ![Azione del passaggio del processo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-5.png)

1. Creare una nuova pianificazione del processo.

   ![Screenshot della finestra di dialogo Nuovo processo con l'opzione Pianificazioni e l'opzione Nuovo evidenziate.](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-6.png)

   ![Screenshot della finestra di dialogo Nuovo processo con l'opzione OK evidenziata.](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-8.png)

1. Avviare il processo.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-9.png" alt="Start the SQL Server Agent job" style="width: 550px;"/>

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:

> [!div class="checklist"]
> * Installare SQL Server Agent in Linux
> * Usare Transact-SQL e le stored procedure di sistema per creare processi
> * Creare un processo che esegue i backup giornalieri del database
> * Usare l'interfaccia utente di SSMS per creare e gestire i processi

Esplorare ora altre funzionalità per la creazione e la gestione dei processi:

> [!div class="nextstepaction"]
>[Documentazione di SQL Server Agent](../ssms/agent/sql-server-agent.md)