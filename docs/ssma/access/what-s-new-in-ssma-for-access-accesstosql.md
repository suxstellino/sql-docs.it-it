---
title: Novità di SSMA per Access (AccessToSQL) | Microsoft Docs
description: Scopri le modifiche apportate a SQL Server Migration Assistant (SSMA) per l'accesso (AccessToSQL) per ogni versione.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a24d3fc0-6911-4bfa-828a-197abf222e02
ms.author: alexiva
ms.openlocfilehash: 446d436a01a222c85e0e0e60083b973a7e16413c
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981087"
---
# <a name="whats-new-in-ssma-for-access-accesstosql"></a>Novità di SSMA per Access (AccessToSQL)

Questo articolo elenca SQL Server Migration Assistant (SSMA) per le modifiche di accesso in ogni versione.

## <a name="ssma-v818"></a>SSMA v 8.18

La versione v 8.18 di SSMA per Access contiene le modifiche seguenti:

* Miglioramenti delle prestazioni secondari e correzioni di bug

## <a name="ssma-v817"></a>SSMA v 8.17

La versione v 8.17 di SSMA per Access contiene le modifiche seguenti:

* Aggiornare i report di valutazione HTML per utilizzare l'editor moderno per visualizzare il testo SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La versione v 8.16 di SSMA per Access contiene le modifiche seguenti:

* Mostra il testo SQL per le query nel report di conversione HTML
* Rimuovere il supporto per il parser legacy
* Correzione di un problema relativo agli oggetti che non si aggiornano dal database

## <a name="ssma-v815"></a>SSMA v 8.15

Oltre a diversi miglioramenti dell'accessibilità, la versione v 8.15 di SSMA per Access contiene le modifiche seguenti:

* Ignora indici creati automaticamente per chiavi esterne
* Rinnovare i report di valutazione per lavorare nei browser moderni
* Usare l'autorità fornita dal database per l'autenticazione Azure AD
* Migliorare la denominazione per le istruzioni caricate da file

## <a name="ssma-v814"></a>SSMA v 8.14

Oltre a diversi miglioramenti per garantire maggiore accessibilità per utenti con particolari esigenze, la versione 8.14 di SSMA per l'accesso richiede un aggiornamento del progetto, perché archivia ora la versione completa del server di origine/destinazione nei metadati del progetto.

## <a name="ssma-v813"></a>SSMA v 8.13

La versione v 8.13 di SSMA per Access contiene le modifiche seguenti:

* Correzione della `ORDER BY` conversione con la `UNION` clausola
* Supporto per indici univoci filtrati
* Considerare i cast di tipo implicito durante la conversione di chiamate di funzione e routine

## <a name="ssma-v812"></a>SSMA v 8.12

La versione v 8.12 di SSMA per Access contiene le modifiche seguenti:

* Supporto per `BigInt` il `Large Number` tipo di dati ()
* Risoluzione del tipo di colonna migliorata
* Migliore conversione delle regole di convalida delle colonne
* Uso del provider di OLE DB ACE disponibile più recente per la migrazione dei dati

## <a name="ssma-v811"></a>SSMA v 8.11

La versione v 8.11 di SSMA per Access contiene le modifiche seguenti:

* Usare la libreria MSAL.NET per l'autenticazione Azure Active Directory interattiva

## <a name="ssma-v810"></a>SSMA v 8.10

La versione v 8.10 di SSMA per l'accesso contiene miglioramenti delle prestazioni e correzioni di bug minori.

## <a name="ssma-v89"></a>SSMA v 8.9

La versione v 8.9 di SSMA per Access contiene le modifiche seguenti:

* Conversione migliorata per le query con riferimento automatico
* Correzione del problema con i caratteri speciali nel nome del progetto

## <a name="ssma-v88"></a>SSMA v 8.8

La versione v 8.8 di SSMA per l'accesso include:

* Miglioramenti alla stabilità della sincronizzazione degli oggetti SQL Server
* Miglioramenti delle prestazioni dell'interfaccia utente grafica durante valutazione e conversione
* Nuovo parser della sintassi di accesso per migliorare ulteriormente le prestazioni di conversione

## <a name="ssma-v87"></a>SSMA v 8,7

La versione v 8.7 di SSMA per Access ha migliorato la conversione per la `IIF` funzione nelle query, nonché le correzioni minime e i miglioramenti delle prestazioni nell'interfaccia utente grafica.

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Oltre a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni, la versione 8.6 di SSMA per l'accesso è stata migliorata aggiungendo un'impostazione che consente agli utenti di omettere le proprietà estese di SSMA nel codice convertito.

Per sfruttare questa impostazione, in SSMA per Access passare a **strumenti**  >  **Impostazioni progetto**  >    >  **conversione** generale, quindi in **varie** aggiornare il valore dell'opzione **omette proprietà estese** su **Sì**.

![Omettere l'impostazione delle proprietà estese](../access/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Con SSMA v 8.5 e versioni successive, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versione 8.5 di SSMA per l'accesso è stata migliorata con il supporto per l'autenticazione Azure Active Directory e il supporto di base per le funzionalità JSON in SQL Server, insieme a un set di correzioni mirato progettato per migliorare l'usabilità e le prestazioni.

Inoltre, SSMA per l'accesso supporta ora la conversione di più funzioni standard ( `ISNULL` , `IIF` e così via).

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 è un prerequisito di installazione. Se è necessario installare questa versione, è possibile scaricare il file di runtime da [qui](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versione di SSMA per l'accesso a v 8.4 è stata migliorata con correzioni mirate progettate per risolvere i problemi di accessibilità e correggere un bug correlato alle colonne di indice Max (per consentire 32 anziché 16) per SQL Server 2016 e versioni successive.

> [!IMPORTANT]
> Con SSMA versioni 7,4, sebbene 8,4, .NET 4.5.2 è un prerequisito di installazione.

## <a name="ssma-v83"></a>SSMA v 8.3

La versione di SSMA per l'accesso a v 8.3 è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione. Inoltre, questa versione di SSMA per Access fornisce le correzioni seguenti:

* Risolvere i problemi di accessibilità.
* Aggiungere il supporto di base per il `hierarchyid` tipo in SQL Server.

## <a name="ssma-v82"></a>SSMA v 8.2

La versione di SSMA per l'accesso a v 8.2 è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.1 a v 8.2. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versione v 8.1 di SSMA per l'accesso è stata migliorata con correzioni mirate progettate per migliorare la qualità e le metriche di conversione.

> [!NOTE]
> Un problema noto relativo all'aggiornamento automatico può provocare l'errore di un aggiornamento da SSMA v 8.0 a v 8.1. Se si verifica questo errore, scaricare la nuova versione e installarla manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versione v 8.0 di SSMA per l'accesso è stata migliorata con correzioni mirate progettate per migliorare la qualità e la metrica di conversione. Questa versione offre anche le seguenti nuove funzionalità:

* Supporto per **istanza gestita SQL di Azure** come destinazione. È ora possibile creare nuovi progetti destinati a Istanza gestita SQL di Azure:

  ![Progetto SQL MI](../media/ssma-newproject-sqldbmi.png)

* **Avviso di correzione** post-conversione. Altre informazioni sono disponibili [qui](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Accelerate-your-Oracle-migrations-with-new-machine-learning/ba-p/368733).

* Selezione dello schema o del database preliminare.

    Quando ci si connette all'origine, l'utente può ora selezionare database/schemi di interesse. Se si selezionano solo gli schemi di cui si intende eseguire la migrazione, si risparmia tempo durante la connessione iniziale e si migliorano le prestazioni complessive del SSMA.

   ![Oggetti filtro SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La versione v 7.10 di SSMA per l'accesso è stata migliorata con correzioni mirate progettate per fornire protezione aggiuntiva per la sicurezza e la privacy per soddisfare le modifiche nei requisiti globali.

## <a name="ssma-v79"></a>SSMA v 7.9

La versione v 7.9 di SSMA per Access contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* Supporto nella riga di comando di SSMA per modificare il mapping dei tipi di dati e le preferenze di progetto.
* La finestra di dialogo di connessione del database SQL di Azure in SSMA è stata anche modificata per specificare il nome completo del server. Nelle versioni precedenti di SSMA, il prefisso del database SQL di Azure doveva essere indicato in modo esplicito all'interno delle impostazioni dei progetti.

## <a name="ssma-v78"></a>SSMA v 7.8

La versione v 7.8 di SSMA per Access contiene le modifiche seguenti:

* Modificare il mapping dei tipi evidenziato nelle impostazioni del progetto.
* Possibilità per gli utenti di disabilitare la telemetria.

## <a name="ssma-v77"></a>SSMA v 7.7

La versione v 7.7 di SSMA per Access contiene le modifiche seguenti:

* Correzioni mirate che migliorano le metriche di qualità e conversione.
* In base alla richiesta più diffusa, la versione a 32 bit di SSMA per l'accesso è stata riattivata. Rispetto all'implementazione precedente (prima della versione 7.4), sono disponibili due pacchetti di installazione, ma non possono essere installati side-by-side. Di conseguenza, è necessario scegliere la versione più appropriata in base ai componenti di connettività disponibili. È sempre preferibile usare la versione a 64 bit, se possibile.

## <a name="ssma-v76"></a>SSMA v 7.6

La versione 7.6 di SSMA per l'accesso è stata migliorata con correzioni mirate che migliorano le metriche di qualità e conversione e con il supporto per SQL Server 2017 (anteprima pubblica). Il supporto per SQL Server 2017 in Windows e Linux è in versione di anteprima pubblica e non deve essere usato per le migrazioni di produzione.

## <a name="ssma-v75"></a>SSMA v 7.5

La versione v 7.5 di SSMA per l'accesso è stata migliorata con diversi miglioramenti per garantire una maggiore accessibilità per gli utenti con particolari esigenze.

## <a name="ssma-v74"></a>SSMA v 7.4

La versione v 7.4 di SSMA per Access contiene le modifiche seguenti:

* L'opzione **query timeout** è ora disponibile durante l'individuazione di oggetti dello schema all'origine e alla destinazione.

  ![query timeout-opzione](../media/query-timeout_red.png)

* La metrica relativa alla qualità e alla conversione è stata migliorata con correzioni mirate, basate sui suggerimenti dei clienti.

  > [!IMPORTANT]
  > .NET 4.5.2 è un prerequisito per l'installazione di SSMA v 7.4. Inoltre, a partire da v 7.4, la versione a 32 bit di SSMA è stata sospesa.

## <a name="ssma-v73"></a>SSMA v 7.3

La versione v 7.3 di SSMA per Access contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* SSMA Extensibility Framework esposto tramite gli elementi seguenti:
  * Esporta la funzionalità in un progetto di SQL Server Data Tools (SSDT).
    * È ora possibile esportare gli script dello schema da SSMA a un progetto SSDT. È possibile utilizzare gli script dello schema per apportare ulteriori modifiche allo schema e distribuire il database.

        ![Comando Salva come progetto SSDT](../media/export-schema-scripts_red.png)
  * Librerie che possono essere utilizzate da SSMA per l'esecuzione di conversioni personalizzate.
    * È ora possibile costruire codice in grado di gestire le conversioni e le conversioni di sintassi personalizzate che non sono state precedentemente gestite da SSMA.
      * Le istruzioni su come creare un convertitore personalizzato, insieme a un progetto di esempio per la conversione, sono disponibili nel post di Blog relativo all' [estensione delle funzionalità di conversione di SQL Server Migration Assistant](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Extending-SQL-Server-Migration-Assistant-s-conversion/ba-p/1004181).

## <a name="ssma-v72"></a>SSMA v 7.2

La versione 7.2 di SSMA per Access contiene le modifiche seguenti:

* Miglioramento della qualità e della metrica di conversione con correzioni mirate basate sui suggerimenti dei clienti.
* Miglioramenti della telemetria per fornire punti dati migliori per la risoluzione dei problemi dei clienti e per migliorare i tassi di conversione di SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versione v 7.1 di SSMA per Access contiene le modifiche seguenti:

* SQL Server 2017 in Windows e Linux CTP1 è ora una piattaforma di destinazione supportata per la migrazione. Questa funzionalità è in anteprima tecnica e supporta lo spostamento dello schema e dei dati per SQL Server di destinazione.
* SSMA supporta ora gli aggiornamenti automatici per scaricare la versione più recente di SSMA non appena disponibile.
* I file binari installabili SSMA vengono ora recapitati tramite Windows Installer file di pacchetto (con estensione msi).

## <a name="may-2016"></a>Maggio 2016

La versione 2016 di SSMA per l'accesso contiene le modifiche seguenti:

* Aggiunta del supporto ufficiale per SQL Server 2016.
* Controllo del programma di installazione rimosso per .NET 2,0.
* Correzione `save-project` `open-project` dei comandi e per la console di SSMA.
* Correzione `securepassword` del comando per la console SSMA.
* Correzione del conteggio degli oggetti per il caricamento iniziale.
* Caricamento dei dati delle tabelle fisse per le schede dell'interfaccia utente per l'accesso.
* Correzione del bug nelle impostazioni globali.

## <a name="march-2016"></a>Marzo 2016

La versione di anteprima di SSMA di marzo 2016 per Access aggiunge il supporto per la migrazione a SQL Server 2016.

## <a name="january-2016"></a>Gennaio 2016

La versione di manutenzione di SSMA per l'accesso di gennaio 2016 contiene le modifiche seguenti:

* Correzione della funzione non valida per il valore predefinito di un campo GUID (RFC 3894811).
* Correzione del problema per cui il sistema smette di rispondere durante l'importazione di record nel database SQL (Azure) (RFC 4919573).
* Aggiunta voce di menu Visualizza log a SSMA (RFC 5706203).
* Aggiunta di dati di telemetria.

## <a name="july-2014"></a>Luglio 2014

La versione di SSMA per l'accesso del 2014 luglio contiene le modifiche seguenti:

* Migliore conversione del codice del database SQL di Azure.
* Spostamento della funzionalità Pack di estensione nello schema per supportare il database SQL di Azure.
* Miglioramenti delle prestazioni testati per i database con oltre 10.000 oggetti.
* Aggiunta di miglioramenti dell'interfaccia utente per la gestione di un numero elevato di oggetti.
* È stato aggiunto il supporto per evidenziare gli schemi LOB "noti", in modo che possano essere ignorati durante la conversione.
* Aggiunta di miglioramenti della velocità di conversione.
* Aggiunta del supporto per la visualizzazione dei conteggi degli oggetti nell'interfaccia utente.
* Dimensioni del report ridotte di oltre il 25%.
* Messaggi di errore migliorati per costrutti non analizzati.

## <a name="april-2014"></a>Aprile 2014

La versione aprile 2014 di SSMA per Access contiene le modifiche seguenti:

* Aggiunta del supporto per MS SQL Server 2014.
* Correzione dei bug relativi alla conversione in Azure.
* Correzione dei bug relativi alle pagine del report invisibile in IE 10.

## <a name="january-2012"></a>gennaio 2012

La versione di SSMA di gennaio 2012 per Access contiene le modifiche seguenti:

* È stata specificata l'opzione per non salvare in modo permanente nome utente e password per le tabelle collegate MS Access dopo la migrazione.
* Impostare le azioni CASCADE per i riferimenti circolari su No Action.
* Sono stati forniti messaggi appropriati che indicano che le azioni a cascata per i riferimenti circolari sono state impostate su No Action.

## <a name="july-2011"></a>luglio 2011

La versione di SSMA per Access del 2011 luglio aggiunge una segnalazione errori migliorata durante la migrazione dei dati.

## <a name="april-2011"></a>Aprile 2011

La versione aprile 2011 di SSMA per Access contiene le modifiche seguenti:

* Aggiunta di una singola installazione di "SSMA for Access", che supporta [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] e SQL di Azure.
* È stata aggiunta la possibilità di connettersi a [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Aggiunta di SSMA per il supporto della versione della console di Access per compatibilità con le versioni precedenti È possibile aprire i progetti creati dalle versioni precedenti a SSMA v 5.0.
* Aggiunta della possibilità di installare il prodotto SSMA v 5.0 side-by-Side (SxS) con le versioni precedenti del prodotto SSMA.

## <a name="july-2010"></a>Luglio 2010

La versione di SSMA per l'accesso del 2010 luglio contiene le modifiche seguenti:

* Aggiunta del supporto per la migrazione a SQL Server 2008 R2 e Azure SQL.
* È stata aggiunta una connessione sicura sia a SQL Server che a SQL di Azure.
* Aggiunta del supporto per i database di Access 2010.
* Aggiunta di una nuova applicazione console SSMA per l'esecuzione da riga di comando.
* Aggiunta del supporto per il `DateTime2` tipo di dati SQL Server.

## <a name="june-2008"></a>2008 giugno

La versione di SSMA per Access di giugno 2008 aggiunge il supporto per i database di Access 2007.

## <a name="may-2007"></a>2007 maggio

La versione 2007 di SSMA per l'accesso contiene le modifiche seguenti:

* Aggiunta del supporto per i database di Access che utilizzano i criteri del gruppo di lavoro.
* Consente di eliminare gli oggetti convertiti dal SQL Server Esplora metadati.
* Aggiunta del supporto per i commenti immessi dall'utente in SQL Server modalità SQL formattata.
* Aggiunta di miglioramenti nella conversione degli oggetti.

## <a name="november-2006"></a>2006 novembre

La versione di novembre 2006 di SSMA per Access contiene le modifiche seguenti:

* Aggiunta di una nuova migrazione guidata database che consente di eseguire la migrazione di un singolo database da Access a [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] .
* È stato aggiunto un nuovo comando Convert, Load e migrate che converte i database di Access, carica gli oggetti convertiti in [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] ed esegue la migrazione dei dati in [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] un unico passaggio.
* Miglioramento della migrazione delle query. La migrazione delle query ora converte più query SELECT in viste. Per ulteriori informazioni, vedere [conversione di oggetti di database di Access](converting-access-database-objects-accesstosql.md).
* È stata aggiunta la possibilità di modificare le proprietà delle tabelle e degli indici nella [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] scheda **tabella** .
* Sono state aggiunte nuove impostazioni globali:
  * È possibile scegliere di visualizzare i numeri di riga nelle finestre dell'editor.
  * È possibile configurare SSMA in modo da richiedere la sostituzione di oggetti duplicati o sempre o mai sostituire gli oggetti duplicati durante la conversione dello schema.
* È stata aggiunta una nuova opzione di conversione che consente di specificare se SSMA Visualizza un avviso quando una query complessa contiene un carattere jolly.

## <a name="july-2006"></a>2006 luglio

La versione 2006 di SSMA per Access è stata la versione iniziale.
