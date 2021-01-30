---
description: Funzione SQLExecDirect
title: Funzione SQLExecDirect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExecDirect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLExecDirect
helpviewer_keywords:
- SQLExecDirect function [ODBC]
ms.assetid: 985fcee1-f204-425c-bdd1-deb0e7d7bbd9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 938d2aa1631041e9dbb7a51ea952ff7d60811fb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179889"
---
# <a name="sqlexecdirect-function"></a>Funzione SQLExecDirect
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLExecDirect** esegue un'istruzione preparabile, usando i valori correnti delle variabili del marcatore di parametro se nell'istruzione sono presenti parametri. **SQLExecDirect** è il modo più rapido per inviare un'istruzione SQL per l'esecuzione una sola volta.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLExecDirect(  
     SQLHSTMT     StatementHandle,  
     SQLCHAR *    StatementText,  
     SQLINTEGER   TextLength);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *StatementText*  
 Input Istruzione SQL da eseguire.  
  
 *TextLength*  
 Input Lunghezza di **StatementText* in caratteri.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_NO_DATA, SQL_INVALID_HANDLE o SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLExecDirect** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti comunemente da **SQLExecDirect** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01001|Conflitto operazione cursore|\**StatementText* conteneva un'istruzione Update o DELETE posizionata e non sono state aggiornate o eliminate righe o più righe. Per ulteriori informazioni sugli aggiornamenti a più di una riga, vedere la descrizione dell' *attributo* SQL_ATTR_SIMULATE_CURSOR in **SQLSetStmtAttr**.)<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01003|Valore NULL eliminato nella funzione set|L'argomento *StatementText* contiene una funzione set, ad esempio **AVG**, **Max**, **min** e così via, ma non la funzione **count** set e i valori di argomento null sono stati eliminati prima dell'applicazione della funzione. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati stringa o binari restituiti per un parametro di input/output o di output hanno causato il troncamento del carattere non vuoto o dei dati binari non NULL. Se si trattasse di un valore stringa, viene troncato a destra. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01006|Privilegio non revocato|\**StatementText* conteneva un'istruzione **Revoke** e l'utente non disponeva del privilegio specificato. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01007|Privilegio non concesso|*\* StatementText* è un'istruzione **Grant** e all'utente non è stato concesso il privilegio specificato.|  
|01S02|Valore di opzione modificato|Un attributo di istruzione specificato non è valido a causa di condizioni di lavoro di implementazione, quindi un valore simile è stato sostituito temporaneamente. È possibile chiamare **SQLGetStmtAttr** per determinare il valore temporaneamente sostituito. Il valore sostitutivo è valido per *statementHandle* fino a quando il cursore non viene chiuso, a quel punto l'attributo dell'istruzione ripristina il valore precedente. Gli attributi di istruzione che è possibile modificare sono:<br /><br /> SQL_ ATTR_CONCURRENCY SQL_ ATTR_CURSOR_TYPE SQL_ ATTR_KEYSET_SIZE SQL_ ATTR_MAX_LENGTH SQL_ ATTR_MAX_ROWS SQL_ ATTR_QUERY_TIMEOUT SQL_ ATTR_SIMULATE_CURSOR<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S07|Troncamento frazionario|I dati restituiti per un parametro di input/output o di output sono stati troncati in modo che la parte frazionaria di un tipo di dati numerico sia stata troncata o che la parte frazionaria del componente ora di un tipo di dati time, timestamp o Interval sia stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07002|Campo COUNT non corretto|Il numero di parametri specificati in **SQLBindParameter** è inferiore al numero di parametri nell'istruzione SQL contenuta in \* *StatementText*.<br /><br /> **SQLBindParameter** è stato chiamato con *ParameterValuePtr* impostato su un puntatore null, *StrLen_or_IndPtr* non è impostato su SQL_NULL_DATA o SQL_DATA_AT_EXEC e *InputOutputType* non è impostato su SQL_PARAM_OUTPUT, in modo che il numero di parametri specificati in **SQLBINDPARAMETER** sia maggiore del numero di parametri nell'istruzione SQL contenuta in **StatementText*.|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Non è stato possibile convertire il valore di dati identificato dall'argomento *ValueType* in **SQLBindParameter** per il parametro associato nel tipo di dati identificato dall'argomento *ParameterType* in **SQLBindParameter**.<br /><br /> Il valore di dati restituito per un parametro associato come SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_OUTPUT non può essere convertito nel tipo di dati identificato dall'argomento *ValueType* in **SQLBindParameter**.<br /><br /> Se non è stato possibile convertire i valori dei dati per una o più righe, ma una o più righe sono state restituite correttamente, questa funzione restituisce SQL_SUCCESS_WITH_INFO.|  
|07007|Violazione del valore del parametro limitato|Il tipo di parametro SQL_PARAM_INPUT_OUTPUT_STREAM viene utilizzato solo per un parametro che invia e riceve dati in parti. Un buffer con binding di input non è consentito per questo tipo di parametro.<br /><br /> Questo errore si verifica quando il tipo di parametro è SQL_PARAM_INPUT_OUTPUT e quando il \* *StrLen_or_IndPtr* specificato in **SQLBindParameter** non è uguale a SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_LEN_DATA_AT_EXEC (LEN) o SQL_DATA_AT_EXEC.|  
|07S01|Uso non valido del parametro predefinito|Il valore di un parametro, impostato con **SQLBindParameter**, è stato SQL_DEFAULT_PARAM e il parametro corrispondente non ha un valore predefinito.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|21S01|L'elenco di valori di inserimento non corrisponde all'elenco di colonne|\**StatementText* conteneva un'istruzione **Insert** e il numero di valori da inserire non corrispondeva al grado della tabella derivata.|  
|21S02|Il grado della tabella derivata non corrisponde all'elenco di colonne|\**StatementText* conteneva un'istruzione **Create View** e l'elenco di colonne non qualificato (il numero di colonne specificato per la vista negli argomenti *identificatore di colonna* dell'istruzione SQL) conteneva più nomi rispetto al numero di colonne nella tabella derivata definita dall'argomento *query-Specification* dell'istruzione SQL.|  
|22001|Dati stringa, troncamento a destra|L'assegnazione di un valore di tipo carattere o binario a una colonna ha determinato il troncamento di dati di tipo carattere non vuoti o di dati binari non null.|  
|22002|Variabile indicatore obbligatoria ma non fornita|I dati NULL sono stati associati a un parametro di output il cui *StrLen_or_IndPtr* impostato da **SQLBindParameter** è un puntatore null.|  
|22003|Valore numerico non compreso nell'intervallo|**StatementText* conteneva un'istruzione SQL che conteneva un parametro numerico associato o un valore letterale e il valore ha causato il troncamento della parte intera (anziché frazionaria) del numero quando è stata assegnata alla colonna della tabella associata.<br /><br /> La restituzione di un valore numerico (come valore numerico o stringa) per uno o più parametri di input/output o di output avrebbe causato il troncamento dell'intero, anziché della parte frazionaria del numero.|  
|22007|Formato DateTime non valido|**StatementText* conteneva un'istruzione SQL che conteneva una struttura di data, ora o timestamp come parametro associato e il parametro era, rispettivamente, una data, un'ora o un timestamp non valido.<br /><br /> Un parametro di input/output o di output è stato associato a una struttura di data, ora o timestamp C e un valore nel parametro restituito era, rispettivamente, una data, un'ora o un timestamp non valido. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|22008|Overflow del campo DateTime|**StatementText* conteneva un'istruzione SQL che conteneva un'espressione DateTime che, quando calcolata, generava una struttura di data, ora o timestamp non valida.<br /><br /> Un'espressione datetime calcolata per un parametro di input/output o di output ha restituito una struttura di data, ora o timestamp non valida.|  
|22012|Divisione per zero|**StatementText* conteneva un'istruzione SQL che conteneva un'espressione aritmetica che causava la divisione per zero.<br /><br /> Un'espressione aritmetica calcolata per un parametro di input/output o di output ha restituito una divisione per zero.|  
|22015|Overflow del campo Interval|*\* StatementText* contiene un parametro numerico o di intervallo esatto che, quando viene convertito in un tipo di dati SQL intervallo, ha causato una perdita di cifre significative.<br /><br /> *\* StatementText* conteneva un parametro Interval con più di un campo che, se convertito in un tipo di dati numerico in una colonna, non aveva alcuna rappresentazione nel tipo di dati numeric.<br /><br /> *\* StatementText* conteneva i dati dei parametri assegnati a un tipo SQL intervallo e non era presente alcuna rappresentazione del valore del tipo C nel tipo di intervallo SQL.<br /><br /> L'assegnazione di un parametro di input/output o di output che era un tipo SQL esatto o di intervallo a un tipo intervallo C ha causato una perdita di cifre significative.<br /><br /> Quando un parametro di input/output o di output è stato assegnato a una struttura di intervallo C, non esisteva alcuna rappresentazione dei dati nella struttura dei dati dell'intervallo.|  
|22018|Valore di carattere non valido per la specifica del cast|*\* StatementText* conteneva un tipo C che era un tipo di dati numerico esatto o approssimativo, DateTime o Interval. il tipo SQL della colonna era un tipo di dati character e il valore nella colonna non era un valore letterale valido del tipo c associato.<br /><br /> Quando viene restituito un parametro di input/output o output, il tipo SQL è un valore numerico esatto o approssimato, un valore DateTime o un tipo di dati interval; il tipo C è stato SQL_C_CHAR; e il valore nella colonna non è un valore letterale valido del tipo SQL associato.|  
|22019|Carattere di escape non valido|\**StatementText* conteneva un'istruzione SQL che conteneva un predicato **like** con **escape** nella clausola **where** e la lunghezza del carattere di escape che segue il carattere **di escape non** è uguale a 1.|  
|22025|Sequenza di escape non valida|\**StatementText* conteneva un'istruzione SQL che conteneva "**come** _carattere_ **di escape per** il valore del _criterio_ " nella clausola **where** e il carattere che segue il carattere di escape nel valore del criterio non era uno "%" o "_".|  
|23000|Violazione del vincolo di integrità|**StatementText* conteneva un'istruzione SQL che conteneva un parametro o un valore letterale. Il valore del parametro è NULL per una colonna definita come NOT NULL nella colonna della tabella associata, è stato specificato un valore duplicato per una colonna vincolata a contenere solo valori univoci oppure è stato violato un altro vincolo di integrità.|  
|24000|Stato del cursore non valido|Un cursore è stato posizionato in *statementHandle* da **SQLFetch** o **SQLFetchScroll**. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Cursore aperto ma non posizionato in corrispondenza di *statementHandle*.<br /><br /> **StatementText* contiene un'istruzione Update o DELETE posizionata e il cursore è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati.|  
|34000|Nome di cursore non valido|**StatementText* conteneva un'istruzione Update o DELETE posizionata e il cursore a cui viene fatto riferimento dall'istruzione eseguita non è stato aperto.|  
|3D000|Nome catalogo non valido|Il nome del catalogo specificato in *StatementText* non è valido.|  
|3F000|Nome schema non valido|Il nome dello schema specificato in *StatementText* non è valido.|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|42000|Errore di sintassi o violazione di accesso|\**StatementText* conteneva un'istruzione SQL che non era preparabile o conteneva un errore di sintassi.<br /><br /> L'utente non dispone delle autorizzazioni necessarie per eseguire l'istruzione SQL contenuta in **StatementText*.|  
|42S01|La tabella o la vista di base esiste già|\**StatementText* conteneva un'istruzione **Create Table** o **Create View** e il nome della tabella o della vista specificato esiste già.|  
|42S02|La tabella o la vista di base non è stata trovata|\**StatementText* conteneva un'istruzione **Drop Table** o **Drop View** e il nome della tabella o della vista specificata non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **ALTER TABLE** e il nome della tabella specificato non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **Create View** e il nome di una tabella o di una vista definito dalla specifica della query non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **create index** e il nome della tabella specificato non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **Grant** o **Revoke** e il nome della tabella o della vista specificato non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **SELECT** e il nome della tabella o della vista specificato non esisteva.<br /><br /> \**StatementText* contiene un'istruzione **Delete**, **Insert** o **Update** e il nome della tabella specificato non esiste.<br /><br /> \**StatementText* conteneva un'istruzione **Create Table** e una tabella specificata in un vincolo (che fa riferimento a una tabella diversa da quella da creare) non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **create schema** e il nome della tabella o della vista specificato non esisteva.|  
|42S11|Indice già esistente|\**StatementText* conteneva un'istruzione **create index** e il nome di indice specificato esisteva già.<br /><br /> \**StatementText* conteneva un'istruzione **create schema** e il nome di indice specificato esisteva già.|  
|42S12|Indice non trovato|\**StatementText* conteneva un'istruzione **Drop index** e il nome di indice specificato non esisteva.|  
|42S21|La colonna esiste già|\**StatementText* conteneva un'istruzione **ALTER TABLE** e la colonna specificata nella clausola **Add** non è univoca o identifica una colonna esistente nella tabella di base.|  
|42S22|Colonna non trovata|\**StatementText* conteneva un'istruzione **create index** e uno o più nomi di colonna specificati nell'elenco delle colonne non esistevano.<br /><br /> \**StatementText* conteneva un'istruzione **Grant** o **Revoke** e non esisteva un nome di colonna specificato.<br /><br /> \**StatementText* conteneva un'istruzione **Select**, **Delete**, **Insert** o **Update** e non esisteva un nome di colonna specificato.<br /><br /> \**StatementText* conteneva un'istruzione **Create Table** e una colonna specificata in un vincolo (che fa riferimento a una tabella diversa da quella da creare) non esisteva.<br /><br /> \**StatementText* conteneva un'istruzione **create schema** e non esisteva un nome di colonna specificato.|  
|44000|Violazione della clausola WITH CHECK OPTION|L'argomento *StatementText* conteneva un'istruzione **Insert** eseguita su una tabella visualizzata o una tabella derivata dalla tabella visualizzata che è stata creata specificando **with check Option**, in modo tale che una o più righe interessate dall'istruzione **Insert** non saranno più presenti nella tabella visualizzata.<br /><br /> L'argomento *StatementText* conteneva un'istruzione **Update** eseguita su una tabella visualizzata o una tabella derivata dalla tabella visualizzata che è stata creata specificando **with check Option**, in modo tale che una o più righe interessate dall'istruzione **Update** non saranno più presenti nella tabella visualizzata.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|(DM) **StatementText* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLExecDirect** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) l'argomento *TextLength* era minore o uguale a 0 ma non uguale a SQL_NTS.<br /><br /> Un valore di parametro, impostato con **SQLBindParameter**, è un puntatore null e il valore di lunghezza del parametro non è 0, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM o minore o uguale a SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Un valore di parametro, impostato con **SQLBindParameter**, non è un puntatore null. il tipo di dati C è SQL_C_BINARY o SQL_C_CHAR; e il valore della lunghezza del parametro è minore di 0 ma non è SQL_NTS, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM o minore o uguale a SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Un valore di lunghezza parametro associato da **SQLBindParameter** è stato impostato su SQL_DATA_AT_EXEC; il tipo SQL è SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati lungo specifico dell'origine dati. e il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y".|  
|HY105|Tipo di parametro non valido|Il valore specificato per l'argomento *InputOutputType* in **SQLBindParameter** è stato SQL_PARAM_OUTPUT e il parametro è un parametro di input.|  
|HY109|Posizione del cursore non valida|\**StatementText* contiene un'istruzione Update o DELETE posizionata e il cursore è stato posizionato (da **SQLSetPos** o **SQLFetchScroll**) su una riga che è stata eliminata o che non è stato possibile recuperare.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 L'applicazione chiama **SQLExecDirect** per inviare un'istruzione SQL all'origine dati. Per ulteriori informazioni sull'esecuzione diretta, vedere [esecuzione diretta](../../../odbc/reference/develop-app/direct-execution-odbc.md). Il driver modifica l'istruzione in modo che utilizzi il formato di SQL utilizzato dall'origine dati e quindi lo invia all'origine dati. In particolare, il driver modifica le sequenze di escape utilizzate per definire determinate funzionalità di SQL. Per la sintassi delle sequenze di escape, vedere [sequenze di escape in ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
 L'applicazione può includere uno o più indicatori di parametro nell'istruzione SQL. Per includere un marcatore di parametro, l'applicazione incorpora un punto interrogativo (?) nell'istruzione SQL nella posizione appropriata. Per informazioni sui parametri, vedere [parametri di istruzione](../../../odbc/reference/develop-app/statement-parameters.md).  
  
 Se l'istruzione SQL è un'istruzione **Select** e se l'applicazione ha chiamato **SQLSetCursorName** per associare un cursore a un'istruzione, il driver utilizzerà il cursore specificato. In caso contrario, il driver genera un nome di cursore.  
  
 Se l'origine dati è in modalità di commit manuale (che richiede l'avvio esplicito della transazione) e non è ancora stata avviata una transazione, il driver avvia una transazione prima di inviare l'istruzione SQL. Per altre informazioni, vedere [modalità di commit manuale](../../../odbc/reference/develop-app/manual-commit-mode.md).  
  
 Se un'applicazione utilizza **SQLExecDirect** per inviare un'istruzione **commit** o **rollback** , non sarà interoperativa tra i prodotti DBMS. Per eseguire il commit o il rollback di una transazione, un'applicazione chiama **SQLEndTran**.  
  
 Se **SQLExecDirect** rileva un parametro data-at-execution, restituisce SQL_NEED_DATA. L'applicazione invia i dati usando **SQLParamData** e **SQLPutData**. Vedere [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)e [invio di dati Long](../../../odbc/reference/develop-app/sending-long-data.md).  
  
 Se **SQLExecDirect** esegue un'istruzione Update, INSERT o DELETE con ricerca che non influisce su alcuna riga nell'origine dati, la chiamata a **SQLExecDirect** restituisce SQL_NO_DATA.  
  
 Se il valore dell'attributo dell'istruzione SQL_ATTR_PARAMSET_SIZE è maggiore di 1 e l'istruzione SQL contiene almeno un marcatore di parametro, **SQLExecDirect** eseguirà l'istruzione SQL una volta per ogni set di valori di parametro dalle matrici a cui punta l'argomento *ParameterValuePointer* nella chiamata a **SQLBindParameter**. Per altre informazioni, vedere [matrici di valori di parametri](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Se i segnalibri sono attivati e viene eseguita una query che non è in grado di supportare segnalibri, il driver deve tentare di assegnare l'ambiente a un oggetto che supporta i segnalibri modificando un valore di attributo e restituendo SQLSTATE 01S02 (valore di opzione modificato). Se l'attributo non può essere modificato, il driver deve restituire SQLSTATE HY024 (valore di attributo non valido).  
  
> [!NOTE]  
>  Quando si utilizza il pool di connessioni, un'applicazione non deve eseguire istruzioni SQL che modificano il database o il contesto del database, ad esempio l'istruzione **use** _database_ in SQL Server, che modifica il catalogo utilizzato da un'origine dati.  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)e il [programma ODBC di esempio](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Esecuzione di un'operazione di commit o rollback|[SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Recupero di più righe di dati|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Restituzione di un nome di cursore|[Funzione SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Recupero di parte o di tutte le colonne di dati|[Funzione SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Restituzione del parametro successivo per l'invio dei dati|[Funzione SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Preparazione di un'istruzione per l'esecuzione|[Pagina relativa alla funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Invio di dati dei parametri in fase di esecuzione|[SQLPutData Function](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Impostazione di un nome di cursore|[Funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
