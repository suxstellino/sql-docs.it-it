---
description: Funzione SQLMoreResults
title: Funzione SQLMoreResults | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLMoreResults
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLMoreResults
helpviewer_keywords:
- SQLMoreResults function [ODBC]
ms.assetid: bf169ed5-4d55-412c-b184-12065a726e89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d894cc8cf87a3db794bed763c9bffc596910299a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160800"
---
# <a name="sqlmoreresults-function"></a>Funzione SQLMoreResults
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLMoreResults** determina se sono disponibili più risultati in un'istruzione che contiene istruzioni **Select**, **Update**, **Insert** o **Delete** e, in caso affermativo, Inizializza l'elaborazione per tali risultati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLMoreResults(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_NO_DATA, SQL_ERROR, SQL_INVALID_HANDLE O SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLMoreResults** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLMoreResults** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Il valore dell'opzione è stato modificato|Il valore di un attributo di istruzione è stato modificato durante l'elaborazione del batch. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione **SQLMoreResults** è stata chiamata e, prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle*. La funzione **SQLMoreResults** è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione **SQLMoreResults** è stata chiamata e, prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLMoreResults** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 Le istruzioni **Select** restituiscono set di risultati. Le istruzioni **Update**, **Insert** e **Delete** restituiscono un conteggio delle righe interessate. Se una di queste istruzioni viene raggruppata in batch, viene inviata con matrici di parametri (numerati in ordine di parametro crescente, nell'ordine in cui sono visualizzati nel batch) o nelle procedure, possono restituire più set di risultati o conteggi di righe. Per informazioni sui batch di istruzioni e matrici di parametri, vedere [batch di istruzioni SQL](../../../odbc/reference/develop-app/batches-of-sql-statements.md) e [matrici di valori di parametri](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Dopo l'esecuzione del batch, l'applicazione viene posizionata sul primo set di risultati. L'applicazione può chiamare **SQLBindCol**, **SQLBulkOperations**, **SQLFetch**, **SQLGetData**, **SQLFetchScroll**, **SQLSetPos** e tutte le funzioni di metadati, nel primo o in tutti i set di risultati successivi, così come se si trattasse di un unico set di risultati. Al termine del primo set di risultati, l'applicazione chiama **SQLMoreResults** per passare al set di risultati successivo. Se è disponibile un altro set di risultati o un conteggio, **SQLMoreResults** restituisce SQL_SUCCESS e inizializza il set di risultati o il conteggio per l'elaborazione aggiuntiva. Se vengono visualizzate istruzioni di generazione dei conteggi delle righe tra le istruzioni che generano un set di risultati, è possibile richiamarle chiamando **SQLMoreResults**. Dopo la chiamata di **SQLMoreResults** per le istruzioni **Update**, **Insert** o **Delete** , un'applicazione può chiamare **SQLRowCount**.  
  
 Se è presente un set di risultati corrente con righe non recuperate, **SQLMoreResults** rimuove il set di risultati e rende disponibile il set di risultati o il conteggio successivo. Se tutti i risultati sono stati elaborati, **SQLMoreResults** restituisce SQL_NO_DATA. Per alcuni driver, i parametri di output e i valori restituiti non sono disponibili finché non vengono elaborati tutti i set di risultati e i conteggi delle righe. Per tali driver, i parametri di output e i valori restituiti diventano disponibili quando **SQLMoreResults** restituisce SQL_NO_DATA.  
  
 Le associazioni stabilite per il set di risultati precedente rimangono comunque valide. Se le strutture delle colonne sono diverse per questo set di risultati, la chiamata a **SQLFetch** o **SQLFetchScroll** può causare un errore o un troncamento. Per evitare questo problema, l'applicazione deve chiamare **SQLBindCol** per eseguire il riassociazione in modo esplicito, a seconda del caso, impostando i campi del descrittore. In alternativa, l'applicazione può chiamare **SQLFreeStmt** con un' *opzione* di SQL_UNBIND per annullare l'associazione di tutti i buffer di colonna.  
  
 I valori degli attributi di istruzione, ad esempio tipo di cursore, concorrenza del cursore, dimensione del keyset o lunghezza massima, possono cambiare quando l'applicazione passa attraverso il batch tramite chiamate a **SQLMoreResults**. In tal caso, **SQLMoreResults** restituirà SQL_SUCCESS_WITH_INFO e SQLSTATE 01S02 (il valore dell'opzione è stato modificato).  
  
 Se si chiama **SQLCloseCursor** o **SQLFreeStmt** con un' *opzione* di SQL_CLOSE, vengono eliminati tutti i set di risultati e i conteggi delle righe disponibili come risultato dell'esecuzione del batch. L'handle di istruzione restituisce lo stato allocato o preparato. Se si chiama **SQLCancel** per annullare una funzione in esecuzione asincrona quando è stato eseguito un batch e l'handle di istruzione è nello stato eseguito, posizionato su cursore o asincrono, tutti i set di risultati e i conteggi delle righe generati dal batch vengono eliminati se la chiamata di annullamento ha avuto esito positivo. L'istruzione restituisce quindi allo stato preparato o allocato.  
  
 Se un batch di istruzioni o una routine combina altre istruzioni SQL con le istruzioni **Select**, **Update**, **Insert** e **Delete** , queste altre istruzioni non influiscono su **SQLMoreResults**.  
  
 Per ulteriori informazioni, vedere [più risultati](../../../odbc/reference/develop-app/multiple-results.md).  
  
 Se un'istruzione Update, INSERT o DELETE cercata in un batch di istruzioni non influisce su alcuna riga nell'origine dati, **SQLMoreResults** restituisce SQL_SUCCESS. Questa operazione è diversa dal caso di un'istruzione di aggiornamento, inserimento o eliminazione ricercata eseguita tramite **SQLExecDirect**, **SQLExecute** o **SQLParamData**, che restituisce SQL_NO_DATA se non influisce su alcuna riga nell'origine dati. Se un'applicazione chiama **SQLRowCount** per recuperare il conteggio delle righe dopo che una chiamata a **SQLMoreResults** non ha interessato alcuna riga, **SQLRowCount** restituirà SQL_NO_DATA.  
  
 Per ulteriori informazioni sulla sequenziazione valida delle funzioni di elaborazione dei risultati, vedere [Appendice B: tabelle di transizione dello stato ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
 Per ulteriori informazioni sui parametri di output SQL_PARAM_DATA_AVAILABLE e trasmessi, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="availability-of-row-counts"></a>Disponibilità dei conteggi delle righe  
 Quando un batch contiene più istruzioni consecutive di generazione di conteggi di righe, è possibile che venga eseguito il rollup di questi conteggi di righe in un solo numero di righe. Se, ad esempio, un batch include cinque istruzioni INSERT, alcune origini dati sono in grado di restituire cinque conteggi di righe singoli. Determinate altre origini dati restituiscono solo un conteggio di righe che rappresenta la somma dei cinque conteggi di righe singoli.  
  
 Quando un batch contiene una combinazione di istruzioni di generazione del set di risultati e di generazione dei conteggi delle righe, i conteggi delle righe possono essere o meno disponibili. Il comportamento del driver rispetto alla disponibilità dei conteggi delle righe viene enumerato nel tipo di informazioni SQL_BATCH_ROW_COUNT disponibile tramite una chiamata a **SQLGetInfo**. Si supponga, ad esempio, che il batch includa un oggetto **Select**, seguito da due **istruzioni INSERT** e un'altra **Select**. Sono quindi possibili i casi seguenti:  
  
-   I conteggi delle righe corrispondenti alle due istruzioni **Insert** non sono disponibili. La prima chiamata a **SQLMoreResults** sarà posizionata nel set di risultati della seconda istruzione **SELECT** .  
  
-   I conteggi delle righe corrispondenti alle due istruzioni **Insert** sono disponibili singolarmente. Una chiamata a **SQLGetInfo** non restituisce il bit SQL_BRC_ROLLED_UP per il tipo di informazioni di SQL_BATCH_ROW_COUNT. La prima chiamata a **SQLMoreResults** consente di posizionare il numero di righe del primo **inserimento** e la seconda chiamata si posiziona sul numero di righe della seconda operazione di **inserimento**. La terza chiamata a **SQLMoreResults** sarà posizionata nel set di risultati della seconda istruzione **SELECT** .  
  
-   Viene eseguito il rollup dei conteggi delle righe corrispondenti ai due **inserimenti** in un solo numero di righe disponibile. Una chiamata a **SQLGetInfo** restituisce il bit SQL_BRC_ROLLED_UP per il tipo di informazioni di SQL_BATCH_ROW_COUNT. La prima chiamata a **SQLMoreResults** consentirà di posizionare il numero di righe di cui è stato eseguito il rollup e la seconda chiamata a **SQLMoreResults** sarà posizionata nel set di risultati della seconda istruzione **Select**.  
  
 Alcuni driver rendono i conteggi delle righe disponibili solo per i batch espliciti e non per le stored procedure.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di sola trasmissione|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Recupero di parte o di tutte le colonne di dati|[Funzione SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
