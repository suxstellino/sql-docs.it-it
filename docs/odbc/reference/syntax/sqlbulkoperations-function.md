---
description: Funzione SQLBulkOperations
title: Funzione SQLBulkOperations | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBulkOperations
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBulkOperations
helpviewer_keywords:
- SQLBulkOperations function [ODBC]
ms.assetid: 7029d0da-b0f2-44e6-9114-50bd96f47196
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a95e7701b12f9673452962c005a7bac44701e2d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181067"
---
# <a name="sqlbulkoperations-function"></a>Funzione SQLBulkOperations
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLBulkOperations** esegue inserimenti in blocco e operazioni di segnalibro bulk, tra cui l'aggiornamento, l'eliminazione e il recupero tramite segnalibro.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLBulkOperations(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Operation);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *Operazione*  
 Input Operazione da eseguire:  
  
 SQL_ADD SQL_UPDATE_BY_BOOKMARK SQL_DELETE_BY_BOOKMARK SQL_FETCH_BY_BOOKMARK  
  
 Per ulteriori informazioni, vedere "Commenti".  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLBulkOperations** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLBulkOperations** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
 Per tutti questi SQLSTATE che possono restituire SQL_SUCCESS_WITH_INFO o SQL_ERROR (ad eccezione di sql01xxxs), viene restituito SQL_SUCCESS_WITH_INFO se si verifica un errore in una o più righe, ma non in tutte, righe di un'operazione più righe e viene restituito SQL_ERROR se si verifica un errore in un'operazione a riga singola.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Troncamento a destra dei dati stringa|L'argomento dell' *operazione* è stato SQL_FETCH_BY_BOOKMARK e i dati stringa o binari restituiti per una colonna o colonne con tipo di dati SQL_C_CHAR o SQL_C_BINARY hanno causato il troncamento di dati binari non vuoti o non null.|  
|01S01|Errore nella riga|L'argomento dell' *operazione* è stato SQL_ADD e si è verificato un errore in una o più righe durante l'esecuzione dell'operazione, ma è stata aggiunta almeno una riga. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)<br /><br /> Questo errore viene generato solo quando un'applicazione utilizza un ODBC 2. driver *x* .)|  
|01S07|Troncamento frazionario|L'argomento *Operation* è stato SQL_FETCH_BY_BOOKMARK, il tipo di dati del buffer dell'applicazione non è stato SQL_C_CHAR o SQL_C_BINARY e i dati restituiti ai buffer dell'applicazione per una o più colonne sono stati troncati. (Per i tipi di dati C numerici, la parte frazionaria del numero è stata troncata. Per i tipi di dati time, timestamp e Interval C che contengono un componente ora, la porzione frazionaria del tempo è stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|L'argomento *Operation* è stato SQL_FETCH_BY_BOOKMARK e il valore dei dati di una colonna nel set di risultati non può essere convertito nel tipo di dati specificato dall'argomento *targetType* nella chiamata a **SQLBindCol**.<br /><br /> L'argomento *Operation* è stato SQL_UPDATE_BY_BOOKMARK o SQL_ADD e il valore dei dati nei buffer dell'applicazione non può essere convertito nel tipo di dati di una colonna del set di risultati.|  
|07009|Indice del descrittore non valido|L' *operazione* argument è stata SQL_ADD e una colonna è stata associata a un numero di colonna maggiore del numero di colonne nel set di risultati.|  
|21S02|Il grado della tabella derivata non corrisponde all'elenco di colonne|L' *operazione* dell'argomento è stata SQL_UPDATE_BY_BOOKMARK; e non è stato possibile aggiornare le colonne perché tutte le colonne sono di tipo non associato o di sola lettura oppure il valore nel buffer di lunghezza/indicatore associato è stato SQL_COLUMN_IGNORE.|  
|22001|Troncamento a destra dei dati stringa|L'assegnazione di un valore di tipo carattere o binario a una colonna nel set di risultati ha comportato il troncamento di caratteri non vuoti (per i caratteri) o byte non null (per binari).|  
|22003|Valore numerico non compreso nell'intervallo|L'argomento *Operation* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK e l'assegnazione di un valore numerico a una colonna nel set di risultati ha causato il troncamento dell'intero oggetto, anziché della parte frazionaria del numero.<br /><br /> L' *operazione* relativa all'argomento è stata SQL_FETCH_BY_BOOKMARK e la restituzione del valore numerico per una o più colonne limite avrebbe causato la perdita di cifre significative.|  
|22007|Formato DateTime non valido|L'argomento dell' *operazione* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK e l'assegnazione di un valore di data o di timestamp a una colonna nel set di risultati ha determinato che il campo anno, mese o giorno non è compreso nell'intervallo.<br /><br /> L' *operazione* relativa all'argomento è stata SQL_FETCH_BY_BOOKMARK e la restituzione del valore di data o timestamp per una o più colonne limite avrebbe causato il campo anno, mese o giorno non compreso nell'intervallo.|  
|22008|Overflow del campo Data/ora|L'argomento *Operation* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK e le prestazioni dell'aritmetica DateTime sui dati inviati a una colonna nel set di risultati hanno generato un campo DateTime (anno, mese, giorno, ora, minuto o secondo) del risultato che non rientra nell'intervallo consentito di valori per il campo o non è valido in base alle regole naturali del calendario gregoriano per DateTime.<br /><br /> L'argomento *Operation* è stato SQL_FETCH_BY_BOOKMARK e le prestazioni dell'aritmetica DateTime sui dati recuperati dal set di risultati hanno generato un campo DateTime (anno, mese, giorno, ora, minuto o secondo) del risultato che non rientra nell'intervallo consentito di valori per il campo o non è valido in base alle regole naturali del calendario gregoriano per le DateTime.|  
|22015|Overflow del campo Interval|L'argomento dell' *operazione* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK e l'assegnazione di un tipo numerico o intervallo C esatto a un tipo di dati SQL intervallo ha causato una perdita di cifre significative.<br /><br /> L'argomento dell' *operazione* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK; Quando si assegna a un tipo SQL intervallo, non esiste alcuna rappresentazione del valore del tipo C nel tipo SQL intervallo.<br /><br /> L'argomento dell' *operazione* è stato SQL_FETCH_BY_BOOKMARK e l'assegnazione da un tipo SQL numerico o intervallo esatto a un tipo intervallo C ha causato la perdita di cifre significative nel campo iniziali.<br /><br /> L'argomento dell' *operazione* è stato SQL_FETCH_BY_BOOKMARK; Quando si assegna a un tipo intervallo C, non è presente alcuna rappresentazione del valore del tipo SQL nel tipo intervallo C.|  
|22018|Valore di carattere non valido per la specifica del cast|L'argomento dell' *operazione* è stato SQL_FETCH_BY_BOOKMARK; il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. e il valore nella colonna non è un valore letterale valido del tipo C associato.<br /><br /> L' *operazione* dell'argomento è stata SQL_ADD o SQL_UPDATE_BY_BOOKMARK; il tipo SQL è un tipo numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo C è stato SQL_C_CHAR; e il valore nella colonna non è un valore letterale valido del tipo SQL associato.|  
|23000|Violazione del vincolo di integrità|L'argomento dell' *operazione* è stato SQL_ADD, SQL_DELETE_BY_BOOKMARK o SQL_UPDATE_BY_BOOKMARK ed è stato violato un vincolo di integrità.<br /><br /> L'argomento *Operation* è stato SQL_ADD e una colonna non associata è definita come not null e non ha alcun valore predefinito.<br /><br /> L'argomento *Operation* è stato SQL_ADD, la lunghezza specificata nel buffer *StrLen_or_IndPtr* associato è stata SQL_COLUMN_IGNORE e la colonna non ha un valore predefinito.|  
|24000|Stato del cursore non valido|Lo stato di *statementHandle* è stato eseguito, ma nessun set di risultati è stato associato a *statementHandle*.|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|42000|Errore di sintassi o violazione di accesso|Il driver non è stato in grado di bloccare la riga in base alle esigenze per eseguire l'operazione richiesta nell'argomento *Operation* .|  
|44000|Violazione della clausola WITH CHECK OPTION|L'argomento *Operation* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK e l'inserimento o l'aggiornamento è stato eseguito in una tabella visualizzata, o una tabella derivata dalla tabella visualizzata, che è stata creata specificando **with check Option**, in modo tale che una o più righe interessate dall'istruzione INSERT o Update non saranno più presenti nella tabella visualizzata.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLBulkOperations** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) il *statementHandle* specificato non si trova in uno stato eseguito. La funzione è stata chiamata senza prima chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) il driver era ODBC 2. è stato chiamato il driver *x* e **SQLBulkOperations** per *statementHandle* prima della chiamata a **SQLFetchScroll** o **SQLFetch** .<br /><br /> (DM) **SQLBulkOperations** è stato chiamato dopo la chiamata di **SQLExtendedFetch** su *statementHandle*.|  
|HY011|Non è possibile impostare l'attributo adesso|(DM) il driver era ODBC 2. il driver *x* e l'attributo SQL_ATTR_ROW_STATUS_PTR istruzione sono stati impostati tra le chiamate a **SQLFetch** o **SQLFetchScroll** e **SQLBulkOperations**.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|L'argomento dell' *operazione* è stato SQL_ADD o SQL_UPDATE_BY_BOOKMARK; un valore di dati non è un puntatore null. il tipo di dati C è SQL_C_BINARY o SQL_C_CHAR; il valore della lunghezza della colonna è minore di 0, ma non uguale a SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NTS o SQL_NULL_DATA o minore o uguale a SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Il valore in un buffer di lunghezza/indicatore è stato SQL_DATA_AT_EXEC; il tipo SQL è SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati lungo specifico dell'origine dati. e il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y".<br /><br /> L'argomento *Operation* è stato SQL_ADD, l'attributo SQL_ATTR_USE_BOOKMARK Statement è stato impostato su SQL_UB_VARIABLE e la colonna 0 è stata associata a un buffer la cui lunghezza non è uguale alla lunghezza massima del segnalibro per il set di risultati. Questa lunghezza è disponibile nel campo SQL_DESC_OCTET_LENGTH di IRD e può essere ottenuta chiamando **SQLDescribeCol**, **SQLColAttribute** o **SQLGetDescField**.|  
|HY092|Identificatore di attributo non valido|(DM) il valore specificato per l'argomento dell' *operazione* non è valido.<br /><br /> L'argomento *Operation* è SQL_ADD, SQL_UPDATE_BY_BOOKMARK o SQL_DELETE_BY_BOOKMARK e l'attributo SQL_ATTR_CONCURRENCY Statement è stato impostato su SQL_CONCUR_READ_ONLY.<br /><br /> L'argomento dell' *operazione* è stato SQL_DELETE_BY_BOOKMARK, SQL_FETCH_BY_BOOKMARK o SQL_UPDATE_BY_BOOKMARK e la colonna del segnalibro non è associata oppure l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS è stato impostato su SQL_UB_OFF.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta l'operazione richiesta nell'argomento *Operation* .|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr** con un argomento di *attributo* SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
  
> [!CAUTION]  
>  Per informazioni sugli stati di istruzione in cui è possibile chiamare **SQLBulkOperations** e sulle operazioni che è necessario eseguire per la compatibilità con ODBC 2. *x* applicazioni, vedere la sezione [cursori a blocchi, cursori scorrevoli e compatibilità con le versioni precedenti](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) in Appendice G: linee guida per la compatibilità con le versioni precedenti.  
  
 Un'applicazione utilizza **SQLBulkOperations** per eseguire le operazioni seguenti sulla tabella o sulla vista di base che corrisponde alla query corrente:  
  
-   Aggiungere nuove righe.  
  
-   Aggiorna un set di righe in cui ogni riga viene identificata da un segnalibro.  
  
-   Consente di eliminare un set di righe in cui ogni riga viene identificata da un segnalibro.  
  
-   Recuperare un set di righe in cui ogni riga viene identificata da un segnalibro.  
  
 Dopo una chiamata a **SQLBulkOperations**, la posizione del cursore a blocchi non è definita. Per impostare la posizione del cursore, l'applicazione deve chiamare **SQLFetchScroll** . Un'applicazione deve chiamare **SQLFetchScroll** solo con un argomento *FetchOrientation* di SQL_FETCH_FIRST, SQL_FETCH_LAST, SQL_FETCH_ABSOLUTE o SQL_FETCH_BOOKMARK. La posizione del cursore non è definita se l'applicazione chiama **SQLFetch** o **SQLFetchScroll** con un argomento *FetchOrientation* di SQL_FETCH_PRIOR, SQL_FETCH_NEXT o SQL_FETCH_RELATIVE.  
  
 Una colonna può essere ignorata nelle operazioni bulk eseguite da una chiamata a **SQLBulkOperations** impostando il buffer di lunghezza/indicatore di colonna specificato nella chiamata a **SQLBindCol**, per SQL_COLUMN_IGNORE.  
  
 Non è necessario che l'applicazione imposti l'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR quando chiama **SQLBulkOperations** perché le righe non possono essere ignorate durante l'esecuzione di operazioni bulk con questa funzione.  
  
 Il buffer a cui punta l'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR contiene il numero di righe interessate da una chiamata a **SQLBulkOperations**.  
  
 Quando l'argomento *Operation* è SQL_ADD o SQL_UPDATE_BY_BOOKMARK e l'elenco SELECT della specifica della query associata al cursore contiene più di un riferimento alla stessa colonna, viene definito dal driver se viene generato un errore o se il driver ignora i riferimenti duplicati ed esegue le operazioni richieste.  
  
 Per altre informazioni su come usare **SQLBulkOperations**, vedere [aggiornamento dei dati con SQLBulkOperations](../../../odbc/reference/develop-app/updating-data-with-sqlbulkoperations.md).  
  
## <a name="performing-bulk-inserts"></a>Esecuzione di inserimenti bulk  
 Per inserire dati con **SQLBulkOperations**, un'applicazione esegue la sequenza di passaggi seguente:  
  
1.  Esegue una query che restituisce un set di risultati.  
  
2.  Imposta l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE sul numero di righe che desidera inserire.  
  
3.  Chiama **SQLBindCol** per associare i dati che desidera inserire. I dati sono associati a una matrice con una dimensione uguale al valore di SQL_ATTR_ROW_ARRAY_SIZE.  
  
    > [!NOTE]  
    >  Le dimensioni della matrice a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR devono essere uguali a SQL_ATTR_ROW_ARRAY_SIZE o SQL_ATTR_ROW_STATUS_PTR deve essere un puntatore null.  
  
4.  Chiama **SQLBulkOperations**(*StatementHandle,* SQL_ADD) per eseguire l'inserimento.  
  
5.  Se l'applicazione ha impostato l'attributo SQL_ATTR_ROW_STATUS_PTR Statement, può ispezionare questa matrice per vedere il risultato dell'operazione.  
  
 Se un'applicazione associa la colonna 0 prima di chiamare **SQLBulkOperations** con un argomento *Operation* di SQL_ADD, il driver aggiornerà i buffer della colonna 0 associati con i valori dei segnalibri per la riga appena inserita. Per eseguire questa operazione, l'applicazione deve impostare l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS su SQL_UB_VARIABLE prima di eseguire l'istruzione. Questa operazione non funziona con ODBC 2. driver *x* .)  
  
 I dati Long possono essere aggiunti in parti da SQLBulkOperations, usando le chiamate a SQLParamData e SQLPutData. Per ulteriori informazioni, vedere "fornitura di dati Long per gli inserimenti e aggiornamenti bulk" più avanti in questo riferimento alla funzione.  
  
 Non è necessario che l'applicazione chiami **SQLFetch** o **SQLFetchScroll** prima di chiamare **SQLBulkOperations** (eccetto quando si passa a un ODBC 2.*driver x* ; vedere [compatibilità con le versioni precedenti e conformità agli standard](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).  
  
 Il comportamento è definito dal driver se **SQLBulkOperations**, con un argomento *Operation* di SQL_ADD, viene chiamato su un cursore che contiene colonne duplicate. Il driver può restituire un valore SQLSTATE definito dal driver, aggiungere i dati alla prima colonna visualizzata nel set di risultati oppure eseguire altro comportamento definito dal driver.  
  
## <a name="performing-bulk-updates-by-using-bookmarks"></a>Esecuzione di aggiornamenti bulk tramite segnalibri  
 Per eseguire gli aggiornamenti in blocco utilizzando segnalibri con **SQLBulkOperations**, un'applicazione esegue i passaggi seguenti in sequenza:  
  
1.  Imposta l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS su SQL_UB_VARIABLE.  
  
2.  Esegue una query che restituisce un set di risultati.  
  
3.  Imposta l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE sul numero di righe che si desidera aggiornare.  
  
4.  Chiama **SQLBindCol** per associare i dati che desidera aggiornare. I dati sono associati a una matrice con una dimensione uguale al valore di SQL_ATTR_ROW_ARRAY_SIZE. Chiama anche **SQLBindCol** per associare la colonna 0 (colonna del segnalibro).  
  
5.  Copia i segnalibri per le righe di cui è interessato l'aggiornamento nella matrice associata alla colonna 0.  
  
6.  Aggiorna i dati nei buffer associati.  
  
    > [!NOTE]  
    >  Le dimensioni della matrice a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR devono essere uguali a SQL_ATTR_ROW_ARRAY_SIZE o SQL_ATTR_ROW_STATUS_PTR deve essere un puntatore null.  
  
7.  Chiama **SQLBulkOperations**(*StatementHandle,* SQL_UPDATE_BY_BOOKMARK).  
  
    > [!NOTE]  
    >  Se l'applicazione ha impostato l'attributo SQL_ATTR_ROW_STATUS_PTR Statement, può ispezionare questa matrice per vedere il risultato dell'operazione.  
  
8.  Chiama facoltativamente **SQLBulkOperations**(*statementHandle*, SQL_FETCH_BY_BOOKMARK) per recuperare i dati nei buffer dell'applicazione associati per verificare che l'aggiornamento sia stato eseguito.  
  
9. Se i dati sono stati aggiornati, il driver modifica il valore nella matrice di stato della riga per le righe appropriate SQL_ROW_UPDATED.  
  
 Gli aggiornamenti in blocco eseguiti da **SQLBulkOperations** possono includere dati di lunghezza usando le chiamate a **SQLParamData** e **SQLPutData**. Per ulteriori informazioni, vedere "fornitura di dati Long per gli inserimenti e aggiornamenti bulk" più avanti in questo riferimento alla funzione.  
  
 Se i segnalibri vengono mantenuti tra i cursori, non è necessario che l'applicazione chiami **SQLFetch** o **SQLFetchScroll** prima dell'aggiornamento da parte dei segnalibri. Può utilizzare i segnalibri archiviati da un cursore precedente. Se i segnalibri non vengono mantenuti tra i cursori, l'applicazione deve chiamare **SQLFetch** o **SQLFetchScroll** per recuperare i segnalibri.  
  
 Il comportamento è definito dal driver se **SQLBulkOperations**, con un argomento *Operation* di SQL_UPDATE_BY_BOOKMARK, viene chiamato su un cursore che contiene colonne duplicate. Il driver può restituire un valore SQLSTATE definito dal driver, aggiornare la prima colonna visualizzata nel set di risultati o eseguire altro comportamento definito dal driver.  
  
## <a name="performing-bulk-fetches-using-bookmarks"></a>Esecuzione di operazioni di recupero bulk mediante segnalibri  
 Per eseguire il recupero bulk mediante segnalibri con **SQLBulkOperations**, un'applicazione esegue i passaggi seguenti in sequenza:  
  
1.  Imposta l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS su SQL_UB_VARIABLE.  
  
2.  Esegue una query che restituisce un set di risultati.  
  
3.  Imposta l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE sul numero di righe che desidera recuperare.  
  
4.  Chiama **SQLBindCol** per associare i dati che desidera recuperare. I dati sono associati a una matrice con una dimensione uguale al valore di SQL_ATTR_ROW_ARRAY_SIZE. Chiama anche **SQLBindCol** per associare la colonna 0 (colonna del segnalibro).  
  
5.  Copia i segnalibri per le righe di cui è interessato il recupero nella matrice associata alla colonna 0. Si presuppone che l'applicazione abbia già ottenuto i segnalibri separatamente.  
  
    > [!NOTE]  
    >  Le dimensioni della matrice a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR devono essere uguali a SQL_ATTR_ROW_ARRAY_SIZE o SQL_ATTR_ROW_STATUS_PTR deve essere un puntatore null.  
  
6.  Chiama **SQLBulkOperations**(*StatementHandle,* SQL_FETCH_BY_BOOKMARK).  
  
7.  Se l'applicazione ha impostato l'attributo SQL_ATTR_ROW_STATUS_PTR Statement, può ispezionare questa matrice per vedere il risultato dell'operazione.  
  
 Se i segnalibri vengono mantenuti tra i cursori, non è necessario che l'applicazione chiami **SQLFetch** o **SQLFetchScroll** prima di recuperare i segnalibri. Può utilizzare i segnalibri archiviati da un cursore precedente. Se i segnalibri non vengono mantenuti tra i cursori, l'applicazione deve chiamare **SQLFetch** o **SQLFetchScroll** una volta per recuperare i segnalibri.  
  
## <a name="performing-bulk-deletes-using-bookmarks"></a>Esecuzione di eliminazioni bulk mediante segnalibri  
 Per eseguire le eliminazioni bulk mediante segnalibri con **SQLBulkOperations**, un'applicazione esegue i passaggi seguenti in sequenza:  
  
1.  Imposta l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS su SQL_UB_VARIABLE.  
  
2.  Esegue una query che restituisce un set di risultati.  
  
3.  Imposta l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE sul numero di righe che si desidera eliminare.  
  
4.  Chiama **SQLBindCol** per associare la colonna 0 (colonna del segnalibro).  
  
5.  Copia i segnalibri per le righe che desidera eliminare nella matrice associata alla colonna 0.  
  
    > [!NOTE]  
    >  Le dimensioni della matrice a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR devono essere uguali a SQL_ATTR_ROW_ARRAY_SIZE o SQL_ATTR_ROW_STATUS_PTR deve essere un puntatore null.  
  
6.  Chiama **SQLBulkOperations**(*StatementHandle,* SQL_DELETE_BY_BOOKMARK).  
  
7.  Se l'applicazione ha impostato l'attributo SQL_ATTR_ROW_STATUS_PTR Statement, può ispezionare questa matrice per vedere il risultato dell'operazione.  
  
 Se i segnalibri vengono mantenuti tra i cursori, l'applicazione non deve chiamare **SQLFetch** o **SQLFetchScroll** prima dell'eliminazione da parte dei segnalibri. Può utilizzare i segnalibri archiviati da un cursore precedente. Se i segnalibri non vengono mantenuti tra i cursori, l'applicazione deve chiamare **SQLFetch** o **SQLFetchScroll** una volta per recuperare i segnalibri.  
  
## <a name="providing-long-data-for-bulk-inserts-and-updates"></a>Fornire dati lunghi per gli inserimenti e gli aggiornamenti bulk  
 È possibile fornire dati lunghi per gli inserimenti e gli aggiornamenti bulk eseguiti da chiamate a **SQLBulkOperations**. Per inserire o aggiornare i dati lunghi, un'applicazione esegue i passaggi seguenti oltre ai passaggi descritti nella sezione "esecuzione di inserimenti bulk" e "esecuzione di aggiornamenti bulk mediante segnalibri" più indietro in questo argomento.  
  
1.  Quando associa i dati tramite **SQLBindCol**, l'applicazione inserisce un valore definito dall'applicazione, ad esempio il numero di colonna, nel buffer *\* TargetValuePtr* per le colonne data-at-execution. Il valore può essere utilizzato in un secondo momento per identificare la colonna.  
  
     L'applicazione inserisce il risultato della macro SQL_LEN_DATA_AT_EXEC (*length*) nel buffer di *\* StrLen_or_IndPtr* . Se il tipo di dati SQL della colonna è SQL_LONGVARBINARY, SQL_LONGVARCHAR o un tipo di dati lungo specifico dell'origine dati e il driver restituisce "Y" per il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo**, *length* è il numero di byte di dati da inviare per il parametro. in caso contrario, deve essere un valore non negativo e viene ignorato.  
  
2.  Quando viene chiamato **SQLBulkOperations** , se sono presenti colonne data-at-execution, la funzione restituisce SQL_NEED_DATA e procede al passaggio 3, che segue. Se non sono presenti colonne data-at-execution, il processo è completo.  
  
3.  L'applicazione chiama **SQLParamData** per recuperare l'indirizzo del buffer *\* TargetValuePtr* per la prima colonna data-at-execution da elaborare. **SQLParamData** restituisce SQL_NEED_DATA. L'applicazione recupera il valore definito dall'applicazione dal buffer *\* TargetValuePtr* .  
  
    > [!NOTE]  
    >  Sebbene i parametri data-at-execution siano simili alle colonne data-at-execution, il valore restituito da **SQLParamData** è diverso per ogni.  
  
     Le colonne data-at-execution sono colonne di un set di righe per cui i dati verranno inviati con **SQLPutData** quando una riga viene aggiornata o inserita con **SQLBulkOperations**. Sono associati a **SQLBindCol**. Il valore restituito da **SQLParamData** è l'indirizzo della riga nel buffer **TargetValuePtr* in fase di elaborazione.  
  
4.  L'applicazione chiama **SQLPutData** una o più volte per inviare i dati per la colonna. È necessaria più di una chiamata se non è possibile restituire tutto il valore dei dati nel buffer *\* TargetValuePtr* specificato in **SQLPutData**. sono consentite più chiamate a **SQLPutData** per la stessa colonna solo quando si inviano dati di tipo carattere c a una colonna con un tipo di dati carattere, binario o specifico dell'origine dati oppure quando si inviano dati c binari a una colonna con tipo di dati character, Binary  
  
5.  L'applicazione chiama di nuovo **SQLParamData** per segnalare che tutti i dati sono stati inviati per la colonna.  
  
    -   Se sono presenti più colonne data-at-execution, **SQLParamData** restituisce SQL_NEED_DATA e l'indirizzo del buffer *TargetValuePtr* per la colonna data-at-execution successiva da elaborare. L'applicazione ripete i passaggi 4 e 5.  
  
    -   Se non sono presenti altre colonne data-at-execution, il processo è completo. Se l'istruzione è stata eseguita correttamente, **SQLParamData** restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO; Se l'esecuzione ha esito negativo, restituisce SQL_ERROR. A questo punto, **SQLParamData** può restituire qualsiasi SQLSTATE che può essere restituito da **SQLBulkOperations**.  
  
 Se l'operazione viene annullata o si verifica un errore in **SQLParamData** o **SQLPutData** dopo che **SQLBulkOperations** restituisce SQL_NEED_DATA e prima che i dati vengano inviati per tutte le colonne data-at-execution, l'applicazione può chiamare solo  **SQLCancel**, **SQLGetDiagField** **, SQLGetDiagRec, SQLGetFunctions**, **SQLParamData** o **SQLPutData** per l'istruzione o la connessione associata all'istruzione. Se chiama qualsiasi altra funzione per l'istruzione o la connessione associata all'istruzione, la funzione restituisce SQL_ERROR e SQLSTATE HY010 (errore della sequenza di funzioni).  
  
 Se l'applicazione chiama **SQLCancel** mentre il driver necessita ancora di dati per le colonne data-at-execution, il driver Annulla l'operazione. L'applicazione può quindi chiamare di nuovo **SQLBulkOperations** . l'annullamento non influisce sullo stato del cursore o sulla posizione corrente del cursore.  
  
## <a name="row-status-array"></a>Matrice di stato riga  
 La matrice di stato della riga contiene i valori di stato per ogni riga di dati nel set di righe dopo una chiamata a **SQLBulkOperations**. Il driver imposta i valori di stato in questa matrice dopo una chiamata a **SQLFetch**, **SQLFetchScroll**, **SQLSetPos** o **SQLBulkOperations**. Questa matrice viene inizialmente popolata da una chiamata a **SQLBulkOperations** se **SQLFetch** o **SQLFetchScroll** non è stato chiamato prima di **SQLBulkOperations**. Questa matrice fa riferimento all'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR. Il numero di elementi nelle matrici di stato della riga deve essere uguale al numero di righe nel set di righe (come definito dall'attributo SQL_ATTR_ROW_ARRAY_SIZE Statement). Per informazioni su questa matrice di stato della riga, vedere [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente vengono recuperate 10 righe di dati alla volta dalla tabella Customers. Viene quindi richiesto all'utente di eseguire un'azione. Per ridurre il traffico di rete, l'esempio memorizza nel buffer gli aggiornamenti, le eliminazioni e gli inserimenti in locale nelle matrici limitate, ma in corrispondenza degli offset oltre i dati del set di righe. Quando l'utente sceglie di inviare aggiornamenti, eliminazioni e inserimenti nell'origine dati, il codice imposta l'offset del binding in modo appropriato e chiama **SQLBulkOperations**. Per semplicità, l'utente non può memorizzare nel buffer più di 10 aggiornamenti, eliminazioni o inserimenti.  
  
```cpp  
// SQLBulkOperations_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include "stdio.h"  
  
#define UPDATE_ROW 100  
#define DELETE_ROW 101  
#define ADD_ROW 102  
#define SEND_TO_DATA_SOURCE 103  
#define UPDATE_OFFSET 10  
#define INSERT_OFFSET 20  
#define DELETE_OFFSET 30  
  
// Define structure for customer data (assume 10 byte maximum bookmark size).  
typedef struct tagCustStruct {  
   SQLCHAR Bookmark[10];  
   SQLINTEGER BookmarkLen;  
   SQLUINTEGER CustomerID;  
   SQLINTEGER CustIDInd;  
   SQLCHAR CompanyName[51];  
   SQLINTEGER NameLenOrInd;  
   SQLCHAR Address[51];  
   SQLINTEGER AddressLenOrInd;  
   SQLCHAR Phone[11];  
   SQLINTEGER PhoneLenOrInd;  
} CustStruct;  
  
// Allocate 40 of these structures. Elements 0-9 are for the current rowset,  
// elements 10-19 are for the buffered updates, elements 20-29 are for  
// the buffered inserts, and elements 30-39 are for the buffered deletes.  
CustStruct CustArray[40];  
SQLUSMALLINT RowStatusArray[10], Action, RowNum, NumUpdates = 0, NumInserts = 0,  
NumDeletes = 0;  
SQLLEN BindOffset = 0;  
SQLRETURN retcode;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLHSTMT hstmt = NULL;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Set the following statement attributes:  
   // SQL_ATTR_CURSOR_TYPE:           Keyset-driven  
   // SQL_ATTR_ROW_BIND_TYPE:         Row-wise  
   // SQL_ATTR_ROW_ARRAY_SIZE:        10  
   // SQL_ATTR_USE_BOOKMARKS:         Use variable-length bookmarks  
   // SQL_ATTR_ROW_STATUS_PTR:        Points to RowStatusArray  
   // SQL_ATTR_ROW_BIND_OFFSET_PTR:   Points to BindOffset  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER)SQL_CURSOR_KEYSET_DRIVEN, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER)sizeof(CustStruct), 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)10, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_USE_BOOKMARKS, (SQLPOINTER)SQL_UB_VARIABLE, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_STATUS_PTR, RowStatusArray, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_BIND_OFFSET_PTR, &BindOffset, 0);  
  
   // Bind arrays to the bookmark, CustomerID, CompanyName, Address, and Phone columns.  
   retcode = SQLBindCol(hstmt, 0, SQL_C_VARBOOKMARK, CustArray[0].Bookmark, sizeof(CustArray[0].Bookmark), &CustArray[0].BookmarkLen);  
   retcode = SQLBindCol(hstmt, 1, SQL_C_ULONG, &CustArray[0].CustomerID, 0, &CustArray[0].CustIDInd);  
   retcode = SQLBindCol(hstmt, 2, SQL_C_CHAR, CustArray[0].CompanyName, sizeof(CustArray[0].CompanyName), &CustArray[0].NameLenOrInd);  
   retcode = SQLBindCol(hstmt, 3, SQL_C_CHAR, CustArray[0].Address, sizeof(CustArray[0].Address), &CustArray[0].AddressLenOrInd);  
   retcode = SQLBindCol(hstmt, 4, SQL_C_CHAR, CustArray[0].Phone, sizeof(CustArray[0].Phone), &CustArray[0].PhoneLenOrInd);  
  
   // Execute a statement to retrieve rows from the Customers table.  
   retcode = SQLExecDirect(hstmt, (SQLCHAR*)"SELECT CustomerID, CompanyName, Address, Phone FROM Customers", SQL_NTS);  
  
   // Fetch and display the first 10 rows.  
   retcode = SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
   // DisplayCustData(CustArray, 10);  
  
   // Call GetAction to get an action and a row number from the user.  
   // while (GetAction(&Action, &RowNum)) {  
   Action = SQL_FETCH_NEXT;  
   RowNum = 2;  
   switch (Action) {  
      case SQL_FETCH_NEXT:  
      case SQL_FETCH_PRIOR:  
      case SQL_FETCH_FIRST:  
      case SQL_FETCH_LAST:  
      case SQL_FETCH_ABSOLUTE:  
      case SQL_FETCH_RELATIVE:  
         // Fetch and display the requested data.  
         SQLFetchScroll(hstmt, Action, RowNum);  
         // DisplayCustData(CustArray, 10);  
         break;  
  
      case UPDATE_ROW:  
         // Check if we have reached the maximum number of buffered updates.  
         if (NumUpdates < 10) {  
            // Get the new customer data and place it in the next available element of  
            // the buffered updates section of CustArray, copy the bookmark of the row  
            // being updated to the same element, and increment the update counter.  
            // Checking to see we have not already buffered an update for this  
            // row not shown.  
            // GetNewCustData(CustArray, UPDATE_OFFSET + NumUpdates);  
            memcpy(CustArray[UPDATE_OFFSET + NumUpdates].Bookmark,  
               CustArray[RowNum - 1].Bookmark,  
               CustArray[RowNum - 1].BookmarkLen);  
            CustArray[UPDATE_OFFSET + NumUpdates].BookmarkLen =  
               CustArray[RowNum - 1].BookmarkLen;  
            NumUpdates++;  
         } else {  
            printf("Buffers full. Send buffered changes to the data source.");  
         }  
         break;  
      case DELETE_ROW:  
         // Check if we have reached the maximum number of buffered deletes.  
         if (NumDeletes < 10) {  
            // Copy the bookmark of the row being deleted to the next available element  
            // of the buffered deletes section of CustArray and increment the delete  
            // counter. Checking to see we have not already buffered an update for  
            // this row not shown.  
            memcpy(CustArray[DELETE_OFFSET + NumDeletes].Bookmark,  
               CustArray[RowNum - 1].Bookmark,  
               CustArray[RowNum - 1].BookmarkLen);  
  
            CustArray[DELETE_OFFSET + NumDeletes].BookmarkLen =  
               CustArray[RowNum - 1].BookmarkLen;  
  
            NumDeletes++;  
         } else  
            printf("Buffers full. Send buffered changes to the data source.");  
         break;  
  
      case ADD_ROW:  
         // reached maximum number of buffered inserts?  
         if (NumInserts < 10) {  
            // Get the new customer data and place it in the next available element of  
            // the buffered inserts section of CustArray and increment insert counter.  
            // GetNewCustData(CustArray, INSERT_OFFSET + NumInserts);  
            NumInserts++;  
         } else  
            printf("Buffers full. Send buffered changes to the data source.");  
         break;  
  
      case SEND_TO_DATA_SOURCE:  
         // If there are any buffered updates, inserts, or deletes, set the array size  
         // to that number, set the binding offset to use the data in the buffered  
         // update, insert, or delete part of CustArray, and call SQLBulkOperations to  
         // do the updates, inserts, or deletes. Because we will never have more than  
         // 10 updates, inserts, or deletes, we can use the same row status array.  
         if (NumUpdates) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumUpdates, 0);  
            BindOffset = UPDATE_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_UPDATE_BY_BOOKMARK);  
            NumUpdates = 0;  
         }  
  
         if (NumInserts) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumInserts, 0);  
            BindOffset = INSERT_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_ADD);  
            NumInserts = 0;  
         }  
  
         if (NumDeletes) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumDeletes, 0);  
            BindOffset = DELETE_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_DELETE_BY_BOOKMARK);  
            NumDeletes = 0;  
         }  
  
         // If there were any updates, inserts, or deletes, reset the binding offset  
         // and array size to their original values.  
         if (NumUpdates || NumInserts || NumDeletes) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)10, 0);  
            BindOffset = 0;  
         }  
         break;  
   }  
   // }  
  
   // Close the cursor.  
   SQLFreeStmt(hstmt, SQL_CLOSE);  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di un singolo campo di un descrittore|[Funzione SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Recupero di più campi di un descrittore|[Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Impostazione di un singolo campo di un descrittore|[Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Impostazione di più campi di un descrittore|[Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|Posizionamento del cursore, aggiornamento dei dati nel set di righe o aggiornamento o eliminazione di dati nel set di righe|[Funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
