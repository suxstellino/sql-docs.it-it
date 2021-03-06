---
title: Valutazione di un'azienda e consolidamento dei report di valutazione con Data Migration Assistant
description: Informazioni su come usare DMA per valutare un'azienda e consolidare i report di valutazione prima di aggiornare SQL Server o eseguire la migrazione al database SQL di Azure.
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: c82b5cf03b5db429903dd90fdf8a5fa5d6ec3772
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247510"
---
# <a name="assess-an-enterprise-and-consolidate-assessment-reports-with-dma"></a>Valutare un'azienda e consolidare i report di valutazione con DMA

Le istruzioni dettagliate riportate di seguito consentono di usare la Data Migration Assistant per eseguire una valutazione con scalabilità corretta per l'aggiornamento SQL Server locale o SQL Server in esecuzione in macchine virtuali di Azure o per la migrazione al database SQL di Azure.

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="prerequisites"></a>Prerequisiti

- Designare un computer degli strumenti nella rete da cui verrà avviato il DMA. Verificare che il computer disponga della connettività alle destinazioni SQL Server.
- Scaricare e installare:
  - [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v 3.6 o versioni successive.
  - [PowerShell](https://aka.ms/wmf5download) v 5.0 o versioni successive.
  - [.NET Framework](https://www.microsoft.com/download/details.aspx?id=30653) versione 4.5 o successiva.
  - [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 17,0 o versione successiva.
  - [Power bi desktop](/power-bi/fundamentals/desktop-get-the-desktop).
  - [Moduli di Azure PowerShell](/powershell/azure/install-az-ps)
- Scaricare ed estrarre:
  - Il [modello di report DMA Power bi](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/4/PowerBI-Reports.zip).
  - [Script LoadWarehouse](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/3/LoadWarehouse1.zip).

## <a name="loading-the-powershell-modules"></a>Caricamento dei moduli di PowerShell

Il salvataggio dei moduli di PowerShell nella directory dei moduli di PowerShell consente di chiamare i moduli senza la necessità di caricarli in modo esplicito prima dell'uso.

Per caricare i moduli, seguire questa procedura:

1. Passare a C:\Program Files\WindowsPowerShell\Modules, quindi creare una cartella denominata **DataMigrationAssistant**.
2. Aprire i [moduli di PowerShell](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/1/PowerShell-Modules2.zip)e salvarli nella cartella creata.

      ![Moduli di PowerShell](../dma/media//dma-consolidatereports/dma-powershell-modules.png)

    Ogni cartella contiene il file psm1 associato, come illustrato nell'immagine seguente:

   ![File psm1 dei moduli di PowerShell](../dma/media//dma-consolidatereports/dma-powershell-modules-psm1-files.png)

   > [!NOTE]
   > La cartella e il file psm1 che contiene devono avere lo stesso nome.

   > [!IMPORTANT]
   > Potrebbe essere necessario sbloccare i file di PowerShell dopo averli salvati nella directory WindowsPowerShell per assicurarsi che i moduli vengano caricati correttamente. Per sbloccare un file di PowerShell, fare clic con il pulsante destro del mouse sul file, scegliere **Proprietà**, selezionare la casella di testo **Sblocca** e quindi fare clic su **OK**.

   ![Proprietà del file psm1](../dma/media//dma-consolidatereports/dma-psm1-file-properties.png)

    A questo punto PowerShell caricherà questi moduli automaticamente all'avvio di una nuova sessione di PowerShell.

## <a name="create-an-inventory-of-sql-servers"></a><a name="create-inventory"></a> Creazione di un inventario di SQL Server

Prima di eseguire lo script di PowerShell per valutare i server SQL, è necessario compilare un inventario dei server SQL che si desidera valutare.

Questo inventario può trovarsi in uno dei due formati seguenti:

- File CSV di Excel
- Tabella di SQL Server

### <a name="if-using-a-csv-file"></a>Se si usa un file CSV

> [!IMPORTANT]
> Verificare che il file di inventario venga salvato come file con valori delimitati da virgole (CSV).
>
> Per le istanze predefinite, impostare il nome dell'istanza su MSSQLServer.

Quando si usa un file CSV per importare i dati, verificare che siano presenti solo due colonne di **nome dell'istanza** di dati e il **nome del database** e che nelle colonne non siano presenti righe di intestazione.

 ![contenuto del file CSV](../dma/media//dma-consolidatereports/dma-csv-file-contents.png)

### <a name="if-using-a-sql-server-table"></a>Se si usa una tabella SQL Server

> [!IMPORTANT]
> Per le istanze predefinite, impostare il nome dell'istanza su MSSQLServer.

Creare un database denominato **EstateInventory** e una tabella denominata **DatabaseInventory**. La tabella che contiene i dati di inventario può includere un numero qualsiasi di colonne, purché siano presenti quattro colonne seguenti:

- ServerName
- InstanceName
- DatabaseName
- AssessmentFlag

![Screenshot di un SQL Server contenuto della tabella se si utilizza una tabella SQL Server.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-database-inventory.png)

Se il database non è presente nel computer degli strumenti, verificare che il computer degli strumenti disponga della connettività di rete a questa istanza di SQL Server.

Il vantaggio derivante dall'utilizzo di una tabella di SQL Server su un file CSV consiste nel fatto che è possibile utilizzare la colonna flag di valutazione per controllare l'istanza o il database che viene prelevato per la valutazione, semplificando così la separazione delle valutazioni in blocchi più piccoli.  È quindi possibile estendere più valutazioni. vedere la sezione relativa all'esecuzione di una valutazione più avanti in questo articolo, che è più semplice rispetto alla gestione di più file CSV.

Tenere presente che, a seconda del numero di oggetti e della relativa complessità, una valutazione può richiedere un tempo eccezionalmente lungo (ore +), quindi è consigliabile separare la valutazione in blocchi gestibili.

### <a name="if-using-an-instance-inventory"></a>Se si utilizza un inventario dell'istanza

Creare un database denominato **EstateInventory** e una tabella denominata **InstanceInventory**. La tabella che contiene i dati di inventario può includere un numero qualsiasi di colonne, purché siano presenti quattro colonne seguenti:

- ServerName
- InstanceName
- Porta
- AssessmentFlag

![Screenshot di un SQL Server contenuto della tabella se si utilizza un inventario dell'istanza.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-instance-inventory.png)

## <a name="running-a-scaled-assessment"></a>Esecuzione di una valutazione ridimensionata

Dopo aver caricato i moduli di PowerShell nella directory Modules e aver creato un inventario, è necessario eseguire una valutazione con scalabilità aprendo PowerShell ed eseguendo la funzione dmaDataCollector.

  ![elenchi di funzioni dmaDataCollector](../dma/media//dma-consolidatereports/dma-dmaDataCollector-function-listing.png)

I parametri associati alla funzione dmaDataCollector sono descritti nella tabella seguente.

|Parametro  |Descrizione |
|---------|---------|
|**getServerListFrom** | L'inventario. I valori possibili sono **SqlServer** e **CSV**.<br/>Per altre informazioni, vedere [creare un inventario di SQL Server](#create-inventory). |
|**csvPath** | Percorso del file di inventario CSV.  Utilizzato solo quando **getServerListFrom** è impostato su  **CSV**. |
|**serverName** | Nome dell'istanza di SQL Server dell'inventario quando si utilizza **SqlServer** nel parametro **getServerListFrom** . |
|**databaseName** | Database che ospita la tabella di inventario. |
|**useInstancesOnly** | Flag di bit per specificare se utilizzare o meno un elenco di istanze per la valutazione.  Se impostato su 0, la tabella DatabaseInventory verrà utilizzata per compilare l'elenco di destinazione della valutazione. |
|**AssessmentName** | Nome della valutazione DMA. |
|**TargetPlatform** | Tipo di destinazione della valutazione che si desidera eseguire.  I valori possibili sono **AzureSQLDatabase**, **ManagedSqlServer**, **SQLServer2012**, **SQLServer2014**, **SQLServer2016**, **SQLServerLinux2017**, **SQLServerWindows2017**,  **SqlServerWindows2019** e **SqlServerLinux2019**.  |
|**AuthenticationMethod** | Metodo di autenticazione per la connessione alle destinazioni SQL Server che si desidera valutare. I valori possibili sono **SQLAuth** e **WindowsAuth**. |
|**OutputLocation** | Directory in cui archiviare il file di output di valutazione JSON. A seconda del numero di database da valutare e del numero di oggetti all'interno dei database, le valutazioni possono richiedere un tempo eccezionalmente lungo. Il file verrà scritto al termine di tutte le valutazioni. |

Se si verifica un errore imprevisto, la finestra di comando che viene avviata da questo processo verrà terminata.  Esaminare il log degli errori per determinare il motivo per cui non è riuscito.

  ![Percorso log degli errori](../dma/media//dma-consolidatereports/dma-error-log-file-location.png)

## <a name="consuming-the-assessment-json-file"></a>Utilizzo del file JSON di valutazione

Al termine della valutazione, è ora possibile importare i dati in SQL Server per l'analisi. Per utilizzare il file JSON di valutazione, aprire PowerShell ed eseguire la funzione dmaProcessor.

  ![elenco delle funzioni dmaProcessor](../dma/media//dma-consolidatereports/dma-dmaProcessor-function-listing.png)

I parametri associati alla funzione dmaProcessor sono descritti nella tabella seguente.

|Parametro  |Descrizione |
|---------|---------|
|**processTo** | Percorso in cui verrà elaborato il file JSON. I valori possibili sono **SqlServer** e **AzureSQLDatabase**. |
|**serverName** | Istanza SQL Server in cui verranno elaborati i dati.  Se si specifica **AzureSQLDatabase** per il parametro **processTo** , includere solo il nome del SQL Server (non includere. database.Windows.NET). Quando la destinazione è il database SQL di Azure, verranno richiesti due account di accesso. il primo è quello delle credenziali del tenant di Azure, mentre il secondo è l'account di accesso amministratore per il SQL Server di Azure. |
|**CreateDMAReporting** | Database di gestione temporanea da creare per l'elaborazione del file JSON.  Se il database specificato esiste già e si imposta questo parametro su uno, gli oggetti non vengono creati.  Questo parametro è utile per ricreare un singolo oggetto che è stato eliminato. |
|**CreateDataWarehouse** | Crea la data warehouse che verrà utilizzata dal report Power BI. |
|**databaseName** | Nome del database DMAReporting. |
|**data warehouse** | Il nome del database del data warehouse. |
|**jsonDirectory** | Directory che contiene il file di valutazione JSON.  Se nella directory sono presenti più file JSON, questi vengono elaborati uno alla volta. |

La funzione dmaProcessor deve richiedere solo alcuni secondi per elaborare un singolo file.

## <a name="loading-the-data-warehouse"></a>Caricamento della data warehouse

Al termine dell'elaborazione dei file di valutazione da parte di dmaProcessor, i dati verranno caricati nel database DMAReporting nella tabella ReportData. A questo punto, è necessario caricare il data warehouse.

1. Utilizzare lo script LoadWarehouse per popolare i valori mancanti nelle dimensioni.

    Lo script prenderà i dati dalla tabella ReportData nel database DMAReporting e li caricherà nel warehouse.  Se durante questo processo di caricamento sono presenti errori, è probabile che si verifichino voci mancanti nelle tabelle delle dimensioni.

2. Caricare il data warehouse.

  ![Contenuto LoadWarehouse caricato](../dma/media//dma-consolidatereports/dma-load-warehouse-loaded.png)

## <a name="set-your-database-owners"></a>Impostare i proprietari del database

Sebbene non sia obbligatorio, per ottenere il massimo valore dai report, è consigliabile impostare i proprietari del database nella dimensione **dimDBOwner** , quindi aggiornare **DBOwnerKey** nella tabella **FactAssessment** .  Il processo che segue consente di sezionare e filtrare il report Power BI in base a proprietari di database specifici.

È inoltre possibile utilizzare lo script LoadWarehouse per fornire le istruzioni TSQL di base per l'impostazione dei proprietari del database.

  ![Proprietari delle impostazioni LoadWarehouse](../dma/media//dma-consolidatereports/dma-load-warehouse-set-owners.png)

## <a name="dma-reports"></a>Report DMA

1. Aprire il modello di report DMA Power BI nel Power BI Desktop.
2. Immettere i dettagli del server che puntano al database di **DMAWarehouse** e quindi selezionare **carica**.

   ![Report DMA Power BI modello caricato](../dma/media//dma-consolidatereports/dma-reports-powerbi-template-loaded.png)

   Una volta che il report ha aggiornato i dati dal database **DMAWarehouse** , viene visualizzato un report simile al seguente.

   ![Visualizzazione report DMAWarehouse](../dma/media//dma-consolidatereports/dma-DMAWarehouse-report1.png)

   > [!TIP]
   > Se non vengono visualizzati i dati previsti, provare a modificare il segnalibro attivo.  Per ulteriori informazioni, vedere i dettagli nella sezione seguente.

## <a name="working-with-dma-reports"></a>Uso dei report DMA

Per lavorare con i report DMA, usare i segnalibri e i filtri dei dati per filtrare in base a:

- Tipi di valutazione (database SQL di Azure, Istanza gestita SQL di Azure, SQL Server) 
- Nome dell'istanza
- Nome database
- Nome team

Per accedere al pannello segnalibri e filtri, selezionare il segnalibro Filters (filtri) nella pagina principale del report:

![Segnalibri e filtri del report DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks-filters.png)

Selezionando il segnalibro filters viene abilitato il pannello seguente:

![Pannello visualizzazioni report DMA](../dma/media//dma-consolidatereports/dma-report-views-blade.png)

È possibile utilizzare i segnalibri per cambiare il contesto di Reporting tra:

- Valutazioni cloud del database SQL di Azure
- Valutazioni cloud di Azure SQL Istanza gestita
- Valutazioni locali

![Segnalibri visualizzazioni report DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks1.png)

Per nascondere il pannello filtri, fare clic sul pulsante indietro:

![Pulsante indietro visualizzazioni report DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks-back.png)

Nella parte inferiore sinistra della pagina del report è presente un prompt per indicare se un filtro è attualmente applicato a uno degli elementi seguenti:

- FactAssessment-NomeIstanza
- FactAssessment-DatabaseName
- dimDBOwner-DBOwner

![Filtro richiesta applicato](../dma/media//dma-consolidatereports/dma-filter-applied-prompt.png)

> [!NOTE]
> Se si esegue solo una valutazione del database SQL di Azure, vengono popolati solo i report cloud. Viceversa, se si esegue solo una valutazione locale, vengono popolati solo i report locali. Tuttavia, se si esegue una valutazione di Azure e una valutazione locale e quindi si caricano entrambe le valutazioni nel warehouse, è possibile spostarsi tra i report cloud e i report locali facendo clic con il pulsante destro del mouse sull'icona associata.

## <a name="reports-visuals"></a>Oggetti visivi dei report

Le sezioni seguenti illustrano i dettagli visualizzati nei report Power BI.

### <a name="readiness-"></a>Conformità

  ![Percentuale di conformità DMA](../dma/media//dma-consolidatereports/dma-readiness-percentage.png)

Questo oggetto visivo viene aggiornato in base al contesto di selezione (Everything, instance, database [multipli di]).

### <a name="readiness-count"></a>Conteggio conformità

  ![Conteggio conformità DMA](../dma/media//dma-consolidatereports/dma-readiness-count.png)

Questo oggetto visivo Mostra il numero di database pronti per la migrazione del numero di database non ancora pronti per la migrazione.

### <a name="readiness-bucket"></a>Bucket conformità

  ![Bucket conformità DMA](../dma/media//dma-consolidatereports/dma-readiness-bucket.png)

Questo oggetto visivo mostra una suddivisione dei database in base ai bucket di conformità seguenti:

- 100% PRONTO
- 75-99% PRONTO
- 50-75% PRONTO
- NON PRONTO

### <a name="issues-word-cloud"></a>Problemi di Word cloud

  ![Problemi DMA WordCloud](../dma/media//dma-consolidatereports/dma-issues-word-cloud.png)

Questo oggetto visivo Mostra i problemi attualmente in corso nel contesto di selezione (tutto, istanza, database [multipli di]). Più grande è la parola visualizzata sullo schermo, maggiore è il numero di problemi in tale categoria. Se si passa il puntatore del mouse su una parola, viene visualizzato il numero di problemi che si verificano nella categoria.

### <a name="database-readiness"></a>Conformità database

  ![Report di conformità del database DMA](../dma/media//dma-consolidatereports/dma-database-readiness-report.png)

Questa sezione è la parte principale del report, che mostra la conformità di un database di istanza. Questo report presenta una gerarchia di drill-down di:

- InstanceDatabase
- ChangeCategory
- Titolo
- ObjectType
- ImpactedObjectName

 ![Drill-down del report conformità database DMA](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown.png)

Questo report funge anche da punto di filtro per la creazione del rapporto del piano di correzione.

Per esaminare il rapporto del piano di correzione, fare clic con il pulsante destro del mouse su un punto dati nel grafico, scegliere **drill-through**, quindi selezionare **piani di correzione**.

Questa attività filtra il rapporto del piano di monitoraggio e aggiornamento al livello della gerarchia corrente in base al punto in cui si seleziona l'opzione drill-through.

  ![Drill-down del report di conformità del database DMA filtrato](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown-filtered.png)

  ![Report piano di correzione DMA](../dma/media//dma-consolidatereports/dma-remediation-plan-report.png)

È anche possibile usare il report del piano di monitoraggio e aggiornamento per creare un piano di correzione personalizzato usando i filtri nel pannello **filtri visualizzazioni** .

  ![Opzioni di filtro del rapporto del piano di monitoraggio e aggiornamento DMA](../dma/media//dma-consolidatereports/dma-remediation-plan-report-filter-options.png)

### <a name="script-disclaimer"></a>Dichiarazione di non responsabilità

*Gli script di esempio forniti in questo articolo non sono supportati in alcun programma o servizio di supporto standard Microsoft. Tutti gli script vengono forniti così come sono senza garanzia di alcun tipo. Microsoft declina inoltre tutte le garanzie implicite, tra cui, senza limitazioni, eventuali garanzie implicite di commerciabilità o di idoneità per uno scopo specifico. L'intero rischio derivante dall'utilizzo o dalle prestazioni degli script e della documentazione di esempio rimane inalterato. In nessun caso Microsoft, gli autori o altri utenti responsabili della creazione, la produzione o la consegna degli script è soggetta a eventuali danni (inclusi, a titolo esemplificativo, danni per la perdita di profitti aziendali, interruzioni aziendali, perdita di informazioni aziendali o altre perdite pecuniarie) derivanti dall'utilizzo o dall'impossibilità di utilizzare gli script o la documentazione di esempio, anche se Microsoft è stata avvisata della possibilità di tali danni.  Richiedere l'autorizzazione prima di reinserire questi script in altri siti, repository/Blog.*