---
description: Funzione SQLGetCursorName
title: Funzione SQLGetCursorName | Microsoft Docs
ms.custom: ''
ms.date: 06/12/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetCursorName
helpviewer_keywords:
- SQLGetCursorName function [ODBC]
ms.assetid: e6e92199-7bb6-447c-8987-049a4c6ce05d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f413bb9fb5a11a9e09b932725219e47177aa0c2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199212"
---
# <a name="sqlgetcursorname-function"></a>Funzione SQLGetCursorName
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetCursorName** restituisce il nome del cursore associato a un'istruzione specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetCursorName(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CursorName,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   NameLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *Nomecursore*  
 Output Puntatore a un buffer in cui restituire il nome del cursore.  
  
 Se *CursorName* è null, *NameLengthPtr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta il *cursore*.  
  
 *BufferLength*  
 Input Lunghezza di \* *CursorName*, in caratteri. 
  
 *NameLengthPtr*  
 Output Puntatore alla memoria in cui restituire il numero totale di caratteri, escluso il carattere di terminazione null, disponibile per la restituzione in \* *CursorName*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *bufferLength*, il nome del cursore in \* *CursorName* viene troncato in *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetCursorName** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLGetCursorName** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il buffer \* *CursorName* non è abbastanza grande per restituire l'intero nome del cursore, quindi il nome del cursore è stato troncato. La lunghezza del nome del cursore non troncato viene restituita in **NameLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLGetCursorName** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY015|Nessun nome di cursore disponibile|(DM) il driver era un driver ODBC 2 *. x* , non è presente alcun cursore aperto sull'istruzione e non è stato impostato alcun nome di cursore con **SQLSetCursorName**.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato nell'argomento *bufferLength* è minore di 0.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 I nomi dei cursori vengono utilizzati solo nelle istruzioni Update e Delete posizionate, ad esempio **Update** _Table-Name_ ... **Dove Current di** _Cursor-Name_). Per altre informazioni, vedere [istruzioni Update e Delete posizionate](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Se l'applicazione non chiama **SQLSetCursorName** per definire un nome di cursore, il driver genera un nome. Questo nome inizia con le lettere SQL_CUR.  
  
> [!NOTE]
>  In ODBC 2 *. x*, quando non è presente alcun cursore aperto e non è stato impostato alcun nome da una chiamata a **SQLSetCursorName**, una chiamata a **SQLGetCursorName** ha restituito SQLSTATE HY015 (nessun nome di cursore disponibile). In ODBC 3 *. x* questa operazione non è più vera. indipendentemente dal momento in cui viene chiamato **SQLGetCursorName** , il driver restituisce il nome del cursore.  
  
 **SQLGetCursorName** restituisce il nome di un cursore indipendentemente dal fatto che il nome sia stato creato in modo esplicito o implicito. Se **SQLSetCursorName** non viene chiamato, viene generato in modo implicito un nome di cursore. È possibile chiamare **SQLSetCursorName** per rinominare un cursore su un'istruzione purché il cursore sia in uno stato allocato o preparato.  
  
 Il nome di un cursore impostato in modo esplicito o implicito rimane impostato fino a quando non viene eliminato il *statementHandle* a cui è associato, utilizzando **SQLFreeHandle** con *HandleType* di SQL_HANDLE_STMT.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Preparazione di un'istruzione per l'esecuzione|[Pagina relativa alla funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Impostazione di un nome di cursore|[Funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
