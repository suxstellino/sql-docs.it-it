---
description: Funzione SQLBindCol
title: Funzione SQLBindCol | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBindCol
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindCol
helpviewer_keywords:
- SQLBindCol function [ODBC]
ms.assetid: 41a37655-84cd-423f-9daa-e0b47b88dc54
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d057b3e7b5842cafba2be18afa21f2d528175ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181095"
---
# <a name="sqlbindcol-function"></a>Funzione SQLBindCol
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLBindCol** associa i buffer di dati dell'applicazione alle colonne nel set di risultati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLBindCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *ColumnNumber*  
 Input Numero della colonna del set di risultati da associare. Le colonne sono numerate in base a un ordine crescente di colonne a partire da 0, dove la colonna 0 è la colonna del segnalibro. Se non vengono utilizzati segnalibri, ovvero l'attributo SQL_ATTR_USE_BOOKMARKS istruzione è impostato su SQL_UB_OFF, i numeri di colonna iniziano da 1.  
  
 *TargetType*  
 Input Identificatore del tipo di dati C del \* buffer *TargetValuePtr* . Quando si recuperano dati dall'origine dati con **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** o **SQLSetPos**, il driver converte i dati in questo tipo; Quando invia dati all'origine dati con **SQLBulkOperations** o **SQLSetPos**, il driver converte i dati da questo tipo. Per un elenco dei tipi di dati C e degli identificatori di tipi validi, vedere la sezione tipi di dati [c](../../../odbc/reference/appendixes/c-data-types.md) in Appendice D: tipi di dati.  
  
 Se l'argomento *targetType* è un tipo di dati interval, per i dati vengono utilizzati rispettivamente la precisione principale (2) e la precisione dell'intervallo predefinito (6), come impostato nei campi SQL_DESC_DATETIME_INTERVAL_PRECISION e SQL_DESC_PRECISION di ARD. Se l'argomento *targetType* è SQL_C_NUMERIC, per i dati viene usata la precisione predefinita (definita dal driver) e la scala predefinita (0), come impostato nei campi SQL_DESC_PRECISION e SQL_DESC_SCALE di ARD. Se la precisione o la scala predefinita non è appropriata, l'applicazione deve impostare in modo esplicito il campo del descrittore appropriato mediante una chiamata a **SQLSetDescField** o **SQLSetDescRec**.  
  
 È inoltre possibile specificare un tipo di dati C esteso. Per ulteriori informazioni, vedere [tipi di dati C in ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 [Input/output posticipato] Puntatore al buffer di dati da associare alla colonna. **SQLFetch** e **SQLFetchScroll** restituiscono i dati in questo buffer. **SQLBulkOperations** restituisce i dati in questo buffer quando l' *operazione* è SQL_FETCH_BY_BOOKMARK; Recupera i dati da questo buffer quando l' *operazione* è SQL_ADD o SQL_UPDATE_BY_BOOKMARK. **SQLSetPos** restituisce i dati in questo buffer quando l' *operazione* è SQL_REFRESH; Recupera i dati da questo buffer quando l' *operazione* viene SQL_UPDATE.  
  
 Se *TargetValuePtr* è un puntatore null, il driver Annulla l'associazione del buffer dei dati per la colonna. Un'applicazione può annullare il binding di tutte le colonne chiamando **SQLFreeStmt** con l'opzione SQL_UNBIND. Un'applicazione può disassociare il buffer dei dati per una colonna, ma è ancora associato a un buffer di lunghezza/indicatore per la colonna, se l'argomento *TargetValuePtr* nella chiamata a **SQLBindCol** è un puntatore null, ma l'argomento *StrLen_or_IndPtr* è un valore valido.  
  
 *BufferLength*  
 Input Lunghezza del buffer \* *TargetValuePtr* in byte.  
  
 Il driver usa *bufferLength* per evitare di scrivere oltre la fine del \* buffer *TargetValuePtr* quando restituisce dati a lunghezza variabile, ad esempio dati di tipo carattere o binario. Si noti che il driver conta il carattere di terminazione null quando restituisce dati di tipo carattere a \* *TargetValuePtr*. \**TargetValuePtr* deve quindi contenere spazio per il carattere di terminazione null oppure il driver tronca i dati.  
  
 Quando il driver restituisce dati a lunghezza fissa, ad esempio un numero intero o una struttura di data, il driver ignora *bufferLength* e presuppone che il buffer sia sufficientemente grande da consentire l'inserimento dei dati. Pertanto, è importante che l'applicazione allochi un buffer sufficientemente grande per i dati a lunghezza fissa o che il driver scriva oltre la fine del buffer.  
  
 **SQLBindCol** restituisce SQLSTATE HY090 (lunghezza di stringa o di buffer non valida) quando *bufferLength* è minore di 0 ma non quando *bufferLength* è 0. Tuttavia, se *targetType* specifica un tipo di carattere, un'applicazione non deve impostare *bufferLength* su 0, perché i driver conformi a cli ISO restituiscono SQLSTATE HY090 (lunghezza di stringa o di buffer non valida) in questo caso.  
  
 *StrLen_or_IndPtr*  
 [Input/output posticipato] Puntatore al buffer di lunghezza/indicatore da associare alla colonna. **SQLFetch** e **SQLFetchScroll** restituiscono un valore in questo buffer. **SQLBulkOperations** recupera un valore da questo buffer quando l' *operazione* è SQL_ADD, SQL_UPDATE_BY_BOOKMARK o SQL_DELETE_BY_BOOKMARK. Quando l' *operazione* è SQL_FETCH_BY_BOOKMARK, **SQLBulkOperations** restituisce un valore in questo buffer. **SQLSetPos** restituisce un valore in questo buffer quando l' *operazione* è SQL_REFRESH; Recupera un valore da questo buffer quando l' *operazione* viene SQL_UPDATE.  
  
 **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** e **SQLSetPos** possono restituire i valori seguenti nel buffer di lunghezza/indicatore:  
  
-   Lunghezza dei dati disponibili per la restituzione  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 L'applicazione può inserire i valori seguenti nel buffer di lunghezza/indicatore per l'uso con **SQLBulkOperations** o **SQLSetPos**:  
  
-   Lunghezza dei dati inviati  
  
-   SQL_NTS  
  
-   SQL_NULL_DATA  
  
-   SQL_DATA_AT_EXEC  
  
-   Risultato della macro SQL_LEN_DATA_AT_EXEC  
  
-   SQL_COLUMN_IGNORE  
  
 Se il buffer dell'indicatore e il buffer di lunghezza sono buffer distinti, il buffer dell'indicatore può restituire solo SQL_NULL_DATA, mentre il buffer di lunghezza può restituire tutti gli altri valori.  
  
 Per ulteriori informazioni, vedere funzione [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md), funzione [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)e utilizzo di [valori di lunghezza/indicatore](../../../odbc/reference/develop-app/using-length-and-indicator-values.md).  
  
 Se *StrLen_or_IndPtr* è un puntatore null, non viene utilizzato alcun valore di lunghezza o indicatore. Si tratta di un errore durante il recupero dei dati e i dati sono NULL.  
  
 Vedere [informazioni su ODBC 64 bit](../../../odbc/reference/odbc-64-bit-information.md), se l'applicazione viene eseguita su un sistema operativo a 64 bit.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLBindCol** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLBindCol** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|(DM) l'argomento *ColumnNumber* è 0 e l'argomento *targetType* non è stato SQL_C_BOOKMARK o SQL_C_VARBOOKMARK.|  
|07009|Indice del descrittore non valido|Il valore specificato per l'argomento *ColumnNumber* supera il numero massimo di colonne nel set di risultati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY003|Tipo di buffer dell'applicazione non valido|L'argomento *targetType* non è né un tipo di dati valido né SQL_C_DEFAULT.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stato chiamato **SQLBindCol** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *bufferLength* è minore di 0.<br /><br /> (DM) il driver era ODBC 2. driver *x* , l'argomento *ColumnNumber* è stato impostato su 0 e il valore specificato per l'argomento *bufferLength* non è uguale a 4.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta la conversione specificata dalla combinazione dell'argomento *targetType* e del tipo di dati SQL specifico del driver della colonna corrispondente.<br /><br /> L'argomento *ColumnNumber* è 0 e il driver non supporta i segnalibri.<br /><br /> Il driver supporta solo ODBC 2. *x* e l'argomento *targetType* è uno dei seguenti:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> e uno dei tipi di dati interval C elencati nei tipi di dati [c](../../../odbc/reference/appendixes/c-data-types.md) in Appendice D: tipi di dati.<br /><br /> Il driver supporta solo le versioni ODBC precedenti alla 3,50 e l'argomento *targetType* è stato SQL_C_GUID.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 **SQLBindCol** viene utilizzato per associare, o *associare,* le colonne nel set di risultati a buffer di dati e buffer di lunghezza/indicatore nell'applicazione. Quando l'applicazione chiama **SQLFetch**, **SQLFetchScroll** o **SQLSetPos** per recuperare i dati, il driver restituisce i dati per le colonne con binding nei buffer specificati. Per ulteriori informazioni, vedere [funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Quando l'applicazione chiama **SQLBulkOperations** per aggiornare o inserire una riga o **SQLSetPos** per aggiornare una riga, il driver recupera i dati per le colonne con binding dai buffer specificati. per altre informazioni, vedere [funzione SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) o [funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md). Per ulteriori informazioni sull'associazione, vedere [recupero di risultati (di base)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 Si noti che non è necessario associare le colonne per recuperare i dati. Un'applicazione può anche chiamare **SQLGetData** per recuperare dati dalle colonne. Sebbene sia possibile associare alcune colonne in una riga e chiamare **SQLGetData** per altri, questo è soggetto ad alcune restrizioni. Per ulteriori informazioni, vedere [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
## <a name="binding-unbinding-and-rebinding-columns"></a>Associazione, annullamento dell'associazione e riassociazione delle colonne  
 Una colonna può essere associata, non associata o riassociata in qualsiasi momento, anche dopo che i dati sono stati recuperati dal set di risultati. Il nuovo binding viene applicato la volta successiva che viene chiamata una funzione che usa associazioni. Si supponga, ad esempio, che un'applicazione associ le colonne in un set di risultati e chiama **SQLFetch**. Il driver restituisce i dati nei buffer associati. Si supponga ora che l'applicazione associ le colonne a un set di buffer diverso. Il driver non inserisce i dati per la riga appena recuperata nei buffer appena associati. Al contrario, attende il completamento della chiamata a **SQLFetch** e quindi inserisce i dati per la riga successiva nei buffer appena associati.  
  
> [!NOTE]  
>  L'attributo Statement SQL_ATTR_USE_BOOKMARKS deve sempre essere impostato prima di associare una colonna alla colonna 0. Questa operazione non è necessaria, ma è fortemente consigliata.  
  
## <a name="binding-columns"></a>Associazione di colonne  
 Per associare una colonna, un'applicazione chiama **SQLBindCol** e passa il numero di colonna, il tipo, l'indirizzo e la lunghezza di un buffer di dati e l'indirizzo di un buffer di lunghezza/indicatore. Per informazioni sul modo in cui vengono usati questi indirizzi, vedere "indirizzi del buffer" più avanti in questa sezione. Per ulteriori informazioni sulle colonne di binding, vedere [utilizzo di SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md).  
  
 L'uso di questi buffer è posticipato; ovvero, l'applicazione li associa in **SQLBindCol** , ma il driver li accede da altre funzioni, ovvero **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** o **SQLSetPos**. È responsabilità dell'applicazione verificare che i puntatori specificati in **SQLBindCol** rimangano validi fino a quando l'associazione rimane attiva. Se l'applicazione consente a questi puntatori di diventare non validi, ad esempio, libera un buffer e quindi chiama una funzione che prevede che siano validi, le conseguenze non sono definite. Per ulteriori informazioni, vedere [buffer posticipati](../../../odbc/reference/develop-app/deferred-buffers.md).  
  
 Il binding rimane attivo fino a quando non viene sostituito da una nuova associazione, la colonna non è associata o l'istruzione viene liberata.  
  
## <a name="unbinding-columns"></a>Annullamento dell'associazione di colonne  
 Per annullare l'associazione di una singola colonna, un'applicazione chiama **SQLBindCol** con *ColumnNumber* impostato sul numero di tale colonna e *TargetValuePtr* impostato su un puntatore null. Se *ColumnNumber* fa riferimento a una colonna non associata, **SQLBindCol** restituisce ancora SQL_SUCCESS.  
  
 Per annullare l'associazione di tutte le colonne, un'applicazione chiama **SQLFreeStmt** con *fOption* impostato su SQL_UNBIND. Questa operazione può essere eseguita anche impostando il campo SQL_DESC_COUNT di ARD su zero.  
  
## <a name="rebinding-columns"></a>Riassociazione di colonne  
 Per modificare un'associazione, un'applicazione può eseguire una delle due operazioni seguenti:  
  
-   Chiamare **SQLBindCol** per specificare una nuova associazione per una colonna già associata. Il driver sovrascrive il vecchio binding con quello nuovo.  
  
-   Specificare un offset da aggiungere all'indirizzo del buffer specificato dalla chiamata di binding a **SQLBindCol**. Per ulteriori informazioni, vedere la sezione successiva "offset di associazione".  
  
## <a name="binding-offsets"></a>Offset di binding  
 Un offset di binding è un valore aggiunto agli indirizzi dei buffer di dati e di lunghezza/indicatore (come specificato nell'argomento *TargetValuePtr* e *StrLen_or_IndPtr* ) prima che vengano dereferenziati. Quando si utilizzano gli offset, i binding sono un "modello" di come sono disposti i buffer dell'applicazione e l'applicazione può spostare questo "modello" in aree di memoria diverse modificando l'offset. Poiché lo stesso offset viene aggiunto a ogni indirizzo in ogni binding, gli offset relativi tra i buffer per colonne diverse devono essere uguali all'interno di ogni set di buffer. Questa operazione è sempre vera quando viene utilizzata l'associazione per riga. Quando si utilizza l'associazione per colonna, l'applicazione deve disporre con particolare attenzione dei buffer.  
  
 L'utilizzo di un offset di binding ha sostanzialmente lo stesso effetto della riassociazione di una colonna chiamando **SQLBindCol**. La differenza è che una nuova chiamata a **SQLBindCol** specifica nuovi indirizzi per il buffer di dati e il buffer di lunghezza/indicatore, mentre l'utilizzo di un offset di binding non modifica gli indirizzi, ma aggiunge semplicemente un offset. L'applicazione può specificare un nuovo offset ogni volta che vuole e questo offset viene sempre aggiunto agli indirizzi associati originariamente. In particolare, se l'offset è impostato su 0 o se l'attributo dell'istruzione è impostato su un puntatore null, il driver utilizza gli indirizzi associati originariamente.  
  
 Per specificare un offset dell'associazione, l'applicazione imposta l'attributo dell'istruzione SQL_ATTR_ROW_BIND_OFFSET_PTR sull'indirizzo di un buffer SQLINTEGER. Prima che l'applicazione chiami una funzione che usa associazioni, inserisce un offset in byte in questo buffer. Per determinare l'indirizzo del buffer da utilizzare, il driver aggiunge l'offset all'indirizzo nell'associazione. La somma dell'indirizzo e dell'offset deve essere un indirizzo valido, ma non è necessario che l'indirizzo a cui viene aggiunto l'offset sia valido. Per ulteriori informazioni sull'utilizzo degli offset di associazione, vedere "indirizzi del buffer" più avanti in questa sezione.  
  
## <a name="binding-arrays"></a>Associazioni di matrici  
 Se la dimensione del set di righe (il valore dell'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE) è maggiore di 1, l'applicazione associa matrici di buffer anziché singoli buffer. Per ulteriori informazioni, vedere [blocchi cursori](../../../odbc/reference/develop-app/block-cursors.md).  
  
 L'applicazione può associare matrici in due modi:  
  
-   Associare una matrice a ogni colonna. Questa operazione viene definita *associazione per colonna* perché ogni struttura di dati (matrice) contiene dati per una singola colonna.  
  
-   Definire una struttura per conservare i dati per un'intera riga e associare una matrice di queste strutture. Questa operazione viene definita *associazione per riga* perché ogni struttura di dati contiene i dati per una singola riga.  
  
 Ogni matrice di buffer deve avere almeno un numero di elementi pari alla dimensione del set di righe.  
  
> [!NOTE]  
>  Un'applicazione deve verificare che l'allineamento sia valido. Per ulteriori informazioni sulle considerazioni sull'allineamento, vedere [allineamento](../../../odbc/reference/develop-app/alignment.md).  
  
## <a name="column-wise-binding"></a>Associazione per colonna  
 Nell'associazione a livello di colonna, l'applicazione associa a ogni colonna dati separati e matrici di lunghezza/indicatore.  
  
 Per utilizzare l'associazione per colonna, l'applicazione imposta innanzitutto l'attributo dell'istruzione SQL_ATTR_ROW_BIND_TYPE su SQL_BIND_BY_COLUMN. (Impostazione predefinita). Per ogni colonna da associare, l'applicazione esegue i passaggi seguenti:  
  
1.  Alloca una matrice di buffer di dati.  
  
2.  Alloca una matrice di buffer di lunghezza/indicatore.  
  
    > [!NOTE]  
    >  Se l'applicazione scrive direttamente nei descrittori quando viene utilizzata l'associazione per colonna, è possibile utilizzare matrici separate per i dati di lunghezza e indicatore.  
  
3.  Chiama **SQLBindCol** con gli argomenti seguenti:  
  
    -   *TargetType* è il tipo di un singolo elemento nella matrice di buffer di dati.  
  
    -   *TargetValuePtr* è l'indirizzo della matrice di buffer di dati.  
  
    -   *BufferLength* è la dimensione di un singolo elemento nella matrice di buffer di dati. L'argomento *bufferLength* viene ignorato quando i dati sono dati a lunghezza fissa.  
  
    -   *StrLen_or_IndPtr* è l'indirizzo della matrice di lunghezza/indicatore.  
  
 Per ulteriori informazioni sull'utilizzo di queste informazioni, vedere "indirizzi del buffer" più avanti in questa sezione. Per ulteriori informazioni sull'associazione per colonna, vedere [associazione per colonna](../../../odbc/reference/develop-app/column-wise-binding.md).  
  
## <a name="row-wise-binding"></a>Associazione per riga  
 Nell'associazione per riga, l'applicazione definisce una struttura che contiene dati e buffer di lunghezza/indicatore per ogni colonna da associare.  
  
 Per utilizzare l'associazione per riga, l'applicazione esegue i passaggi seguenti:  
  
1.  Definisce una struttura per includere una singola riga di dati (inclusi sia i dati che i buffer di lunghezza/indicatore) e alloca una matrice di queste strutture.  
  
    > [!NOTE]  
    >  Se l'applicazione scrive direttamente nei descrittori quando viene utilizzata l'associazione per riga, è possibile utilizzare campi separati per i dati di lunghezza e indicatore.  
  
2.  Imposta l'attributo dell'istruzione SQL_ATTR_ROW_BIND_TYPE sulla dimensione della struttura che contiene una singola riga di dati o sulle dimensioni di un'istanza di un buffer in cui verranno associate le colonne di risultati. La lunghezza deve includere lo spazio per tutte le colonne associate e qualsiasi riempimento della struttura o del buffer, per assicurarsi che quando l'indirizzo di una colonna associata venga incrementato con la lunghezza specificata, il risultato punterà all'inizio della stessa colonna nella riga successiva. Quando si usa l'operatore *sizeof* in ANSI C, questo comportamento è garantito.  
  
3.  Chiama **SQLBindCol** con gli argomenti seguenti per ogni colonna da associare:  
  
    -   *TargetType* è il tipo del membro del buffer di dati da associare alla colonna.  
  
    -   *TargetValuePtr* è l'indirizzo del membro del buffer di dati nel primo elemento della matrice.  
  
    -   *BufferLength* è la dimensione del membro del buffer di dati.  
  
    -   *StrLen_or_IndPtr* è l'indirizzo del membro di lunghezza/indicatore da associare.  
  
 Per ulteriori informazioni sull'utilizzo di queste informazioni, vedere "indirizzi del buffer" più avanti in questa sezione. Per ulteriori informazioni sull'associazione per colonna, vedere [associazione per riga](../../../odbc/reference/develop-app/row-wise-binding.md).  
  
## <a name="buffer-addresses"></a>Indirizzi buffer  
 L' *indirizzo del buffer* è l'indirizzo effettivo del buffer di dati o di lunghezza/indicatore. Il driver calcola l'indirizzo del buffer appena prima di scrivere nei buffer, ad esempio durante il recupero. Viene calcolato dalla formula seguente, che usa gli indirizzi specificati negli argomenti *TargetValuePtr* e *StrLen_or_IndPtr* , l'offset dell'associazione e il numero di riga:  
  
 *Indirizzo associato*  +  *Offset binding* + ((*numero di riga* -1) x *dimensioni elemento*)  
  
 dove le variabili della formula sono definite come descritto nella tabella seguente.  
  
|Variabile|Descrizione|  
|--------------|-----------------|  
|*Indirizzo associato*|Per i buffer di dati, l'indirizzo specificato con l'argomento *TargetValuePtr* in **SQLBindCol**.<br /><br /> Per i buffer di lunghezza/indicatore, l'indirizzo specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindCol**. Per ulteriori informazioni, vedere "Commenti aggiuntivi" nella sezione "descrittori e SQLBindCol".<br /><br /> Se l'indirizzo associato è 0, non viene restituito alcun valore di dati, anche se l'indirizzo calcolato dalla formula precedente è diverso da zero.|  
|*Offset binding*|Se viene utilizzata l'associazione per riga, il valore archiviato in corrispondenza dell'indirizzo specificato con l'attributo dell'istruzione SQL_ATTR_ROW_BIND_OFFSET_PTR.<br /><br /> Se viene utilizzata l'associazione per colonna o se il valore dell'attributo dell'istruzione SQL_ATTR_ROW_BIND_OFFSET_PTR è un puntatore null, l' *offset dell'associazione* è 0.|  
|*Numero di riga*|Numero in base 1 della riga nel set di righe. Per i recuperi a riga singola, che corrispondono all'impostazione predefinita, questo valore è 1.|  
|*Dimensioni elemento*|Dimensioni di un elemento nella matrice associata.<br /><br /> Se viene utilizzata l'associazione per colonna, è **sizeof (SQLINTEGER)** per i buffer di lunghezza/indicatore. Per i buffer di dati, è il valore dell'argomento *bufferLength* in **SQLBindCol** se il tipo di dati è a lunghezza variabile e la dimensione del tipo di dati se il tipo di dati è a lunghezza fissa.<br /><br /> Se viene utilizzata l'associazione per riga, si tratta del valore dell'attributo SQL_ATTR_ROW_BIND_TYPE Statement per i buffer di dati e di lunghezza/indicatore.|  
  
## <a name="descriptors-and-sqlbindcol"></a>Descrittori e SQLBindCol  
 Nelle sezioni seguenti viene descritto il modo in cui **SQLBindCol** interagisce con i descrittori.  
  
> [!CAUTION]  
>  La chiamata di **SQLBindCol** per un'istruzione può influire su altre istruzioni. Questo errore si verifica quando il ARD associato all'istruzione viene allocato in modo esplicito ed è associato anche ad altre istruzioni. Poiché **SQLBindCol** modifica il descrittore, le modifiche si applicano a tutte le istruzioni a cui è associato questo descrittore. Se questo non è il comportamento necessario, l'applicazione deve annullare l'associazione di questo descrittore dalle altre istruzioni prima di chiamare **SQLBindCol**.  
  
## <a name="argument-mappings"></a>Mapping degli argomenti  
 A livello concettuale, **SQLBindCol** esegue i passaggi seguenti in sequenza:  
  
1.  Chiama **SQLGetStmtAttr** per ottenere l'handle ARD.  
  
2.  Chiama **SQLGetDescField** per ottenere il campo SQL_DESC_COUNT del descrittore e se il valore nell'argomento *ColumnNumber* supera il valore di SQL_DESC_COUNT, chiama **SQLSetDescField** per aumentare il valore di SQL_DESC_COUNT a *ColumnNumber*.  
  
3.  Chiama **SQLSetDescField** più volte per assegnare valori ai seguenti campi di ARD:  
  
    -   Imposta SQL_DESC_TYPE e SQL_DESC_CONCISE_TYPE sul valore di *targetType*, ad eccezione del fatto che se *targetType* è uno degli identificatori concisi di un sottotipo DateTime o Interval, imposta SQL_DESC_TYPE su SQL_DATETIME o SQL_INTERVAL, rispettivamente. imposta SQL_DESC_CONCISE_TYPE sull'identificatore conciso; e imposta SQL_DESC_DATETIME_INTERVAL_CODE sul sottocodice DateTime o Interval corrispondente.  
  
    -   Imposta uno o più SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE e SQL_DESC_DATETIME_INTERVAL_PRECISION, come appropriato per *targetType*.  
  
    -   Imposta il campo SQL_DESC_OCTET_LENGTH sul valore di *bufferLength*.  
  
    -   Imposta il campo SQL_DESC_DATA_PTR sul valore di *TargetValuePtr*.  
  
    -   Imposta il campo SQL_DESC_INDICATOR_PTR sul valore di *StrLen_or_IndPtr*. Vedere il paragrafo seguente.  
  
    -   Imposta il campo SQL_DESC_OCTET_LENGTH_PTR sul valore di *StrLen_or_IndPtr*. Vedere il paragrafo seguente.  
  
 La variabile a cui fa riferimento l'argomento *StrLen_or_IndPtr* viene utilizzata per le informazioni su indicatore e lunghezza. Se un'operazione di recupero rileva un valore null per la colonna, archivia SQL_NULL_DATA in questa variabile; in caso contrario, archivia la lunghezza dei dati in questa variabile. Il passaggio di un puntatore null come *StrLen_or_IndPtr* impedisce all'operazione di recupero di restituire la lunghezza dei dati, ma fa in modo che il recupero abbia esito negativo se rileva un valore null e non è in grado di restituire SQL_NULL_DATA.  
  
 Se la chiamata a **SQLBindCol** ha esito negativo, il contenuto dei campi di descrizione impostati in ARD non è definito e il valore del campo SQL_DESC_COUNT di ARD è invariato.  
  
## <a name="implicit-resetting-of-count-field"></a>Reimpostazione implicita del campo COUNT  
 **SQLBindCol** imposta SQL_DESC_COUNT sul valore dell'argomento *ColumnNumber* solo quando aumenterà il valore di SQL_DESC_COUNT. Se il valore nell'argomento *TargetValuePtr* è un puntatore null e il valore nell'argomento *ColumnNumber* è uguale a SQL_DESC_COUNT (ovvero, quando si annulla l'associazione della colonna con binding più alto), SQL_DESC_COUNT viene impostato sul numero della colonna con binding rimanente più alta.  
  
## <a name="cautions-regarding-sql_default"></a>Avvertenze relative SQL_DEFAULT  
 Per recuperare correttamente i dati della colonna, l'applicazione deve determinare correttamente la lunghezza e il punto iniziale dei dati nel buffer dell'applicazione. Quando l'applicazione specifica un *targetType* esplicito, vengono rilevate facilmente le idee errate. Tuttavia, quando l'applicazione specifica un *targetType* di SQL_DEFAULT, **SQLBindCol** può essere applicato a una colonna di un tipo di dati diverso da quello previsto dall'applicazione, da modifiche ai metadati o applicando il codice a una colonna diversa. In questo caso, l'applicazione potrebbe non determinare sempre l'inizio o la lunghezza dei dati della colonna recuperata. Questo può causare errori di dati non segnalati o violazioni di memoria.  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente, un'applicazione esegue un'istruzione **Select** nella tabella Customers per restituire un set di risultati di ID cliente, nomi e numeri di telefono, ordinati in base al nome. Chiama quindi **SQLBindCol** per associare le colonne di dati ai buffer locali. Infine, l'applicazione recupera ogni riga di dati con **SQLFetch** e stampa il nome, l'ID e il numero di telefono di ogni cliente.  
  
 Per altri esempi di codice, vedere funzione [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [funzione SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)e [funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp  
// SQLBindCol_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
  
#define UNICODE  
#include <sqlext.h>  
  
#define NAME_LEN 50  
#define PHONE_LEN 60
  
void show_error() {  
   printf("error\n");  
}  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
   SQLWCHAR szName[NAME_LEN], szPhone[PHONE_LEN], sCustID[NAME_LEN];  
   SQLLEN cbName = 0, cbCustID = 0, cbPhone = 0;  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLWCHAR*) L"NorthWind", SQL_NTS, (SQLWCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {   
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               retcode = SQLExecDirect(hstmt, (SQLWCHAR *) L"SELECT CustomerID, ContactName, Phone FROM CUSTOMERS ORDER BY 2, 1, 3", SQL_NTS);  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
                  // Bind columns 1, 2, and 3  
                  retcode = SQLBindCol(hstmt, 1, SQL_C_WCHAR, &sCustID, 100, &cbCustID);  
                  retcode = SQLBindCol(hstmt, 2, SQL_C_WCHAR, szName, NAME_LEN, &cbName);  
                  retcode = SQLBindCol(hstmt, 3, SQL_C_WCHAR, szPhone, PHONE_LEN, &cbPhone);   
  
                  // Fetch and print each row of data. On an error, display a message and exit.  
                  for (int i=0 ; ; i++) {  
                     retcode = SQLFetch(hstmt);  
                     if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
                        show_error();  
                     if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                     {
                        //replace wprintf with printf
                        //%S with %ls
                        //warning C4477: 'wprintf' : format string '%S' requires an argument of type 'char *'
                        //but variadic argument 2 has type 'SQLWCHAR *'
                        //wprintf(L"%d: %S %S %S\n", i + 1, sCustID, szName, szPhone);  
                        printf("%d: %ls %ls %ls\n", i + 1, sCustID, szName, szPhone);  
                    }    
                     else  
                        break;  
                  }  
               }  
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLCancel(hstmt);  
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
  
 Vedere anche il [programma ODBC di esempio](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione di informazioni su una colonna in un set di risultati|[Funzione SQLDescribeCol ](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di più righe di dati|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Rilascio di buffer di colonna nell'istruzione|[SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Recupero di parte o di tutte le colonne di dati|[Funzione SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Restituzione del numero di colonne del set di risultati|[Funzione SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
