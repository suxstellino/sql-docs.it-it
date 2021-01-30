---
description: Funzione SQLBrowseConnect
title: Funzione SQLBrowseConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBrowseConnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBrowseConnect
helpviewer_keywords:
- SQLBrowseConnect function [ODBC]
ms.assetid: b7f1be66-e6c7-4790-88ec-62b7662103c0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33983c9fb41625fa6479c47045ca4a04738a1463
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181077"
---
# <a name="sqlbrowseconnect-function"></a>Funzione SQLBrowseConnect
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLBrowseConnect** supporta un metodo iterativo per l'individuazione e l'enumerazione degli attributi e dei valori di attributo necessari per la connessione a un'origine dati. Ogni chiamata a **SQLBrowseConnect** restituisce i livelli successivi di attributi e valori di attributo. Quando tutti i livelli sono stati enumerati, viene completata una connessione all'origine dati e una stringa di connessione completa viene restituita da **SQLBrowseConnect**. Un codice restituito di SQL_SUCCESS o SQL_SUCCESS_WITH_INFO indica che sono state specificate tutte le informazioni di connessione e che l'applicazione è ora connessa all'origine dati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLBrowseConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *InConnectionString*  
 Input Sfoglia stringa di connessione richiesta (vedere "argomento *InConnectionString* " in "Commenti").  
  
 *StringLength1*  
 Input Lunghezza di **InConnectionString* in caratteri.  
  
 *OutConnectionString*  
 Output Puntatore a un buffer di caratteri in cui restituire la stringa di connessione del risultato della ricerca (vedere "argomento *OutConnectionString* " in "comments").  
  
 Se *OutConnectionString* è null, *StringLength2Ptr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *OutConnectionString*.  
  
 *BufferLength*  
 Input Lunghezza, in caratteri, del buffer **OutConnectionString* .  
  
 *StringLength2Ptr*  
 Output Numero totale di caratteri (esclusa la terminazione null) disponibili per restituire in \* *OutConnectionString*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *bufferLength*, la stringa di connessione in \* *OutConnectionString* viene troncata a *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLBrowseConnect** restituisce SQL_ERROR, SQL_SUCCESS_WITH_INFO o SQL_NEED_DATA, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle di connectionHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLBrowseConnect** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|L' \* *OutConnectionString* del buffer non era sufficientemente grande da restituire l'intera stringa di connessione del risultato browse, quindi la stringa è stata troncata. Il buffer **StringLength2Ptr* contiene la lunghezza della stringa di connessione per i risultati di ricerca non troncata. (La funzione restituisce SQL_NEED_DATA.)|  
|01S00|Attributo della stringa di connessione non valido|È stata specificata una parola chiave di attributo non valida nella stringa di connessione della richiesta Browse (*InConnectionString*). (La funzione restituisce SQL_NEED_DATA.)<br /><br /> È stata specificata una parola chiave attribute nella stringa di connessione della richiesta Browse (*InConnectionString*) che non si applica al livello di connessione corrente. (La funzione restituisce SQL_NEED_DATA.)|  
|01S02|Valore modificato|Il driver non supporta il valore specificato dell'argomento *ValuePtr* in **SQLSetConnectAttr** e sostituisce un valore simile. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08001|Il client non è in grado di stabilire la connessione|Il driver non è stato in grado di stabilire una connessione con l'origine dati.|  
|08002|Nome della connessione in uso|(DM) la connessione specificata è già stata utilizzata per stabilire una connessione con un'origine dati e la connessione è stata aperta.|  
|08004|Il server ha rifiutato la connessione|L'origine dati ha rifiutato la creazione della connessione per motivi definiti dall'implementazione.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato effettuato il tentativo di connessione del driver non è riuscito prima dell'elaborazione della funzione.|  
|28000|Specifica di autorizzazione non valida|L'identificatore utente o la stringa di autorizzazione o entrambi, come specificato nella stringa di connessione della richiesta Browse (*InConnectionString*), hanno violato le restrizioni definite dall'origine dati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|(DM) Gestione driver non è in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.<br /><br /> Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|Un'operazione asincrona è stata annullata chiamando la [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md). Quindi, la funzione originale è stata chiamata nuovamente in *connectionHandle*.<br /><br /> Un'operazione è stata annullata chiamando **SQLCancelHandle** su *connectionHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *StringLength1* è minore di 0 e non è uguale a SQL_NTS.<br /><br /> (DM) il valore specificato per l'argomento *bufferLength* è minore di 0.|  
|HY114|Il driver non supporta l'esecuzione di funzioni asincrone a livello di connessione|(DM) l'applicazione ha abilitato l'operazione asincrona sull'handle di connessione prima di effettuare la connessione. Tuttavia, il driver non supporta l'operazione asincrona sull'handle di connessione.|  
|HYT00|Timeout|Il periodo di timeout di accesso è scaduto prima del completamento della connessione all'origine dati. Il periodo di timeout viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver corrispondente al nome dell'origine dati specificato non supporta la funzione.|  
|IM002|Origine dati non trovata e non è stato specificato alcun driver predefinito|(DM) il nome dell'origine dati specificato nella stringa di connessione della richiesta Browse (*InConnectionString*) non è stato trovato nelle informazioni di sistema, né esiste una specifica del driver predefinita.<br /><br /> (DM) non è stato possibile trovare le informazioni relative all'origine dati ODBC e al driver predefinito nelle informazioni di sistema.|  
|IM003|Non è stato possibile caricare il driver specificato|(DM) il driver elencato nella specifica dell'origine dati nelle informazioni di sistema o specificato dalla parola chiave del **driver** non è stato trovato o non è stato possibile caricarlo per altri motivi.|  
|IM004|**SQLAllocHandle** del Driver SQL_HANDLE _ENV non riuscita|(DM) durante **SQLBrowseConnect**, gestione driver ha chiamato la funzione **SQLAllocHandle** del driver con una *HandleType* di SQL_HANDLE_ENV e il driver ha restituito un errore.|  
|IM005|**SQLAllocHandle** del Driver su SQL_HANDLE_DBC non riuscito|(DM) durante **SQLBrowseConnect**, gestione driver ha chiamato la funzione **SQLAllocHandle** del driver con una *HandleType* di SQL_HANDLE_DBC e il driver ha restituito un errore.|  
|IM006|Errore di **SQLSetConnectAttr** del driver|(DM) durante **SQLBrowseConnect**, gestione driver ha chiamato la funzione **SQLSetConnectAttr** del driver e il driver ha restituito un errore.|  
|IM009|Impossibile caricare la DLL di traduzione|Il driver non è stato in grado di caricare la DLL di conversione specificata per l'origine dati o per la connessione.|  
|IM010|Nome origine dati troppo lungo|(DM) il valore dell'attributo per la parola chiave DSN è più lungo di SQL_MAX_DSN_LENGTH caratteri.|  
|IM011|Nome driver troppo lungo|(DM) il valore dell'attributo per la parola chiave DRIVER è più lungo di 255 caratteri.|  
|IM012|Errore di sintassi della parola chiave DRIVER|(DM) la coppia parola chiave-valore per la parola chiave DRIVER contiene un errore di sintassi.|  
|IM014|Il DSN specificato contiene una mancata corrispondenza dell'architettura tra il driver e l'applicazione|(DM) l'applicazione a 32 bit utilizza un DSN che si connette a un driver a 64 bit. o viceversa.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
|S1118|Il driver non supporta la notifica asincrona|Quando il driver non supporta la notifica asincrona, non è possibile impostare SQL_ATTR_ASYNC_DBC_EVENT o SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="inconnectionstring-argument"></a>Argomento InConnectionString  
 Una stringa di connessione per la richiesta browse presenta la sintassi seguente:  
  
 *Connection-String* :: = *attribute*[ `;` ] &#124;  `;` *stringa di connessione* dell'attributo;<br>
 *attribute::* = Attribute *-keyword*- `=` *valore* &#124; `DRIVER=` [ `{` ]*attributo-valore*[ `}` ]<br>
 *attribute-keyword* :: = `DSN` &#124; `UID` &#124; `PWD` &#124; il *driver-defined-attribute-keyword*<br>
 *attribute-value* :: = *stringa di caratteri*<br>
 *driver-defined-attribute-keyword* :: = *Identifier*<br>
  
 dove la *stringa di caratteri* contiene zero o più caratteri; l' *identificatore* contiene uno o più caratteri; *attribute-keyword* non distingue tra maiuscole e minuscole. *attribute-value* può fare distinzione tra maiuscole e minuscole; il valore della parola chiave **DSN** , quindi, non è costituito solo da spazi vuoti. A causa della sintassi della stringa di connessione e del file di inizializzazione, parole chiave e valori di attributo contenenti i caratteri **[] {} (),;? \* =! @** deve essere evitato. A causa della grammatica nelle informazioni di sistema, le parole chiave e i nomi delle origini dati non possono contenere il carattere barra rovesciata ( \\ ). Per ODBC 2. driver *x* , le parentesi graffe sono necessarie intorno al valore dell'attributo per la parola chiave del driver.  
  
 Se vengono ripetute parole chiave nella stringa di connessione della richiesta browse, il driver utilizzerà il valore associato alla prima occorrenza della parola chiave. Se le parole chiave **DSN** e **driver** sono incluse nella stessa stringa di connessione della richiesta browse, il driver e la gestione driver utilizzeranno prima di tutto la parola chiave.  
  
 Per informazioni sul modo in cui un'applicazione sceglie un'origine dati o un driver, vedere [scelta di un'origine dati o](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)di un driver.  
  
## <a name="outconnectionstring-argument"></a>Argomento OutConnectionString  
 La stringa di connessione per il risultato della ricerca è un elenco di attributi di connessione. Un attributo Connection è costituito da una parola chiave Attribute e da un valore di attributo corrispondente. La stringa di connessione del risultato browse presenta la sintassi seguente:  
  
 *Connection-String* :: = *attribute*[ `;` ] &#124;  `;` *stringa di connessione* dell'attributo<br>
 *attribute::* = [ `*` ] attributo *-parola chiave* `=` *-valore*<br>
 *attribute-keyword* :: = *ODBC-attribute-keyword* &#124; *driver-defined-attribute-keyword*<br>
 *ODBC-attribute-keyword* = { `UID` &#124; `PWD` } [ `:` *identificatore localizzato*] *driver-defined-attribute-keyword* :: = *Identifier*[ `:` *localizzated-Identifier*] *attribute-value* :: = `{` *Attribute-Value-List* `}` &#124; `?` (le parentesi graffe sono valore letterale, vengono restituite dal driver).<br>
 *Attribute-Value-List* :: = *character-string* [ `:` *stringa di caratteri localizzati*] &#124; *stringa di* caratteri [ `:` *localizzato-* stringa di caratteri] `,` *attributo-valore-elenco*<br>
  
 dove la *stringa di caratteri* e la *stringa di caratteri localizzati* hanno zero o più caratteri; l' *identificatore* e l' *identificatore localizzato* contengono uno o più caratteri; *attribute-keyword* non distingue tra maiuscole e minuscole. e *attribute-value* può fare distinzione tra maiuscole e minuscole. A causa della sintassi della stringa di connessione e del file di inizializzazione, parole chiave, identificatori localizzati e valori di attributo contenenti i caratteri **[] {} (),;? \* =! @** deve essere evitato. A causa della grammatica nelle informazioni di sistema, le parole chiave e i nomi delle origini dati non possono contenere il carattere barra rovesciata ( \\ ).  
  
 La sintassi della stringa di connessione result Browse viene utilizzata in base alle regole semantiche seguenti:  
  
-   Se un asterisco ( \* ) precede una *parola chiave attribute*, l' *attributo* è facoltativo e può essere omesso nella chiamata successiva a **SQLBrowseConnect**.  
  
-   Le parole chiave degli attributi **UID** e **pwd** hanno lo stesso significato definito in **SQLDriverConnect**.  
  
-   Una *parola chiave attribute-defined definisce* il tipo di attributo per il quale è possibile fornire un valore di attributo. Ad esempio, potrebbe essere **Server**, **database**, **host** o **DBMS**.  
  
-   *ODBC-attribute-Keywords* e *driver-defined-attribute-* Keywords includono una versione localizzata o intuitiva della parola chiave. Questa operazione può essere utilizzata dalle applicazioni come etichetta in una finestra di dialogo. Tuttavia, quando si passa una stringa della richiesta browse al driver, è necessario usare **UID**, **pwd** o l' *identificatore* da solo.  
  
-   {*Attribute-Value-List*} è un'enumerazione di valori effettivi validi per la *parola chiave attribute* corrispondente. Si noti che le parentesi graffe ( {} ) non indicano un elenco di opzioni, ma vengono restituite dal driver. Ad esempio, potrebbe trattarsi di un elenco di nomi di server o di un elenco di nomi di database.  
  
-   Se il *valore dell'attributo* è un punto interrogativo singolo (?), un singolo valore corrisponde alla *parola chiave attribute*. Ad esempio, UID = JohnS; PWD = sesamo.  
  
-   Ogni chiamata a **SQLBrowseConnect** restituisce solo le informazioni necessarie per soddisfare il livello successivo del processo di connessione. Il driver associa le informazioni di stato con l'handle di connessione in modo che il contesto possa essere sempre determinato a ogni chiamata.  
  
## <a name="using-sqlbrowseconnect"></a>Uso di SQLBrowseConnect  
 **SQLBrowseConnect** richiede una connessione allocata. Gestione driver carica il driver specificato in o che corrisponde al nome dell'origine dati specificato nella stringa di connessione per la richiesta di visualizzazione iniziale. per informazioni su quando si verifica questa situazione, vedere la sezione "Commenti" nella [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Il driver può stabilire una connessione con l'origine dati durante il processo di esplorazione. Se **SQLBrowseConnect** restituisce SQL_ERROR, le connessioni in attesa vengono interrotte e la connessione viene restituita a uno stato non connesso.  
  
> [!NOTE]  
>  **SQLBrowseConnect** non supporta il pool di connessioni. Se viene chiamato **SQLBrowseConnect** mentre il pool di connessioni è abilitato, viene restituito SQLSTATE HY000 (errore generale).  
  
 Quando **SQLBrowseConnect** viene chiamato per la prima volta in una connessione, la stringa di connessione della richiesta browse deve contenere la parola chiave **DSN** o la parola chiave **driver** . Se la stringa di connessione della richiesta browse contiene la parola chiave **DSN** , gestione driver individua una specifica dell'origine dati corrispondente nelle informazioni di sistema:  
  
-   Se Gestione driver trova la specifica dell'origine dati corrispondente, carica la DLL del driver associata; il driver può recuperare le informazioni sull'origine dati dalle informazioni di sistema.  
  
-   Se Gestione driver non è in grado di trovare la specifica dell'origine dati corrispondente, individua la specifica dell'origine dati predefinita e carica la DLL del driver associato. il driver può recuperare le informazioni sull'origine dati predefinita dalle informazioni di sistema. "DEFAULT" viene passato al driver per il DSN.  
  
-   Se Gestione driver non è in grado di trovare la specifica dell'origine dati corrispondente e non esiste alcuna specifica dell'origine dati predefinita, restituisce SQL_ERROR con SQLSTATE IM002 (origine dati non trovata e nessun driver predefinito specificato).  
  
 Se la stringa di connessione della richiesta browse contiene la parola chiave **driver** , il driver specificato viene caricato da Gestione driver. non tenta di individuare un'origine dati nelle informazioni di sistema. Poiché la parola chiave **driver** non utilizza informazioni delle informazioni di sistema, il driver deve definire parole chiave sufficienti in modo che un driver possa connettersi a un'origine dati utilizzando solo le informazioni contenute nelle stringhe di connessione per la richiesta di ricerca.  
  
 Per ogni chiamata a **SQLBrowseConnect**, l'applicazione specifica i valori degli attributi di connessione nella stringa di connessione della richiesta browse. Il driver restituisce i livelli successivi di attributi e valori di attributo nella stringa di connessione del risultato browse. restituisce SQL_NEED_DATA finché sono presenti attributi di connessione non ancora enumerati nella stringa di connessione della richiesta browse. L'applicazione usa il contenuto della stringa di connessione result Browse per compilare la stringa di connessione per la richiesta Browse per la chiamata successiva a **SQLBrowseConnect**. Tutti gli attributi obbligatori (quelli non preceduti da un asterisco nell'argomento *OutConnectionString* ) devono essere inclusi nella chiamata successiva a **SQLBrowseConnect**. Si noti che l'applicazione non può usare il contenuto delle stringhe di connessione del risultato della ricerca precedente durante la compilazione della stringa di connessione della richiesta di visualizzazione corrente. ovvero non può specificare valori diversi per gli attributi impostati nei livelli precedenti.  
  
 Quando tutti i livelli di connessione e gli attributi associati sono stati enumerati, il driver restituisce SQL_SUCCESS, la connessione all'origine dati è completata e viene restituita una stringa di connessione completa all'applicazione. La stringa di connessione è adatta per l'utilizzo di, insieme a **SQLDriverConnect**, con l'opzione SQL_DRIVER_NOPROMPT per stabilire un'altra connessione. Tuttavia, non è possibile usare la stringa di connessione completa in un'altra chiamata a **SQLBrowseConnect**. Se **SQLBrowseConnect** è stato chiamato nuovamente, è necessario ripetere l'intera sequenza di chiamate.  
  
 **SQLBrowseConnect** restituisce inoltre SQL_NEED_DATA se sono presenti errori reversibili e non irreversibili durante il processo di visualizzazione. ad esempio, una password non valida o una parola chiave dell'attributo fornita dall'applicazione. Quando viene restituito SQL_NEED_DATA e la stringa di connessione del risultato della ricerca non viene modificata, si è verificato un errore e l'applicazione può chiamare **SQLGetDiagRec** per restituire il valore SQLSTATE per gli errori in fase di visualizzazione. Ciò consente all'applicazione di correggere l'attributo e continuare l'esplorazione.  
  
 Un'applicazione può terminare il processo di esplorazione in qualsiasi momento chiamando **Disconnect**. Il driver terminerà tutte le connessioni in attesa e restituirà la connessione a uno stato non connesso.  
  
 Se le operazioni asincrone sono abilitate nell'handle di connessione, **SQLBrowseConnect** potrebbe restituire anche SQL_STILL_EXECUTING. Quando restituisce SQL_NEED_DATA, un'applicazione deve usare **SQLConnect** per annullare il processo di esplorazione. Se **SQLBrowseConnect** restituisce SQL_STILL_EXECUTING, un'applicazione deve usare **SQLCancelHandle** per annullare l'operazione in corso. La chiamata di **SQLCancelHandle** dopo che la funzione restituisce SQL_NEED_DATA non ha alcun effetto.  
  
 Per ulteriori informazioni, vedere [connessione con SQLBrowseConnect](../../../odbc/reference/develop-app/connecting-with-sqlbrowseconnect.md).  
  
 Se un driver supporta **SQLBrowseConnect**, la sezione relativa alla parola chiave driver nelle informazioni di sistema per il driver deve contenere la parola chiave **ConnectFunctions** con il terzo carattere impostato su "Y".  
  
## <a name="code-example"></a>Esempio di codice  
  
> [!NOTE]  
>  Se ci si connette a un provider dell'origine dati che supporta l'autenticazione di Windows, è necessario specificare al `Trusted_Connection=yes` posto delle informazioni sull'ID utente e sulla password nella stringa di connessione.  
  
 Nell'esempio seguente un'applicazione chiama **SQLBrowseConnect** ripetutamente. Ogni volta che **SQLBrowseConnect** restituisce SQL_NEED_DATA, vengono restituite informazioni sui dati necessari in \* *OutConnectionString*. L'applicazione passa la proprietà *OutConnectionString* alla routine **GetUserInput** (non mostrata). **GetUserInput** analizza le informazioni, compila e visualizza una finestra di dialogo e restituisce le informazioni immesse dall'utente in \* *InConnectionString*. L'applicazione passa le informazioni dell'utente al driver nella chiamata successiva a **SQLBrowseConnect**. Dopo che l'applicazione ha fornito tutte le informazioni necessarie per il driver per la connessione all'origine dati, **SQLBrowseConnect** restituisce SQL_SUCCESS e l'applicazione continua.  
  
 Per un esempio più dettagliato di connessione a un driver di SQL Server chiamando **SQLBrowseConnect**, vedere [SQL Server esempio di esplorazione](../../../odbc/reference/develop-app/sql-server-browsing-example.md).  
  
 Per connettersi all'origine dati Sales, ad esempio, è possibile che si verifichino le azioni seguenti. In primo luogo, l'applicazione passa la stringa seguente a **SQLBrowseConnect**:  
  
```  
"DSN=Sales"  
```  
  
 Gestione driver carica il driver associato alle vendite dell'origine dati. Chiama quindi la funzione **SQLBrowseConnect** del driver con gli stessi argomenti ricevuti dall'applicazione. Il driver restituisce la stringa seguente in **OutConnectionString*:  
  
```  
"HOST:Server={red,blue,green};UID:ID=?;PWD:Password=?"  
```  
  
 L'applicazione passa questa stringa alla routine **GetUserInput** , che compila una finestra di dialogo in cui viene chiesto all'utente di selezionare il server rosso, blu o verde e di immettere un ID utente e una password. La routine passa nuovamente le seguenti informazioni specificate dall'utente in \* *InConnectionString*, che l'applicazione passa a **SQLBrowseConnect**:  
  
```  
"HOST=red;UID=Smith;PWD=Sesame"  
```  
  
 **SQLBrowseConnect** usa queste informazioni per connettersi al server Red come Smith con la password sesamo, quindi restituisce la stringa seguente in **OutConnectionString*:  
  
```  
"*DATABASE:Database={SalesEmployees,SalesGoals,SalesOrders}"  
```  
  
 L'applicazione passa questa stringa alla routine **GetUserInput** , che compila una finestra di dialogo in cui viene chiesto all'utente di selezionare un database. L'utente seleziona empdata e l'applicazione chiama **SQLBrowseConnect** all'ora finale con la stringa seguente:  
  
```  
"DATABASE=SalesOrders"  
```  
  
 Questo è l'ultimo elemento di informazioni che il driver deve connettere all'origine dati. **SQLBrowseConnect** restituisce SQL_SUCCESS e **OutConnectionString* contiene la stringa di connessione completata:  
  
```cpp  
// SQLBrowseConnect_Function.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define BRWS_LEN 100  
SQLHENV henv;  
SQLHDBC hdbc;  
SQLHSTMT hstmt;  
SQLRETURN retcode;  
SQLCHAR szConnStrIn[BRWS_LEN], szConnStrOut[BRWS_LEN];  
SQLSMALLINT cbConnStrOut;  
  
void GetUserInput(SQLCHAR * szConnStrOut, SQLCHAR * szConnStrIn) {}  
  
int main() {  
   // Allocate the environment handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);        
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
      // Set the version environment attribute.  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
         // Allocate the connection handle.  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            // Call SQLBrowseConnect until it returns a value other than SQL_NEED_DATA   
            // (pass data source name the first time).  If SQL_NEED_DATA is returned, call GetUserInput   
            // (not shown) to build a dialog from the values in szConnStrOut.  The user-supplied values   
            // are returned in szConnStrIn, which is passed in the next call to SQLBrowseConnect.  
  
            strcpy_s((char*)szConnStrIn, _countof(szConnStrIn), "DSN=Sales");  
            do {  
               retcode = SQLBrowseConnect(hdbc, szConnStrIn, SQL_NTS,  
                  szConnStrOut, BRWS_LEN, &cbConnStrOut);  
               if (retcode == SQL_NEED_DATA)  
                  GetUserInput(szConnStrOut, szConnStrIn);  
            } while (retcode == SQL_NEED_DATA);  
  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
               // Allocate the statement handle.  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                  // Process data after successful connection  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               SQLDisconnect(hdbc);  
            }  
         }  
         SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      }  
   }  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle di connessione|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Connessione a un'origine dati|[Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Disconnessione da un'origine dati|[Funzione SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Connessione a un'origine dati utilizzando una stringa di connessione o una finestra di dialogo|[Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Restituzione di attributi e descrizioni dei driver|[Funzione SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|Liberazione di un handle di connessione|[SQLFreeHandle Function](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
