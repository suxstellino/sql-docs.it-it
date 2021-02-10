---
title: Installare PolyBase in Linux
titlesuffix: SQL Server
description: Informazioni su come installare PolyBase di SQL Server in Linux. PolyBase consente di eseguire query esterne su origini dati remote.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: dakryze
ms.date: 8/18/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 85beab1eb4d256c0433678ca34a8ac2627ebfb52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062256"
---
# <a name="install-polybase-on-linux"></a>Installare PolyBase in Linux

[!INCLUDE [sqlserver2019-linux](../../includes/applies-to-version/sqlserver2019-linux.md)]

Seguire questi passaggi per installare [PolyBase](../../relational-databases/polybase/polybase-guide.md) (`mssql-server-polybase` e `mssql-server-polybase-hadoop`) in Linux. PolyBase consente di eseguire query esterne su origini dati remote.

>[!NOTE]
> Prima di installare PolyBase, [installare SQL Server 2019](../../linux/sql-server-linux-setup.md#platforms). In questo modo verranno configurati le chiavi e i repository usati durante l'installazione del pacchetto `mssql-server-polybase` e `mssql-server-polybase-hadoop`.

>[!NOTE]
>
> - PolyBase non è supportato in SQL Server 2017 per Linux.
> - Lo scale-out non è attualmente disponibile per PolyBase in Linux.

Installare PolyBase per il sistema operativo in uso:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name="install-on-rhel"></a><a name="RHEL"></a>Eseguire l'installazione in RHEL

Usare il comando seguente per installare `mssql-server-polybase` in Red Hat Enterprise Linux. 

```bash
sudo yum install -y mssql-server-polybase
```

Verrà richiesto di riavviare l'istanza di SQL Server. Usare il comando seguente per eseguire questa operazione.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Dopo l'installazione, è necessario [abilitare la funzionalità PolyBase](#enable).

Immettere il comando seguente per installare `mssql-server-polybase-hadoop`. 

```bash
sudo yum install -y mssql-server-polybase-hadoop
```

Il pacchetto PolyBase Hadoop include dipendenze dai pacchetti seguenti:
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

L'installazione richiede di riavviare `launchpadd`. Usare il comando seguente per eseguire questa operazione.

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Dopo l'installazione, è necessario [impostare il livello di connettività di Hadoop](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#c-set-hadoop-connectivity).

Se è necessaria un'installazione offline, individuare il download del pacchetto di PolyBase nelle [Note sulla versione](../../linux/sql-server-linux-release-notes.md). Usare quindi la stessa procedura di installazione offline descritta nell'articolo [Installare SQL Server](../../linux/sql-server-linux-setup.md#offline).

## <a name="install-on-ubuntu"></a><a name="ubuntu"></a>Eseguire l'installazione in Ubuntu

Usare il comando seguente per installare `mssql-server-polybase` in Ubuntu. 

```bash
sudo apt-get install mssql-server-polybase
```

Verrà richiesto di riavviare l'istanza di SQL Server. Usare il comando seguente per eseguire questa operazione.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Dopo l'installazione, è necessario [abilitare la funzionalità PolyBase](#enable).

Se è necessaria un'installazione offline, individuare il download del pacchetto di PolyBase nelle [Note sulla versione](../../linux/sql-server-linux-release-notes.md). Usare quindi la stessa procedura di installazione offline descritta nell'articolo [Installare SQL Server](../../linux/sql-server-linux-setup.md#offline).

Immettere il comando seguente per installare `mssql-server-polybase-hadoop`. 

```bash
sudo apt-get install mssql-server-polybase-hadoop
```

Il pacchetto PolyBase Hadoop include dipendenze dai pacchetti seguenti:
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

L'installazione richiede di riavviare `launchpadd`. Usare il comando seguente per eseguire questa operazione.

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Dopo l'installazione, è necessario [impostare il livello di connettività di Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md#configure-hadoop-connectivity).

## <a name="install-on-sles"></a><a name="SLES"></a>Eseguire l'installazione in SLES

Usare i comandi seguenti per installare `mssql-server-polybase` in SUSE Linux Enterprise Server. 

```bash
sudo zypper install mssql-server-polybase
```

Verrà richiesto di riavviare l'istanza di SQL Server. Usare il comando seguente per eseguire questa operazione.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Dopo l'installazione, è necessario [abilitare la funzionalità PolyBase](#enable).

Se è necessaria un'installazione offline, individuare il download del pacchetto di PolyBase nelle [Note sulla versione](../../linux/sql-server-linux-release-notes.md). Usare quindi la stessa procedura di installazione offline descritta nell'articolo [Installare SQL Server](../../linux/sql-server-linux-setup.md#offline).


## <a name="enable-polybase"></a><a name="enable"></a> Abilitare PolyBase

Dopo l'installazione, è necessario abilitare PolyBase per accedere alle relative funzionalità. Connettersi all'istanza installata di SQL Server e usare il comando di Transact-SQL seguente per eseguire l'abilitazione.

```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="update-polybase"></a>Aggiornare PolyBase

Se il pacchetto `mssql-server-polybase` è già installato, è possibile eseguire l'aggiornamento all'ultima versione con i comandi seguenti:

### <a name="rhel"></a>RHEL

```bash
sudo yum remove -y mssql-server-polybase-hadoop
sudo yum remove -y mssql-server-polybase
sudo yum check-update
sudo yum install -y mssql-server-polybase
sudo yum install -y mssql-server-polybase-hadoop
```

Verrà richiesto di riavviare l'istanza di SQL Server. Usare il comando seguente per eseguire questa operazione.

```
sudo systemctl restart mssql-server
```

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get remove mssql-server-polybase-hadoop
sudo apt-get remove mssql-server-polybase
sudo apt-get update 
sudo apt-get install mssql-server-polybase
sudo apt-get remove mssql-server-polybase-hadoop
```

Verrà richiesto di riavviare l'istanza di SQL Server. Usare il comando seguente per eseguire questa operazione.

```
sudo systemctl restart mssql-server
```

### <a name="sles"></a>SLES

```bash
sudo zypper remove mssql-server-polybase
sudo zypper refresh
sudo zypper install mssql-server-polybase
```

Verrà richiesto di riavviare l'istanza di SQL Server. Usare il comando seguente per eseguire questa operazione.

```
sudo systemctl restart mssql-server
```

>[!NOTE]
>Dopo l'installazione, è necessario [abilitare la funzionalità PolyBase](#enable).

## <a name="next-steps"></a>Passaggi successivi

PolyBase in Linux può accedere alle origini dati seguenti. Vedere i collegamenti forniti per altre informazioni su come creare una tabella esterna da queste origini quando PolyBase è abilitato. 

- [SQL Server, Database SQL, Azure Synapse Analytics](../../relational-databases/polybase/polybase-configure-sql-server.md)
- [Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md)
- [Archiviazione BLOB di Azure](../../relational-databases/polybase/polybase-configure-azure-blob-storage.md)
- [Oracle](../../relational-databases/polybase/polybase-configure-oracle.md)
- [Teradata](../../relational-databases/polybase/polybase-configure-teradata.md)
- [MongoDB (e Cosmos DB)](../../relational-databases/polybase/polybase-configure-mongodb.md)

Per altre informazioni su come usare questa funzionalità, vedere l'articolo di riferimento per Transact-SQL per [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
