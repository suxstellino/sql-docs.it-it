---
title: Configurare la replica (SSMS)
description: Informazioni su come configurare la replica di SQL Server in Linux. Configurare la replica tramite stored procedure di SQL Server Management Studio (SSMS) o Transact-SQL.
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/20/2018
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 911446480834b9ff8b51ce806729f2771d28d84f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346489"
---
# <a name="configure-sql-server-replication-on-linux"></a>Configurare la replica di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] introduce la replica di SQL Server per le istanze di SQL Server in Linux.

Per informazioni dettagliate sulla replica, vedere la [documentazione della replica di SQL Server](../relational-databases/replication/sql-server-replication.md).

Configurare la replica in Linux tramite stored procedure di SQL Server Management Studio (SSMS) o Transact-SQL.

* Per usare SSMS, seguire le istruzioni riportate in questo articolo.

  Usare SSMS in un sistema operativo Windows per connettersi a istanze di SQL Server. Per informazioni di background e istruzioni, vedere [Usare SSMS per gestire SQL Server in Linux](./sql-server-linux-manage-ssms.md).
  
* Per un esempio con stored procedure, seguire l'esercitazione [Configurare la replica di SQL Server in Linux](sql-server-linux-replication-tutorial-tsql.md).

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare i server di pubblicazione, i database di distribuzione e i sottoscrittori, è necessario completare alcuni passaggi di configurazione per l'istanza di SQL Server.

1. Abilitare l'uso degli agenti di replica in SQL Server Agent. Nel terminale di tutti i server Linux eseguire i comandi seguenti.

  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
  sudo systemctl restart mssql-server
  ```

1. Configurare l'istanza di SQL Server per la replica. Per configurare l'istanza di SQL Server per la replica, eseguire `sys.sp_MSrepl_createdatatypemappings` in tutte le istanze interessate dalla replica.

  ```sql
  USE msdb
  GO
  exec sys.sp_MSrepl_createdatatypemappings;
  GO
  ```

1. Creare una cartella snapshot. Gli agenti SQL Server richiedono una cartella snapshot in cui leggere o scrivere. Creare la cartella snapshot nel server di distribuzione.

  Per creare la cartella snapshot e consentirne l'accesso all'utente `mssql`, eseguire il comando seguente:

  ```bash
  sudo mkdir /var/opt/mssql/data/ReplData/
  sudo chown mssql /var/opt/mssql/data/ReplData/
  sudo chgrp mssql /var/opt/mssql/data/ReplData/
  ```

## <a name="configure-and-monitor-replication-with-sql-server-management-studio-ssms"></a>Eseguire la configurazione e il monitoraggio della replica con SQL Server Management Studio (SSMS)

### <a name="configure-the-distributor"></a>Configurare il server di distribuzione
  
Per configurare il server di distribuzione: 

1. In SSMS connettersi all'istanza di SQL Server in Esplora oggetti.

1. Fare clic con il pulsante destro del mouse su **Replica** e scegliere **Configura distribuzione...**.

1. Seguire le istruzioni della **Configurazione guidata distribuzione**.

### <a name="create-publication-and-articles"></a>Creare la pubblicazione e gli articoli

Per creare la pubblicazione e gli articoli:

1. In Esplora oggetti fare clic su **Replica** > **Pubblicazioni locali**> **Nuova pubblicazione...** .

1. Seguire le istruzioni della **Creazione guidata nuova pubblicazione** per configurare il tipo di replica e gli articoli che appartengono alla pubblicazione.

### <a name="configure-the-subscription"></a>Configurare la sottoscrizione

Per configurare la sottoscrizione in Esplora oggetti, fare clic su **Replica** > **Sottoscrizioni locali**> **Nuova sottoscrizione...** .

### <a name="monitor-replication-jobs"></a>Eseguire il monitoraggio dei processi di replica

Usare Monitoraggio replica per eseguire il monitoraggio dei processi di replica.

In Esplora oggetti fare clic con il pulsante destro del mouse su **Replica** e fare clic su **Avvia Monitoraggio replica**.

## <a name="next-steps"></a>Passaggi successivi

[Concetti: replica di SQL Server in Linux](sql-server-linux-replication.md)

[Stored procedure per la replica](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).
