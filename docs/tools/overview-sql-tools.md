---
title: Panoramica degli strumenti SQL
description: Strumenti di gestione e query SQL per SQL Server, Azure SQL (database SQL di Azure, istanza gestita di SQL di Azure, macchine virtuali SQL) e Azure Synapse Analytics.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 3595e84b4da525894c353c8b5d702b69b19f354d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338688"
---
# <a name="sql-tools-overview"></a>Panoramica degli strumenti SQL

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Per gestire il database, è necessario uno strumento. Sia che i database vengano eseguiti nel cloud, in Windows, in macOS o in [Linux](../linux/sql-server-linux-overview.md), non è necessario che lo strumento venga eseguito sulla stessa piattaforma del database.

È possibile visualizzare i collegamenti ai diversi strumenti SQL nelle tabelle seguenti.

> [!Note]
> Per scaricare SQL Server, vedere [Installare SQL Server](../database-engine/install-windows/install-sql-server.md).

## <a name="recommended-tools"></a>Strumenti consigliati

Gli strumenti seguenti offrono un'interfaccia utente grafica (GUI).

| Strumento | Descrizione | Sistema operativo |
|:--|:--|:--|
| [ **![ADS image](../tools/media/overview-sql-tools/azure-data-studio.svg)</br></br>Azure Data Studio**](../azure-data-studio/download.md) | Editor leggero che consente di eseguire query SQL su richiesta e visualizzare e salvare i risultati come testo, JSON o Excel, oltre che modificare i dati, organizzare le connessioni ai database preferiti e sfogliare gli oggetti di database in un'esperienza di esplorazione familiare. | **Windows</br>macOS</br>Linux** |
| [ **![SSMS image](../tools/media/overview-sql-tools/ssms.svg)</br></br>SQL Server Management Studio (SSMS)**](../ssms/download-sql-server-management-studio-ssms.md) | Consente di gestire un'istanza o un database SQL Server con il supporto completo di un'interfaccia utente grafica, nonché di accedere, configurare, gestire, amministrare e sviluppare tutti i componenti di SQL Server, del database SQL di Azure e di Azure Synapse Analytics. Offre una singola utilità completa che integra un'ampia gamma di strumenti grafici con numerosi editor di script avanzati per garantire l'accesso a SQL per gli sviluppatori e gli amministratori di database con qualsiasi livello di competenza. | **Windows** |
| [ **![SSDT image](../tools/media/overview-sql-tools/ssdt.svg)</br>SQL Server Data Tools (SSDT)**](../ssdt/download-sql-server-data-tools-ssdt.md) | Moderno strumento di sviluppo che consente di compilare database relazionali SQL Server, database SQL di Azure, modelli di dati di Analysis Services (AS), pacchetti di Integration Services (IS) e report di Reporting Services (RS). Con SSDT è possibile progettare e distribuire qualsiasi tipo di contenuto SQL Server con la stessa facilità con la quale si sviluppa un'applicazione in **[Visual Studio](https://visualstudio.microsoft.com/downloads/)** . | **Windows** |
| [ **![VS Code image](../tools/media/overview-sql-tools/visual-studio-code.svg)</br></br>Visual Studio Code**](https://code.visualstudio.com/) | L' **[estensione mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)** per Visual Studio Code è l'estensione ufficiale di SQL Server che supporta le connessioni a SQL Server e un'esperienza di modifica avanzata per T-SQL in Visual Studio Code. Scrivere script T-SQL in un editor leggero. | **Windows</br>macOS</br>Linux** |

## <a name="command-line-tools"></a>Strumenti da riga di comando

Gli strumenti seguenti sono i principali strumenti da riga di comando.

| Strumento | Descrizione | Sistema operativo |
|:--|:--|:--|
|[**bcp**](bcp-utility.md)|L'utilità del **p** rogramma di **c** opia **b** ulk (**bcp**) esegue operazioni di copia bulk di dati tra un'istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e un file di dati in un formato specificato dall'utente.| **Windows</br>macOS</br>Linux** |
|[**mssql-cli (anteprima)**](mssql-cli.md)|**mssql-cli** è uno strumento da riga di comando interattivo per l'esecuzione di query su SQL Server. Inoltre, è possibile eseguire query su SQL Server con uno strumento da riga di comando che include IntelliSense, l'evidenziazione della sintassi e altro ancora. | **Windows</br>macOS</br>Linux** |
|[**mssql-conf**](../linux/sql-server-linux-configure-mssql-conf.md) | **mssql-conf** consente di configurare SQL Server eseguito in Linux. | **Linux** |
|[**mssql-scripter (anteprima)**](https://github.com/Microsoft/mssql-scripter) | **mssql-scripter** è un'esperienza da riga di comando multipiattaforma per la creazione di script per i database SQL Server. | **Windows</br>macOS</br>Linux** |
| [**sqlcmd**](sqlcmd-utility.md) |L'utilità **sqlcmd** consente di immettere istruzioni Transact-SQL, procedure di sistema e file di script al prompt dei comandi. | **Windows</br>macOS</br>Linux** |
| [**sqlpackage**](sqlpackage/sqlpackage.md) |**sqlpackage.exe** è un'utilità della riga di comando che automatizza diverse attività di sviluppo di database. |**Windows</br>macOS</br>Linux** |
|[**SQL Server PowerShell**](../powershell/sql-server-powershell.md)| **SQL Server PowerShell** offre dei cmdlet per l'utilizzo di SQL. | **Windows</br>macOS</br>Linux** |

## <a name="migration-and-other-tools"></a>Migrazione e altri strumenti

Questi strumenti vengono usati per la migrazione, la configurazione e la disponibilità di altre funzionalità per i database SQL.

| Strumento | Descrizione |
|:--|:--|
| **[Configuration Manager](../tools/configuration-manager/sql-server-configuration-manager-help.md)** | Usare SQL Server Configuration Manager per configurare i servizi SQL Server e configurare la connettività di rete. Configuration Manager viene eseguito in Windows|
| **[Database Experimentation Assistant](../dea/database-experimentation-assistant-overview.md)** | Usare Database Experimentation Assistant per valutare una versione di destinazione di SQL per un determinato carico di lavoro. |
| **[Data Migration Assistant](../dma/dma-overview.md)** | Lo strumento Data Migration Assistant consente di eseguire l'aggiornamento a una piattaforma dati moderna individuando i problemi di compatibilità che possono influire sulle funzionalità del database nella nuova versione di SQL Server o in Database SQL di Azure. |
| **[Riesecuzione distribuita](../tools/distributed-replay/install-distributed-replay-overview.md)** | Usare la funzionalità Riesecuzione distribuita per agevolare la valutazione dell'impatto dei futuri aggiornamenti di SQL Server. È possibile usare Riesecuzione distribuita anche per valutare l'impatto degli aggiornamenti hardware e del sistema operativo e dell'ottimizzazione di SQL Server. |
| **[ssbdiagnose](../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)** | L'utilità ssbdiagnose segnala la presenza di problemi nelle conversazioni di Service Broker o nella configurazione dei servizi di Service Broker. |
| **[SQL Server Migration Assistant](../ssma/sql-server-migration-assistant.md)** | Usare SQL Server Migration Assistant per automatizzare la migrazione di database a SQL Server da Microsoft Access, DB2, MySQL, Oracle e Sybase.|

Per informazioni su ulteriori strumenti non citati in questa pagina, vedere [Utilità del prompt dei comandi SQL](command-prompt-utility-reference-database-engine.md) e [Scaricare le funzionalità estese e gli strumenti di SQL Server](download-sql-feature-packs.md)
