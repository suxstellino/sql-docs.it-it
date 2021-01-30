---
description: Funzione SQLGetTypeInfo
title: Funzione SQLGetTypeInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTypeInfo
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTypeInfo
helpviewer_keywords:
- SQLGetTypeInfo function [ODBC]
ms.assetid: bdedb044-8924-4ca4-85f3-8b37578e0257
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 55de323b6eace8fa2aa8b138f30ee93938122bc0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189715"
---
# <a name="sqlgettypeinfo-function"></a>Funzione SQLGetTypeInfo
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetTypeInfo** restituisce informazioni sui tipi di dati supportati dall'origine dati. Il driver restituisce le informazioni sotto forma di un set di risultati SQL. I tipi di dati devono essere utilizzati nelle istruzioni DDL (Data Definition Language).  
  
> [!IMPORTANT]  
>  Le applicazioni devono utilizzare i nomi dei tipi restituiti nella colonna TYPE_NAME del set di risultati **SQLGetTypeInfo** nelle istruzioni **ALTER TABLE** e **Create Table** . **SQLGetTypeInfo** può restituire più di una riga con lo stesso valore nella colonna DATA_TYPE.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetTypeInfo(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   DataType);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione per il set di risultati.  
  
 *DataType*  
 Input Tipo di dati SQL. Deve essere uno dei valori nella sezione tipi di [dati SQL](../../../odbc/reference/appendixes/sql-data-types.md) di Appendice D: tipi di dati o un tipo di dati SQL specifico del driver. SQL_ALL_TYPES specifica che devono essere restituite informazioni su tutti i tipi di dati.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetTypeInfo** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLGetTypeInfo** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Un attributo di istruzione specificato non è valido a causa di condizioni di lavoro di implementazione, quindi un valore simile è stato sostituito temporaneamente. (Chiamare **SQLGetStmtAttr** per determinare il valore temporaneamente sostituito). Il valore sostitutivo è valido per *statementHandle* finché il cursore non viene chiuso. Gli attributi di istruzione che è possibile modificare sono: SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_KEYSET_SIZE, SQL_ATTR_MAX_LENGTH, SQL_ATTR_MAX_ROWS, SQL_ATTR_QUERY_TIMEOUT e SQL_ATTR_SIMULATE_CURSOR. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Un set di risultati è aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY004|Tipo di dati SQL non valido|Il valore specificato per *il tipo di dati dell'argomento non* è né un identificatore del tipo di dati ODBC SQL valido né un identificatore del tipo di dati specifico del driver supportato dal driver.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*, quindi la funzione è stata chiamata e, prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e, prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLGetTypeInfo** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver corrispondente a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLGetTypeInfo** restituisce i risultati come set di risultati standard, ordinati in base DATA_TYPE e quindi in base a quanto viene eseguito il mapping del tipo di dati al tipo di dati SQL ODBC corrispondente. I tipi di dati definiti dall'origine dati hanno la precedenza sui tipi di dati definiti dall'utente. Di conseguenza, l'ordinamento non è necessariamente coerente ma può essere generalizzato come DATA_TYPE prima, seguito da TYPE_NAME, sia ascendente. Si supponga, ad esempio, che un'origine dati abbia definito tipi di dati INTEGER e COUNTER, in cui il contatore è a incremento automatico e che sia stato definito anche un tipo di dati definito dall'utente WHOLENUM. Che verrebbero restituiti nell'ordine INTEGER, WHOLENUM e COUNTER, perché WHOLENUM è associato a un tipo di dati SQL ODBC SQL_INTEGER, mentre il tipo di dati a incremento automatico, anche se supportato dall'origine dati, non esegue il mapping di un tipo di dati SQL ODBC. Per informazioni sul modo in cui queste informazioni possono essere utilizzate, vedere [istruzioni DDL](../../../odbc/reference/develop-app/ddl-statements.md).  
  
 Se l'argomento *DataType* specifica un tipo di dati valido per la versione di ODBC supportata dal driver, ma che non è supportato dal driver, verrà restituito un set di risultati vuoto.  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Le colonne seguenti sono state rinominate per ODBC 3. *x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|ODBC 3. colonna *x*|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|MONEY|FIXED_PREC_SCALE|  
|AUTO_INCREMENT|AUTO_UNIQUE_VALUE|  
  
 Le colonne seguenti sono state aggiunte al set di risultati restituito da **SQLGetTypeInfo** per ODBC 3. *x*:  
  
-   SQL_DATA_TYPE  
  
-   INTERVAL_PRECISION  
  
-   SQL_DATETIME_SUB  
  
-   NUM_PREC_RADIX  
  
 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 19 (INTERVAL_PRECISION) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
> [!NOTE]  
>  **SQLGetTypeInfo** potrebbe non restituire tutti i tipi di dati. Un driver, ad esempio, potrebbe non restituire tipi di dati definiti dall'utente. Le applicazioni possono utilizzare qualsiasi tipo di dati valido, indipendentemente dal fatto che venga restituito da **SQLGetTypeInfo**. I tipi di dati restituiti da **SQLGetTypeInfo** sono quelli supportati dall'origine dati. Sono destinate all'uso in istruzioni DDL (Data Definition Language). I driver possono restituire i dati del set di risultati utilizzando tipi di dati diversi dai tipi restituiti da **SQLGetTypeInfo**. Quando si crea il set di risultati per una funzione di catalogo, il driver può utilizzare un tipo di dati non supportato dall'origine dati.  
  
|Nome colonna|Colonna<br /><br /> d'acquisto|Tipo di dati|Commenti|  
|-----------------|-----------------------|---------------|--------------|  
|TYPE_NAME (ODBC 2,0)|1|Varchar NOT NULL|Nome del tipo di dati dipendente dall'origine dati; ad esempio, "CHAR ()", "VARCHAR ()", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA". Le applicazioni devono utilizzare questo nome nelle istruzioni **Create Table** e **ALTER TABLE** .|  
|DATA_TYPE (ODBC 2,0)|2|Smallint non NULL|Tipo di dati SQL. Può trattarsi di un tipo di dati SQL ODBC o di un tipo di dati SQL specifico del driver. Per i tipi di dati DateTime o Interval, questa colonna restituisce il tipo di dati conciso, ad esempio SQL_TYPE_TIME o SQL_INTERVAL_YEAR_TO_MONTH. Per un elenco dei tipi di dati ODBC SQL validi, vedere tipi di dati [SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.|  
|COLUMN_SIZE (ODBC 2,0)|3|Integer|Dimensione massima della colonna supportata dal server per questo tipo di dati. Per i dati numerici, si tratta della precisione massima. Per i dati di stringa, si tratta della lunghezza in caratteri. Per i tipi di dati DateTime, si tratta della lunghezza in caratteri della rappresentazione di stringa (presupponendo la precisione massima consentita del componente per i secondi frazionari). Viene restituito NULL per i tipi di dati in cui le dimensioni della colonna non sono applicabili. Per i tipi di dati interval, questo è il numero di caratteri nella rappresentazione in forma di carattere del valore letterale intervallo (come definito dalla precisione principale dell'intervallo. vedere [lunghezza del tipo di dati intervallo](../../../odbc/reference/appendixes/interval-data-type-length.md) in Appendice D: tipi di dati).<br /><br /> Per ulteriori informazioni sulle dimensioni delle colonne, vedere [dimensioni delle colonne, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|LITERAL_PREFIX (ODBC 2,0)|4|Varchar|Carattere o caratteri utilizzati per anteporre un valore letterale. ad esempio, una virgoletta singola (') per i tipi di dati carattere o 0x per i tipi di dati binari; Viene restituito NULL per i tipi di dati in cui non è applicabile un prefisso letterale.|  
|LITERAL_SUFFIX (ODBC 2,0)|5|Varchar|Carattere o caratteri utilizzati per terminare un valore letterale; ad esempio, una virgoletta singola (') per i tipi di dati carattere; Viene restituito NULL per i tipi di dati in cui non è applicabile un suffisso letterale.|  
|CREATE_PARAMS (ODBC 2,0)|6|Varchar|Elenco di parole chiave, separate da virgole, corrispondenti a ogni parametro che l'applicazione può specificare tra parentesi quando si usa il nome restituito nel campo TYPE_NAME. Le parole chiave nell'elenco possono essere una delle seguenti: lunghezza, precisione o scala. Vengono visualizzati nell'ordine in cui è necessario usare la sintassi. Ad esempio, CREATE_PARAMS per DECIMAL è "Precision, scale"; CREATE_PARAMS per VARCHAR sarebbe uguale a "length". Se non sono presenti parametri per la definizione del tipo di dati, viene restituito NULL. ad esempio, INTEGER.<br /><br /> Il driver fornisce il testo CREATE_PARAMS nella lingua del paese in cui viene usato.|  
|NULLABLE (ODBC 2,0)|7|Smallint non NULL|Indica se il tipo di dati accetta un valore NULL:<br /><br /> SQL_NO_NULLS se il tipo di dati non accetta valori NULL.<br /><br /> SQL_NULLABLE se il tipo di dati accetta valori NULL.<br /><br /> SQL_NULLABLE_UNKNOWN se non è noto se la colonna accetta valori NULL.|  
|CASE_SENSITIVE (ODBC 2,0)|8|Smallint non NULL|Indica se un tipo di dati character distingue tra maiuscole e minuscole nelle regole di confronto e nei confronti:<br /><br /> SQL_TRUE se il tipo di dati è un tipo di dati character e fa distinzione tra maiuscole e minuscole.<br /><br /> SQL_FALSE se il tipo di dati non è di tipo carattere o non fa distinzione tra maiuscole e minuscole.|  
|RICERCABILE (ODBC 2,0)|9|Smallint non NULL|Modalità di utilizzo del tipo di dati in una clausola **where** :<br /><br /> SQL_PRED_NONE se la colonna non può essere utilizzata in una clausola **where** . (Corrisponde al valore SQL_UNSEARCHABLE in ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR se la colonna può essere utilizzata in una clausola **where** , ma solo con il predicato **like** . (Corrisponde al valore SQL_LIKE_ONLY in ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC se la colonna può essere utilizzata in una clausola **where** con tutti gli operatori di confronto, ad eccezione di **like** (confronto, confronto quantificato, **between**, **Distinct**, **in**, **match** e **Unique**). (Corrisponde al valore SQL_ALL_EXCEPT_LIKE in ODBC 2. *x*.)<br /><br /> SQL_SEARCHABLE se la colonna può essere utilizzata in una clausola **where** con qualsiasi operatore di confronto.|  
|UNSIGNED_ATTRIBUTE (ODBC 2,0)|10|Smallint|Indica se il tipo di dati è senza segno:<br /><br /> SQL_TRUE se il tipo di dati è senza segno.<br /><br /> SQL_FALSE se il tipo di dati è firmato.<br /><br /> Viene restituito NULL se l'attributo non è applicabile al tipo di dati o il tipo di dati non è numerico.|  
|FIXED_PREC_SCALE (ODBC 2,0)|11|Smallint non NULL|Indica se il tipo di dati ha una precisione e una scala fisse predefinite (specifiche dell'origine dati), ad esempio un tipo di dati Money:<br /><br /> SQL_TRUE se ha una precisione e una scala fisse predefinite.<br /><br /> SQL_FALSE se non ha una precisione e una scala fisse predefinite.|  
|AUTO_UNIQUE_VALUE (ODBC 2,0)|12|Smallint|Indica se il tipo di dati viene incrementato automaticamente:<br /><br /> SQL_TRUE se il tipo di dati è incremento automatico.<br /><br /> SQL_FALSE se il tipo di dati non viene incrementato automaticamente.<br /><br /> Viene restituito NULL se l'attributo non è applicabile al tipo di dati o il tipo di dati non è numerico.<br /><br /> Un'applicazione può inserire valori in una colonna con questo attributo, ma in genere non può aggiornare i valori nella colonna.<br /><br /> Quando viene eseguita un'istruzione INSERT in una colonna con incremento automatico, viene inserito un valore univoco nella colonna in fase di inserimento. L'incremento non è definito, ma è specifico dell'origine dati. Un'applicazione non deve presupporre che una colonna con incremento automatico venga avviata in un determinato punto o incrementi di un determinato valore.|  
|LOCAL_TYPE_NAME (ODBC 2,0)|13|Varchar|Versione localizzata del nome del tipo di dati dipendente dall'origine dati. Se il nome localizzato non è supportato dall'origine dati, viene restituito NULL. Questo nome viene utilizzato solo per la visualizzazione, ad esempio nelle finestre di dialogo.|  
|MINIMUM_SCALE (ODBC 2,0)|14|Smallint|Scala minima del tipo di dati nell'origine dati. Se a un tipo di dati è associata una scala fissa, le colonne MINIMUM_SCALE e MAXIMUM_SCALE contengono entrambe lo stesso valore. Una colonna SQL_TYPE_TIMESTAMP, ad esempio, potrebbe avere una scala fissa per i secondi frazionari. Se la scala non è applicabile, viene restituito NULL. Per ulteriori informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|MAXIMUM_SCALE (ODBC 2,0)|15|Smallint|Scala massima del tipo di dati nell'origine dati. Se la scala non è applicabile, viene restituito NULL. Se la scala massima non viene definita separatamente nell'origine dati, ma viene invece definita come uguale alla precisione massima, questa colonna contiene lo stesso valore della colonna COLUMN_SIZE. Per ulteriori informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|SQL_DATA_TYPE (ODBC 3,0)|16|Smallint NON NULL|Il valore del tipo di dati SQL visualizzato nel campo SQL_DESC_TYPE del descrittore. Questa colonna corrisponde alla colonna DATA_TYPE, ad eccezione dei tipi di dati interval e DateTime.<br /><br /> Per i tipi di dati interval e DateTime, il campo SQL_DATA_TYPE nel set di risultati restituirà SQL_INTERVAL o SQL_DATETIME e il campo SQL_DATETIME_SUB restituirà il sottocodice per l'intervallo o il tipo di dati DateTime specifico. Vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|SQL_DATETIME_SUB (ODBC 3,0)|17|Smallint|Quando il valore di SQL_DATA_TYPE è SQL_DATETIME o SQL_INTERVAL, questa colonna contiene il sottocodice DateTime/Interval. Per i tipi di dati diversi da DateTime e Interval, questo campo è NULL.<br /><br /> Per i tipi di dati Interval o DateTime, il campo SQL_DATA_TYPE nel set di risultati restituirà SQL_INTERVAL o SQL_DATETIME e il campo SQL_DATETIME_SUB restituirà il sottocodice per l'intervallo o il tipo di dati DateTime specifico. Vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|NUM_PREC_RADIX (ODBC 3,0)|18|Integer|Se il tipo di dati è un tipo numerico approssimato, questa colonna contiene il valore 2 per indicare che COLUMN_SIZE specifica un numero di bit. Per i tipi numerici esatti, questa colonna contiene il valore 10 per indicare che COLUMN_SIZE specifica un numero di cifre decimali. Negli altri casi la colonna è NULL.|  
|INTERVAL_PRECISION (ODBC 3,0)|19|Smallint|Se il tipo di dati è un tipo di dati interval, questa colonna contiene il valore della precisione principale dell'intervallo. (Vedere [precisione del tipo di dati interval](../../../odbc/reference/appendixes/interval-data-type-precision.md) in Appendice D: tipi di dati.) In caso contrario, questa colonna è NULL.|  
  
 Le informazioni sugli attributi possono essere applicate ai tipi di dati o a colonne specifiche di un set di risultati. **SQLGetTypeInfo** restituisce informazioni sugli attributi associati ai tipi di dati; **SQLColAttribute** restituisce informazioni sugli attributi associati alle colonne in un set di risultati.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su una colonna in un set di risultati|[Funzione SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di sola trasmissione|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Restituzione di informazioni su un driver o un'origine dati|[Funzione SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
