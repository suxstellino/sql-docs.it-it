---
description: Funzione SQLDisconnect
title: Funzione Disconnect | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDisconnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDisconnect
helpviewer_keywords:
- SQLDisconnect function [ODBC]
ms.assetid: 9e84a58e-db48-4821-a0cd-5c711fcbe36b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a5f246c3d5aafbc02a138e809a0a07805f8ae30f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165320"
---
# <a name="sqldisconnect-function"></a>Funzione SQLDisconnect
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **Disconnect** chiude la connessione associata a un handle di connessione specifico.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLDisconnect(  
     SQLHDBC     ConnectionHandle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **Disconnect** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_DBC e un *handle* di *connectionHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti comunemente da **Disconnect** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01002|Errore di disconnessione|Si è verificato un errore durante la disconnessione. Tuttavia, la disconnessione è riuscita. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08003|Connessione non aperta|(DM) la connessione specificata nell'argomento *connectionHandle* non è stata aperta.|  
|25000|Stato della transazione non valido|Transazione in corso sulla connessione specificata dall'argomento *connectionHandle*. La transazione rimane attiva.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *connectionHandle*. La funzione è stata chiamata e prima di finshed l'esecuzione della [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) è stata chiamata su *connectionHandle*. La funzione è stata chiamata nuovamente in *connectionHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione di **SQLCancelHandle** è stato chiamato su *connectionHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per un *statementHandle* associato a *connectionHandle* ed è ancora in esecuzione quando è stato chiamato **SqlConnection** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per un *statementHandle* associato a *connectionHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta e la connessione è ancora attiva. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *connectionHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 Se un'applicazione chiama **disconnection** dopo che **SQLBrowseConnect** restituisce SQL_NEED_DATA e prima che restituisca un altro codice restituito, il driver Annulla il processo di esplorazione della connessione e restituisce la connessione a uno stato non connesso.  
  
 Se un'applicazione chiama **Disconnect** mentre esiste una transazione incompleta associata all'handle di connessione, il driver restituisce SQLSTATE 25000 (stato della transazione non valido), a indicare che la transazione rimane invariata e la connessione è aperta. Una transazione incompleta non è stata sottoposta a commit o ne è stato eseguito il rollback con **SQLEndTran**.  
  
 Se un'applicazione chiama **SqlConnection** prima di avere liberato tutte le istruzioni associate alla connessione, il driver, dopo la disconnessione dall'origine dati, libera tali istruzioni e tutti i descrittori che sono stati allocati in modo esplicito nella connessione. Tuttavia, se una o più istruzioni associate alla connessione sono ancora in esecuzione in modo asincrono, **Disconnect** restituisce SQL_ERROR con un valore SQLSTATE di HY010 (errore della sequenza di funzioni). **Disconnect** , inoltre, libererà tutte le istruzioni associate e tutti i descrittori che sono stati allocati in modo esplicito sulla connessione, se la connessione è in uno stato sospeso oppure se  **Disconnect** è stato annullato correttamente da SQLCancelHandle.  
  
 Per informazioni sul modo in cui un'applicazione usa **Disconnect**, vedere [disconnessione da un'origine dati o da un driver](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md).  
  
## <a name="disconnecting-from-a-pooled-connection"></a>Disconnessione da una connessione in pool  
 Se il pool di connessioni è abilitato per un ambiente condiviso e un'applicazione chiama **SQLConnect** in una connessione in tale ambiente, la connessione viene restituita al pool di connessioni ed è ancora disponibile per altri componenti che utilizzano lo stesso ambiente condiviso.  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere il [programma ODBC di esempio](../../../odbc/reference/sample-odbc-program.md), la [funzione SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)e la [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Connessione a un'origine dati|[Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Connessione a un'origine dati utilizzando una stringa di connessione o una finestra di dialogo|[Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Esecuzione di un'operazione di commit o rollback|[SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Liberazione di un handle di connessione|[Funzione SQLFreeConnect](../../../odbc/reference/syntax/sqlfreeconnect-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
