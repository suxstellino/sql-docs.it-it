---
title: Novità di SSMA per Oracle (OracleToSQL) | Microsoft Docs
description: Scopri le modifiche apportate a SQL Server Migration Assistant (SSMA) per Oracle (OracleToSQL) per ogni versione.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f305ebb6-7393-4a43-abb3-6332b739d690
ms.author: alexiva
ms.openlocfilehash: 4dc76c57f7194c1ee96bd091d674b793dad29faa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081878"
---
# <a name="whats-new-in-ssma-for-oracle-oracletosql"></a>Novità di SSMA per Oracle (OracleToSQL)

Questo articolo elenca SQL Server Migration Assistant (SSMA) per le modifiche Oracle in ogni versione.

## <a name="ssma-v816"></a>SSMA v 8.16

La versione v 8.16 di SSMA per Oracle contiene le modifiche seguenti:

* Aggiungere il supporto per le colonne Identity
* Rimuovere il supporto per il parser legacy
* Correzione di un problema relativo agli oggetti che non si aggiornano dal database

## <a name="ssma-v815"></a>SSMA v 8.15

Oltre a diversi miglioramenti dell'accessibilità, la versione v 8.15 di SSMA per Oracle contiene le modifiche seguenti:

* Aggiungere un'impostazione per usare la specifica del tipo completo per `%type` `%rowtype` gli attributi e
* Rinnovare i report di valutazione per lavorare nei browser moderni
* Usare l'autorità fornita dal database per l'autenticazione Azure AD
* Migliorare la denominazione per le istruzioni caricate da file

## <a name="ssma-v814"></a>SSMA v 8.14

Oltre a diversi miglioramenti per garantire maggiore accessibilità per utenti con particolari esigenze, la versione 8.14 di SSMA per Oracle contiene le modifiche seguenti:

* Archivia la versione completa del server di origine/destinazione nei metadati del progetto (richiede l'aggiornamento del progetto)
* Usare i dizionari dati DBA per l'individuazione degli oggetti quando possibile
* Risolvere i problemi relativi a più parser ( `PIVOT` / `UNPIVOT` , `MERGE` , quotazione alternativa)
* Correzione della conversione di `INSERTING` / `DELETING` / `UPDATING` funzioni speciali nei trigger

## <a name="ssma-v813"></a>SSMA v 8.13

La versione v 8.13 di SSMA per Oracle contiene le modifiche seguenti:

* Correzione della conversione di `SQLCODE` e `SQLERRM` funzioni speciali nelle procedure locali
* Considerare i cast di tipo implicito durante la conversione di chiamate di funzione e routine
* Migliorare la registrazione per la stringa di connessione di origine per risolvere i problemi di connessione

## <a name="ssma-v812"></a>SSMA v 8.12

La versione v 8.12 di SSMA per Oracle contiene le modifiche seguenti:

* Supporto per le clausole di factoring delle sottoquery nelle `INSERT` / `UPDATE` / `MERGE` / `DELETE` istruzioni
* Messaggio di conversione per la `ON DELETE SET NULL` clausola nei casi di riferimenti a percorsi multipli o circolari
* Conversione migliorata dei cursori creati dalla stringa SQL dinamica
* Aggiornare ODP.NET a v 19,8

## <a name="ssma-v811"></a>SSMA v 8.11

La versione v 8.11 di SSMA per Oracle contiene le modifiche seguenti:

* Supporto per sottoquery nell' `INSERT ... VALUES` istruzione
* Conversione dell' `COMMIT` istruzione migliorata
* Correzione della conversione della clausola bug nella `CONNECT BY LEVEL` clausola
* La logica di ripristino degli errori del parser è stata aggiornata per essere meno greedy
* Usare la libreria MSAL.NET per l'autenticazione Azure Active Directory interattiva

## <a name="ssma-v810"></a>SSMA v 8.10

La versione v 8.10 di SSMA per Oracle contiene piccoli miglioramenti delle prestazioni e le modifiche seguenti:

* Correzione per il problema del tester con le tabelle organizzate dall'indice
* Correzione per i nomi delle stored procedure estese nel pacchetto di estensione

## <a name="ssma-v89"></a>SSMA v 8.9

La versione v 8.9 di SSMA per Oracle contiene le modifiche seguenti:

* Conversione di valori letterali stringa SQL dinamici
* Conversione per `LAG` `FIRST_VALUE` e `LAST_VALUE` funzioni analitiche
* Aggiunta del supporto per DDL di base `ALTER TRIGGER` / `ALTER INDEX` (abilitazione/disabilitazione e così via)
* Conversione migliorata per le colonne che corrispondono ai nomi di funzione predefiniti
* Genera indici univoci filtrati per le colonne che possono essere filtrate `NULL`
* Conversione della dichiarazione di variabile migliorata per Azure sinapsi Analytics
* Correzione del problema con i caratteri speciali nel nome del progetto

## <a name="ssma-v88"></a>SSMA v 8.8

La versione v 8.8 di SSMA per Oracle include:

* Miglioramenti alla stabilità della sincronizzazione degli oggetti SQL Server
* Miglioramenti delle prestazioni dell'interfaccia utente grafica durante valutazione e conversione
* Conversione migliorata delle `OVER PARTITION` clausole analitiche
* Nuova conversione per la `LEAD` funzione analitica
* Nuova conversione per le clausole di factoring delle sottoquery
* Nuova `REPLICATE` opzione di distribuzione per Azure sinapsi Analytics
* Nuovo parser della sintassi Oracle per migliorare ulteriormente le prestazioni di conversione

## <a name="ssma-v87"></a>SSMA v 8,7

La versione v 8.7 di SSMA per Oracle presenta correzioni minime e miglioramenti delle prestazioni nell'interfaccia utente grafica.

Inoltre, SSMA per Oracle consente ora di filtrare gli oggetti in base allo stato di validità nella finestra di dialogo "selezione avanzata oggetti".

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Oltre a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni, la versione v 8.6 di SSMA per Oracle è stata migliorata aggiungendo un'impostazione che consente agli utenti di omettere le proprietà estese di SSMA nel codice convertito.

Per sfruttare questa impostazione, in SSMA per Oracle passare a **strumenti**  >  **Impostazioni progetto**  >    >  **conversione** generale, quindi in **varie** aggiornare il valore dell'opzione **omette proprietà estese** su **Sì**.

![Omettere l'impostazione delle proprietà estese](../oracle/media/ssma-omit-extended-properties.png)

Inoltre, SSMA per Oracle offre ora un'analisi migliorata della `XMLTABLE` clausola.

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versione v 8.5 di SSMA per Oracle è stata migliorata con il supporto per l'autenticazione Azure Active Directory e il supporto di base per le funzionalità JSON in SQL Server, insieme a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni.

Inoltre, SSMA per Oracle è stato migliorato con il supporto per:

* Limitazione del numero di oggetti selezionati per l'individuazione a 990 (il `WHERE .. IN (..)` limite delle clausole di Oracle è 1000 elementi).
* Migrazione dei dati da `RAW` a `UNIQUEIDENTIFIER` .
* Analisi della `PARALLEL_ENABLE` clausola.

Infine, la versione 8.5 di SSMA per Oracle offre ora:

* Miglioramento delle prestazioni delle costanti convertite in pacchetto.
* Aggiornamento per Oracle provider di dati per .NET alla versione 19.5.0.

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versione v 8.4 di SSMA per Oracle è stata migliorata con correzioni mirate progettate per risolvere i problemi di accessibilità e correggere un bug correlato alle colonne di indice Max (per consentire 32 anziché 16) per SQL Server 2016 e versioni successive.

Inoltre, questa versione di SSMA per Oracle aggiunge la conversione per `SYS_REFCURSOR` come `OUT` parametri stored procedure.

> [!IMPORTANT]
> Con SSMA versioni da 7,4 a 8,4, .NET 4.5.2 è un prerequisito di installazione.

## <a name="ssma-v83"></a>SSMA v 8.3

La versione 8.3 di SSMA per Oracle è stata migliorata con correzioni mirate progettate per migliorare le metriche di qualità e conversione. Inoltre, questa versione di SSMA per Oracle fornisce le correzioni seguenti:

* Risolvere i problemi di accessibilità.
* Aggiungere il supporto di base per il `hierarchyid` tipo in SQL Server.
* Risolvere un problema con un tipo restituito sconosciuto per una funzione chiamata tramite il sinonimo.
* Aggiornare ODP.NET a v 19,3.

## <a name="ssma-v82"></a>SSMA v 8.2

La versione v 8.2 di SSMA per Oracle è stata migliorata per:

* Aggiungere il supporto per `DBMS_OUTPUT.ENABLE` / `DISABLE` .
* Rimuovere `CAST AS FLOAT` per `BINARY_FLOAT` le `BINARY_DOUBLE` colonne e nella query di migrazione dei dati predefinita.
* Correggi sequenze Aggiorna se il valore corrente è stato modificato.
* Correzione del bug relativo all'errata interpretazione delle pseudo-colonne ( `ROWNUM` e così via) se esiste una colonna con lo stesso nome.
* Correzione di un arresto anomalo che si verifica durante la conversione `FOR` di cicli con un identificatore non risolto ambiguo.

Inoltre, questa versione include un set di correzioni mirato progettato per migliorare la qualità e la metrica di conversione, nonché le correzioni per:

* Un problema con gli indici non cluster disabilitati dopo la migrazione dei dati.
* Rilevamento dei .NET Framework durante l'installazione invisibile all'utente.
* Arresti anomali intermittenti che si verifica quando viene scaricata una nuova versione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.1 a v 8.2. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versione v 8.1 di SSMA per Oracle è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.0 a v 8.1. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versione v 8.0 di SSMA per Oracle è stata migliorata con correzioni mirate progettate per migliorare la qualità e la metrica di conversione. Questa versione offre anche le seguenti nuove funzionalità:

* Supporto per **istanza gestita SQL di Azure** come destinazione. È ora possibile creare nuovi progetti destinati a Istanza gestita SQL di Azure:

  ![Progetto SQL MI](../media/ssma-newproject-sqldbmi.png)

  > [!NOTE]
  > SSMA per Oracle Extension Pack è stato aggiornato anche per consentire installazioni remote in Azure SQL Istanza gestita:
  >
  > ![SSMA per Oracle Extension Pack](../media/ssma-oracle-ext-pack.png)

  Alcune funzionalità, tra cui il tester e la migrazione dei dati sul lato server, non sono supportate quando la destinazione è la Istanza gestita di Azure SQL. Per altre informazioni, leggere [qui](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/migrate-your-oracle-database-to-azure-sql-database-managed-instance-using-ssma-8-0/).

* **Avviso di correzione** post-conversione. Altre informazioni sono disponibili [qui](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selezione dello schema o del database preliminare.

  Quando ci si connette all'origine, l'utente può ora selezionare database/schemi di interesse. Se si selezionano solo gli schemi di cui si intende eseguire la migrazione, si risparmia tempo durante la connessione iniziale e si migliorano le prestazioni complessive del SSMA.

  ![Oggetti filtro SSMA](../media/ssma-filter-objects.png)

* La possibilità di utilizzare il driver .NET gestito ufficiale per connettersi a Oracle. Il driver OCI non è più un prerequisito per l'utilizzo di SQL Server Migration Assistant per Oracle.

* Possibilità di eseguire il `ROWID` mapping `UROWID` di e a per `VARCHAR` impostazione predefinita. Modificato da `uniqueidentifier` per gestire la migrazione dei dati per le colonne esplicite `ROWID` .

## <a name="ssma-v710"></a>SSMA v 7.10

La versione v 7.10 di SSMA per Oracle contiene le modifiche seguenti:

* Correzioni mirate progettate per fornire protezione aggiuntiva per la sicurezza e la privacy per soddisfare le modifiche nei requisiti globali.
* Miglioramento della conversione correlato alle query gerarchiche.

## <a name="ssma-v79"></a>SSMA v 7.9

La versione v 7.9 di SSMA per Oracle contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* Supporto per la migrazione di istruzioni "continue" da Oracle a SQL Server.
* Supporto nella riga di comando di SSMA per modificare il mapping dei tipi di dati e le preferenze di progetto.
* Supporto per la migrazione dei dati tramite SQL Server Integration Services (SSIS). Dopo la conversione dello schema, è possibile creare un pacchetto SSIS utilizzando l'opzione del menu di scelta rapida.
* La finestra di dialogo di connessione del database SQL di Azure in SSMA è stata anche modificata per specificare il nome completo del server. Nelle versioni precedenti di SSMA, il prefisso del database SQL di Azure doveva essere indicato in modo esplicito all'interno delle impostazioni dei progetti.

## <a name="ssma-v78"></a>SSMA v 7.8

La versione v 7.8 di SSMA per Oracle contiene le modifiche seguenti:

* Supporto per:
  * Espressione di riga per la `IN` clausola.
  * Cast di tipo implicito.
  * `UID` conversione per il database SQL di Azure.
* Modificare il mapping dei tipi evidenziato nelle **impostazioni del progetto**.
* Possibilità per gli utenti di disabilitare la telemetria.

## <a name="ssma-v77"></a>SSMA v 7.7

La versione v 7.7 di SSMA per Oracle contiene le modifiche seguenti:

* SSMA per Oracle è stato migliorato con correzioni mirate che migliorano la qualità e la metrica di conversione.
* In base alla richiesta più diffusa, la versione a 32 bit di SSMA per Oracle è di nuovo. Rispetto all'implementazione precedente (prima della versione 7.4), sono disponibili due pacchetti di installazione, ma non possono essere installati side-by-side. Di conseguenza, è necessario scegliere la versione più appropriata in base ai componenti di connettività disponibili. È sempre preferibile usare la versione a 64 bit, se possibile.
* Il supporto di SQL Server 2017 è ora ufficiale con Oracle Extension Pack supportato anche in Linux (nuova opzione di installazione remota). Si noti che la funzionalità del pacchetto di estensione è limitata se installata in Linux, perché le funzionalità di migrazione dei dati del tester e lato server non sono supportate.
* SSMA per Oracle consente di eseguire la migrazione delle viste materializzate come tabelle regolari (configurabili tramite le impostazioni nella sincronizzazione **delle impostazioni di progetto**  ->    ->  **individuazione di tabelle di backup per le viste materializzate**).

## <a name="ssma-v76"></a>SSMA v 7.6

La versione 7.6 di SSMA per Oracle è stata migliorata con correzioni mirate che migliorano le metriche di qualità e conversione e con il supporto per SQL Server 2017 (anteprima pubblica). Il supporto per SQL Server 2017 in Windows e Linux è in versione di anteprima pubblica e non deve essere usato per le migrazioni di produzione.

## <a name="ssma-v75"></a>SSMA v 7.5

La versione v 7.5 di SSMA per Oracle contiene le modifiche seguenti:

* Migliorato con diversi miglioramenti per garantire maggiore accessibilità per utenti con particolari esigenze.
* Aggiornamento per migliorare la metrica di qualità e conversione con correzioni mirate, ad esempio la gestione migliorata dei tipi di dati date e float durante la migrazione dei dati, in base ai suggerimenti dei clienti.

## <a name="ssma-v74"></a>SSMA v 7.4

La versione v 7.4 di SSMA per Oracle contiene le modifiche seguenti:

* SSMA per Oracle supporta ora Azure sinapsi Analytics come piattaforma di destinazione per la migrazione.

  ![Finestra Nuovo progetto](../media/new-project.png)
  * Supporta le opzioni di archiviazione data warehouse come illustrato nell'immagine seguente:

  ![opzioni di archiviazione per data warehouse](../media/storage-options_red.png)
  * Supporta le opzioni di distribuzione dei dati come illustrato nell'immagine seguente:

  ![distribuzione dei dati per data warehouse](../media/data-distribution_red.png)

* L'opzione **query timeout** è ora disponibile durante l'individuazione di oggetti dello schema all'origine e alla destinazione.

  ![query timeout-opzione](../media/query-timeout_red.png)

* La metrica relativa alla qualità e alla conversione è stata migliorata con correzioni mirate, basate sui suggerimenti dei clienti.

> [!IMPORTANT]
> .NET 4.5.2 è un prerequisito per l'installazione di SSMA v 7.4. Inoltre, a partire da v 7.4, la versione a 32 bit di SSMA viene sospesa.

## <a name="ssma-v73"></a>SSMA v 7.3

La versione v 7.3 di SSMA per Oracle contiene le modifiche seguenti:

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

La versione 7.2 di SSMA per Oracle contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* Miglioramenti della telemetria per fornire punti dati migliori per la risoluzione dei problemi dei clienti e per migliorare i tassi di conversione di SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versione v 7.1 di SSMA per Oracle contiene le modifiche seguenti:

* SQL Server 2017 in Windows e Linux CTP1 è ora una piattaforma di destinazione supportata per la migrazione. Questa funzionalità è in anteprima tecnica e consente lo spostamento dello schema e dei dati per i server SQL di destinazione.
* SSMA supporta ora gli aggiornamenti automatici per scaricare la versione più recente di SSMA non appena disponibile.
* I file binari installabili SSMA vengono ora recapitati tramite Windows Installer file di pacchetto (con estensione msi).

## <a name="may-2016"></a>Maggio 2016

La versione 2016 di SSMA per Oracle contiene le seguenti modifiche:

* Aggiunta del supporto per SQL Server 2016.
* Aggiunta della conversione delle tabelle di archivio di Oracle flashback a SQL Server tabelle temporali.

  > [!NOTE]
  > SSMA non copia i dati di cronologia dalle tabelle di archivio dati di Oracle flashback. Di conseguenza, i dati della cronologia devono essere copiati manualmente durante il processo di migrazione. Inoltre, mentre SSMA non Visualizza la tabella di cronologia in Esplora metadati di SQL Server perché viene considerata come una tabella di sistema, è possibile visualizzare la tabella di cronologia in SQL Server Management Studio.
  >
  > SQL Server 2016 non supporta diverse funzionalità del flashback Oracle, tra cui:
  >
  >   * Query di transazione del flashback Oracle
  >   * `DBMS_FLASHBACK` Pacchetto
  >   * Transazione flashback
  >   * Archivio dati flashback
  >   * Tabella flashback
  >   * Rilascio flashback
  >   * Database flashback

* Aggiunta della conversione del criterio Oracle Vital a oggetti Criteri di SQL Server (Sicurezza a livello di riga per Oracle).
* Riduzione del tempo di caricamento iniziale per Oracle.
* Migliorato parser e resolver.
* Controllo del programma di installazione rimosso per .NET 2,0.
* La dipendenza del pacchetto di estensione è stata aggiornata da .NET 3,5 a .NET 4,0.
* Correzione `save-project` `open-project` dei comandi e per la console di SSMA.
* Correzione `securepassword` del comando per la console SSMA.
* Correzione del conteggio degli oggetti per il caricamento iniziale.
* Correzione della conversione dei tipi di dati carattere per Oracle.
* Correzione del bug nelle impostazioni globali.

## <a name="march-2016"></a>Marzo 2016

La versione di anteprima 2016 di SSMA per Oracle ha aggiunto il supporto per:

* Migrazione a SQL Server 2016.
* Migrazione di Sicurezza a livello di riga Oracle (con alcune limitazioni).
* Eseguire la migrazione di tabelle Oracle in memoria a SQL Server archivio colonne.

## <a name="january-2016"></a>Gennaio 2016

La versione di manutenzione di SSMA per Oracle di gennaio 2014 contiene le modifiche seguenti:

* Aggiunta del supporto per gli indici cluster.
* Correzione delle query dello schema Oracle lente (RFC 5076207).
* Correzione della connessione ad Azure dalla console.
* Aggiunta voce di menu Visualizza log a SSMA (RFC 5706203).
* Aggiunta di dati di telemetria.

## <a name="july-2014"></a>Luglio 2014

La versione luglio 2014 di SSMA per Oracle contiene le modifiche seguenti:

* Aggiunta del supporto per il database SQL di Azure.
* Funzionalità del pacchetto di estensione spostata nello schema per supportare il database SQL di Azure.
* Aggiunta del supporto per le viste materializzate Oracle.
* Aggiunta del supporto per le tabelle con ottimizzazione per la memoria SQL Server 2014.
* Miglioramenti delle prestazioni inclusi testati per i database con oltre 10.000 oggetti.
* Aggiunta di miglioramenti dell'interfaccia utente per la gestione di un numero elevato di oggetti.
* Aggiunta dell'evidenziazione degli schemi LOB noti.
* Miglioramenti della velocità di conversione inclusi.
* Aggiunta del supporto per la visualizzazione dei conteggi degli oggetti nell'interfaccia utente.
* Dimensioni del report ridotte di oltre il 25%.
* Messaggi di errore migliorati per costrutti non analizzati.

## <a name="april-2014"></a>Aprile 2014

La versione aprile 2014 di SSMA per Oracle contiene le modifiche seguenti:

* Aggiunta del supporto per MS SQL Server 2014.
* Aggiunta del supporto per Oracle 12 e l'ottimizzazione delle query.
* Correzione dei bug relativi alla conversione in Azure.
* Correzione dei bug relativi alle pagine del report invisibile in IE 10.

## <a name="january-2012"></a>gennaio 2012

La versione di gennaio 2012 di SSMA per Oracle aggiunge il supporto per i `RowType` `RecordType` parametri di input e impostati come predefiniti `NULL` .

## <a name="july-2011"></a>luglio 2011

La versione luglio 2011 di SSMA per Oracle contiene le modifiche seguenti:

* Aggiunta del supporto per la conversione di una sequenza Oracle in [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] Sequence Generator.
* Segnalazione errori migliorata durante la migrazione dei dati.
* Conversione migliorata dell'istruzione mediante parole riservate.
* Conversione implicita migliorata del valore di data in una funzione.

## <a name="april-2011"></a>Aprile 2011

La versione aprile 2011 di SSMA per Oracle contiene le modifiche seguenti:

* Prodotto consolidato "SSMA per Oracle", che supporta [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] e [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Aggiunta del supporto per la connessione e la migrazione a [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Motore di migrazione dei dati avanzato sul lato client, che supporta la migrazione parallela dei dati.
* Miglioramento delle prestazioni di migrazione dei dati con i `Simple` `Bulk` modelli di recupero con registrazione.
* Aggiunta del supporto per la compatibilità con le versioni precedenti dei progetti creati da versioni precedenti di SSMA (v 4.0 e v 4.2).
* Aggiunta della funzionalità di installazione side-by-side di SSMA per il prodotto Oracle v 5.0 (SxS) con le versioni precedenti di SSMA (v 4.0 e v 4.2).
* Aggiunto il supporto per la creazione di report User-Defined tipi (include sottotipo, `VARRAY` , `NESTED TABLE` , tabella oggetti e visualizzazione oggetti) e i relativi utilizzi nei blocchi PL/SQL con messaggi di errore speciali.

## <a name="july-2010"></a>Luglio 2010

È stata aggiunta la versione 2010 di SSMA per Oracle:

* Supporto per la migrazione a SQL Server 2008 R2.
* Nuova applicazione console SSMA per l'esecuzione da riga di comando.
* Supporto per la migrazione dei dati tramite i motori di migrazione dei dati sia Server-Side che Client-Side.
* Supporto per l'istruzione "Custom SELECT" nella migrazione dei dati.
* Supporto per la migrazione da Oracle 11g R2.

## <a name="june-2008"></a>2008 giugno

La versione del 2008 giugno di SSMA per Oracle contiene le modifiche seguenti:

* Aggiunta di miglioramenti al report di valutazione, incluse informazioni aggiuntive per i sinonimi, origine non elaborata per gli oggetti analizzabili, pannelli e SQL Server rimozione del logo e persistenza del layout.
* Aggiunta di miglioramenti nella conversione degli oggetti:
  * Pacchetti `DBMS_LOB` , `DBMS_SQL` conversione aggiunta.
  * La conversione di join è stata modificata.
  * Modifica della conversione di raccolte e record, ora conversione di record in semplici casi rilasciati tramite variabili separate per ogni campo.
  * Miglioramenti dell'implementazione dei record e delle raccolte.
  * Aggiunta di funzioni di aggregazione finestra.
  * `ROLLUP`/`CUBE` Aggiunta della clausola.
  * Miglioramento per `NEXTVAL` / `CURVAL` .
  * `SET`Sono state aggiunte le colonne raggruppamento in clausola, GROUPING SETS e ID raggruppamento.
  * `MERGE` Aggiunta istruzione.
  * Supporto dei nuovi tipi DateTime e della conversione di record e raccolte come tipi di dati CLR aggiunti.
* Sono state aggiunte nuove funzionalità di tester. È ora possibile testare le tabelle come oggetti usando il tester, un ordine di chiamata di diversi oggetti testabili in test case può essere modificato, l'utente può testare routine e funzioni con record e raccolte come parametri e valori restituiti e un analizzatore delle dipendenze è stato aggiunto per controllare solo le tabelle utilizzate.
  
## <a name="august-2007"></a>2007 agosto

È stata aggiunta la versione agosto 2007 di SSMA per Oracle:

* Un nuovo componente tester consente di creare, gestire ed eseguire test case per verificare il codice SQL convertito.
* Al convertitore SQL è stato aggiunto il supporto per la conversione di sottotipi, raccolte e moduli locali Oracle.
* Una nuova funzionalità di sincronizzazione consente di sincronizzare oggetti specifici con SQL Server database.
* Nuove opzioni di conversione.
  
## <a name="april-2007"></a>Aprile 2007

La versione 2007 di SSMA per Oracle è stata la versione iniziale.
