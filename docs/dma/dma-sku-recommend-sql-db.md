---
title: Identificare lo SKU del database SQL di Azure appropriato per il database locale (Data Migration Assistant) | Microsoft Docs
description: Informazioni su come usare Data Migration Assistant per identificare lo SKU del database SQL di Azure appropriato per il database locale
ms.custom: ''
ms.date: 05/06/2019
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
ms.openlocfilehash: ffb47ee457a03315821fee863816d725f9691ac3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062586"
---
# <a name="identify-the-right-azure-sql-databasemanaged-instance-sku-for-your-on-premises-database"></a>Identificare il database SQL di Azure o lo SKU Istanza gestita appropriato per il database locale

La migrazione dei database nel cloud può essere complessa, soprattutto quando si cerca di selezionare la migliore destinazione di database di Azure e lo SKU per il database. Il nostro obiettivo con il database Migration Assistant (DMA) è aiutare a rispondere a queste domande e rendere più semplice l'esperienza di migrazione del database fornendo queste raccomandazioni per gli SKU in un output intuitivo.

Questo articolo è incentrato sulla funzionalità di raccomandazioni dello SKU del database SQL di Azure DMA. Il database SQL di Azure e Azure SQL Istanza gestita hanno diverse opzioni di distribuzione, tra cui:

- Database singolo
- Pool elastici
- database SQL

La funzionalità consigli SKU consente di identificare sia il database singolo del database SQL di Azure, sia lo SKU di SQL Istanza gestita di Azure in base ai contatori delle prestazioni raccolti dai computer che ospitano i database. La funzionalità fornisce consigli relativi al piano tariffario, al livello di calcolo e alle dimensioni massime dei dati, oltre ai costi stimati al mese. Offre inoltre la possibilità di eseguire il provisioning bulk di database singoli e istanze gestite per tutti i database consigliati.

> [!NOTE]
> Questa funzionalità è attualmente disponibile solo tramite l'interfaccia della riga di comando (CLI).

Di seguito sono riportate le istruzioni che consentono di determinare le raccomandazioni dello SKU e di effettuare il provisioning di singoli database o istanze gestite in Azure tramite DMA.

## <a name="prerequisites"></a>Prerequisiti

- Scaricare e installare la versione più recente di [DMA](https://aka.ms/get-dma). Se si dispone già di una versione precedente dello strumento, aprirla e verrà richiesto di aggiornare DMA.
- Verificare che il computer disponga di [PowerShell versione 5,1](https://www.microsoft.com/download/details.aspx?id=54616) o successiva, che è necessario per eseguire tutti gli script. Per informazioni su come scoprire quale versione di PowerShell è installata nel computer, vedere l'articolo [scaricare e installare Windows powershell 5,1](/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-5-1).
  > [!NOTE]
  > Per raccogliere informazioni sul computer, lo script di raccolta dati usa il cmdlet Get-WmiObject, che è stato deprecato in PowerShell 6. Per eseguire questo script in PowerShell 6 o 7, è necessario sostituire i cmdlet WMI con i cmdlet CIM più recenti.
- Verificare che nel computer sia installato il modulo Azure PowerShell. Per ulteriori informazioni, vedere l'articolo [installare il modulo Azure PowerShell](/powershell/azure/install-az-ps?view=azps-1.8.0&preserve-view=true).
- Verificare che il file di PowerShell **SkuRecommendationDataCollectionScript.ps1**, che è necessario per raccogliere i contatori delle prestazioni, sia installato nella cartella DMA.
- Verificare che il computer in cui si eseguirà questo processo disponga delle autorizzazioni di amministratore per il computer in cui sono ospitati i database di.

## <a name="collect-performance-counters"></a>Raccogliere i contatori delle prestazioni

Il primo passaggio del processo consiste nel raccogliere i contatori delle prestazioni per i database. È possibile raccogliere i contatori delle prestazioni eseguendo un comando di PowerShell nel computer che ospita i database. DMA fornisce una copia di questo file di PowerShell, ma è anche possibile usare il proprio metodo per acquisire i contatori delle prestazioni dal computer.

Non è necessario eseguire questa attività per ogni database singolarmente. I contatori delle prestazioni raccolti da un computer possono essere utilizzati per consigliare lo SKU per tutti i database ospitati nel computer.

1. Nella cartella DMA individuare il file di PowerShell SkuRecommendationDataCollectionScript.ps1. Questo file è necessario per raccogliere i contatori delle prestazioni.

    ![File di PowerShell visualizzato nella cartella DMA](../dma/media/dma-sku-recommend-data-collection-file.png)

2. Eseguire lo script di PowerShell con gli argomenti seguenti:
    - **ComputerName**: nome del computer che ospita i database.
    - **OutputFilePath**: percorso del file di output in cui salvare i contatori raccolti.
    - **CollectionTimeInSeconds**: periodo di tempo durante il quale si desidera raccogliere i dati dei contatori delle prestazioni. Acquisire i contatori delle prestazioni per almeno 40 minuti per ottenere una raccomandazione significativa. Maggiore è la durata dell'acquisizione, maggiore sarà l'accuratezza dell'indicazione. Assicurarsi inoltre che i carichi di lavoro siano in esecuzione per i database desiderati per abilitare raccomandazioni più accurate.
    - **DbConnectionString**: stringa di connessione che punta al database master ospitato nel computer da cui vengono raccolti i dati dei contatori delle prestazioni.

    Ecco una chiamata di esempio:

    ```
    .\SkuRecommendationDataCollectionScript.ps1
     -ComputerName Foobar1
     -OutputFilePath D:\counters2.csv
     -CollectionTimeInSeconds 2400
     -DbConnectionString Server=localhost;Initial Catalog=master;Integrated Security=SSPI;
    ```

    Dopo l'esecuzione del comando, il processo restituirà un file che include i contatori delle prestazioni nel percorso specificato. È possibile usare questo file come input per la parte successiva del processo, che fornirà raccomandazioni dello SKU per le opzioni di database singolo e istanze gestite.

## <a name="use-the-dma-cli-to-get-sku-recommendations"></a>Usare l'interfaccia della riga di comando DMA per ottenere le raccomandazioni dello SKU

Usare il file di output dei contatori delle prestazioni creato come input per questo processo.

Per l'opzione di database singolo, DMA fornirà consigli per il piano tariffario del database SQL di Azure, il livello di calcolo e le dimensioni massime dei dati per ogni database nel computer. Se si dispone di più database nel computer, è anche possibile specificare i database per i quali si desiderano le raccomandazioni. Il DMA fornirà inoltre il costo mensile stimato per ogni database.

Per istanza gestita, le raccomandazioni supportano uno scenario Lift-and-Shift. Di conseguenza, DMA fornirà consigli per il piano tariffario di SQL Istanza gestita di Azure, il livello di calcolo e le dimensioni massime dei dati per il set di database nel computer. Anche in questo caso, se si dispone di più database nel computer, è anche possibile specificare i database per i quali si desiderano le raccomandazioni. DMA fornirà inoltre il costo mensile stimato per l'istanza gestita.

Per usare l'interfaccia della riga di comando DMA per ottenere le raccomandazioni dello SKU, al prompt dei comandi eseguire dmacmd.exe con gli argomenti seguenti:

- **/Action = SkuRecommendation**: immettere questo argomento per eseguire valutazioni SKU.
- **/SkuRecommendationInputDataFilePath**: percorso del file del contatore raccolto nella sezione precedente.
- **/SkuRecommendationTsvOutputResultsFilePath**: percorso in cui scrivere i risultati dell'output in formato TSV.
- **/SkuRecommendationJsonOutputResultsFilePath**: percorso in cui scrivere i risultati dell'output in formato JSON.
- **/SkuRecommendationHtmlResultsFilePath**: percorso in cui scrivere i risultati dell'output in formato HTML.

Inoltre, selezionare uno degli argomenti seguenti:

- Impedisci aggiornamento prezzi
  - **/SkuRecommendationPreventPriceRefresh**: se impostato su true, impedisce l'aggiornamento del prezzo e presuppone i prezzi predefiniti. Usare se è in esecuzione in modalità offline. Se non si usa questo parametro, è necessario specificare i parametri seguenti per ottenere i prezzi più recenti in base a un'area specificata.
- Ottieni i prezzi più recenti
  - **/SkuRecommendationCurrencyCode**: valuta in cui visualizzare i prezzi, ad esempio "USD".
  - **/SkuRecommendationOfferName**: nome dell'offerta, ad esempio "MS-AZR-0003P". Per ulteriori informazioni, vedere la pagina dei [Dettagli dell'offerta Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) .
    - **/SkuRecommendationRegionName**: nome dell'area (ad esempio, "westus").
    - **/SkuRecommendationSubscriptionId**: ID sottoscrizione.
    - **/AzureAuthenticationTenantId**: tenant di autenticazione.
    - **/AzureAuthenticationClientId**: ID client dell'app AAD usata per l'autenticazione.
    - Una delle opzioni di autenticazione seguenti:
      - Interattività
        - **AzureAuthenticationInteractiveAuthentication**: impostare su true per una finestra popup di autenticazione.
      - Basato su certificati
        - **AzureAuthenticationCertificateStoreLocation**: impostare sul percorso dell'archivio certificati, ad esempio "CurrentUser".
        - **AzureAuthenticationCertificateThumbprint**: impostare sull'identificazione personale del certificato.
      - Basato su token
        - **AzureAuthenticationToken**: impostato sul token del certificato.

> [!NOTE]
> Per ottenere i ClientID e TenantId per l'autenticazione interattiva, è necessario configurare una nuova applicazione di AAD. Per altre informazioni sull'autenticazione e sull'acquisizione di queste credenziali, vedere l'articolo [Microsoft Azure esempi di codice dell'API di fatturazione: API tariffario](https://github.com/Azure-Samples/billing-dotnet-ratecard-api), seguire le istruzioni in **passaggio 1: configurare un'applicazione client nativa nel tenant di AAD**.

Infine, è disponibile un argomento facoltativo che è possibile usare per specificare i database per i quali si desiderano le raccomandazioni: 

- **/SkuRecommendationDatabasesToRecommend**: elenco di database per i quali eseguire le raccomandazioni. I nomi di database fanno distinzione tra maiuscole e minuscole e devono (1) essere trovati nel file di input. csv, (2) ciascuno racchiuso tra virgolette doppie e (3) ciascuno è separato da uno spazio singolo tra i nomi (ad esempio/SkuRecommendationDatabasesToRecommend = "Database1" "Database2" "Database3"). Se si omette questo parametro, assicurarsi che vengano fornite le indicazioni per tutti i database utente identificati nel file input. csv.  

Di seguito sono riportate alcune chiamate di esempio:

**Esempio 1: recupero di raccomandazioni con prezzi predefiniti. Usare quando si esegue in modalità offline o quando non si dispone delle credenziali di autenticazione.**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationPreventPriceRefresh=true
```

**Esempio 2: ottenere raccomandazioni con i prezzi più recenti per l'area specificata (ad esempio, "UKWest").**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationCurrencyCode=USD
/SkuRecommendationOfferName=MS-AZR-0044p
/SkuRecommendationRegionName=UKWest
/SkuRecommendationSubscriptionId=<Your Subscription Id>
/AzureAuthenticationInteractiveAuthentication=true
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId>
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

**Esempio 3: ottenere consigli per database specifici, ad esempio "TPCDS1G, EDW_3G, TPCDS10G".**

```
.\DmaCmd.exe /Action=SkuRecommendation 
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv" 
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv" 
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json" 
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html" 
/SkuRecommendationCurrencyCode=USD 
/SkuRecommendationOfferName=MS-AZR-0044p 
/SkuRecommendationRegionName=UKWest 
/SkuRecommendationSubscriptionId=<Your Subscription Id> 
/SkuRecommendationDatabasesToRecommend=“TPCDS1G” “EDW_3G” “TPCDS10G” 
/AzureAuthenticationInteractiveAuthentication=true 
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId> 
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

Per indicazioni su database singolo, il file di output TSV sarà simile al seguente:

![File PowerShell a database singolo visualizzato nella cartella DMA](../dma/media/dma-sku-recommend-single-db-recommendations.png)

Per indicazioni sulle istanze gestite, il file di output TSV sarà simile al seguente:

![File di istanza gestita di PowerShell visualizzato nella cartella DMA](../dma/media/dma-sku-recommend-mi-recommendations.png)

Segue una descrizione di ogni colonna nel file di output.

- **DatabaseName** : nome del database.
- **MetricType** : livello di prestazioni consigliato.
- **MetricValue** -SKU consigliato.
- **PricePerMonth** : Prezzo stimato al mese per lo SKU corrispondente.
- **RegionName** : nome dell'area per lo SKU corrispondente. 
- **IsTierRecommended** -viene apportata una raccomandazione per lo SKU minimo per ogni livello. Viene quindi applicata l'euristica per determinare il livello corretto per il database. Indica il livello consigliato per il database. 
- **ExclusionReasons** : questo valore è vuoto se si consiglia un livello. Per ogni livello non consigliato, vengono forniti i motivi per cui non è stata selezionata.
- **AppliedRules** : breve notazione delle regole applicate.

Il livello consigliato finale (ad esempio, **MetricType**) e il valore (ad esempio, **MetricValue**), trovato dove la colonna **IsTierRecommended** è true, riflette lo SKU minimo necessario per l'esecuzione delle query in Azure con una percentuale di successo simile ai database locali. Per il Istanza gestita SQL di Azure, DMA supporta attualmente le raccomandazioni per gli SKU 8vcore più comunemente usati per 40vcore. Se, ad esempio, lo SKU minimo consigliato è S4 per il livello standard, la scelta di S3 o di seguito provocherà il timeout o l'esecuzione delle query.

Il file HTML contiene queste informazioni in formato grafico. Fornisce un mezzo intuitivo per visualizzare la raccomandazione finale e il provisioning della parte successiva del processo. Altre informazioni sull'output HTML sono riportate nella sezione seguente.

## <a name="provision-recommended-skus-to-azure"></a>Eseguire il provisioning degli SKU consigliati in Azure

Con pochi clic, è possibile usare i consigli identificati per eseguire il provisioning di SKU di destinazione in Azure in cui è possibile eseguire la migrazione dei database. È possibile usare il file HTML per inserire la sottoscrizione di Azure; Selezionare il piano tariffario, il livello di calcolo e le dimensioni massime dei dati per i database. e generano uno script per eseguire il provisioning dei database. È possibile eseguire questo script usando PowerShell.

Questo processo può essere eseguito in un singolo computer oppure può essere eseguito su più computer per determinare le raccomandazioni dello SKU su larga scala. DMA attualmente rende un'esperienza semplice e scalabile supportando l'intero processo tramite l'interfaccia della riga di comando.

Per inserire le informazioni di provisioning e apportare modifiche alle raccomandazioni, aggiornare il file HTML come indicato di seguito.

**Per consigli su database singolo**

![Schermata raccomandazioni SKU del database SQL di Azure](../dma/media/dma-sku-recommend-single-db-recommendations1.png)

1. Aprire il file HTML e immettere le informazioni seguenti:
    - **ID sottoscrizione** : l'ID sottoscrizione della sottoscrizione di Azure a cui si vuole eseguire il provisioning dei database.
    - **Gruppo di risorse** : il gruppo di risorse in cui si desidera distribuire i database. Immettere un gruppo di risorse esistente.
    - **Region** (area): area in cui eseguire il provisioning dei database. Assicurarsi che la sottoscrizione supporti l'area di selezione.
    - **Nome del server** : il server di database SQL di Azure in cui si desidera distribuire i database. Se si immette un nome di server che non esiste, verrà creato.
    - **Nome utente amministratore** : nome utente amministratore del server. Verificare che il nome di accesso soddisfi i requisiti seguenti:
      - Il nome dell'account di accesso non deve contenere un identificatore SQL o un nome di sistema tipico, ad esempio admin, Administrator, SA, root, dbmanager, loginmanager e così via, oppure un utente o un ruolo predefinito del database, ad esempio dbo, Guest, Public e così via.
      - Il nome dell'account di accesso non deve includere caratteri non alfanumerici, inclusi gli spazi vuoti e i caratteri Unicode.
      - Il nome dell'account di accesso non deve iniziare con numeri o simboli.

    - **Password amministratore** : password amministratore server. 
      - La password deve avere una lunghezza di almeno 8 caratteri e non può contenere più di 128 caratteri.
      - La password deve contenere caratteri di tre delle categorie seguenti: lettere maiuscole, lettere minuscole, numeri (0-9) e caratteri non alfanumerici (!, $, #, % e così via).
      - La password non può contenere tutto o parte del nome dell'account di accesso. (Parte di un nome di account di accesso è definita come tre o più caratteri alfanumerici consecutivi).

2. Esaminare i consigli per ogni database e modificare il piano tariffario, il livello di calcolo e le dimensioni massime dei dati in base alle esigenze. Assicurarsi di deselezionare i database di cui non si vuole eseguire il provisioning.

3. Selezionare **Genera script di provisioning**, salvare lo script e quindi eseguirlo in PowerShell.

    Questo processo deve creare tutti i database selezionati nella pagina HTML.

**Per le raccomandazioni di Azure SQL Istanza gestita**

![Schermata delle raccomandazioni dello SKU di Azure SQL MI](../dma/media/dma-sku-recommend-mi-recommendations1.png)

1. Aprire il file HTML e immettere le informazioni seguenti:
    - **ID sottoscrizione** : l'ID sottoscrizione della sottoscrizione di Azure a cui si vuole eseguire il provisioning dei database.
    - **Gruppo di risorse** : il gruppo di risorse in cui si desidera distribuire i database. Immettere un gruppo di risorse esistente.
    - **Region** (area): area in cui eseguire il provisioning dei database. Assicurarsi che la sottoscrizione supporti l'area di selezione.
    - **Nome istanza** : istanza di Azure SQL istanza gestita cui si desidera eseguire la migrazione dei database. Il nome dell'istanza può contenere solo lettere minuscole, numeri è-', ma non può iniziare o terminare con '-' o contenere più di 63 caratteri.
    - **Nome utente amministratore istanza** : nome utente amministratore istanza. Verificare che il nome di accesso soddisfi i requisiti seguenti:
      - Il nome dell'account di accesso non deve contenere un identificatore SQL o un nome di sistema tipico, ad esempio admin, Administrator, SA, root, dbmanager, loginmanager e così via, oppure un utente o un ruolo predefinito del database, ad esempio dbo, Guest, Public e così via.
      - Il nome dell'account di accesso non deve includere caratteri non alfanumerici, inclusi gli spazi vuoti e i caratteri Unicode.
      - Il nome dell'account di accesso non deve iniziare con numeri o simboli.

    - **Password amministratore istanza** : password amministratore istanza. 
      - La password deve avere una lunghezza di almeno 16 caratteri e non può contenere più di 128 caratteri.
      - La password deve contenere caratteri di tre delle categorie seguenti: lettere maiuscole, lettere minuscole, numeri (0-9) e caratteri non alfanumerici (!, $, #, % e così via).
      - La password non può contenere tutto o parte del nome dell'account di accesso. (Parte di un nome di account di accesso è definita come tre o più caratteri alfanumerici consecutivi).

    - **Nome VNET** : il nome VNET in cui deve essere eseguito il provisioning dell'istanza gestita. Immettere un nome di VNet esistente.
    - **Nome subnet** : il nome della subnet in cui deve essere eseguito il provisioning dell'istanza gestita. Immettere un nome di subnet esistente.

2. Esaminare le raccomandazioni per ogni istanza e modificare il piano tariffario, il livello di calcolo e le dimensioni massime dei dati in base alle esigenze. Sebbene le raccomandazioni siano attualmente limitate agli SKU di 8vcore a 40vcore, è comunque possibile effettuare il provisioning degli SKU 64vcore e 80vcore, se lo si desidera. Assicurarsi di deselezionare le istanze di cui non si vuole eseguire il provisioning.

    Questo processo deve creare tutti i database selezionati nella pagina HTML.

    > [!NOTE]
    > La creazione di istanze gestite in una subnet (soprattutto per la prima volta) può richiedere diverse ore. Dopo aver eseguito lo script di provisioning tramite PowerShell, è possibile controllare lo stato della distribuzione nel portale di Azure.

## <a name="next-step"></a>Passaggio successivo

- Per un elenco completo dei comandi per l'esecuzione di DMA dall'interfaccia della riga di comando, vedere l'articolo [eseguire Data Migration Assistant dalla riga di comando](./dma-commandline.md).
