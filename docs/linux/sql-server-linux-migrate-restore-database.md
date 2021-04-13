---
title: Eseguire la migrazione di un database di SQL Server da Windows a Linux
description: Questa esercitazione illustra come eseguire il backup di un database di SQL Server in Windows e ripristinarlo in un computer Linux che esegue SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/08/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9ac64d1a-9fe5-446e-93c3-d17b8f55a28f
ms.openlocfilehash: 23a575bfabdd16f30007a8b70d84a568f45b8d3e
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210820"
---
# <a name="migrate-a-sql-server-database-from-windows-to-linux-using-backup-and-restore"></a>Eseguire la migrazione di un database di SQL Server da Windows a Linux usando il backup e ripristino

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

La funzionalità di backup e ripristino di SQL Server è il metodo consigliato per eseguire la migrazione di un database da SQL Server in Windows a SQL Server in Linux. In questa esercitazione verranno illustrati i passaggi necessari per spostare un database in Linux con le tecniche di backup e ripristino.

> [!div class="checklist"]
> * Creare un file di backup in Windows con SSMS
> * Installare una shell Bash in Windows
> * Spostare il file di backup in Linux dalla shell Bash
> * Ripristinare il file di backup in Linux con Transact-SQL
> * Eseguire una query per verificare la migrazione

È anche possibile creare un gruppo di disponibilità SQL Server Always On di SQL Server per eseguire la migrazione di un database di SQL Server da Windows a Linux. Vedere [sql-server-linux-availability-group-cross-platform](sql-server-linux-availability-group-cross-platform.md).

## <a name="prerequisites"></a>Prerequisites

Per completare l'esercitazione, è necessario soddisfare i prerequisiti seguenti:

* Computer Windows con gli elementi seguenti:
  * [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) installato.
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) installato.
  * Database di destinazione in cui eseguire la migrazione.

* Computer Linux con gli elementi seguenti installati:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) o [Ubuntu](quickstart-install-connect-ubuntu.md)) con gli strumenti da riga di comando.

## <a name="create-a-backup-on-windows"></a>Creare un backup in Windows

Esistono diversi modi per creare un file di backup di un database in Windows. La procedura seguente usa SQL Server Management Studio (SSMS).

1. Avviare **SQL Server Management Studio** nel computer Windows.

1. Nella finestra di dialogo della connessione immettere **localhost**.

1. In Esplora oggetti espandere **Database**.

1. Fare clic con il pulsante destro del mouse sul database di destinazione, scegliere **Attività** e quindi fare clic su **Backup**.

   ![Usare SSMS per creare un file di backup](./media/sql-server-linux-migrate-restore-database/ssms-create-backup.png)

1. Nella finestra di dialogo **Backup database** verificare che **Tipo backup** sia impostato su **Completo** e che **Backup su** sia impostato su **Disco**. Prendere nota del nome e del percorso del file. Ad esempio, un database denominato **YourDB** in SQL Server 2016 ha il percorso predefinito `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak`.

1. Fare clic su **OK** per eseguire un backup del database.

> [!NOTE]
> Un'altra opzione consiste nell'eseguire una query Transact-SQL per creare il file di backup. Il comando Transact-SQL seguente esegue le stesse azioni dei passaggi precedenti per un database denominato **YourDB**:
>
> ```sql
> BACKUP DATABASE [YourDB] TO  DISK =
> N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak'
> WITH NOFORMAT, NOINIT, NAME = N'YourDB-Full Database Backup',
> SKIP, NOREWIND, NOUNLOAD, STATS = 10
> GO
> ```

## <a name="install-a-bash-shell-on-windows"></a>Installare una shell Bash in Windows

Per ripristinare il database, è prima necessario trasferire il file di backup dal computer Windows al computer Linux di destinazione. In questa esercitazione il file viene spostato in Linux da una shell Bash (finestra del terminale) in esecuzione in Windows.

1. Installare una shell Bash nel computer Windows che supporta i comandi **scp** (copia sicura) e **ssh** (accesso remoto). Due esempi di tali situazioni:

   * [Sottosistema Windows per Linux](/windows/wsl/about) (Windows 10)
   * Shell git bash ( [https://git-scm.com/downloads](https://git-scm.com/downloads) )

1. Aprire una sessione di Bash in Windows.

## <a name="copy-the-backup-file-to-linux"></a><a id="scp"></a> Copiare il file di backup in Linux

1. Nella sessione di Bash passare alla directory che contiene il file di backup. Ad esempio:

   ```bash
   cd 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\'
   ```

1. Usare il comando **scp** per trasferire il file nel computer Linux di destinazione. L'esempio seguente trasferisce **YourDB.bak** nella home directory di *user1* nel server Linux con indirizzo IP *192.0.2.9*:

   ```bash
   scp YourDB.bak user1@192.0.2.9:./
   ```
   ![Comando scp](./media/sql-server-linux-migrate-restore-database/scp-command.png)

> [!TIP]
> Sono disponibili alternative all'uso di SCP per il trasferimento del file. Una prevede l'uso di [Samba](https://help.ubuntu.com/community/Samba) per configurare una condivisione di rete SMB tra Windows e Linux. Per una procedura dettagliata in Ubuntu, vedere [How to Create a Network Share Via Samba](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!) (Come creare una condivisione di rete tramite Samba). Una volta stabilita, è possibile accedervi come una condivisione di file di rete da Windows, ad esempio **\\ \\ machinenameorip \\ share**.

## <a name="move-the-backup-file-before-restoring"></a>Spostare il file di backup prima del ripristino

A questo punto, il file di backup si trova nel server Linux nella home directory dell'utente. Prima di ripristinare il database in SQL Server, è necessario inserire il backup in una sottodirectory di **/var/opt/MSSQL**, in quanto è di proprietà dell'utente `mssql` e del gruppo `mssql` . Per modificare il percorso di backup predefinito, vedere l'articolo [configurare con MSSQL-conf](sql-server-linux-configure-mssql-conf.md#backupdir) .

1. Nella stessa sessione di Bash in Windows connettersi in modalità remota al computer Linux di destinazione con **ssh**. Nell'esempio seguente viene eseguita la connessione al computer Linux **192.0.2.9** come utente **user1**.

   ```bash
   ssh user1@192.0.2.9
   ```

   A questo punto i comandi vengono eseguiti nel server Linux remoto.

1. Entrare in modalità utente con privilegi avanzati.

   ```bash
   sudo su
   ```

1. Creare una nuova directory di backup. Il parametro -p non esegue alcuna operazione se la directory esiste già.

   ```bash
   mkdir -p /var/opt/mssql/backup
   ```

1. Spostare il file di backup in tale directory. Nell'esempio seguente il file di backup si trova nella home directory di *user1*. Modificare il comando in modo che corrisponda al percorso e al nome del file di backup.

   ```bash
   mv /home/user1/YourDB.bak /var/opt/mssql/backup/
   ```

1. Uscire dalla modalità utente con privilegi avanzati.

   ```bash
   exit
   ```

## <a name="restore-your-database-on-linux"></a>Ripristinare il database in Linux

Per ripristinare il backup del database, è possibile usare il comando **RESTORE DATABASE** di Transact-SQL.

> [!NOTE]
> Nei passaggi seguenti viene usato lo strumento **sqlcmd**. Se gli strumenti di SQL Server non sono stati installati, vedere [Installare gli strumenti da riga di comando di SQL Server in Linux](sql-server-linux-setup-tools.md).

1. Nello stesso terminale avviare **sqlcmd**. Nell'esempio seguente viene eseguita la connessione all'istanza di SQL Server locale con l'utente **SA**. Immettere la password quando richiesto o specificare la password aggiungendo il parametro **-P**.

   ```bash
   sqlcmd -S localhost -U SA
   ```

1. Al prompt `>1` immettere il comando **RESTORE DATABASE** seguente, premendo INVIO dopo ogni riga. Non è possibile copiare e incollare tutte le righe del comando in una sola volta. Sostituire tutte le occorrenze di `YourDB` con il nome del database.

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```

   Verrà visualizzato un messaggio che informa che il database è stato ripristinato.

   `RESTORE DATABASE` può restituire un errore come quello dell'esempio seguente:

   ```bash
   File 'YourDB_Product' cannot be restored to 'Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf'. Use WITH MOVE to identify a valid location for the file.
   Msg 5133, Level 16, State 1, Server servername, Line 1
   Directory lookup for the file "Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf" failed with the operating system error 2(The system cannot find the file specified.).
   ```
   
   In questo caso, il database contiene file secondari. Se questi file non sono specificati nella clausola `MOVE` di `RESTORE DATABASE`, la procedura di ripristino proverà a crearli nello stesso percorso del server originale. 

   È possibile elencare tutti i file inclusi nel backup:
   ```sql
   RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   GO
   ```
   Verrà visualizzato un elenco simile a quello seguente (che elenca solo le prime due colonne):
   ```sql
   LogicalName         PhysicalName                                                                 ..............
   ----------------------------------------------------------------------------------------------------------------------
   YourDB              Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB.mdf          ..............
   YourDB_Product      Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf  ..............
   YourDB_Customer     Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Customer.ndf ..............
   YourDB_log          Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Log.ldf      ..............
   ```
   
   È possibile usare questo elenco per creare le clausole `MOVE` per i file aggiuntivi. In questo esempio `RESTORE DATABASE` è:

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Product' TO '/var/opt/mssql/data/YourDB_Product.ndf',
   MOVE 'YourDB_Customer' TO '/var/opt/mssql/data/YourDB_Customer.ndf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```


1. Verificare il ripristino elencando tutti i database nel server. Il database ripristinato deve essere elencato.

   ```sql
   SELECT Name FROM sys.Databases
   GO
   ```

1. Eseguire altre query sul database di cui è stata eseguita la migrazione. Il comando seguente sostituisce il contesto con il database **YourDB** e seleziona alcune righe da una delle tabelle.

   ```sql
   USE YourDB
   SELECT * FROM YourTable
   GO
   ```

1. Dopo aver terminato di usare **sqlcmd**, digitare `exit`.

1. Dopo aver terminato di usare la sessione **ssh** remota, digitare di nuovo `exit`.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come eseguire il backup di un database in Windows e come spostarlo in un server Linux che esegue SQL Server. Si è appreso come:
> [!div class="checklist"]
> * Usare SSMS e Transact-SQL per creare un file di backup in Windows
> * Installare una shell Bash in Windows
> * Usare **scp** per spostare i file di backup da Windows a Linux
> * Usare **ssh** per connettersi in modalità remota al computer Linux
> * Rilocare il file di backup per prepararsi al ripristino
> * Usare **sqlcmd** per eseguire i comandi Transact-SQL
> * Ripristinare il backup del database con il comando **RESTORE DATABASE** 
> * Eseguire la query per verificare la migrazione

Esaminare quindi gli altri scenari di migrazione per SQL Server in Linux. 

> [!div class="nextstepaction"]
>[Eseguire la migrazione di database a SQL Server in Linux](sql-server-linux-migrate-overview.md)
