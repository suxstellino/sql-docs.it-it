---
description: SQLFreeStmt Function
title: Funzione SQLFreeStmt | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeStmt
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeStmt
helpviewer_keywords:
- SQLFreeStmt function [ODBC]
ms.assetid: 03408162-8b63-4470-90c4-e6c7d8d33892
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0dcab0413f353b4c600964dd38818df37f86bfe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201561"
---
# <a name="sqlfreestmt-function"></a>SQLFreeStmt Function
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLFreeStmt** interrompe l'elaborazione associata a un'istruzione specifica, chiude tutti i cursori aperti associati all'istruzione, rimuove i risultati in sospeso o, facoltativamente, libera tutte le risorse associate all'handle di istruzione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLFreeStmt(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Option);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione  
  
 *Opzione*  
 Input Una delle opzioni seguenti:  
  
 SQL_ CLOSE: chiude il cursore associato a *statementHandle* (se ne è stato definito uno) ed Elimina tutti i risultati in sospeso. L'applicazione può riaprire il cursore in un secondo momento eseguendo un'istruzione **Select** con lo stesso valore o con valori di parametro diversi. Se il cursore non è aperto, questa opzione non ha alcun effetto sull'applicazione. **SQLCloseCursor** può essere chiamato anche per chiudere un cursore. Per ulteriori informazioni, vedere [chiusura del cursore](../../../odbc/reference/develop-app/closing-the-cursor.md).  
  
 SQL_DROP: questa opzione è deprecata. Una chiamata a **SQLFreeStmt** con un' *opzione* di SQL_DROP viene mappata in Gestione driver a [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
 SQL_UNBIND: imposta il campo SQL_DESC_COUNT di ARD su 0, rilasciando tutti i buffer di colonna associati da **SQLBindCol** per il *statementHandle* specificato. Questa operazione non associa la colonna del segnalibro; a tale scopo, il campo SQL_DESC_DATA_PTR della colonna ARD per il segnalibro è impostato su NULL. Si noti che se questa operazione viene eseguita su un descrittore allocato in modo esplicito condiviso da più di un'istruzione, l'operazione influirà sui binding di tutte le istruzioni che condividono il descrittore. Per ulteriori informazioni, vedere [Panoramica del recupero di risultati (di base)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 SQL_RESET_PARAMS: imposta il campo SQL_DESC_COUNT di APD su 0, rilasciando tutti i buffer dei parametri impostati da **SQLBindParameter** per il *statementHandle* specificato. Se questa operazione viene eseguita su un descrittore allocato in modo esplicito condiviso da più di un'istruzione, questa operazione influirà sui binding di tutte le istruzioni che condividono il descrittore. Per ulteriori informazioni, vedere [Binding Parameters](../../../odbc/reference/develop-app/binding-parameters-odbc.md).  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLFreeStmt** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLFreeStmt** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stato chiamato **SQLFreeStmt** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata con l' *opzione* impostata su SQL_RESET_PARAMS prima che siano stati recuperati i dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY092|Tipo di opzione non compreso nell'intervallo|(DM) il valore specificato per l' *opzione* argument non è:<br /><br /> SQL_CLOSE SQL_DROP SQL_UNBIND SQL_RESET_PARAMS|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 La chiamata di **SQLFreeStmt** con l'opzione SQL_CLOSE equivale alla chiamata a **SQLCloseCursor**, ad eccezione del fatto che **SQLFreeStmt** con SQL_CLOSE non influisce sull'applicazione se nessun cursore è aperto nell'istruzione. Se non è aperto alcun cursore, una chiamata a **SQLCloseCursor** restituisce SQLSTATE 24000 (stato del cursore non valido).  
  
 Un'applicazione non deve usare un handle di istruzione dopo che è stata liberata. Gestione driver non controlla la validità di un handle in una chiamata di funzione.  
  
## <a name="example"></a>Esempio  
 Si tratta di una procedura di programmazione efficace per gli handle gratuiti. Tuttavia, per semplicità, l'esempio seguente non include il codice che libera gli handle allocati. Per un esempio di come liberare handle, vedere [funzione SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
```cpp  
// SQLFreeStmt.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
   retCode = SQLFreeStmt(hstmt, SQL_UNBIND);  
   retCode = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Chiusura di un cursore|[Funzione SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)|  
|Liberare un handle|[SQLFreeHandle Function](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Impostazione di un nome di cursore|[Funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
