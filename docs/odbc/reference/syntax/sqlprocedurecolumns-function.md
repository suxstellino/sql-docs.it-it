---
description: Funzione SQLProcedureColumns
title: Funzione SQLProcedureColumns | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLProcedureColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedureColumns
helpviewer_keywords:
- SQLProcedureColumns function [ODBC]
ms.assetid: 4ca37b28-a6df-465b-8988-d422d37fc025
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 63025751b1ec5e93f5889d77c373cbedb19bf543
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192576"
---
# <a name="sqlprocedurecolumns-function"></a>Funzione SQLProcedureColumns
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLProcedureColumns** restituisce l'elenco dei parametri di input e output, nonché le colonne che costituiscono il set di risultati per le procedure specificate. Il driver restituisce le informazioni come un set di risultati nell'istruzione specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLProcedureColumns(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     ProcName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *CatalogName*  
 Input Nome del catalogo delle procedure. Se un driver supporta i cataloghi per alcune procedure ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le procedure che non dispongono di cataloghi. *CatalogName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *CatalogName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *CatalogName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo. Per ulteriori informazioni, vedere [arguments in Catalog Functions](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Input Lunghezza in caratteri di **CatalogName*.  
  
 *SchemaName*  
 Input Modello di ricerca di stringhe per i nomi degli schemi di procedura. Se un driver supporta schemi per alcune procedure ma non per altri, ad esempio quando il driver recupera dati da sistemi DBMS diversi, una stringa vuota ("") indica le procedure che non dispongono di schemi.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *SchemaName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *SchemaName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength2*  
 Input Lunghezza in caratteri di **SchemaName*.  
  
 *ProcName*  
 Input Modello di ricerca di stringhe per i nomi delle procedure.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *ProcName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *ProcName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength3*  
 Input Lunghezza in caratteri di **ProcName*.  
  
 *ColumnName*  
 Input Modello di ricerca di stringhe per i nomi di colonna.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *ColumnName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *ColumnName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength4*  
 Input Lunghezza in caratteri di **ColumnName*.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLProcedureColumns** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLProcedureColumns** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Un cursore è stato aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLError** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|L'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE, l'argomento *CatalogName* è un puntatore null e il SQL_CATALOG_NAME *InfoType* restituisce i nomi dei cataloghi supportati.<br /><br /> (DM) l'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE e l'argomento *SchemaName*, *ProcName* o *ColumnName* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione aynschronous era ancora in esecuzione quando è stata chiamata la funzione SQLProcedureColumns.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore di uno degli argomenti della lunghezza del nome è minore di 0 ma non uguale a SQL_NTS.<br /><br /> Il valore di uno degli argomenti della lunghezza del nome supera il valore di lunghezza massima per il nome del catalogo, dello schema, della routine o della colonna corrispondente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|È stato specificato un catalogo delle procedure e il driver o l'origine dati non supporta i cataloghi.<br /><br /> È stato specificato uno schema di routine e il driver o l'origine dati non supporta gli schemi.<br /><br /> È stato specificato un criterio di ricerca di stringhe per lo schema della routine, il nome della procedura o il nome della colonna e l'origine dati non supporta i criteri di ricerca per uno o più di questi argomenti.<br /><br /> La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 Questa funzione viene in genere utilizzata prima dell'esecuzione dell'istruzione per recuperare informazioni sui parametri di routine e sulle colonne che costituiscono il set di risultati o i set restituiti dalla procedura, se presente. Per altre informazioni, vedere [Routine](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  **SQLProcedureColumns** potrebbe non restituire tutte le colonne utilizzate da una routine. Un driver, ad esempio, potrebbe restituire solo le informazioni sui parametri utilizzati da una stored procedure e non sulle colonne di un set di risultati che genera.  
  
 Gli argomenti *SchemaName*, *ProcName* e *ColumnName* accettano i criteri di ricerca. Per ulteriori informazioni sui criteri di ricerca validi, vedere argomenti relativi ai [valori di pattern](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedureColumns** restituisce i risultati come set di risultati standard, ordinati in base PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME e COLUMN_TYPE. I nomi di colonna vengono restituiti per ogni routine nell'ordine seguente: il nome del valore restituito, i nomi di ogni parametro nella chiamata di procedura (in ordine di chiamata) e quindi i nomi di ogni colonna nel set di risultati restituito dalla procedura (in ordine di colonna).  
  
 Le applicazioni devono associare le colonne specifiche del driver rispetto alla fine del set di risultati. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
 Per determinare le lunghezze effettive delle colonne PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME e COLUMN_NAME, un'applicazione può chiamare **SQLGetInfo** con le opzioni SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_PROCEDURE_NAME_LEN e SQL_MAX_COLUMN_NAME_LEN.  
  
 Le colonne seguenti sono state rinominate per ODBC 3. *x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|ODBC 3. colonna *x*|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|_OWNER PROCEDURE|PROCEDURE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Le colonne seguenti sono state aggiunte al set di risultati restituito da **SQLProcedureColumns** per ODBC 3. *x*:  
  
-   COLUMN_DEF  
  
-   DATETIME_CODE  
  
-   CHAR_OCTET_LENGTH  
  
-   ORDINAL_POSITION  
  
-   IS_NULLABLE  
  
 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 19 (IS_NULLABLE) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nome colonna|Numero di colonna|Tipo di dati|Commenti|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC 2,0)|1|Varchar|Nome catalogo procedure; NULL se non è applicabile all'origine dati. Se un driver supporta i cataloghi per alcune procedure ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le procedure che non dispongono di cataloghi.|  
|PROCEDURE_SCHEM (ODBC 2,0)|2|Varchar|Nome schema procedura; NULL se non è applicabile all'origine dati. Se un driver supporta schemi per alcune procedure ma non per altri, ad esempio quando il driver recupera dati da sistemi DBMS diversi, restituisce una stringa vuota ("") per le procedure che non dispongono di schemi.|  
|PROCEDURE_NAME (ODBC 2,0)|3|Varchar NOT NULL|Nome della procedura. Viene restituita una stringa vuota per una routine che non ha un nome.|  
|COLUMN_NAME (ODBC 2,0)|4|Varchar NOT NULL|Nome della colonna della procedura. Il driver restituisce una stringa vuota per una colonna di routine che non ha un nome.|  
|COLUMN_TYPE (ODBC 2,0)|5|Smallint non NULL|Definisce la colonna procedure come parametro o colonna del set di risultati:<br /><br /> SQL_PARAM_TYPE_UNKNOWN: la colonna procedure è un parametro il cui tipo è sconosciuto. (ODBC 1,0)<br /><br /> SQL_PARAM_INPUT: la colonna procedure è un parametro di input. (ODBC 1,0)<br /><br /> SQL_PARAM_INPUT_OUTPUT: la colonna procedure è un parametro di input/output. (ODBC 1,0)<br /><br /> SQL_PARAM_OUTPUT: la colonna procedure è un parametro di output. (ODBC 2,0)<br /><br /> SQL_RETURN_VALUE: la colonna procedure è il valore restituito della stored procedure. (ODBC 2,0)<br /><br /> SQL_RESULT_COL: la colonna procedure è una colonna del set di risultati. (ODBC 1,0)|  
|DATA_TYPE (ODBC 2,0)|6|Smallint non NULL|Tipo di dati SQL. Può trattarsi di un tipo di dati SQL ODBC o di un tipo di dati SQL specifico del driver. Per i tipi di dati DateTime e Interval, questa colonna restituisce i tipi di dati concisi, ad esempio SQL_TYPE_TIME o SQL_INTERVAL_YEAR_TO_MONTH. Per un elenco dei tipi di dati ODBC SQL validi, vedere tipi di dati [SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.|  
|TYPE_NAME (ODBC 2,0)|7|Varchar NOT NULL|Nome del tipo di dati dipendente dall'origine dati; ad esempio, "CHAR", "VARCHAR", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA".|  
|COLUMN_SIZE (ODBC 2,0)|8|Integer|Dimensione della colonna della procedura nell'origine dati. Viene restituito NULL per i tipi di dati in cui le dimensioni della colonna non sono applicabili. Per ulteriori informazioni sulla precisione, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|BUFFER_LENGTH (ODBC 2,0)|9|Integer|Lunghezza in byte dei dati trasferiti in un'operazione **SQLGetData** o **SQLFetch** se viene specificato SQL_C_DEFAULT. Per i dati numerici, questa dimensione può essere diversa da quella dei dati archiviati nell'origine dati. Per ulteriori informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), in Appendice D: tipi di dati.|  
|DECIMAL_DIGITS (ODBC 2,0)|10|Smallint|Cifre decimali della colonna della procedura nell'origine dati. Viene restituito NULL per i tipi di dati in cui le cifre decimali non sono applicabili. Per ulteriori informazioni sulle cifre decimali, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), in Appendice D: tipi di dati.|  
|NUM_PREC_RADIX (ODBC 2,0)|11|Smallint|Per i tipi di dati numerici, 10 o 2.<br /><br /> Se è 10, i valori in COLUMN_SIZE e DECIMAL_DIGITS indicano il numero di cifre decimali consentito per la colonna. Una colonna DECIMAL (12, 5) restituisce ad esempio un NUM_PREC_RADIX di 10, un COLUMN_SIZE di 12 e un DECIMAL_DIGITS di 5. una colonna FLOAT può restituire un NUM_PREC_RADIX di 10, un COLUMN_SIZE di 15 e un DECIMAL_DIGITS NULL.<br /><br /> Se è 2, i valori in COLUMN_SIZE e DECIMAL_DIGITS forniscono il numero di bit consentiti nella colonna. Una colonna FLOAT, ad esempio, può restituire un NUM_PREC_RADIX di 2, una COLUMN_SIZE di 53 e un DECIMAL_DIGITS NULL.<br /><br /> Viene restituito NULL per i tipi di dati in cui NUM_PREC_RADIX non è applicabile.|  
|NULLABLE (ODBC 2,0)|12|Smallint non NULL|Indica se la colonna della procedura accetta un valore NULL:<br /><br /> SQL_NO_NULLS: la colonna procedure non accetta valori NULL.<br /><br /> SQL_NULLABLE: la colonna procedure accetta i valori NULL.<br /><br /> SQL_NULLABLE_UNKNOWN: non è noto se la colonna della procedura accetta valori NULL.|  
|OSSERVAZIONI (ODBC 2,0)|13|Varchar|Descrizione della colonna della procedura.|  
|COLUMN_DEF (ODBC 3,0)|14|Varchar|Valore predefinito della colonna.<br /><br /> Se è stato specificato NULL come valore predefinito, questa colonna è la parola NULL, non racchiusa tra virgolette. Se il valore predefinito non può essere rappresentato senza troncamento, la colonna contiene TRONCAta, senza virgolette singole di inclusione. Se non è stato specificato alcun valore predefinito, questa colonna è NULL.<br /><br /> Il valore di COLUMN_DEF può essere utilizzato per generare una nuova definizione di colonna, tranne quando contiene il valore TRONCAto.|  
|SQL_DATA_TYPE (ODBC 3,0)|15|Smallint non NULL|Il valore del tipo di dati SQL visualizzato nel campo SQL_DESC_TYPE del descrittore. Questa colonna corrisponde alla colonna DATA_TYPE, ad eccezione dei tipi di dati DateTime e Interval.<br /><br /> Per i tipi di dati DateTime e Interval, il campo SQL_DATA_TYPE nel set di risultati restituirà SQL_INTERVAL o SQL_DATETIME e il campo SQL_DATETIME_SUB restituirà il sottocodice per l'intervallo o il tipo di dati DateTime specifico. Vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|SQL_DATETIME_SUB (ODBC 3,0)|16|Smallint|Codice del sottotipo per i tipi di dati DateTime e Interval. Per gli altri tipi di dati in questa colonna viene restituito NULL.|  
|CHAR_OCTET_LENGTH (ODBC 3,0)|17|Integer|Lunghezza massima in byte di una colonna con tipo di dati character o Binary. Per gli tutti gli altri tipi di dati, il valore di questa colonna è NULL.|  
|ORDINAL_POSITION (ODBC 3,0)|18|Integer non NULL|Per i parametri di input e di output, la posizione ordinale del parametro nella definizione della procedura (in ordine crescente del parametro, a partire da 1). Per un valore restituito (se presente), viene restituito 0. Per le colonne del set di risultati, la posizione ordinale della colonna nel set di risultati, con la prima colonna del set di risultati che è il numero 1. Se sono presenti più set di risultati, le posizioni ordinali di colonna vengono restituite in modo specifico del driver.|  
|IS_NULLABLE (ODBC 3,0)|19|Varchar|"NO" se la colonna non include valori NULL.<br /><br /> "YES" se la colonna può includere valori NULL.<br /><br /> Quando non è noto se i valori Null sono supportati, in questa colonna viene restituita una stringa di lunghezza zero.<br /><br /> Per determinare il supporto di valori Null vengono seguite le regole ISO. In un sistema DBMS conforme a ISO SQL non vengono restituite stringhe vuote.<br /><br /> Il valore restituito per questa colonna è diverso dal valore restituito per la colonna NULLABLE. (Vedere la descrizione della colonna NULLABLE).|  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [chiamate di routine](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di sola trasmissione|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Restituzione di un elenco di procedure in un'origine dati|[Funzione SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
