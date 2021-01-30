---
description: Funzione SQLCancel
title: Funzione SQLCancel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCancel
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCancel
helpviewer_keywords:
- SQLCancel function [ODBC]
ms.assetid: ac0b5972-627f-4440-8c5a-0e8da728726d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c1d630af340bf676deda8c18f415b85aba15403c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185801"
---
# <a name="sqlcancel-function"></a>Funzione SQLCancel
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLCancel** Annulla l'elaborazione di un'istruzione.  
  
 Per annullare l'elaborazione su una connessione o un'istruzione, usare la [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLCancel(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLCancel** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLCancel** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) nell'argomento buffer *\* MessageText* descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLCancel** .<br /><br /> L'operazione di annullamento (DM) non è riuscita perché è in corso un'operazione asincrona su un handle di connessione associato a *statementHandle*.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY018|Il server ha rifiutato la richiesta di annullamento|Il server ha rifiutato la richiesta di annullamento.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 **SQLCancel** può annullare i tipi di elaborazione seguenti in un'istruzione:  
  
-   Funzione in esecuzione in modo asincrono sull'istruzione.  
  
-   Funzione su un'istruzione che necessita di dati.  
  
-   Funzione in esecuzione nell'istruzione su un altro thread.  
  
 In ODBC 2. *x*, se un'applicazione chiama **SQLCancel** quando non viene eseguita alcuna elaborazione sull'istruzione, **SQLCancel** ha lo stesso effetto di **SQLFreeStmt** con l'opzione SQL_CLOSE; Questo comportamento viene definito solo per completezza e le applicazioni devono chiamare **SQLFreeStmt** o **SQLCloseCursor** per chiudere i cursori.  
  
 Quando viene chiamato **SQLCancel** per annullare una funzione in esecuzione in modo asincrono in un'istruzione o in una funzione in un'istruzione che necessita di dati, i record di diagnostica pubblicati dalla funzione annullata vengono cancellati e **SQLCancel** invia i propri record di diagnostica; Quando viene chiamato **SQLCancel** per annullare una funzione in esecuzione in un'istruzione in un altro thread, tuttavia, non cancella i record di diagnostica della funzione annullata e non pubblica i propri record di diagnostica.  
  
## <a name="canceling-asynchronous-processing"></a>Annullamento dell'elaborazione asincrona  
 Quando un'applicazione chiama una funzione in modo asincrono, chiama ripetutamente la funzione per determinare se l'elaborazione è terminata. Se la funzione viene ancora elaborata, restituisce SQL_STILL_EXECUTING. Se la funzione ha terminato l'elaborazione, restituisce un codice diverso.  
  
 Dopo qualsiasi chiamata alla funzione che restituisce SQL_STILL_EXECUTING, un'applicazione può chiamare **SQLCancel** per annullare la funzione. Se la richiesta di annullamento ha esito positivo, il driver restituisce SQL_SUCCESS. Questo messaggio non indica che la funzione è stata effettivamente annullata; indica che la richiesta di annullamento è stata elaborata. Quando o se la funzione è effettivamente annullata è dipendente dal driver e dipendente dall'origine dati. L'applicazione deve continuare a chiamare la funzione originale fino a quando non viene SQL_STILL_EXECUTING il codice restituito. Se la funzione è stata annullata correttamente, il codice restituito è SQL_ERROR e SQLSTATE HY008 (operazione annullata). Se la funzione ha completato la normale elaborazione, il codice restituito viene SQL_SUCCESS o SQL_SUCCESS_WITH_INFO se la funzione ha avuto esito positivo o SQL_ERROR e un valore SQLSTATE diverso da HY008 (operazione annullata) se la funzione ha avuto esito negativo.  
  
> [!NOTE]  
>  In ODBC 3,5, una chiamata a **SQLCancel** quando nessuna elaborazione viene eseguita nell'istruzione non viene trattata come **SQLFreeStmt** con l'opzione SQL_CLOSE, ma non ha alcun effetto. Per chiudere un cursore, un'applicazione deve chiamare **SQLCloseCursor**, non **SQLCancel**.  
  
 Per ulteriori informazioni sull'elaborazione asincrona, vedere [esecuzione asincrona](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).  
  
## <a name="canceling-functions-that-need-data"></a>Annullamento di funzioni che necessitano di dati  
 Quando **SQLExecute** o **SQLExecDirect** restituisce SQL_NEED_DATA e prima dell'invio dei dati per tutti i parametri data-at-execution, un'applicazione può chiamare **SQLCancel** per annullare l'esecuzione dell'istruzione. Dopo che l'istruzione è stata annullata, l'applicazione può chiamare nuovamente **SQLExecute** o **SQLExecDirect** . Per ulteriori informazioni, vedere [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 Dopo che **SQLBulkOperations** o **SQLSetPos** restituisce SQL_NEED_DATA e prima dell'invio dei dati per tutte le colonne data-at-execution, un'applicazione può chiamare **SQLCancel** per annullare l'operazione. Dopo che l'operazione è stata annullata, l'applicazione può chiamare di nuovo **SQLBulkOperations** o **SQLSetPos** . l'annullamento non influisce sullo stato del cursore o sulla posizione corrente del cursore. Per ulteriori informazioni, vedere [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) o [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="canceling-functions-executing-on-another-thread"></a>Annullamento di funzioni in esecuzione su un altro thread  
 In un'applicazione multithread, l'applicazione può annullare una funzione in esecuzione in un altro thread. Per annullare la funzione, l'applicazione chiama **SQLCancel** con lo stesso handle di istruzione usato dalla funzione di destinazione, ma in un thread diverso. Il modo in cui la funzione viene annullata dipende dal driver e dal sistema operativo. Come per l'annullamento di una funzione in esecuzione in modo asincrono, il codice restituito di **SQLCancel** indica solo se il driver ha elaborato la richiesta correttamente. È possibile restituire solo SQL_SUCCESS o SQL_ERROR. non vengono restituite informazioni di diagnostica. Se la funzione originale viene annullata, restituisce SQL_ERROR e SQLSTATE HY008 (operazione annullata).  
  
 Se un'istruzione SQL viene eseguita quando **SQLCancel** viene chiamato su un altro thread per annullare l'esecuzione dell'istruzione, è possibile che l'esecuzione riesca e restituisca SQL_SUCCESS mentre l'annullamento ha esito positivo. In questo caso, gestione driver presuppone che il cursore aperto dall'esecuzione dell'istruzione venga chiuso da Annulla, quindi l'applicazione non sarà in grado di utilizzare il cursore.  
  
 Per ulteriori informazioni sul threading, vedere [multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Esecuzione di operazioni di inserimento o aggiornamento bulk|[Funzione SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annulla una funzione in esecuzione in modo asincrono in un handle di connessione, oltre alla funzionalità di **SQLCancel**.|[Funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Liberazione di un handle di istruzione|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Recupero di un campo di un record di diagnostica o di un campo dell'intestazione di diagnostica|[Funzione SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
|Recupero di più campi di una struttura di dati di diagnostica|[Funzione SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|  
|Restituzione del parametro successivo per l'invio dei dati|[Funzione SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Invio di dati dei parametri in fase di esecuzione|[SQLPutData Function](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Posizionare il cursore in un set di righe, aggiornare i dati nel set di righe o aggiornare o eliminare dati nel set di risultati|[Funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
