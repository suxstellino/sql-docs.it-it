---
title: SandDance per Azure Data Studio
description: Informazioni su come usare un'estensione di Azure Data Studio per creare rapidamente visualizzazioni dei dati, ovvero visualizzazioni che forniscono informazioni dettagliate.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 07/03/2019
ms.openlocfilehash: d031ddea065f035bf4d1f4ffc70bd65d90f1bd0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040336"
---
# <a name="sanddance-for-azure-data-studio-preview"></a>SandDance per Azure Data Studio (anteprima)

Azure Data Studio offre ora la possibilità di creare visualizzazioni rapide per i dati. Questa estensione è utile quando si vuole esaminare i dati per capire cosa sta accadendo. È basata su una tecnologia denominata SandDance di Microsoft Research, in grado di generare visualizzazioni sul posto dei dati.

![sanddance-animazione](https://user-images.githubusercontent.com/11507384/54236654-52d42800-44d1-11e9-859e-6c5d297a46d2.gif)

Avvalendosi di visualizzazioni di facile interpretazione, SandDance consente di trovare informazioni dettagliate sui dati, che a loro volta permettono di illustrare episodi supportati dai dati, creare casi basati su evidenze, testare ipotesi, approfondire spiegazioni sulle superfici, supportare decisioni di acquisto o correlare i dati in un contesto più ampio e reale.

SandDance usa visualizzazioni di unità che applicano un mapping uno-a-uno tra le righe del database e gli indicatori sullo schermo.
Uniformi transizioni animate tra le visualizzazioni consentono inoltre di mantenere il contesto durante l'interazione con i dati.

## <a name="usage"></a>Uso

### <a name="view-csv-or-tsv-files"></a>Visualizzare file con estensione csv o tsv
inclusi i file locali e i file in HDFS nell'ambito del cluster Big Data di SQL Server 2019.
 
Dal menu file usare Apri cartella o [CTRL + K CTRL + O] per aprire la directory contenente il file CSV.  Quindi, nel pannello di Explorer fare clic con il pulsante destro del mouse sul file CSV o TSV e scegliere *Visualizza in SandDance*.

Fare clic con il pulsante destro del mouse su un file CSV o TSV in HDFS, se si è connessi al cluster Big Data di SQL Server 2019, e scegliere *Visualizza in SandDance*.

### <a name="view-sql-query-results"></a>Visualizzare i risultati delle query SQL

Partendo da una finestra di query SQL, eseguire una query per ottenere una griglia di risultati. Fare clic sull'icona del visualizzatore sul lato destro del riquadro dei risultati.

## <a name="known-issues"></a>Problemi noti

Attualmente non è possibile limitare il numero di righe visualizzato. L'utilizzo della memoria, tuttavia, aumenta proporzionalmente al numero di righe ed è quindi consigliabile che la visualizzazione o il set di dati sia limitato a circa 100.000 righe.

Vedere [Problemi noti](https://microsoft.github.io/SandDance/#known-issues)

## <a name="release-notes"></a>Note sulla versione

### <a name="100"></a>1.0.0

Versione iniziale di azdata-sanddance

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni, [visitare il repository GitHub](https://github.com/Microsoft/SandDance).
