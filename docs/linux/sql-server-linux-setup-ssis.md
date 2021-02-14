---
title: Installare SQL Server Integration Services in Linux
description: Questo articolo descrive come installare SQL Server Integration Services (SSIS) in Linux. È possibile installare SSIS in Ubuntu 16.04 e Red Hat Enterprise Linux.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 311b8fec7114c4d63680d131ac0988ad4f4a8b15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346366"
---
# <a name="install-sql-server-integration-services-ssis-on-linux"></a>Installare SQL Server Integration Services (SSIS) in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Per installare SQL Server Integration Services (**mssql-server-is**) in Linux, seguire le procedure descritte in questo articolo. Per informazioni sulle funzionalità supportate in questa versione di Integration Services per Linux, vedere le [note sulla versione](sql-server-linux-release-notes.md).

È possibile installare SQL Server Integration Services nelle piattaforme seguenti:

- [Ubuntu 16.04](#ubuntu)
- [Red Hat Enterprise Linux](#RHEL)

## <a name="install-ssis-on-ubuntu"></a><a name="ubuntu"></a> Installare SSIS in Ubuntu

Per installare il pacchetto **mssql-server-is** in Ubuntu, seguire questa procedura:

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Importare le chiavi GPG del repository pubblico.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrare il repository SQL Server per Ubuntu.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

1. Eseguire i comandi seguenti per installare SQL Server Integration Services.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. Dopo aver installato Integration Services, eseguire **ssis-conf**. Per altre informazioni, vedere [Configurare SSIS in Linux con ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Al termine della configurazione, impostare la variabile di ambiente PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Importare le chiavi GPG del repository pubblico.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrare il repository SQL Server per Ubuntu.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```

1. Eseguire i comandi seguenti per installare SQL Server Integration Services.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. Dopo aver installato Integration Services, eseguire **ssis-conf**. Per altre informazioni, vedere [Configurare SSIS in Linux con ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Al termine della configurazione, impostare la variabile di ambiente PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>Aggiornare SSIS

Se il pacchetto **mssql-server-is** è già installato, eseguire l'aggiornamento all'ultima versione con il comando seguente:

```bash
sudo apt-get install mssql-server-is
```

### <a name="remove-ssis"></a>Rimuovere SSIS

Per rimuovere **mssql-server-is**, eseguire il comando seguente:

```bash
sudo apt-get remove mssql-server-is
```

## <a name="install-ssis-on-rhel"></a><a name="RHEL"></a> Installare SSIS in RHEL
Per installare il pacchetto **mssql-server-is** in RHEL, seguire questa procedura:

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Scaricare il file di configurazione del repository SQL Server per Red Hat.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```

1. Eseguire il comando seguente per installare SQL Server Integration Services.

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. Dopo l'installazione, eseguire **ssis-conf**. Per altre informazioni, vedere [Configurare SSIS in Linux con ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Al termine della configurazione, impostare la variabile di ambiente PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Scaricare il file di configurazione del repository SQL Server per Red Hat.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```

1. Eseguire il comando seguente per installare SQL Server Integration Services.

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. Dopo l'installazione, eseguire **ssis-conf**. Per altre informazioni, vedere [Configurare SSIS in Linux con ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Al termine della configurazione, impostare la variabile di ambiente PATH.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>Aggiornare SSIS

Se il pacchetto **mssql-server-is** è già installato, eseguire l'aggiornamento all'ultima versione con il comando seguente:

```bash
sudo yum update mssql-server-is
```

### <a name="remove-ssis"></a>Rimuovere SSIS
Per rimuovere **mssql-server-is**, eseguire il comando seguente:

```bash
sudo yum remove mssql-server-is
```

## <a name="unattended-installation"></a>Installazione automatica

Per eseguire **ssis-conf setup** come installazione automatica, seguire questa procedura:

1. Specificare l'opzione **-n** (nessun prompt).
1. Per specificare i valori obbligatori, impostare variabili di ambiente.

L'esempio seguente esegue queste operazioni:

- Installa SSIS
- Indica l'edizione Developer specificando un valore per la variabile di ambiente SSIS_PID
- Accetta le condizioni di licenza software Microsoft fornendo un valore per la variabile di ambiente ACCEPT_EULA
- Esegue l'installazione automatica specificando l'opzione **-n** (nessun prompt)

```
sudo SSIS_PID=Developer ACCEPT_EULA=Y /opt/ssis/bin/ssis-conf -n setup 
```

### <a name="environment-variables-for-unattended-installation"></a>Variabili di ambiente per l'installazione automatica

| Variabile di ambiente | Descrizione |
|---|---|
| ACCEPT_EULA | Accetta le condizioni di licenza di SQL Server quando è impostato su un valore qualsiasi come "Y".|
| SSIS_PID | Imposta l'edizione o il codice Product Key di SQL Server. Ecco i valori possibili:<ul><li>Versione di valutazione</li><li>Sviluppatore</li><li>Express</li><li>Web</li><li>Standard</li><li>Funzionalità per le aziende</li><li>Un codice Product Key</li></ul>Se si specifica un codice Product Key, questo deve avere il formato *#####* - *#####* - *#####* - *#####* - *#####* , dove *#* è una lettera o un numero.  |
| | |

## <a name="next-steps"></a>Passaggi successivi

Per eseguire pacchetti SSIS in Linux, vedere [Estrarre, trasformare e caricare i dati per SQL Server in Linux con SSIS](sql-server-linux-migrate-ssis.md).

Per configurare impostazioni SSIS aggiuntive in Linux, vedere [Configurare SQL Server Integration Services in Linux con ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="related-content-about-ssis-on-linux"></a>Contenuto correlato su SSIS in Linux

- [Estrarre, trasformare e caricare i dati in Linux con SSIS](sql-server-linux-migrate-ssis.md)
- [Configurare SQL Server Integration Services in Linux con ssis-conf](sql-server-linux-configure-ssis.md)
- [Limitazioni e problemi noti di SSIS in Linux](sql-server-linux-ssis-known-issues.md)
- [Pianificare l'esecuzione del pacchetto SQL Server Integration Services in Linux con cron](sql-server-linux-schedule-ssis-packages.md)
