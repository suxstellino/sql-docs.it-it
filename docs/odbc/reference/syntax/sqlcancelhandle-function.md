---
description: Funzione SQLCancelHandle
title: Funzione SQLCancelHandle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCancelHandle
helpviewer_keywords:
- SQLCancelHandle function [ODBC]
ms.assetid: 16049b5b-22a7-4640-9897-c25dd0f19d21
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 727f9c15809969560410cfa54bdf9b70ec765f53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181039"
---
# <a name="sqlcancelhandle-function"></a>Funzione SQLCancelHandle
**Conformità**  
 Versione introdotta: ODBC 3,8 Standard Compliance: None  
  
 Si prevede che la maggior parte dei driver ODBC 3,8 (e versioni successive) implementerà questa funzione. Se non è presente un driver, una chiamata a **SQLCancelHandle** con un handle di connessione nel parametro *handle* restituirà SQL_ERROR con un valore SQLSTATE di IM001 e il driver message ' non supporta questa funzione '' una chiamata a **SQLCancelHandle** con un handle di istruzione perché il parametro *handle* verrà mappato a una chiamata a **SQLCancel** da Gestione driver e può essere elaborato se il driver implementa **SQLCancel**. Un'applicazione può usare **SQLGetFunctions** per determinare se un driver supporta **SQLCancelHandle**.  
  
 **Summary**  
 **SQLCancelHandle** Annulla l'elaborazione in una connessione o in un'istruzione. Gestione driver esegue il mapping di una chiamata a **SQLCancelHandle** a una chiamata a **SQLCancel** quando *HandleType* è SQL_HANDLE_STMT.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLCancelHandle(  
      SQLSMALLINT  HandleType,  
      SQLHANDLE    Handle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HandleType*  
 Input Tipo dell'handle su cui cacel l'elaborazione. I valori validi sono SQL_HANDLE_DBC o SQL_HANDLE_STMT.  
  
 *Handle*  
 Input Handle su cui annullare l'elaborazione.  
  
 Se *handle* non è un handle valido del tipo specificato da *HandleType*, **SQLCancelHandle** restituisce SQL_INVALID_HANDLE.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLCancelHandle** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con una *HandleType* di SQL_HANDLE_STMT e un handle di handle di *istruzione o un* *HandleType* di SQL_HANDLE_DBC e un handle *di handle di connessione.*  
  
 La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLCancelHandle** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) nell'argomento buffer *\* MessageText* descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|È stata chiamata una funzione in esecuzione asincrona, correlata all'istruzione per uno degli handle di istruzione associati all' *handle* e *HandleType* è stato impostato su SQL_HANDLE_DBC. La funzione asincrona era ancora in esecuzione quando è stato chiamato **SQLCancelHandle** .<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_STMT; una funzione in esecuzione asincrona è stata chiamata sull'handle di connessione associato; e la funzione era ancora in esecuzione quando questa funzione è stata chiamata.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati all' *handle* e *HandleType* è stato impostato su SQL_HANDLE_DBC e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> **SQLBrowseConnect** è stato chiamato per *connectionHandle* e ha restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima del completamento del processo di esplorazione.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY092|Identificatore di attributo/opzione non valido|*HandleType* è stato impostato su SQL_HANDLE_ENV o SQL_HANDLE_DESC.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato all' *handle* non supporta la funzione.|  
  
 Se viene chiamato **SQLCancelHandle** con *HandleType* impostato su SQL_HANDLE_STMT, può restituire qualsiasi SQLSTATE che può essere restituito dalla funzione **SQLCancel**.  
  
## <a name="comments"></a>Commenti  
 Questa funzione è simile a **SQLCancel** , ma può richiedere un handle di connessione o di istruzione come parametro anziché solo un handle di istruzione. Gestione driver esegue il mapping di una chiamata a **SQLCancelHandle** a una chiamata a **SQLCancel** quando *HandleType* è SQL_HANDLE_STMT. In questo modo le applicazioni possono utilizzare **SQLCancelHandle** per annullare le operazioni di istruzione, anche se il driver non implementa **SQLCancelHandle**.  
  
 Per ulteriori informazioni sull'annullamento di un'operazione di istruzione, vedere [funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md).  
  
 Se non sono in corso operazioni per *gestire* la chiamata a **SQLCancelHandle** non ha alcun effetto.  
  
 **SQLCancelHandle** su un handle di connessione può annullare i tipi di elaborazione seguenti:  
  
-   Funzione in esecuzione in modo asincrono sulla connessione.  
  
-   Funzione in esecuzione nell'handle di connessione su un altro thread.  
  
 Quando **SQLCancelHandle** viene chiamato per annullare una funzione in esecuzione in modo asincrono in una connessione, i record di diagnostica pubblicati da **SQLCancelHandle** vengono aggiunti a quelli restituiti dall'operazione annullata; Tuttavia, **SQLCancelHandle** non restituisce i record di diagnostica quando si annulla una funzione in esecuzione in una connessione in un altro thread.  
  
 L'utilizzo di **SQLCancelHandle** per annullare **SQLEndTran** può impostare la connessione nello stato Suspended. Per altre informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).  
  
> [!NOTE]  
>  Per informazioni sull'utilizzo di **SQLCancelHandle** in un'applicazione che verrà distribuita in un sistema operativo Windows precedente a Windows 7, vedere [Compatibility Matrix](../../../odbc/reference/develop-app/compatibility-matrix.md).  
  
#### <a name="canceling-connection-related-asynchronous-processing"></a>Annullamento Connection-Related elaborazione asincrona  
 Se una funzione restituisce SQL_STILL_EXECUTING, un'applicazione può chiamare **SQLCancelHandle** per annullare l'operazione. Se la richiesta di annullamento ha esito positivo, **SQLCancelHandle** restituisce SQL_SUCCESS. Ciò non significa che la funzione originale è stata annullata; indica che la richiesta di annullamento è stata elaborata. Il driver e l'origine dati determinano quando o se l'operazione viene annullata. L'applicazione deve continuare a chiamare la funzione originale fino a quando non viene SQL_STILL_EXECUTING il codice restituito. Se la funzione originale è stata annullata, il codice restituito è SQL_ERROR e SQLSTATE HY008 (operazione annullata). Se la funzione originale ha completato l'elaborazione normale (non è stata annullata), il codice restituito è SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, oppure SQL_ERROR e un valore SQLSTATE diverso da HY008 (operazione annullata), se la funzione originale non è riuscita.  
  
#### <a name="canceling-functions-executing-on-another-thread"></a>Annullamento di funzioni in esecuzione su un altro thread  
 In un'applicazione multithread, l'applicazione può annullare un'operazione in esecuzione su un altro thread. Per annullare l'operazione, l'applicazione chiama **SQLCancelHandle** con l'handle usato dalla funzione, ma su un thread diverso. Il driver e il sistema operativo determinano il modo in cui viene annullata l'operazione. Il codice restituito **SQLCancelHandle** indica se il driver ha elaborato la richiesta, restituendo SQL_SUCCESS o SQL_ERROR (non vengono restituite informazioni di diagnostica). Se l'elaborazione sulla funzione originale viene annullata, la funzione originale restituisce SQL_ERROR e SQLSTATE HY008 (operazione annullata).  
  
 Se una funzione viene eseguita quando **SQLCancelHandle** viene chiamato su un altro thread per annullare la funzione, è possibile che la funzione abbia esito positivo e restituisca SQL_SUCCESS prima che l'annullamento possa essere applicato. Una chiamata a **SQLCancelHandle** non ha alcun effetto se l'operazione è stata completata prima che **SQLCancelHandle** fosse in grado di annullare l'operazione.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Annullamento di una funzione in esecuzione in modo asincrono in un handle di istruzione, annullamento di una funzione in un'istruzione che necessita di dati o annullamento di una funzione in esecuzione in un'istruzione in un altro thread.|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Esecuzione asincrona (metodo di polling)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
