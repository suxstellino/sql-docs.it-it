---
description: SQLGetFunctions Function
title: Funzione SQLGetFunctions | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetFunctions
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetFunctions
helpviewer_keywords:
- SQLGetFunctions function [ODBC]
ms.assetid: 0451d2f9-0f4f-46ba-b252-670956a52183
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: edc7b7875e3ee3f7512ee19a62d68a50a1617436
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081362"
---
# <a name="sqlgetfunctions-function"></a>SQLGetFunctions Function
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetFunctions** restituisce informazioni sul fatto che un driver supporti una specifica funzione ODBC. Questa funzione è implementata in Gestione driver; può anche essere implementato nei driver. Se un driver implementa **SQLGetFunctions**, gestione driver chiama la funzione nel driver. In caso contrario, viene eseguita la funzione stessa.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetFunctions(  
     SQLHDBC           ConnectionHandle,  
     SQLUSMALLINT      FunctionId,  
     SQLUSMALLINT *    SupportedPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *FunctionId*  
 Input Valore **#define** che identifica la funzione ODBC di interesse. **SQL_API_ODBC3_ALL_FUNCTIONS orSQL_API_ALL_FUNCTIONS**. **SQL_API_ODBC3_ALL_FUNCTIONS** viene utilizzata da un'applicazione ODBC 3 *. x* per determinare il supporto di ODBC 3 *. x* e delle funzioni precedenti. **SQL_API_ALL_FUNCTIONS** viene utilizzata da un'applicazione ODBC 2 *. x* per determinare il supporto di ODBC 2 *. x* e delle funzioni precedenti.  
  
 Per un elenco di valori di **#define** che identificano le funzioni ODBC, vedere le tabelle in "Commenti".  
  
 *SupportedPtr*  
 Output  Se *FunctionId* identifica una singola funzione ODBC, *SupportedPtr* punta a un singolo valore SQLUSMALLINT che viene SQL_TRUE se la funzione specificata è supportata dal driver e SQL_FALSE se non è supportata.  
  
 Se *FunctionId* è SQL_API_ODBC3_ALL_FUNCTIONS, *SupportedPtr* punta a una matrice SQLSMALLINT con un numero di elementi uguale a SQL_API_ODBC3_ALL_FUNCTIONS_SIZE. Questa matrice viene gestita da Gestione driver come bitmap a 4.000 bit che può essere utilizzata per determinare se una funzione ODBC 3 *. x* o precedente è supportata. La macro SQL_FUNC_EXISTS viene chiamata per determinare il supporto della funzione. (Vedere "Commenti"). Un'applicazione ODBC 3 *. x* può chiamare **SQLGetFunctions** con SQL_API_ODBC3_ALL_FUNCTIONS a fronte di un driver ODBC 3 *. x* o ODBC 2 *. x* .  
  
 Se *FunctionId* è SQL_API_ALL_FUNCTIONS, *SupportedPtr* punta a una matrice SQLUSMALLINT di 100 elementi. La matrice viene indicizzata in base ai valori **#define** utilizzati da *FunctionId* per identificare ogni funzione ODBC. alcuni elementi della matrice sono inutilizzati e riservati per un uso futuro. Un elemento viene SQL_TRUE se identifica una funzione ODBC 2 *. x* o precedente supportata dal driver. È SQL_FALSE se identifica una funzione ODBC non supportata dal driver o non identifica una funzione ODBC.  
  
 Le matrici restituite in **SupportedPtr* utilizzano l'indicizzazione in base zero.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetFunctions** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DBC e un *handle* di *connectionHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLGetFunctions** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------|-----|-----------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) **SQLGetFunctions** è stato chiamato prima di **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect**.<br /><br /> (DM) **SQLBrowseConnect** è stato chiamato per *ConnectionHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima che **SQLBrowseConnect** restituisse SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *connectionHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY095|Tipo di funzione non compreso nell'intervallo|(DM) è stato specificato un valore *FunctionId* non valido.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
  
## <a name="comments"></a>Commenti  
 **SQLGetFunctions** restituisce sempre che sono supportati **SQLGetFunctions**, **SQLDataSources** e **SQLDrivers** . Questa operazione viene eseguita perché queste funzioni sono implementate in Gestione driver. Gestione driver eseguirà il mapping di una funzione ANSI alla funzione Unicode corrispondente se la funzione Unicode esiste e eseguirà il mapping di una funzione Unicode alla funzione ANSI corrispondente se esiste la funzione ANSI. Per informazioni sul modo in cui le applicazioni utilizzano **SQLGetFunctions**, vedere [livelli di conformità dell'interfaccia](../../../odbc/reference/develop-app/interface-conformance-levels.md).  
  
 Di seguito è riportato un elenco di valori validi per *FunctionId* per le funzioni conformi al livello di conformità standard ISO 92:  
  
|Valore FunctionId|Valore FunctionId|  
|----------|----------|  
|SQL_API_SQLALLOCHANDLE|SQL_API_SQLGETDESCFIELD|  
|SQL_API_SQLBINDCOL|SQL_API_SQLGETDESCREC|  
|SQL_API_SQLCANCEL|SQL_API_SQLGETDIAGFIELD|  
|SQL_API_SQLCLOSECURSOR|SQL_API_SQLGETDIAGREC|  
|SQL_API_SQLCOLATTRIBUTE|SQL_API_SQLGETENVATTR|  
|SQL_API_SQLCONNECT|SQL_API_SQLGETFUNCTIONS|  
|SQL_API_SQLCOPYDESC|SQL_API_SQLGETINFO|  
|SQL_API_SQLDATASOURCES|SQL_API_SQLGETSTMTATTR|  
|SQL_API_SQLDESCRIBECOL|SQL_API_SQLGETTYPEINFO|  
|SQL_API_SQLDISCONNECT|SQL_API_SQLNUMRESULTCOLS|  
|SQL_API_SQLDRIVERS|SQL_API_SQLPARAMDATA|  
|SQL_API_SQLENDTRAN|SQL_API_SQLPREPARE|  
|SQL_API_SQLEXECDIRECT|SQL_API_SQLPUTDATA|  
|SQL_API_SQLEXECUTE|SQL_API_SQLROWCOUNT|  
|SQL_API_SQLFETCH|SQL_API_SQLSETCONNECTATTR|  
|SQL_API_SQLFETCHSCROLL|SQL_API_SQLSETCURSORNAME|  
|SQL_API_SQLFREEHANDLE|SQL_API_SQLSETDESCFIELD|  
|SQL_API_SQLFREESTMT|SQL_API_SQLSETDESCREC|  
|SQL_API_SQLGETCONNECTATTR|SQL_API_SQLSETENVATTR|  
|SQL_API_SQLGETCURSORNAME|SQL_API_SQLSETSTMTATTR|  
|SQL_API_SQLGETDATA| |  
  
 Di seguito è riportato un elenco di valori validi per *FunctionId* per le funzioni conformi al livello di conformità standard del gruppo aperto:  
  
|Valore FunctionId|Valore FunctionId|  
|-|-|  
|SQL_API_SQLCOLUMNS|SQL_API_SQLSTATISTICS|  
|SQL_API_SQLSPECIALCOLUMNS|SQL_API_SQLTABLES|  
  
 Di seguito è riportato un elenco di valori validi per *FunctionId* per le funzioni conformi al livello di conformità standard ODBC.  
  
|Valore FunctionId|Valore FunctionId|  
|-|-|  
|SQL_API_SQLBINDPARAMETER|SQL_API_SQLNATIVESQL|  
|SQL_API_SQLBROWSECONNECT|SQL_API_SQLNUMPARAMS|  
|SQL_API_SQLBULKOPERATIONS [1]|SQL_API_SQLPRIMARYKEYS|  
|SQL_API_SQLCOLUMNPRIVILEGES|SQL_API_SQLPROCEDURECOLUMNS|  
|SQL_API_SQLDESCRIBEPARAM|SQL_API_SQLPROCEDURES|  
|SQL_API_SQLDRIVERCONNECT|SQL_API_SQLSETPOS|  
|SQL_API_SQLFOREIGNKEYS|SQL_API_SQLTABLEPRIVILEGES|  
|SQL_API_SQLMORERESULTS| |  
  
 [1] quando si utilizza un driver ODBC 2 *. x* , **SQLBulkOperations** verrà restituito come supportato solo se vengono soddisfatte entrambe le condizioni seguenti: il driver ODBC 2 *. x* supporta **SQLSetPos** e il tipo di informazioni SQL_POS_OPERATIONS restituisce il bit SQL_POS_ADD come impostato.  
  
 Di seguito è riportato un elenco di valori validi per *FunctionId* per le funzioni introdotte in ODBC 3,8 o versioni successive:  
  
|Valore FunctionId|  
|-|  
|SQL_API_SQLCANCELHANDLE [2]|  
  
 [2]   **SQLCancelHandle** verrà restituito come supportato solo se il driver supporta sia **SQLCancel** che **SQLCancelHandle**. Se **SQLCancel** è supportato, ma **SQLCancelHandle** non lo è, l'applicazione può comunque chiamare **SQLCancelHandle** su un handle di istruzione, perché verrà mappato a **SQLCancel**.  
  
## <a name="sql_func_exists-macro"></a>SQL_FUNC_EXISTS macro  
 La macro SQL_FUNC_EXISTS (*SupportedPtr*, *FunctionID*) viene utilizzata per determinare il supporto di ODBC 3 *. x* o funzioni precedenti dopo che **SQLGetFunctions** è stato chiamato con un argomento *FunctionID* di SQL_API_ODBC3_ALL_FUNCTIONS. L'applicazione chiama SQL_FUNC_EXISTS con l'argomento *SupportedPtr* impostato su *SupportedPtr* passato in *SQLGetFunctions* e con l'argomento *FunctionID* impostato sul **#define** per la funzione. SQL_FUNC_EXISTS Restituisce SQL_TRUE se la funzione è supportata e SQL_FALSE in caso contrario.  
  
> [!NOTE]
>  Quando si utilizza un driver ODBC *2. x* , gestione driver ODBC 3 *. x* Restituisce SQL_TRUE per **SQLAllocHandle** e **SQLFreeHandle** perché **SQLAllocHandle** è mappato a **SQLAllocEnv**, **SQLAllocConnect** o **SQLAllocStmt** e perché **SQLFreeHandle** è mappato a **SQLFreeEnv**, **SQLFreeConnect** o **SQLFreeStmt**. Tuttavia, **SQLAllocHandle** o **SQLFreeHandle** con un argomento *HandleType* di SQL_HANDLE_DESC non è supportato, sebbene venga restituito SQL_TRUE per le funzioni, perché in questo caso non esiste alcuna funzione ODBC 2 *. x* a cui eseguire il mapping.  
  
## <a name="code-example"></a>Esempio di codice  
 Nei tre esempi seguenti viene illustrato il modo in cui un'applicazione utilizza **SQLGetFunctions** per determinare se un driver supporta **SQLTables**, **SQLColumns** e **SQLStatistics**. Se il driver non supporta queste funzioni, l'applicazione si disconnette dal driver. Nel primo esempio viene chiamato **SQLGetFunctions** una volta per ogni funzione.  
  
```cpp  
SQLUSMALLINT TablesExists, ColumnsExists, StatisticsExists;  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
  
retcodeTables = SQLGetFunctions(hdbc, SQL_API_SQLTABLES, &TablesExists);  
retcodeColumns = SQLGetFunctions(hdbc, SQL_API_SQLCOLUMNS, &ColumnsExists);  
retcodeStatistics = SQLGetFunctions(hdbc, SQL_API_SQLSTATISTICS, &StatisticsExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (retcodeTables == SQL_SUCCESS && TablesExists == SQL_TRUE &&   
retcodeColumns == SQL_SUCCESS && ColumnsExists == SQL_TRUE &&   
retcodeStatistics == SQL_SUCCESS && StatisticsExists == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 Nel secondo esempio, un'applicazione ODBC 3. x chiama **SQLGetFunctions** e la passa a una matrice in cui **SQLGetFunctions** restituisce informazioni su tutte le funzioni ODBC 3. x e precedenti.  
  
```cpp  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[SQL_API_ODBC3_ALL_FUNCTIONS_SIZE];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ODBC3_ALL_FUNCTIONS, fExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (reccode == SQL_SUCCESS &&   
SQL_FUNC_EXISTS(fExists, SQL_API_SQLTABLES) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLCOLUMNS) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLSTATISTICS) == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 Il terzo esempio è un'applicazione ODBC 2. x che chiama **SQLGetFunctions** e la passa a una matrice di 100 elementi in cui **SQLGetFunctions** restituisce informazioni su tutte le funzioni ODBC 2. x e precedenti.  
  
```cpp  
#define FUNCTIONS 100  
  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[FUNCTIONS];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ALL_FUNCTIONS, fExists);  
  
/* SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver. */  
if (retcode == SQL_SUCCESS &&   
fExists[SQL_API_SQLTABLES] == SQL_TRUE &&  
   fExists[SQL_API_SQLCOLUMNS] == SQL_TRUE &&  
   fExists[SQL_API_SQLSTATISTICS] == SQL_TRUE)   
{  
  
   /* Continue with application */  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione dell'impostazione di un attributo di connessione|[Funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Restituzione di informazioni su un driver o un'origine dati|[Funzione SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Restituzione dell'impostazione di un attributo di istruzione|[Funzione SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
