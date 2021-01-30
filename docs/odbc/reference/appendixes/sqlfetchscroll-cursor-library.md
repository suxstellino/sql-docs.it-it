---
description: SQLFetchScroll (libreria di cursori)
title: SQLFetchScroll (libreria di cursori) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function [ODBC], Cursor Library
ms.assetid: 4417e57c-31dd-475e-8fe9-eab00a459c80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 719504a6e1e412439314ee6971cf41e05805f592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202845"
---
# <a name="sqlfetchscroll-cursor-library"></a>SQLFetchScroll (libreria di cursori)
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 In questo argomento viene illustrato l'utilizzo della funzione **SQLFetchScroll** nella libreria di cursori. Per informazioni generali su **SQLFetchScroll**, vedere [funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 La libreria di cursori implementa **SQLFetchScroll** chiamando ripetutamente **SQLFetch** nel driver. Trasferisce i dati recuperati dal driver ai buffer dei set di righe forniti dall'applicazione. Memorizza anche nella cache i dati in memoria e nei file su disco. Quando un'applicazione richiede un nuovo set di righe, la libreria di cursori lo recupera secondo necessità dal driver (se non è stato recuperato in precedenza) o dalla cache (se è stata recuperata in precedenza). Infine, la libreria di cursori mantiene lo stato dei dati memorizzati nella cache e restituisce tali informazioni all'applicazione nella matrice di stato della riga.  
  
 Quando si utilizza la libreria di cursori, le chiamate a **SQLFetchScroll** non possono essere combinate con chiamate a **SQLFetch** o **SQLExtendedFetch**.  
  
 Quando si utilizza la libreria di cursori, le chiamate a **SQLFetchScroll** sono supportate sia per ODBC 2. *x* e per ODBC 3. driver *x* .  
  
## <a name="rowset-buffers"></a>Buffer dei set di righe  
 La libreria di cursori ottimizza il trasferimento dei dati dal driver al buffer del set di righe fornito dall'applicazione se:  
  
-   L'applicazione usa l'associazione per riga.  
  
-   Non sono presenti byte non utilizzati tra i campi nella struttura dichiarata dall'applicazione per includere una riga di dati.  
  
-   I campi in cui **SQLFetch** o **SQLFetchScroll** restituisce la lunghezza o l'indicatore per una colonna seguono il buffer della colonna e precede il buffer per la colonna successiva. Questi campi sono facoltativi.  
  
 Quando l'applicazione richiede un nuovo set di righe, la libreria di cursori recupera i dati dalla relativa cache e dal driver in base alle esigenze. Se i set di righe nuovi e precedenti si sovrappongono, la libreria di cursori può ottimizzare le prestazioni riutilizzando i dati delle sezioni sovrapposte dei buffer del set di righe. Pertanto, le modifiche non salvate apportate ai buffer del set di righe vengono perse a meno che i set di righe nuovi e precedenti si sovrappongano e le modifiche si trovino nelle sezioni sovrapposte dei buffer del set di righe. Per salvare le modifiche, un'applicazione invia un'istruzione UPDATE posizionata.  
  
 Si noti che la libreria di cursori Aggiorna sempre i buffer dei set di righe con i dati della cache quando un'applicazione chiama **SQLFetchScroll** con l'argomento *FetchOrientation* impostato su SQL_FETCH_RELATIVE e l'argomento *FetchOffset* impostato su 0.  
  
 La libreria di cursori supporta la chiamata a **SQLSetStmtAttr** con un *attributo* di SQL_ATTR_ROW_ARRAY_SIZE per modificare la dimensione del set di righe mentre un cursore è aperto. Le nuove dimensioni del set di righe diverranno effettive alla successiva chiamata di **SQLFetchScroll** .  
  
## <a name="result-set-membership"></a>Appartenenza al set di risultati  
 La libreria di cursori recupera i dati dal driver solo quando vengono richiesti dall'applicazione. A seconda dell'origine dati e dell'impostazione dell'attributo SQL_CONCURRENCY Statement, questo ha le conseguenze seguenti:  
  
-   I dati recuperati dalla libreria di cursori potrebbero essere diversi dai dati disponibili al momento dell'esecuzione dell'istruzione. Dopo l'apertura del cursore, ad esempio, le righe inserite in un punto oltre la posizione corrente del cursore possono essere recuperate da alcuni driver.  
  
-   I dati nel set di risultati possono essere bloccati dall'origine dati per la libreria di cursori e pertanto non sono disponibili ad altri utenti.  
  
 Dopo che la libreria di cursori ha memorizzato nella cache una riga di dati, non è in grado di rilevare le modifiche apportate alla riga nell'origine dati sottostante, ad eccezione degli aggiornamenti posizionati e delle eliminazioni che operano sulla stessa cache del cursore. Questo problema si verifica perché, per le chiamate a **SQLFetchScroll**, la libreria di cursori non recupera mai i dati dall'origine dati. Ma recupera i dati dalla relativa cache.  
  
## <a name="scrolling"></a>Scorrimento  
 La libreria di cursori supporta i tipi di recupero seguenti in **SQLFetchScroll**.  
  
|Tipo di cursore|Tipi di recupero|  
|-----------------|-----------------|  
|Forward-only|SQL_FETCH_NEXT|  
|Static|SQL_FETCH_NEXT<br /><br /> SQL_FETCH_PRIOR<br /><br /> SQL_FETCH_FIRST<br /><br /> SQL_FETCH_LAST<br /><br /> SQL_FETCH_RELATIVE<br /><br /> SQL_FETCH_ABSOLUTE<br /><br /> SQL_FETCH_BOOKMARK|  
  
## <a name="errors"></a>Errors  
 Quando viene chiamato **SQLFetchScroll** e una delle chiamate a **sqlfetch** restituisce SQL_ERROR, la libreria di cursori procede come segue. Al termine di questi passaggi, la libreria di cursori continuerà l'elaborazione.  
  
1.  Chiama **SQLGetDiagRec** per ottenere informazioni sull'errore dal driver e lo inserisce come record di diagnostica in Gestione driver.  
  
2.  Imposta il campo SQL_DIAG_ROW_NUMBER nel record di diagnostica sul valore appropriato.  
  
3.  Imposta il campo SQL_DIAG_COLUMN_NUMBER nel record di diagnostica sul valore appropriato, se applicabile. in caso contrario, lo imposta su 0.  
  
4.  Imposta il valore per la riga in errore nella matrice di stato della riga su SQL_ROW_ERROR.  
  
 Dopo che la libreria di cursori ha chiamato **SQLFetch** più volte nell'implementazione di **SQLFetchScroll**, gli eventuali errori o avvisi restituiti da una delle chiamate a **SQLFetch** saranno in un record di diagnostica e potranno essere recuperati da una chiamata a **SQLGetDiagRec**. Se i dati sono stati troncati al momento del recupero, i dati troncati risiederanno ora nella cache della libreria di cursori. Le chiamate successive a **SQLFetchScroll** per scorrere fino a una riga con dati troncati restituiranno i dati troncati e non verrà generato alcun avviso poiché i dati vengono recuperati dalla cache della libreria di cursori. Per tenere traccia della lunghezza dei dati restituiti in modo che sia in grado di determinare se i dati restituiti in un buffer sono stati troncati, un'applicazione deve associare il buffer di lunghezza/indicatore.  
  
## <a name="bookmark-operations"></a>Operazioni di segnalibro  
 La libreria di cursori supporta la chiamata a **SQLFetchScroll** con *FetchOrientation* SQL_FETCH_BOOKMARK. Supporta inoltre la specifica di un offset nell'argomento *FetchOffset* che può essere utilizzato nell'operazione di segnalibro. Questa è l'unica operazione di segnalibro supportata dalla libreria di cursori. La libreria di cursori non supporta la chiamata a **SQLBulkOperations**.  
  
 Se l'applicazione ha impostato l'attributo SQL_ATTR_USE_BOOKMARKS Statement ed è stato associato alla colonna bookmark, la libreria di cursori genera un segnalibro a lunghezza fissa e lo restituisce all'applicazione. La libreria di cursori crea e gestisce i segnalibri che utilizza; non vengono utilizzati segnalibri conservati nell'origine dati. Quando **SQLFetchScroll** viene chiamato per recuperare un blocco di dati che è già stato recuperato dall'origine dati, recupera i dati dalla cache della libreria di cursori. Di conseguenza, il segnalibro utilizzato in una chiamata a **SQLFetchScroll** con un *FetchOrientation* di SQL_FETCH_BOOKMARK deve essere creato e gestito dalla libreria di cursori.  
  
## <a name="interaction-with-other-functions"></a>Interazione con altre funzioni  
 Prima di preparare o eseguire le istruzioni Update o DELETE posizionate, un'applicazione deve chiamare **SQLFetch** o **SQLFetchScroll** .
