---
description: 'Lezione 1-2: Aggiungere e configurare una gestione connessione file flat'
title: 'Passaggio 2: Aggiungere e configurare una gestione connessione file flat | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9a77dd32-d8c2-4961-ad37-2a971f9d6043
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 57bf9bd3863e5486c890088324641a5100581a02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099225"
---
# <a name="lesson-1-2-add-and-configure-a-flat-file-connection-manager"></a>Lezione 1-2: Aggiungere e configurare una gestione connessione file flat

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In questa attività si aggiungerà una gestione connessione file flat al pacchetto appena creato. Una gestione connessione file flat abilita un pacchetto all'estrazione di dati da un file flat. Utilizzando tale gestione connessione è possibile specificare il nome file e la posizione, le impostazioni locali e la tabella codici e il formato del file, inclusi i delimitatori di colonna, da applicare quando il pacchetto estrae i dati dal file flat. È anche possibile specificare manualmente il tipo di dati per le singole colonne o usare la finestra di dialogo **Suggerisci tipo di colonne** per eseguire automaticamente il mapping delle colonne di dati estratti ai tipi di dati di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
È necessario creare una nuova gestione connessione file flat per ogni formato di file con cui si lavora. Dal momento che questa esercitazione esegue l'estrazione di dati da più file flat con lo stesso formato di dati, è necessario aggiungere e configurare una sola gestione connessione file flat al pacchetto di esempio.  
  
In questa esercitazione si configureranno le proprietà seguenti nella gestione connessione file flat:  
  
-   **Nomi di colonna:** dal momento che il file flat non presenta nomi di colonna, la gestione connessione file flat crea nomi di colonna predefiniti. Questi nomi predefiniti non sono utili per identificare i dati rappresentati da ogni colonna. Modificarli in nomi che corrispondano alla tabella dei fatti in cui i dati dei file flat devono essere caricati.  
  
-   **Mapping dei dati:** i mapping dei tipi di dati specificati per la gestione connessione file flat vengono usati da tutti i componenti di origine dati dei file flat che fanno riferimento alla gestione connessione. È possibile eseguire manualmente il mapping dei tipi di dati usando la gestione connessione file flat oppure usare la finestra di dialogo **Suggerisci tipi di colonne** . In questa attività vengono visualizzati i mapping suggeriti nella finestra di dialogo **Suggerisci tipi di colonne** e quindi vengono creati manualmente i mapping necessari nella finestra di dialogo **Editor gestione connessione file flat**.  
  
> [!NOTE]
> Gestione connessione file flat fornisce informazioni sulle impostazioni locali per il file di dati. Se il computer non è configurato per l'uso dell'opzione **Inglese (Stati Uniti)** delle impostazioni internazionali, è necessario impostare proprietà aggiuntive nella finestra di dialogo **Editor gestione connessione file flat**.  
  
## <a name="add-a-flat-file-connection-manager-to-the-ssis-package"></a>Aggiungere una gestione connessione file flat al pacchetto SSIS  
  
1.  Nel riquadro **Esplora soluzioni** fare clic con il pulsante destro del mouse su **Gestioni connessioni** e selezionare **Nuova gestione connessione**.
1. Nella finestra di dialogo **Aggiungi gestione connessione SSIS** selezionare **FLATFILE** e quindi **Aggiungi**.
  
2.  Nella finestra di dialogo **Editor gestione connessione file flat**, per **Nome gestione connessione** digitare **Sample Flat File Source Data**.  
  
3.  Selezionare **Sfoglia**.  
  
4.  Nella finestra di dialogo **Apri** individuare il file **SampleCurrencyData.txt** nel computer.  
  
5.  Deselezionare la casella di controllo **Nomi di colonne nella prima riga di dati**.  
  
### <a name="set-locale-sensitive-properties"></a>Impostare le proprietà dipendenti dalle impostazioni locali  
  
1.  Nella finestra di dialogo **Editor gestione connessione file flat** selezionare **Generale**.  
  
2.  Impostare **Impostazioni locali** su **Inglese (Stati Uniti)** e **Tabella codici** su **1252**.  
  
### <a name="rename-columns-in-the-flat-file-connection-manager"></a>Rinominare le colonne nella gestione connessione file flat  
  
1.  Nella finestra di dialogo **Editor gestione connessione file flat** selezionare **Avanzate**.  
  
2.  Nel riquadro delle proprietà apportare le seguenti modifiche:  
  
    -   Modificare la proprietà nome **Colonna 0** in **AverageRate**.  
  
    -   Modificare la proprietà nome **Colonna 1** in **CurrencyID**.  
  
    -   Modificare la proprietà nome **Colonna 2** in **CurrencyDate**.  
  
    -   Modificare la proprietà nome **Colonna 3** in **EndOfDayRate**.  
  
### <a name="remap-column-data-types"></a>Modificare il mapping dei tipi di dati delle colonne  
  
Per impostazione predefinita, le quattro colonne sono inizialmente impostate su un tipo di dati stringa [DT_STR] con un valore **OutputColumnWidth** di 50.  

1.  Nella finestra di dialogo **Editor gestione connessione file flat** selezionare **Suggerisci tipi**.  
  
    [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] indica automaticamente tipi di dati appropriati in base alle prime 200 righe di dati. È inoltre possibile modificare le opzioni suggerite in modo da campionare più o meno dati, specificare il tipo di dati predefinito per numeri interi o dati booleani oppure aggiungere spazi come spaziatura interna nelle colonne stringa.  
  
    Per il momento non apportare modifiche alle opzioni nella finestra di dialogo **Suggerisci tipi di colonne** e selezionare **OK** perché [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggerisca i tipi di dati per le colonne. Questa azione visualizza di nuovo il riquadro **Avanzate** della finestra di dialogo **Editor gestione connessione file flat** in cui è possibile visualizzare i tipi di dati delle colonne suggeriti da [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. In alternativa, se si seleziona **Annulla** non verranno indicati suggerimenti relativi ai metadati delle colonne e verrà usato il tipo di dati string predefinito, ovvero DT_STR.  
  
    In questa esercitazione, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggerisce i tipi di dati mostrati nella seconda colonna della tabella seguente per i dati ricavati dal file SampleCurrencyData.txt. La quarta colonna visualizza i tipi di dati necessari per le colonne nella destinazione, definite in un passaggio successivo.  
  
    |Colonna file flat|Tipo suggerito|Colonna destinazione|Tipo destinazione|  
    |--------------------|------------------|----------------------|--------------------|  
    |AverageRate|float [DT_R4]|FactCurrencyRate.AverageRate|float|  
    |CurrencyID|string [DT_STR]|DimCurrency.CurrencyAlternateKey|nchar(3)|  
    |CurrencyDate|date [DT_DATE]|DimDate.FullDateAlternateKey|Data|  
    |EndOfDayRate|float [DT_R4]|FactCurrencyRate.EndOfDayRate|float|  
  
    Il tipo di dati suggerito per la colonna **CurrencyID** non è compatibile con il tipo di dati del campo della tabella di destinazione. Dal momento che il tipo di dati di `DimCurrency.CurrencyAlternateKey` è nchar(3), è necessario modificare **CurrencyID** da string [DT_STR] a Unicode string [DT_WSTR]. Il campo `DimDate.FullDateAlternateKey`, poi, viene definito come tipo di dati relativo alla data, quindi il tipo di **CurrencyDate** deve essere modificato da date [DT_Date] a database date [DT_DBDATE].  
  
2.  Nell'elenco selezionare la colonna **CurrencyID** e nel riquadro delle proprietà cambiare il tipo di dati della colonna **CurrencyID** da string [DT_STR] a Unicode string [DT_WSTR].  
  
3.  Nel riquadro delle proprietà modificare il tipo di dati della colonna **CurrencyDate** da date [DT_DATE] a database date [DT_DBDATE].  
  
4.  Selezionare **OK**.  
  
## <a name="go-to-next-task"></a>Esecuzione del passaggio successivo
[Passaggio 3: Aggiungere e configurare una gestione connessione OLE DB](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
## <a name="see-also"></a>Vedi anche  
[Gestione connessione file flat](../integration-services/connection-manager/flat-file-connection-manager.md)  
[Tipi di dati di Integration Services](../integration-services/data-flow/integration-services-data-types.md)  
  
  
  
