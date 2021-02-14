---
title: Novità di SQL Server 2017 in Linux
description: Questo articolo descrive le funzionalità e i servizi principali disponibili per SQL Server 2017 in esecuzione in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e4d772f7355982b1856ac467558242c561e6b391
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346194"
---
# <a name="whats-new-for-sql-server-2017-on-linux"></a>Novità di SQL Server 2017 in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo descrive le funzionalità e i servizi principali disponibili per SQL Server 2017 in esecuzione in Linux.

> [!NOTE]
> Oltre alle funzionalità descritte in questo articolo, vengono rilasciati a intervalli regolari aggiornamenti cumulativi che mettono a disposizione numerosi miglioramenti e diverse correzioni. Per informazioni dettagliate sulla versione più recente degli aggiornamenti cumulativi, vedere [https://aka.ms/sql2017cu](https://aka.ms/sql2017cu). Per il download dei pacchetti e per informazioni sui problemi noti, vedere le [Note sulla versione ](sql-server-linux-release-notes.md).

## <a name="ubuntu-1804-supported"></a>Supporto per Ubuntu 18.04

A partire da SQL Server 2017 CU20, ora è supportato Ubuntu 18.04. Vedere l'articolo di avvio rapido [Installazione di SQL Server e creazione di un database in Ubuntu](quickstart-install-connect-ubuntu.md).

## <a name="rhel-8-supported"></a>Supporto per RHEL 8

A partire da SQL Server 2017 CU20, ora è supportato RHEL 8. Vedere l'articolo di avvio rapido [Installazione di SQL Server e creazione di un database in Red Hat](quickstart-install-connect-red-hat.md).

## <a name="sql-server-database-engine"></a>Motore di database di SQL Server

- Abilitazione delle funzionalità di base del motore di database di SQL Server.
- Supporto dei percorsi Linux nativi.
- Supporto di IPV6.
- Supporto dei file di database in NFS.
- Abilitazione della crittografia [Transport Layer Security](sql-server-linux-encrypted-connections.md) (TLS).
- Abilitazione dell'[autenticazione di Active Directory](sql-server-linux-active-directory-authentication.md).
- [Funzionalità Gruppi di disponibilità](sql-server-linux-availability-group-overview.md) per garantire la disponibilità elevata.
- Supporto della [ricerca full-text](sql-server-linux-setup-full-text-search.md).

## <a name="sql-server-agent"></a>SQL Server Agent

- Supporto di [SQL Server Agent](sql-server-linux-setup-sql-agent.md) per le attività seguenti:
  - [Processi Transact-SQL](sql-server-linux-run-sql-server-agent-job.md)
  - [Posta elettronica database](sql-server-linux-db-mail-sql-agent.md)
  - [Log shipping](sql-server-linux-use-log-shipping.md)

## <a name="sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS)

- Possibilità di eseguire pacchetti SSIS in Linux. Per altre informazioni, vedere [Configurare SQL Server Integration Services in Linux con ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="other-improvements"></a>Altri miglioramenti

- Strumento di configurazione da riga di comando [mssql-conf](sql-server-linux-configure-mssql-conf.md).
- Supporto dell'installazione automatica con [variabili di ambiente](sql-server-linux-configure-environment-variables.md).
- [Estensione mssql-server di Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) multipiattaforma.
- Generatore di script multipiattaforma, [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli/blob/dev/doc/usage_guide.md).
- Strumento di monitoraggio DMV multipiattaforma, [DBFS](https://github.com/Microsoft/dbfs).

## <a name="next-steps"></a>Passaggi successivi

Per installare SQL Server in Linux, usare una delle esercitazioni seguenti:

- [Eseguire l'installazione in Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Eseguire l'installazione in SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Eseguire l'installazione in Ubuntu](quickstart-install-connect-ubuntu.md)
- [Esecuzione in Docker](quickstart-install-connect-docker.md)
- [Eseguire il provisioning di una macchina virtuale SQL in Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](sql-server-linux-faq.md). Per informazioni su altri miglioramenti introdotti in SQL Server 2017, vedere [Novità di SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
