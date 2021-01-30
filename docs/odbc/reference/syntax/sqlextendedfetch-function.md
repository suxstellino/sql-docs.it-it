---
description: Funzione SQLExtendedFetch
title: Funzione SQLExtendedFetch | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExtendedFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLExtendedFetch
helpviewer_keywords:
- SQLExtendedFetch function [ODBC]
ms.assetid: 940b5cf7-581c-4ede-8533-c67d5e9ef488
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 403b8d426049ba6a333920d472c2e69ed279d761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179861"
---
# <a name="sqlextendedfetch-function"></a>Funzione SQLExtendedFetch
**Conformità**  
 Versione introdotta: conformità agli standard ODBC 1,0: deprecato  
  
 **Summary**  
 **SQLExtendedFetch** Recupera il set di righe specificato di dati dal set di risultati e restituisce i dati per tutte le colonne con binding. I set di righe possono essere specificati in una posizione assoluta o relativa o tramite segnalibro.  
  
> [!NOTE]
>  In ODBC 3 *. x*, **SQLExtendedFetch** è stato sostituito da **SQLFetchScroll**. Le applicazioni ODBC 3 *. x* non devono chiamare **SQLExtendedFetch**; devono invece chiamare **SQLFetchScroll**. Gestione driver esegue il mapping di **SQLFetchScroll** a **SQLExtendedFetch** quando si utilizza un driver ODBC 2 *. x* . I driver ODBC 3 *. x* devono supportare **SQLExtendedFetch** se desiderano utilizzare le applicazioni ODBC 2 *. x* che lo chiamano. Per ulteriori informazioni, vedere "Commenti" e [cursori a blocchi, cursori scorrevoli e compatibilità con le versioni precedenti](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) in Appendice G: linee guida per la compatibilità con le versioni precedenti.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLExtendedFetch(  
      SQLHSTMT         StatementHandle,  
      SQLUSMALLINT     FetchOrientation,  
      SQLLEN           FetchOffset,  
      SQLULEN *        RowCountPtr,  
      SQLUSMALLINT *   RowStatusArray);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *FetchOrientation*  
 Input Tipo di recupero. Corrisponde a *FetchOrientation* in **SQLFetchScroll**.  
  
 *FetchOffset*  
 Input Numero della riga da recuperare. Corrisponde a *FetchOffset* in **SQLFetchScroll**, con un'eccezione. Quando *FetchOrientation* è SQL_FETCH_BOOKMARK, *FetchOffset* è un segnalibro a lunghezza fissa, non un offset da un segnalibro. In altre parole, **SQLExtendedFetch** Recupera il segnalibro da questo argomento, non l'attributo dell'istruzione SQL_ATTR_FETCH_BOOKMARK_PTR. Non supporta i segnalibri a lunghezza variabile e non supporta il recupero di un set di righe con un offset (diverso da 0) da un segnalibro.  
  
 *RowCountPtr*  
 Output Puntatore a un buffer in cui restituire il numero di righe effettivamente recuperate. Questo buffer viene usato nello stesso modo del buffer specificato dall'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR. Questo buffer viene usato solo da **SQLExtendedFetch**. Non viene usato da **SQLFetch** o **SQLFetchScroll**.  
  
 *RowStatusArray*  
 Output Puntatore a una matrice in cui restituire lo stato di ogni riga. Questa matrice viene utilizzata allo stesso modo della matrice specificata dall'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.  
  
 Tuttavia, l'indirizzo di questa matrice non viene archiviato nel campo SQL_DESC_STATUS_ARRAY_PTR di IRD. Questa matrice viene inoltre usata solo da **SQLExtendedFetch** e da **SQLBulkOperations** con un' *operazione* di SQL_ADD o **SQLSetPos** quando viene chiamata dopo **SQLExtendedFetch**. Non viene usato da **SQLFetch** o **SQLFetchScroll** e non viene usato da **SQLBulkOperations** o **SQLSetPos** quando vengono chiamati dopo **SQLFetch** o **SQLFetchScroll**. Non viene inoltre usato quando **SQLBulkOperations** con un' *operazione* di SQL_ADD viene chiamato prima di chiamare qualsiasi funzione fetch. In altre parole, viene usato solo nello stato dell'istruzione S7. Non viene utilizzato negli Stati di istruzione S5 o S6. Per ulteriori informazioni, vedere [transizioni di istruzioni](../../../odbc/reference/appendixes/statement-transitions.md) nell'Appendice B: tabelle di transizione dello stato ODBC.  
  
 Le applicazioni devono fornire un puntatore valido nell'argomento *RowStatusArray* . in caso contrario, il comportamento di **SQLExtendedFetch** e il comportamento delle chiamate a **SQLBulkOperations** o **SQLSetPos** dopo che un cursore è stato posizionato da **SQLExtendedFetch** non sono definiti.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLExtendedFetch** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLError**. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLExtendedFetch** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente. Se si verifica un errore in una singola colonna, è possibile chiamare **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_COLUMN_NUMBER per determinare la colonna in cui si è verificato l'errore. è possibile chiamare e **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_ROW_NUMBER per determinare la riga contenente la colonna.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati di tipo stringa o binario restituiti per una colonna hanno causato il troncamento di dati di tipo carattere non vuoto o binari non NULL. Se si trattasse di un valore stringa, viene troncato a destra. Se si trattasse di un valore numerico, la parte frazionaria del numero veniva troncata.  (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S01|Errore nella riga|Si è verificato un errore durante il recupero di una o più righe. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S06|Tentativo di recupero prima che il set di risultati restituisse il primo set di righe|Il set di righe richiesto ha sovrapposto l'inizio del set di risultati quando la posizione corrente supera la prima riga e *FetchOrientation* è stato SQL_PRIOR o *FetchOrientation* è stato SQL_RELATIVE con un *FetchOffset* negativo il cui valore assoluto era minore o uguale al SQL_ROWSET_SIZE corrente. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S07|Troncamento frazionario|I dati restituiti per una colonna sono stati troncati. Per i tipi di dati numerici, la parte frazionaria del numero è stata troncata. Per i tipi di dati time, timestamp e Interval contenenti un componente ora, la parte frazionaria del tempo è stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Non è stato possibile convertire un valore di dati nel tipo di dati C specificato da *targetType* in **SQLBindCol**.|  
|07009|Indice del descrittore non valido|La colonna 0 è stata associata a **SQLBindCol** e l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS è stato impostato su SQL_UB_OFF.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22002|Variabile indicatore obbligatoria ma non fornita|Sono stati recuperati dati NULL in una colonna la cui *StrLen_or_IndPtr* impostata da **SQLBindCol** è un puntatore null.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|22003|Valore numerico non compreso nell'intervallo|La restituzione del valore numerico (come valore numerico o stringa) per una o più colonne avrebbe causato il troncamento dell'intero, anziché della parte frazionaria del numero.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)<br /><br /> Per ulteriori informazioni, vedere [linee guida per i tipi di dati interval e numeric](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md) in Appendice D: tipi di dati.|  
|22007|Formato DateTime non valido|Una colonna di tipo carattere nel set di risultati è stata associata a una struttura di data, ora o timestamp e un valore nella colonna era, rispettivamente, una data, un'ora o un timestamp non valido.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|22012|Divisione per zero|È stato restituito un valore da un'espressione aritmetica, che ha comportato la divisione per zero.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|22015|Overflow del campo Interval|L'assegnazione da un tipo SQL esatto o intervallo di tipo SQL a un tipo intervallo C ha causato la perdita di cifre significative nel campo iniziali.<br /><br /> Quando si recuperano i dati in un tipo intervallo C, non esiste alcuna rappresentazione del valore del tipo SQL nel tipo intervallo C.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|22018|Valore di carattere non valido per la specifica del cast|Il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. e il valore nella colonna non è un valore letterale valido del tipo C associato.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|24000|Stato del cursore non valido|Lo stato di *statementHandle* è stato eseguito, ma nessun set di risultati è stato associato a *statementHandle*.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLError** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* e quindi la funzione è stata chiamata nuovamente su *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLExtendedFetch** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) il *statementHandle* specificato non si trova in uno stato eseguito. La funzione è stata chiamata senza prima chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) **SQLExtendedFetch** è stato chiamato per *statementHandle* dopo la chiamata a **SQLFetch** o **SQLFetchScroll** e prima della chiamata a **SQLFreeStmt** con l'opzione SQL_CLOSE.<br /><br /> (DM) **SQLBulkOperations** è stato chiamato per un'istruzione prima della chiamata a **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch** e quindi **SQLExtendedFetch** è stato chiamato prima della chiamata di **SQLFreeStmt** con l'opzione SQL_CLOSE.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY106|Tipo di recupero non compreso nell'intervallo|(DM) il valore specificato per l'argomento *FetchOrientation* non è valido. (Vedere "Commenti").<br /><br /> L'argomento *FetchOrientation* è stato SQL_FETCH_BOOKMARK e l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è stato impostato su SQL_UB_OFF.<br /><br /> Il valore dell'opzione dell'istruzione SQL_CURSOR_TYPE è stato SQL_CURSOR_FORWARD_ONLY e il valore dell'argomento *FetchOrientation* non è stato SQL_FETCH_NEXT.<br /><br /> L'argomento *FetchOrientation* è stato SQL_FETCH_RESUME.|  
|HY107|Valore di riga non compreso nell'intervallo|Il valore specificato con l'opzione dell'istruzione SQL_CURSOR_TYPE è stato SQL_CURSOR_KEYSET_DRIVEN, ma il valore specificato con l'attributo SQL_KEYSET_SIZE istruzione è maggiore di 0 e minore del valore specificato con l'attributo dell'istruzione SQL_ROWSET_SIZE.|  
|HY111|Valore segnalibro non valido|L'argomento *FetchOrientation* è stato SQL_FETCH_BOOKMARK e il segnalibro specificato nell'argomento *FetchOffset* non è valido.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta il tipo di recupero specificato.<br /><br /> Il driver o l'origine dati non supporta la conversione specificata dalla combinazione di *targetType* in **SQLBindCol** e del tipo di dati SQL della colonna corrispondente. Questo errore si verifica solo quando è stato eseguito il mapping del tipo di dati SQL della colonna a un tipo di dati SQL specifico del driver.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtOption**, SQL_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Il comportamento di **SQLExtendedFetch** è identico a quello di **SQLFetchScroll**, con le eccezioni seguenti:  
  
-   **SQLExtendedFetch** e **SQLFetchScroll** usano metodi diversi per restituire il numero di righe recuperate. **SQLExtendedFetch** restituisce il numero di righe recuperate in *\* RowCountPtr*; **SQLFetchScroll** restituisce il numero di righe recuperate direttamente nel buffer a cui punta SQL_ATTR_ROWS_FETCHED_PTR. Per ulteriori informazioni, vedere l'argomento *RowCountPtr* .  
  
-   **SQLExtendedFetch** e **SQLFetchScroll** restituiscono lo stato di ogni riga in matrici diverse. Per ulteriori informazioni, vedere l'argomento *RowStatusArray* .  
  
-   **SQLExtendedFetch** e **SQLFetchScroll** usano metodi diversi per recuperare il segnalibro quando *FetchOrientation* è SQL_FETCH_BOOKMARK. **SQLExtendedFetch** non supporta segnalibri a lunghezza variabile o recupero di set di righe con un offset diverso da 0 da un segnalibro. Per ulteriori informazioni, vedere l'argomento *FetchOffset* .  
  
-   **SQLExtendedFetch** e **SQLFetchScroll** usano dimensioni del set di righe diverse. **SQLExtendedFetch** usa il valore dell'attributo dell'istruzione SQL_ROWSET_SIZE e **SQLFetchScroll** usa il valore dell'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE.  
  
-   **SQLExtendedFetch** presenta una semantica di gestione degli errori leggermente diversa rispetto a **SQLFetchScroll**. Per ulteriori informazioni, vedere "gestione degli errori" nella sezione "Commenti" di [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
-   **SQLExtendedFetch** non supporta gli offset di binding (attributo dell'istruzione SQL_ATTR_ROW_BIND_OFFSET_PTR).  
  
-   Le chiamate a **SQLExtendedFetch** non possono essere combinate con chiamate a **SQLFetch** o **SQLFetchScroll** e, se **SQLBulkOperations** viene chiamato prima della chiamata di una funzione di recupero, **SQLExtendedFetch** non può essere chiamato finché il cursore non viene chiuso e riaperto. Ovvero, **SQLExtendedFetch** può essere chiamato solo nello stato dell'istruzione S7. Per ulteriori informazioni, vedere [transizioni di istruzioni](../../../odbc/reference/appendixes/statement-transitions.md) nell'Appendice B: tabelle di transizione dello stato ODBC.  
  
 Quando un'applicazione chiama **SQLFetchScroll** quando si utilizza un driver ODBC 2 *. x* , gestione driver esegue il mapping della chiamata a **SQLExtendedFetch**. Per ulteriori informazioni, vedere "SQLFetchScroll and ODBC 2 *. x* Drivers" in [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 In ODBC 2 *. x*, **SQLExtendedFetch** è stato chiamato per recuperare più righe ed è stato chiamato **SQLFetch** per recuperare una singola riga. In ODBC 3 *. x*, invece, è possibile chiamare **SQLFetch** per recuperare più righe.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Esecuzione di operazioni bulk di inserimento, aggiornamento o eliminazione|[Funzione SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su una colonna in un set di risultati|[Funzione SQLDescribeCol ](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Restituzione del numero di colonne del set di risultati|[Funzione SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Posizionamento del cursore, aggiornamento dei dati nel set di righe o aggiornamento o eliminazione di dati nel set di risultati|[Funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
