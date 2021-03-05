---
title: Novità di SSMA per MySQL (MySQLToSql) | Microsoft Docs
description: Scopri le modifiche apportate a SQL Server Migration Assistant (SSMA) per MySQL (MySQLToSQL) per ogni versione.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1451a0b0-6713-4d0c-954f-ea3d8fce1d31
ms.author: alexiva
ms.openlocfilehash: 138cb90558bfb73d0b9e0cd3ba75a7a41fc84229
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186457"
---
# <a name="whats-new-in-ssma-for-mysql-mysqltosql"></a>Novità di SSMA per MySQL (MySQLToSql)

Questo articolo elenca SQL Server Migration Assistant (SSMA) per le modifiche di MySQL in ogni versione.

## <a name="ssma-v817"></a>SSMA v 8.17

La versione v 8.17 di SSMA per MySQL contiene le modifiche seguenti:

* Aggiornare i report di valutazione HTML per utilizzare l'editor moderno per visualizzare il testo SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La versione v 8.16 di SSMA per MySQL contiene le modifiche seguenti:

* Aggiungere il supporto per le colonne calcolate
* Correzione di problemi durante `INSERT` la conversione di un'istruzione per le tabelle con vincoli univoci e chiavi primarie
* Aggiornare il parser rispetto `ANSI_QUOTES` alle `NO_BACKSLASH_ESCAPES` modalità server
* Rimuovere il supporto per il parser legacy
* Correzione di un problema relativo agli oggetti che non si aggiornano dal database

## <a name="ssma-v815"></a>SSMA v 8.15

Oltre a diversi miglioramenti dell'accessibilità, la versione v 8.15 di SSMA per MySQL contiene le modifiche seguenti:

* Rinnovare i report di valutazione per lavorare nei browser moderni
* Usare l'autorità fornita dal database per l'autenticazione Azure AD
* Migliorare la denominazione per le istruzioni caricate da file

## <a name="ssma-v814"></a>SSMA v 8.14

Oltre a diversi miglioramenti per garantire maggiore accessibilità per gli utenti con particolari esigenze, la versione 8.14 di SSMA per MySQL richiede un aggiornamento del progetto, perché archivia ora la versione completa del server di origine/destinazione nei metadati del progetto.

## <a name="ssma-v813"></a>SSMA v 8.13

La versione v 8.13 di SSMA per MySQL contiene le modifiche seguenti:

* Considerare i cast di tipo implicito durante la conversione di chiamate di funzione e routine
* Migliorare la registrazione per la stringa di connessione di origine per risolvere i problemi di connessione

## <a name="ssma-v812"></a>SSMA v 8.12

La versione v 8.12 di SSMA per MySQL contiene le modifiche seguenti:

* Conversione di tabelle temporanee DDL

## <a name="ssma-v811"></a>SSMA v 8.11

La versione v 8.11 di SSMA per MySQL contiene le modifiche seguenti:

* Usare la libreria MSAL.NET per l'autenticazione Azure Active Directory interattiva

## <a name="ssma-v810"></a>SSMA v 8.10

La versione v 8.10 di SSMA per MySQL contiene piccoli miglioramenti delle prestazioni e correzioni di bug.

## <a name="ssma-v89"></a>SSMA v 8.9

La versione v 8.9 di SSMA per MySQL contiene le modifiche seguenti:

* Correzione per la migrazione dei dati di tipi spaziali
* Correzione del problema con i caratteri speciali nel nome del progetto

## <a name="ssma-v88"></a>SSMA v 8.8

La versione v 8.8 di SSMA per MySQL include:

* Miglioramenti alla stabilità della sincronizzazione degli oggetti SQL Server
* Miglioramenti delle prestazioni dell'interfaccia utente grafica durante valutazione e conversione

## <a name="ssma-v87"></a>SSMA v 8,7

La versione v 8.7 di SSMA per MySQL presenta correzioni minime e miglioramenti delle prestazioni nell'interfaccia utente grafica.

Inoltre, SSMA per MySQL ora fornisce la clausola di conversione per `LIMIT` quando la destinazione è SQL Azure.

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Oltre a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni, la versione 8.6 di SSMA per MySQL è stata migliorata aggiungendo un'impostazione che consente agli utenti di omettere le proprietà estese di SSMA nel codice convertito.

Per sfruttare questa impostazione, in SSMA per MySQL passare a **strumenti**  >  **Impostazioni progetto**  >    >  **conversione** generale, quindi in **varie** aggiornare il valore dell'opzione **omette proprietà estese** su **Sì**.

![Omettere l'impostazione delle proprietà estese](../mysql/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versione v 8.5 di SSMA per MySQL è stata migliorata con il supporto per l'autenticazione Azure Active Directory e il supporto di base per le funzionalità JSON in SQL Server, insieme a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni.

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versione v 8.4 di SSMA per MySQL è stata migliorata con correzioni mirate progettate per risolvere i problemi di accessibilità e correggere un bug correlato alle colonne di indice Max (per consentire 32 anziché 16) per SQL Server 2016 e versioni successive.

> [!IMPORTANT]
> Con SSMA versioni 7,4, sebbene 8,4, .NET 4.5.2 è un prerequisito di installazione.

## <a name="ssma-v83"></a>SSMA v 8.3

La versione 8.3 di SSMA per MySQL è stata migliorata con correzioni mirate progettate per migliorare le metriche di qualità e conversione. Inoltre, questa versione di SSMA per MySQL fornisce le correzioni seguenti:

* Risolvere i problemi di accessibilità.
* Aggiungere il supporto di base per il `hierarchyid` tipo in SQL Server.

## <a name="ssma-v82"></a>SSMA v 8.2

La versione v 8.2 di SSMA per MySQL è stata migliorata con un set di correzioni mirato progettato per migliorare le metriche di qualità e conversione, nonché le correzioni per:

* Un problema con gli indici non cluster disabilitati dopo la migrazione dei dati.
* Rilevamento dei .NET Framework durante l'installazione invisibile all'utente.
* Arresti anomali intermittenti che si verifica quando viene scaricata una nuova versione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.1 a v 8.2. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versione v 8.1 di SSMA per MySQL è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.0 a v 8.1. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versione v 8.0 di SSMA per MySQL è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione. Questa versione offre anche le seguenti nuove funzionalità:

* Supporto per **istanza gestita SQL di Azure** come destinazione. È ora possibile creare nuovi progetti destinati a Istanza gestita SQL di Azure:

  ![Progetto SQL MI](../media/ssma-newproject-sqldbmi.png)

* **Avviso di correzione** post-conversione. Altre informazioni sono disponibili [qui](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selezione dello schema o del database preliminare.

  Quando ci si connette all'origine, l'utente può ora selezionare database/schemi di interesse. Se si selezionano solo gli schemi di cui si intende eseguire la migrazione, si risparmia tempo durante la connessione iniziale e si migliorano le prestazioni complessive del SSMA.

  ![Oggetti filtro SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La versione v 7.10 di SSMA per MySQL contiene le modifiche seguenti:

* Correzioni mirate progettate per fornire protezione aggiuntiva per la sicurezza e la privacy per soddisfare le modifiche nei requisiti globali.
* Correzione per la conversione di spazi tra il nome della funzione e l'elenco di argomenti.

## <a name="ssma-v79"></a>SSMA v 7.9

La versione v 7.9 di SSMA per MySQL contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* Supporto parziale per la migrazione di tipi di dati spaziali da MySQL al database SQL di Azure.
* Supporto nella riga di comando di SSMA per modificare il mapping dei tipi di dati e le preferenze di progetto.
* Supporto per la migrazione dei dati tramite SQL Server Integration Services (SSIS). Dopo la conversione dello schema, è possibile creare un pacchetto SSIS utilizzando l'opzione del menu di scelta rapida.
* La finestra di dialogo di connessione del database SQL di Azure in SSMA è stata anche modificata per specificare il nome completo del server. Nelle versioni precedenti di SSMA, il prefisso del database SQL di Azure doveva essere indicato in modo esplicito all'interno delle impostazioni dei progetti.

## <a name="ssma-v78"></a>SSMA v 7.8

La versione v 7.8 di SSMA per MySQL contiene le modifiche seguenti:

* Modificare il mapping dei tipi evidenziato nelle impostazioni del progetto.
* Possibilità per gli utenti di disabilitare la telemetria.

## <a name="ssma-v77"></a>SSMA v 7.7

La versione v 7.7 di SSMA per MySQL contiene le modifiche seguenti:

* SSMA per MySQL è stato migliorato con correzioni mirate che migliorano la qualità e la metrica di conversione.
* In base alla richiesta più diffusa, la versione a 32 bit di SSMA per MySQL è di nuovo. Rispetto all'implementazione precedente (prima della versione 7.4), sono disponibili due pacchetti di installazione, ma non possono essere installati side-by-side. Di conseguenza, è necessario scegliere la versione più appropriata in base ai componenti di connettività disponibili. È sempre preferibile usare la versione a 64 bit, se possibile.
* SSMA per MySQL dispone ora della modalità di connessione della stringa di connessione ODBC, che consente di usare qualsiasi driver ODBC di terze parti compatibile con MySQL.

## <a name="ssma-v76"></a>SSMA v 7.6

La versione 7.6 di SSMA per MySQL è stata migliorata con correzioni mirate che migliorano le metriche di qualità e conversione e con il supporto per SQL Server 2017 (anteprima pubblica). Il supporto per SQL Server 2017 in Windows e Linux è in versione di anteprima pubblica e non deve essere usato per le migrazioni di produzione.

## <a name="ssma-v75"></a>SSMA v 7.5

La versione v 7.5 di SSMA per MySQL è stata migliorata con diversi miglioramenti per garantire una maggiore accessibilità per gli utenti con particolari esigenze.

## <a name="ssma-v74"></a>SSMA v 7.4

La versione v 7.4 di SSMA per MySQL contiene le modifiche seguenti:

* L'opzione **query timeout** è ora disponibile durante l'individuazione di oggetti dello schema all'origine e alla destinazione.

    ![query timeout-opzione](../media/query-timeout_red.png)
* La metrica relativa alla qualità e alla conversione è stata migliorata con correzioni mirate, basate sui suggerimenti dei clienti.

> [!IMPORTANT]
> .NET 4.5.2 è un prerequisito per l'installazione di SSMA v 7.4. Inoltre, a partire da v 7.4, la versione a 32 bit di SSMA viene sospesa.

## <a name="ssma-v73"></a>SSMA v 7.3

La versione v 7.3 di SSMA per MySQL contiene le modifiche seguenti:

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

La versione 7.2 di SSMA per MySQL contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* Miglioramenti della telemetria per fornire punti dati migliori per la risoluzione dei problemi dei clienti e per migliorare i tassi di conversione di SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versione v 7.1 di SSMA per MySQL contiene le modifiche seguenti:

* SQL Server 2017 in Windows e Linux CTP1 è ora una piattaforma di destinazione supportata per la migrazione. Questa funzionalità è in anteprima tecnica e consente lo spostamento dello schema e dei dati per i server SQL di destinazione.
* SSMA supporta ora gli aggiornamenti automatici per scaricare la versione più recente di SSMA non appena disponibile.
* I file binari installabili SSMA vengono ora recapitati tramite Windows Installer file di pacchetto (con estensione msi).

## <a name="may-2016"></a>Maggio 2016

La versione 2016 di SSMA per MySQL contiene le modifiche seguenti:

* Aggiunta del supporto per SQL Server 2016.
* Migliorato parser e resolver.
* Controllo del programma di installazione rimosso per .NET 2,0.
* La dipendenza del pacchetto di estensione è stata aggiornata da .NET 3,5 a .NET 4,0.
* Correzione del mapping dei tipi BigInt predefiniti per MySql.
* Correzione `save-project` `open-project` dei comandi e per la console di SSMA.
* Correzione `securepassword` del comando per la console SSMA.
* Correzione del conteggio degli oggetti per il caricamento iniziale.
* Caricamento degli oggetti MsSql corretti.
* Correzione del bug nelle impostazioni globali.

## <a name="march-2016"></a>Marzo 2016

La versione di anteprima di marzo 2016 di SSMA per MySQL aggiunge il supporto per la migrazione a SQL Server 2016.
  
## <a name="january-2016"></a>Gennaio 2016

La versione di manutenzione di SSMA per MySQL di gennaio 2016 contiene le modifiche seguenti:

* Aggiunta voce di menu Visualizza log a SSMA (RFC 5706203).
* Aggiunta di dati di telemetria.
  
## <a name="july-2014"></a>Luglio 2014

La versione luglio 2014 di SSMA per MySQL contiene le modifiche seguenti:
  
* Migliore conversione del codice del database SQL di Azure.
* Funzionalità del pacchetto di estensione spostata nello schema per supportare il database SQL di Azure.
* Miglioramenti delle prestazioni testati per i database con oltre 10.000 oggetti.
* Miglioramenti dell'interfaccia utente per la gestione di un numero elevato di oggetti.
* Evidenziazione degli schemi LOB noti, in modo che possano essere ignorati durante la conversione.
* Miglioramenti della velocità di conversione.
* Mostra i conteggi degli oggetti nell'interfaccia utente.
* Riduzione delle dimensioni del report superiore al 25%.
* Messaggi di errore migliorati per costrutti non analizzati.
  
## <a name="april-2014"></a>Aprile 2014

La versione April 2014 di SSMA per MySQL contiene le modifiche seguenti:
  
* Aggiunta del supporto per SQL Server 2014.
* Correzione dei bug relativi alla conversione in Azure.
* Correzione dei bug relativi alle pagine del report invisibile in IE 10.
  
## <a name="july-2011"></a>luglio 2011

La versione luglio 2011 di SSMA per MySQL contiene le modifiche seguenti:
  
* Supporto per la conversione di `LIMIT` in [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] `OFFSET` .
* Segnalazione errori migliorata durante la migrazione dei dati.
  
## <a name="april-2011"></a>Aprile 2011

La versione April 2011 di SSMA per MySQL contiene le modifiche seguenti:
  
* Singolo installabile di "SSMA per MySQL", che supporta [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] e SQL di Azure.
* Possibilità di connettersi [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Motore di migrazione dei dati avanzato sul lato client, che supporta la migrazione parallela dei dati.
* Miglioramento delle prestazioni di migrazione dei dati con modelli di recupero con registrazione minima
* SSMA per la versione della console MySQL supporta la compatibilità con le versioni precedenti. È possibile aprire i progetti creati dalle versioni precedenti a SSMA v 5.0.
* Il prodotto SSMA per MySQL v 5.0 può essere installato side-by-Side (SxS) con le versioni precedenti del prodotto SSMA.
  
## <a name="july-2010"></a>Luglio 2010

La versione July 2010 di SSMA per MySQL contiene le funzionalità seguenti:
  
**1. miglioramenti all'interfaccia utente:**

* Scheda ' modalità SQL ' per oggetti di database MySQL
* Scheda ' impostazioni ' per oggetti di database MySQL
* Scheda ' dati ' per le tabelle MySQL
* Impostazioni di progetto aggiornate nelle pagine di conversione e migrazione
* ' Impostazioni migrazione dati ' a livello di tabella
  
**2. miglioramenti per la connessione a MySQL e SQL Server:**
  
* Connettività SSL in MySQL
* Connettività crittografata in SQL Server
  
**3. miglioramenti apportati a MySQL Metabase Explorer:**
  
* Caricamento di tutti gli oggetti di database MySQL e delle rispettive schede.
  
**4. miglioramenti alla conversione degli oggetti:**
  
* Conversione di oggetti della metabase di MySQL: procedure, funzioni, viste, trigger e istruzioni.
* Supporto limitato per i tipi di dati spaziali nelle tabelle.
* Opzione per la conversione di funzioni MySQL in SQL Server stored procedure
* Opzione per applicare le modalità SQL e il mapping del set di caratteri durante la conversione dell'oggetto
  
**5. miglioramenti alla migrazione dei dati:**  
  
* Supporto per la migrazione dei dati tramite i motori di migrazione dei dati sia Server-Side che Client-Side
* Supporto per la migrazione dei dati spaziali
* SQL personalizzato per la migrazione dei dati per le tabelle
  
**6. SSMA per la console MySQL:**  
  
* Funzionalità della console di supporto per SSMA per MySQL  
* Supporto per Script-Level l'interazione  
  
## <a name="january-2010"></a>2010 gennaio

La versione di SSMA per MySQL di gennaio 2010 è stata la versione iniziale. Contiene le funzionalità seguenti:
  
* È stato aggiunto il supporto per la migrazione sia alla SQL Server locale che a SQL di Azure.  
  
* **Snapshot funzionalità:** Migrazione dello schema e dei dati di tabelle/indici/vincoli MySQL.
