---
description: Funzione SQLDescribeParam
title: Funzione SQLDescribeParam | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeParam
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeParam
helpviewer_keywords:
- SQLDescribeParam function [ODBC]
ms.assetid: 1f5b63c4-2f3e-44da-b155-876405302281
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8459ca7dd493ba8ab316f832951170f87a77664
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192036"
---
# <a name="sqldescribeparam-function"></a>Funzione SQLDescribeParam
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLDescribeParam** restituisce la descrizione di un marcatore di parametro associato a un'istruzione SQL preparata. Queste informazioni sono disponibili anche nei campi del dip.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLDescribeParam(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT *   DataTypePtr,  
      SQLULEN *       ParameterSizePtr,  
      SQLSMALLINT *   DecimalDigitsPtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *ParameterNumber*  
 Input Numero di marcatori di parametro ordinato sequenzialmente nell'ordine crescente del parametro, a partire da 1.  
  
 *DataTypePtr*  
 Output Puntatore a un buffer in cui restituire il tipo di dati SQL del parametro. Questo valore viene letto dal campo record SQL_DESC_CONCISE_TYPE di dpi. Si tratta di uno dei valori nella sezione [tipi di dati SQL](../../../odbc/reference/appendixes/sql-data-types.md) di Appendice D: tipi di dati o di un tipo di dati SQL specifico del driver.  
  
 In ODBC 3. *x*, SQL_TYPE_DATE, SQL_TYPE_TIME o SQL_TYPE_TIMESTAMP verranno restituiti rispettivamente in *\* DataTypePTR* per i dati di data, ora o timestamp, in ODBC 2.** verranno restituiti x, SQL_DATE, SQL_TIME o SQL_TIMESTAMP. Gestione driver esegue i mapping necessari quando ODBC 2. l'applicazione *x* funziona con ODBC 3. driver *x* o quando ODBC 3. l'applicazione *x* funziona con ODBC 2. driver *x* .  
  
 Quando *ColumnNumber* è uguale a 0 (per una colonna di segnalibro), SQL_BINARY viene restituito in *\* DataTypePTR* per i segnalibri a lunghezza variabile. (SQL_INTEGER viene restituito se i segnalibri vengono utilizzati da ODBC 3. applicazione *x* che utilizza ODBC 2. driver *x* o ODBC 2. applicazione *x* che utilizza un ODBC 3. driver *x* .)  
  
 Per altre informazioni, vedere [tipi di dati SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.  
  
 *ParameterSizePtr*  
 Output Puntatore a un buffer in cui restituire la dimensione, in caratteri, della colonna o dell'espressione del marcatore di parametro corrispondente come definito dall'origine dati. Per ulteriori informazioni sulle dimensioni delle colonne, vedere [dimensioni delle colonne, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *DecimalDigitsPtr*  
 Output Puntatore a un buffer in cui restituire il numero di cifre decimali della colonna o dell'espressione del parametro corrispondente come definito dall'origine dati. Per ulteriori informazioni sulle cifre decimali, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *NullablePtr*  
 Output Puntatore a un buffer in cui restituire un valore che indica se il parametro ammette valori NULL. Questo valore viene letto dal campo SQL_DESC_NULLABLE del dip. I tipi validi sono:  
  
-   SQL_NO_NULLS: il parametro non consente valori NULL (questo è il valore predefinito).  
  
-   SQL_NULLABLE: il parametro consente valori NULL.  
  
-   SQL_NULLABLE_UNKNOWN: il driver non è in grado di determinare se il parametro ammette valori NULL.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLDescribeParam** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLDescribeParam** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07009|Indice del descrittore non valido|(DM) il valore specificato per l'argomento *ParameterNumber* è minore di 1.<br /><br /> Il valore specificato per l'argomento *ParameterNumber* è maggiore del numero di parametri nell'istruzione SQL associata.<br /><br /> Il marcatore di parametro fa parte di un'istruzione non DML.<br /><br /> Il marcatore di parametro fa parte di un elenco di **selezione** .|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|21S01|L'elenco di valori di inserimento non corrisponde all'elenco di colonne|Il numero di parametri nell'istruzione **Insert** non corrisponde al numero di colonne nella tabella specificata nell'istruzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) la funzione è stata chiamata prima di chiamare **SQLPrepare** o **SQLExecDirect** per *statementHandle*.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLDescribeParam** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 I marcatori di parametro sono numerati in ordine crescente di parametri, a partire da 1, nell'ordine in cui sono visualizzati nell'istruzione SQL.  
  
 **SQLDescribeParam** non restituisce il tipo (input, input/output o output) di un parametro in un'istruzione SQL. Ad eccezione delle chiamate alle routine, tutti i parametri nelle istruzioni SQL sono parametri di input. Per determinare il tipo di ogni parametro in una chiamata a una routine, un'applicazione chiama **SQLProcedureColumns**.  
  
 Per ulteriori informazioni, vedere [Descrizione dei parametri](../../../odbc/reference/develop-app/describing-parameters.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente viene richiesto all'utente l'utilizzo di un'istruzione SQL, quindi viene preparata l'istruzione. Chiama quindi **SQLNumParams** per determinare se l'istruzione contiene parametri. Se l'istruzione contiene parametri, chiama **SQLDescribeParam** per descrivere tali parametri e **SQLBindParameter** per associarli. Infine, viene richiesto all'utente i valori di tutti i parametri, quindi viene eseguita l'istruzione.  
  
```cpp  
SQLCHAR       Statement[100];  
SQLSMALLINT   NumParams, i, DataType, DecimalDigits, Nullable;  
SQLUINTEGER   ParamSize;  
SQLHSTMT      hstmt;  
  
// Prompt the user for an SQL statement and prepare it.  
GetSQLStatement(Statement);  
SQLPrepare(hstmt, Statement, SQL_NTS);  
  
// Check to see if there are any parameters. If so, process them.  
SQLNumParams(hstmt, &NumParams);  
if (NumParams) {  
   // Allocate memory for three arrays. The first holds pointers to buffers in which  
   // each parameter value will be stored in character form. The second contains the  
   // length of each buffer. The third contains the length/indicator value for each  
   // parameter.  
   SQLPOINTER * PtrArray = (SQLPOINTER *) malloc(NumParams * sizeof(SQLPOINTER));  
   SQLINTEGER * BufferLenArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
   SQLINTEGER * LenOrIndArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
  
   for (i = 0; i < NumParams; i++) {  
   // Describe the parameter.  
   SQLDescribeParam(hstmt, i + 1, &DataType, &ParamSize, &DecimalDigits, &Nullable);  
  
   // Call a helper function to allocate a buffer in which to store the parameter  
   // value in character form. The function determines the size of the buffer from  
   // the SQL data type and parameter size returned by SQLDescribeParam and returns  
   // a pointer to the buffer and the length of the buffer.  
   AllocParamBuffer(DataType, ParamSize, &PtrArray[i], &BufferLenArray[i]);  
  
   // Bind the memory to the parameter. Assume that we only have input parameters.  
   SQLBindParameter(hstmt, i + 1, SQL_PARAM_INPUT, SQL_C_CHAR, DataType, ParamSize,  
         DecimalDigits, PtrArray[i], BufferLenArray[i],  
         &LenOrIndArray[i]);  
  
   // Prompt the user for the value of the parameter and store it in the memory  
   // allocated earlier. For simplicity, this function does not check the value  
   // against the information returned by SQLDescribeParam. Instead, the driver does  
   // this when the statement is executed.  
   GetParamValue(PtrArray[i], BufferLenArray[i], &LenOrIndArray[i]);  
   }  
}  
  
// Execute the statement.  
SQLExecute(hstmt);  
  
// Process the statement further, such as retrieving results (if any) and closing the  
// cursor (if any). Code not shown.  
  
// Free the memory allocated for each parameter and the memory allocated for the arrays  
// of pointers, buffer lengths, and length/indicator values.  
for (i = 0; i < NumParams; i++) free(PtrArray[i]);  
free(PtrArray);  
free(BufferLenArray);  
free(LenOrIndArray);  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Preparazione di un'istruzione per l'esecuzione|[Pagina relativa alla funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
