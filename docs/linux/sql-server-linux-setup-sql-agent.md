---
title: Configurare SQL Server Agent in Linux
description: Informazioni su come abilitare o installare SQL Server Agent in Linux. A partire da SQL Server 2017 CU4, SQL Server Agent è incluso nel pacchetto mssql-server.
author: VanMSFT
ms.author: vanto
ms.date: 12/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 77f16adc-e6cb-4a57-82f3-7b9780369868
ms.openlocfilehash: 8ca8de4b922ad2108ad40996713cf47501b29dca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346395"
---
# <a name="install-sql-server-agent-on-linux"></a>Installare SQL Server Agent in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo descrive come abilitare o installare SQL Server Agent in Linux.

[SQL Server Agent](../ssms/agent/sql-server-agent.md) esegue processi di SQL Server pianificati. A partire da SQL Server 2017 CU4, SQL Server Agent è incluso nel pacchetto **mssql-server** ed è disabilitato per impostazione predefinita. Per informazioni sulle funzionalità supportate per questa versione di SQL Server Agent, nonché per informazioni sulla versione, vedere le [note sulla versione](sql-server-linux-release-notes.md).

## <a name="instructions"></a>Istruzioni

Prima di usare SQL Server Agent in Linux, attenersi alla procedura seguente per abilitarlo o installarlo.

1. Aggiungere il nome host (con e senza dominio) nei file `/etc/hosts`. Le righe seguenti sono un esempio del formato di queste voci:

   ```bash
   "IP Address" "hostname"
   "IP Address" "hostname.domain.com"
   ```

1. Seguire le istruzioni riportate in una delle sezioni seguenti a seconda della versione di SQL Server:

   | Versioni | Istruzioni |
   |---|---|
   | SQL Server 2017 CU4 e versioni successive</br>SQL Server 2019 | [Abilitare SQL Server Agent](#EnableAgentAfterCU4) |
   | SQL Server 2017 CU3 e versioni precedenti | [Installare SQL Server Agent](#InstallAgentBelowCU4) |

## <a name="enable-the-sql-server-agent"></a><a id="EnableAgentAfterCU4"></a>Abilitare SQL Server Agent

Per SQL Server 2019, SQL Server 2017 CU4 e versioni successive, è sufficiente abilitare SQL Server Agent. Non è necessario installare un pacchetto separato.

Per abilitare SQL Server Agent, seguire questa procedura.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
sudo systemctl restart mssql-server
```

> [!NOTE]
> Se si esegue l'aggiornamento da 2017 CU3 o versione precedente e SQL Server Agent è installato, SQL Server Agent verrà abilitato automaticamente e i pacchetti di SQL Server Agent precedenti verranno disinstallati.  

## <a name="install-the-sql-server-agent"></a><a name="InstallAgentBelowCU4"></a>Installare SQL Server Agent

Per SQL Server 2017 CU3 e versioni precedenti, è necessario installare il pacchetto di SQL Server Agent.

> [!NOTE]
> Le istruzioni di installazione riportate di seguito si applicano a SQL Server versioni 2017 CU3 e precedenti. Prima di installare SQL Server Agent, [installare SQL Server](sql-server-linux-setup.md#platforms). In questo modo verranno configurate le chiavi e i repository usati durante l'installazione del pacchetto **mssql-server-agent**.

Installare SQL Server Agent per la piattaforma in uso:
- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

### <a name=""></a><a name="RHEL">Eseguire l'installazione in RHEL</a>

Seguire questa procedura per installare **mssql-server-agent** in Red Hat Enterprise Linux. 

```bash
sudo yum install mssql-server-agent
sudo systemctl restart mssql-server
```

Se il pacchetto **mssql-server-agent** è già installato, è possibile eseguire l'aggiornamento all'ultima versione con i comandi seguenti:

```bash
sudo yum check-update
sudo yum update mssql-server-agent
sudo systemctl restart mssql-server
```

Se è necessaria un'installazione offline, individuare il download del pacchetto di SQL Server Agent nelle [Note sulla versione](sql-server-linux-release-notes.md). Usare quindi la stessa procedura di installazione offline descritta nell'articolo [Installare SQL Server](sql-server-linux-setup.md#offline).

### <a name=""></a><a name="ubuntu">Eseguire l'installazione in Ubuntu</a>

Seguire questa procedura per installare **mssql-server-agent** in Ubuntu. 

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Se il pacchetto **mssql-server-agent** è già installato, è possibile eseguire l'aggiornamento all'ultima versione con i comandi seguenti:

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Se è necessaria un'installazione offline, individuare il download del pacchetto di SQL Server Agent nelle [Note sulla versione](sql-server-linux-release-notes.md). Usare quindi la stessa procedura di installazione offline descritta nell'articolo [Installare SQL Server](sql-server-linux-setup.md#offline).

### <a name=""></a><a name="SLES">Eseguire l'installazione in SLES</a>

Seguire questa procedura per installare **mssql-server-agent** in SUSE Linux Enterprise Server. 

Installare **mssql-server-agent** 

```bash
sudo zypper install mssql-server-agent
sudo systemctl restart mssql-server
```

Se il pacchetto **mssql-server-agent** è già installato, è possibile eseguire l'aggiornamento all'ultima versione con i comandi seguenti:

```bash
sudo zypper refresh
sudo zypper update mssql-server-agent
sudo systemctl restart mssql-server
```

Se è necessaria un'installazione offline, individuare il download del pacchetto di SQL Server Agent nelle [Note sulla versione](sql-server-linux-release-notes.md). Usare quindi la stessa procedura di installazione offline descritta nell'articolo [Installare SQL Server](sql-server-linux-setup.md#offline).

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni su come usare SQL Server Agent per creare, pianificare ed eseguire processi, vedere [Eseguire un processo di SQL Server Agent in Linux](sql-server-linux-run-sql-server-agent-job.md).