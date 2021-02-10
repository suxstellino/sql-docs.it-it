---
title: Risoluzione dei problemi di Azure Data Studio
description: Informazioni su come ottenere i log e risolvere i problemi di Azure Data Studio, utili per la creazione di report sui bug.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: hanqin, maghan
ms.custom: seodec18
ms.date: 11/24/2020
ms.openlocfilehash: 81fb4957672fa2008582b0ab2397322fb2b30043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039971"
---
# <a name="azure-data-studio-troubleshooting"></a>Risoluzione dei problemi di Azure Data Studio
Azure Data Studio tiene traccia dei problemi e delle richieste di funzionalità tramite una scheda [Issue Tracker del repository GitHub](https://github.com/Microsoft/azuredatastudio/issues) per il repository `azuredatastudio`. 

## <a name="if-youve-experienced-any-issue"></a>In caso di problemi

Segnalare i problemi nella scheda [Issue Tracker di GitHub](https://github.com/Microsoft/azuredatastudio/issues) e comunicare i dettagli che consentiranno di riprodurre l'errore. Includere tutte le [informazioni del log](#how-to-set-the-logging-level) presenti nel file di log.

## <a name="writing-good-bug-reports-and-feature-requests"></a>Creazione di report sui bug e richieste di funzionalità utili

Compilare un singolo problema per ogni bug e richiesta di funzionalità.

* Non elencare più bug o richieste di funzionalità all'interno dello stesso problema.
* Non aggiungere il problema come commento a un problema esistente a meno che non si tratti di un input identico. Molti problemi sembrano simili ma hanno cause diverse.

Maggiori sono le informazioni inviate, maggiore sarà la probabilità che un utente riesca a riprodurre il problema e a trovare una soluzione. 

Includere le informazioni seguenti con ogni problema:

* Versione di Azure Data Studio
* Passaggi riproducibili (1... 2... 3...) e ciò che era previsto rispetto a ciò che è avvenuto realmente. 
* Immagini, animazioni o un collegamento a un video. Immagini e animazioni illustrano la procedura per riprodurre il problema, ma non la sostituiscono.
* Un frammento di codice che illustra il problema o un collegamento a un repository di codice facilmente visualizzabile sul computer per riprodurre il problema. 

> [!NOTE]
>  Poiché è necessario copiare e incollare il frammento di codice, non è sufficiente includere un frammento di codice sotto forma di file multimediale, ad esempio un file con estensione gif. 

* Errori nella console degli strumenti di sviluppo (Guida | Attiva/Disattiva strumenti di sviluppo)

Ricordare di eseguire le operazioni seguenti:

* Eseguire una ricerca nel repository dei problemi per verificare l'eventuale presenza di un duplicato. 
* Semplificare il codice intorno al problema in modo da poterlo isolare meglio. 

Non scoraggiarsi se non è possibile riprodurre il problema e sono necessarie altre informazioni!

## <a name="how-to-set-the-logging-level"></a>Come impostare il livello di registrazione

### <a name="azure-data-studio"></a>Azure Data Studio
Eseguire il comando `Developer: Set Log Level...` per selezionare il livello di registrazione per la sessione corrente. Questo valore NON viene salvato in modo permanente su più sessioni, perciò in caso di riavvio di Azure Data Studio verrà ripristinato il livello `Info` predefinito. 

Se si vuole abilitare la registrazione del debug per l'avvio, impostare il livello di registrazione su `Debug` ed eseguire il comando `Developer: Reload Window`

### <a name="mssql-built-in-extension"></a>MSSQL (estensione incorporata)

Se l'impostazione utente `Mssql: Log Debug Info` è true, le informazioni sul log di debug verranno inviate al canale di output `MSSQL`.

L'impostazione utente `Mssql: Tracing Level` viene usata per controllare il livello di dettaglio della registrazione.

## <a name="debug-log-location"></a>Posizione del log di debug
Da Azure Data Studio eseguire il comando `Developer: Open Logs Folder` per aprire il percorso ai log. In questa posizione scrivono molti tipi di file diversi, tra cui i più comuni sono i seguenti:

1. `renderer#.log` (ad esempio, renderer1.log): si tratta del file di log del processo principale.
1. `telemetry.log`: quando il livello di registrazione è impostato su `Trace`, questo file conterrà gli eventi di telemetria inviati da Azure Data Studio
1. `exthost#/exthost.log`: file di log per il processo host di estensione (si tratta solo del processo e non delle estensioni in esecuzione al suo interno)
1. `exthost#/Microsoft.mssql`: log per l'estensione MSSQL, contenente gran parte della logica di base per le funzionalità correlate a MSSQL
   * sqltools.log è il log per il servizio SQL Tools
1. `exthost#/output_logging_#######`: queste cartelle contengono i messaggi visualizzati nel pannello `Output` in Azure Data Studio. Ogni file è denominato `#-<Channel Name>`, perciò ad esempio il canale di output `Notebooks` può restituire un file denominato `3-Notebooks.log`.

Se viene richiesto di inviare i log, comprimere l'intera cartella per assicurarsi che siano inclusi i log corretti. 

## <a name="next-steps"></a>Passaggi successivi
- [Segnalare un problema](https://github.com/Microsoft/azuredatastudio/issues)
- [Informazioni su Azure Data Studio](what-is-azure-data-studio.md)