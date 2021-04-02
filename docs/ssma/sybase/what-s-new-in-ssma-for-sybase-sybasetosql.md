---
title: Novità di SSMA per SAP ASE (SybaseToSQL) | Microsoft Docs
description: Informazioni sulle modifiche apportate a SQL Server Migration Assistant (SSMA) per Sybase (SybaseToSQL) per ogni versione.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2be0cf8d-6dbe-443a-abbd-036249922205
ms.author: alexiva
ms.openlocfilehash: 4732f2ca968788cb59efd14d9410929f99d9423f
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980893"
---
# <a name="whats-new-in-ssma-for-sap-ase-sybasetosql"></a>Novità di SSMA per SAP ASE (SybaseToSQL)

Questo articolo elenca SQL Server Migration Assistant (SSMA) per le modifiche di SAP ASE (in precedenza SSMA per Sybase) in ogni versione.

## <a name="ssma-v818"></a>SSMA v 8.18

La versione 8.18 di SSMA per SAP ASE contiene le modifiche seguenti:

* Miglioramenti delle prestazioni secondari e correzioni di bug

## <a name="ssma-v817"></a>SSMA v 8.17

La versione 8.17 di SSMA per SAP ASE contiene le modifiche seguenti:

* Aggiungere il supporto per le colonne calcolate
* Aggiornare i report di valutazione HTML per utilizzare l'editor moderno per visualizzare il testo SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La versione 8.16 di SSMA per SAP ASE contiene le modifiche seguenti:

* Rimuovere il supporto per il parser legacy
* Correzione di un problema relativo agli oggetti che non si aggiornano dal database

## <a name="ssma-v815"></a>SSMA v 8.15

Oltre a diversi miglioramenti dell'accessibilità, la versione v 8.15 di SSMA per SAP ASE contiene le modifiche seguenti:

* Rinnovare i report di valutazione per lavorare nei browser moderni
* Usare l'autorità fornita dal database per l'autenticazione Azure AD
* Migliorare la denominazione per le istruzioni caricate da file

## <a name="ssma-v814"></a>SSMA v 8.14

Oltre a diversi miglioramenti per garantire maggiore accessibilità per gli utenti con particolari esigenze, la versione 8.14 di SSMA per SAP ASE richiede un aggiornamento del progetto, perché archivia ora la versione completa del server di origine/destinazione nei metadati del progetto.

## <a name="ssma-v813"></a>SSMA v 8.13

La versione 8.13 di SSMA per SAP ASE contiene le modifiche seguenti:

* Considerare i cast di tipo implicito durante la conversione di chiamate di funzione e routine
* Migliorare la registrazione per la stringa di connessione di origine per risolvere i problemi di connessione

## <a name="ssma-v812"></a>SSMA v 8.12

La versione 8.12 di SSMA per SAP ASE contiene piccoli miglioramenti delle prestazioni e correzioni di bug.

## <a name="ssma-v811"></a>SSMA v 8.11

La versione 8.11 di SSMA per SAP ASE contiene le modifiche seguenti:

* Correzione della conversione di tabelle temporanee
* Usare la libreria MSAL.NET per l'autenticazione Azure Active Directory interattiva

## <a name="ssma-v810"></a>SSMA v 8.10

La versione v 8.10 di SSMA per SAP ASE contiene piccoli miglioramenti delle prestazioni e correzioni di bug.

## <a name="ssma-v89"></a>SSMA v 8.9

La versione v 8.9 di SSMA per SAP ASE contiene le modifiche seguenti:

* Migliorare la conversione del formato di data e ora
* Correzione del problema con i caratteri mancanti nelle definizioni SQL per gli oggetti

## <a name="ssma-v88"></a>SSMA v 8.8

La versione v 8.8 di SSMA per SAP ASE include:

* Miglioramenti alla stabilità della sincronizzazione degli oggetti SQL Server
* Miglioramenti delle prestazioni dell'interfaccia utente grafica durante valutazione e conversione
* Correzione del problema con i caratteri mancanti nelle definizioni SQL per gli oggetti

## <a name="ssma-v87"></a>SSMA v 8,7

La versione v 8,7 di SSMA per SAP ASE presenta correzioni minime e miglioramenti delle prestazioni nell'interfaccia utente grafica.

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Oltre a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni, la versione 8.6 di SSMA per SAP ASE è stata migliorata aggiungendo un'impostazione che consente agli utenti di omettere le proprietà estese di SSMA nel codice convertito.

Per sfruttare questa impostazione, in SSMA per SAP ASE passare a **strumenti**  >  **Impostazioni progetto**  >    >  **conversione** generale, quindi in **varie** aggiornare il valore dell'opzione **omette proprietà estese** su **Sì**.

![Omettere l'impostazione delle proprietà estese](../sybase/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versione 8.5 di SSMA per SAP ASE è stata migliorata con il supporto per l'autenticazione Azure Active Directory e il supporto di base per le funzionalità JSON in SQL Server, insieme a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni.

Inoltre, SSMA per SAP ASE consente ora di nascondere le tabelle e le viste di sistema (escluderle dalla conversione).

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versione v 8.4 di SSMA per SAP ASE è stata migliorata con correzioni mirate progettate per risolvere i problemi di accessibilità e correggere un bug correlato alle colonne di indice Max (per consentire 32 anziché 16) per SQL Server 2016 e versioni successive.

> [!IMPORTANT]
> Con SSMA versioni da 7,4 a 8,4, .NET 4.5.2 è un prerequisito di installazione.

## <a name="ssma-v83"></a>SSMA v 8.3

La versione 8.3 di SSMA per SAP ASE è stata migliorata con correzioni mirate progettate per migliorare le metriche di qualità e conversione. Questa versione di SSMA per SAP ASE fornisce inoltre le correzioni seguenti:

* Risolvere i problemi di accessibilità
* Aggiungere il supporto di base per il `hierarchyid` tipo in SQL Server

## <a name="ssma-v82"></a>SSMA v 8.2

La versione v 8.2 di SSMA per SAP ASE è stata migliorata con un set di correzioni mirato progettato per migliorare le metriche di qualità e conversione, nonché le correzioni per:

* Un problema con gli indici non cluster disabilitati dopo la migrazione dei dati.
* Rilevamento dei .NET Framework durante l'installazione invisibile all'utente.
* Arresti anomali intermittenti che si verifica quando viene scaricata una nuova versione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.1 a v 8.2. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versione v 8.1 di SSMA per SAP ASE è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.0 a v 8.1. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versione v 8.0 di SSMA per SAP ASE è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione. Inoltre, questa versione offre le seguenti nuove funzionalità:

* Supporto per **istanza gestita SQL di Azure** come destinazione. È ora possibile creare nuovi progetti destinati a Istanza gestita SQL di Azure:

  ![Progetto MI del database SQL](../media/ssma-newproject-sqldbmi.png)

* **Avviso di correzione** post-conversione. Altre informazioni sono disponibili [qui](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selezione dello schema o del database preliminare.

  Quando ci si connette all'origine, l'utente può ora selezionare database/schemi di interesse. Se si selezionano solo gli schemi di cui si intende eseguire la migrazione, si risparmia tempo durante la connessione iniziale e si migliorano le prestazioni complessive del SSMA.

  ![Oggetti filtro SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La versione v 7.10 di SSMA per SAP ASE è stata migliorata con correzioni mirate progettate per fornire protezione aggiuntiva per la sicurezza e la privacy per soddisfare le modifiche nei requisiti globali.

## <a name="ssma-v79"></a>SSMA v 7.9

La versione v 7.9 di SSMA per SAP ASE contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* Supporto nella riga di comando di SSMA per modificare il mapping dei tipi di dati e le preferenze di progetto.
* Supporto per la migrazione dei dati tramite SQL Server Integration Services (SSIS). Dopo la conversione dello schema, è possibile creare un pacchetto SSIS utilizzando l'opzione del menu di scelta rapida.
* La finestra di dialogo di connessione del database SQL di Azure in SSMA è stata anche modificata per specificare il nome completo del server. Nelle versioni precedenti di SSMA, il prefisso del database SQL di Azure doveva essere indicato in modo esplicito all'interno delle impostazioni dei progetti.

## <a name="ssma-v78"></a>SSMA v 7.8

La versione v 7.8 di SSMA per SAP ASE contiene le modifiche seguenti:

* Modificare il mapping dei tipi evidenziato nelle **impostazioni del progetto**.
* Possibilità per gli utenti di disabilitare la telemetria.

## <a name="ssma-v77"></a>SSMA v 7.7

La versione v 7.7 di SSMA per SAP ASE contiene le modifiche seguenti:

* SSMA per SAP ASE è stato migliorato con correzioni mirate che migliorano la qualità e la metrica di conversione.
* In base alla richiesta più diffusa, la versione a 32 bit di SSMA per SAP ASE è di nuovo. Rispetto all'implementazione precedente (prima della versione 7.4), sono disponibili due pacchetti di installazione, ma non possono essere installati side-by-side. Di conseguenza, è necessario scegliere la versione più appropriata in base ai componenti di connettività disponibili. È sempre preferibile usare la versione a 64 bit, se possibile.

## <a name="ssma-v76"></a>SSMA v 7.6

La versione v 7.6 di SSMA per SAP ASE contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione e con il supporto per SQL Server 2017 (anteprima pubblica). Il supporto per SQL Server 2017 in Windows e Linux è in versione di anteprima pubblica e non deve essere usato per le migrazioni di produzione.
* Supporto per la conversione di funzioni Sybase.

## <a name="ssma-v75"></a>SSMA v 7.5

La versione v 7.5 di SSMA per SAP ASE (in precedenza SSMA per Sybase) contiene le modifiche seguenti:

* Sono stati apportati numerosi miglioramenti per garantire maggiore accessibilità per utenti con particolari esigenze.
* Supporto per la `CREATE OR REPLACE` sintassi.

## <a name="ssma-v74"></a>SSMA v 7.4

La versione v 7.4 di SSMA per Sybase contiene le modifiche seguenti:

* L'opzione **query timeout** è ora disponibile durante l'individuazione di oggetti dello schema all'origine e alla destinazione.

  ![query timeout-opzione](../media/query-timeout_red.png)
* La metrica relativa alla qualità e alla conversione è stata migliorata con correzioni mirate, basate sui suggerimenti dei clienti.

  > [!IMPORTANT]
  > .NET 4.5.2 è un prerequisito per l'installazione di SSMA v 7.4. Inoltre, a partire da v 7.4, la versione a 32 bit di SSMA viene sospesa.

## <a name="ssma-v73"></a>SSMA v 7.3

La versione v 7.3 di SSMA per Sybase contiene le modifiche seguenti:

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

La versione 7.2 di SSMA per Sybase contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* Miglioramenti della telemetria per fornire punti dati migliori per la risoluzione dei problemi dei clienti e per migliorare i tassi di conversione di SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versione v 7.1 di SSMA per Sybase contiene le modifiche seguenti:

* SQL Server 2017 in Windows e Linux CTP1 è ora una piattaforma di destinazione supportata per la migrazione. Questa funzionalità è in anteprima tecnica e supporta lo spostamento dello schema e dei dati per SQL Server di destinazione.
* Supporto per aggiornamenti automatici per scaricare la versione più recente di SSMA non appena disponibile.
* I file binari installabili SSMA vengono ora recapitati tramite Windows Installer file di pacchetto (con estensione msi).

## <a name="may-2016"></a>Maggio 2016

La versione 2016 di SSMA per Sybase contiene le modifiche seguenti:

* Aggiunta del supporto per SQL Server 2016.
* Controllo del programma di installazione rimosso per .NET 2,0.
* La dipendenza del pacchetto di estensione è stata aggiornata da .NET 3,5 a .NET 4,0.
* Correzione `save-project` `open-project` dei comandi e per la console di SSMA.
* Correzione `securepassword` del comando per la console SSMA.
* Correzione del conteggio degli oggetti per il caricamento iniziale.
* Correzione del bug nelle impostazioni globali.

## <a name="march-2016"></a>Marzo 2016

La versione di anteprima di marzo 2016 di SSMA per Sybase aggiunge il supporto per la migrazione a SQL Server 2016.

## <a name="january-2016"></a>Gennaio 2016

La versione di manutenzione di SSMA per Sybase di gennaio 2016 contiene le modifiche seguenti:

* Aggiunta voce di menu Visualizza log a SSMA (RFC 5706203).
* Aggiunta di dati di telemetria.

## <a name="july-2014"></a>Luglio 2014

La versione luglio 2014 di SSMA per Sybase contiene le modifiche seguenti:

* Migliore conversione del codice del database SQL di Azure.
* Spostamento della funzionalità Pack di estensione nello schema per supportare il database SQL di Azure.
* Miglioramenti apportati alle prestazioni testati per i database con oltre 10.000 oggetti.
* Aggiunta di miglioramenti dell'interfaccia utente per la gestione di un numero elevato di oggetti.
* È stata aggiunta la possibilità di evidenziare gli schemi LOB noti, in modo che possano essere ignorati durante la conversione.
* Aggiunta di miglioramenti della velocità di conversione.
* Aggiunta della possibilità di visualizzare i conteggi degli oggetti nell'interfaccia utente.
* Dimensioni del report ridotte di oltre il 25%.
* Messaggi di errore migliorati per costrutti non analizzati.

## <a name="april-2014"></a>Aprile 2014

La versione aprile 2014 di SSMA per Sybase contiene le modifiche seguenti:

* Aggiunta del supporto per MS SQL Server 2014.
* Correzione dei bug relativi alla conversione in Azure.
* Correzione dei bug relativi alle pagine del report invisibile in IE 10.

## <a name="january-2012"></a>gennaio 2012

La versione di SSMA per Sybase di gennaio 2012 contiene le modifiche seguenti:

* Aggiunta del supporto per la conversione del trigger di rollback.
* È stata fornita la correzione per la conversione di `@@ROWCOUNT` e `@@ERROR` nella stessa `SET` istruzione.

## <a name="july-2011"></a>luglio 2011

La versione 2011 di SSMA per Sybase fornisce una segnalazione degli errori migliorata durante la migrazione dei dati.

## <a name="april-2011"></a>Aprile 2011

La versione aprile 2011 di SSMA per Sybase contiene le modifiche seguenti:

* Prodotto consolidato "SSMA for Sybase", che supporta [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/ssSQL11-md.md)] e SQL di Azure.
* Aggiunta del supporto per la connessione e la migrazione a [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Aggiunta di una nuova funzionalità per la conversione e la migrazione dei database Sybase in Azure SQL.
* Motore di migrazione dei dati avanzato sul lato client, che supporta la migrazione parallela dei dati.
* Miglioramento delle prestazioni di migrazione dei dati con modelli di recupero con registrazione minima
* È stata aggiunta la possibilità di convertire e migrare correttamente i database Sybase con distinzione tra maiuscole e minuscole in SQL Server.
* Aggiunta del supporto per la conversione di istruzioni join non ANSI di Sybase ASE per SQL Server istruzioni di join ANSI è stata estesa per eliminare e aggiornare le istruzioni.
* Sono state fornite opzioni di connettività aggiuntive per la connessione ai Server Sybase ASE usando il provider ODBC Sybase ASE e i provider ADO.NET di Sybase ASE.
* È stata rimossa la dipendenza da un database separato denominato `SysDB` , che contiene le funzioni di emulazione Sybase (installate come parte del pacchetto di estensione).
* Aggiunta della possibilità di installare SSMA per Sybase Extension Pack nei [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] cluster.
* Aggiunta compatibilità con le versioni precedenti dei progetti creati da versioni precedenti di SSMA (v 4.0 e v 4.2).
* Aggiunta della possibilità di installare il prodotto SSMA per Sybase v 5.0 side-by-Side (SxS) con le versioni precedenti di SSMA (v 4.0 e v 4.2).

## <a name="july-2010"></a>Luglio 2010

La versione 2010 di SSMA per Sybase è stata aggiunta:

* Supporto per la migrazione a SQL Server 2008 R2.
* Nuova applicazione console SSMA per l'esecuzione da riga di comando.
* Supporto per la migrazione dei dati tramite i motori di migrazione dei dati sia Server-Side che Client-Side.
* Supporto per l'istruzione "Custom SELECT" nella migrazione dei dati.
* Supporto per la migrazione da Sybase ASE 15.0.3 e 15,5.

## <a name="june-2008"></a>2008 giugno

La versione di giugno 2008 di SSMA per Sybase contiene le modifiche seguenti:

* È stato aggiunto SSMA tester, che testa automaticamente la conversione dell'oggetto di database e la migrazione dei dati eseguita da SSMA. Al termine di tutti i passaggi della migrazione di SSMA, utilizzare SSMA tester per verificare che gli oggetti convertiti funzionino allo stesso modo e che tutti i dati siano stati trasferiti correttamente.
* Aggiunta della conversione precedente a SQL. L'utente può ora specificare dichiarazioni di tabella temporanea (e di altro oggetto) per ogni routine di origine da usare nella conversione.
* Aggiunta di miglioramenti nella conversione degli oggetti:
  * La conversione di join è stata modificata.
  * Aggregazioni e non aggregazioni senza clausole/Group by.
  * `IDENTITY`Funzione con un' `SELECT INTO` istruzione.
  * Vincoli e indici cluster in blocco solo dati.
  * Tabelle temporanee create da `SELECT INTO` .
  * Vincoli/indici per le tabelle temporanee.
  * [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)]Sono supportati i nuovi tipi DateTime.
  * Supporto per la connettività e i tipi di dati di Sybase 15,0.

## <a name="may-2007"></a>2007 maggio

È stata aggiunta la versione 2007 di SSMA per Sybase:

* Possibilità di caricare più velocemente il contenuto del database durante il salvataggio di un progetto.
* Supporto per i commenti immessi dall'utente in SQL Server modalità SQL formattata.
* Miglioramenti nella conversione degli oggetti.

## <a name="november-2006"></a>2006 novembre

La versione di novembre 2006 di SSMA per Sybase contiene le modifiche seguenti:

* Sono state aggiunte nuove impostazioni globali:
  * È possibile scegliere di visualizzare i numeri di riga nelle finestre dell'editor.
  * È possibile configurare SSMA in modo da richiedere la sostituzione di oggetti duplicati o sempre o mai sostituire gli oggetti duplicati durante la conversione dello schema.
* È stata aggiunta una nuova opzione di conversione che consente di configurare il modo in cui SSMA gestisce le situazioni seguenti:
  * `CAST`Istruzione o `CONVERT` che contiene una stringa binaria.
  * Verifica la presenza di valori null nelle espressioni di uguaglianza.
  * Tabelle proxy.
  * Numeri di errore del messaggio utente per `RAISERROR` .
  * `UPDATE` istruzioni che contengono identificatori non risolti.
* Aggiunta di una nuova opzione di migrazione che consente di specificare in che modo SSMA deve gestire le date che non rientrano nell' [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] intervallo di date.
* È stata aggiunta un'impostazione **SQL formattata** nella scheda **SQL** , che formatta il codice per migliorare la leggibilità.
* Correzioni di bug, tra cui:
  * SSMA converte ora `LOCK TABLE <table> IN { SHARED | EXCLUSIVE } MODE` le istruzioni aggiungendo un `TABLOCK` `TABLOCKX` hint o alla query successiva `SELECT` nella tabella.
  * I cast necessari vengono ora aggiunti quando si utilizzano tipi binari nelle espressioni di caratteri.
  * Miglioramenti delle prestazioni e della memoria.

## <a name="july-2006"></a>2006 luglio

La versione di SSMA per Sybase di luglio 2006 costituisce la versione iniziale.

## <a name="see-also"></a>Vedi anche

[Introduzione con SSMA per Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)
