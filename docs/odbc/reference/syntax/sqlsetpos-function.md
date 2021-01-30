---
description: Funzione SQLSetPos
title: Funzione SQLSetPos | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetPos
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetPos
helpviewer_keywords:
- SQLSetPos function [ODBC]
ms.assetid: 80190ee7-ae3b-45e5-92a9-693eb558f322
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4793fe06d25b090dfa6306eb8c4e72d513abc8d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188037"
---
# <a name="sqlsetpos-function"></a>Funzione SQLSetPos
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLSetPos** imposta la posizione del cursore in un set di righe e consente a un'applicazione di aggiornare i dati nel set di righe o di aggiornare o eliminare i dati nel set di risultati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLSetPos(  
      SQLHSTMT        StatementHandle,  
      SQLSETPOSIROW   RowNumber,  
      SQLUSMALLINT    Operation,  
      SQLUSMALLINT    LockType);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *RowNumber*  
 Input Posizione della riga nel set di righe su cui eseguire l'operazione specificata con l'argomento *Operation* . Se *RowNumber* è 0, l'operazione viene applicata a ogni riga nel set di righe.  
  
 Per ulteriori informazioni, vedere "Commenti".  
  
 *Operazione*  
 Input Operazione da eseguire:  
  
 SQL_POSITION SQL_REFRESH SQL_UPDATE SQL_DELETE  
  
> [!NOTE]
>  Il valore SQL_ADD per l'argomento *Operation* è stato deprecato per ODBC *3. x*. I driver ODBC *3. x* dovranno supportare SQL_ADD per la compatibilità con le versioni precedenti. Questa funzionalità è stata sostituita da una chiamata a **SQLBulkOperations** con un' *operazione* di SQL_ADD. Quando un'applicazione ODBC *3. x* funziona con un driver *ODBC 2. x* , gestione driver esegue il mapping di una chiamata a **SQLBulkOperations** con un' *operazione* di SQL_ADD a **SQLSetPos** con un' *operazione* di SQL_ADD.  
  
 Per ulteriori informazioni, vedere "Commenti".  
  
 *LockType*  
 Input Specifica la modalità di blocco della riga dopo l'esecuzione dell'operazione specificata nell'argomento *Operation* .  
  
 SQL_LOCK_NO_CHANGE SQL_LOCK_EXCLUSIVE SQL_LOCK_UNLOCK  
  
 Per ulteriori informazioni, vedere "Commenti".  
  
## <a name="returns"></a>Restituisce  
  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetPos** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLSetPos** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
 Per tutti questi SQLSTATE che possono restituire SQL_SUCCESS_WITH_INFO o SQL_ERROR (ad eccezione di sql01xxxs), viene restituito SQL_SUCCESS_WITH_INFO se si verifica un errore in una o più righe, ma non in tutte, righe di un'operazione più righe e viene restituito SQL_ERROR se si verifica un errore in un'operazione a riga singola.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01001|Conflitto operazione cursore|L'argomento dell' *operazione* è stato SQL_DELETE o SQL_UPDATE e non sono state eliminate o aggiornate righe o più righe. Per ulteriori informazioni sugli aggiornamenti a più di una riga, vedere la descrizione dell' *attributo* SQL_ATTR_SIMULATE_CURSOR in **SQLSetStmtAttr**.) (La funzione restituisce SQL_SUCCESS_WITH_INFO.)<br /><br /> L'argomento dell' *operazione* è stato SQL_DELETE o SQL_UPDATE e l'operazione non è riuscita a causa della concorrenza ottimistica. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Troncamento a destra dei dati stringa|L'argomento dell' *operazione* è stato SQL_REFRESH e i dati stringa o binari restituiti per una colonna o colonne con tipo di dati SQL_C_CHAR o SQL_C_BINARY hanno causato il troncamento di dati binari non vuoti o non null.|  
|01S01|Errore nella riga|L'argomento *RowNumber* è 0 e si è verificato un errore in una o più righe durante l'esecuzione dell'operazione specificata con l'argomento *Operation* .<br /><br /> (SQL_SUCCESS_WITH_INFO viene restituito se si verifica un errore in una o più righe, ma non tutte, righe di un'operazione più righe e viene restituito SQL_ERROR se si verifica un errore in un'operazione a riga singola).<br /><br /> Questo valore SQLSTATE viene restituito solo quando **SQLSetPos** viene chiamato dopo **SQLExtendedFetch**, se il driver è un driver ODBC *2. x* e la libreria di cursori non viene utilizzata.|  
|01S07|Troncamento frazionario|L'argomento *Operation* è stato SQL_REFRESH, il tipo di dati del buffer dell'applicazione non è stato SQL_C_CHAR o SQL_C_BINARY e i dati restituiti ai buffer dell'applicazione per una o più colonne sono stati troncati. Per i tipi di dati numerici, la parte frazionaria del numero è stata troncata. Per i tipi di dati time, timestamp e Interval contenenti un componente ora, la parte frazionaria del tempo è stata troncata.<br /><br /> (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Impossibile convertire il valore dei dati di una colonna nel set di risultati nel tipo di dati specificato da *targetType* nella chiamata a **SQLBindCol**.|  
|07009|Indice del descrittore non valido|L' *operazione* argument è stata SQL_REFRESH o SQL_UPDATE e una colonna è stata associata a un numero di colonna maggiore del numero di colonne nel set di risultati.|  
|21S02|Il grado della tabella derivata non corrisponde all'elenco di colonne|L' *operazione* dell'argomento è stata SQL_UPDATE e nessuna colonna è aggiornabile perché tutte le colonne sono di tipo non associato o di sola lettura oppure il valore nel buffer di lunghezza/indicatore è stato SQL_COLUMN_IGNORE.|  
|22001|Dati stringa, troncamento a destra|L'argomento dell' *operazione* è stato SQL_UPDATE e l'assegnazione di un valore di tipo carattere o binario a una colonna ha determinato il troncamento di caratteri non vuoti (per i caratteri) o di byte o caratteri non null (per binari).|  
|22003|Valore numerico non compreso nell'intervallo|L' *operazione* relativa all'argomento è stata SQL_UPDATE e l'assegnazione di un valore numerico a una colonna nel set di risultati ha causato il troncamento dell'intero oggetto, invece della parte frazionaria del numero.<br /><br /> L' *operazione* relativa all'argomento è stata SQL_REFRESH e la restituzione del valore numerico per una o più colonne limite avrebbe causato la perdita di cifre significative.|  
|22007|Formato DateTime non valido|L' *operazione* relativa all'argomento è stata SQL_UPDATE e l'assegnazione di un valore di data o timestamp a una colonna nel set di risultati ha determinato che il campo anno, mese o giorno non è compreso nell'intervallo.<br /><br /> L' *operazione* relativa all'argomento è stata SQL_REFRESH e la restituzione del valore di data o timestamp per una o più colonne limite avrebbe causato il campo anno, mese o giorno non compreso nell'intervallo.|  
|22008|Overflow del campo Data/ora|L'argomento dell' *operazione* è stato SQL_UPDATE e le prestazioni dell'aritmetica di DateTime sui dati inviati a una colonna nel set di risultati hanno generato un campo DateTime (anno, mese, giorno, ora, minuto o secondo) del risultato non compreso nell'intervallo consentito di valori per il campo o non valido in base alle regole naturali del calendario gregoriano per DateTime.<br /><br /> L'argomento *Operation* è stato SQL_REFRESH e le prestazioni dell'aritmetica DateTime sui dati recuperati dal set di risultati hanno generato un campo DateTime (anno, mese, giorno, ora, minuto o secondo) del risultato che non rientra nell'intervallo consentito di valori per il campo oppure non è valido in base alle regole naturali del calendario gregoriano per i valori DateTime.|  
|22015|Overflow del campo Interval|L'argomento dell' *operazione* è stato SQL_UPDATE e l'assegnazione di un tipo numerico o intervallo C esatto a un tipo di dati SQL intervallo ha causato una perdita di cifre significative.<br /><br /> L'argomento dell' *operazione* è stato SQL_UPDATE; Quando si assegna a un tipo SQL intervallo, non esiste alcuna rappresentazione del valore del tipo C nel tipo SQL intervallo.<br /><br /> L'argomento dell' *operazione* è stato SQL_REFRESH e l'assegnazione da un tipo SQL numerico o intervallo esatto a un tipo intervallo C ha causato la perdita di cifre significative nel campo iniziali.<br /><br /> L'argomento dell' *operazione* è stato SQL_ aggiornamento; Quando si assegna a un tipo intervallo C, non è presente alcuna rappresentazione del valore del tipo SQL nel tipo intervallo C.|  
|22018|Valore di carattere non valido per la specifica del cast|L'argomento dell' *operazione* è stato SQL_REFRESH; il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. e il valore nella colonna non è un valore letterale valido del tipo C associato.<br /><br /> L' *operazione* dell'argomento è stata SQL_UPDATE; il tipo SQL è un tipo numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo C è stato SQL_C_CHAR; e il valore nella colonna non è un valore letterale valido del tipo SQL associato.|  
|23000|Violazione del vincolo di integrità|L' *operazione* argument è stata SQL_DELETE o SQL_UPDATE ed è stato violato un vincolo di integrità.|  
|24000|Stato del cursore non valido|Lo stato di *statementHandle* è stato eseguito, ma nessun set di risultati è stato associato a *statementHandle*.<br /><br /> (DM) un cursore è stato aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .<br /><br /> Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato, ma il cursore è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati.<br /><br /> L' *operazione* argument è stata SQL_DELETE, SQL_REFRESH o SQL_UPDATE e il cursore è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati.|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|42000|Errore di sintassi o violazione di accesso|Il driver non è stato in grado di bloccare la riga in base alle esigenze per eseguire l'operazione richiesta nell' *operazione* dell'argomento.<br /><br /> Il driver non è stato in grado di bloccare la riga come richiesto nell'argomento *LockType*.|  
|44000|Violazione della clausola WITH CHECK OPTION|L'argomento *Operation* è stato SQL_UPDATE e l'aggiornamento è stato eseguito su una tabella visualizzata o una tabella derivata dalla tabella visualizzata che è stata creata specificando **with check Option**, in modo che una o più righe interessate dall'aggiornamento non siano più presenti nella tabella visualizzata.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* e quindi la funzione è stata chiamata nuovamente su *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione SQLSetPos.<br /><br /> (DM) il *statementHandle* specificato non si trova in uno stato eseguito. La funzione è stata chiamata senza prima chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) il driver era un driver ODBC *2. x* ed è stato chiamato **SQLSetPos** per *statementHandle* dopo la chiamata a **SQLFetch** .|  
|HY011|Non è possibile impostare l'attributo adesso|(DM) il driver era un driver ODBC *2. x* . l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR è stato impostato. **SQLSetPos** è stato chiamato prima della chiamata a **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch** .|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|L'argomento dell' *operazione* è stato SQL_UPDATE, un valore di dati è un puntatore null e il valore della lunghezza della colonna non è 0, SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NULL_DATA o minore o uguale a SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> L'argomento dell' *operazione* è stato SQL_UPDATE; un valore di dati non è un puntatore null. il tipo di dati C è SQL_C_BINARY o SQL_C_CHAR; il valore della lunghezza della colonna è minore di 0 ma non uguale a SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NTS o SQL_NULL_DATA o minore o uguale a SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Il valore in un buffer di lunghezza/indicatore è stato SQL_DATA_AT_EXEC; il tipo SQL è SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati lungo specifico dell'origine dati. e il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y".|  
|HY092|Identificatore di attributo non valido|(DM) il valore specificato per l'argomento dell' *operazione* non è valido.<br /><br /> (DM) il valore specificato per l'argomento *LockType* non è valido.<br /><br /> L'argomento *Operation* è stato SQL_UPDATE o SQL_DELETE e l'attributo SQL_ATTR_CONCURRENCY Statement è stato SQL_ATTR_CONCUR_READ_ONLY.|  
|HY107|Valore di riga non compreso nell'intervallo|Il valore specificato per l'argomento *RowNumber* è maggiore del numero di righe nel set di righe.|  
|HY109|Posizione del cursore non valida|Il cursore associato a *statementHandle* è stato definito come di sola trasmissione, pertanto non è stato possibile posizionare il cursore all'interno del set di righe. Vedere la descrizione dell'attributo SQL_ATTR_CURSOR_TYPE in **SQLSetStmtAttr**.<br /><br /> L'argomento *Operation* è stato SQL_UPDATE, SQL_DELETE o SQL_REFRESH e la riga identificata dall'argomento *RowNumber* è stata eliminata o non è stata recuperata.<br /><br /> (DM) l'argomento *RowNumber* è 0 e l'argomento *Operation* è stato SQL_POSITION.<br /><br /> **SQLSetPos** è stato chiamato dopo la chiamata di **SQLBulkOperations** e prima della chiamata a **SQLFetchScroll** o **SQLFetch** .|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta l'operazione richiesta nell'argomento *Operation* o nell'argomento *LockType* .|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr** con un *attributo* di SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
  
> [!CAUTION]
>  Per informazioni sugli stati di istruzione in cui è possibile chiamare **SQLSetPos** e sulle operazioni che è necessario eseguire per la compatibilità con le applicazioni ODBC *2. x* , vedere [bloccare i cursori, cursori scorrevoli e compatibilità con le versioni precedenti](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md).  
  
## <a name="rownumber-argument"></a>Argomento RowNumber  
 L'argomento *RowNumber* specifica il numero della riga nel set di righe su cui eseguire l'operazione specificata dall'argomento *Operation* . Se *RowNumber* è 0, l'operazione viene applicata a ogni riga nel set di righe. *RowNumber* deve essere un valore compreso tra 0 e il numero di righe nel set di righe.  
  
> [!NOTE]  
>  Nel linguaggio C, le matrici sono basate su 0 e l'argomento *RowNumber* è in base 1. Ad esempio, per aggiornare la quinta riga del set di righe, un'applicazione modifica i buffer dei set di righe in corrispondenza dell'indice di matrice 4, ma specifica un *RowNumber* di 5.  
  
 Tutte le operazioni posizionano il cursore sulla riga specificata da *RowNumber*. Per le operazioni seguenti è necessaria una posizione del cursore:  
  
-   Istruzioni Update e Delete posizionate.  
  
-   Chiamate a **SQLGetData**.  
  
-   Chiama a **SQLSetPos** con le opzioni SQL_DELETE, SQL_REFRESH e SQL_UPDATE.  
  
 Se, ad esempio, *RowNumber* è 2 per una chiamata a **SQLSetPos** con un' *operazione* di SQL_DELETE, il cursore viene posizionato sulla seconda riga del set di righe e la riga viene eliminata. La voce nella matrice di stato della riga di implementazione (a cui fa riferimento l'attributo SQL_ATTR_ROW_STATUS_PTR Statement) per la seconda riga viene modificata in SQL_ROW_DELETED.  
  
 Un'applicazione può specificare una posizione del cursore quando chiama **SQLSetPos**. In genere, chiama **SQLSetPos** con l'operazione SQL_POSITION o SQL_REFRESH per posizionare il cursore prima di eseguire un'istruzione Update o DELETE posizionata o la chiamata a **SQLGetData**.  
  
## <a name="operation-argument"></a>Argomento Operation  
 L'argomento *Operation* supporta le operazioni seguenti. Per determinare quali opzioni sono supportate da un'origine dati, un'applicazione chiama **SQLGetInfo** con il tipo di informazioni SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 o SQL_STATIC_CURSOR_ATTRIBUTES1 (a seconda del tipo di cursore).  
  
|*Operazione*<br /><br /> argomento|Operazione|  
|------------------------------|---------------|  
|SQL_POSITION|Il driver posiziona il cursore sulla riga specificata da *RowNumber*.<br /><br /> Il contenuto della matrice di stato della riga a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR viene ignorato per l' *operazione* di SQL_POSITION.|  
|SQL_REFRESH|Il driver posiziona il cursore sulla riga specificata da *RowNumber* e aggiorna i dati nei buffer dei set di righe per la riga. Per ulteriori informazioni sul modo in cui il driver restituisce i dati nei buffer dei set di righe, vedere le descrizioni dell'associazione per riga e per colonna in **SQLBindCol**.<br /><br /> **SQLSetPos** con un' *operazione* di SQL_REFRESH aggiorna lo stato e il contenuto delle righe all'interno del set di righe recuperato corrente. Ciò include l'aggiornamento dei segnalibri. Poiché i dati nei buffer vengono aggiornati ma non recuperati, l'appartenenza al set di righe è fissa. Questa operazione è diversa dall'aggiornamento eseguito da una chiamata a **SQLFetchScroll** con un *FetchOrientation* di SQL_FETCH_RELATIVE e un *RowNumber* uguale a 0, che consente di recuperare il set di righe dal set di risultati in modo da visualizzare i dati aggiunti e rimuovere i dati eliminati se tali operazioni sono supportate dal driver e dal cursore.<br /><br /> Un aggiornamento riuscito con **SQLSetPos** non modificherà lo stato della riga SQL_ROW_DELETED. Le righe eliminate all'interno del set di righe continueranno a essere contrassegnate come eliminate fino alla successiva operazione di recupero. Le righe scompariranno alla successiva operazione di recupero se il cursore supporta la compressione (in cui un oggetto **SQLFetch** o **SQLFetchScroll** successivo non restituisce righe eliminate).<br /><br /> Le righe aggiunte non vengono visualizzate quando viene eseguito un aggiornamento con **SQLSetPos** . Questo comportamento è diverso da **SQLFetchScroll** con un *FetchType* di SQL_FETCH_RELATIVE e da un *RowNumber* uguale a 0, che aggiorna anche il set di righe corrente, ma Visualizza i record aggiunti o i record eliminati del pacchetto se queste operazioni sono supportate dal cursore.<br /><br /> Un aggiornamento corretto con **SQLSetPos** modificherà lo stato della riga SQL_ROW_ADDED SQL_ROW_SUCCESS (se la matrice di stato della riga esiste).<br /><br /> Un aggiornamento corretto con **SQLSetPos** modificherà lo stato della riga SQL_ROW_UPDATED al nuovo stato della riga, se esiste la matrice di stato della riga.<br /><br /> Se si verifica un errore in un'operazione **SQLSetPos** su una riga, lo stato della riga viene impostato su SQL_ROW_ERROR (se la matrice di stato della riga esiste).<br /><br /> Per un cursore aperto con un attributo di istruzione SQL_ATTR_CONCURRENCY di SQL_CONCUR_ROWVER o SQL_CONCUR_VALUES, un aggiornamento con **SQLSetPos** potrebbe aggiornare i valori di concorrenza ottimistica utilizzati dall'origine dati per rilevare che la riga è stata modificata. In tal caso, le versioni di riga o i valori utilizzati per garantire la concorrenza dei cursori vengono aggiornati ogni volta che i buffer del set di righe vengono aggiornati dal server. Questo errore si verifica per ogni riga aggiornata.<br /><br /> Il contenuto della matrice di stato della riga a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR viene ignorato per l' *operazione* di SQL_REFRESH.|  
|SQL_UPDATE|Il driver posiziona il cursore sulla riga specificata da *RowNumber* e aggiorna la riga di dati sottostante con i valori nei buffer del set di righe (l'argomento *TargetValuePtr* in **SQLBindCol**). Recupera le lunghezze dei dati dai buffer di lunghezza/indicatore (l'argomento *StrLen_or_IndPtr* in **SQLBindCol**). Se la lunghezza di una colonna è SQL_COLUMN_IGNORE, la colonna non viene aggiornata. Dopo l'aggiornamento della riga, il driver modifica l'elemento corrispondente della matrice di stato della riga in SQL_ROW_UPDATED o SQL_ROW_SUCCESS_WITH_INFO (se la matrice di stato della riga esiste).<br /><br /> Il comportamento è definito dal driver se **SQLSetPos** con un argomento *Operation* of SQL_UPDATE viene chiamato su un cursore che contiene colonne duplicate. Il driver può restituire un valore SQLSTATE definito dal driver, può aggiornare la prima colonna visualizzata nel set di risultati oppure eseguire altro comportamento definito dal driver.<br /><br /> La matrice di operazioni di riga a cui punta l'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR può essere utilizzata per indicare che una riga nel set di righe corrente deve essere ignorata durante un aggiornamento bulk. Per ulteriori informazioni, vedere l'argomento relativo alle matrici di stato e di operazione più avanti in questo riferimento alla funzione.|  
|SQL_DELETE|Il driver posiziona il cursore sulla riga specificata da *RowNumber* ed elimina la riga di dati sottostante. Modifica l'elemento corrispondente della matrice di stato della riga in SQL_ROW_DELETED. Dopo che la riga è stata eliminata, gli elementi seguenti non sono validi per la riga: le istruzioni Update e Delete posizionate, le chiamate a **SQLGetData** e le chiamate a **SQLSetPos** con *Operation* impostato su anything eccetto SQL_POSITION. Per i driver che supportano la compressione, la riga viene eliminata dal cursore quando vengono recuperati nuovi dati dall'origine dati.<br /><br /> Il fatto che la riga rimanga visibile dipende dal tipo di cursore. Ad esempio, le righe eliminate sono visibili ai cursori statici e gestiti da keyset ma invisibile ai cursori dinamici.<br /><br /> La matrice di operazioni di riga a cui punta l'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR può essere utilizzata per indicare che una riga nel set di righe corrente deve essere ignorata durante un'eliminazione bulk. Per ulteriori informazioni, vedere l'argomento relativo alle matrici di stato e di operazione più avanti in questo riferimento alla funzione.|  
  
## <a name="locktype-argument"></a>Argomento LockType  
 L'argomento *LockType* fornisce un modo per le applicazioni per il controllo della concorrenza. Nella maggior parte dei casi, le origini dati che supportano i livelli di concorrenza e le transazioni supporteranno solo il valore SQL_LOCK_NO_CHANGE dell'argomento *LockType* . L'argomento *LockType* viene in genere usato solo per il supporto basato su file.  
  
 L'argomento *LockType* specifica lo stato di blocco della riga dopo l'esecuzione di **SQLSetPos** . Se il driver non è in grado di bloccare la riga per eseguire l'operazione richiesta o per soddisfare l'argomento *LockType* , restituisce SQL_ERROR e SQLSTATE 42000 (errore di sintassi o violazione di accesso).  
  
 Sebbene l'argomento *LockType* sia specificato per una singola istruzione, il blocco si accorda sugli stessi privilegi a tutte le istruzioni della connessione. In particolare, un blocco acquisito da un'istruzione su una connessione può essere sbloccato da un'istruzione diversa nella stessa connessione.  
  
 Una riga bloccata tramite **SQLSetPos** rimane bloccata fino a quando l'applicazione non chiama **SQLSetPos** per la riga con *LockType* impostato su SQL_LOCK_UNLOCK o finché l'applicazione non chiama **SQLFreeHandle** per l'istruzione o **SQLFreeStmt** con l'opzione SQL_CLOSE. Per un driver che supporta le transazioni, una riga bloccata tramite **SQLSetPos** viene sbloccata quando l'applicazione chiama **SQLEndTran** per eseguire il commit o il rollback di una transazione sulla connessione (se un cursore viene chiuso quando viene eseguito il commit o il rollback di una transazione, come indicato dal SQL_CURSOR_COMMIT_BEHAVIOR e SQL_CURSOR_ROLLBACK_BEHAVIOR i tipi di informazioni restituiti da **SQLGetInfo**).  
  
 L'argomento *LockType* supporta i tipi di blocchi seguenti. Per determinare quali blocchi sono supportati da un'origine dati, un'applicazione chiama **SQLGetInfo** con il tipo di informazioni SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 o SQL_STATIC_CURSOR_ATTRIBUTES1 (a seconda del tipo di cursore).  
  
|Argomento *LockType*|Tipo di blocco|  
|-------------------------|---------------|  
|SQL_LOCK_NO_CHANGE|Il driver o l'origine dati garantisce che la riga si trovi nello stesso stato bloccato o sbloccato di prima della chiamata a **SQLSetPos** . Questo valore di *LockType* consente alle origini dati che non supportano il blocco esplicito a livello di riga di utilizzare il blocco richiesto dai livelli di isolamento della concorrenza e delle transazioni correnti.|  
|SQL_LOCK_EXCLUSIVE|Il driver o l'origine dati blocca la riga in modo esclusivo. Un'istruzione su una connessione diversa o in un'altra applicazione non può essere utilizzata per acquisire blocchi sulla riga.|  
|SQL_LOCK_UNLOCK|Il driver o l'origine dati sblocca la riga.|  
  
 Se un driver supporta SQL_LOCK_EXCLUSIVE ma non supporta SQL_LOCK_UNLOCK, una riga bloccata rimarrà bloccata fino a quando non viene eseguita una delle chiamate di funzione descritte nel paragrafo precedente.  
  
 Se un driver supporta SQL_LOCK_EXCLUSIVE ma non supporta SQL_LOCK_UNLOCK, una riga bloccata rimarrà bloccata fino a quando l'applicazione non chiamerà **SQLFreeHandle** per l'istruzione o **SQLFreeStmt** con l'opzione SQL_CLOSE. Se il driver supporta le transazioni e chiude il cursore quando si esegue il commit o il rollback della transazione, l'applicazione chiama **SQLEndTran**.  
  
 Per le operazioni di aggiornamento ed eliminazione in **SQLSetPos**, l'applicazione usa l'argomento *LockType* come indicato di seguito:  
  
-   Per garantire che una riga non venga modificata dopo che è stata recuperata, un'applicazione chiama **SQLSetPos** con *Operation* set su SQL_REFRESH e *LockType* impostato su SQL_LOCK_EXCLUSIVE.  
  
-   Se l'applicazione imposta *LockType* su SQL_LOCK_NO_CHANGE, il driver garantisce che un'operazione di aggiornamento o eliminazione avrà esito positivo solo se l'applicazione specificata SQL_CONCUR_LOCK per l'attributo SQL_ATTR_CONCURRENCY Statement.  
  
-   Se l'applicazione specifica SQL_CONCUR_ROWVER o SQL_CONCUR_VALUES per l'attributo dell'istruzione SQL_ATTR_CONCURRENCY, il driver confronta le versioni di riga o i valori e rifiuta l'operazione se la riga è stata modificata dopo che l'applicazione ha recuperato la riga.  
  
-   Se l'applicazione specifica SQL_CONCUR_READ_ONLY per l'attributo dell'istruzione SQL_ATTR_CONCURRENCY, il driver rifiuterà qualsiasi operazione di aggiornamento o eliminazione.  
  
 Per ulteriori informazioni sull'attributo SQL_ATTR_CONCURRENCY Statement, vedere [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="status-and-operation-arrays"></a>Matrici di stato e di operazioni  
 Quando si chiama **SQLSetPos** vengono usati gli Stati e le matrici di operazioni seguenti:  
  
-   La matrice di stato della riga (a cui fa riferimento il campo SQL_DESC_ARRAY_STATUS_PTR in IRD e l'attributo SQL_ATTR_ROW_STATUS_ARRAY Statement) contiene i valori di stato per ogni riga di dati nel set di righe. Il driver imposta i valori di stato in questa matrice dopo una chiamata a **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** o **SQLSetPos**. Questa matrice fa riferimento all'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.  
  
-   La matrice dell'operazione di riga (a cui fa riferimento il campo SQL_DESC_ARRAY_STATUS_PTR in ARD e l'attributo SQL_ATTR_ROW_OPERATION_ARRAY Statement) contiene un valore per ogni riga del set di righe che indica se una chiamata a **SQLSetPos** per un'operazione bulk viene ignorata o eseguita. Ogni elemento nella matrice è impostato su SQL_ROW_PROCEED (impostazione predefinita) o SQL_ROW_IGNORE. Questa matrice fa riferimento all'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR.  
  
 Il numero di elementi nelle matrici di stato e di operazione deve essere uguale al numero di righe nel set di righe (come definito dall'attributo SQL_ATTR_ROW_ARRAY_SIZE Statement).  
  
 Per informazioni sulla matrice di stato della riga, vedere [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Per informazioni sulla matrice di operazioni di riga, vedere "ignorare una riga in un'operazione bulk", più avanti in questa sezione.  
  
## <a name="using-sqlsetpos"></a>Uso di SQLSetPos  
 Prima che un'applicazione chiami **SQLSetPos**, deve eseguire la sequenza di passaggi seguente:  
  
1.  Se l'applicazione chiamerà **SQLSetPos** con *Operation* set su SQL_UPDATE, chiamare **SQLBindCol** (o **SQLSetDescRec**) per ogni colonna per specificare il tipo di dati e i buffer di associazione per i dati e la lunghezza della colonna.  
  
2.  Se l'applicazione chiamerà **SQLSetPos** con *Operation* set SQL_DELETE o SQL_UPDATE, chiamare **SQLColAttribute** per assicurarsi che le colonne da eliminare o aggiornare siano aggiornabili.  
  
3.  Chiamare **SQLExecDirect**, **SQLExecute** o una funzione di catalogo per creare un set di risultati.  
  
4.  Chiamare **SQLFetch** o **SQLFetchScroll** per recuperare i dati.  
  
 Per altre informazioni sull'uso di **SQLSetPos**, vedere [aggiornamento dei dati con SQLSetPos](../../../odbc/reference/develop-app/updating-data-with-sqlsetpos.md).  
  
## <a name="deleting-data-using-sqlsetpos"></a>Eliminazione di dati tramite SQLSetPos  
 Per eliminare i dati con **SQLSetPos**, un'applicazione chiama **SQLSetPos** con *RowNumber* impostato sul numero della riga da eliminare e il set di *operazioni* su SQL_DELETE.  
  
 Dopo che i dati sono stati eliminati, il driver modifica il valore nella matrice di stato della riga di implementazione per la riga appropriata in SQL_ROW_DELETED o SQL_ROW_ERROR.  
  
## <a name="updating-data-using-sqlsetpos"></a>Aggiornamento dei dati con SQLSetPos  
 Un'applicazione può passare il valore per una colonna nel buffer dei dati associato o con una o più chiamate a **SQLPutData**. Le colonne i cui dati vengono passati con **SQLPutData** sono note come *colonne* *data-at-execution* . Sono comunemente utilizzati per inviare dati per le colonne SQL_LONGVARBINARY e SQL_LONGVARCHAR e possono essere combinati con altre colonne.  
  
#### <a name="to-update-data-with-sqlsetpos-an-application"></a>Per aggiornare i dati con SQLSetPos, un'applicazione:  
  
1.  Inserisce i valori nei buffer di dati e di lunghezza/indicatore associati a **SQLBindCol**:  
  
    -   Per le colonne normali, l'applicazione inserisce il valore della nuova colonna nel buffer *\* TargetValuePtr* e la lunghezza del valore nel buffer *\* StrLen_or_IndPtr* . Se la riga non deve essere aggiornata, l'applicazione inserisce SQL_ROW_IGNORE nell'elemento di tale riga della matrice dell'operazione di riga.  
  
    -   Per le colonne data-at-execution, l'applicazione inserisce un valore definito dall'applicazione, ad esempio il numero di colonna, nel buffer di *\* TargetValuePtr* . Il valore può essere utilizzato in un secondo momento per identificare la colonna.  
  
         L'applicazione inserisce il risultato della macro SQL_LEN_DATA_AT_EXEC (*length*) nel buffer **StrLen_or_IndPtr* . Se il tipo di dati SQL della colonna è SQL_LONGVARBINARY, SQL_LONGVARCHAR o un tipo di dati lungo specifico dell'origine dati e il driver restituisce "Y" per il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo**, *length* è il numero di byte di dati da inviare per il parametro. in caso contrario, deve essere un valore non negativo e viene ignorato.  
  
2.  Chiama **SQLSetPos** con l'argomento *Operation* impostato su SQL_UPDATE per aggiornare la riga di dati.  
  
    -   Se non sono presenti colonne data-at-execution, il processo è completo.  
  
    -   Se sono presenti colonne data-at-execution, la funzione restituisce SQL_NEED_DATA e procede al passaggio 3.  
  
3.  Chiama **SQLParamData** per recuperare l'indirizzo del buffer *\* TargetValuePtr* per la prima colonna data-at-execution da elaborare. **SQLParamData** restituisce SQL_NEED_DATA. L'applicazione recupera il valore definito dall'applicazione dal buffer *\* TargetValuePtr* .  
  
    > [!NOTE]  
    >  Sebbene i parametri data-at-execution siano simili alle colonne data-at-execution, il valore restituito da **SQLParamData** è diverso per ogni oggetto.  
  
    > [!NOTE]  
    >  I parametri data-at-execution sono parametri in un'istruzione SQL per cui i dati verranno inviati con **SQLPutData** quando l'istruzione viene eseguita con **SQLExecDirect** o **SQLExecute**. Sono associati a **SQLBindParameter** o impostando descrittori con **SQLSetDescRec**. Il valore restituito da **SQLParamData** è un valore a 32 bit passato a **SQLBindParameter** nell'argomento *ParameterValuePtr* .  
  
    > [!NOTE]  
    >  Le colonne data-at-execution sono colonne di un set di righe per cui i dati verranno inviati con **SQLPutData** quando una riga viene aggiornata con **SQLSetPos**. Sono associati a **SQLBindCol**. Il valore restituito da **SQLParamData** è l'indirizzo della riga nel buffer **TargetValuePtr* in fase di elaborazione.  
  
4.  Chiama **SQLPutData** una o più volte per inviare i dati per la colonna. È necessaria più di una chiamata se non è possibile restituire tutti i valori dei dati nel buffer *\* TargetValuePtr* specificato in **SQLPutData**. sono consentite più chiamate a **SQLPutData** per la stessa colonna solo quando si inviano dati di tipo carattere c a una colonna con tipo di dati character, Binary o origine dati oppure quando si inviano dati binari c a una colonna con tipo di dati character, Binary o data source  
  
5.  Chiama di nuovo **SQLParamData** per segnalare che tutti i dati sono stati inviati per la colonna.  
  
    -   Se sono presenti più colonne data-at-execution, **SQLParamData** restituisce SQL_NEED_DATA e l'indirizzo del buffer *TargetValuePtr* per la colonna data-at-execution successiva da elaborare. L'applicazione ripete i passaggi 4 e 5.  
  
    -   Se non sono presenti altre colonne data-at-execution, il processo è completo. Se l'istruzione è stata eseguita correttamente, **SQLParamData** restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO; Se l'esecuzione ha esito negativo, restituisce SQL_ERROR. A questo punto, **SQLParamData** può restituire qualsiasi SQLSTATE che può essere restituito da **SQLSetPos**.  
  
 Se i dati sono stati aggiornati, il driver modifica il valore nella matrice di stato della riga di implementazione per la riga appropriata in SQL_ROW_UPDATED.  
  
 Se l'operazione viene annullata o si verifica un errore in **SQLParamData** o **SQLPutData**, dopo che **SQLSetPos** restituisce SQL_NEED_DATA e prima dell'invio dei dati per tutte le colonne data-at-execution, l'applicazione può chiamare solo **SQLCancel**, **SQLGetDiagField** **, SQLGetDiagRec, SQLGetFunctions** **,** **SQLParamData** o **SQLPutData** per l'istruzione o la connessione associata all'istruzione. Se chiama qualsiasi altra funzione per l'istruzione o la connessione associata all'istruzione, la funzione restituisce SQL_ERROR e SQLSTATE HY010 (errore della sequenza di funzioni).  
  
 Se l'applicazione chiama **SQLCancel** mentre il driver necessita ancora di dati per le colonne data-at-execution, il driver Annulla l'operazione. L'applicazione può quindi chiamare di nuovo **SQLSetPos** . l'annullamento non influisce sullo stato del cursore o sulla posizione corrente del cursore.  
  
 Quando l'elenco SELECT della specifica della query associata al cursore contiene più di un riferimento alla stessa colonna, se viene generato un errore o se il driver ignora i riferimenti duplicati ed esegue le operazioni richieste è definito dal driver.  
  
## <a name="performing-bulk-operations"></a>Esecuzione di operazioni bulk  
 Se l'argomento *RowNumber* è 0, il driver esegue l'operazione specificata nell'argomento *Operation* per ogni riga del set di righe con un valore SQL_ROW_PROCEED nel campo della matrice dell'operazione di riga a cui punta SQL_ATTR_ROW_OPERATION_PTR attributo Statement. Si tratta di un valore valido dell'argomento *RowNumber* per un argomento *Operation* di SQL_DELETE, SQL_REFRESH o SQL_UPDATE, ma non SQL_POSITION. **SQLSetPos** con un' *operazione* di SQL_POSITION e un *RowNumber* uguale a 0 restituirà SQLSTATE HY109 (posizione del cursore non valida).  
  
 Se si verifica un errore relativo all'intero set di righe, ad esempio SQLSTATE HYT00 (timeout scaduto), il driver restituisce SQL_ERROR e il valore SQLSTATE appropriato. Il contenuto dei buffer del set di righe non è definito e la posizione del cursore è invariata.  
  
 Se si verifica un errore relativo a una singola riga, il driver:  
  
-   Imposta l'elemento per la riga nella matrice di stato della riga a cui fa riferimento l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR SQL_ROW_ERROR.  
  
-   Inserisce uno o più Stati SQLSTATE aggiuntivi per l'errore nella coda degli errori e imposta il campo SQL_DIAG_ROW_NUMBER nella struttura dei dati di diagnostica.  
  
 Dopo l'elaborazione dell'errore o dell'avviso, se il driver completa l'operazione per le righe rimanenti nel set di righe, restituisce SQL_SUCCESS_WITH_INFO. Pertanto, per ogni riga che ha restituito un errore, la coda degli errori contiene zero o più SQLSTATE aggiuntivi. Se il driver interrompe l'operazione dopo che l'errore o l'avviso è stato elaborato, viene restituito SQL_ERROR.  
  
 Se il driver restituisce eventuali avvisi, ad esempio SQLSTATE 01004 (dati troncati), restituisce gli avvisi che si applicano all'intero set di righe o a righe sconosciute nel set di righe prima di restituire le informazioni sull'errore applicabili a righe specifiche. Restituisce gli avvisi per righe specifiche insieme a eventuali altre informazioni sull'errore relative a tali righe.  
  
 Se *RowNumber* è uguale a 0 e *Operation* è SQL_UPDATE, SQL_REFRESH o SQL_DELETE, il numero di righe su cui opera **SQLSetPos** è indicato dall'attributo SQL_ATTR_ROWS_FETCHED_PTR Statement.  
  
 Se *RowNumber* è uguale a 0 e *Operation* è SQL_DELETE, SQL_REFRESH o SQL_UPDATE, la riga corrente dopo l'operazione corrisponde alla riga corrente prima dell'operazione.  
  
## <a name="ignoring-a-row-in-a-bulk-operation"></a>Ignorare una riga in un'operazione bulk  
 La matrice dell'operazione Row può essere utilizzata per indicare che una riga nel set di righe corrente deve essere ignorata durante un'operazione bulk utilizzando **SQLSetPos**. Per indicare al driver di ignorare una o più righe durante un'operazione bulk, un'applicazione deve eseguire i passaggi seguenti:  
  
1.  Chiamare **SQLSetStmtAttr** per impostare l'attributo dell'istruzione SQL_ATTR_ROW_OPERATION_PTR in modo che punti a una matrice di SQLUSMALLINTs. Questo campo può essere impostato anche chiamando **SQLSetDescField** per impostare il campo di intestazione SQL_DESC_ARRAY_STATUS_PTR di ARD, che richiede che un'applicazione ottenga l'handle del descrittore.  
  
2.  Impostare ogni elemento della matrice dell'operazione Row su uno dei due valori seguenti:  
  
    -   SQL_ROW_IGNORE, per indicare che la riga è esclusa per l'operazione bulk.  
  
    -   SQL_ROW_PROCEED, per indicare che la riga è inclusa nell'operazione bulk. Si tratta del valore predefinito.  
  
3.  Chiamare **SQLSetPos** per eseguire l'operazione bulk.  
  
 Le regole seguenti si applicano alla matrice dell'operazione di riga:  
  
-   SQL_ROW_IGNORE e SQL_ROW_PROCEED influiscono solo sulle operazioni bulk che usano **SQLSetPos** con un' *operazione* di SQL_DELETE o SQL_UPDATE. Non influiscono sulle chiamate a **SQLSetPos** con un' *operazione* di SQL_REFRESH o SQL_POSITION.  
  
-   Per impostazione predefinita, il puntatore è impostato su null.  
  
-   Se il puntatore è null, tutte le righe vengono aggiornate come se tutti gli elementi fossero impostati su SQL_ROW_PROCEED.  
  
-   L'impostazione di un elemento su SQL_ROW_PROCEED non garantisce che l'operazione venga eseguita in una determinata riga. Se, ad esempio, una determinata riga del set di righe presenta lo stato SQL_ROW_ERROR, il driver potrebbe non essere in grado di aggiornare la riga indipendentemente dal fatto che l'applicazione sia stata specificata SQL_ROW_PROCEED. Un'applicazione deve sempre controllare la matrice di stato della riga per verificare se l'operazione è stata completata correttamente.  
  
-   SQL_ROW_PROCEED è definito come 0 nel file di intestazione. Un'applicazione può inizializzare la matrice dell'operazione di riga su 0 per elaborare tutte le righe.  
  
-   Se il numero di elemento "n" nella matrice dell'operazione di riga è impostato su SQL_ROW_IGNORE e **SQLSetPos** viene chiamato per eseguire un'operazione di aggiornamento o eliminazione in blocco, l'ennesima riga del set di righe rimane invariata dopo la chiamata a **SQLSetPos**.  
  
-   Un'applicazione deve impostare automaticamente una colonna di sola lettura su SQL_ROW_IGNORE.  
  
## <a name="ignoring-a-column-in-a-bulk-operation"></a>Ignorare una colonna in un'operazione bulk  
 Per evitare la diagnostica di elaborazione non necessaria generata da aggiornamenti tentati a una o più colonne di sola lettura, un'applicazione può impostare il valore del buffer di lunghezza/indicatore associato su SQL_COLUMN_IGNORE. Per ulteriori informazioni, vedere [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente un'applicazione consente a un utente di esplorare la tabella ORDERs e lo stato dell'ordine di aggiornamento. Il cursore è gestito da keyset con una dimensione del set di righe pari a 20 e usa il controllo della concorrenza ottimistica per confrontare le versioni di riga. Dopo il recupero di ogni set di righe, l'applicazione lo stampa e consente all'utente di selezionare e aggiornare lo stato di un ordine. L'applicazione utilizza **SQLSetPos** per posizionare il cursore sulla riga selezionata ed esegue un aggiornamento posizionato della riga. Per maggiore chiarezza, viene omessa la gestione degli errori.  
  
```cpp  
#define ROWS 20  
#define STATUS_LEN 6  
  
SQLCHAR        szStatus[ROWS][STATUS_LEN], szReply[3];  
SQLINTEGER     cbStatus[ROWS], cbOrderID;  
SQLUSMALLINT   rgfRowStatus[ROWS];  
SQLUINTEGER    sOrderID, crow = ROWS, irow;  
SQLHSTMT       hstmtS, hstmtU;  
  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CONCURRENCY, (SQLPOINTER) SQL_CONCUR_ROWVER, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER) SQL_CURSOR_KEYSET_DRIVEN, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_STATUS_PTR, (SQLPOINTER) rgfRowStatus, 0);  
SQLSetCursorName(hstmtS, "C1", SQL_NTS);  
SQLExecDirect(hstmtS, "SELECT ORDERID, STATUS FROM ORDERS ", SQL_NTS);  
  
SQLBindCol(hstmtS, 1, SQL_C_ULONG, &sOrderID, 0, &cbOrderID);  
SQLBindCol(hstmtS, 2, SQL_C_CHAR, szStatus, STATUS_LEN, &cbStatus);  
  
while ((retcode == SQLFetchScroll(hstmtS, SQL_FETCH_NEXT, 0)) != SQL_ERROR) {  
   if (retcode == SQL_NO_DATA_FOUND)  
      break;  
   for (irow = 0; irow < crow; irow++) {  
      if (rgfRowStatus[irow] != SQL_ROW_DELETED)  
         printf("%2d %5d %*s\n", irow+1, sOrderID, NAME_LEN-1, szStatus[irow]);  
   }  
   while (TRUE) {  
      printf("\nRow number to update?");  
      gets_s(szReply, 3);  
      irow = atoi(szReply);  
      if (irow > 0 && irow <= crow) {  
         printf("\nNew status?");  
         gets_s(szStatus[irow-1], (ROWS * STATUS_LEN));  
         SQLSetPos(hstmtS, irow, SQL_POSITION, SQL_LOCK_NO_CHANGE);  
         SQLPrepare(hstmtU,  
          "UPDATE ORDERS SET STATUS=? WHERE CURRENT OF C1", SQL_NTS);  
         SQLBindParameter(hstmtU, 1, SQL_PARAM_INPUT,  
            SQL_C_CHAR, SQL_CHAR,  
            STATUS_LEN, 0, szStatus[irow], 0, NULL);  
         SQLExecute(hstmtU);  
      } else if (irow == 0) {  
         break;  
      }  
   }  
}  
```  
  
 Per altri esempi, vedere [istruzioni Update e Delete posizionate](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md) e [aggiornamento delle righe nel set di righe con SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Esecuzione di operazioni bulk che non si riferiscono alla posizione del cursore a blocchi|[Funzione SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di un singolo campo di un descrittore|[Funzione SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Recupero di più campi di un descrittore|[Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Impostazione di un singolo campo di un descrittore|[Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Impostazione di più campi di un descrittore|[Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
