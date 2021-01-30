---
description: SQLEndTran Function
title: Funzione SQLEndTran | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLEndTran
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLEndTran
helpviewer_keywords:
- SQLEndTran function [ODBC]
ms.assetid: ff375ce1-eb50-4693-b1e6-70181a6dbf9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2961cab7c061583f68387e29ed564efc16916870
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195602"
---
# <a name="sqlendtran-function"></a>SQLEndTran Function
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLEndTran** richiede un'operazione di commit o di rollback per tutte le operazioni attive su tutte le istruzioni associate a una connessione. **SQLEndTran** può anche richiedere l'esecuzione di un'operazione di commit o di rollback per tutte le connessioni associate a un ambiente.  
  
> [!NOTE]  
>  Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando ODBC 3. l'applicazione *x* funziona con ODBC 2. driver *x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLEndTran(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle,  
     SQLSMALLINT   CompletionType);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HandleType*  
 Input Identificatore del tipo di handle. Contiene SQL_HANDLE_ENV (se *handle* è un handle di ambiente) o SQL_HANDLE_DBC (se *handle* è un handle di connessione).  
  
 *Handle*  
 Input Handle del tipo indicato da *HandleType*, che indica l'ambito della transazione. Per ulteriori informazioni, vedere "Commenti".  
  
 *CompletionType*  
 Input Uno dei due valori seguenti:  
  
 SQL_COMMIT SQL_ROLLBACK  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLEndTran** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con l' *handle* e il *HandleType* appropriati. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLEndTran** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08003|Connessione non aperta|(DM) *HandleType* è stato SQL_HANDLE_DBC e l' *handle* non si trovava in uno stato connesso.|  
|08007|Errore di connessione durante la transazione|*HandleType* è stato SQL_HANDLE_DBC e la connessione associata all' *handle* non è riuscita durante l'esecuzione della funzione e non può essere determinata se il **commit** o il **rollback** richiesto è stato eseguito prima dell'errore.|  
|25S01|Stato transazione sconosciuto|Una o più connessioni nell' *handle* non sono in grado di completare la transazione con il risultato specificato e il risultato è sconosciuto.|  
|25S02|La transazione è ancora attiva|Il driver non è in grado di garantire che tutto il lavoro nella transazione globale possa essere completato in modo atomico e la transazione è ancora attiva.|  
|25S03|Viene eseguito il rollback della transazione|Il driver non è stato in grado di garantire che tutto il lavoro nella transazione globale possa essere completato in modo atomico e che sia stato eseguito il rollback di tutte le operazioni nella transazione attiva nell' *handle* .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40002|Violazione del vincolo di integrità|Il *CompletionType* è stato SQL_COMMIT e l'impegno delle modifiche ha causato la violazione del vincolo di integrità. Di conseguenza, è stato eseguito il rollback della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* szMessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *connectionHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione della [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) è stata chiamata in *connectionHandle*. La funzione è stata chiamata nuovamente in *connectionHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione di **SQLCancelHandle** è stato chiamato su *connectionHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per un handle di istruzione associato a *connectionHandle* ed è ancora in esecuzione quando è stato chiamato **SQLEndTran** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per un handle di istruzione associato a *connectionHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per l' *handle* con *HandleType* impostato su SQL_HANDLE_DBC ed è ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati all' *handle* e restituiti SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY012|Codice operazione transazione non valido|(DM) il valore specificato per l'argomento *CompletionType* non è né SQL_COMMIT né SQL_ROLLBACK.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY092|Identificatore di attributo/opzione non valido|(DM) il valore specificato per l'argomento *HandleType* non è né SQL_HANDLE_ENV né SQL_HANDLE_DBC.|  
|HY115|SQLEndTran non è consentito per un ambiente che contiene una connessione con esecuzione della funzione asincrona abilitata|(DM) *HandleType* non può essere impostato su SQL_HANDLE_ENV se l'esecuzione asincrona di funzioni di connessione è abilitata per una connessione nell'ambiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere la sezione dei commenti di questo argomento.|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta l'operazione di **rollback** .|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *connectionHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 Per un ODBC 3. driver *x* , se *HandleType* è SQL_HANDLE_ENV e *handle* è un handle di ambiente valido, gestione driver chiamerà **SQLEndTran** in ogni driver associato all'ambiente. L'argomento *handle* per la chiamata a un driver sarà l'handle di ambiente del driver. Per ODBC 2. driver *x* , se *HandleType* è SQL_HANDLE_ENV e *handle* è un handle di ambiente valido e sono presenti più connessioni in uno stato connesso in tale ambiente, gestione driver chiamerà **SQLTransact** nel driver una volta per ogni connessione in uno stato connesso in tale ambiente. L'argomento *handle* in ogni chiamata sarà l'handle della connessione. In entrambi i casi, il driver tenterà di eseguire il commit o il rollback delle transazioni, a seconda del valore di *CompletionType*, in tutte le connessioni in stato connesso in tale ambiente. Le connessioni non attive non influiscono sulla transazione.  
  
> [!NOTE]  
>  Non è possibile usare **SQLEndTran** per eseguire il commit o il rollback delle transazioni in un ambiente condiviso. Se **SQLEndTran** viene chiamato con *handle* impostato sull'handle di un ambiente condiviso o sull'handle di una connessione in un ambiente condiviso, viene restituito il valore SQLState HY092 (identificatore di opzione o attributo non valido).  
  
 Gestione driver restituirà SQL_SUCCESS solo se riceve SQL_SUCCESS per ogni connessione. Se Gestione driver riceve SQL_ERROR in una o più connessioni, restituisce SQL_ERROR all'applicazione e le informazioni di diagnostica vengono inserite nella struttura dei dati di diagnostica dell'ambiente. Per determinare la connessione o le connessioni non riuscite durante l'operazione di commit o di rollback, l'applicazione può chiamare **SQLGetDiagRec** per ogni connessione.  
  
> [!NOTE]  
>  Gestione driver non simula una transazione globale in tutte le connessioni e pertanto non usa protocolli di commit in due fasi.  
  
 Se *CompletionType* è SQL_COMMIT, **SQLEndTran** emette una richiesta di commit per tutte le operazioni attive in qualsiasi istruzione associata a una connessione interessata. Se *CompletionType* è SQL_ROLLBACK, **SQLEndTran** emette una richiesta di rollback per tutte le operazioni attive in qualsiasi istruzione associata a una connessione interessata. Se non è attiva alcuna transazione, **SQLEndTran** restituisce SQL_SUCCESS senza effetti sulle origini dati. Per ulteriori informazioni, vedere [commit e rollback delle transazioni](../../../odbc/reference/develop-app/committing-and-rolling-back-transactions.md).  
  
 Se il driver è in modalità di commit manuale (chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_AUTOCOMMIT impostato su SQL_AUTOCOMMIT_OFF), una nuova transazione viene avviata in modo implicito quando un'istruzione SQL che può essere contenuta in una transazione viene eseguita sull'origine dati corrente. Per altre informazioni, vedere [modalità di commit](../../../odbc/reference/develop-app/commit-mode.md).  
  
 Per determinare il modo in cui le operazioni di transazione influiscono sui cursori, un'applicazione chiama **SQLGetInfo** con le opzioni SQL_CURSOR_ROLLBACK_BEHAVIOR e SQL_CURSOR_COMMIT_BEHAVIOR. Per ulteriori informazioni, vedere i paragrafi seguenti e vedere anche l' [effetto delle transazioni sui cursori e sulle istruzioni preparate](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
 Se il valore SQL_CURSOR_ROLLBACK_BEHAVIOR o SQL_CURSOR_COMMIT_BEHAVIOR è uguale a SQL_CB_DELETE, **SQLEndTran** chiude ed Elimina tutti i cursori aperti in tutte le istruzioni associate alla connessione e ignora tutti i risultati in sospeso. **SQLEndTran** lascia qualsiasi istruzione presente in uno stato allocato (non preparato); l'applicazione può riutilizzarle per le successive richieste SQL oppure può chiamare **SQLFreeStmt** o **SQLFreeHandle** con *HandleType* di SQL_HANDLE_STMT per deallocarle.  
  
 Se il valore SQL_CURSOR_ROLLBACK_BEHAVIOR o SQL_CURSOR_COMMIT_BEHAVIOR è uguale a SQL_CB_CLOSE, **SQLEndTran** chiude tutti i cursori aperti in tutte le istruzioni associate alla connessione. **SQLEndTran** lascia qualsiasi istruzione presente in uno stato preparato. l'applicazione può chiamare **SQLExecute** per un'istruzione associata alla connessione senza prima chiamare **SQLPrepare**.  
  
 Se il valore SQL_CURSOR_ROLLBACK_BEHAVIOR o SQL_CURSOR_COMMIT_BEHAVIOR è uguale a SQL_CB_PRESERVE, **SQLEndTran** non influisce sui cursori aperti associati alla connessione. I cursori rimangono in corrispondenza della riga a cui puntava prima della chiamata a **SQLEndTran**.  
  
 Per i driver e le origini dati che supportano le transazioni, la chiamata di **SQLEndTran** con SQL_COMMIT o SQL_ROLLBACK quando nessuna transazione è attiva restituisce SQL_SUCCESS (a indicare che non è presente alcun lavoro di cui eseguire il commit o il rollback) e non ha alcun effetto sull'origine dati.  
  
 Quando un driver è in modalità autocommit, gestione driver non chiama **SQLEndTran** nel driver. **SQLEndTran** restituisce sempre SQL_SUCCESS indipendentemente dal fatto che venga chiamato con un *CompletionType* di SQL_COMMIT o SQL_ROLLBACK.  
  
 I driver o le origini dati che non supportano le transazioni ( *opzione* **SQLGetInfo** SQL_TXN_CAPABLE è SQL_TC_NONE) sono in realtà sempre in modalità autocommit e pertanto restituiscono sempre SQL_SUCCESS per **SQLEndTran** , indipendentemente dal fatto che vengano chiamati con una *CompletionType* di SQL_COMMIT o SQL_ROLLBACK. I driver e le origini dati in realtà non eseguono il rollback delle transazioni quando richiesto.  
  
## <a name="suspended-state"></a>Stato sospeso  
 Nelle gestioni driver rilasciate prima di Windows 7 è stata attiva una transazione se **SQLEndTran** ha restituito SQL_ERROR dal driver. Tuttavia, è possibile che sia stato eseguito correttamente il commit della transazione sul server, ma che il driver sul client non sia stato notificato, ad esempio perché si è verificato un errore di rete. Questa operazione lascia la connessione in uno stato non valido. A partire da Windows 7, quando **SQLEndTran** restituisce SQL_ERROR, la connessione potrebbe trovarsi in uno stato sospeso. In uno stato sospeso, è possibile chiamare funzioni di sola lettura. Infine, l'applicazione deve chiamare **SQLConnect** su una connessione sospesa per rilasciare le risorse.  
  
 Se tutte le condizioni seguenti sono vere, la connessione verrà messa in stato sospeso:  
  
-   Il driver restituisce SQL_ERROR da **SQLEndTran**.  
  
-   Il driver è ODBC versione 3,8 o successiva.  
  
-   La versione dell'applicazione è 3,8 o successiva. in alternativa, l'applicazione ODBC 2. x o 3. x ricompilata Annulla correttamente la funzione **SQLEndTran** tramite **SQLCancelHandle**.  
  
-   Il driver non ha restituito uno dei messaggi seguenti, che confermano che la transazione non è stata completata:  
  
    -   25S03: viene eseguito il rollback della transazione  
  
    -   40001: errore di serializzazione  
  
    -   40002: vincolo di integrità  
  
    -   HYC00: funzionalità facoltativa non implementata  
  
 Se **SQLEndTran** è stato chiamato su un handle di ambiente e una delle connessioni soddisfa le condizioni indicate sopra, tutte le connessioni che si connettono allo stesso driver verranno inserite nello stato Suspended.  
  
 Dopo che un'applicazione ha chiamato **Disconnect** su una connessione sospesa, è possibile usare la connessione per riconnettersi a un'altra origine dati o alla stessa origine dati.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Annullamento di una funzione in esecuzione in modo asincrono in un handle di connessione.|[Funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Restituzione di informazioni su un driver o un'origine dati|[Funzione SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Liberare un handle|[SQLFreeHandle Function](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Liberazione di un handle di istruzione|[SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Esecuzione asincrona (metodo di polling)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
