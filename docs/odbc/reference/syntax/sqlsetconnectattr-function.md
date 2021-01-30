---
description: Pagina relativa alla funzione SQLSetConnectAttr
title: Funzione SQLSetConnectAttr | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConnectAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConnectAttr
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC]
ms.assetid: 97fc7445-5a66-4eb9-8e77-10990b5fd685
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06935fe7eca6f19a8c6fcc052cd3a9503144a296
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192445"
---
# <a name="sqlsetconnectattr-function"></a>Pagina relativa alla funzione SQLSetConnectAttr
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLSetConnectAttr** imposta gli attributi che governano gli aspetti delle connessioni.  
  
> [!NOTE]
>  Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando un'applicazione ODBC 3 *. x* utilizza un driver ODBC 2 *. x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLSetConnectAttr(  
     SQLHDBC       ConnectionHandle,  
     SQLINTEGER    Attribute,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    StringLength);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *Attributo*  
 Input Attributo da impostare, elencato in "Commenti".  
  
 *ValuePtr*  
 Input Puntatore al valore da associare all' *attributo*. A seconda del valore dell' *attributo*, *ValuePtr* sarà un valore Unsigned Integer o punterà a una stringa di caratteri con terminazione null. Si noti che il tipo integrale dell'argomento *attribute* non può essere a lunghezza fissa. per informazioni dettagliate, vedere la sezione relativa ai commenti.  
  
 *StringLength*  
 Input Se *attribute* è un attributo definito da ODBC e *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di **ValuePtr*. Per i dati di tipo stringa di caratteri, questo argomento deve contenere il numero di byte nella stringa.  
  
 Se *attribute* è un attributo definito da ODBC e *ValuePtr* è un numero intero, *StringLength* viene ignorato.  
  
 Se *attribute* è un attributo definito dal driver, l'applicazione indica la natura dell'attributo a gestione driver impostando l'argomento *StringLength* . *StringLength* può avere i valori seguenti:  
  
-   Se *ValuePtr* è un puntatore a una stringa di caratteri, *StringLength* è la lunghezza della stringa o del SQL_NTS.  
  
-   Se *ValuePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *StringLength*. Questo inserisce un valore negativo in *StringLength*.  
  
-   Se *ValuePtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *StringLength* deve essere SQL_IS_POINTER.  
  
-   Se *ValuePtr* contiene un valore a lunghezza fissa, *StringLength* è SQL_IS_INTEGER o SQL_IS_UINTEGER, a seconda delle esigenze.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetConnectAttr** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_DBC e un *handle* di *connectionHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLSetConnectAttr** e ne illustra ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
 Il driver può restituire SQL_SUCCESS_WITH_INFO per fornire informazioni sul risultato dell'impostazione di un'opzione.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Il driver non supporta il valore specificato in *ValuePtr* e sostituisce un valore simile. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08002|Nome della connessione in uso|L'argomento dell' *attributo* è stato SQL_ATTR_ODBC_CURSORS e il driver era già connesso all'origine dati.|  
|08003|Connessione non aperta|(DM) è stato specificato un valore di *attributo* che richiedeva una connessione aperta, ma lo stato di *connectionHandle* non era connected.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|L'argomento dell' *attributo* è stato SQL_ATTR_CURRENT_CATALOG e un set di risultati era in sospeso.|  
|25000|Operazione non valida in una transazione locale|Una connessione si trovava in una transazione locale durante il tentativo di integrazione in una connessione di transazione distribuita (DTC) impostando l'attributo di connessione SQL_ATTR_ENLIST_IN_DTC.<br /><br /> Una connessione è già integrata in un DTC.<br /><br /> Una connessione è stata integrata in una connessione di transazione distribuita e una transazione locale è stata avviata impostando SQL_ATTR_AUTOCOMMIT su SQL_AUTOCOMMIT_OFF.|  
|3D000|Nome catalogo non valido|L'argomento dell' *attributo* è stato SQL_CURRENT_CATALOG e il nome del catalogo specificato non è valido.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *connectionHandle*. La funzione **SQLSetConnectAttr** è stata chiamata e prima del completamento dell'esecuzione è stata chiamata la [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) su *connectionHandle* e quindi la funzione **SQLSetConnectAttr** è stata chiamata nuovamente in *connectionHandle*.<br /><br /> In alternativa, la funzione  **SQLSetConnectAttr** è stata chiamata e prima del completamento dell'esecuzione, **SQLCancelHandle** è stato chiamato su *connectionHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|L'argomento dell' *attributo* ha identificato un attributo di connessione che richiede un valore stringa e l'argomento *ValuePtr* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per un *statementHandle* associato a *connectionHandle* ed è ancora in esecuzione quando è stato chiamato **SQLSetConnectAttr** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati a *connectionHandle* e restituiti SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per un *statementHandle* associato a *connectionHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) **SQLBrowseConnect** è stato chiamato per *ConnectionHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima che **SQLBrowseConnect** restituisse SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.|  
|HY011|Non è possibile impostare l'attributo adesso|L'argomento dell' *attributo* è stato SQL_ATTR_TXN_ISOLATION e una transazione è stata aperta.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY024|Valore di attributo non valido|Dato il valore dell' *attributo* specificato, in *ValuePtr* è stato specificato un valore non valido. (Gestione driver restituisce questo valore SQLSTATE solo per gli attributi di connessione e di istruzione che accettano un set discreto di valori, ad esempio SQL_ATTR_ACCESS_MODE o SQL_ATTR_ASYNC_ENABLE. Per tutti gli altri attributi di connessione e di istruzione, il driver deve verificare il valore specificato in *ValuePtr*.<br /><br /> L'argomento dell' *attributo* è SQL_ATTR_TRACEFILE o SQL_ATTR_TRANSLATE_LIB e *ValuePtr* è una stringa vuota.|  
|HY090|Lunghezza della stringa o del buffer non valida|*(DM) \* ValuePtr* è una stringa di caratteri e l'argomento *StringLength* è minore di 0 ma non è stato SQL_NTS.|  
|HY092|Identificatore di attributo/opzione non valido|(DM) il valore specificato per l' *attributo* argument non era valido per la versione di ODBC supportata dal driver.<br /><br /> (DM) il valore specificato per l' *attributo* argument è un attributo di sola lettura.|  
|HY114|Il driver non supporta l'esecuzione di funzioni asincrone a livello di connessione|(DM) un'applicazione ha tentato di abilitare l'esecuzione della funzione asincrona con SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE per un driver che non supporta le operazioni di connessione asincrona.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HY121|Non è possibile abilitare contemporaneamente la libreria di cursori e il pool di Driver-Aware|Per ulteriori informazioni, vedere [pool di connessioni compatibile](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)con i driver.|  
|HYC00|Funzionalità facoltativa non implementata|Il valore specificato per l' *attributo* argument è un attributo di istruzione o di connessione ODBC valido per la versione di ODBC supportata dal driver ma non supportato dal driver.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *connectionHandle* non supporta la funzione.|  
|IM009|Impossibile caricare la DLL di traduzione|Il driver non è stato in grado di caricare la DLL di traduzione specificata per la connessione. Questo errore può essere restituito solo quando l' *attributo* è SQL_ATTR_TRANSLATE_LIB.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
|S1118|Il driver non supporta la notifica asincrona|È stato impostato SQL_ATTR_ASYNC_DBC_EVENT (dopo la connessione), ma la notifica asincrona non è supportata dal driver.|  
  
 Quando *attribute* è un attributo Statement, **SQLSetConnectAttr** può restituire qualsiasi SQLSTATE restituito da **SQLSetStmtAttr**.  
  
## <a name="comments"></a>Commenti  
 Per informazioni generali sugli attributi di connessione, vedere [attributi di connessione](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Gli attributi attualmente definiti e la versione di ODBC in cui sono stati introdotti vengono visualizzati nella tabella più avanti in questa sezione. si prevede che verranno definiti più attributi per sfruttare le diverse origini dati. Un intervallo di attributi è riservato da ODBC; gli sviluppatori di driver devono riservare i valori per l'uso specifico del driver da un gruppo aperto.  
  
> [!NOTE]
>  La possibilità di impostare gli attributi di istruzione a livello di connessione chiamando **SQLSetConnectAttr** è stata deprecata in ODBC 3 *. x*. Le applicazioni ODBC 3 *. x* non devono mai impostare gli attributi di istruzione a livello di connessione. Gli attributi dell'istruzione ODBC 3 *. x* non possono essere impostati a livello di connessione, ad eccezione degli attributi SQL_ATTR_METADATA_ID e SQL_ATTR_ASYNC_ENABLE, che sono sia attributi di connessione sia attributi di istruzione e possono essere impostati a livello di connessione o di istruzione.  
> 
>  I driver ODBC 3 *. x* devono supportare questa funzionalità solo se dovrebbero funzionare con le applicazioni ODBC 2 *. x* che impostano le opzioni dell'istruzione ODBC 2 *. x* a livello di connessione. Per ulteriori informazioni, vedere [SQLSetConnectOption mapping](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md) in Appendice G: linee guida sui driver per la compatibilità con le versioni precedenti.  
  
 Un'applicazione può chiamare **SQLSetConnectAttr** in qualsiasi momento tra il momento in cui la connessione viene allocata e liberata. Tutti gli attributi di connessione e di istruzione impostati correttamente dall'applicazione per la connessione vengono mantenuti fino a quando non viene chiamato **SQLFreeHandle** sulla connessione. Se, ad esempio, un'applicazione chiama **SQLSetConnectAttr** prima di connettersi a un'origine dati, l'attributo viene reso persistente anche in caso di errore di **SQLSetConnectAttr** nel driver quando l'applicazione si connette all'origine dati. Se un'applicazione imposta un attributo specifico del driver, l'attributo viene reso permanente anche se l'applicazione si connette a un driver diverso per la connessione.  
  
 Alcuni attributi di connessione possono essere impostati solo prima che venga stabilita una connessione. altri possono essere impostati solo dopo che è stata stabilita una connessione. La tabella seguente indica gli attributi di connessione che è necessario impostare prima o dopo che è stata stabilita una connessione. *Indica che* l'attributo può essere impostato prima o dopo una connessione.  
  
|Attributo|Impostare prima o dopo la connessione?|  
|---------------|-------------------------------------|  
|SQL_ATTR_ACCESS_MODE|[1]|  
|SQL_ATTR_ASYNC_DBC_EVENT|Prima o dopo|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE|[4]|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK|Prima o dopo|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT|Prima o dopo|  
|SQL_ATTR_ASYNC_ENABLE|[2]|  
|SQL_ATTR_AUTO_IPD|Prima o dopo|  
|SQL_ATTR_AUTOCOMMIT|[5]|  
|SQL_ATTR_CONNECTION_DEAD|After|  
|SQL_ATTR_CONNECTION_TIMEOUT|Prima o dopo|  
|SQL_ATTR_CURRENT_CATALOG|[1]|  
|SQL_ATTR_DBC_INFO_TOKEN|After|  
|SQL_ATTR_ENLIST_IN_DTC|After|  
|SQL_ATTR_LOGIN_TIMEOUT|Prima|  
|SQL_ATTR_METADATA_ID|Prima o dopo|  
|SQL_ATTR_ODBC_CURSORS|Prima|  
|SQL_ATTR_PACKET_SIZE|Prima|  
|SQL_ATTR_QUIET_MODE|Prima o dopo|  
|SQL_ATTR_TRACE|Prima o dopo|  
|SQL_ATTR_TRACEFILE|Prima o dopo|  
|SQL_ATTR_TRANSLATE_LIB|After|  
|SQL_ATTR_TRANSLATE_OPTION|After|  
|SQL_ATTR_TXN_ISOLATION|[3]|  
  
 [1] SQL_ATTR_ACCESS_MODE e SQL_ATTR_CURRENT_CATALOG possono essere impostate prima o dopo la connessione, a seconda del driver. Tuttavia, le applicazioni interoperative le impostano prima della connessione perché alcuni driver non supportano la modifica di questi elementi dopo la connessione.  
  
 [2] SQL_ATTR_ASYNC_ENABLE necessario impostare prima che sia presente un'istruzione attiva.  
  
 [3] SQL_ATTR_TXN_ISOLATION può essere impostata solo se non sono presenti transazioni aperte sulla connessione. Alcuni attributi di connessione supportano la sostituzione di un valore simile se l'origine dati non supporta il valore specificato in \* *ValuePtr*. In questi casi, il driver restituisce SQL_SUCCESS_WITH_INFO e SQLSTATE 01S02 (il valore dell'opzione è stato modificato). Se, ad esempio, *attribute* è SQL_ATTR_PACKET_SIZE e \* *ValuePtr* supera la dimensione massima del pacchetto, il driver sostituisce le dimensioni massime. Per determinare il valore sostituito, un'applicazione chiama **SQLGetConnectAttr**.  
  
 [4] se SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE viene impostato prima dell'apertura di una connessione, gestione driver imposta l'attributo del driver quando il driver viene caricato durante una chiamata a **SQLBrowseConnect**, **SQLConnect** o **SQLDriverConnect**. Prima di una chiamata a **SQLBrowseConnect**, **SQLConnect** o **SQLDriverConnect**, gestione driver non sa quale driver connettersi a e non sa se il driver supporta le operazioni di connessione asincrona. Di conseguenza, gestione driver restituisce sempre SQL_SUCCESS. Tuttavia, se il driver non supporta le operazioni di connessione asincrone, la chiamata a **SQLBrowseConnect**, **SQLConnect** o **SQLDriverConnect** avrà esito negativo.  
  
 [5] quando SQL_ATTR_AUTOCOMMIT è impostato su FALSE, le applicazioni devono chiamare SQLEndTran (SQL_ROLLBACK) se un'API restituisce SQL_ERROR per garantire la coerenza delle transazioni.  
  
 Il formato delle informazioni impostate nel buffer \* *ValuePtr* dipende dall' *attributo* specificato. **SQLSetConnectAttr** accetterà le informazioni sugli attributi in uno dei due formati seguenti: una stringa di caratteri con terminazione null o un valore integer. Il formato di ogni è indicato nella descrizione dell'attributo. Le stringhe di caratteri a cui punta l'argomento *ValuePtr* di **SQLSetConnectAttr** hanno una lunghezza di *StringLength* byte.  
  
 L'argomento *StringLength* viene ignorato se la lunghezza è definita dall'attributo, come nel caso di tutti gli attributi introdotti in ODBC 2 *. x* o versioni precedenti.  
  
|*Attributo*|Contenuto di *ValuePtr*|  
|-----------------|-------------------------|  
|SQL_ATTR_ACCESS_MODE (ODBC 1,0)|Valore SQLUINTEGER. SQL_MODE_READ_ONLY viene utilizzato dal driver o dall'origine dati come indicatore del mancato supporto della connessione per supportare istruzioni SQL che determinano l'esecuzione degli aggiornamenti. Questa modalità può essere utilizzata per ottimizzare le strategie di blocco, la gestione delle transazioni o altre aree in base alle esigenze del driver o dell'origine dati. Il driver non è necessario per impedire che tali istruzioni vengano inviate all'origine dati. Il comportamento del driver e dell'origine dati quando viene richiesto di elaborare istruzioni SQL che non sono di sola lettura durante una connessione di sola lettura è definito dall'implementazione. Il valore predefinito è SQL_MODE_READ_WRITE.|  
|SQL_ATTR_ASYNC_DBC_EVENT (ODBC 3,8)|Valore SQLPOINTER che rappresenta un handle di evento.<br /><br /> La notifica del completamento delle funzioni asincrone viene abilitata chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_ASYNC_STMT_EVENT e specificando l'handle dell'evento. **Nota:**  Il metodo di notifica non è supportato con la libreria di cursori. Un'applicazione riceverà un messaggio di errore se tenta di abilitare la libreria di cursori tramite SQLSetConnectAttr, quando il metodo di notifica è abilitato.|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE (ODBC 3,8)|Valore SQLUINTEGER che Abilita o Disabilita l'esecuzione asincrona delle funzioni selezionate nell'handle di connessione. Per ulteriori informazioni, vedere [esecuzione asincrona (metodo di polling)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).<br /><br /> SQL_ASYNC_DBC_ENABLE_ON = Abilita l'operazione asincrona per le funzioni specificate correlate alla connessione.<br /><br /> SQL_ASYNC_DBC_ENABLE_OFF = (impostazione predefinita) Disabilita l'operazione asincrona per le funzioni specificate correlate alla connessione.<br /><br /> L'impostazione SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE è sempre sincrona (ovvero non restituirà mai SQL_STILL_EXECUTING).<br /><br /> L'esecuzione asincrona delle operazioni di istruzione è abilitata con SQL_ATTR_ASYNC_ENABLE.|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK (ODBC 3,8)|Valore SQLPOINTER che punta alla struttura del contesto.<br /><br /> Solo Gestione driver può chiamare la funzione **SQLSetStmtAttr** di un driver con questo attributo.|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT (ODBC 3,8)|Valore SQLPOINTER che punta alla struttura del contesto.<br /><br /> Solo Gestione driver può chiamare la funzione **SQLSetStmtAttr** di un driver con questo attributo.|  
|SQL_ATTR_ASYNC_ENABLE (ODBC 3,0)|Valore SQLULEN che specifica se una funzione chiamata con un'istruzione nella connessione specificata viene eseguita in modo asincrono:<br /><br /> SQL_ASYNC_ENABLE_OFF = Disabilita il supporto per l'esecuzione asincrona a livello di connessione per le operazioni di istruzione (impostazione predefinita).<br /><br /> SQL_ASYNC_ENABLE_ON = Abilita supporto per l'esecuzione asincrona a livello di connessione per le operazioni di istruzione.<br /><br /> Questo attributo può essere impostato se **SQLGetInfo** con il tipo di informazioni SQL_ASYNC_MODE restituisce SQL_AM_CONNECTION o SQL_AM_STATEMENT.|  
|SQL_ATTR_AUTO_IPD (ODBC 3,0)|Valore SQLUINTEGER di sola lettura che specifica se il popolamento automatico del dip dopo una chiamata a **SQLPrepare** è supportato:<br /><br /> SQL_TRUE = il popolamento automatico del dip dopo una chiamata a **SQLPrepare** è supportato dal driver.<br /><br /> SQL_FALSE = il popolamento automatico del dip dopo una chiamata a **SQLPrepare** non è supportato dal driver. I server che non supportano le istruzioni preparate non saranno in grado di popolare automaticamente il dpi.<br /><br /> Se SQL_TRUE viene restituito per l'attributo di connessione SQL_ATTR_AUTO_IPD, l'attributo dell'istruzione SQL_ATTR_ENABLE_AUTO_IPD può essere impostato per attivare o disattivare il popolamento automatico del dpi. Se SQL_ATTR_AUTO_IPD è SQL_FALSE, non è possibile impostare SQL_ATTR_ENABLE_AUTO_IPD su SQL_TRUE. Il valore predefinito di SQL_ATTR_ENABLE_AUTO_IPD è uguale al valore di SQL_ATTR_AUTO_IPD.<br /><br /> Questo attributo di connessione può essere restituito da **SQLGetConnectAttr** , ma non può essere impostato da **SQLSetConnectAttr**.|  
|SQL_ATTR_AUTOCOMMIT (ODBC 1,0)|Valore SQLUINTEGER che specifica se utilizzare la modalità autocommit o con commit manuale:<br /><br /> SQL_AUTOCOMMIT_OFF = il driver usa la modalità con commit manuale e l'applicazione deve eseguire il commit o il rollback delle transazioni in modo esplicito con **SQLEndTran**.<br /><br /> SQL_AUTOCOMMIT_ON = il driver usa la modalità autocommit. Viene eseguito il commit di ogni istruzione immediatamente dopo l'esecuzione. Questo è il valore predefinito. Viene eseguito il commit di tutte le transazioni aperte sulla connessione quando SQL_ATTR_AUTOCOMMIT è impostato su SQL_AUTOCOMMIT_ON per passare dalla modalità con commit manuale alla modalità autocommit.<br /><br /> Per altre informazioni, vedere [modalità di commit](../../../odbc/reference/develop-app/commit-mode.md). **Importante:**  Alcune origini dati eliminano i piani di accesso e chiudono i cursori per tutte le istruzioni in una connessione ogni volta che viene eseguito il commit di un'istruzione. la modalità autocommit può causare questo problema dopo l'esecuzione di ogni istruzione non di query o quando il cursore viene chiuso per una query. Per ulteriori informazioni, vedere i tipi di informazioni SQL_CURSOR_COMMIT_BEHAVIOR e SQL_CURSOR_ROLLBACK_BEHAVIOR in [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) e l' [effetto delle transazioni sui cursori e sulle istruzioni preparate](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md). <br /><br /> Quando un batch viene eseguito in modalità autocommit, è possibile procedere in due modi. L'intero batch può essere considerato come un'unità autocommitbile o ogni istruzione in un batch viene considerata come un'unità autocommitbile. Alcune origini dati sono in grado di supportare entrambi questi comportamenti e possono fornire un modo per scegliere uno o l'altro. Viene definito dal driver se un batch viene considerato come un'unità autocommitbile o se ogni singola istruzione all'interno del batch è autocommitbile.|  
|SQL_ATTR_CONNECTION_DEAD<br /><br /> (ODBC 3,5)|Valore SQLUINTEGER di sola lettura che indica lo stato della connessione. Se SQL_CD_TRUE, la connessione è stata persa. Se SQL_CD_FALSE, la connessione rimane attiva.|  
|SQL_ATTR_CONNECTION_TIMEOUT (ODBC 3,0)|Valore SQLUINTEGER che corrisponde al numero di secondi di attesa per il completamento di una richiesta sulla connessione prima di tornare all'applicazione. Il driver deve restituire SQLSTATE HYT00 (timeout scaduto) in qualsiasi momento in cui è possibile che si verifichi un timeout in una situazione non associata all'esecuzione della query o all'account di accesso.<br /><br /> Se *ValuePtr* è uguale a 0 (impostazione predefinita), non è previsto alcun timeout.|  
|SQL_ATTR_CURRENT_CATALOG (ODBC 2,0)|Stringa di caratteri contenente il nome del catalogo che deve essere utilizzato dall'origine dati. Ad esempio, in SQL Server, il catalogo è un database, pertanto il driver invia un'istruzione **use** _database_ all'origine dati, dove *database* è il database specificato in \* *ValuePtr*. Per un driver a livello singolo, il catalogo potrebbe essere una directory, quindi il driver cambia la directory corrente con la directory specificata in **ValuePtr*.|  
|SQL_ATTR_DBC_INFO_TOKEN (ODBC 3,8|Valore SQLPOINTER utilizzato per impostare di nuovo il token delle informazioni di connessione nell'handle DBC quando il parametro di [SQLRateConnection](../../../odbc/reference/syntax/sqlrateconnection-function.md)( \* *Prating*) non è uguale a 100.<br /><br /> SQL_ATTR_DBC_INFO_TOKEN è di sola impostazione. Non è possibile usare **SQLGetConnectAttr** o **SQLGetConnectOption** per recuperare questo valore. L'oggetto **SQLSetConnectAttr** di gestione driver non accetta SQL_ATTR_DBC_INFO_TOKEN, perché un'applicazione non deve impostare questo attributo.<br /><br /> Se un driver restituisce SQL_ERROR dopo aver impostato SQL_ATTR_DBC_INFO_TOKEN, la connessione appena ottenuta dal pool verrà liberata. Gestione driver tenterà quindi di ottenere un'altra connessione dal pool. Per ulteriori informazioni, vedere [sviluppo Connection-Pool consapevolezza in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md) .|  
|SQL_ATTR_ENLIST_IN_DTC (ODBC 3,0)|Valore SQLPOINTER che specifica se utilizzare il driver ODBC nelle transazioni distribuite coordinate da Servizi componenti Microsoft.<br /><br /> Passare un oggetto transazione OLE DTC che specifica la transazione da esportare in SQL Server o SQL_DTC_DONE per terminare l'associazione DTC della connessione.<br /><br /> Il client chiama il metodo OLE ITransactionDispenser:: BeginTransaction di Microsoft Distributed Transaction Coordinator (MS DTC) per iniziare una transazione MS DTC e creare un oggetto transazione MS DTC che rappresenta la transazione. L'applicazione chiama quindi SQLSetConnectAttr con l'opzione SQL_ATTR_ENLIST_IN_DTC per associare l'oggetto transazione alla connessione ODBC. Tutte le attività del database correlate verranno eseguite sotto la protezione della transazione MS DTC. L'applicazione chiama SQLSetConnectAttr con SQL_DTC_DONE per terminare l'associazione DTC della connessione. Per ulteriori informazioni, vedere la documentazione di MS DTC.|  
|SQL_ATTR_LOGIN_TIMEOUT (ODBC 1,0)|Valore SQLUINTEGER che corrisponde al numero di secondi di attesa per il completamento di una richiesta di accesso prima di tornare all'applicazione. Il valore predefinito è dipendente dal driver. Se *ValuePtr* è 0, il timeout è disabilitato e un tentativo di connessione resterà in attesa per un periodo illimitato.<br /><br /> Se il timeout specificato supera il timeout massimo di accesso nell'origine dati, il driver sostituisce tale valore e restituisce SQLSTATE 01S02 (valore di opzione modificato).|  
|SQL_ATTR_METADATA_ID (ODBC 3,0)|Valore SQLUINTEGER che determina il modo in cui vengono gestiti gli argomenti di stringa delle funzioni di catalogo.<br /><br /> Se SQL_TRUE, l'argomento di stringa delle funzioni di catalogo viene considerato come identificatori. Il caso non è significativo. Per le stringhe non delimitate, il driver rimuove tutti gli spazi finali e la stringa viene riflessa in maiuscolo. Per le stringhe delimitate, il driver rimuove tutti gli spazi iniziali o finali e accetta letteralmente qualsiasi valore tra i delimitatori. Se uno di questi argomenti è impostato su un puntatore null, la funzione restituisce SQL_ERROR e SQLSTATE HY009 (utilizzo non valido del puntatore null).<br /><br /> Se SQL_FALSE, gli argomenti di stringa delle funzioni di catalogo non vengono considerati come identificatori. Il caso è significativo. Possono contenere un criterio di ricerca di stringhe o meno, a seconda dell'argomento.<br /><br /> Il valore predefinito è SQL_FALSE.<br /><br /> L'argomento *TableType* di **SQLTables**, che accetta un elenco di valori, non è influenzato da questo attributo.<br /><br /> SQL_ATTR_METADATA_ID possono essere impostate anche a livello di istruzione. È l'unico attributo di connessione che è anche un attributo di istruzione.<br /><br /> Per ulteriori informazioni, vedere [arguments in Catalog Functions](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).|  
|SQL_ATTR_ODBC_CURSORS (ODBC 2,0)|Valore SQLULEN che specifica la modalità di utilizzo della libreria di cursori ODBC da parte di gestione driver:<br /><br /> SQL_CUR_USE_IF_NEEDED = Gestione driver utilizza la libreria di cursori ODBC solo se necessaria. Se il driver supporta l'opzione SQL_FETCH_PRIOR in **SQLFetchScroll**, gestione driver utilizza le funzionalità di scorrimento del driver. In caso contrario, viene utilizzata la libreria di cursori ODBC.<br /><br /> SQL_CUR_USE_ODBC = Gestione driver utilizza la libreria di cursori ODBC.<br /><br /> SQL_CUR_USE_DRIVER = Gestione driver utilizza le funzionalità di scorrimento del driver. Si tratta dell'impostazione predefinita.<br /><br /> Per ulteriori informazioni sulla libreria di cursori ODBC, vedere [Appendice F: libreria di cursori ODBC](../../../odbc/reference/appendixes/appendix-f-odbc-cursor-library.md). **Avviso:**  La libreria di cursori verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.|  
|SQL_ATTR_PACKET_SIZE (ODBC 2,0)|Valore SQLUINTEGER che specifica le dimensioni del pacchetto di rete in byte. **Nota:**  Molte origini dati non supportano questa opzione oppure possono solo restituire ma non impostare le dimensioni del pacchetto di rete. <br /><br /> Se le dimensioni specificate superano le dimensioni massime del pacchetto o sono inferiori alle dimensioni minime del pacchetto, il driver sostituisce tale valore e restituisce SQLSTATE 01S02 (il valore dell'opzione è stato modificato).<br /><br /> Se l'applicazione imposta le dimensioni del pacchetto dopo che è già stata stabilita una connessione, il driver restituirà SQLSTATE HY011 (l'attributo non può essere impostato adesso).|  
|SQL_ATTR_QUIET_MODE (ODBC 2,0)|Handle di finestra (HWND).<br /><br /> Se l'handle della finestra è un puntatore null, non vengono visualizzate finestre di dialogo.<br /><br /> Se l'handle della finestra non è un puntatore null, deve essere l'handle della finestra padre dell'applicazione. Questo è il valore predefinito. Il driver utilizza questo handle per visualizzare le finestre di dialogo. **Nota:**  L'attributo di connessione SQL_ATTR_QUIET_MODE non si applica alle finestre di dialogo visualizzate da **SQLDriverConnect**.|  
|SQL_ATTR_TRACE (ODBC 1,0)|Valore SQLUINTEGER che indica a gestione driver se eseguire la traccia:<br /><br /> SQL_OPT_TRACE_OFF = tracciato disattivato (impostazione predefinita)<br /><br /> SQL_OPT_TRACE_ON = traccia<br /><br /> Quando la traccia è attiva, gestione driver scrive ogni chiamata di funzione ODBC nel file di traccia. **Nota:**  Quando la traccia è attiva, gestione driver può restituire SQLSTATE IM013 (Error file di traccia) da qualsiasi funzione. <br /><br /> Un'applicazione specifica un file di traccia con l'opzione SQL_ATTR_TRACEFILE. Se il file esiste già, gestione driver aggiunge al file. In caso contrario, viene creato il file. Se la traccia è impostata su on e non è stato specificato alcun file di traccia, gestione driver scrive nel file SQL. ACCEDERE alla directory radice.<br /><br /> Un'applicazione può impostare la variabile **ODBCSharedTraceFlag** per abilitare la traccia in modo dinamico. La traccia viene quindi abilitata per tutte le applicazioni ODBC attualmente in esecuzione. Se un'applicazione disattiva la traccia, viene disattivata solo per tale applicazione.<br /><br /> Se la parola chiave **Trace** nelle informazioni di sistema è impostata su 1 quando un'applicazione chiama **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_ENV, la traccia è abilitata per tutti gli handle. Questa funzionalità è abilitata solo per l'applicazione che ha chiamato **SQLAllocHandle**.<br /><br /> Per chiamare **SQLSetConnectAttr** con un *attributo* di SQL_ATTR_TRACE non è necessario che l'argomento *connectionHandle* sia valido e non restituirà SQL_ERROR se *connectionHandle* è null. Questo attributo si applica a tutte le connessioni.|  
|SQL_ATTR_TRACEFILE (ODBC 1,0)|Stringa di caratteri con terminazione null che contiene il nome del file di traccia.<br /><br /> Il valore predefinito dell'attributo SQL_ATTR_TRACEFILE viene specificato con la parola chiave **TraceFile** nelle informazioni di sistema. Per ulteriori informazioni, vedere la [sottochiave ODBC](../../../odbc/reference/install/odbc-subkey.md).<br /><br /> La chiamata a **SQLSetConnectAttr** con un *attributo* di SQL_ATTR_TRACEFILE non richiede che l'argomento *connectionHandle* sia valido e non restituirà SQL_ERROR se *connectionHandle* non è valido. Questo attributo si applica a tutte le connessioni.|  
|SQL_ATTR_TRANSLATE_LIB (ODBC 1,0)|Stringa di caratteri con terminazione null contenente il nome di una libreria contenente le funzioni **SQLDriverToDataSource** e **SQLDataSourceToDriver** a cui il driver accede per eseguire attività come la conversione del set di caratteri. Questa opzione può essere specificata solo se il driver si è connesso all'origine dati. L'impostazione di questo attributo verrà mantenute tra le connessioni. Per ulteriori informazioni sulla conversione dei dati, vedere la pagina relativa alle DLL di [traduzione](../../../odbc/reference/develop-app/translation-dlls.md) e ai [riferimenti alle funzioni DLL della traduzione](../../../odbc/reference/syntax/translation-dll-api-reference.md).|  
|SQL_ATTR_TRANSLATE_OPTION (ODBC 1,0)|Valore del flag a 32 bit passato alla DLL di traduzione. Questo attributo può essere specificato solo se il driver si è connesso all'origine dati. Per informazioni sulla conversione dei dati, vedere [dll di traduzione](../../../odbc/reference/develop-app/translation-dlls.md).|  
|SQL_ATTR_TXN_ISOLATION (ODBC 1,0)|Maschera di bit a 32 bit che imposta il livello di isolamento della transazione per la connessione corrente. Un'applicazione deve chiamare **SQLEndTran** per eseguire il commit o il rollback di tutte le transazioni aperte in una connessione, prima di chiamare **SQLSetConnectAttr** con questa opzione.<br /><br /> I valori validi per *ValuePtr* possono essere determinati chiamando **SQLGetInfo** con *InfoType* uguale a SQL_TXN_ISOLATION_OPTIONS.<br /><br /> Per una descrizione dei livelli di isolamento delle transazioni, vedere la descrizione del tipo di informazioni SQL_DEFAULT_TXN_ISOLATION in [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) e [livelli di isolamento delle transazioni](../../../odbc/reference/develop-app/transaction-isolation-levels.md).|  
  
 [1] queste funzioni possono essere chiamate in modo asincrono solo se il descrittore è un descrittore di implementazione, non un descrittore di applicazione.  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Restituzione dell'impostazione di un attributo di connessione|[Funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
