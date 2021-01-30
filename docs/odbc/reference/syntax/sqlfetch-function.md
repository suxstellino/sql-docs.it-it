---
description: Funzione SQLFetch
title: Funzione SQLFetch | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetch
helpviewer_keywords:
- SQLFetch function [ODBC]
ms.assetid: 6c6611d2-bc6a-4390-87c9-1c5dd9cfe07c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 095a1ca4a67122344926d9ca363dd9f05205c48b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209794"
---
# <a name="sqlfetch-function"></a>Funzione SQLFetch
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLFetch** Recupera il set di righe successivo di dati dal set di risultati e restituisce i dati per tutte le colonne con binding.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLFetch(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLFetch** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando la [funzione SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE generalmente restituiti da **SQLFetch** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente. Se si verifica un errore in una singola colonna, è possibile chiamare [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) con un *DiagIdentifier* di SQL_DIAG_COLUMN_NUMBER per determinare la colonna in cui si è verificato l'errore. è possibile chiamare e **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_ROW_NUMBER per determinare la riga che contiene la colonna.  
  
 Per tutti questi SQLSTATE che possono restituire SQL_SUCCESS_WITH_INFO o SQL_ERROR (ad eccezione di sql01xxxs), viene restituito SQL_SUCCESS_WITH_INFO se si verifica un errore in una o più righe, ma non in tutte, righe di un'operazione più righe e viene restituito SQL_ERROR se si verifica un errore in un'operazione a riga singola.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati di tipo stringa o binario restituiti per una colonna hanno causato il troncamento di dati di tipo carattere non vuoto o binari non NULL. Se si trattasse di un valore stringa, viene troncato a destra.|  
|01S01|Errore nella riga|Si è verificato un errore durante il recupero di una o più righe.<br /><br /> Se questo valore SQLSTATE viene restituito quando un'applicazione ODBC 3 *. x* utilizza un driver ODBC 2 *. x* , è possibile ignorarla.|  
|01S07|Troncamento frazionario|I dati restituiti per una colonna sono stati troncati. Per i tipi di dati numerici, la parte frazionaria del numero è stata troncata. Per i tipi di dati time, timestamp e Interval che contengono un componente ora, la parte frazionaria del tempo è stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Impossibile convertire il valore dei dati di una colonna nel set di risultati nel tipo di dati specificato da *targetType* in **SQLBindCol**.<br /><br /> La colonna 0 è stata associata a un tipo di dati di SQL_C_BOOKMARK e l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è stato impostato su SQL_UB_VARIABLE.<br /><br /> La colonna 0 è stata associata a un tipo di dati di SQL_C_VARBOOKMARK e l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS non è stato impostato su SQL_UB_VARIABLE.|  
|07009|Indice del descrittore non valido|Il driver era un driver ODBC 2 *. x* che non supporta **SQLExtendedFetch** e il numero di colonna specificato nell'associazione per una colonna era 0.<br /><br /> La colonna 0 è stata associata e l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS è stato impostato su SQL_UB_OFF.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22001|Dati stringa, troncati a destra|Un segnalibro a lunghezza variabile restituito per una colonna è stato troncato.|  
|22002|Variabile indicatore obbligatoria ma non fornita|Sono stati recuperati dati NULL in una colonna la cui *StrLen_or_IndPtr* impostata da **SQLBindCol** (o SQL_DESC_INDICATOR_PTR impostata da **SQLSetDescField** o **SQLSetDescRec**) è un puntatore null.|  
|22003|Valore numerico non compreso nell'intervallo|La restituzione del valore numerico come valore numerico o stringa per una o più colonne con binding avrebbe causato il troncamento dell'intero oggetto, invece della parte frazionaria del numero.<br /><br /> Per ulteriori informazioni, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) in Appendice D: tipi di dati.|  
|22007|Formato DateTime non valido|Una colonna di tipo carattere nel set di risultati è stata associata a una struttura di data, ora o timestamp e un valore nella colonna era, rispettivamente, una data, un'ora o un timestamp non valido.|  
|22012|Divisione per zero|È stato restituito un valore da un'espressione aritmetica, che ha comportato la divisione per zero.|  
|22015|Overflow del campo Interval|L'assegnazione da un tipo SQL esatto o intervallo di tipo SQL a un tipo intervallo C ha causato la perdita di cifre significative nel campo iniziali.<br /><br /> Quando si recuperano i dati in un tipo intervallo C, non esiste alcuna rappresentazione del valore del tipo SQL nel tipo intervallo C.|  
|22018|Valore di carattere non valido per la specifica del cast|Una colonna di tipo carattere nel set di risultati è stata associata a un buffer C di caratteri e la colonna conteneva un carattere per il quale non era presente alcuna rappresentazione nel set di caratteri del buffer.<br /><br /> Il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. e il valore nella colonna non è un valore letterale valido del tipo C associato.|  
|24000|Stato del cursore non valido|Lo stato di *statementHandle* è stato eseguito ma nessun set di risultati è stato associato a *statementHandle*.|  
|40001|Errore di serializzazione|La transazione in cui è stata eseguita la lettura è stata terminata per impedire il deadlock.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione **SQLFetch** è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. Quindi, la funzione **SQLFetch** è stata chiamata nuovamente in *statementHandle*.<br /><br /> In alternativa, la funzione  **SQLFetch** è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLFetch** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) il *statementHandle* specificato non si trova in uno stato eseguito. La funzione è stata chiamata senza prima chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) **SQLFetch** è stato chiamato per *statementHandle* dopo la chiamata di **SQLExtendedFetch** e prima della chiamata a **SQLFreeStmt** con l'opzione SQL_CLOSE.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|L'attributo SQL_ATTR_USE_BOOKMARK Statement è stato impostato su SQL_UB_VARIABLE e la colonna 0 è stata associata a un buffer la cui lunghezza non è uguale alla lunghezza massima per il segnalibro per questo set di risultati. Questa lunghezza è disponibile nel campo SQL_DESC_OCTET_LENGTH di IRD e può essere ottenuta chiamando **SQLDescribeCol**, **SQLColAttribute** o **SQLGetDescField**.|  
|HY107|Valore di riga non compreso nell'intervallo|Il valore specificato con l'attributo dell'istruzione SQL_ATTR_CURSOR_TYPE è stato SQL_CURSOR_KEYSET_DRIVEN, ma il valore specificato con l'attributo SQL_ATTR_KEYSET_SIZE istruzione è maggiore di 0 e minore del valore specificato con l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta la conversione specificata dalla combinazione di *targetType* in **SQLBindCol** e del tipo di dati SQL della colonna corrispondente.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati richiesto. Il periodo di timeout viene impostato tramite SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLFetch** restituisce il set di righe successivo nel set di risultati. Può essere chiamato solo quando esiste un set di risultati, ovvero dopo una chiamata che crea un set di risultati e prima che il cursore su tale set di risultati venga chiuso. Se vengono associate colonne, vengono restituiti i dati in tali colonne. Se l'applicazione ha specificato un puntatore a una matrice di stato della riga o a un buffer in cui restituire il numero di righe recuperate, **SQLFetch** restituisce anche queste informazioni. Le chiamate a **SQLFetch** possono essere combinate con chiamate a **SQLFetchScroll** , ma non possono essere combinate con chiamate a **SQLExtendedFetch**. Per ulteriori informazioni, vedere [recupero di una riga di dati](../../../odbc/reference/develop-app/fetching-a-row-of-data.md).  
  
 Se un'applicazione ODBC 3 *. x* funziona con un driver ODBC *2. x* , gestione driver esegue il mapping delle chiamate **SQLFetch** a **SQLExtendedFetch** per un driver ODBC 2 *. x* che supporta **SQLExtendedFetch**. Se il driver ODBC 2 *. x* non supporta **SQLExtendedFetch**, gestione driver esegue il mapping delle chiamate **SQLFetch** a **SQLFetch** nel driver ODBC 2 *. x* , che può recuperare solo una singola riga.  
  
 Per ulteriori informazioni, vedere [cursori a blocchi, cursori scorrevoli e compatibilità con le versioni precedenti](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) in Appendice G: linee guida per la compatibilità con le versioni precedenti.  
  
## <a name="positioning-the-cursor"></a>Posizionamento del cursore  
 Quando viene creato il set di risultati, il cursore viene posizionato prima dell'inizio del set di risultati. **SQLFetch** Recupera il set di righe successivo. Equivale a chiamare **SQLFetchScroll** con *FetchOrientation* impostato su SQL_FETCH_NEXT. Per ulteriori informazioni sui cursori, vedere [cursori](../../../odbc/reference/develop-app/cursors.md) e [cursori a blocchi](../../../odbc/reference/develop-app/block-cursors.md).  
  
 L'attributo SQL_ATTR_ROW_ARRAY_SIZE Statement specifica il numero di righe nel set di righe. Se il set di righe recuperato da **SQLFetch** si sovrappone alla fine del set di risultati, **SQLFetch** restituisce un set di righe parziale. Ovvero, se S + R-1 è maggiore di L, dove S è la riga iniziale del set di righe da recuperare, R è la dimensione del set di righe e L è l'ultima riga nel set di risultati, quindi sono valide solo le prime righe L-S + 1 del set di righe. Le righe rimanenti sono vuote e hanno lo stato SQL_ROW_NOROW.  
  
 Dopo la restituzione di **SQLFetch** , la riga corrente è la prima riga del set di righe.  
  
 Le regole elencate nella tabella seguente descrivono il posizionamento del cursore dopo una chiamata a **SQLFetch**, in base alle condizioni elencate nella seconda tabella in questa sezione.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|Prima dell'avvio|1|  
| \< CurrRowsetStart =  *LastResultRow-RowsetSize*[1]|*CurrRowsetStart*  +  *RowsetSize*[2]|  
|*CurrRowsetStart*  >  *LastResultRow-RowsetSize*[1]|Dopo la fine|  
|Dopo la fine|Dopo la fine|  
  
 [1] se le dimensioni del set di righe vengono modificate tra le operazioni di recupero, si tratta della dimensione del set di righe utilizzata con il recupero precedente.  
  
 [2] se le dimensioni del set di righe vengono modificate tra le operazioni di recupero, si tratta della dimensione del set di righe utilizzata con la nuova operazione di recupero.  
  
|Notation|Significato|  
|--------------|-------------|  
|Prima dell'avvio|Il cursore a blocchi è posizionato prima dell'inizio del set di risultati. Se la prima riga del nuovo set di righe precede l'inizio del set di risultati, **SQLFetch** restituisce SQL_NO_DATA.|  
|Dopo la fine|Il cursore a blocchi viene posizionato dopo la fine del set di risultati. Se la prima riga del nuovo set di righe è successiva alla fine del set di risultati, **SQLFetch** restituisce SQL_NO_DATA.|  
|*CurrRowsetStart*|Il numero della prima riga nel set di righe corrente.|  
|*LastResultRow*|Numero dell'ultima riga nel set di risultati.|  
|*RowsetSize*|Dimensioni del set di righe.|  
  
 Si supponga, ad esempio, che un set di risultati includa 100 righe e che la dimensione del set di righe sia 5. Nella tabella seguente vengono illustrati il set di righe e il codice restituito restituiti da **SQLFetch** per posizioni di inizio diverse.  
  
|Set di righe corrente|Codice restituito|Nuovo set di righe|numero di righe recuperate|  
|--------------------|-----------------|----------------|------------------------|  
|Prima dell'avvio|SQL_SUCCESS|da 1 a 5|5|  
|da 1 a 5|SQL_SUCCESS|da 6 a 10|5|  
|da 52 a 56|SQL_SUCCESS|da 57 a 61|5|  
|da 91 a 95|SQL_SUCCESS|da 96 a 100|5|  
|da 93 a 97|SQL_SUCCESS|da 98 a 100. Le righe 4 e 5 della matrice di stato della riga sono impostate su SQL_ROW_NOROW.|3|  
|da 96 a 100|SQL_NO_DATA|Nessuna.|0|  
|da 99 a 100|SQL_NO_DATA|Nessuna.|0|  
|Dopo la fine|SQL_NO_DATA|Nessuna.|0|  
  
## <a name="returning-data-in-bound-columns"></a>Restituzione di dati nelle colonne con binding  
 Poiché **SQLFetch** restituisce ogni riga, inserisce i dati per ogni colonna associata nel buffer associato a tale colonna. Se non è associata alcuna colonna, **SQLFetch** non restituisce dati, ma sposta il cursore del blocco avanti. È comunque possibile recuperare i dati utilizzando **SQLGetData**. Se il cursore è un cursore più righe (ovvero, il SQL_ATTR_ROW_ARRAY_SIZE è maggiore di 1), **SQLGetData** può essere chiamato solo se viene restituito SQL_GD_BLOCK quando **SQLGetInfo** viene chiamato con un *InfoType* di SQL_GETDATA_EXTENSIONS. Per ulteriori informazioni, vedere [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
 Per ogni colonna associata in una riga, **SQLFetch** esegue le operazioni seguenti:  
  
1.  Imposta il buffer di lunghezza/indicatore su SQL_NULL_DATA e procede alla successiva colonna se i dati sono NULL. Se i dati sono NULL e non è stato associato alcun buffer di lunghezza/indicatore, **SQLFetch** restituisce SQLState 22002 (variabile indicatore obbligatoria ma non fornita) per la riga e procede alla riga successiva. Per informazioni su come determinare l'indirizzo del buffer di lunghezza/indicatore, vedere "indirizzi del buffer" in [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
     Se i dati della colonna non sono NULL, **SQLFetch** procede al passaggio 2.  
  
2.  Se l'attributo dell'istruzione SQL_ATTR_MAX_LENGTH è impostato su un valore diverso da zero e la colonna contiene dati di tipo carattere o binario, i dati vengono troncati in SQL_ATTR_MAX_LENGTH byte.  
  
    > [!NOTE]  
    >  L'attributo SQL_ATTR_MAX_LENGTH Statement è progettato per ridurre il traffico di rete. Viene in genere implementato dall'origine dati, che tronca i dati prima di restituirli sulla rete. I driver e le origini dati non sono necessari per supportarla. Pertanto, per garantire che i dati vengano troncati a una determinata dimensione, un'applicazione deve allocare un buffer di tale dimensione e specificare le dimensioni nell'argomento *cbValueMax* in **SQLBindCol**.  
  
3.  Converte i dati nel tipo specificato da *targetType* in **SQLBindCol**.  
  
4.  Se i dati sono stati convertiti in un tipo di dati a lunghezza variabile, ad esempio carattere o binario, **SQLFetch** controlla se la lunghezza dei dati supera la lunghezza del buffer di dati. Se la lunghezza dei dati di tipo carattere, incluso il carattere di terminazione null, supera la lunghezza del buffer di dati, **SQLFetch** tronca i dati alla lunghezza del buffer di dati meno la lunghezza di un carattere di terminazione null. Quindi, termina i dati null. Se la lunghezza dei dati binari supera la lunghezza del buffer di dati, **SQLFetch** lo tronca alla lunghezza del buffer di dati. La lunghezza del buffer di dati viene specificata con *bufferLength* in **SQLBindCol**.  
  
     **SQLFetch** non tronca mai i dati convertiti in tipi di dati a lunghezza fissa; si presuppone sempre che la lunghezza del buffer di dati sia la dimensione del tipo di dati.  
  
5.  Inserisce i dati convertiti (e possibilmente troncati) nel buffer di dati. Per informazioni su come determinare l'indirizzo del buffer di dati, vedere "indirizzi del buffer" in [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
6.  Inserisce la lunghezza dei dati nel buffer di lunghezza/indicatore. Se il puntatore dell'indicatore e il puntatore di lunghezza sono entrambi impostati sullo stesso buffer (come una chiamata a **SQLBindCol** ), la lunghezza viene scritta nel buffer per i dati validi e SQL_NULL_DATA viene scritta nel buffer per i dati null. Se non è stato associato alcun buffer di lunghezza/indicatore, **SQLFetch** non restituisce la lunghezza.  
  
    -   Per i dati di tipo carattere o binario, questa è la lunghezza dei dati dopo la conversione e prima del troncamento perché il buffer dei dati è troppo piccolo. Se il driver non è in grado di determinare la lunghezza dei dati dopo la conversione, come accade talvolta con i dati Long, imposta la lunghezza su SQL_NO_TOTAL. Se i dati sono stati troncati a causa dell'attributo SQL_ATTR_MAX_LENGTH Statement, il valore di questo attributo viene inserito nel buffer di lunghezza/indicatore anziché la lunghezza effettiva. Questo è dovuto al fatto che questo attributo è progettato per troncare i dati nel server prima della conversione, in modo che il driver non abbia alcun modo per determinare la lunghezza effettiva.  
  
    -   Per tutti gli altri tipi di dati, si tratta della lunghezza dei dati dopo la conversione. ovvero la dimensione del tipo in cui sono stati convertiti i dati.  
  
     Per informazioni su come determinare l'indirizzo del buffer di lunghezza/indicatore, vedere "indirizzi del buffer" in [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
7.  Se i dati vengono troncati durante la conversione senza perdita di cifre significative (ad esempio, il numero reale 1,234 viene troncato al numero intero 1 quando viene convertito), **SQLFetch** restituisce SQLSTATE 01S07 (troncamento frazionario) e SQL_SUCCESS_WITH_INFO. Se i dati vengono troncati perché la lunghezza del buffer dei dati è troppo piccola (ad esempio, la stringa "abcdef" viene inserita in un buffer a 4 byte), **SQLFetch** restituisce SQLSTATE 01004 (dati troncati) e SQL_SUCCESS_WITH_INFO. Se i dati vengono troncati a causa dell'attributo SQL_ATTR_MAX_LENGTH Statement, **SQLFetch** restituisce SQL_SUCCESS e non restituisce SQLSTATE 01S07 (troncamento frazionario) o SQLSTATE 01004 (dati troncati). Se i dati vengono troncati durante la conversione con una perdita di cifre significative (ad esempio, se un valore SQL_INTEGER maggiore di 100.000 è stato convertito in un SQL_C_TINYINT), **SQLFetch** restituisce SQLSTATE 22003 (valore numerico non compreso nell'intervallo) e SQL_ERROR (se le dimensioni del set di righe sono pari a 1) o SQL_SUCCESS_WITH_INFO (se le dimensioni del set di righe sono maggiori di 1).  
  
 Il contenuto del buffer dei dati associato e il buffer di lunghezza/indicatore non sono definiti se **SQLFetch** o **SQLFetchScroll** non restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO.  
  
## <a name="row-status-array"></a>Matrice di stato riga  
 La matrice di stato della riga viene utilizzata per restituire lo stato di ogni riga nel set di righe. L'indirizzo di questa matrice viene specificato con l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR. La matrice viene allocata dall'applicazione e deve avere tutti gli elementi specificati dall'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE. I relativi valori vengono impostati da **SQLFetch**, **SQLFetchScroll** e **SQLBulkOperations** o **SQLSetPos** (tranne quando sono stati chiamati dopo che il cursore è stato posizionato da **SQLExtendedFetch**). Se il valore dell'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR è un puntatore null, queste funzioni non restituiscono lo stato della riga.  
  
 Il contenuto del buffer della matrice di stato della riga non è definito se **SQLFetch** o **SQLFetchScroll** non restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO.  
  
 Nella matrice di stato della riga vengono restituiti i valori seguenti.  
  
|Valore matrice stato riga|Descrizione|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|La riga è stata recuperata e non è stata modificata dall'ultimo recupero del set di risultati.|  
|SQL_ROW_SUCCESS_WITH_INFO|La riga è stata recuperata e non è stata modificata dall'ultimo recupero del set di risultati. Tuttavia, è stato restituito un avviso sulla riga.|  
|SQL_ROW_ERROR|Si è verificato un errore durante il recupero della riga.|  
|SQL_ROW_UPDATED [1], [2] e [3]|La riga è stata recuperata ed è stata modificata dall'ultimo recupero del set di risultati. Se la riga viene recuperata di nuovo da questo set di risultati o viene aggiornata da **SQLSetPos**, lo stato viene impostato sul nuovo stato della riga.|  
|SQL_ROW_DELETED [3]|La riga è stata eliminata dall'ultimo recupero del set di risultati.|  
|SQL_ROW_ADDED [4]|La riga è stata inserita da **SQLBulkOperations**. Se la riga viene recuperata di nuovo da questo set di risultati o viene aggiornata da **SQLSetPos**, il relativo stato viene SQL_ROW_SUCCESS.|  
|SQL_ROW_NOROW|Il set di righe ha sovrapposto la fine del set di risultati e non è stata restituita alcuna riga corrispondente a questo elemento della matrice di stato della riga.|  
  
 [1] per i cursori keyset, misti e dinamici, se viene aggiornato un valore di chiave, la riga di dati viene considerata eliminata e viene aggiunta una nuova riga.  
  
 [2] alcuni driver non sono in grado di rilevare gli aggiornamenti ai dati e pertanto non possono restituire questo valore. Per determinare se un driver è in grado di rilevare gli aggiornamenti alle righe recuperate, un'applicazione chiama **SQLGetInfo** con l'opzione SQL_ROW_UPDATES.  
  
 [3]   **SQLFetch** può restituire questo valore solo quando è combinato con chiamate a **SQLFetchScroll**. Questo perché **SQLFetch** si sposta nel set di risultati e quando viene utilizzato in modo esclusivo, non recupera alcuna riga. Poiché non viene recuperata alcuna riga, **SQLFetch** non rileva le modifiche apportate alle righe recuperate in precedenza. Tuttavia, se **SQLFetchScroll** posiziona il cursore prima che vengano utilizzate righe recuperate in precedenza e **SQLFetch** per recuperare tali righe, **SQLFetch** può rilevare eventuali modifiche apportate a tali righe.  
  
 [4] restituito da SQLBulkOperations. Non impostato da **SQLFetch** o **SQLFetchScroll**.  
  
### <a name="rows-fetched-buffer"></a>Buffer recuperato righe  
 Il buffer recuperato dalle righe viene utilizzato per restituire il numero di righe recuperate, incluse le righe per le quali non è stato restituito alcun dato perché si è verificato un errore durante il recupero. In altre parole, è il numero di righe per cui il valore nella matrice di stato della riga non è SQL_ROW_NOROW. L'indirizzo di questo buffer viene specificato con l'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR. Il buffer viene allocato dall'applicazione. Viene impostato da **SQLFetch** e **SQLFetchScroll**. Se il valore dell'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR è un puntatore null, queste funzioni non restituiscono il numero di righe recuperate. Per determinare il numero della riga corrente nel set di risultati, un'applicazione può chiamare **SQLGetStmtAttr** con l'attributo SQL_ATTR_ROW_NUMBER.  
  
 Il contenuto del buffer recuperato dalle righe non è definito se **SQLFetch** o **SQLFetchScroll** non restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, tranne quando viene restituito SQL_NO_DATA, nel qual caso il valore nel buffer recuperato dalle righe viene impostato su 0.  
  
### <a name="error-handling"></a>Gestione degli errori  
 Gli errori e gli avvisi possono essere applicati a singole righe o all'intera funzione. Per ulteriori informazioni sui record di diagnostica, vedere [Diagnostics](../../../odbc/reference/develop-app/diagnostics.md) and [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
#### <a name="errors-and-warnings-on-the-entire-function"></a>Errori e avvisi sull'intera funzione  
 Se si verifica un errore per l'intera funzione, ad esempio SQLSTATE HYT00 (timeout scaduto) o SQLSTATE 24000 (stato del cursore non valido), **SQLFetch** restituisce SQL_ERROR e il valore SQLSTATE applicabile. Il contenuto dei buffer del set di righe non è definito e la posizione del cursore è invariata.  
  
 Se viene visualizzato un avviso per l'intera funzione, **SQLFetch** restituisce SQL_SUCCESS_WITH_INFO e il valore SQLSTATE applicabile. I record di stato per gli avvisi che si applicano all'intera funzione vengono restituiti prima dei record di stato che si applicano a singole righe.  
  
#### <a name="errors-and-warnings-in-individual-rows"></a>Errori e avvisi in singole righe  
 Se un errore, ad esempio SQLSTATE 22012 (divisione per zero), o un avviso, ad esempio SQLSTATE 01004 (dati troncati), si applica a una singola riga, **SQLFetch** esegue le operazioni seguenti:  
  
-   Imposta l'elemento corrispondente della matrice di stato della riga su SQL_ROW_ERROR per errori o SQL_ROW_SUCCESS_WITH_INFO per gli avvisi.  
  
-   Aggiunge zero o più record di stato che contengono SQLSTATE per l'errore o l'avviso.  
  
-   Imposta i campi numero di riga e colonna nei record di stato. Se **SQLFetch** non è in grado di determinare un numero di riga o di colonna, imposta tale numero rispettivamente su SQL_ROW_NUMBER_UNKNOWN o SQL_COLUMN_NUMBER_UNKNOWN. Se il record di stato non si applica a una colonna specifica, **SQLFetch** imposta il numero di colonna su SQL_NO_COLUMN_NUMBER.  
  
 **SQLFetch** continua a recuperare le righe fino a recuperare tutte le righe nel set di righe. Restituisce SQL_SUCCESS_WITH_INFO a meno che non si verifichi un errore in ogni riga del set di righe (escluse le righe con stato SQL_ROW_NOROW), nel qual caso restituisce SQL_ERROR. In particolare, se le dimensioni del set di righe sono pari a 1 e si verifica un errore in tale riga, **SQLFetch** restituisce SQL_ERROR.  
  
 **SQLFetch** restituisce i record di stato nell'ordine dei numeri di riga. Ovvero restituisce tutti i record di stato per le eventuali righe sconosciute; restituisce quindi tutti i record di stato per la prima riga (se presente), quindi restituisce tutti i record di stato per la seconda riga (se presente) e così via. I record di stato per ogni riga vengono ordinati in base alle normali regole per l'ordinamento dei record di stato; Per ulteriori informazioni, vedere la sezione relativa alla sequenza di record di stato in [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
### <a name="descriptors-and-sqlfetch"></a>Descrittori e SQLFetch  
 Nelle sezioni seguenti viene descritto il modo in cui **SQLFetch** interagisce con i descrittori.  
  
#### <a name="argument-mappings"></a>Mapping degli argomenti  
 Il driver non imposta alcun campo del descrittore in base agli argomenti di **SQLFetch**.  
  
#### <a name="other-descriptor-fields"></a>Altri campi del descrittore  
 I campi di descrizione seguenti vengono utilizzati da **SQLFetch**.  
  
|Campo di descrizione|DESC.|Campo in|Imposta tramite|  
|----------------------|-----------|--------------|-----------------|  
|SQL_DESC_ARRAY_SIZE|ARD|header|Attributo SQL_ATTR_ROW_ARRAY_SIZE Statement|  
|SQL_DESC_ARRAY_STATUS_PTR|IRD|header|Attributo SQL_ATTR_ROW_STATUS_PTR Statement|  
|SQL_DESC_BIND_OFFSET_PTR|ARD|header|Attributo SQL_ATTR_ROW_BIND_OFFSET_PTR Statement|  
|SQL_DESC_BIND_TYPE|ARD|header|Attributo SQL_ATTR_ROW_BIND_TYPE Statement|  
|SQL_DESC_COUNT|ARD|header|Argomento *ColumnNumber* di **SQLBindCol**|  
|SQL_DESC_DATA_PTR|ARD|record|Argomento *TargetValuePtr* di **SQLBindCol**|  
|SQL_DESC_INDICATOR_PTR|ARD|record|Argomento *StrLen_or_IndPtr* in **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH|ARD|record|Argomento *bufferLength* in **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH_PTR|ARD|record|Argomento *StrLen_or_IndPtr* in **SQLBindCol**|  
|SQL_DESC_ROWS_PROCESSED_PTR|IRD|header|Attributo SQL_ATTR_ROWS_FETCHED_PTR Statement|  
|SQL_DESC_TYPE|ARD|record|Argomento *targetType* in **SQLBindCol**|  
  
 Tutti i campi di descrizione possono essere impostati anche tramite **SQLSetDescField**.  
  
#### <a name="separate-length-and-indicator-buffers"></a>Buffer di lunghezza e indicatore distinti  
 Le applicazioni possono associare un singolo buffer o due buffer distinti che possono essere utilizzati per mantenere i valori di lunghezza e indicatore. Quando un'applicazione chiama **SQLBindCol**, il driver imposta i campi SQL_DESC_OCTET_LENGTH_PTR e SQL_DESC_INDICATOR_PTR di Ard sullo stesso indirizzo, che viene passato nell'argomento *StrLen_or_IndPtr* . Quando un'applicazione chiama **SQLSetDescField** o **SQLSetDescRec**, può impostare questi due campi su indirizzi diversi.  
  
 **SQLFetch** determina se l'applicazione ha specificato buffer di lunghezza e indicatore distinti. In questo caso, quando i dati non sono NULL, **SQLFetch** imposta il buffer dell'indicatore su 0 e restituisce la lunghezza nel buffer di lunghezza. Quando i dati sono NULL, **SQLFetch** imposta il buffer dell'indicatore su SQL_NULL_DATA e non modifica il buffer di lunghezza.  
  
### <a name="code-example"></a>Esempio di codice  
 Vedere [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)e [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md).  
  
### <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su una colonna in un set di risultati|[Funzione SQLDescribeCol ](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Chiusura del cursore sull'istruzione|[SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Recupero di parte o di tutte le colonne di dati|[Funzione SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Restituzione del numero di colonne del set di risultati|[Funzione SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Preparazione di un'istruzione per l'esecuzione|[Pagina relativa alla funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
