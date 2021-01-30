---
description: SQLFreeHandle Function
title: Funzione SQLFreeHandle | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeHandle
helpviewer_keywords:
- SQLFreeHandle function [ODBC]
ms.assetid: 17a6fcdc-b05a-4de7-be93-a316f39696a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 14c649c894250abdcac54010c0d18444d20b3baa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201570"
---
# <a name="sqlfreehandle-function"></a>SQLFreeHandle Function
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLFreeHandle** libera le risorse associate a un ambiente, una connessione, un'istruzione o un handle descrittore specifico.  
  
> [!NOTE]
>  Questa funzione è una funzione generica per liberare handle. Sostituisce le funzioni ODBC 2,0 **SQLFreeConnect** (per liberare un handle di connessione) e **SQLFreeEnv** (per liberare un handle di ambiente). **SQLFreeConnect** e **SQLFreeEnv** sono entrambi deprecati in ODBC 3 *. x*. **SQLFreeHandle** sostituisce anche la funzione ODBC 2,0 **SQLFreeStmt** (con l' *opzione* SQL_DROP) per liberare un handle di istruzione. Per ulteriori informazioni, vedere "Commenti". Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando un'applicazione ODBC 3 *. x* utilizza un driver ODBC 2 *. x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLFreeHandle(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HandleType*  
 Input Tipo di handle che deve essere liberato da **SQLFreeHandle**. Deve essere uno dei valori seguenti:   
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN handle viene utilizzato solo dal driver e dalla gestione driver. Le applicazioni non devono usare questo tipo di handle. Per ulteriori informazioni su SQL_HANDLE_DBC_INFO_TOKEN, vedere [sviluppo di Connection-Pool awareness in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 Se *HandleType* non è uno di questi valori, **SQLFreeHandle** restituisce SQL_INVALID_HANDLE.  
  
 *Handle*  
 Input Handle da liberare.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_ERROR o SQL_INVALID_HANDLE.  
  
 Se **SQLFreeHandle** restituisce SQL_ERROR, l'handle è ancora valido.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLFreeHandle** restituisce SQL_ERROR, è possibile ottenere un valore SQLSTATE associato dalla struttura dei dati di diagnostica per l'handle che **SQLFreeHandle** ha tentato di liberare, ma non è stato possibile. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLFreeHandle** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) l'argomento *HandleType* è stato SQL_HANDLE_ENV e almeno una connessione si trovava in uno stato allocato o connesso. **Disconnect** e **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DBC devono essere chiamati per ogni connessione prima di chiamare **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_ENV.<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_DBC e la funzione è stata chiamata prima di chiamare **Disconnect** per la connessione.<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_DBC. Una funzione in esecuzione asincrona è stata chiamata con *handle* e la funzione era ancora in esecuzione quando questa funzione è stata chiamata.<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_STMT. **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato con l'handle dell'istruzione e ha restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_STMT. Una funzione in esecuzione asincrona è stata chiamata nell'handle di istruzione o nell'handle di connessione associato e la funzione era ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_DESC. Una funzione in esecuzione asincrona è stata chiamata sull'handle di connessione associato; e la funzione era ancora in esecuzione quando questa funzione è stata chiamata.<br /><br /> (DM) tutti gli handle sussidiari e altre risorse non sono stati rilasciati prima della chiamata a **SQLFreeHandle** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati all' *handle* e *HandleType* è stato impostato su SQL_HANDLE_STMT o SQL_HANDLE_DESC restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|L'argomento *HandleType* è stato SQL_HANDLE_STMT o SQL_HANDLE_DESC e non è stato possibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti di memoria sottostanti, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY017|Utilizzo non valido di un handle descrittore allocato automaticamente.|(DM) l'argomento *handle* è stato impostato sull'handle di un descrittore allocato automaticamente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) l'argomento *HandleType* è stato SQL_HANDLE_DESC e il driver era un driver ODBC 2 *. x* .<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_STMT e il driver non è un driver ODBC valido.|  
  
## <a name="comments"></a>Commenti  
 **SQLFreeHandle** viene usato per liberare handle per gli ambienti, le connessioni, le istruzioni e i descrittori, come descritto nelle sezioni riportate di seguito. Per informazioni generali sugli handle, vedere [Handles](../../../odbc/reference/develop-app/handles.md).  
  
 Un'applicazione non deve usare un handle dopo che è stata liberata. Gestione driver non controlla la validità di un handle in una chiamata di funzione.  
  
## <a name="freeing-an-environment-handle"></a>Liberare un handle di ambiente  
 Prima di chiamare **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_ENV, un'applicazione deve chiamare **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DBC per tutte le connessioni allocate nell'ambiente. In caso contrario, la chiamata a **SQLFreeHandle** restituisce SQL_ERROR e l'ambiente e qualsiasi connessione attiva rimangono validi. Per altre informazioni, vedere [handle di ambiente](../../../odbc/reference/develop-app/environment-handles.md) e [allocare l'handle di ambiente](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 Se l'ambiente è un ambiente condiviso, l'applicazione che chiama **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_ENV non dispone più dell'accesso all'ambiente dopo la chiamata, ma le risorse dell'ambiente non vengono necessariamente liberate. La chiamata a **SQLFreeHandle** decrementa il conteggio dei riferimenti dell'ambiente. Il conteggio dei riferimenti viene gestito da Gestione driver. Se non raggiunge zero, l'ambiente condiviso non viene liberato, perché è ancora in uso da parte di un altro componente. Se il conteggio dei riferimenti raggiunge zero, le risorse dell'ambiente condiviso vengono liberate.  
  
## <a name="freeing-a-connection-handle"></a>Liberazione di un handle di connessione  
 Prima di chiamare **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DBC, un'applicazione deve chiamare **Disconnect** per la connessione se è presente una connessione a questo handle *.* In caso contrario, la chiamata a **SQLFreeHandle** restituisce SQL_ERROR e la connessione rimane valida.  
  
 Per ulteriori informazioni, vedere [handle di connessione](../../../odbc/reference/develop-app/connection-handles.md) e [disconnessione da un'origine dati o da un driver](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md).  
  
## <a name="freeing-a-statement-handle"></a>Rilascio di un handle di istruzione  
 Una chiamata a **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_STMT libera tutte le risorse allocate da una chiamata a **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_STMT. Quando un'applicazione chiama **SQLFreeHandle** per liberare un'istruzione con risultati in sospeso, i risultati in sospeso vengono eliminati. Quando un'applicazione libera un handle di istruzione, il driver libera i quattro descrittori allocati automaticamente associati a tale handle. Per ulteriori informazioni, vedere [handle di istruzione](../../../odbc/reference/develop-app/statement-handles.md) e [liberare un handle di istruzione](../../../odbc/reference/develop-app/freeing-a-statement-handle-odbc.md).  
  
 Si noti che **sqldisconnette** rilascia automaticamente le istruzioni e i descrittori aperti sulla connessione.  
  
## <a name="freeing-a-descriptor-handle"></a>Liberare un handle descrittore  
 Una chiamata a **SQLFreeHandle** con *HandleType* SQL_HANDLE_DESC libera l'handle del descrittore nell' *handle*. La chiamata a **SQLFreeHandle** non rilascia alcuna memoria allocata dall'applicazione a cui è possibile fare riferimento da un campo puntatore (inclusi SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR) di qualsiasi record di descrittore di *handle*. La memoria allocata dal driver per i campi che non sono campi puntatore viene liberata quando l'handle viene liberato. Quando viene liberato un handle di descrittore allocato dall'utente, a tutte le istruzioni a cui è stato associato il punto di controllo liberato viene ripristinato il rispettivo handle descrittore automaticamente  
  
> [!NOTE]
>  I driver ODBC 2 *. x* non supportano la liberazione degli handle descrittore, così come non supportano l'allocazione di handle di descrittore.  
  
 Si noti che **sqldisconnette** rilascia automaticamente le istruzioni e i descrittori aperti sulla connessione. Quando un'applicazione libera un handle di istruzione, il driver libera tutti i descrittori generati automaticamente associati a tale handle.  
  
 Per ulteriori informazioni sui descrittori, vedere [descrittori](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Per altri esempi di codice, vedere [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md) e [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
### <a name="code"></a>Codice  
  
```cpp  
// SQLFreeHandle.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include <stdio.h>  
  
int main() {  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
  
   // Initialize the environment, connection, statement handles.  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   // Allocate the environment.  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set environment attributes.  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
  
   // Allocate the connection.  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
   // Set the login timeout.  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
  
   // Let the user select the data source and connect to the database.  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Free handles, and disconnect.     
   if (hstmt) {   
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
      hstmt = NULL;   
   }  
   if (hdbc) {   
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      hdbc = NULL;   
   }  
   if (henv) {   
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
      henv = NULL;   
   }  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCance](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Impostazione di un nome di cursore|[Funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Programma di esempio ODBC](../../../odbc/reference/sample-odbc-program.md)
