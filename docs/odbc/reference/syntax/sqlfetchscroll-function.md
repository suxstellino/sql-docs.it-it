---
description: Funzione SQLFetchScroll
title: Funzione SQLFetchScroll | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetchScroll
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetchScroll
helpviewer_keywords:
- SQLFetchScroll function [ODBC]
ms.assetid: c0243667-428c-4dda-ae91-3c307616a1ac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cfa442ab1ada0b17e06137257b4d17bf37f854c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198541"
---
# <a name="sqlfetchscroll-function"></a>Funzione SQLFetchScroll
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLFetchScroll** Recupera il set di righe specificato di dati dal set di risultati e restituisce i dati per tutte le colonne con binding. I set di righe possono essere specificati in una posizione assoluta o relativa o tramite segnalibro.  
  
 Quando si utilizza un driver ODBC 2. x, gestione driver esegue il mapping di questa funzione a **SQLExtendedFetch**. Per ulteriori informazioni, vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLFetchScroll(  
      SQLHSTMT      StatementHandle,  
      SQLSMALLINT   FetchOrientation,  
      SQLLEN        FetchOffset);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *FetchOrientation*  
 Input  
  
 Tipo di recupero:  
  
 SQL_FETCH_NEXT  
  
 SQL_FETCH_PRIOR  
  
 SQL_FETCH_FIRST  
  
 SQL_FETCH_LAST  
  
 SQL_FETCH_ABSOLUTE  
  
 SQL_FETCH_RELATIVE  
  
 SQL_FETCH_BOOKMARK  
  
 Per ulteriori informazioni, vedere "posizionamento del cursore" nella sezione "Commenti".  
  
 *FetchOffset*  
 Input  
  
 Numero della riga da recuperare. L'interpretazione di questo argomento dipende dal valore dell'argomento *FetchOrientation* . Per ulteriori informazioni, vedere "posizionamento del cursore" nella sezione "Commenti".  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLFetchScroll** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con HandleType di SQL_HANDLE_STMT e un handle di statementHandle. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLFetchScroll** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente. Se si verifica un errore in una singola colonna, è possibile chiamare **SQLGetDiagField** con un DiagIdentifier di SQL_DIAG_COLUMN_NUMBER per determinare la colonna in cui si è verificato l'errore. è possibile chiamare e **SQLGetDiagField** con un DiagIdentifier di SQL_DIAG_ROW_NUMBER per determinare la riga contenente la colonna.  
  
 Per tutti questi SQLSTATE che possono restituire SQL_SUCCESS_WITH_INFO o SQL_ERROR (ad eccezione di sql01xxxs), viene restituito SQL_SUCCESS_WITH_INFO se si verifica un errore in una o più righe, ma non in tutte, righe di un'operazione più righe e viene restituito SQL_ERROR se si verifica un errore in un'operazione a riga singola.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati di tipo stringa o binario restituiti per una colonna hanno causato il troncamento di dati di tipo carattere non vuoto o binari non NULL. Se si trattasse di un valore stringa, viene troncato a destra.|  
|01S01|Errore nella riga|Si è verificato un errore durante il recupero di una o più righe.<br /><br /> Se questo valore SQLSTATE viene restituito quando un'applicazione ODBC 3 *. x* utilizza un driver ODBC 2 *. x* , è possibile ignorarla.|  
|01S06|Tentativo di recupero prima che il set di risultati restituisse il primo set di righe|Il set di righe richiesto ha sovrapposto l'inizio del set di risultati quando FetchOrientation è stato SQL_FETCH_PRIOR, la posizione corrente supera la prima riga e il numero della riga corrente è minore o uguale alla dimensione del set di righe.<br /><br /> Il set di righe richiesto ha sovrapposto l'inizio del set di risultati quando FetchOrientation è stato SQL_FETCH_PRIOR, la posizione corrente supera la fine del set di risultati e le dimensioni del set di righe sono maggiori delle dimensioni del set di risultati.<br /><br /> Il set di righe richiesto ha sovrapposto l'inizio del set di risultati quando FetchOrientation è stato SQL_FETCH_RELATIVE, FetchOffset è negativo e il valore assoluto di FetchOffset è minore o uguale alla dimensione del set di righe.<br /><br /> Il set di righe richiesto ha sovrapposto l'inizio del set di risultati quando FetchOrientation è stato SQL_FETCH_ABSOLUTE, FetchOffset è negativo e il valore assoluto di FetchOffset è maggiore della dimensione del set di risultati, ma minore o uguale alla dimensione del set di righe.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S07|Troncamento frazionario|I dati restituiti per una colonna sono stati troncati. Per i tipi di dati numerici, la parte frazionaria del numero è stata troncata. Per i tipi di dati time, timestamp e Interval contenenti un componente ora, la parte frazionaria del tempo è stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Impossibile convertire il valore dei dati di una colonna nel set di risultati nel tipo di dati specificato da *targetType* in **SQLBindCol**.<br /><br /> La colonna 0 è stata associata a un tipo di dati di SQL_C_BOOKMARK e l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è stato impostato su SQL_UB_VARIABLE.<br /><br /> La colonna 0 è stata associata a un tipo di dati di SQL_C_VARBOOKMARK e l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS non è stato impostato su SQL_UB_VARIABLE.|  
|07009|Indice del descrittore non valido|Il driver era un driver ODBC 2 *. x* che non supporta **SQLExtendedFetch** e il numero di colonna specificato nell'associazione per una colonna era 0.<br /><br /> La colonna 0 è stata associata e l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS è stato impostato su SQL_UB_OFF.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22001|Dati stringa, troncati a destra|Un segnalibro a lunghezza variabile restituito per una colonna è stato troncato.|  
|22002|Variabile indicatore obbligatoria ma non fornita|Sono stati recuperati dati NULL in una colonna la cui *StrLen_or_IndPtr* impostata da **SQLBindCol** (o SQL_DESC_INDICATOR_PTR impostata da **SQLSetDescField** o **SQLSetDescRec**) è un puntatore null.|  
|22003|Valore numerico non compreso nell'intervallo|La restituzione del valore numerico (come valore numerico o stringa) per una o più colonne con binding avrebbe causato il troncamento dell'intero oggetto, invece della parte frazionaria del numero.<br /><br /> Per ulteriori informazioni, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) in [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Formato DateTime non valido|Una colonna di tipo carattere nel set di risultati è stata associata a una struttura di data, ora o timestamp e un valore nella colonna era, rispettivamente, una data, un'ora o un timestamp non valido.|  
|22012|Divisione per zero|È stato restituito un valore da un'espressione aritmetica, che ha comportato la divisione per zero.|  
|22015|Overflow del campo Interval|L'assegnazione da un tipo SQL esatto o intervallo di tipo SQL a un tipo intervallo C ha causato la perdita di cifre significative nel campo iniziali.<br /><br /> Quando si recuperano i dati in un tipo intervallo C, non esiste alcuna rappresentazione del valore del tipo SQL nel tipo intervallo C.|  
|22018|Valore di carattere non valido per la specifica del cast|Una colonna di tipo carattere nel set di risultati è stata associata a un buffer C di caratteri e la colonna conteneva un carattere per il quale non era presente alcuna rappresentazione nel set di caratteri del buffer.<br /><br /> Il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. e il valore nella colonna non è un valore letterale valido del tipo C associato.|  
|24000|Stato del cursore non valido|Lo stato di *statementHandle* è stato eseguito ma nessun set di risultati è stato associato a *statementHandle*.|  
|40001|Errore di serializzazione|La transazione in cui è stata eseguita la lettura è stata terminata per impedire il deadlock.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLFetchScroll** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) il *statementHandle* specificato non si trova in uno stato eseguito. La funzione è stata chiamata senza prima chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) **SQLFetch** è stato chiamato per *statementHandle* dopo la chiamata di **SQLExtendedFetch** e prima della chiamata a **SQLFreeStmt** con l'opzione SQL_CLOSE.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|L'attributo SQL_ATTR_USE_BOOKMARK Statement è stato impostato su SQL_UB_VARIABLE e la colonna 0 è stata associata a un buffer la cui lunghezza non è uguale alla lunghezza massima per il segnalibro per questo set di risultati. Questa lunghezza è disponibile nel campo SQL_DESC_OCTET_LENGTH di IRD e può essere ottenuta chiamando **SQLDescribeCol**, **SQLColAttribute** o **SQLGetDescField**.|  
|HY106|Tipo di recupero non compreso nell'intervallo|DM) il valore specificato per l'argomento FetchOrientation non è valido.<br /><br /> (DM) l'argomento FetchOrientation è stato SQL_FETCH_BOOKMARK e l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è stato impostato su SQL_UB_OFF.<br /><br /> Il valore dell'attributo dell'istruzione SQL_ATTR_CURSOR_TYPE è stato SQL_CURSOR_FORWARD_ONLY e il valore dell'argomento FetchOrientation non è stato SQL_FETCH_NEXT.<br /><br /> Il valore dell'attributo dell'istruzione SQL_ATTR_CURSOR_SCROLLABLE è stato SQL_NONSCROLLABLE e il valore dell'argomento FetchOrientation non è stato SQL_FETCH_NEXT.|  
|HY107|Valore di riga non compreso nell'intervallo|Il valore specificato con l'attributo dell'istruzione SQL_ATTR_CURSOR_TYPE è stato SQL_CURSOR_KEYSET_DRIVEN, ma il valore specificato con l'attributo SQL_ATTR_KEYSET_SIZE istruzione è maggiore di 0 e minore del valore specificato con l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY111|Valore segnalibro non valido|L'argomento FetchOrientation è stato SQL_FETCH_BOOKMARK e il segnalibro a cui fa riferimento il valore nell'attributo SQL_ATTR_FETCH_BOOKMARK_PTR istruzione non è valido o è un puntatore null.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta la conversione specificata dalla combinazione di *targetType* in **SQLBindCol** e del tipo di dati SQL della colonna corrispondente.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati richiesto. Il periodo di timeout viene impostato tramite SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLFetchScroll** restituisce un set di righe specificato dal set di risultati. I set di righe possono essere specificati in base alla posizione assoluta o relativa o al segnalibro. **SQLFetchScroll** può essere chiamato solo quando esiste un set di risultati, ovvero dopo una chiamata che crea un set di risultati e prima che il cursore su tale set di risultati venga chiuso. Se vengono associate colonne, vengono restituiti i dati in tali colonne. Se l'applicazione ha specificato un puntatore a una matrice di stato della riga o a un buffer in cui restituire il numero di righe recuperate, **SQLFetchScroll** restituisce anche queste informazioni. Le chiamate a **SQLFetchScroll** possono essere combinate con chiamate a **SQLFetch** ma non possono essere combinate con chiamate a **SQLExtendedFetch**.  
  
 Per ulteriori informazioni, vedere [utilizzo di cursori a blocchi](../../../odbc/reference/develop-app/using-block-cursors.md) e [utilizzo di cursori scorrevoli](../../../odbc/reference/develop-app/using-scrollable-cursors.md).  
  
## <a name="positioning-the-cursor"></a>Posizionamento del cursore  
 Quando viene creato il set di risultati, il cursore viene posizionato prima dell'inizio del set di risultati. **SQLFetchScroll** posiziona il cursore del blocco in base ai valori degli argomenti *FetchOrientation* e *FetchOffset* , come illustrato nella tabella seguente. Le regole esatte per determinare l'inizio del nuovo set di righe sono illustrate nella sezione successiva.  
  
|FetchOrientation|Significato|  
|----------------------|-------------|  
|SQL_FETCH_NEXT|Restituisce il set di righe successivo. Equivale alla chiamata a **SQLFetch**.<br /><br /> **SQLFetchScroll** ignora il valore di *FetchOffset*.|  
|SQL_FETCH_PRIOR|Restituisce il set di righe precedente.<br /><br /> **SQLFetchScroll** ignora il valore di *FetchOffset*.|  
|SQL_FETCH_RELATIVE|Restituisce il set di righe *FetchOffset* dall'inizio del set di righe corrente.|  
|SQL_FETCH_ABSOLUTE|Restituisce il set di righe a partire dalla riga *FetchOffset*.|  
|SQL_FETCH_FIRST|Restituisce il primo set di righe nel set di risultati.<br /><br /> **SQLFetchScroll** ignora il valore di *FetchOffset*.|  
|SQL_FETCH_LAST|Restituisce l'ultimo set di righe completo nel set di risultati.<br /><br /> **SQLFetchScroll** ignora il valore di *FetchOffset*.|  
|SQL_FETCH_BOOKMARK|Restituisce il set di righe FetchOffset righe dal segnalibro specificato dall'attributo dell'istruzione SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
 I driver non sono necessari per supportare tutti gli orientamenti di recupero; un'applicazione chiama **SQLGetInfo** con un tipo di informazioni SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 o SQL_STATIC_CURSOR_ATTRIBUTES1 (a seconda del tipo di cursore) per determinare gli orientamenti di recupero supportati dal driver. L'applicazione deve esaminare le maschere di SQL_CA1_NEXT, SQL_CA1_RELATIVE, SQL_CA1_ABSOLUTE e WQL_CA1_BOOKMARK in questi tipi di informazioni. Inoltre, se il cursore è di tipo "inoltr" e FetchOrientation non è SQL_FETCH_NEXT, **SQLFetchScroll** restituisce SQLSTATE HY106 (tipo di recupero non compreso nell'intervallo).  
  
 L'attributo SQL_ATTR_ROW_ARRAY_SIZE Statement specifica il numero di righe nel set di righe. Se il set di righe recuperato da **SQLFetchScroll** si sovrappone alla fine del set di risultati, **SQLFetchScroll** restituisce un set di righe parziale. Ovvero, se S + R-1 è maggiore di L, dove S è la riga iniziale del set di righe da recuperare, R è la dimensione del set di righe e L è l'ultima riga nel set di risultati, quindi sono valide solo le prime righe L-S + 1 del set di righe. Le righe rimanenti sono vuote e hanno lo stato SQL_ROW_NOROW.  
  
 Dopo la restituzione di **SQLFetchScroll** , la riga corrente è la prima riga del set di righe.  
  
## <a name="cursor-positioning-rules"></a>Regole di posizionamento del cursore  
 Le sezioni seguenti descrivono le regole esatte per ogni valore di FetchOrientation. Queste regole utilizzano la notazione seguente:  
  
|Notation|Significato|  
|--------------|-------------|  
|*Prima dell'avvio*|Il cursore a blocchi è posizionato prima dell'inizio del set di risultati. Se la prima riga del nuovo set di righe precede l'inizio del set di risultati, **SQLFetchScroll** restituisce SQL_NO_DATA.|  
|*Dopo la fine*|Il cursore a blocchi viene posizionato dopo la fine del set di risultati. Se la prima riga del nuovo set di righe è successiva alla fine del set di risultati, **SQLFetchScroll** restituisce SQL_NO_DATA.|  
|*CurrRowsetStart*|Il numero della prima riga nel set di righe corrente.|  
|*LastResultRow*|Numero dell'ultima riga nel set di risultati.|  
|*RowsetSize*|Dimensioni del set di righe.|  
|*FetchOffset*|Valore dell'argomento *FetchOffset* .|  
|*BookmarkRow*|Riga corrispondente al segnalibro specificato dall'attributo dell'istruzione SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
## <a name="sql_fetch_next"></a>SQL_FETCH_NEXT  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*Prima dell'avvio*|1|  
|*CurrRowsetStart + RowsetSize*[1] *\< = LastResultRow*|*CurrRowsetStart + RowsetSize*[1]|  
|*CurrRowsetStart + RowsetSize*[1]*> LastResultRow*|*Dopo la fine*|  
|*Dopo la fine*|*Dopo la fine*|  
  
 [1] se la dimensione del set di righe è stata modificata rispetto alla precedente chiamata a fetch Rows, si tratta della dimensione del set di righe utilizzata con la chiamata precedente.  
  
## <a name="sql_fetch_prior"></a>SQL_FETCH_PRIOR  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*Prima dell'avvio*|*Prima dell'avvio*|  
|*CurrRowsetStart = 1*|*Prima dell'avvio*|  
|*1 < CurrRowsetStart <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*CurrRowsetStart > RowsetSize* <sup>[2]</sup>|*CurrRowsetStart-RowsetSize* <sup>[2]</sup>|  
|*After end e LastResultRow < RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*After end e LastResultRow >= RowsetSize* <sup>[2]</sup>|*LastResultRow-RowsetSize + 1* <sup>[2]</sup>|  
  
 [1]   **SQLFetchScroll** restituisce SQLSTATE 01S06 (tentativo di recupero prima che il set di risultati restituisse il primo set di righe) e SQL_SUCCESS_WITH_INFO.  
  
 [2] se le dimensioni del set di righe sono state modificate dopo la chiamata precedente per recuperare le righe, si tratta delle nuove dimensioni del set di righe.  
  
## <a name="sql_fetch_relative"></a>SQL_FETCH_RELATIVE  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*(Prima di iniziare e FetchOffset > 0) OPPURE (dopo end e FetchOffset < 0)*|*--*<sup>[1]</sup>|  
|*BeforeStart e FetchOffset <= 0*|*Prima dell'avvio*|  
|*CurrRowsetStart = 1 e FetchOffset < 0*|*Prima dell'avvio*|  
|*CurrRowsetStart > 1 e CurrRowsetStart + FetchOffset < 1 e &#124; FetchOffset &#124; > RowsetSize* <sup>[3]</sup>|*Prima dell'avvio*|  
|*CurrRowsetStart > 1 e CurrRowsetStart + FetchOffset < 1 e &#124; FetchOffset &#124; <= RowsetSize* <sup>[3]</sup>|*1* <sup>[2]</sup>|  
|*1 <= CurrRowsetStart + FetchOffset \< = LastResultRow*|*CurrRowsetStart + FetchOffset*|  
|*CurrRowsetStart + FetchOffset > LastResultRow*|*Dopo la fine*|  
|*After end e FetchOffset >= 0*|*Dopo la fine*|  
  
 [1] ***SQLFetchScroll** _ restituisce lo stesso set di righe come se fosse stato chiamato con FetchOrientation impostato su SQL_FETCH_ABSOLUTE. Per ulteriori informazioni, vedere la sezione "SQL_FETCH_ABSOLUTE".  
  
 [2] _ *SQLFetchScroll** restituisce SQLSTATE 01S06 (tentativo di recupero prima che il set di risultati restituisse il primo set di righe) e SQL_SUCCESS_WITH_INFO.  
  
 [3] se le dimensioni del set di righe sono state modificate dopo la chiamata precedente per recuperare le righe, si tratta delle nuove dimensioni del set di righe.  
  
## <a name="sql_fetch_absolute"></a>SQL_FETCH_ABSOLUTE  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*FetchOffset < 0 e &#124; FetchOffset &#124; <= LastResultRow*|*LastResultRow + FetchOffset + 1*|  
|*FetchOffset < 0 e &#124; FetchOffset &#124; > LASTRESULTROW e &#124; FetchOffset &#124; > RowsetSize* <sup>[2]</sup>|*Prima dell'avvio*|  
|*FetchOffset < 0 e &#124; FetchOffset &#124; > LASTRESULTROW e &#124; FetchOffset &#124; <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*FetchOffset = 0*|*Prima dell'avvio*|  
|*1 <= FetchOffset \< = LastResultRow*|*FetchOffset*|  
|*FetchOffset > LastResultRow*|*Dopo la fine*|  
  
 [1]   **SQLFetchScroll** restituisce SQLSTATE 01S06 (tentativo di recupero prima che il set di risultati restituisse il primo set di righe) e SQL_SUCCESS_WITH_INFO.  
  
 [2] se le dimensioni del set di righe sono state modificate dopo la chiamata precedente per recuperare le righe, si tratta delle nuove dimensioni del set di righe.  
  
 Un recupero assoluto eseguito su un cursore dinamico non può fornire il risultato necessario perché le posizioni delle righe in un cursore dinamico non sono determinate. Tale operazione equivale a un recupero, seguito da un relativo recupero; non si tratta di un'operazione atomica, così come un recupero assoluto su un cursore statico.  
  
## <a name="sql_fetch_first"></a>SQL_FETCH_FIRST  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*qualsiasi*|*1*|  
  
## <a name="sql_fetch_last"></a>SQL_FETCH_LAST  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*RowsetSize* <sup>[1]</sup> <= LastResultRow|*LastResultRow-RowsetSize + 1* <sup>[1]</sup>|  
|*RowsetSize* <sup>[1]</sup> > LastResultRow|*1*|  
  
 [1] se le dimensioni del set di righe sono state modificate dopo la chiamata precedente per recuperare le righe, si tratta delle nuove dimensioni del set di righe.  
  
## <a name="sql_fetch_bookmark"></a>SQL_FETCH_BOOKMARK  
 Si applicano le regole seguenti.  
  
|Condizione|Prima riga del nuovo set di righe|  
|---------------|-----------------------------|  
|*BookmarkRow + FetchOffset < 1*|*Prima dell'avvio*|  
|*1 <= BookmarkRow + FetchOffset \< = LastResultRow*|*BookmarkRow + FetchOffset*|  
|*BookmarkRow + FetchOffset > LastResultRow*|*Dopo la fine*|  
  
 Per informazioni sui segnalibri, vedere [segnalibri (ODBC)](../../../odbc/reference/develop-app/bookmarks-odbc.md).  
  
## <a name="effect-of-deleted-added-and-error-rows-on-cursor-movement"></a>Effetto delle righe eliminate, aggiunte ed errori sullo spostamento del cursore  
 I cursori statici e gestiti da keyset consentono talvolta di rilevare le righe aggiunte al set di risultati e di rimuovere le righe eliminate dal set di risultati. Chiamando **SQLGetInfo** con le opzioni SQL_STATIC_CURSOR_ATTRIBUTES2 e SQL_KEYSET_CURSOR_ATTRIBUTES2 ed esaminando le maschere di SQL_CA2_SENSITIVITY_ADDITIONS, SQL_CA2_SENSITIVITY_DELETIONS e SQL_CA2_SENSITIVITY_UPDATES, un'applicazione determina se i cursori implementati da un determinato driver eseguono questa operazione. Per i driver in grado di rilevare le righe eliminate e di rimuoverle, i paragrafi seguenti descrivono gli effetti di questo comportamento. Per i driver in grado di rilevare le righe eliminate ma non di rimuoverle, le eliminazioni non hanno effetto sui movimenti del cursore e i paragrafi seguenti non sono applicabili.  
  
 Se il cursore rileva le righe aggiunte al set di risultati o rimuove le righe eliminate dal set di risultati, viene visualizzato come se rilevi tali modifiche solo quando recupera i dati. Ciò include il caso in cui viene chiamato **SQLFetchScroll** con FetchOrientation impostato su SQL_FETCH_RELATIVE e FetchOffset impostato su 0 per recuperare lo stesso set di righe, ma non include il case quando SQLSetPos viene chiamato con fOption impostato su SQL_REFRESH. Nel secondo caso, i dati nei buffer dei set di righe vengono aggiornati, ma non recuperati, e le righe eliminate non vengono rimosse dal set di risultati. Pertanto, quando una riga viene eliminata o inserita nel set di righe corrente, il cursore non modifica i buffer dei set di righe. Viene invece rilevata la modifica quando recupera tutti i set di righe che in precedenza includevano la riga eliminata o include ora la riga inserita.  
  
 Ad esempio:  
  
```cpp  
// Fetch the next rowset.  
SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
// Delete third row of the rowset. Does not modify the rowset buffers.  
SQLSetPos(hstmt, 3, SQL_DELETE, SQL_LOCK_NO_CHANGE);  
// The third row has a status of SQL_ROW_DELETED after this call.  
SQLSetPos(hstmt, 3, SQL_REFRESH, SQL_LOCK_NO_CHANGE);  
// Refetch the same rowset. The third row is removed, replaced by what  
// was previously the fourth row.  
SQLFetchScroll(hstmt, SQL_FETCH_RELATIVE, 0);  
```  
  
 Quando **SQLFetchScroll** restituisce un nuovo set di righe con una posizione rispetto al set di righe corrente, ovvero FetchOrientation è SQL_FETCH_NEXT, SQL_FETCH_PRIOR o SQL_FETCH_RELATIVE: non include le modifiche apportate al set di righe corrente durante il calcolo della posizione iniziale del nuovo set di righe. Tuttavia, include le modifiche al di fuori del set di righe corrente se è in grado di rilevarle. Inoltre, quando **SQLFetchScroll** restituisce un nuovo set di righe con una posizione indipendente dal set di righe corrente, ovvero FetchOrientation è SQL_FETCH_FIRST, SQL_FETCH_LAST, SQL_FETCH_ABSOLUTE o SQL_FETCH_BOOKMARK, include tutte le modifiche che è in grado di rilevare, anche se si trovano nel set di righe corrente.  
  
 Quando si determina se le righe appena aggiunte sono all'interno o all'esterno del set di righe corrente, un set di righe parziale viene considerato che termina in corrispondenza dell'ultima riga valida. ovvero l'ultima riga per la quale lo stato della riga non è SQL_ROW_NOROW. Si supponga, ad esempio, che il cursore sia in grado di rilevare righe appena aggiunte, che il set di righe corrente sia un set di righe parziale, che l'applicazione aggiunga nuove righe e che il cursore aggiunga tali righe alla fine del set di risultati. Se l'applicazione chiama **SQLFetchScroll** con FetchOrientation impostato su SQL_FETCH_NEXT, **SQLFetchScroll** restituisce il set di righe che inizia con la prima riga appena aggiunta.  
  
 Si supponga ad esempio che il set di righe corrente contenga righe da 21 a 30, che la dimensione del set di righe sia 10, che il cursore rimuova le righe eliminate dal set di risultati e che il cursore rilevi le righe aggiunte al set di risultati. La tabella seguente mostra le righe restituite da **SQLFetchScroll** in varie situazioni.  
  
|Modifica|Tipo di recupero|FetchOffset|Nuovo set di righe [1]|  
|------------|----------------|-----------------|---------------------|  
|Elimina riga 21|NEXT|0|da 31 a 40|  
|Elimina riga 31|NEXT|0|da 32 a 41|  
|Inserisci riga tra le righe 21 e 22|NEXT|0|da 31 a 40|  
|Inserisci riga tra le righe 30 e 31|NEXT|0|Inserita riga, da 31 a 39|  
|Elimina riga 21|PRIOR|0|da 11 a 20|  
|Elimina riga 20|PRIOR|0|da 10 a 19|  
|Inserisci riga tra le righe 21 e 22|PRIOR|0|da 11 a 20|  
|Inserisci riga tra le righe 20 e 21|PRIOR|0|da 12 a 20, riga inserita|  
|Elimina riga 21|RELATIVE|0|da 22 a 31<sup>[2]</sup>|  
|Elimina riga 21|RELATIVE|1|da 22 a 31|  
|Inserisci riga tra le righe 21 e 22|RELATIVE|0|21, riga inserita, da 22 a 29|  
|Inserisci riga tra le righe 21 e 22|RELATIVE|1|da 22 a 31|  
|Elimina riga 21|ABSOLUTE|21|da 22 a 31<sup>[2]</sup>|  
|Elimina riga 22|ABSOLUTE|21|da 21, 23 a 31|  
|Inserisci riga tra le righe 21 e 22|ABSOLUTE|22|Inserita riga, da 22 a 29|  
  
 [1] Questa colonna utilizza i numeri di riga prima che siano state inserite o eliminate righe.  
  
 [2] in questo caso, il cursore tenta di restituire righe a partire dalla riga 21. Poiché la riga 21 è stata eliminata, la prima riga restituita è la riga 22.  
  
 Le righe con errori, ovvero le righe con lo stato SQL_ROW_ERROR, non influiscono sul movimento del cursore. Se, ad esempio, il set di righe corrente inizia con la riga 11 e lo stato della riga 11 è SQL_ROW_ERROR, la chiamata a **SQLFetchScroll** con FetchOrientation impostato su SQL_FETCH_RELATIVE e FetchOffset impostato su 5 restituisce il set di righe che inizia con la riga 16, così come se lo stato della riga 11 fosse SQL_SUCCESS.  
  
## <a name="returning-data-in-bound-columns"></a>Restituzione di dati nelle colonne con binding  
 **SQLFetchScroll** restituisce i dati nelle colonne delimitate in modo analogo a **SQLFetch**. Per ulteriori informazioni, vedere "restituzione di dati nelle colonne con binding" nella [funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
 Se non è associata alcuna colonna, **SQLFetchScroll** non restituisce dati, ma sposta il cursore di blocco nella posizione specificata. Il fatto che i dati possano essere recuperati da colonne non associate di un cursore a blocchi con **SQLGetData** dipendano dal driver. Questa funzionalità è supportata se una chiamata a **SQLGetInfo** restituisce il bit SQL_GD_BLOCK per il tipo di informazioni di SQL_GETDATA_EXTENSIONS.  
  
## <a name="buffer-addresses"></a>Indirizzi buffer  
 **SQLFetchScroll** utilizza la stessa formula per determinare l'indirizzo di dati e buffer di lunghezza/indicatore come **SQLFetch**. Per ulteriori informazioni, vedere "buffer Address" nella [funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="row-status-array"></a>Matrice di stato riga  
 **SQLFetchScroll** imposta i valori nella matrice di stato della riga in modo analogo a SQLFetch. Per ulteriori informazioni, vedere "Row Status Array" nella [funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="rows-fetched-buffer"></a>Buffer recuperato righe  
 **SQLFetchScroll** restituisce il numero di righe recuperate nel buffer recuperato dalle righe in modo analogo a **SQLFetch**. Per ulteriori informazioni, vedere "righe recuperate buffer" nella [funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="error-handling"></a>Gestione degli errori  
 Quando un'applicazione chiama **SQLFetchScroll** in un driver ODBC 3. x, gestione driver chiama **SQLFetchScroll** nel driver. Quando un'applicazione chiama **SQLFetchScroll** in un driver ODBC 2. x, gestione driver chiama SQLExtendedFetch nel driver. Poiché **SQLFetchScroll** e SQLExtendedFetch gestiscono gli errori in modo leggermente diverso, l'applicazione rileva un comportamento di errore leggermente diverso quando chiama **SQLFETCHSCROLL** nei driver ODBC 2. x e ODBC 3. x.  
  
 **SQLFetchScroll** restituisce gli errori e gli avvisi in modo analogo a **SQLFetch**; Per ulteriori informazioni, vedere la sezione relativa alla gestione degli errori in **SQLFetch**. **SQLExtendedFetch** restituisce errori in modo analogo a **SQLFetch**, con le eccezioni seguenti:  
  
 Quando si verifica un avviso che si applica a una riga specifica nel set di righe, SQLExtendedFetch imposta la voce corrispondente nella matrice di stato della riga su SQL_ROW_SUCCESS, non SQL_ROW_SUCCESS_WITH_INFO.  
  
 Se si verificano errori in ogni riga del set di righe, SQLExtendedFetch restituisce SQL_SUCCESS_WITH_INFO, non SQL_ERROR.  
  
 In ogni gruppo di record di stato che si applica a una singola riga, il primo record di stato restituito da SQLExtendedFetch deve contenere SQLSTATE 01S01 (errore nella riga); **SQLFetchScroll** non restituisce questo SQLSTATE. Se SQLExtendedFetch non è in grado di restituire SQLSTATE aggiuntivi, deve comunque restituire questo SQLSTATE.  
  
## <a name="sqlfetchscroll-and-optimistic-concurrency"></a>SQLFetchScroll e concorrenza ottimistica  
 Se un cursore utilizza la concorrenza ottimistica, ovvero l'attributo SQL_ATTR_CONCURRENCY istruzione ha il valore SQL_CONCUR_VALUES o SQL_CONCUR_ROWVER- **SQLFetchScroll** aggiorna i valori di concorrenza ottimistica utilizzati dall'origine dati per rilevare se una riga è stata modificata. Questo errore si verifica ogni volta che **SQLFetchScroll** recupera un nuovo set di righe, incluso il recupero del set di righe corrente. Viene chiamato con FetchOrientation impostato su SQL_FETCH_RELATIVE e FetchOffset impostato su 0.  
  
## <a name="sqlfetchscroll-and-odbc-2x-drivers"></a>Driver SQLFetchScroll e ODBC 2. x  
 Quando un'applicazione chiama **SQLFetchScroll** in un driver ODBC 2. x, gestione driver esegue il mapping della chiamata a **SQLExtendedFetch**. Vengono passati i valori seguenti per gli argomenti di **SQLExtendedFetch**.  
  
|Argomento SQLExtendedFetch|Valore|  
|-------------------------------|-----------|  
|StatementHandle|StatementHandle in **SQLFetchScroll**.|  
|FetchOrientation|FetchOrientation in **SQLFetchScroll**.|  
|FetchOffset|Se FetchOrientation non è SQL_FETCH_BOOKMARK, viene usato il valore dell'argomento FetchOffset in **SQLFetchScroll** .<br /><br /> Se FetchOrientation è SQL_FETCH_BOOKMARK, viene utilizzato il valore archiviato in corrispondenza dell'indirizzo specificato dall'attributo SQL_ATTR_FETCH_BOOKMARK_PTR Statement.|  
|RowCountPtr|Indirizzo specificato dall'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR.|  
|RowStatusArray|Indirizzo specificato dall'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.|  
  
 Per ulteriori informazioni, vedere [cursori a blocchi, cursori scorrevoli e compatibilità con le versioni precedenti](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) in Appendice G: linee guida per la compatibilità con le versioni precedenti.  
  
## <a name="descriptors-and-sqlfetchscroll"></a>Descrittori e SQLFetchScroll  
 **SQLFetchScroll** interagisce con i descrittori in modo analogo a **SQLFetch**. Per ulteriori informazioni, vedere la sezione "descrittori e SQLFetchScroll" nella [funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [associazione per colonna](../../../odbc/reference/develop-app/column-wise-binding.md), [associazione per riga](../../../odbc/reference/develop-app/row-wise-binding.md), [istruzioni Update e Delete posizionate](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md)e [aggiornamento delle righe nel set di righe con SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Esecuzione di operazioni bulk di inserimento, aggiornamento o eliminazione|[Funzione SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su una colonna in un set di risultati|[Funzione SQLDescribeCol ](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di sola trasmissione|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Chiusura del cursore sull'istruzione|[SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Restituzione del numero di colonne del set di risultati|[Funzione SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Posizionamento del cursore, aggiornamento dei dati nel set di righe o aggiornamento o eliminazione di dati nel set di risultati|[Funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
