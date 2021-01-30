---
description: Funzione SQLConnect
title: Funzione SQLConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConnect
helpviewer_keywords:
- SQLConnect function [ODBC]
ms.assetid: 59075e46-a0ca-47bf-972a-367b08bb518d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 90d982db52e0656fba25eb54065ea84b751a7b18
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206134"
---
# <a name="sqlconnect-function"></a>Funzione SQLConnect
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLConnect** stabilisce connessioni a un driver e a un'origine dati. L'handle di connessione fa riferimento all'archiviazione di tutte le informazioni sulla connessione all'origine dati, incluso lo stato, lo stato della transazione e le informazioni sull'errore.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLConnect(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      ServerName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      UserName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      Authentication,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *ServerName*  
 Input Nome dell'origine dati. I dati potrebbero trovarsi nello stesso computer del programma o in un altro computer in una rete. Per informazioni sul modo in cui un'applicazione sceglie un'origine dati, vedere scelta di un' [origine dati o](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)di un driver.  
  
 *NameLength1*  
 Input Lunghezza di **ServerName* in caratteri.  
  
 *UserName*  
 Input Identificatore utente.  
  
 *NameLength2*  
 Input Lunghezza di **username* in caratteri.  
  
 *autenticazione*  
 Input Stringa di autenticazione (in genere la password).  
  
 *NameLength3*  
 Input Lunghezza di **autenticazione* in caratteri.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLConnect** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_DBC e un *handle* di *connectionHandle*. Nella tabella seguente sono elencati i valori SQLSTATE generalmente restituiti da **SQLConnect** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Il driver non supporta il valore specificato dell'argomento *ValuePtr* in **SQLSetConnectAttr** e sostituisce un valore simile. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08001|Il client non è in grado di stabilire la connessione|Il driver non è stato in grado di stabilire una connessione con l'origine dati.|  
|08002|Nome della connessione in uso|(DM) il *connectionHandle* specificato è già stato utilizzato per stabilire una connessione con un'origine dati e la connessione è ancora aperta oppure l'utente sta cercando una connessione.|  
|08004|Il server ha rifiutato la connessione|L'origine dati ha rifiutato la creazione della connessione per motivi definiti dall'implementazione.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui il driver stava tentando di connettersi non è riuscito prima del completamento dell'elaborazione della funzione.|  
|28000|Specifica di autorizzazione non valida|Il valore specificato per il *nome utente* dell'argomento o il valore specificato per l' *autenticazione* con argomenti ha violato le restrizioni definite dall'origine dati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|(DM) Gestione driver non è in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *connectionHandle*. È stata chiamata la funzione **SQLConnect** e prima del completamento dell'esecuzione è stata chiamata la [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) su *connectionHandle*, quindi la funzione **SQLConnect** è stata chiamata nuovamente nel *connectionHandle*.<br /><br /> In alternativa, è stata chiamata la funzione **SQLConnect** e prima del completamento dell'esecuzione **SQLCancelHandle** è stato chiamato su *connectionHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *NameLength1*, *NameLength2* o *NameLength3* è minore di 0 ma non uguale a SQL_NTS.<br /><br /> (DM) il valore specificato per l'argomento *NameLength1* supera la lunghezza massima consentita per il nome di un'origine dati.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima del completamento della connessione all'origine dati. Il periodo di timeout viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HY114|Il driver non supporta l'esecuzione di funzioni asincrone a livello di connessione|(DM) l'applicazione ha abilitato l'operazione asincrona sull'handle di connessione prima di effettuare la connessione. Tuttavia, il driver non supporta le operazioni asincrone sull'handle di connessione.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver specificato dal nome dell'origine dati non supporta la funzione.|  
|IM002|Origine dati non trovata e non è stato specificato alcun driver predefinito|(DM) il nome dell'origine dati specificato nell'argomento *ServerName* non è stato trovato nelle informazioni di sistema, né esiste una specifica del driver predefinita.|  
|IM003|Impossibile connettere il driver specificato a|(DM) il driver elencato nella specifica dell'origine dati nelle informazioni di sistema non è stato trovato o non è stato possibile connettersi a per altri motivi.|  
|IM004|SQLAllocHandle del driver su SQL_HANDLE_ENV non riuscito|(DM) durante **SQLConnect**, gestione driver ha chiamato la funzione **SQLAllocHandle** del driver con un *HandleType* di SQL_HANDLE_ENV e il driver ha restituito un errore.|  
|IM005|SQLAllocHandle del driver su SQL_HANDLE_DBC non riuscito|(DM) durante **SQLConnect**, gestione driver ha chiamato la funzione **SQLAllocHandle** del driver con un *HandleType* di SQL_HANDLE_DBC e il driver ha restituito un errore.|  
|IM006|Errore di SQLSetConnectAttr del driver|Durante **SQLConnect**, gestione driver ha chiamato la funzione **SQLSetConnectAttr** del driver e il driver ha restituito un errore. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|IM009|Impossibile connettersi alla DLL di traduzione|Il driver non è stato in grado di connettersi alla DLL di traduzione specificata per l'origine dati.|  
|IM010|Nome origine dati troppo lungo|(DM) *\* nomeserver* è più lungo di SQL_MAX_DSN_LENGTH caratteri.|  
|IM014|Il DSN specificato contiene una mancata corrispondenza dell'architettura tra il driver e l'applicazione|(DM) l'applicazione a 32 bit utilizza un DSN che si connette a un driver a 64 bit. o viceversa.|  
|IM015|SQLConnect del driver su SQL_HANDLE_DBC_INFO_HANDLE non riuscito|Se un driver restituisce SQL_ERROR, gestione driver restituirà SQL_ERROR all'applicazione e la connessione avrà esito negativo.<br /><br /> Per ulteriori informazioni su SQL_HANDLE_DBC_INFO_TOKEN, vedere [sviluppo di Connection-Pool awareness in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
|S1118|Il driver non supporta la notifica asincrona|Quando il driver non supporta la notifica asincrona, non è possibile impostare SQL_ATTR_ASYNC_DBC_EVENT o SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="comments"></a>Commenti  
 Per informazioni sui motivi per cui un'applicazione usa **SQLConnect**, vedere [connessione con SQLConnect](../../../odbc/reference/develop-app/connecting-with-sqlconnect.md).  
  
 Gestione driver non si connette a un driver finché l'applicazione non chiama una funzione (**SQLConnect**, **SQLDriverConnect** o **SQLBrowseConnect**) per connettersi al driver. Fino a quel momento, gestione driver funziona con i propri handle e gestisce le informazioni di connessione. Quando l'applicazione chiama una funzione di connessione, gestione driver controlla se un driver è attualmente connesso a per il *connectionHandle* specificato:  
  
-   Se un driver non è connesso a, gestione driver si connette al driver e chiama **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_ENV, **SQLAllocHandle** con *HandleType* di SQL_HANDLE_DBC, **SQLSetConnectAttr** (se l'applicazione ha specificato eventuali attributi di connessione) e la funzione di connessione nel driver. Gestione driver restituisce SQLSTATE IM006 ( **SQLSetConnectOption** del driver non riuscita) e SQL_SUCCESS_WITH_INFO per la funzione di connessione se il driver ha restituito un errore per **SQLSetConnectAttr**. Per ulteriori informazioni, vedere [connessione a un'origine dati o a un driver](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md).  
  
-   Se il driver specificato è già connesso a in *connectionHandle*, gestione driver chiama solo la funzione di connessione nel driver. In questo caso, il driver deve assicurarsi che tutti gli attributi di connessione per il *connectionHandle* mantengano le impostazioni correnti.  
  
-   Se un altro driver è connesso a, gestione driver chiama **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DBC, quindi, se nessun altro driver è connesso a in tale ambiente, chiama **SQLFreeHandle** con una *HandleType* di SQL_HANDLE_ENV nel driver connesso e quindi disconnette tale driver. Esegue quindi le stesse operazioni di quando un driver non è connesso a.  
  
 Il driver alloca quindi gli handle e Inizializza se stesso.  
  
 Quando l'applicazione chiama **Disconnect**, gestione driver chiama **SQLConnect** nel driver. Tuttavia, il driver non viene disconnesso. In questo modo, il driver viene mantenuto in memoria per le applicazioni che si connettono ripetutamente a un'origine dati. Quando l'applicazione chiama **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DBC, gestione driver chiama **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DBC, quindi **SQLFreeHandle** con una *HandleType* di SQL_HANDLE_ENV nel driver, quindi disconnette il driver.  
  
 Un'applicazione ODBC può stabilire più di una connessione.  
  
## <a name="driver-manager-guidelines"></a>Linee guida per Gestione driver  
 Il contenuto di **ServerName* influisce sul modo in cui gestione driver e un driver collaborano per stabilire una connessione a un'origine dati.  
  
-   Se \* *ServerName* contiene un nome di origine dati valido, gestione driver individua la specifica dell'origine dati corrispondente nelle informazioni di sistema e si connette al driver associato. Gestione driver passa ogni argomento **SQLConnect** al driver.  
  
-   Se non è possibile trovare il nome dell'origine dati o *nomeserver* è un puntatore null, gestione driver individua la specifica dell'origine dati predefinita e si connette al driver associato. Gestione driver passa al driver il *nome utente* e gli argomenti di *autenticazione* non modificati e "predefinito" per l'argomento *ServerName* .  
  
-   Se l'argomento *ServerName* è "default", gestione driver individua la specifica dell'origine dati predefinita e si connette al driver associato. Gestione driver passa ogni argomento **SQLConnect** al driver.  
  
-   Se non è possibile trovare il nome dell'origine dati oppure *ServerName* è un puntatore null e la specifica dell'origine dati predefinita non esiste, Gestione Driver restituisce SQL_ERROR con SQLSTATE IM002 (il nome dell'origine dati non è stato trovato e non è stato specificato alcun driver predefinito).  
  
 Dopo la connessione a da Gestione driver, un driver può individuare la specifica dell'origine dati corrispondente nelle informazioni di sistema e utilizzare informazioni specifiche del driver dalla specifica per completare il set di informazioni di connessione necessarie.  
  
 Se nelle informazioni di sistema per l'origine dati è specificata una libreria di conversione predefinita, il driver si connette a tale libreria. Una libreria di conversione diversa può essere connessa chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_TRANSLATE_LIB. È possibile specificare un'opzione di conversione chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_TRANSLATE_OPTION.  
  
 Se un driver supporta **SQLConnect**, la sezione relativa alla parola chiave driver delle informazioni di sistema per il driver deve contenere la parola chiave **ConnectFunctions** con il primo carattere impostato su "Y".  
  
### <a name="connection-pooling"></a>Pool di connessioni  
 Il pool di connessioni consente a un'applicazione di riutilizzare una connessione già creata. Quando il pool di connessioni è abilitato e viene chiamato **SQLConnect** , gestione driver tenta di stabilire la connessione utilizzando una connessione che fa parte di un pool di connessioni in un ambiente che è stato designato per il pool di connessioni. Questo ambiente è un ambiente condiviso che viene usato da tutte le applicazioni che usano le connessioni nel pool.  
  
 Il pool di connessioni è abilitato prima dell'allocazione dell'ambiente chiamando **SQLSetEnvAttr** per impostare SQL_ATTR_CONNECTION_POOLING su SQL_CP_ONE_PER_DRIVER (che specifica un massimo di un pool per driver) o SQL_CP_ONE_PER_HENV (che specifica un massimo di un pool per ogni ambiente). **SQLSetEnvAttr** in questo caso viene chiamato con *EnvironmentHandle* impostato su null, che rende l'attributo un attributo a livello di processo. Se SQL_ATTR_CONNECTION_POOLING è impostato su SQL_CP_OFF, il pool di connessioni è disabilitato.  
  
 Dopo l'abilitazione del pool di connessioni, viene chiamato **SQLAllocHandle** con *HandleType* SQL_HANDLE_ENV per allocare un ambiente. L'ambiente allocato da questa chiamata è un ambiente condiviso perché il pool di connessioni è stato abilitato. Tuttavia, l'ambiente che verrà utilizzato non viene determinato fino a quando non viene chiamato **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_DBC.  
  
 Per allocare una connessione viene chiamato **SQLAllocHandle** con *HandleType* SQL_HANDLE_DBC. Gestione driver tenta di trovare un ambiente condiviso esistente corrispondente agli attributi dell'ambiente impostati dall'applicazione. Se non esiste alcun ambiente di questo tipo, ne viene creato uno come *ambiente condiviso* implicito. Se viene trovato un ambiente condiviso corrispondente, l'handle di ambiente viene restituito all'applicazione e il relativo conteggio dei riferimenti viene incrementato.  
  
 Tuttavia, la connessione che verrà utilizzata non viene determinata fino a quando non viene chiamato **SQLConnect** . A questo punto, gestione driver tenta di trovare una connessione esistente nel pool di connessioni che corrisponde ai criteri richiesti dall'applicazione. Questi criteri includono le opzioni di connessione richieste nella chiamata a **SQLConnect** (i valori delle parole chiave *ServerName*, *username* e *Authentication* ) ed eventuali attributi di connessione impostati da **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_DBC è stato chiamato. Gestione driver controlla questi criteri con le parole chiave e gli attributi di connessione corrispondenti nelle connessioni del pool. Se viene rilevata una corrispondenza, viene utilizzata la connessione nel pool. Se non viene trovata alcuna corrispondenza, viene creata una nuova connessione.  
  
 Se l'attributo SQL_ATTR_CP_MATCH Environment è impostato su SQL_CP_STRICT_MATCH, la corrispondenza deve essere esatta per una connessione nel pool da usare. Se l'attributo SQL_ATTR_CP_MATCH Environment è impostato su SQL_CP_RELAXED_MATCH, le opzioni di connessione nella chiamata a **SQLConnect** devono corrispondere ma non tutti gli attributi di connessione devono corrispondere.  
  
 Le regole seguenti vengono applicate quando viene chiamato un attributo di connessione, come impostato dall'applicazione prima che venga chiamato **SQLConnect** , non corrisponde all'attributo Connection della connessione nel pool:  
  
-   Se è necessario impostare l'attributo di connessione prima che venga stabilita la connessione:  
  
     Se SQL_ATTR_CP_MATCH è SQL_CP_STRICT_MATCH, SQL_ATTR_PACKET_SIZE nella connessione in pool deve essere identica all'attributo impostato dall'applicazione. Se SQL_CP_RELAXED_MATCH, i valori di SQL_ATTR_PACKET_SIZE possono essere diversi.  
  
     Il valore di SQL_ATTR_LOGIN_VALUE non influisce sulla corrispondenza.  
  
-   Se l'attributo di connessione può essere impostato prima o dopo la connessione:  
  
     Se l'attributo Connection non è stato impostato dall'applicazione ma è stato impostato sulla connessione nel pool ed è disponibile un valore predefinito, l'attributo Connection nella connessione in pool viene impostato di nuovo sul valore predefinito e viene dichiarata una corrispondenza. Se non è disponibile alcun valore predefinito, la connessione in pool non viene considerata una corrispondenza.  
  
     Se l'attributo Connection è stato impostato dall'applicazione ma non è stato impostato sulla connessione nel pool, l'attributo Connection nel pool viene modificato in impostato dall'applicazione e viene dichiarata una corrispondenza.  
  
     Se l'attributo Connection è stato impostato dall'applicazione e è stato impostato anche per la connessione nel pool, ma i valori sono diversi, viene usato il valore dell'attributo Connection dell'applicazione e viene dichiarata una corrispondenza.  
  
-   Se i valori degli attributi di connessione specifici del driver non sono identici e SQL_ATTR_CP_MATCH è impostato su SQL_CP_STRICT_MATCH, la connessione nel pool non viene utilizzata.  
  
 Quando l'applicazione chiama **Disconnect** per la disconnessione, la connessione viene restituita al pool di connessioni ed è disponibile per il riutilizzo.  
  
### <a name="optimizing-connection-pooling-performance"></a>Ottimizzazione delle prestazioni del pool di connessioni  
 Quando sono implicate transazioni distribuite, è possibile ottimizzare le prestazioni del pool di connessioni usando **SQL_DTC_TRANSITION_COST**, ovvero una maschera di SQLUINTEGER. Le transizioni a cui si fa riferimento sono le transizioni dell'attributo di connessione SQL_ATTR_ENLIST_IN_DTC passando dal valore 0 a diverso da zero e viceversa. Si tratta di una connessione che non viene integrata in una transazione distribuita da integrarsi in una transazione distribuita e viceversa. A seconda del modo in cui il driver ha implementato l'integrazione (impostazione dell'attributo di connessione SQL_ATTR_ENLIST_IN_DTC), queste transizioni possono essere costose e pertanto devono essere evitate per ottenere prestazioni ottimali.  
  
 Il valore restituito dal driver contiene qualsiasi combinazione dei bit seguenti:  
  
-   **SQL_DTC_ENLIST_EXPENSIVE**, se impostato, implica che la transizione da zero a zero è significativamente più costosa rispetto a una transizione da un valore diverso da zero a un altro valore diverso da zero (integrando una connessione integrata in precedenza nella transazione successiva).  
  
-   **SQL_DTC_UNENLIST_EXPENSIVE**, se impostato, implica che la transizione da zero a zero è significativamente più costosa rispetto all'uso di una connessione il cui attributo SQL_ATTR_ENLIST_IN_DTC è già impostato su zero.  
  
 Si verifica un compromesso tra prestazioni e utilizzo della connessione. Se un driver indica che una o più di queste transizioni sono costose, il pool di connessione di gestione driver risponde a questa operazione mantenendo più connessioni nel pool. Alcune delle connessioni nel pool sono preferite per l'uso non transazionale e alcune sono preferite per l'uso transazionale. Tuttavia, se il driver indica che queste transizioni non sono costose, è possibile utilizzare un minor numero di connessioni, probabilmente alternando l'utilizzo non transazionale e transazionale.  
  
 I driver che non supportano SQL_ATTR_ENLIST_IN_DTC non devono supportare SQL_DTC_TRANSITION_COST. Per i driver che supportano SQL_ATTR_ENLIST_IN_DTC ma non SQL_DTC_TRANSITION_COST, si presuppone che le transizioni non siano costose, come se il driver restituisse 0 (nessun bit impostato) per questo valore.  
  
 Sebbene SQL_DTC_TRANSITION_COST sia stato introdotto in ODBC 3,5, ODBC 2. il driver *x* può anche supportarlo perché Gestione driver eseguirà una query su queste informazioni indipendentemente dalla versione del driver.  
  
### <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente un'applicazione alloca gli handle di ambiente e di connessione. Si connette quindi all'origine dati SalesOrders con l'ID utente JohnS e la password Sesame ed elabora i dati. Al termine dell'elaborazione dei dati, si disconnette dall'origine dati e libera gli handle.  
  
```cpp  
// SQLConnect_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR * OutConnStr = (SQLCHAR * )malloc(255);  
   SQLSMALLINT * OutConnStrLen = (SQLSMALLINT *)malloc(255);  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLCHAR*) "NorthWind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
### <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Individuazione ed enumerazione dei valori necessari per la connessione a un'origine dati|[Funzione SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Disconnessione da un'origine dati|[Funzione SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Connessione a un'origine dati utilizzando una stringa di connessione o una finestra di dialogo|[Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Restituzione dell'impostazione di un attributo di connessione|[Funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Impostazione di un attributo di connessione|[Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
