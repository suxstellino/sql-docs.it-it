---
description: Funzione SQLGetData
title: Funzione SQLGetData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetData
helpviewer_keywords:
- SQLGetData function [ODBC]
ms.assetid: e3c1356a-5db7-4186-85fd-8b74633317e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42b81d50a83fef0be7c13604902cfe67b2d2dd89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201969"
---
# <a name="sqlgetdata-function"></a>Funzione SQLGetData
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetData** recupera i dati per una singola colonna nel set di risultati o per un singolo parametro dopo che **SQLParamData** restituisce SQL_PARAM_DATA_AVAILABLE. Può essere chiamato più volte per recuperare i dati a lunghezza variabile in parti.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetData(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   Col_or_Param_Num,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *Col_or_Param_Num*  
 Input Per il recupero dei dati della colonna, è il numero della colonna per la quale restituire i dati. Le colonne del set di risultati sono numerate in base a un ordine crescente di colonne a partire da 1. La colonna del segnalibro è il numero di colonna 0; Questa operazione può essere specificata solo se i segnalibri sono abilitati.  
  
 Per recuperare i dati dei parametri, è il numero ordinale del parametro, che inizia da 1.  
  
 *TargetType*  
 Input Identificatore di tipo del tipo di dati C del buffer **TargetValuePtr* . Per un elenco dei tipi di dati C e degli identificatori di tipi validi, vedere la sezione tipi di dati [c](../../../odbc/reference/appendixes/c-data-types.md) in Appendice D: tipi di dati.  
  
 Se *targetType* è SQL_ARD_TYPE, il driver usa l'identificatore del tipo specificato nel campo SQL_DESC_CONCISE_TYPE di ARD. Se *targetType* è SQL_APD_TYPE, **SQLGetData** utilizzerà lo stesso tipo di dati C specificato in **SQLBindParameter**. In caso contrario, il tipo di dati C specificato in **SQLGetData** sostituisce il tipo di dati c specificato in **SQLBindParameter**. Se è SQL_C_DEFAULT, il driver seleziona il tipo di dati C predefinito in base al tipo di dati SQL dell'origine.  
  
 È inoltre possibile specificare un tipo di dati C esteso. Per ulteriori informazioni, vedere [tipi di dati C in ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 Output Puntatore al buffer in cui restituire i dati.  
  
 *TargetValuePtr* non può essere null.  
  
 *BufferLength*  
 Input Lunghezza del buffer **TargetValuePtr* in byte.  
  
 Il driver utilizza *bufferLength* per evitare di scrivere oltre la fine del \* buffer *TargetValuePtr* quando vengono restituiti dati a lunghezza variabile, ad esempio dati di tipo carattere o binario. Si noti che il driver conta il carattere di terminazione null in caso di restituzione di dati di tipo carattere a \* *TargetValuePtr*. **TargetValuePtr* deve quindi contenere spazio per il carattere di terminazione null, altrimenti i dati vengono troncati dal driver.  
  
 Quando il driver restituisce dati a lunghezza fissa, ad esempio un numero intero o una struttura di data, il driver ignora *bufferLength* e presuppone che il buffer sia sufficientemente grande da consentire l'inserimento dei dati. È quindi importante che l'applicazione allochi un buffer sufficientemente grande per i dati a lunghezza fissa o che il driver scriva oltre la fine del buffer.  
  
 **SQLGetData** restituisce SQLSTATE HY090 (lunghezza di stringa o di buffer non valida) se *bufferLength* è minore di 0 ma non quando *bufferLength* è 0.  
  
 *StrLen_or_IndPtr*  
 Output Puntatore al buffer in cui restituire la lunghezza o il valore dell'indicatore. Se è un puntatore null, non viene restituito alcun valore di lunghezza o indicatore. Viene restituito un errore quando i dati recuperati sono NULL.  
  
 **SQLGetData** può restituire i valori seguenti nel buffer di lunghezza/indicatore:  
  
-   Lunghezza dei dati disponibili per la restituzione  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 Per ulteriori informazioni, vedere [utilizzo di valori di lunghezza/indicatore](../../../odbc/reference/develop-app/using-length-and-indicator-values.md) e "Commenti" in questo argomento.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetData** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti comunemente da **SQLGetData** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Non tutti i dati per la colonna specificata, *Col_or_Param_Num*, possono essere recuperati in una singola chiamata alla funzione. SQL_NO_TOTAL o la lunghezza dei dati rimanenti nella colonna specificata prima della chiamata corrente a **SQLGetData** viene restituita \* *StrLen_or_IndPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)<br /><br /> Per ulteriori informazioni sull'utilizzo di più chiamate a **SQLGetData** per una singola colonna, vedere "Commenti".|  
|01S07|Troncamento frazionario|I dati restituiti per una o più colonne sono stati troncati. Per i tipi di dati numerici, la parte frazionaria del numero è stata troncata. Per i tipi di dati time, timestamp e Interval contenenti un componente ora, la parte frazionaria del tempo è stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Il valore dei dati di una colonna nel set di risultati non può essere convertito nel tipo di dati C specificato dall'argomento *targetType*.|  
|07009|Indice del descrittore non valido|Il valore specificato per l'argomento *Col_or_Param_Num* è 0 e l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è stato impostato su SQL_UB_OFF.<br /><br /> Il valore specificato per l'argomento *Col_or_Param_Num* è maggiore del numero di colonne nel set di risultati.<br /><br /> Il valore di *Col_or_Param_Num* non è uguale al numero ordinale del parametro disponibile.<br /><br /> (DM) la colonna specificata è stata associata. Questa descrizione non si applica ai driver che restituiscono la maschera di maschera SQL_GD_BOUND per l'opzione di SQL_GETDATA_EXTENSIONS in **SQLGetInfo**.<br /><br /> (DM) il numero della colonna specificata è minore o uguale al numero della colonna con binding più alto. Questa descrizione non si applica ai driver che restituiscono la maschera di maschera SQL_GD_ANY_COLUMN per l'opzione di SQL_GETDATA_EXTENSIONS in **SQLGetInfo**.<br /><br /> (DM) l'applicazione ha già chiamato **SQLGetData** per la riga corrente. il numero della colonna specificata nella chiamata corrente è inferiore al numero della colonna specificata nella chiamata precedente; il driver non restituisce quindi la maschera di SQL_GD_ANY_ORDER per l'opzione di SQL_GETDATA_EXTENSIONS in **SQLGetInfo**.<br /><br /> (DM) l'argomento *targetType* è stato SQL_ARD_TYPE e il record del descrittore di *Col_or_Param_Num* in ARD non ha superato la verifica della coerenza.<br /><br /> (DM) l'argomento *targetType* è stato SQL_ARD_TYPE e il valore nel campo SQL_DESC_COUNT di ARD è inferiore all'argomento *Col_or_Param_Num* .|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22002|Variabile indicatore obbligatoria ma non fornita|*StrLen_or_IndPtr* è un puntatore null e sono stati recuperati dati null.|  
|22003|Valore numerico non compreso nell'intervallo|La restituzione del valore numerico (come numeric o String) per la colonna avrebbe causato il troncamento dell'intero oggetto, anziché della parte frazionaria del numero.<br /><br /> Per ulteriori informazioni, vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Formato DateTime non valido|La colonna di tipo carattere nel set di risultati è stata associata a una struttura di data, ora o timestamp del linguaggio C e il valore nella colonna era rispettivamente una data, un'ora o un timestamp non valido. Per ulteriori informazioni, vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22012|Divisione per zero|È stato restituito un valore da un'espressione aritmetica che ha generato la divisione per zero.|  
|22015|Overflow del campo Interval|L'assegnazione da un tipo SQL esatto o intervallo di tipo SQL a un tipo intervallo C ha causato la perdita di cifre significative nel campo iniziali.<br /><br /> Quando si restituiscono dati a un tipo intervallo C, non esiste alcuna rappresentazione del valore del tipo SQL nel tipo intervallo C.|  
|22018|Valore di carattere non valido per la specifica del cast|Una colonna di tipo carattere nel set di risultati è stata restituita a un buffer C di caratteri e la colonna conteneva un carattere per il quale non era presente alcuna rappresentazione nel set di caratteri del buffer.<br /><br /> Il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. e il valore nella colonna non è un valore letterale valido del tipo C associato.|  
|24000|Stato del cursore non valido|(DM) la funzione è stata chiamata senza prima chiamare **SQLFetch** o **SQLFetchScroll** per posizionare il cursore sulla riga di dati richiesta.<br /><br /> (DM) lo stato di *statementHandle* è stato eseguito, ma nessun set di risultati è stato associato a *statementHandle*.<br /><br /> Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato, ma il cursore è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY003|Tipo di programma non compreso nell'intervallo|(DM) l'argomento *targetType* non è un tipo di dati valido, SQL_C_DEFAULT SQL_ARD_TYPE (in caso di recupero dei dati della colonna) o SQL_APD_TYPE (in caso di recupero di dati dei parametri).<br /><br /> (DM) l'argomento *Col_or_Param_Num* è 0 e l'argomento *targetType* non è stato SQL_C_BOOKMARK per un segnalibro a lunghezza fissa o SQL_C_VARBOOKMARK per un segnalibro a lunghezza variabile.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* e quindi la funzione è stata chiamata nuovamente su *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread, quindi la funzione è stata chiamata nuovamente sul *statementHandle*.|  
|HY009|Uso non valido del puntatore null|(DM) l'argomento *TargetValuePtr* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) il *statementHandle* specificato non si trova in uno stato eseguito. La funzione è stata chiamata senza prima chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLGetData** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) lo stato di *statementHandle* è stato eseguito, ma nessun set di risultati è stato associato a *statementHandle*.<br /><br /> Una chiamata a **SQLExeceute**, **SQLExecDirect** o **SQLMoreResults** ha restituito SQL_PARAM_DATA_AVAILABLE, ma è stato chiamato **SQLGetData** , anziché **SQLParamData**.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *bufferLength* è minore di 0.<br /><br /> Il valore specificato per l'argomento *bufferLength* è minore di 4, l'argomento *Col_or_Param_Num* è stato impostato su 0 e il driver è un driver ODBC 2 *. x* .|  
|HY109|Posizione del cursore non valida|Il cursore è stato posizionato (da **SQLSetPos**, **SQLFetch**, **SQLFetchScroll** o **SQLBulkOperations**) su una riga che è stata eliminata o che non è stato possibile recuperare.<br /><br /> Il cursore era un cursore di sola trasmissione e la dimensione del set di righe era maggiore di uno.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta l'utilizzo di **SQLGetData** con più righe in **SQLFetchScroll**. Questa descrizione non si applica ai driver che restituiscono la maschera di maschera SQL_GD_BLOCK per l'opzione di SQL_GETDATA_EXTENSIONS in **SQLGetInfo**.<br /><br /> Il driver o l'origine dati non supporta la conversione specificata dalla combinazione dell'argomento *targetType* e del tipo di dati SQL della colonna corrispondente. Questo errore si verifica solo quando è stato eseguito il mapping del tipo di dati SQL della colonna a un tipo di dati SQL specifico del driver.<br /><br /> Il driver supporta solo ODBC 2 *. x* e l'argomento *targetType* è uno dei seguenti:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> e uno dei tipi di dati interval C elencati nei tipi di dati [c](../../../odbc/reference/appendixes/c-data-types.md) in Appendice D: tipi di dati.<br /><br /> Il driver supporta solo le versioni ODBC precedenti alla 3,50 e l'argomento *targetType* è stato SQL_C_GUID.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver corrispondente a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLGetData** restituisce i dati in una colonna specificata. **SQLGetData** può essere chiamato solo dopo che sono state recuperate una o più righe dal set di risultati di **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch**. Se i dati a lunghezza variabile sono troppo grandi per essere restituiti in una singola chiamata a **SQLGetData** (a causa di una limitazione nell'applicazione), **SQLGetData** può recuperarla in parti. È possibile associare alcune colonne in una riga e chiamare **SQLGetData** per altri, sebbene questo sia soggetto ad alcune restrizioni. Per ulteriori informazioni, vedere [recupero di dati Long](../../../odbc/reference/develop-app/getting-long-data.md).  
  
 Per informazioni sull'uso di **SQLGetData** con i parametri di output trasmessi, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-sqlgetdata"></a>Utilizzo di SQLGetData  
 Se il driver non supporta le estensioni di **SQLGetData**, la funzione può restituire dati solo per le colonne non associate con un numero maggiore di quello dell'ultima colonna associata. Inoltre, all'interno di una riga di dati, il valore dell'argomento *Col_or_Param_Num* in ogni chiamata a **SQLGetData** deve essere maggiore o uguale al valore di *Col_or_Param_Num* nella chiamata precedente; ovvero i dati devono essere recuperati in un ordine di numero di colonna crescente. Infine, se non sono supportate estensioni, non è possibile chiamare **SQLGetData** se la dimensione del set di righe è maggiore di 1.  
  
 I driver possono attenuare le restrizioni. Per determinare le restrizioni che un driver si distende, un'applicazione chiama **SQLGetInfo** con una delle opzioni SQL_GETDATA_EXTENSIONS seguenti:  
  
-   SQL_GD_OUTPUT_PARAMS = **SQLGetData** può essere chiamato per restituire i valori dei parametri di output. Per altre informazioni, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_GD_ANY_COLUMN. Se viene restituita questa opzione, è possibile chiamare **SQLGetData** per qualsiasi colonna non associata, inclusi quelli prima dell'ultima colonna associata.  
  
-   SQL_GD_ANY_ORDER. Se viene restituita questa opzione, è possibile chiamare **SQLGetData** per le colonne non vincolate in qualsiasi ordine.  
  
-   SQL_GD_BLOCK. Se questa opzione viene restituita da **SQLGetInfo** per l'SQL_GETDATA_EXTENSIONS InfoType, il driver supporta le chiamate a **SQLGetData** quando la dimensione del set di righe è maggiore di 1 e l'applicazione può chiamare **SQLSetPos** con l'opzione SQL_POSITION per posizionare il cursore sulla riga corretta prima di chiamare **SQLGetData.**  
  
-   SQL_GD_BOUND. Se viene restituita questa opzione, è possibile chiamare **SQLGetData** per le colonne con binding e per le colonne non vincolate.  
  
 Ci sono due eccezioni a queste restrizioni e la capacità di un driver di rilassarsi. Per prima cosa, **SQLGetData** non deve mai essere chiamato per un cursore di sola trasmissione quando la dimensione del set di righe è maggiore di 1. In secondo luogo, se un driver supporta i segnalibri, deve sempre supportare la possibilità di chiamare **SQLGetData** per la colonna 0, anche se non consente alle applicazioni di chiamare **SQLGetData** per altre colonne prima dell'ultima colonna associata. Quando un'applicazione utilizza un ODBC 2 *. driver x* , **SQLGetData** restituisce correttamente un segnalibro quando viene chiamato con *Col_or_Param_Num* uguale a 0 dopo una chiamata a **SQLFetch**, perché **SQLFetch** viene mappato da Gestione driver odbc 3 *. x* a **SQLExtendedFetch** con un *FetchOrientation* di SQL_FETCH_NEXT e **SQLGetData** con un *Col_or_Param_Num* di 0 viene mappato da Gestione driver ODBC 3 *. x* a **SQLGetStmtOption** con *fOption* di SQL_GET_BOOKMARK.  
  
 Impossibile utilizzare **SQLGetData** per recuperare il segnalibro per una riga appena inserita chiamando **SQLBulkOperations** con l'opzione SQL_ADD, perché il cursore non è posizionato sulla riga. Un'applicazione può recuperare il segnalibro per tale riga associando la colonna 0 prima di chiamare **SQLBulkOperations** con SQL_ADD, nel qual caso **SQLBulkOperations** restituisce il segnalibro nel buffer associato. **SQLFetchScroll** può quindi essere chiamato con SQL_FETCH_BOOKMARK per riposizionare il cursore su tale riga.  
  
 Se l'argomento *targetType* è un tipo di dati interval, per i dati vengono utilizzati rispettivamente la precisione principale (2) e la precisione dell'intervallo predefinito (6), come impostato nei campi SQL_DESC_DATETIME_INTERVAL_PRECISION e SQL_DESC_PRECISION di ARD. Se l'argomento *targetType* è un tipo di dati SQL_C_NUMERIC, per i dati vengono usate la precisione predefinita (definita dal driver) e la scala predefinita (0), come impostato nei campi SQL_DESC_PRECISION e SQL_DESC_SCALE di ARD. Se la precisione o la scala predefinita non è appropriata, l'applicazione deve impostare in modo esplicito il campo del descrittore appropriato mediante una chiamata a **SQLSetDescField** o **SQLSetDescRec**. Può impostare il campo SQL_DESC_CONCISE_TYPE su SQL_C_NUMERIC e chiamare **SQLGetData** con un argomento *targetType* di SQL_ARD_TYPE, che causerà l'uso dei valori di precisione e scala nei campi del descrittore.  
  
> [!NOTE]
>  In ODBC 2 *. x*, le applicazioni impostano *targetType* su SQL_C_DATE, SQL_C_TIME o SQL_C_TIMESTAMP per indicare che \* *TargetValuePtr* è una struttura di data, ora o timestamp. In ODBC 3 *. x*, le applicazioni impostano *targetType* su SQL_C_TYPE_DATE, SQL_C_TYPE_TIME o SQL_C_TYPE_TIMESTAMP. Gestione driver esegue i mapping appropriati, se necessario, in base alla versione dell'applicazione e del driver.  
  
## <a name="retrieving-variable-length-data-in-parts"></a>Recupero di dati Variable-Length in parti  
 **SQLGetData** può essere utilizzato per recuperare dati da una colonna contenente dati a lunghezza variabile in parti, ovvero quando l'identificatore del tipo di dati SQL della colonna è SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_WCHAR, SQL_WVARCHAR, SQL_WLONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY o un identificatore specifico del driver per un tipo a lunghezza variabile.  
  
 Per recuperare dati da una colonna in parti, l'applicazione chiama **SQLGetData** più volte in successione per la stessa colonna. A ogni chiamata, **SQLGetData** restituisce la parte successiva dei dati. È responsabilità dell'applicazione riassemblare le parti, prestando attenzione a rimuovere il carattere di terminazione null dalle parti intermedie dei dati di tipo carattere. Se sono presenti più dati da restituire o non è stato allocato un buffer sufficiente per il carattere di terminazione, **SQLGetData** restituisce SQL_SUCCESS_WITH_INFO e SQLSTATE 01004 (dati troncati). Quando restituisce l'ultima parte dei dati, **SQLGetData** restituisce SQL_SUCCESS. Non è possibile restituire né SQL_NO_TOTAL né zero sull'ultima chiamata valida per recuperare i dati da una colonna, perché l'applicazione non avrà alcun modo per conoscere la quantità di dati nel buffer dell'applicazione valida. Se **SQLGetData** viene chiamato dopo, restituisce SQL_NO_DATA. Per ulteriori informazioni, vedere la sezione successiva "recupero di dati con SQLGetData".  
  
 I segnalibri a lunghezza variabile possono essere restituiti in parti da **SQLGetData**. Come per gli altri dati, una chiamata a **SQLGetData** per restituire segnalibri a lunghezza variabile in parti restituirà SQLSTATE 01004 (dati stringa, troncati a destra) e SQL_SUCCESS_WITH_INFO quando saranno presenti più dati da restituire. Questa situazione è diversa rispetto al caso in cui un segnalibro a lunghezza variabile viene troncato da una chiamata a **SQLFetch** o **SQLFetchScroll**, che restituisce SQL_ERROR e SQLState 22001 (dati stringa, troncati a destra).  
  
 Impossibile utilizzare **SQLGetData** per restituire dati a lunghezza fissa in parti. Se **SQLGetData** viene chiamato più di una volta in una riga per una colonna contenente dati a lunghezza fissa, restituisce SQL_NO_DATA per tutte le chiamate successive alla prima.  
  
## <a name="retrieving-streamed-output-parameters"></a>Recupero dei parametri di output trasmessi  
 Se un driver supporta i parametri di output trasmessi, un'applicazione può chiamare **SQLGetData** con un buffer di piccole dimensioni molte volte per recuperare un valore di parametro di grandi dimensioni. Per ulteriori informazioni sul parametro di output trasmesso, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="retrieving-data-with-sqlgetdata"></a>Recupero di dati con SQLGetData  
 Per restituire i dati per la colonna specificata, **SQLGetData** esegue la sequenza di passaggi seguente:  
  
1.  Restituisce SQL_NO_DATA se sono già stati restituiti tutti i dati per la colonna.  
  
2.  Imposta \* *StrLen_or_IndPtr* su SQL_NULL_DATA se i dati sono null. Se i dati sono NULL e *StrLen_or_IndPtr* è un puntatore null, **SQLGetData** restituisce SQLState 22002 (variabile indicatore obbligatoria ma non specificata).  
  
     Se i dati della colonna non sono NULL, **SQLGetData** procede al passaggio 3.  
  
3.  Se l'attributo dell'istruzione SQL_ATTR_MAX_LENGTH è impostato su un valore diverso da zero, se la colonna contiene dati di tipo carattere o binario e se **SQLGetData** non è stato precedentemente chiamato per la colonna, i dati vengono troncati in SQL_ATTR_MAX_LENGTH byte.  
  
    > [!NOTE]  
    >  L'attributo SQL_ATTR_MAX_LENGTH Statement è progettato per ridurre il traffico di rete. Viene in genere implementato dall'origine dati, che tronca i dati prima di restituirli attraverso la rete. I driver e le origini dati non sono necessari per supportarla. Pertanto, per garantire che i dati vengano troncati a una determinata dimensione, un'applicazione deve allocare un buffer di tale dimensione e specificare le dimensioni nell'argomento *bufferLength* .  
  
4.  Converte i dati nel tipo specificato in *targetType*. Ai dati viene assegnata la precisione e la scala predefinite per quel tipo di dati. Se *targetType* è SQL_ARD_TYPE, viene usato il tipo di dati nel campo SQL_DESC_CONCISE_TYPE di ARD. Se *targetType* è SQL_ARD_TYPE, ai dati viene assegnata la precisione e la scala nei campi SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION e SQL_DESC_SCALE di ARD, a seconda del tipo di dati nel campo SQL_DESC_CONCISE_TYPE. Se la precisione o la scala predefinita non è appropriata, l'applicazione deve impostare in modo esplicito il campo del descrittore appropriato mediante una chiamata a **SQLSetDescField** o **SQLSetDescRec**.  
  
5.  Se i dati sono stati convertiti in un tipo di dati a lunghezza variabile, ad esempio carattere o binario, **SQLGetData** controlla se la lunghezza dei dati supera *bufferLength*. Se la lunghezza dei dati di tipo carattere, incluso il carattere di terminazione null, supera *bufferLength*, **SQLGetData** tronca i dati in *bufferLength* meno la lunghezza di un carattere di terminazione null. Quindi, termina i dati null. Se la lunghezza dei dati binari supera la lunghezza del buffer di dati, **SQLGetData** la tronca a *bufferLength* byte.  
  
     Se il buffer di dati fornito è troppo piccolo per mantenere il carattere di terminazione null, **SQLGetData** restituisce SQL_SUCCESS_WITH_INFO e SQLSTATE 01004.  
  
     **SQLGetData** non tronca mai i dati convertiti in tipi di dati a lunghezza fissa. si presuppone sempre che la lunghezza di **TargetValuePtr* sia la dimensione del tipo di dati.  
  
6.  Inserisce i dati convertiti e possibilmente troncati in \* *TargetValuePtr*. Si noti che **SQLGetData** non può restituire dati fuori riga.  
  
7.  Inserisce la lunghezza dei dati in \* *StrLen_or_IndPtr*. Se *StrLen_or_IndPtr* è un puntatore null, **SQLGetData** non restituisce la lunghezza.  
  
    -   Per i dati di tipo carattere o binario, questa è la lunghezza dei dati dopo la conversione e prima del troncamento a causa di *bufferLength*. Se il driver non è in grado di determinare la lunghezza dei dati dopo la conversione, come accade talvolta con i dati Long, restituisce SQL_SUCCESS_WITH_INFO e imposta la lunghezza su SQL_NO_TOTAL. (L'ultima chiamata a **SQLGetData** deve restituire sempre la lunghezza dei dati, non zero o SQL_NO_TOTAL). Se i dati sono stati troncati a causa dell'attributo SQL_ATTR_MAX_LENGTH Statement, il valore di questo attributo, anziché la lunghezza effettiva, viene inserito nel \* *StrLen_or_IndPtr*. Questo è dovuto al fatto che questo attributo è progettato per troncare i dati nel server prima della conversione, quindi il driver non è in grado di individuare la lunghezza effettiva. Quando **SQLGetData** viene chiamato più volte in successione per la stessa colonna, corrisponde alla lunghezza dei dati disponibili all'inizio della chiamata corrente. ovvero la lunghezza diminuisce con ogni chiamata successiva.  
  
    -   Per tutti gli altri tipi di dati, si tratta della lunghezza dei dati dopo la conversione. ovvero la dimensione del tipo in cui sono stati convertiti i dati.  
  
8.  Se i dati vengono troncati senza perdita di significato durante la conversione (ad esempio, il numero reale 1,234 viene troncato quando viene convertito nell'Integer 1) o perché *bufferLength* è troppo piccolo (ad esempio, la stringa "abcdef" viene inserita in un buffer a 4 byte), **SQLGetData** restituisce SQLSTATE 01004 (dati troncati) e SQL_SUCCESS_WITH_INFO. Se i dati vengono troncati senza perdita di significato a causa dell'attributo SQL_ATTR_MAX_LENGTH Statement, **SQLGetData** restituisce SQL_SUCCESS e non restituisce SQLSTATE 01004 (dati troncati).  
  
 Il contenuto del buffer dei dati associato (se **SQLGetData** viene chiamato su una colonna associata) e il buffer di lunghezza/indicatore non è definito se **SQLGetData** non restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO.  
  
 Le chiamate successive a **SQLGetData** recupereranno i dati dall'ultima colonna richiesta. gli offset precedenti diventano non validi. Ad esempio, quando viene eseguita la sequenza seguente:  
  
```cpp  
SQLGetData(icol=n), SQLGetData(icol=m), SQLGetData(icol=n)  
```  
  
 la seconda chiamata a SQLGetData (icol = n) recupera i dati dall'inizio della colonna n. Qualsiasi offset nei dati a causa di chiamate precedenti a **SQLGetData** per la colonna non è più valido.  
  
## <a name="descriptors-and-sqlgetdata"></a>Descrittori e SQLGetData  
 **SQLGetData** non interagisce direttamente con i campi del descrittore.  
  
 Se *targetType* è SQL_ARD_TYPE, viene usato il tipo di dati nel campo SQL_DESC_CONCISE_TYPE di ARD. Se *targetType* è SQL_ARD_TYPE o SQL_C_DEFAULT, ai dati viene assegnata la precisione e la scala nei campi SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION e SQL_DESC_SCALE di ARD, a seconda del tipo di dati nel campo SQL_DESC_CONCISE_TYPE.  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente, un'applicazione esegue un'istruzione **Select** per restituire un set di risultati di ID cliente, nomi e numeri di telefono ordinati in base al nome, all'ID e al numero di telefono. Per ogni riga di dati, chiama **SQLFetch** per posizionare il cursore alla riga successiva. Chiama **SQLGetData** per recuperare i dati recuperati; i buffer per i dati e il numero di byte restituito sono specificati nella chiamata a **SQLGetData**. Infine, stampa il nome, l'ID e il numero di telefono di ogni dipendente.  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 50  
  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   sCustID, cbName, cbAge, cbBirthday;  
SQLRETURN    retcode;  
SQLHSTMT     hstmt;  
  
retcode = SQLExecDirect(hstmt,  
   "SELECT CUSTID, NAME, PHONE FROM CUSTOMERS ORDER BY 2, 1, 3",  
   SQL_NTS);  
  
if (retcode == SQL_SUCCESS) {  
   while (TRUE) {  
      retcode = SQLFetch(hstmt);  
      if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO) {  
         show_error();  
      }  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
         /* Get data for columns 1, 2, and 3 */  
  
         SQLGetData(hstmt, 1, SQL_C_ULONG, &sCustID, 0, &cbCustID);  
         SQLGetData(hstmt, 2, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
         SQLGetData(hstmt, 3, SQL_C_CHAR, szPhone, PHONE_LEN,  
            &cbPhone);  
  
         /* Print the row of data */  
  
         fprintf(out, "%-5d %-*s %*s", sCustID, NAME_LEN-1, szName,   
            PHONE_LEN-1, szPhone);  
      } else {  
         break;  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Assegnazione dello spazio di archiviazione per una colonna in un set di risultati|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Esecuzione di operazioni bulk che non si riferiscono alla posizione del cursore a blocchi|[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Esecuzione di un'istruzione SQL|[SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di una singola riga di dati o di un blocco di dati in una direzione di solo avanti|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Invio di dati dei parametri in fase di esecuzione|[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Posizionamento del cursore, aggiornamento dei dati nel set di righe o aggiornamento o eliminazione di dati nel set di righe|[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
