---
description: Pagina relativa alla funzione SQLPrepare
title: Funzione SQLPrepare | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPrepare
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPrepare
helpviewer_keywords:
- SQLPrepare function [ODBC]
ms.assetid: 332e1b4b-b0ed-4e7a-aa4d-4f35f4f4476b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce0348f2161def7dabc4941f088b883b286d2969
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192600"
---
# <a name="sqlprepare-function"></a>Pagina relativa alla funzione SQLPrepare
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLPrepare** prepara una stringa SQL per l'esecuzione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLPrepare(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     StatementText,  
     SQLINTEGER    TextLength);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *StatementText*  
 Input Stringa di testo SQL.  
  
 *TextLength*  
 Input Lunghezza di **StatementText* in caratteri.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLPrepare** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLPrepare** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Un attributo di istruzione specificato non è valido a causa di condizioni di lavoro di implementazione, quindi un valore simile è stato sostituito temporaneamente. È possibile chiamare **SQLGetStmtAttr** per determinare il valore temporaneamente sostituito. Il valore sostitutivo è valido per *statementHandle* finché il cursore non viene chiuso. Gli attributi di istruzione che è possibile modificare sono: SQL_ATTR_CONCURRENCY SQL_ATTR_CURSOR_TYPE SQL_ATTR_KEYSET_SIZE SQL_ATTR_MAX_LENGTH SQL_ATTR_MAX_ROWS SQL_ATTR_QUERY_TIMEOUT SQL_ATTR_SIMULATE_CURSOR<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|21S01|L'elenco di valori di inserimento non corrisponde all'elenco di colonne|\**StatementText* conteneva un'istruzione **Insert** e il numero di valori da inserire non corrispondeva al grado della tabella derivata.|  
|21S02|Il grado della tabella derivata non corrisponde all'elenco di colonne|\**StatementText* conteneva un'istruzione **Create View** e il numero di nomi specificati non corrisponde alla tabella derivata definita dalla specifica della query.|  
|22018|Valore di carattere non valido per la specifica del cast|**StatementText* conteneva un'istruzione SQL che conteneva un valore letterale o un parametro e il valore era incompatibile con il tipo di dati della colonna della tabella associata.|  
|22019|Carattere di escape non valido|L'argomento *StatementText* contiene un predicato **like** con un **escape** nella clausola **where** e la lunghezza del carattere di escape che segue il carattere **escape** non è uguale a 1.|  
|22025|Sequenza di escape non valida|L'argomento *StatementText* conteneva "**come** _carattere_ di escape del _valore del criterio_ **di escape"** nella clausola **where** e il carattere che segue il carattere di escape nel valore del criterio non è né "%" né "_".|  
|24000|Stato del cursore non valido|(DM) un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato.<br /><br /> Un cursore è stato aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|34000|Nome di cursore non valido|\**StatementText* contiene un' **eliminazione** posizionata o un **aggiornamento** posizionato e il cursore a cui fa riferimento l'istruzione preparata non è aperto.|  
|3D000|Nome catalogo non valido|Il nome del catalogo specificato in *StatementText* non è valido.|  
|3F000|Nome schema non valido|Il nome dello schema specificato in *StatementText* non è valido.|  
|42000|Errore di sintassi o violazione di accesso|\**StatementText* conteneva un'istruzione SQL che non era preparabile o conteneva un errore di sintassi.<br /><br /> **StatementText* contiene un'istruzione per la quale l'utente non ha i privilegi necessari.|  
|42S01|La tabella o la vista di base esiste già|\**StatementText* conteneva un'istruzione **Create Table** o **Create View** e il nome della tabella o della vista specificato esiste già.|  
|42S02|La tabella o la vista di base non è stata trovata|\**StatementText* conteneva un'istruzione **Drop Table** o **Drop View** e il nome della tabella o della vista specificata non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **ALTER TABLE** e il nome della tabella specificato non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **Create View** e il nome di una tabella o di una vista definito dalla specifica della query non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **create index** e il nome della tabella specificato non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **Grant** o **Revoke** e il nome della tabella o della vista specificato non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **SELECT** e il nome della tabella o della vista specificato non esisteva.<br /><br /> \**StatementText* contiene un'istruzione **Delete**, **Insert** o **Update** e il nome della tabella specificato non esiste.<br /><br /> \**StatementText* conteneva un'istruzione **Create Table** e una tabella specificata in un vincolo (che fa riferimento a una tabella diversa da quella da creare) non esisteva.|  
|42S11|Indice già esistente|\**StatementText* conteneva un'istruzione **create index** e il nome di indice specificato esisteva già.|  
|42S12|Indice non trovato|\**StatementText* conteneva un'istruzione **Drop index** e il nome di indice specificato non esisteva.|  
|42S21|La colonna esiste già|\**StatementText* conteneva un'istruzione **ALTER TABLE** e la colonna specificata nella clausola **Add** non è univoca o identifica una colonna esistente nella tabella di base.|  
|42S22|Colonna non trovata|\**StatementText* conteneva un'istruzione **create index** e uno o più nomi di colonna specificati nell'elenco delle colonne non esistevano.<br /><br /> \**StatementText* conteneva un'istruzione **Grant** o **Revoke** e non esisteva un nome di colonna specificato.<br /><br /> \**StatementText* conteneva un'istruzione **Select**, **Delete**, **Insert** o **Update** e non esisteva un nome di colonna specificato.<br /><br /> \**StatementText* conteneva un'istruzione **Create Table** e una colonna specificata in un vincolo (che fa riferimento a una tabella diversa da quella da creare) non esisteva.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* e quindi la funzione è stata chiamata nuovamente su *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|(DM) *StatementText* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLPrepare** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) l'argomento *TextLength* era minore o uguale a 0 ma non uguale a SQL_NTS.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Impostazione della concorrenza non valida per il tipo di cursore definito.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 L'applicazione chiama **SQLPrepare** per inviare un'istruzione SQL all'origine dati per la preparazione. Per ulteriori informazioni sull'esecuzione preparata, vedere [esecuzione preparata](../../../odbc/reference/develop-app/prepared-execution-odbc.md). L'applicazione può includere uno o più indicatori di parametro nell'istruzione SQL. Per includere un marcatore di parametro, l'applicazione incorpora un punto interrogativo (?) nella stringa SQL nella posizione appropriata. Per informazioni sui parametri, vedere [parametri di istruzione](../../../odbc/reference/develop-app/statement-parameters.md).  
  
> [!NOTE]  
>  Se un'applicazione utilizza **SQLPrepare** per preparare e **SQLExecute** per inviare un'istruzione **commit** o **rollback** , non sarà interoperativa tra i prodotti DBMS. Per eseguire il commit o il rollback di una transazione, chiamare **SQLEndTran**.  
  
 Il driver può modificare l'istruzione in modo da utilizzare il formato di SQL utilizzato dall'origine dati e quindi inviarlo all'origine dati per la preparazione. In particolare, il driver modifica le sequenze di escape utilizzate per definire la sintassi SQL per alcune funzionalità. Per una descrizione della grammatica dell'istruzione SQL, vedere [sequenze di escape in ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md) e [Appendice C: grammatica SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md). Per il driver, un handle di istruzione è simile a un identificatore di istruzione nel codice SQL incorporato. Se l'origine dati supporta gli identificatori di istruzione, il driver può inviare all'origine dati un identificatore di istruzione e i valori dei parametri.  
  
 Una volta preparata un'istruzione, l'applicazione utilizza l'handle di istruzione per fare riferimento all'istruzione nelle chiamate di funzione successive. L'istruzione preparata associata all'handle di istruzione può essere eseguita di nuovo chiamando **SQLExecute** fino a quando l'applicazione non libera l'istruzione con una chiamata a **SQLFreeStmt** con l'opzione SQL_DROP o fino a quando l'handle di istruzione non viene utilizzato in una chiamata a **SQLPrepare**, **SQLExecDirect** o una delle funzioni di catalogo (**SQLColumns**, **SQLTables** e così via). Una volta preparata un'istruzione, l'applicazione può richiedere informazioni sul formato del set di risultati. Per alcune implementazioni, la chiamata a **SQLDescribeCol** o **SQLDescribeParam** dopo **SQLPrepare** potrebbe non essere efficiente quanto la chiamata della funzione dopo **SQLExecute** o **SQLExecDirect**.  
  
 Alcuni driver non possono restituire errori di sintassi o violazioni di accesso quando l'applicazione chiama **SQLPrepare**. Un driver può gestire errori di sintassi e violazioni di accesso, solo errori di sintassi o né errori di sintassi né violazioni di accesso. Pertanto, un'applicazione deve essere in grado di gestire queste condizioni quando si chiamano funzioni correlate successive, ad esempio **SQLNumResultCols**, **SQLDescribeCol**, **SQLColAttribute** e **SQLExecute**.  
  
 A seconda delle funzionalità del driver e dell'origine dati, è possibile controllare le informazioni sui parametri, ad esempio i tipi di dati, quando l'istruzione viene preparata (se tutti i parametri sono stati associati) o quando viene eseguita (se tutti i parametri non sono stati associati). Per garantire la massima interoperabilità, un'applicazione deve annullare l'associazione di tutti i parametri applicati a un'istruzione SQL precedente prima di preparare una nuova istruzione SQL nella stessa istruzione. In questo modo si evitano errori dovuti all'applicazione di informazioni sui parametri obsolete alla nuova istruzione.  
  
> [!IMPORTANT]  
>  Il commit di una transazione, chiamando in modo esplicito **SQLEndTran** o utilizzando la modalità autocommit, può causare l'eliminazione dei piani di accesso per tutte le istruzioni in una connessione da parte dell'origine dati. Per ulteriori informazioni, vedere i tipi di informazioni SQL_CURSOR_COMMIT_BEHAVIOR e SQL_CURSOR_ROLLBACK_BEHAVIOR in [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) e l' [effetto delle transazioni sui cursori e sulle istruzioni preparate](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)e [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle di istruzione|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Associazione di un buffer a un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Esecuzione di un'operazione di commit o rollback|[SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Restituzione del numero di righe interessate da un'istruzione|[SQLRowCount Function](../../../odbc/reference/syntax/sqlrowcount-function.md)|  
|Impostazione di un nome di cursore|[Funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
