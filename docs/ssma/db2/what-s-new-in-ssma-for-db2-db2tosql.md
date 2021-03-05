---
title: Novità di SSMA per DB2 (DB2ToSQL) | Microsoft Docs
description: Scopri le modifiche apportate a SQL Server Migration Assistant (SSMA) per DB2 (DB2ToSQL) per ogni versione.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1cc38f85-3caa-42d0-8c76-a380c1d15c67
ms.author: alexiva
ms.openlocfilehash: a12bb61546ed042a98a3cae521572ef55723cfdf
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186508"
---
# <a name="whats-new-in-ssma-for-db2-db2tosql"></a>Novità di SSMA per DB2 (DB2ToSQL)

Questo articolo elenca SQL Server Migration Assistant (SSMA) per le modifiche DB2 in ogni versione.

## <a name="ssma-v817"></a>SSMA v 8.17

La versione v 8.17 di SSMA per DB2 contiene le modifiche seguenti:

* Migliorare la conversione della funzione TRANSLATE
* Correzione della migrazione dei dati per le tabelle con colonne calcolate
* Aggiornare i report di valutazione HTML per utilizzare l'editor moderno per visualizzare il testo SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La versione v 8.16 di SSMA per DB2 contiene le modifiche seguenti:

* Correzione della conversione degli alias di colonna con caratteri speciali
* Correzione della conversione per la `SELECTIVITY` clausola
* Migliorare la conversione per la `WITH ROW MOVEMENT` clausola
* Rimuovere il supporto per il parser legacy
* Correzione di un problema relativo agli oggetti che non si aggiornano dal database

## <a name="ssma-v815"></a>SSMA v 8.15

Oltre a diversi miglioramenti dell'accessibilità, la versione v 8.15 di SSMA per DB2 contiene le modifiche seguenti:

* Correzione della conversione di `MIN` / `MAX` funzioni di aggregazione con argomenti di data/ora
* Correzione di un bug nella `VARCHAR_FORMAT` funzione di emulazione quando `DD` viene usato il segnaposto
* Migliorare i mapping dei tipi per il `TIME` tipo di dati
* Migliorare la conversione `ROUND` delle `TRUNC` funzioni e con argomenti numerici
* Rinnovare i report di valutazione per lavorare nei browser moderni
* Usare l'autorità fornita dal database per l'autenticazione Azure AD
* Migliorare la denominazione per le istruzioni caricate da file

## <a name="ssma-v814"></a>SSMA v 8.14

Oltre a diversi miglioramenti per garantire maggiore accessibilità per gli utenti con particolari esigenze, la versione 8.14 di SSMA per DB2 richiede un aggiornamento del progetto, perché archivia ora la versione completa del server di origine/destinazione nei metadati del progetto.

## <a name="ssma-v813"></a>SSMA v 8.13

La versione v 8.13 di SSMA per DB2 contiene le modifiche seguenti:

* Supporto per indici univoci filtrati
* Considerare i cast di tipo implicito durante la conversione di chiamate di funzione e routine
* Migliorare la registrazione per la stringa di connessione di origine per risolvere i problemi di connessione

## <a name="ssma-v812"></a>SSMA v 8.12

La versione v 8.12 di SSMA per DB2 contiene le modifiche seguenti:

* Conversione della `STRIP` funzione
* Analisi migliorata delle opzioni delle procedure

## <a name="ssma-v811"></a>SSMA v 8.11

La versione v 8.11 di SSMA per DB2 contiene le modifiche seguenti:

* Supporto per DB2 per i (v 7.1 e versioni successive)
* Traduzione di `SQLSTATE` e `SQLCODE`
* Messaggio di errore di conversione per gli operatori con effetto collaterale all'interno di una funzione
* Usare la libreria MSAL.NET per l'autenticazione Azure Active Directory interattiva

## <a name="ssma-v810"></a>SSMA v 8.10

La versione v 8.10 di SSMA per DB2 risolve una regressione nell'individuazione di chiavi esterne e contiene miglioramenti delle prestazioni secondari.

## <a name="ssma-v89"></a>SSMA v 8.9

La versione v 8.9 di SSMA per DB2 contiene le modifiche seguenti:

* Correzione per la conversione della `TIMESTAMPDIFF` funzione
* Correzione per l'individuazione degli indici quando è presente un indice partizionato
* Correzione per l'individuazione di chiavi esterne quando l'indice primario è definito in un altro schema
* Conversione migliorata per le colonne che corrispondono ai nomi di funzione predefiniti
* Correzione del problema con i caratteri speciali nel nome del progetto

## <a name="ssma-v88"></a>SSMA v 8.8

La versione v 8.8 di SSMA per DB2 include:

* Miglioramenti alla stabilità della sincronizzazione degli oggetti SQL Server
* Miglioramenti delle prestazioni dell'interfaccia utente grafica durante valutazione e conversione
* Aggiornamento del mapping da `ROWID` a `varbinary(40)` per facilitare la migrazione dei dati
* Conversione migliorata delle `SELECT ... FROM NEW/OLD TABLE` istruzioni
* Nuova conversione di `ALTER` istruzioni per procedure e funzioni
* Nuova conversione delle assegnazioni di destrutturazione

## <a name="ssma-v87"></a>SSMA v 8,7

La versione v 8.7 di SSMA per DB2 include il parser della sintassi DB2 nuovissimo, nonché correzioni minime e miglioramenti delle prestazioni nell'interfaccia utente grafica.

Inoltre, SSMA per DB2 offre ora:

* Correzione per l'individuazione delle chiavi esterne durante la migrazione da DB2 in LUW.
* Conversione migliorata dell' `SELECT ... FOR UPDATE` istruzione.
* Conversione migliorata per la `COUNT` funzione nelle tabelle mq.
* Conversione delle `SAVEPOINT` istruzioni.
* Conversione per emulare il comportamento di DB2 per `NULL` i valori nella `ORDER BY` clausola.
* Supporto di analisi per l' `ASSOCIATE RESULT SET` istruzione.

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Oltre a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni, la versione 8.6 di SSMA per DB2 è stata migliorata aggiungendo un'impostazione che consente agli utenti di omettere le proprietà estese di SSMA nel codice convertito.

Per sfruttare questa impostazione, in SSMA per DB2 passare a **strumenti**  >  **Impostazioni progetto**  >    >  **conversione** generale, quindi in **varie** aggiornare il valore dell'opzione **omette proprietà estese** su **Sì**.

![Omettere l'impostazione delle proprietà estese](../db2/media/ssma-omit-extended-properties.png)

Inoltre, SSMA per DB2 offre ora:

* Correzione per la conversione di funzioni che utilizzano valori di argomento predefiniti.
* Analisi migliorata della `PARAMETER` clausola per le funzioni.
* Possibilità di convertire l' `LEAVE` istruzione.

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versione v 8.5 di SSMA per DB2 è stata migliorata con il supporto per l'autenticazione Azure Active Directory e il supporto di base per le funzionalità JSON in SQL Server, insieme a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni.

Inoltre, SSMA per DB2 è stato migliorato con:

* Supporto per l'aggiunta della conversione per l' `GET DIAGNOSTICS` istruzione con `ROW_NUMBER` .
* Correzione di un bug relativo agli spazi all'inizio del nome dell'oggetto non rispettato.

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versione v 8.4 di SSMA per DB2 è stata migliorata con correzioni mirate progettate per risolvere i problemi di accessibilità e correggere un bug correlato alle colonne di indice Max (per consentire 32 anziché 16) per SQL Server 2016 e versioni successive.

> [!IMPORTANT]
> Con SSMA versioni 7,4, sebbene 8,4, .NET 4.5.2 è un prerequisito di installazione.

## <a name="ssma-v83"></a>SSMA v 8.3

La versione 8.3 di SSMA per DB2 è stata migliorata con correzioni mirate progettate per migliorare le metriche di qualità e conversione. Inoltre, questa versione di SSMA per DB2 fornisce le correzioni seguenti:

* Risolvere i problemi di accessibilità.
* Aggiungere il supporto di base per il `hierarchyid` tipo in SQL Server.
* Sostituire l'utilizzo della funzione TRIM nelle query di individuazione z/OS con `RTRIM` / `LTRIM` .
* Consenti all'utente di specificare la raccolta di pacchetti durante la connessione in modalità standard (impostazione predefinita `NULLID` ).
* Aggiungere la conversione per `CREATE TABLE AS SELECT` .
* Migliorare le conversioni per le tabelle temporanee globali.
* Risolvere un problema relativo alla verifica dell'univocità degli oggetti per definire la priorità delle tabelle rispetto ai vincoli, se i nomi sono in conflitto.
* Risolvere un problema relativo al caricamento dei valori di colonna predefiniti per `DATE` e `TIMESTAMP` per z/OS.
* Supporta il carattere di avanzamento riga Unicode (noto anche come `NEL` ).
* Risolvere un problema relativo alla conversione del cursore con la `RETURN TO` clausola Missing.
* Aggiungere il supporto per le etichette e `GOTO` .

## <a name="ssma-v82"></a>SSMA v 8.2

La versione v 8.2 di SSMA per DB2 è stata migliorata con per risolvere i problemi relativi alle connessioni al database SQL di Azure dallo strumento Console SSMA ed è mancante COUNT_BIG colonna nella dichiarazione delle visualizzazioni durante la conversione. Inoltre, questa versione include un set di correzioni mirato progettato per migliorare la qualità e la metrica di conversione, nonché le correzioni per:

* Un problema con gli indici non cluster disabilitati dopo la migrazione dei dati.
* Rilevamento dei .NET Framework durante l'installazione invisibile all'utente.
* Arresti anomali intermittenti che si verifica quando viene scaricata una nuova versione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.1 a v 8.2. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versione v 8.1 di SSMA per DB2 è stata migliorata per fornire correzioni mirate progettate per migliorare la qualità e le metriche di conversione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.0 a v 8.1. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versione v 8.0 di SSMA per DB2 è stata migliorata per fornire correzioni mirate progettate per migliorare la qualità e la metrica di conversione. Questa versione offre anche le seguenti nuove funzionalità:

* Supporto per **istanza gestita SQL di Azure** come destinazione. È ora possibile creare nuovi progetti destinati a Istanza gestita SQL di Azure:

  ![Progetto SQL MI](../media/ssma-newproject-sqldbmi.png)

* **Avviso di correzione** post-conversione. Altre informazioni sono disponibili [qui](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selezione dello schema o del database preliminare.

  Quando ci si connette all'origine, l'utente può ora selezionare database/schemi di interesse. Se si selezionano solo gli schemi di cui si intende eseguire la migrazione, si risparmia tempo durante la connessione iniziale e si migliorano le prestazioni complessive del SSMA.

  ![Oggetti filtro SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La versione v 7.10 di SSMA per DB2 contiene le modifiche seguenti:

* Correzioni mirate progettate per fornire protezione aggiuntiva per la sicurezza e la privacy per soddisfare le modifiche nei requisiti globali.
* Correzione per la conversione dei `BEGIN-END` blocchi.

## <a name="ssma-v79"></a>SSMA v 7.9

La versione v 7.9 di SSMA per DB2 contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* Supporto nella riga di comando di SSMA per modificare il mapping dei tipi di dati e le preferenze di progetto.
* Supporto per la migrazione dei dati tramite SQL Server Integration Services (SSIS). Dopo la conversione dello schema, è possibile creare un pacchetto SSIS utilizzando l'opzione del menu di scelta rapida.
* La finestra di dialogo di connessione del database SQL di Azure in SSMA è stata anche modificata per specificare il nome completo del server. Nelle versioni precedenti di SSMA, il prefisso del database SQL di Azure doveva essere indicato in modo esplicito all'interno delle impostazioni dei progetti.

## <a name="ssma-v78"></a>SSMA v 7.8

La versione v 7.8 di SSMA per DB2 contiene le modifiche seguenti:

* Modificare il mapping dei tipi evidenziato nelle *impostazioni del progetto*.
* Possibilità per gli utenti di disabilitare la telemetria.

## <a name="ssma-v77"></a>SSMA v 7.7

La versione v 7.7 di SSMA per DB2 contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* In base alla richiesta più diffusa, la versione a 32 bit di SSMA per DB2 è di nuovo. Rispetto all'implementazione precedente (precedente alla versione 7.4), sono disponibili due pacchetti di installazione, ma non possono essere installati side-by-side. Di conseguenza, è necessario scegliere la versione più appropriata in base ai componenti di connettività disponibili. È sempre preferibile usare la versione a 64 bit, se possibile.

## <a name="ssma-v76"></a>SSMA v 7.6

La versione 7.6 di SSMA per DB2 è stata migliorata con correzioni mirate che migliorano le metriche di qualità e conversione e con il supporto per SQL Server 2017 (anteprima pubblica). Il supporto per SQL Server 2017 in Windows e Linux è in versione di anteprima pubblica e non deve essere usato per le migrazioni di produzione.

## <a name="ssma-v75"></a>SSMA v 7.5

La versione v 7.5 di SSMA per DB2 è stata migliorata con diversi miglioramenti per garantire una maggiore accessibilità per le persone affette da disabilità.

## <a name="ssma-v74"></a>SSMA v 7.4

La versione v 7.4 di SSMA per DB2 contiene le modifiche seguenti:

* L'opzione **query timeout** è ora disponibile durante l'individuazione di oggetti dello schema all'origine e alla destinazione.

  ![query timeout-opzione](../media/query-timeout_red.png)

* La metrica relativa alla qualità e alla conversione è stata migliorata con correzioni mirate, basate sui suggerimenti dei clienti.

  > [!IMPORTANT]
  > .NET 4.5.2 è un prerequisito per l'installazione di SSMA v 7.4. Inoltre, a partire da v 7.4, la versione a 32 bit di SSMA è stata sospesa.

## <a name="ssma-v73"></a>SSMA v 7.3

La versione v 7.3 di SSMA per DB2 contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* SSMA Extensibility Framework esposto tramite gli elementi seguenti:
  * Esporta la funzionalità in un progetto di SQL Server Data Tools (SSDT).
    * È ora possibile esportare gli script dello schema da SSMA a un progetto SSDT. È possibile utilizzare gli script dello schema per apportare ulteriori modifiche allo schema e distribuire il database.

      ![Comando Salva come progetto SSDT](../media/export-schema-scripts_red.png)
  * Librerie che possono essere utilizzate da SSMA per l'esecuzione di conversioni personalizzate.
    * È ora possibile costruire codice in grado di gestire le conversioni e le conversioni di sintassi personalizzate che non sono state precedentemente gestite da SSMA.
      * In questo post di Blog sono disponibili istruzioni per la creazione di un convertitore personalizzato, che [estende le funzionalità di conversione di SQL Server Migration Assistant](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/).
      * Scaricare un progetto di esempio per la conversione da questo [post di Blog](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v 7.2

La versione 7.2 di SSMA per DB2 contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* Miglioramenti della telemetria per fornire punti dati migliori per la risoluzione dei problemi dei clienti e per migliorare i tassi di conversione di SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versione v 7.1 di SSMA per DB2 contiene le modifiche seguenti:

* SQL Server 2017 in Windows e Linux CTP1 è ora una piattaforma di destinazione supportata per la migrazione. Questa funzionalità è in anteprima tecnica e consente lo spostamento dello schema e dei dati per i server SQL di destinazione.
* Supporto per aggiornamenti automatici per scaricare la versione più recente di SSMA non appena disponibile.
* I file binari installabili SSMA vengono ora recapitati tramite Windows Installer file di pacchetto (con estensione msi).

## <a name="may-2016"></a>Maggio 2016

La versione 2016 di SSMA per DB2 contiene le seguenti modifiche:

* Aggiunta del supporto per SQL Server 2016.
* Aggiunta della conversione di tabelle regolari e in memoria di DB2 a SQL Server funzionalità in memoria e Hekaton.
* Aggiunta della conversione dei controlli di accesso DB2 a oggetti Criteri di SQL Server (Sicurezza a livello di riga per DB2).
* Aggiunta della conversione di tabelle DB2 con controllo delle versioni di sistema in SQL Server tabelle temporali.
* Migliorato parser e resolver DB2.
* Controllo del programma di installazione rimosso per .NET 2,0.
* Rimosso file \* dll non necessario dal programma di installazione di DB2.
* Correzione `save-project` `open-project` dei comandi e per la console di SSMA.
* Correzione `securepassword` del comando per la console SSMA.
* Correzione del conteggio degli oggetti per il caricamento iniziale.
* Correzione del bug nelle impostazioni globali.
  
## <a name="march-2016"></a>Marzo 2016

La versione di anteprima di marzo 2016 di SSMA per DB2 aggiunge il supporto per la migrazione a SQL Server 2016.

## <a name="january-2016"></a>Gennaio 2016

La versione di manutenzione di SSMA per DB2 di gennaio 2016 contiene le modifiche seguenti:
  
* Aggiunta del supporto per numerose funzioni standard.
* Correzione degli errori del parser DB2.
* Correzione del supporto per DB2 V9 zOS (RFC 5690920).
* Correzione degli errori di identificazione non risolta DB2 durante la conversione.
* Aggiunta voce di menu Visualizza log a SSMA (RFC 5706203).
* Aggiunta di dati di telemetria.
  
## <a name="november-2014"></a>Novembre 2014

La versione di novembre 2014 di SSMA per DB2 era la versione iniziale.
