---
title: Distribuire ed eseguire i pacchetti SSIS in Azure | Microsoft Docs
description: Informazioni su come spostare progetti, pacchetti e carichi di lavoro di SQL Server Integration Services (SSIS) nel cloud di Microsoft Azure.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 12b4e6c71698845314844e4cf0bcd5b371209641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352006"
---
# <a name="lift-and-shift-sql-server-integration-services-workloads-to-the-cloud"></a>Migrazione lift-and-shift dei carichi di lavoro di SQL Server Integration Services nel cloud

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


È ora possibile spostare progetti, pacchetti e carichi di lavoro di SQL Server Integration Services (SSIS) nel cloud di Azure. Distribuire, eseguire e gestire progetti e pacchetti SSIS nel catalogo SSIS (SSISDB) per il database SQL di Azure o Istanza gestita di SQL con strumenti noti come SQL Server Management Studio (SSMS).

## <a name="benefits"></a>Vantaggi
Spostare i carichi di lavoro SSIS locali in Azure presenta i potenziali vantaggi seguenti:
-   **Riduzione dei costi operativi** e riduzione delle attività di gestione dell'infrastruttura svolte quando si esegue SSIS in locale o in macchine virtuali di Azure.
-   **Aumento della disponibilità elevata** con la possibilità di specificare più nodi per cluster, nonché le funzionalità di disponibilità elevata di Azure e del database SQL di Azure.
-   **Aumento della scalabilità** con la possibilità di specificare più core per nodo (riduzione del numero di istanze) e più nodi per cluster (aumento del numero di istanze).

## <a name="architecture-of-ssis-on-azure"></a>Architettura di SSIS in Azure
Nella tabella seguente vengono evidenziate le differenze tra SSIS in locale e SSIS in Azure.

La differenza più significativa è la separazione dell'archiviazione dal runtime. Azure Data Factory ospita il motore di runtime per i pacchetti SSIS in Azure. Il motore di runtime è definito Runtime di integrazione Azure-SSIS (Azure-SSIS IR). Per altre informazioni, vedere [Runtime di integrazione Azure-SSIS](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime).

| Location | Archiviazione | Runtime | Scalabilità |
|---|---|---|---|
| Locale | SQL Server | Runtime SSIS ospitato da SQL Server | SQL Server Integration Services Scale Out (in SQL Server 2017 e versioni successive)<br/><br/>Soluzioni personalizzate (nelle versioni precedenti di SQL Server) |
| In Azure | Database SQL o Istanza gestita di SQL | Runtime di integrazione Azure-SSIS, un componente di Azure Data Factory | Opzioni di scalabilità per il runtime di integrazione Azure-SSIS |
| | | | |

## <a name="provision-ssis-on-azure"></a>Provisioning di SSIS in Azure

**Provisioning**. Prima di distribuire ed eseguire i pacchetti SSIS in Azure, è necessario eseguire il provisioning del catalogo SSIS (SSISDB) e del runtime di integrazione Azure-SSIS.

-   Per effettuare il provisioning di SSIS in Azure nel portale di Azure, seguire la procedura di provisioning descritta in questo articolo: [Effettuare il provisioning di Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure). 

-   Per effettuare il provisioning di SSIS in Azure in PowerShell, seguire la procedura di provisioning descritta in questo articolo: [Effettuare il provisioning di Azure-SSIS Integration Runtime in Azure Data Factory con PowerShell](/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

È sufficiente eseguire il provisioning del runtime di integrazione Azure-SSIS una sola volta. Successivamente, è possibile usare strumenti familiari, ad esempio SQL Server Data Tools (SSDT) e SQL Server Management Studio (SSMS), per distribuire, configurare, eseguire, monitorare, pianificare e gestire i pacchetti.

> [!NOTE]
> Il runtime di integrazione Azure-SSIS non è ancora disponibile in tutte le aree di Azure. Per informazioni sulle aree supportate, vedere i [prodotti Microsoft Azure disponibili in base all'area geografica](https://azure.microsoft.com/regions/services/).

**Scalabilità verticale e orizzontale**. Quando si esegue il provisioning del runtime di integrazione Azure-SSIS, è possibile aumentare e ridurre il numero di istanze specificando i valori per le opzioni seguenti:
-   La dimensione del nodo, incluso il numero di core, e il numero di nodi del cluster.
-   L'istanza esistente del database SQL di Azure per ospitare il database del catalogo SSIS (SSISDB) e il livello di servizio per il database.
-   Le esecuzioni parallele massime per ogni nodo.

**Miglioramento delle prestazioni**. Per altre informazioni, vedere [Configurare il runtime di integrazione Azure-SSIS per garantire prestazioni elevate](/azure/data-factory/configure-azure-ssis-integration-runtime-performance).

**Riduzione dei costi**. Per ridurre i costi, eseguire il runtime di integrazione Azure-SSIS solo quando necessario. Per altre informazioni, vedere [Come avviare e arrestare il runtime di integrazione Azure SSIS in base a una pianificazione](/azure/data-factory/how-to-schedule-azure-ssis-integration-runtime).

## <a name="design-packages"></a>Progettare pacchetti

Continuare a **progettare e compilare i pacchetti** in locale in SSDT o in Visual Studio con SSDT installato.

### <a name="connect-to-data-sources"></a>Connettersi alle origini dati

Per connettersi alle origini dati locali dal cloud con l'**autenticazione di Windows**, vedere [Connettersi a origini dati e condivisioni file con l'autenticazione di Windows in pacchetti SSIS in Azure](/azure/data-factory/ssis-azure-connect-with-windows-auth).

Per connettersi a file e condivisioni file, vedere [Aprire e salvare file in locale e in Azure con pacchetti SSIS distribuiti in Azure](/azure/data-factory/ssis-azure-files-file-shares).

### <a name="available-ssis-components"></a>Componenti SSIS disponibili

Quando si effettua il provisioning di un'istanza del database SQL per ospitare SSISDB, vengono installati anche il Feature Pack di Azure per SSIS e Access Redistributable. Questi componenti offrono la connettività a varie origini dati di **Azure** e ai file di **Excel e Access**, oltre che alle origini dati supportate dai componenti predefiniti.

È anche possibile installare componenti aggiuntivi, ad esempio un driver non installato per impostazione predefinita. Per altre informazioni, vedere [Personalizzare l'installazione del runtime di integrazione Azure-SSIS](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Se si ha una licenza Enterprise Edition, sono disponibili componenti aggiuntivi. Per altre informazioni, vedere [Provisioning della Enterprise Edition per il Runtime di integrazione Azure-SSIS](/azure/data-factory/how-to-configure-azure-ssis-ir-enterprise-edition).

Se l'utente è un ISV può aggiornare l'installazione dei componenti concessi in licenza per renderli disponibili in Azure. Per altre informazioni, vedere [Installare componenti personalizzati a pagamento o concessi in licenza per il runtime di integrazione Azure-SSIS](/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components).

### <a name="transaction-support"></a>Supporto delle transazioni

Con SQL Server in locale e nelle macchine virtuali Azure, è possibile usare le transazioni Microsoft Distributed Transaction Coordinator (MSDTC). Per configurare MSDTC in ogni nodo del runtime di integrazione Azure-SSIS usare la funzionalità di installazione personalizzata. Per altre informazioni, vedere [Installazione personalizzata per il runtime di integrazione Azure-SSIS](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Con il database SQL di Azure è possibile usare solo le transazioni elastiche. Per altre informazioni, vedere [Transazioni distribuite in database cloud](/azure/sql-database/sql-database-elastic-transactions-overview).

## <a name="deploy-and-run-packages"></a>Distribuire ed eseguire i pacchetti

Per iniziare, vedere [Esercitazione: Distribuire ed eseguire un pacchetto di SQL Server Integration Services (SSIS) in Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="prerequisites"></a>Prerequisiti

Per distribuire pacchetti SSIS in Azure è necessario avere una delle versioni seguenti di SQL Server Data Tools (SSDT):
-   Per Visual Studio 2017, versione 15.3 o successiva.
-   Per Visual Studio 2015, versione 17.2 o successiva.

### <a name="connect-to-ssisdb"></a>Connettersi a SSISDB

Il **nome del database SQL** che ospita SSISDB diventa la prima parte del nome in quattro parti da usare quando si distribuiscono ed eseguono pacchetti da SSDT e SSMS, con il formato seguente: `<sql_database_name>.database.windows.net`. Per altre informazioni su come connettersi al database del catalogo SSIS in Azure, vedere [Connettersi al catalogo SSIS (SSISDB) in Azure](ssis-azure-connect-to-catalog-database.md).

### <a name="deploy-projects-and-packages"></a>Distribuire progetti e pacchetti

Per la distribuzione di progetti a SSISDB in Azure è necessario usare il **modello di distribuzione dei progetti** e non il modello di distribuzione dei pacchetti.

Per distribuire progetti in Azure è possibile usare diversi strumenti e opzioni di scripting comuni:
-   SQL Server Management Studio (SSMS)
-   Transact-SQL (da SSMS, Visual Studio Code o un altro strumento)
-   Uno strumento da riga di comando
-   PowerShell o C# e il modello a oggetti di gestione di SSIS

Il processo di distribuzione convalida i pacchetti, per verificare che siano eseguibili nel runtime di integrazione SSIS di Azure. Per altre informazioni, vedere [Convalidare i pacchetti SQL Server Integration Services (SSIS) distribuiti in Azure](ssis-azure-validate-packages.md).

Per un esempio di distribuzione in cui si usano SSMS e la Distribuzione guidata Integration Services, vedere [Esercitazione: Distribuire ed eseguire un pacchetto di SQL Server Integration Services (SSIS) in Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="version-support"></a>Supporto versione

È possibile distribuire in Azure un pacchetto creato con qualsiasi versione di SSIS. Quando si distribuisce un pacchetto in Azure, se non si verificano errori di convalida il pacchetto viene aggiornato automaticamente al formato più recente. In altri termini il pacchetto viene sempre aggiornato alla versione più recente di SSIS.

### <a name="run-packages"></a>Eseguire i pacchetti

Per eseguire pacchetti SSIS distribuiti in Azure, è possibile usare diversi metodi. Per altre informazioni, vedere [Eseguire i pacchetti SQL Server Integration Services (SSIS) distribuiti in Azure](ssis-azure-run-packages.md).

### <a name="run-packages-in-an-azure-data-factory-pipeline"></a>Eseguire i pacchetti in una pipeline di Azure Data Factory

Per eseguire un pacchetto SSIS in una pipeline di Azure Data Factory, usare l'attività Esegui pacchetto SSIS. Per altre informazioni, vedere [Eseguire un pacchetto SSIS tramite l'attività Esegui pacchetto SSIS in Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Quando si esegue un pacchetto in una pipeline di Data Factory con l'attività Esegui pacchetto SSIS, è possibile passare valori al pacchetto in fase di esecuzione. Per passare uno o più valori di runtime, creare ambienti di esecuzione SSIS in SSISDB con SQL Server Management Studio (SSMS). In ogni ambiente creare variabili e assegnare valori corrispondenti ai parametri dei progetti o dei pacchetti. Configurare i pacchetti SSIS in SSMS in modo da associare le variabili di ambiente ai parametri del progetto o del pacchetto. Quando si eseguono i pacchetti nella pipeline, passare da un ambiente all'altro specificando percorsi di ambiente diversi nella scheda Impostazioni dell'interfaccia utente dell'attività Esegui pacchetto SSIS. Per altre informazioni sugli ambienti SSIS, vedere [Creare ed eseguire il mapping di un ambiente server](../packages/deploy-integration-services-ssis-projects-and-packages.md#create-and-map-a-server-environment).

## <a name="monitor-packages"></a>Monitorare i pacchetti

Per monitorare i pacchetti in esecuzione, usare le opzioni per la creazione di report seguenti di SSMS.
-   Fare clic con il pulsante destro del mouse su **SSISDB**, quindi selezionare **Operazioni attive** per aprire la finestra di dialogo **Operazioni attive**.
-   Selezionare un pacchetto in Esplora oggetti, fare clic con il pulsante destro del mouse e selezionare **Report**, quindi **Report standard** e infine **Tutte le esecuzioni**.

Per monitorare il runtime di integrazione Azure-SSIS, vedere [Monitorare il runtime di integrazione Azure-SSIS](/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

## <a name="schedule-packages"></a>Pianificare i pacchetti
Per pianificare l'esecuzione dei pacchetti distribuiti in Azure, è possibile usare vari strumenti. Per altre informazioni, vedere [Pianificare l'esecuzione dei pacchetti di SQL Server Integration Services (SSIS) distribuiti in Azure](ssis-azure-schedule-packages.md).

## <a name="next-steps"></a>Passaggi successivi
Per iniziare con carichi di lavoro SSIS in Azure, vedere gli articoli seguenti:
-   [Esercitazione: Distribuire ed eseguire un pacchetto di SQL Server Integration Services (SSIS) in Azure](ssis-azure-deploy-run-monitor-tutorial.md)
-   [Eseguire il provisioning del runtime di integrazione SSIS di Azure in Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure)