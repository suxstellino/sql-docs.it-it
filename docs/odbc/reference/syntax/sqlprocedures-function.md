---
description: Funzione SQLProcedures
title: Funzione SQLProcedures | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLProcedures
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedures
helpviewer_keywords:
- SQLProcedures function [ODBC]
ms.assetid: d0d9ef10-2fd4-44a5-9334-649f186f4ba0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8b3c39243e663f58975918abfa9938d098e6f54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192563"
---
# <a name="sqlprocedures-function"></a>Funzione SQLProcedures
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLProcedures** restituisce l'elenco dei nomi di stored procedure archiviati in un'origine dati specifica. La *stored procedure* è un termine generico utilizzato per descrivere un *oggetto eseguibile* o un'entità denominata che può essere richiamata utilizzando parametri di input e output. Per ulteriori informazioni sulle procedure, vedere le [procedure](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLProcedures(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      ProcName,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *CatalogName*  
 Input Catalogo delle procedure. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non contengono cataloghi. *CatalogName* non può contenere un criterio di ricerca di stringhe.  
  
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
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLProcedures** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti comunemente da **SQLProcedures** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Un cursore è stato aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|L'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE, l'argomento *CatalogName* è un puntatore null e il SQL_CATALOG_NAME *InfoType* restituisce i nomi dei cataloghi supportati.<br /><br /> (DM) l'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE e l'argomento *SchemaName* o *ProcName* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore di uno degli argomenti della lunghezza del nome è minore di 0 ma non uguale a SQL_NTS.<br /><br /> Il valore di uno degli argomenti della lunghezza del nome supera il valore di lunghezza massima per il nome corrispondente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|È stato specificato un catalogo delle procedure e il driver o l'origine dati non supporta i cataloghi.<br /><br /> È stato specificato uno schema di routine e il driver o l'origine dati non supporta gli schemi.<br /><br /> È stato specificato un criterio di ricerca di stringhe per lo schema della routine o il nome della procedura e l'origine dati non supporta i criteri di ricerca per uno o più di tali argomenti.<br /><br /> La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati richiesto. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta questa funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLProcedures** elenca tutte le routine nell'intervallo richiesto. È possibile che un utente non disponga delle autorizzazioni necessarie per eseguire una di queste procedure. Per controllare l'accessibilità, un'applicazione può chiamare **SQLGetInfo** e controllare il valore di SQL_ACCESSIBLE_PROCEDURES Information. In caso contrario, l'applicazione deve essere in grado di gestire una situazione in cui l'utente seleziona una procedura che non può essere eseguita. Per informazioni sul modo in cui queste informazioni possono essere utilizzate, vedere [procedure](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedures** restituisce i risultati come set di risultati standard, ordinati in base PROCEDURE_CAT, PROCEDURE_SCHEMA e procedure_name.  
  
> [!NOTE]  
>  **SQLProcedures** non può restituire tutte le routine. Le applicazioni possono utilizzare qualsiasi routine valida, indipendentemente dal fatto che venga restituita da **SQLProcedures**.  
  
 Le colonne seguenti sono state rinominate per ODBC 3 *. x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|Colonna ODBC 3 *. x*|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|_OWNER PROCEDURE|_SCHEM PROCEDURE|  
  
 Per determinare le lunghezze effettive delle colonne PROCEDURE_CAT, PROCEDURE_SCHEM e PROCEDURE_NAME, un'applicazione può chiamare **SQLGetInfo** con le opzioni SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN e SQL_MAX_PROCEDURE_NAME_LEN.  
  
 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 8 (PROCEDURE_TYPE) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nome colonna|Numero di colonna|Tipo di dati|Commenti|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC 2,0)|1|Varchar|Identificatore del catalogo delle procedure. NULL se non è applicabile all'origine dati. Se un driver supporta i cataloghi per alcune procedure ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le procedure che non dispongono di cataloghi.|  
|PROCEDURE_SCHEM (ODBC 2,0)|2|Varchar|Identificatore schema di routine; NULL se non è applicabile all'origine dati. Se un driver supporta schemi per alcune procedure ma non per altri, ad esempio quando il driver recupera dati da sistemi DBMS diversi, restituisce una stringa vuota ("") per le procedure che non dispongono di schemi.|  
|PROCEDURE_NAME (ODBC 2,0)|3|Varchar NOT NULL|Identificatore di procedura.|  
|NUM_INPUT_PARAMS (ODBC 2,0)|4|N/D|Riservato per utilizzi futuri. Le applicazioni non devono basarsi sui dati restituiti nelle colonne di risultati.|  
|NUM_OUTPUT_PARAMS (ODBC 2,0)|5|N/D|Riservato per utilizzi futuri. Le applicazioni non devono basarsi sui dati restituiti nelle colonne di risultati.|  
|NUM_RESULT_SETS (ODBC 2,0)|6|N/D|Riservato per utilizzi futuri. Le applicazioni non devono basarsi sui dati restituiti nelle colonne di risultati.|  
|OSSERVAZIONI (ODBC 2,0)|7|Varchar|Descrizione della procedura.|  
|PROCEDURE_TYPE (ODBC 2,0)|8|Smallint|Definisce il tipo di procedura:<br /><br /> SQL_PT_UNKNOWN: non è possibile determinare se la procedura restituisce un valore.<br /><br /> SQL_PT_PROCEDURE: l'oggetto restituito è una routine. ovvero, non ha un valore restituito.<br /><br /> SQL_PT_FUNCTION: l'oggetto restituito è una funzione. ovvero ha un valore restituito.|  
  
 Gli argomenti *SchemaName* e *ProcName* accettano i criteri di ricerca. Per ulteriori informazioni sui criteri di ricerca validi, vedere argomenti relativi ai [valori di pattern](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [chiamate di routine](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di sola trasmissione|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Restituzione di informazioni su un driver o un'origine dati|[Funzione SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Restituzione dei parametri e delle colonne del set di risultati di una stored procedure|[Funzione SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|  
|Sintassi per la chiamata di stored procedure|[Esecuzione di istruzioni](../../../odbc/reference/develop-app/executing-statements-odbc.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
