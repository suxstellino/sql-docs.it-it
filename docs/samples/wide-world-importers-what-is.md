---
title: Wide World Importers-database di esempio per SQL | Microsoft Docs
description: Informazioni sul modo in cui i database di esempio WideWorldImporters supportano i flussi di lavoro della società WideWorldImporters fittizia.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f844299a392f9521afe3af7d72bc72272304d3a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354046"
---
# <a name="wide-world-importers-sample-databases-for-microsoft-sql"></a>Database di esempio Wide World Importers per Microsoft SQL
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Si tratta di una panoramica delle aziende e dei flussi di lavoro del mondo fittizio che vengono risolti nei database di esempio WideWorldImporters per SQL Server e il database SQL di Azure.  

Wide World Importers (prima guerra) è un ingrosso dei prodotti importatori e distributori che operano dall'area San Francisco Bay.

In qualità di Grossista, i clienti della prima guerra sono principalmente aziende che rivendono a singoli utenti. La prima guerra vende ai clienti al dettaglio tra i Stati Uniti inclusi negozi specializzati, supermercati, archivi di calcolo, negozi di attrazioni turistiche e alcuni utenti. La prima guerra si vende anche ad altri grossisti tramite una rete di agenti che promuovono i prodotti per conto della prima guerra. Anche se tutti i clienti della prima guerra si basano attualmente sulla Stati Uniti, l'azienda intende effettuare il push dell'espansione in altri paesi.

La prima guerra acquista prodotti da fornitori, tra cui produttori di giocattoli e giocattoli, e altri ingrosso. Le scorte vengono risalete nel magazzino della prima guerra e riordinate dai fornitori in base alle esigenze per soddisfare gli ordini dei clienti. Inoltre, acquistano grandi volumi di materiali per la creazione di pacchetti e li vendono in quantità minori come vantaggio per i clienti.

Recentemente, la prima guerra inizia a vendere una varietà di novità commestibili come peperoncino cioccolato.  In precedenza la società non ha dovuto gestire gli elementi refrigerati. A questo punto, per soddisfare i requisiti di gestione degli alimenti, è necessario monitorare la temperatura nella propria stanza chiller e in tutti i relativi autocarri con sezioni chiller.

## <a name="workflow-for-warehouse-stock-items"></a>Flusso di lavoro per gli elementi azionari del magazzino

Il flusso tipico per la modalità di inventario e distribuzione degli elementi è il seguente:
- La prima guerra crea gli ordini di acquisto e invia gli ordini ai fornitori.
- I fornitori inviano gli elementi, la prima guerra li riceve e li archivia nel magazzino.
- Ordinare gli elementi dei clienti dalla prima guerra
- La prima guerra riempie l'ordine del cliente con le scorte del magazzino e, quando non dispone di scorte sufficienti, Ordina le scorte aggiuntive dai fornitori.
- Alcuni clienti non desiderano attendere gli elementi che non sono in magazzino. Se ordinano cinque voci diverse e quattro sono disponibili, vogliono ricevere i quattro elementi ed eseguire il backorder dell'elemento rimanente. L'elemento verrà quindi inviato successivamente in una spedizione separata.
- La prima guerra fattura i clienti per gli elementi azionari, in genere convertendo l'ordine in una fattura.
- I clienti possono ordinare gli elementi che non sono in magazzino. Questi elementi sono riordinati.
- La prima guerra distribuisce articoli ai clienti tramite i propri Furgoni di consegna oppure tramite altri corrieri o metodi di spedizione.
- I clienti pagano le fatture alla prima guerra.
- Periodicamente, la prima guerra paga i fornitori per gli articoli che si trovavano negli ordini di acquisto. Si tratta spesso di una volta che ha ricevuto la merce.

## <a name="data-warehouse-and-analysis-workflow"></a>Flusso di lavoro di data warehouse e analisi

Mentre il team alla prima guerra USA SQL Server Reporting Services per generare report operativi dal database WideWorldImporters, è necessario anche eseguire analisi sui dati ed è necessario generare report strategici. Il team ha creato un modello di dati dimensionale in un database WideWorldImportersDW. Questo database è popolato da un pacchetto di Integration Services.

SQL Server Analysis Services viene utilizzato per creare modelli di dati analitici dai dati nel modello di dati dimensionale. SQL Server Reporting Services viene utilizzato per generare report strategici direttamente dal modello di dati dimensionali e anche dal modello analitico. Power BI viene usato per creare dashboard dagli stessi dati. I dashboard vengono usati nei siti Web e su telefoni e tablet. *Nota: i modelli di dati e i report non sono ancora disponibili*

## <a name="additional-workflows"></a>Flussi di lavoro aggiuntivi

Si tratta di flussi di lavoro aggiuntivi.
- La prima guerra rilascia note di credito quando un cliente non riceve il bene per qualche motivo o quando i beni sono difettosi. Questi vengono considerati come fatture negative.
- La prima guerra conta periodicamente le quantità su misura degli articoli azionari per garantire la correttezza delle quantità di titoli visualizzate come disponibili nel sistema. Il processo di questa operazione è denominato Stocktake.
- Temperature della stanza fredda. Le merci deteriorabili vengono archiviate in celle frigorifere. I dati dei sensori di queste chat vengono inseriti nel database per scopi di monitoraggio e analisi.
- Rilevamento della posizione del veicolo. I veicoli che trasportano merci per la prima guerra includono sensori che tengono traccia della località. Questo percorso viene nuovamente inserito nel database per il monitoraggio e l'analisi.

## <a name="fiscal-year"></a>Anno fiscale

La società opera con un anno finanziario che inizia il 1 ° novembre.

## <a name="terms-of-use"></a>Condizioni per l'utilizzo

La licenza per il database di esempio e il codice di esempio è descritta di seguito: [license.txt](https://github.com/Microsoft/sql-server-samples/blob/master/license.txt)

Il database di esempio include dati pubblici caricati da data.gov e EarthData naturale. Le condizioni per l'utilizzo sono: [https://www.naturalearthdata.com/about/terms-of-use/](https://www.naturalearthdata.com/about/terms-of-use/)
