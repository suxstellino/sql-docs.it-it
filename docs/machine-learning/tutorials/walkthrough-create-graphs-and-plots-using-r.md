---
title: 'Esercitazione su R: Creare grafici e tracciati'
description: Informazioni sulle tecniche per la generazione di tracciati e mappe usando il linguaggio R con dati di SQL Server. Creare un istogramma semplice e quindi sviluppare un tracciato mappa più complesso.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: edf65095f245e5185b9eeadbd1d8b6c8e87e49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272952"
---
# <a name="create-graphs-and-plots-using-sql-and-r-walkthrough"></a>Creare grafici e tracciati con SQL e R (procedura dettagliata)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

In questa parte della procedura dettagliata vengono illustrate le tecniche per generare grafici e mappe usando R con dati di SQL Server. Si creerà un istogramma semplice e quindi si svilupperà un tracciato mappa più complesso.

## <a name="prerequisites"></a>Prerequisiti

Questo passaggio presuppone una sessione R in corso basata sui passaggi precedenti di questa procedura dettagliata. Usa le stringhe di connessione e gli oggetti origine dati creati in tali passaggi. Per eseguire lo script vengono usati gli strumenti e i pacchetti seguenti.

+ Rgui.exe per eseguire i comandi R
+ Management Studio per eseguire T-SQL
+ googMap
+ Pacchetto ggmap
+ Pacchetto mapproj

## <a name="create-a-histogram"></a>Creare un istogramma

1. Generare il primo tracciato usando la funzione [rxHistogram](/r-server/r-reference/revoscaler/rxdatasource) .  La funzione rxHistogram fornisce funzionalità simili a quelle presenti nei pacchetti R open source, ma può essere eseguita in un contesto di esecuzione remota.

    ```R
    # Plot fare amount on SQL Server and return the plot
    start.time <- proc.time()
    rxHistogram(~fare_amount, data = inDataSource, title = "Fare Amount Histogram")
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2), " seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate plot.", sep=""))
    ```

2. L'immagine viene restituita nel dispositivo grafico R per l'ambiente di sviluppo.  Ad esempio, in RStudio fare clic sulla finestra **Plot** .  In [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)]viene aperta una finestra grafica separata.

    ![uso di rxHistogram per tracciare gli importi delle tariffe](media/rsql-e2e-rxhistogramresult.png "uso di rxHistogram per tracciare gli importi delle tariffe")

    > [!NOTE]
    > Il grafico ha un aspetto diverso?
    >  
    > Il motivo è che _inDataSource_ usa solo le prime 1.000 righe. L'ordinamento delle righe mediante TOP non è deterministico in assenza di una clausola ORDER BY, quindi è previsto che i dati e il grafico risultante possano essere diversi.
    > Questa particolare immagine è stata generata usando circa 10.000 righe di dati. È consigliabile provare con diversi numeri di righe per ottenere grafici differenti, osservando il tempo necessario alla restituzione dei risultati nell'ambiente in uso.

## <a name="create-a-map-plot"></a>Creare un tracciato mappa

In genere, i server di database bloccano l'accesso a Internet. Questo può risultare scomodo quando si usano pacchetti R che devono scaricare mappe o altre immagini per generare i tracciati. Tuttavia, esiste una soluzione alternativa che può risultare utile per lo sviluppo di applicazioni personalizzate. Di base, si genera la rappresentazione della mappa nel client e quindi si sovrappongono alla mappa i punti, archiviati come attributi nella tabella di SQL Server.

1. Definire la funzione che crea l'oggetto tracciato R. La funzione personalizzata *mapPlot* crea un grafico a dispersione che usa i punti di inizio corsa dei taxi e traccia il numero di corse iniziate da ogni posizione. Usa i pacchetti **ggplot2** e **ggmap**, che dovrebbero già essere [installati e caricati](walkthrough-data-science-end-to-end-walkthrough.md#add-packages).

    ```R
    mapPlot <- function(inDataSource, googMap){
        library(ggmap)
        library(mapproj)
        ds <- rxImport(inDataSource)
        p <- ggmap(googMap)+
        geom_point(aes(x = pickup_longitude, y =pickup_latitude ), data=ds, alpha =.5,
    color="darkred", size = 1.5)
        return(list(myplot=p))
    }
    ```

    + La funzione *mapPlot* accetta due argomenti: un oggetto dati esistente (definito in precedenza con RxSqlServerData) e la rappresentazione della mappa passata dal client.
    + Nella riga che inizia con la variabile *ds*, rxImport viene usato per caricare i dati in memoria dall'origine dati creata in precedenza, *inDataSource*. Tale origine dati contiene solo 1.000 righe. Se si vuole creare una mappa con più punti dati, è possibile sostituirla con un'origine dati diversa.
    + Quando si usano funzioni R open source è necessario che i dati siano caricati nella memoria locale in data frame. Tuttavia, chiamando la funzione [rxImport](/r-server/r-reference/revoscaler/rximport), è possibile eseguire nella memoria del contesto di calcolo remoto.

2. Cambiare il contesto di calcolo in quello locale e caricare le librerie necessarie per la creazione delle mappe.

    ```R
    rxSetComputeContext("local")
    library(ggmap)
    library(mapproj)
    gc <- geocode("Times Square", source = "google")
    googMap <- get_googlemap(center = as.numeric(gc), zoom = 12, maptype = 'roadmap', color = 'color');
    ```

    + La variabile `gc` memorizza un set di coordinate per Times Square, New York.

    + La riga che inizia con `googmap` genera una mappa con al centro le coordinate specificate.

3. Passare al contesto di calcolo SQL Server ed eseguire il rendering dei risultati eseguendo il wrapping della funzione di creazione del tracciato in [rxExec](/r-server/r-reference/revoscaler/rxexec) come illustrato di seguito. La funzione rxExec è inclusa nel pacchetto **RevoScaleR** e supporta l'esecuzione di funzioni R arbitrarie in un contesto di calcolo remoto.

    ```R
    rxSetComputeContext(sqlcc)
    myplots <- rxExec(mapPlot, inDataSource, googMap, timesToRun = 1)
    plot(myplots[[1]][["myplot"]]);
    ````

    + I dati della mappa in `googMap` vengono passati come argomento alla funzione *mapPlot* eseguita in modalità remota. Poiché le mappe sono state generate nell'ambiente locale, devono essere passate alla funzione per creare il tracciato nel contesto di SQL Server.

    + Quando la riga che inizia con `plot` viene eseguita, i dati di cui è stato eseguito il rendering vengono di nuovo serializzati nell'ambiente R locale, in modo che sia possibile vederli nel client R.

    > [!NOTE]
    > Se si usa SQL Server in una macchina virtuale di Azure, a questo punto si potrebbe ricevere un errore. Si verifica un errore quando la regola del firewall predefinita in Azure blocca l'accesso alla rete da parte del codice R. Per informazioni dettagliate su come correggere questo errore, vedere [Installare Machine Learning Services con R in una macchina virtuale di Azure](../install/sql-machine-learning-azure-virtual-machine.md).

4. La figura seguente mostra il tracciato di output. I punti di inizio corsa dei taxi vengono aggiunti alla mappa come punti rossi. L'immagine potrebbe essere diversa, a seconda del numero di posizioni presenti nell'origine dati usata.

    ![creazione del tracciato delle corse in taxi con una funzione R personalizzata](media/rsql-e2e-mapplot.png "creazione del tracciato delle corse in taxi con una funzione R personalizzata")

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare funzionalità di dati con SQL e R](walkthrough-create-data-features.md)