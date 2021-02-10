---
description: Pool di connessioni di Gestione driver
title: Pool di connessioni di gestione driver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling [ODBC]
- pooled connections [ODBC]
- connecting to driver [ODBC], connection pooling
- connecting to data source [ODBC], connection pooling
ms.assetid: ee95ffdb-5aa1-49a3-beb2-7695b27c3df9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8876696412360275fcf443a5543634c9ac537df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070336"
---
# <a name="driver-manager-connection-pooling"></a>Pool di connessioni di Gestione driver
Il pool di connessioni consente a un'applicazione di usare una connessione da un pool di connessioni che non devono essere ristabilite per ogni uso. Dopo che una connessione è stata creata e inserita in un pool, un'applicazione può riutilizzare tale connessione senza eseguire il processo completo di connessione.  
  
 L'uso di una connessione in pool può comportare un miglioramento significativo delle prestazioni, perché le applicazioni possono salvare il sovraccarico dovuto alla creazione di una connessione. Questo può essere particolarmente importante per le applicazioni di livello intermedio che si connettono tramite una rete o per applicazioni che si connettono e disconnettono ripetutamente, ad esempio le applicazioni Internet.  
  
 Oltre ai miglioramenti in termini di prestazioni, l'architettura del pool di connessioni consente di usare un ambiente e le connessioni associate da più componenti in un unico processo. Ciò significa che i componenti autonomi nello stesso processo possono interagire tra loro senza essere consapevoli gli uni dagli altri. Una connessione in un pool di connessioni può essere utilizzata ripetutamente da più componenti.  
  
> [!NOTE]
>  Il pool di connessioni può essere utilizzato da un'applicazione ODBC che espone ODBC 2. comportamento *x* , purché l'applicazione possa chiamare *SQLSetEnvAttr*. Quando si utilizza il pool di connessioni, l'applicazione non deve eseguire istruzioni SQL che modificano il database o il contesto del database, ad esempio la modifica di \<*database name*> , che modifica il catalogo utilizzato da un'origine dati.  


 Un driver ODBC deve essere completamente thread-safe e le connessioni non devono avere affinità di thread per supportare il pool di connessioni. Ciò significa che il driver è in grado di gestire una chiamata su qualsiasi thread in qualsiasi momento ed è in grado di connettersi a un thread, di usare la connessione su un altro thread e di disconnettersi su un terzo thread.  
  
 Il pool di connessioni viene gestito da Gestione driver. Le connessioni vengono tracciate dal pool quando l'applicazione chiama **SQLConnect** o **SQLDriverConnect** e vengono restituite al pool quando l'applicazione chiama **Disconnect**. Le dimensioni del pool aumentano in modo dinamico, in base alle allocazioni di risorse richieste. Si riduce in base al timeout di inattività: se una connessione è inattiva per un periodo di tempo (non è stata usata in una connessione), viene rimossa dal pool. Le dimensioni del pool sono limitate solo dai vincoli di memoria e dai limiti del server.  
  
 Gestione driver determina se una connessione specifica in un pool deve essere utilizzata in base agli argomenti passati in **SQLConnect** o **SQLDriverConnect** e in base agli attributi di connessione impostati dopo l'allocazione della connessione.  
  
 Quando Gestione driver esegue il pool di connessioni, deve essere in grado di determinare se una connessione funziona ancora prima di distribuire la connessione. In caso contrario, gestione driver continua a distribuire la connessione inattiva all'applicazione ogni volta che si verifica un errore di rete temporaneo. Un nuovo attributo di connessione è stato definito in ODBC 3 *. x*: SQL_ATTR_CONNECTION_DEAD. Si tratta di un attributo di connessione di sola lettura che restituisce SQL_CD_TRUE o SQL_CD_FALSE. Il valore SQL_CD_TRUE indica che la connessione è stata persa, mentre il valore SQL_CD_FALSE indica che la connessione è ancora attiva. (I driver conformi alle versioni precedenti di ODBC possono supportare anche questo attributo).  
  
 Un driver deve implementare questa opzione in modo efficiente oppure può compromettere le prestazioni del pool di connessioni. In particolare, una chiamata per ottenere questo attributo di connessione non deve causare un round trip al server. Un driver deve invece restituire solo l'ultimo stato noto della connessione. La connessione è inattiva se l'ultimo viaggio al server ha avuto esito negativo e non è morto se l'ultimo viaggio ha avuto esito positivo.  
  
## <a name="remarks"></a>Commenti  
 Se una connessione è stata persa (segnalata tramite SQL_ATTR_CONNECTION_DEAD), gestione driver ODBC eliminerà tale connessione chiamando Disconnect nel driver. Le nuove richieste di connessione potrebbero non trovare una connessione utilizzabile nel pool. Infine, gestione driver potrebbe creare una nuova connessione, supponendo che il pool sia vuoto.  
  
 Per usare un pool di connessioni, un'applicazione esegue i passaggi seguenti:  
  
1.  Abilita il pool di connessioni chiamando **SQLSetEnvAttr** per impostare l'attributo dell'ambiente SQL_ATTR_CONNECTION_POOLING su SQL_CP_ONE_PER_DRIVER o SQL_CP_ONE_PER_HENV. Questa chiamata deve essere eseguita prima che l'applicazione allochi l'ambiente condiviso per il quale deve essere abilitato il pool di connessioni. L'handle di ambiente nella chiamata a **SQLSetEnvAttr** deve essere impostato su null, che rende SQL_ATTR_CONNECTION_POOLING un attributo a livello di processo. Se l'attributo è impostato su SQL_CP_ONE_PER_DRIVER, per ogni driver è supportato un singolo pool di connessioni. Se un'applicazione funziona con molti driver e pochi ambienti, questo potrebbe essere più efficiente perché potrebbe essere necessario un minor numero di confronti. Se è impostato su SQL_CP_ONE_PER_HENV, per ogni ambiente è supportato un solo pool di connessioni. Se un'applicazione funziona con molti ambienti e pochi driver, questo potrebbe essere più efficiente perché potrebbe essere necessario un minor numero di confronti. Il pool di connessioni è disabilitato impostando SQL_ATTR_CONNECTION_POOLING su SQL_CP_OFF.  
  
2.  Alloca un ambiente chiamando **SQLAllocHandle** con l'argomento *HandleType* impostato su SQL_HANDLE_ENV. L'ambiente allocato da questa chiamata sarà un ambiente condiviso implicito perché il pool di connessioni è stato abilitato. L'ambiente da usare non è tuttavia determinato, fino a quando non viene chiamato **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_DBC in questo ambiente.  
  
3.  Alloca una connessione chiamando **SQLAllocHandle** con *inputhandle puntare* impostato su SQL_HANDLE_DBC e *InputHandle puntare* impostato sull'handle di ambiente allocato per il pool di connessioni. Gestione driver tenta di trovare un ambiente esistente corrispondente agli attributi dell'ambiente impostati dall'applicazione. Se non esiste alcun ambiente di questo tipo, ne viene creato uno con un conteggio dei riferimenti (gestito da Gestione driver) di 1. Se viene trovato un ambiente condiviso corrispondente, l'ambiente viene restituito all'applicazione e il relativo conteggio dei riferimenti viene incrementato. (La connessione effettiva da usare non è determinata da Gestione driver fino a quando non viene chiamato **SQLConnect** o **SQLDriverConnect** ).  
  
4.  Chiama **SQLConnect** o **SQLDriverConnect** per stabilire la connessione. Gestione driver usa le opzioni di connessione nella chiamata a **SQLConnect** (o le parole chiave di connessione nella chiamata a **SQLDriverConnect**) e gli attributi di connessione impostati dopo l'allocazione della connessione per determinare la connessione nel pool da usare.  
  
    > [!NOTE]  
    >  Il modo in cui una connessione richiesta viene abbinata a una connessione in pool è determinata dall'attributo dell'ambiente SQL_ATTR_CP_MATCH. Per ulteriori informazioni, vedere [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md).  
  
     Le applicazioni ODBC che utilizzano il pool di connessioni devono chiamare [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) durante l'inizializzazione dell'applicazione e [CoUninitialize](/windows/win32/api/combaseapi/nf-combaseapi-couninitialize) quando l'applicazione viene chiusa.  
  
5.  Chiama **SQLConnect** quando viene eseguita con la connessione. La connessione viene restituita al pool di connessioni e diventa disponibile per il riutilizzo.  
  
 Per una discussione approfondita, vedere l'articolo relativo al [pool nei componenti di accesso ai dati di Microsoft](/previous-versions/ms810829(v=msdn.10)).  
  
## <a name="connection-pooling-considerations"></a>Considerazioni sul pool di connessioni  
 L'esecuzione di una delle azioni seguenti tramite un comando SQL, anziché tramite l'API ODBC, può influire sullo stato della connessione e causare problemi imprevisti quando il pool di connessioni è attivo:  
  
-   Apertura di una connessione e modifica del database predefinito.  
  
-   Utilizzando l'istruzione SET per modificare tutte le opzioni configurabili (incluse SET ROWCOUNT, ANSI_NULL, IMPLICIT_TRANSACTIONS, SHOWPLAN, STATISTICs, TEXTSIZE e DATEFORMAT).  
  
-   Creazione di tabelle e stored procedure temporanee.  
  
 Se una di queste azioni viene eseguita al di fuori dell'API ODBC, l'utente successivo che utilizza la connessione erediterà automaticamente le impostazioni, le tabelle o le procedure precedenti.  
  
> [!NOTE]  
>  Non prevedere che determinate impostazioni siano presenti nello stato della connessione. È sempre necessario impostare lo stato di connessione nell'applicazione e assicurarsi che l'applicazione rimuova le impostazioni del pool di connessioni non utilizzate.  
  
## <a name="driver-aware-connection-pooling"></a>Pool di connessioni compatibile con il driver  
 A partire da Windows 8, un driver ODBC può utilizzare le connessioni nel pool in modo più efficiente. Per ulteriori informazioni, vedere [pool di connessioni compatibile](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)con i driver.  
  
## <a name="see-also"></a>Vedere anche  
 [Connessione a un'origine dati o a un driver](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md)   
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Raggruppamento in Microsoft Data Access Components](/previous-versions/ms810829(v=msdn.10))