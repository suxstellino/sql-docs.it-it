---
title: Generare dati in esempi SQL WideWorldImporters
description: Usare queste istruzioni SQL per generare e importare dati di esempio fino alla data corrente per i database di esempio WideWorldImporters.
ms.date: 10/23/2020
ms.reviewer: ''
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: f54370d9aa54602964817fc17704a324134c0757
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354101"
---
# <a name="wideworldimporters-data-generation"></a>Generazione di dati WideWorldImporters
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Le versioni rilasciate dei database WideWorldImporters e WideWorldImportersDW hanno dati del 1 ° gennaio 2013, fino al giorno in cui i database sono stati generati.

Quando si usano questi database di esempio, potrebbe essere necessario includere dati di esempio più recenti.

## <a name="data-generation-in-wideworldimporters"></a>Generazione dei dati in WideWorldImporters

Per generare dati di esempio fino alla data corrente:

1. Se non è ancora stato fatto, installare una versione pulita del database WideWorldImporters. Per le istruzioni di installazione, vedere [installazione e configurazione](wide-world-importers-oltp-install-configure.md).
2. Eseguire l'istruzione seguente nel database:

    ```
        EXECUTE DataLoadSimulation.PopulateDataToCurrentDate
            @AverageNumberOfCustomerOrdersPerDay = 60,
            @SaturdayPercentageOfNormalWorkDay = 50,
            @SundayPercentageOfNormalWorkDay = 0,
            @IsSilentMode = 1,
            @AreDatesPrinted = 1;
    ```

    Questa istruzione consente di aggiungere al database i dati di esempio relativi a vendite e acquisti, fino alla data corrente. Visualizza lo stato di avanzamento della generazione dei dati in base al giorno. A causa di un fattore casuale nella generazione dei dati, esistono alcune differenze nei dati generati tra le esecuzioni.

    Per aumentare o ridurre la quantità di dati generati per gli ordini al giorno, modificare il valore per il parametro `@AverageNumberOfCustomerOrdersPerDay` . Usare i parametri `@SaturdayPercentageOfNormalWorkDay` e `@SundayPercentageOfNormalWorkDay` per determinare il volume dell'ordine per i giorni del fine settimana.

> [!TIP]
> La forzatura della [durabilità ritardata](../relational-databases/logs/control-transaction-durability.md) nel database può migliorare la velocità di generazione dei dati, in particolare quando il log delle transazioni del database si trova in un sottosistema di archiviazione a latenza elevata. Tenere presente le implicazioni potenziali per la [perdita di dati](../relational-databases/logs/control-transaction-durability.md#bkmk_DataLoss) quando si usa la durabilità posticipata e si consiglia di abilitare solo la durabilità posticipata per la durata della generazione dei dati.

## <a name="import-generated-data-in-wideworldimportersdw"></a>Importare i dati generati in WideWorldImportersDW

Per importare dati di esempio fino alla data corrente nel database OLAP WideWorldImportersDW:

1. Eseguire la logica di generazione dei dati nel database OLTP WideWorldImporters usando la procedura descritta nella sezione precedente.
2. Se non è ancora stato fatto, installare una versione pulita del database WideWorldImportersDW. Per le istruzioni di installazione, vedere [installazione e configurazione](wide-world-importers-oltp-install-configure.md).
3. Eseguire nuovamente il seeding del database OLAP eseguendo l'istruzione seguente nel database:

    ```sql
    EXECUTE [Application].Configuration_ReseedETL
    ```

4. Eseguire il pacchetto di SQL Server Integration Services *ETL. ispac giornaliero* per importare i dati nel database OLAP. Per informazioni su come eseguire il processo ETL, vedere [Workflow ETL wideworldimporters](wide-world-importers-perform-etl.md).

## <a name="generate-data-in-wideworldimportersdw-for-performance-testing"></a>Generare dati in WideWorldImportersDW per il test delle prestazioni

WideWorldImportersDW può aumentare arbitrariamente le dimensioni dei dati per il test delle prestazioni. Ad esempio, può aumentare la dimensione dei dati da usare con l'indicizzazione columnstore cluster.

Una delle difficoltà consiste nel ridurre le dimensioni del download abbastanza piccolo da scaricare facilmente, ma sufficientemente grande da dimostrare SQL Server funzionalità di prestazioni. Ad esempio, i vantaggi significativi per gli indici columnstore vengono realizzati solo quando si lavora con un numero maggiore di righe. 

È possibile utilizzare la `Application.Configuration_PopulateLargeSaleTable` procedura per aumentare il numero di righe nella `Fact.Sale` tabella. Le righe vengono inserite nell'anno di calendario 2012 per evitare la collisione con i dati esistenti dell'importatore mondiale che iniziano il 1 ° gennaio 2013.

### <a name="procedure-details"></a>Dettagli procedura

#### <a name="name"></a>Nome

`Application.Configuration_PopulateLargeSaleTable`

#### <a name="parameters"></a>Parametri

`@EstimatedRowsFor2012`**bigint** (il cui valore predefinito è 12 milioni)

#### <a name="result"></a>Risultato

Approssimativamente il numero di righe richiesto viene inserito nella `Fact.Sale` tabella nell'anno 2012. La procedura limita artificialmente il numero di righe a 50.000 al giorno. È possibile modificare questa limitazione, ma la limitazione consente di evitare Sovrainflazione accidentali della tabella.

La procedura applica anche l'indicizzazione columnstore cluster se non è già stata applicata.
