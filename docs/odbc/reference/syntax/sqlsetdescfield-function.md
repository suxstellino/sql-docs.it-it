---
description: Funzione SQLSetDescField
title: Funzione SQLSetDescField | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescField
helpviewer_keywords:
- SQLSetDescField function [ODBC]
ms.assetid: 8c544388-fe9d-4f94-a0ac-fa0b9c9c88a5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bd48da2caec1acba5cfe775a2baa1efaaf886aab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200971"
---
# <a name="sqlsetdescfield-function"></a>Funzione SQLSetDescField

**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLSetDescField** imposta il valore di un singolo campo di un record di descrittore.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
SQLRETURN SQLSetDescField(  
     SQLHDESC      DescriptorHandle,  
     SQLSMALLINT   RecNumber,  
     SQLSMALLINT   FieldIdentifier,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    BufferLength);  
```  
  
## <a name="arguments"></a>Argomenti  
 *DescriptorHandle*  
 Input Handle descrittore.  
  
 *RecNumber*  
 Input Indica il record del descrittore contenente il campo che l'applicazione cerca di impostare. I record del descrittore sono numerati da 0, con il numero di record 0 che è il record del segnalibro. L'argomento *RecNumber* viene ignorato per i campi di intestazione.  
  
 *FieldIdentifier*  
 Input Indica il campo del descrittore il cui valore deve essere impostato. Per ulteriori informazioni, vedere "argomento *FieldIdentifier* " nella sezione "Commenti".  
  
 *ValuePtr*  
 Input Puntatore a un buffer contenente le informazioni sul descrittore o un valore integer. Il tipo di dati dipende dal valore di *FieldIdentifier*. Se *ValuePtr* è un valore integer, può essere considerato come 8 byte (SQLLEN), 4 byte (SQLINTEGER) o 2 byte (SQLSMALLINT), a seconda del valore dell'argomento *FieldIdentifier* .  
  
 *BufferLength*  
 Input Se *FieldIdentifier* è un campo definito da ODBC e *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di **ValuePtr*. Per i dati di tipo stringa di caratteri, questo argomento deve contenere il numero di byte nella stringa.  
  
 Se *FieldIdentifier* è un campo definito da ODBC e *ValuePtr* è un numero intero, *bufferLength* viene ignorato.  
  
 Se *FieldIdentifier* è un campo definito dal driver, l'applicazione indica la natura del campo per Gestione driver impostando l'argomento *bufferLength* . *BufferLength* può avere i valori seguenti:  
  
-   Se *ValuePtr* è un puntatore a una stringa di caratteri, *bufferLength* è la lunghezza della stringa o del SQL_NTS.  
  
-   Se *ValuePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *bufferLength*. Questo inserisce un valore negativo in *bufferLength*.  
  
-   Se *ValuePtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *BufferLength* deve essere SQL_IS_POINTER.  
  
-   Se *ValuePtr* contiene un valore a lunghezza fissa, *bufferLength* è SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT, a seconda delle esigenze.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetDescField** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DESC e un *handle* di *DescriptorHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLSetDescField** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Il driver non supporta il valore specificato in *\* ValuePtr* (se *ValuePtr* è un puntatore) o il valore in *ValuePtr* (se *ValuePtr* è un valore Integer) oppure *\* ValuePtr* non è valido a causa di condizioni di lavoro di implementazione, quindi il driver sostituisce un valore simile. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07009|Indice del descrittore non valido|L'argomento *FieldIdentifier* è un campo record, l'argomento *RecNumber* è 0 e l'argomento *DescriptorHandle* fa riferimento a un handle DIP.<br /><br /> L'argomento *RecNumber* è minore di 0 e l'argomento *DescriptorHandle* fa riferimento a un ARD o a un APD.<br /><br /> L'argomento *RecNumber* è maggiore del numero massimo di colonne o parametri che l'origine dati è in grado di supportare e l'argomento *DescriptorHandle* fa riferimento a un oggetto APD o ARD.<br /><br /> (DM) l'argomento *FieldIdentifier* è stato SQL_DESC_COUNT e l'argomento *\* ValuePtr* è minore di 0.<br /><br /> L'argomento *RecNumber* è uguale a 0 e l'argomento *DescriptorHandle* fa riferimento a un oggetto APD allocato in modo implicito. Questo errore non si verifica con un descrittore di applicazione allocato in modo esplicito, perché non è noto se un descrittore di applicazione allocato in modo esplicito è un valore APD o ARD fino alla fase di esecuzione.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22001|Dati stringa, troncati a destra|L'argomento *FieldIdentifier* è stato SQL_DESC_NAME e l'argomento *bufferLength* è un valore maggiore di SQL_MAX_IDENTIFIER_LEN.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) *DescriptorHandle* è stato associato a un *statementHandle* per il quale è stata chiamata una funzione in esecuzione asincrona (non questa) ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* a cui il *DescriptorHandle* è stato associato e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *DescriptorHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLSetDescField** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati a *DescriptorHandle* e restituiti SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY016|Impossibile modificare il descrittore di una riga di implementazione|L'argomento *DescriptorHandle* è stato associato a un IRD e l'argomento *FieldIdentifier* non è SQL_DESC_ARRAY_STATUS_PTR o SQL_DESC_ROWS_PROCESSED_PTR.|  
|HY021|Informazioni descrittore incoerenti|I campi SQL_DESC_TYPE e SQL_DESC_DATETIME_INTERVAL_CODE non formano un tipo SQL ODBC valido o un tipo SQL valido specifico del driver (per IPD) o un tipo C ODBC valido (per APD o ARDs).<br /><br /> Le informazioni sul descrittore controllate durante una verifica di coerenza non erano coerenti. (Vedere "Verifica coerenza" in **SQLSetDescRec**).|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) *\* ValuePtr* è una stringa di caratteri e *bufferLength* è minore di zero, ma non è uguale a SQL_NTS.<br /><br /> (DM) il driver era un driver ODBC 2 *. x* , il descrittore era un ARD, l'argomento *ColumnNumber* è stato impostato su 0 e il valore specificato per l'argomento *bufferLength* non è uguale a 4.|  
|HY091|Identificatore del campo del descrittore non valido|Il valore specificato per l'argomento *FieldIdentifier* non è un campo definito da ODBC e non è un valore definito dall'implementazione.<br /><br /> L'argomento *FieldIdentifier* non è valido per l'argomento *DescriptorHandle* .<br /><br /> L'argomento *FieldIdentifier* è un campo di sola lettura definito da ODBC.|  
|HY092|Identificatore di attributo/opzione non valido|Il valore in *\* ValuePtr* non è valido per l'argomento *FieldIdentifier* .<br /><br /> L'argomento *FieldIdentifier* è stato SQL_DESC_UNNAMED e *ValuePtr* è stato SQL_NAMED.|  
|HY105|Tipo di parametro non valido|(DM) il valore specificato per il campo SQL_DESC_PARAMETER_TYPE non è valido. Per ulteriori informazioni, vedere la sezione "argomento *InputOutputType* " in **SQLBindParameter**.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere Novità di [ODBC 3,8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *DescriptorHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Un'applicazione può chiamare **SQLSetDescField** per impostare un qualsiasi campo del descrittore uno alla volta. Una chiamata a **SQLSetDescField** imposta un singolo campo in un singolo descrittore. Questa funzione può essere chiamata per impostare qualsiasi campo in qualsiasi tipo di descrittore, purché sia possibile impostare il campo. Vedere la tabella più avanti in questa sezione.  
  
> [!NOTE]  
>  Se una chiamata a **SQLSetDescField** ha esito negativo, il contenuto del record del descrittore identificato dall'argomento *RecNumber* non è definito.  
  
 È possibile chiamare altre funzioni per impostare più campi di descrizione con una singola chiamata della funzione. La funzione **SQLSetDescRec** imposta una serie di campi che influiscono sul tipo di dati e sul buffer associato a una colonna o a un parametro (i campi SQL_DESC_TYPE, SQL_DESC_DATETIME_INTERVAL_CODE, SQL_DESC_OCTET_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_DATA_PTR, SQL_DESC_OCTET_LENGTH_PTR e SQL_DESC_INDICATOR_PTR). **SQLBindCol** o **SQLBindParameter** può essere usato per creare una specifica completa per l'associazione di una colonna o un parametro. Queste funzioni impostano un gruppo specifico di campi di descrizione con una chiamata di funzione.  
  
 È possibile chiamare **SQLSetDescField** per modificare i buffer di associazione aggiungendo un offset ai puntatori di binding (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR o SQL_DESC_OCTET_LENGTH_PTR). Questa operazione modifica i buffer di associazione senza chiamare **SQLBindCol** o **SQLBindParameter**, che consente a un'applicazione di modificare SQL_DESC_DATA_PTR senza modificare altri campi, ad esempio SQL_DESC_DATA_TYPE.  
  
 Se un'applicazione chiama **SQLSetDescField** per impostare un campo diverso da SQL_DESC_COUNT o i campi posticipati SQL_DESC_DATA_PTR, SQL_DESC_OCTET_LENGTH_PTR o SQL_DESC_INDICATOR_PTR, il record diventa non associato.  
  
 I campi di intestazione del descrittore vengono impostati chiamando **SQLSetDescField** con il *FieldIdentifier* appropriato. Molti campi di intestazione sono anche attributi di istruzione, quindi possono essere impostati anche tramite una chiamata a **SQLSetStmtAttr**. Questo consente alle applicazioni di impostare un campo del descrittore senza prima ottenere un handle descrittore. Quando **SQLSetDescField** viene chiamato per impostare un campo di intestazione, l'argomento *RecNumber* viene ignorato.  
  
 Per impostare i campi dei segnalibri viene usato un *RecNumber* di 0.  
  
> [!NOTE]  
>  L'attributo Statement SQL_ATTR_USE_BOOKMARKS deve sempre essere impostato prima di chiamare **SQLSetDescField** per impostare i campi dei segnalibri. Sebbene non sia obbligatorio, si tratta di un'operazione fortemente consigliata.  
  
## <a name="sequence-of-setting-descriptor-fields"></a>Sequenza dei campi del descrittore dell'impostazione  
 Quando si impostano i campi del descrittore chiamando **SQLSetDescField**, l'applicazione deve seguire una sequenza specifica:  
  
1.  L'applicazione deve innanzitutto impostare il campo SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE o SQL_DESC_DATETIME_INTERVAL_CODE.  
  
2.  Una volta impostato uno di questi campi, l'applicazione può impostare un attributo di un tipo di dati e il driver imposta i campi dell'attributo del tipo di dati sui valori predefiniti appropriati per il tipo di dati. L'impostazione predefinita automatica dei campi attributo di tipo garantisce che il descrittore sia sempre pronto per l'utilizzo dopo che l'applicazione ha specificato un tipo di dati. Se l'applicazione imposta in modo esplicito un attributo del tipo di dati, esegue l'override dell'attributo predefinito.  
  
3.  Dopo che uno dei campi elencati nel passaggio 1 è stato impostato e sono stati impostati gli attributi del tipo di dati, l'applicazione può impostare SQL_DESC_DATA_PTR. Viene richiesta una verifica di coerenza dei campi del descrittore. Se l'applicazione modifica il tipo di dati o gli attributi dopo aver impostato il campo SQL_DESC_DATA_PTR, il driver imposta SQL_DESC_DATA_PTR su un puntatore null, annullando l'associazione del record. Questa operazione impone all'applicazione di completare i passaggi appropriati in sequenza, prima che il record del descrittore sia utilizzabile.  
  
## <a name="initialization-of-descriptor-fields"></a>Inizializzazione dei campi di descrizione  
 Quando viene allocato un descrittore, i campi del descrittore possono essere inizializzati su un valore predefinito, essere inizializzati senza un valore predefinito o essere indefiniti per il tipo di descrittore. Le tabelle seguenti indicano l'inizializzazione di ogni campo per ogni tipo di descrittore, con "D" che indica che il campo viene inizializzato con un valore predefinito e "ND" che indica che il campo viene inizializzato senza un valore predefinito. Se viene visualizzato un numero, il valore predefinito del campo corrisponde a tale numero. Le tabelle indicano anche se un campo è di lettura/scrittura (R/W) o di sola lettura (R).  
  
 I campi di un IRD hanno un valore predefinito solo dopo che l'istruzione è stata preparata o eseguita e il IRD è stato popolato, non quando è stato allocato l'handle o il descrittore dell'istruzione. Fino a quando il IRD non è stato popolato, qualsiasi tentativo di accedere a un campo di un IRD restituirà un errore.  
  
 Alcuni campi di descrizione sono definiti per uno o più tipi di descrittori (ARDs, IRDs e APD e IPD). Quando un campo non è definito per un tipo di descrittore, non è necessario per le funzioni che usano tale descrittore.  
  
 I campi a cui è possibile accedere da **SQLGetDescField** non possono essere impostati necessariamente da **SQLSetDescField**. I campi che possono essere impostati da **SQLSetDescField** sono elencati nelle tabelle seguenti.  
  
 L'inizializzazione dei campi di intestazione è illustrata nella tabella seguente.  
  
|Nome campo intestazione|Tipo|L/S|Predefinito|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_ALLOC_TYPE|SQLSMALLINT|ARD: R APD: R IRD: R R: R|ARD: SQL_DESC_ALLOC_AUTO per implicito o SQL_DESC_ALLOC_USER per esplicito<br /><br /> APD: SQL_DESC_ALLOC_AUTO per implicito o SQL_DESC_ALLOC_USER per esplicito<br /><br /> IRD: SQL_DESC_ALLOC_AUTO<br /><br /> DPI: SQL_DESC_ALLOC_AUTO|  
|SQL_DESC_ARRAY_SIZE|SQLULEN|ARD: R/W APD: R/W IRD: DIP non usato: non usato|ARD: [1] APD: [1] IRD: DIP non usato: non usato|  
|SQL_DESC_ARRAY_STATUS_PTR|SQLUSMALLINT*|ARD: R/W APD: R/W IRD: R/W DPI: R/W|ARD: null ptr APD: null ptr IRD: null ptr dpi: null ptr|  
|SQL_DESC_BIND_OFFSET_PTR|SQLLEN|ARD: R/W APD: R/W IRD: DIP non usato: non usato|ARD: null ptr APD: null ptr IRD: dpi non usato: non usato|  
|SQL_DESC_BIND_TYPE|SQLINTEGER|ARD: R/W APD: R/W IRD: DIP non usato: non usato|ARD: SQL_BIND_BY_COLUMN<br /><br /> APD: SQL_BIND_BY_COLUMN<br /><br /> IRD: non usato<br /><br /> DPI: non usato|  
|SQL_DESC_COUNT|SQLSMALLINT|ARD: R/W APD: R/W IRD: R R: R/W|ARD: 0 APD: 0 IRD: D DPI: 0|  
|SQL_DESC_ROWS_PROCESSED_PTR|SQLULEN|ARD: APD non usato: IRD non usato: R/W dpi: R/W|ARD: APD non usato: inutilizzato IRD: null ptr dpi: ptr null|  
  
 [1] questi campi vengono definiti solo quando il dpi viene popolato automaticamente dal driver. In caso contrario, non sono definite. Se un'applicazione tenta di impostare questi campi, viene restituito l'errore SQLSTATE HY091 (identificatore del campo del descrittore non valido).  
  
 L'inizializzazione dei campi di record è come illustrato nella tabella seguente.  
  
|Nome campo record|Tipo|L/S|Predefinito|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQLINTEGER|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_BASE_COLUMN_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_BASE_TABLE_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_CASE_SENSITIVE|SQLINTEGER|ARD: APD non usato: IRD non usato: R: r|ARD: APD non usato: IRD non usato: D dpi: D [1]|  
|SQL_DESC_CATALOG_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_CONCISE_TYPE|SQLSMALLINT|ARD: R/W APD: R/W IRD: R R: R/W|ARD: SQL_C_ APD PREDEFINITO: SQL_C_ VALORE PREDEFINITO IRD: D DPI: ND|  
|SQL_DESC_DATA_PTR|SQLPOINTER|ARD: R/W APD: R/W IRD: DIP non usato: non usato|ARD: null ptr APD: null ptr IRD: dpi non usato: non usato [2]|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQLSMALLINT|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|SQLINTEGER|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_DISPLAY_SIZE|SQLLEN|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_FIXED_PREC_SCALE|SQLSMALLINT|ARD: APD non usato: IRD non usato: R: r|ARD: APD non usato: IRD non usato: D dpi: D [1]|  
|SQL_DESC_INDICATOR_PTR|SQLLEN|ARD: R/W APD: R/W IRD: DIP non usato: non usato|ARD: null ptr APD: null ptr IRD: dpi non usato: non usato|  
|SQL_DESC_LABEL|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_LENGTH|SQLULEN|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_LITERAL_PREFIX|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_LITERAL_SUFFIX|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_LOCAL_TYPE_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R: r|ARD: APD non usato: IRD non usato: D dpi: D [1]|  
|SQL_DESC_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R: r/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_NULLABLE|SQLSMALLINT|ARD: APD non usato: IRD non usato: R: r|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_NUM_PREC_RADIX|SQLINTEGER|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_OCTET_LENGTH|SQLLEN|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_OCTET_LENGTH_PTR|SQLLEN|ARD: R/W APD: R/W IRD: DIP non usato: non usato|ARD: null ptr APD: null ptr IRD: dpi non usato: non usato|  
|SQL_DESC_PARAMETER_TYPE|SQLSMALLINT|ARD: APD non usato: IRD non usato: dpi non usato: R/W|ARD: APD non usato: IRD non usato: dpi non usato: D = SQL_PARAM_INPUT|  
|SQL_DESC_PRECISION|SQLSMALLINT|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_ROWVER|SQLSMALLINT|ARD: non usato<br /><br /> APD: non usato<br /><br /> IRD: R<br /><br /> DPI: R|ARD: non usato<br /><br /> APD: non usato<br /><br /> IRD: ND<br /><br /> DPI: ND|  
|SQL_DESC_SCALE|SQLSMALLINT|ARD: R/W APD: R/W IRD: R R: R/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_SCHEMA_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_SEARCHABLE|SQLSMALLINT|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_TABLE_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
|SQL_DESC_TYPE|SQLSMALLINT|ARD: R/W APD: R/W IRD: R R: R/W|ARD: SQL_C_DEFAULT APD: SQL_C_DEFAULT IRD: D DPI: ND|  
|SQL_DESC_TYPE_NAME|SQLCHAR|ARD: APD non usato: IRD non usato: R: r|ARD: APD non usato: IRD non usato: D dpi: D [1]|  
|SQL_DESC_UNNAMED|SQLSMALLINT|ARD: APD non usato: IRD non usato: R: r/W|ARD: ND APD: ND IRD: D DPI: ND|  
|SQL_DESC_UNSIGNED|SQLSMALLINT|ARD: APD non usato: IRD non usato: R: r|ARD: APD non usato: IRD non usato: D dpi: D [1]|  
|SQL_DESC_UPDATABLE|SQLSMALLINT|ARD: APD non usato: IRD non usato: R dpi: non usato|ARD: APD non usato: IRD non usato: D dpi: non usato|  
  
 [1] questi campi vengono definiti solo quando il dpi viene popolato automaticamente dal driver. In caso contrario, non sono definite. Se un'applicazione tenta di impostare questi campi, viene restituito l'errore SQLSTATE HY091 (identificatore del campo del descrittore non valido).  
  
 [2] il campo SQL_DESC_DATA_PTR in dpi può essere impostato in modo da forzare una verifica di coerenza. In una chiamata successiva a **SQLGetDescField** o **SQLGetDescRec**, non è necessario che il driver restituisca il valore su cui è stato impostato SQL_DESC_DATA_PTR.  
  
## <a name="fieldidentifier-argument"></a>Argomento FieldIdentifier  
 L'argomento *FieldIdentifier* indica il campo del descrittore da impostare. Un descrittore contiene l' *intestazione del descrittore,* costituita dai campi di intestazione descritti nella sezione successiva, "campi di intestazione" e zero o più *record di descrittori,* costituito dai campi di record descritti nella sezione che segue la sezione "campi di intestazione".  
  
## <a name="header-fields"></a>Campi di intestazione  
 Ogni descrittore ha un'intestazione costituita dai campi seguenti:  
  
 **SQL_DESC_ALLOC_TYPE [tutti]**  
 Questo campo di intestazione SQLSMALLINT di sola lettura specifica se il descrittore è stato allocato automaticamente dal driver o in modo esplicito dall'applicazione. L'applicazione può ottenere, ma non modificare, questo campo. Il campo viene impostato in modo da SQL_DESC_ALLOC_AUTO dal driver se il descrittore è stato allocato automaticamente dal driver. Viene impostato su SQL_DESC_ALLOC_USER dal driver se il descrittore è stato allocato in modo esplicito dall'applicazione.  
  
 **SQL_DESC_ARRAY_SIZE [descrittori applicazione]**  
 In ARDs questo campo di intestazione SQLULEN specifica il numero di righe nel set di righe. Si tratta del numero di righe che devono essere restituite da una chiamata a **SQLFetch** o **SQLFetchScroll** oppure da una chiamata a **SQLBulkOperations** o **SQLSetPos**.  
  
 In APD, questo campo di intestazione SQLULEN specifica il numero di valori per ogni parametro.  
  
 Il valore predefinito di questo campo è 1. Se SQL_DESC_ARRAY_SIZE è maggiore di 1, SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR del punto di APD o ARD alle matrici. La cardinalità di ogni matrice è uguale al valore di questo campo.  
  
 Questo campo in ARD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_ROW_ARRAY_SIZE. Questo campo nell'APD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_PARAMSET_SIZE.  
  
 **SQL_DESC_ARRAY_STATUS_PTR [tutti]**  
 Per ogni tipo di descrittore, questo campo di intestazione SQLUSMALLINT * punta a una matrice di valori SQLUSMALLINT. Queste matrici sono denominate come segue: Row Status Array (IRD), Parameter status Array (dpi), Row Operation Array (ARD) e Parameter Operation Array (APD).  
  
 In IRD, questo campo di intestazione punta a una matrice di stato della riga contenente i valori di stato dopo una chiamata a **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** o **SQLSetPos**. Il numero di elementi della matrice è il numero di righe nel set di righe. L'applicazione deve allocare una matrice di SQLUSMALLINTs e impostare questo campo in modo che punti alla matrice. Per impostazione predefinita, il campo è impostato su un puntatore null. Il driver compilerà la matrice, a meno che il campo SQL_DESC_ARRAY_STATUS_PTR non sia impostato su un puntatore null, nel qual caso non verrà generato alcun valore di stato e la matrice non verrà popolata.  
  
> [!CAUTION]  
>  Il comportamento del driver non è definito se l'applicazione imposta gli elementi della matrice di stato della riga a cui punta il campo SQL_DESC_ARRAY_STATUS_PTR di IRD.  
  
 Inizialmente la matrice viene popolata da una chiamata a **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** o **SQLSetPos**. Se la chiamata non ha restituito SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il contenuto della matrice a cui fa riferimento questo campo non è definito. Gli elementi nella matrice possono contenere i valori seguenti:  
  
-   SQL_ROW_SUCCESS: la riga è stata recuperata e non è stata modificata dopo l'ultima operazione di recupero.  
  
-   SQL_ROW_SUCCESS_WITH_INFO: la riga è stata recuperata e non è stata modificata dopo l'ultima operazione di recupero. Tuttavia, è stato restituito un avviso sulla riga.  
  
-   SQL_ROW_ERROR: si è verificato un errore durante il recupero della riga.  
  
-   SQL_ROW_UPDATED: la riga è stata recuperata ed è stata aggiornata da quando è stata eseguita l'ultima operazione di recupero. Se la riga viene recuperata di nuovo, lo stato viene SQL_ROW_SUCCESS.  
  
-   SQL_ROW_DELETED: la riga è stata eliminata dall'ultimo recupero.  
  
-   SQL_ROW_ADDED: la riga è stata inserita da **SQLBulkOperations**. Se la riga viene recuperata di nuovo, lo stato viene SQL_ROW_SUCCESS.  
  
-   SQL_ROW_NOROW: il set di righe ha sovrapposto la fine del set di risultati e non è stata restituita alcuna riga corrispondente a questo elemento della matrice di stato della riga.  
  
 Questo campo in IRD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_ROW_STATUS_PTR.  
  
 Il campo SQL_DESC_ARRAY_STATUS_PTR di IRD è valido solo dopo la restituzione di SQL_SUCCESS o SQL_SUCCESS_WITH_INFO. Se il codice restituito non è uno di questi, la posizione a cui punta SQL_DESC_ROWS_PROCESSED_PTR non è definita.  
  
 In dpi questo campo di intestazione punta a una matrice di stato dei parametri che contiene informazioni sullo stato per ogni set di valori di parametro dopo una chiamata a **SQLExecute** o **SQLExecDirect**. Se la chiamata a **SQLExecute** o **SQLExecDirect** non restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il contenuto della matrice a cui fa riferimento questo campo non è definito. L'applicazione deve allocare una matrice di SQLUSMALLINTs e impostare questo campo in modo che punti alla matrice. Il driver compilerà la matrice, a meno che il campo SQL_DESC_ARRAY_STATUS_PTR non sia impostato su un puntatore null, nel qual caso non verrà generato alcun valore di stato e la matrice non verrà popolata. Gli elementi nella matrice possono contenere i valori seguenti:  
  
-   SQL_PARAM_SUCCESS: l'istruzione SQL è stata eseguita correttamente per questo set di parametri.  
  
-   SQL_PARAM_SUCCESS_WITH_INFO: l'istruzione SQL è stata eseguita correttamente per questo set di parametri. Tuttavia, le informazioni sugli avvisi sono disponibili nella struttura dei dati di diagnostica.  
  
-   SQL_PARAM_ERROR: si è verificato un errore durante l'elaborazione di questo set di parametri. Informazioni aggiuntive sull'errore sono disponibili nella struttura dei dati di diagnostica.  
  
-   SQL_PARAM_UNUSED: questo set di parametri è stato inutilizzato, probabilmente a causa del fatto che alcuni set di parametri precedenti hanno causato un errore che ha interrotto l'ulteriore elaborazione o perché SQL_PARAM_IGNORE è stato impostato per il set di parametri nella matrice specificata dal campo SQL_DESC_ARRAY_STATUS_PTR di APD.  
  
-   SQL_PARAM_DIAG_UNAVAILABLE: le informazioni di diagnostica non sono disponibili. Un esempio è quando il driver considera le matrici di parametri come unità monolitica e pertanto non genera questo livello di informazioni sugli errori.  
  
 Questo campo in dpi può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_PARAM_STATUS_PTR.  
  
 In ARD, questo campo di intestazione punta a una matrice di operazioni di riga di valori che possono essere impostati dall'applicazione per indicare se questa riga deve essere ignorata per le operazioni **SQLSetPos** . Gli elementi nella matrice possono contenere i valori seguenti:  
  
-   SQL_ROW_PROCEED: la riga è inclusa nell'operazione bulk utilizzando **SQLSetPos**. Questa impostazione non garantisce che l'operazione venga eseguita sulla riga. Se la riga presenta lo stato SQL_ROW_ERROR nella matrice di stato della riga IRD, il driver potrebbe non essere in grado di eseguire l'operazione nella riga.  
  
-   SQL_ROW_IGNORE: la riga viene esclusa dall'operazione bulk utilizzando **SQLSetPos**.  
  
 Se non sono impostati elementi della matrice, tutte le righe vengono incluse nell'operazione bulk. Se il valore nel campo SQL_DESC_ARRAY_STATUS_PTR di ARD è un puntatore null, tutte le righe vengono incluse nell'operazione bulk; l'interpretazione è identica a quella di un puntatore che punta a una matrice valida e che tutti gli elementi della matrice sono stati SQL_ROW_PROCEED. Se un elemento nella matrice è impostato su SQL_ROW_IGNORE, il valore nella matrice di stato della riga per la riga ignorata non viene modificato.  
  
 Questo campo in ARD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_ROW_OPERATION_PTR.  
  
 In APD, questo campo di intestazione punta a una matrice di operazioni di parametro di valori che possono essere impostati dall'applicazione per indicare se questo set di parametri deve essere ignorato quando viene chiamato **SQLExecute** o **SQLExecDirect** . Gli elementi nella matrice possono contenere i valori seguenti:  
  
-   SQL_PARAM_PROCEED: il set di parametri è incluso nella chiamata a **SQLExecute** o **SQLExecDirect** .  
  
-   SQL_PARAM_IGNORE: il set di parametri è escluso dalla chiamata a **SQLExecute** o **SQLExecDirect** .  
  
 Se non sono impostati elementi della matrice, vengono utilizzati tutti i set di parametri nella matrice nelle chiamate a **SQLExecute** o **SQLExecDirect** . Se il valore nel campo SQL_DESC_ARRAY_STATUS_PTR di APD è un puntatore null, vengono usati tutti i set di parametri. l'interpretazione è identica a quella di un puntatore che punta a una matrice valida e che tutti gli elementi della matrice sono stati SQL_PARAM_PROCEED.  
  
 Questo campo nell'APD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_PARAM_OPERATION_PTR.  
  
 **SQL_DESC_BIND_OFFSET_PTR [descrittori applicazione]**  
 Questo campo di intestazione SQLLEN * punta all'offset del binding. Per impostazione predefinita, viene impostato su un puntatore null. Se questo campo non è un puntatore null, il driver dereferenzia il puntatore e aggiunge il valore dereferenziato a ognuno dei campi posticipati con un valore non null nel record del descrittore (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR) in fase di recupero e utilizza i nuovi valori del puntatore durante l'associazione.  
  
 L'offset dell'associazione viene sempre aggiunto direttamente ai valori nei campi SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR. Se l'offset viene impostato su un valore diverso, il nuovo valore viene ancora aggiunto direttamente al valore in ogni campo del descrittore. Il nuovo offset non viene aggiunto al valore del campo più qualsiasi offset precedente.  
  
 Questo campo è un *campo posticipato*: non viene utilizzato nel momento in cui è impostato ma viene utilizzato in un secondo momento dal driver quando è necessario determinare gli indirizzi per i buffer di dati.  
  
 Questo campo in ARD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_ROW_BIND_OFFSET_PTR. Questo campo in ARD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_PARAM_BIND_OFFSET_PTR.  
  
 Per ulteriori informazioni, vedere la descrizione dell'associazione per riga in [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md) e [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 **SQL_DESC_BIND_TYPE [descrittori applicazione]**  
 Questo campo di intestazione SQLUINTEGER imposta l'orientamento dell'associazione da utilizzare per l'associazione di colonne o parametri.  
  
 In ARDs questo campo specifica l'orientamento dell'associazione quando **SQLFetchScroll** o **SQLFetch** viene chiamato sull'handle di istruzione associato.  
  
 Per selezionare l'associazione per colonna per le colonne, questo campo è impostato su SQL_BIND_BY_COLUMN (impostazione predefinita).  
  
 Questo campo in ARD può essere impostato anche chiamando **SQLSetStmtAttr** con l' *attributo* SQL_ATTR_ROW_BIND_TYPE.  
  
 In APD questo campo specifica l'orientamento dell'associazione da utilizzare per i parametri dinamici.  
  
 Per selezionare l'associazione per colonna per i parametri, questo campo è impostato su SQL_BIND_BY_COLUMN (impostazione predefinita).  
  
 Questo campo nell'APD può essere impostato anche chiamando **SQLSetStmtAttr** con l' *attributo* SQL_ATTR_PARAM_BIND_TYPE.  
  
 **SQL_DESC_COUNT [tutti]**  
 Questo campo di intestazione SQLSMALLINT specifica l'indice in base 1 del record con il numero più alto che contiene i dati. Quando il driver imposta la struttura dei dati per il descrittore, deve impostare anche il campo SQL_DESC_COUNT per visualizzare il numero di record significativi. Quando un'applicazione alloca un'istanza di questa struttura di dati, non è necessario specificare il numero di record per i quali riservare spazio. Quando l'applicazione specifica il contenuto dei record, il driver esegue le azioni necessarie per garantire che l'handle del descrittore faccia riferimento a una struttura di dati di dimensioni appropriate.  
  
 SQL_DESC_COUNT non è un conteggio di tutte le colonne di dati associate (se il campo si trova in un ARD) o di tutti i parametri associati (se il campo si trova in un oggetto APD), ma il numero del record con il numero più alto. Se la colonna o il parametro con il numero più alto non è associato, SQL_DESC_COUNT viene modificato nel numero della colonna o del parametro con il numero più alto successivo. Se una colonna o un parametro con un numero minore del numero della colonna con il numero più alto è non associato (chiamando **SQLBindCol** con l'argomento *TargetValuePtr* impostato su un puntatore null o **SQLBindParameter** con l'argomento *ParameterValuePtr* impostato su un puntatore null), SQL_DESC_COUNT non viene modificato. Se altre colonne o parametri sono associati con numeri maggiori rispetto al record con il numero più alto che contiene dati, il driver aumenta automaticamente il valore nel campo SQL_DESC_COUNT. Se tutte le colonne sono non associate chiamando **SQLFreeStmt** con l'opzione SQL_UNBIND, i campi SQL_DESC_COUNT in ARD e IRD vengono impostati su 0. Se **SQLFreeStmt** viene chiamato con l'opzione SQL_RESET_PARAMS, i campi SQL_DESC_COUNT in APD e dpi vengono impostati su 0.  
  
 Il valore in SQL_DESC_COUNT può essere impostato in modo esplicito da un'applicazione chiamando **SQLSetDescField**. Se il valore in SQL_DESC_COUNT viene ridotto in modo esplicito, tutti i record con numeri maggiori del nuovo valore in SQL_DESC_COUNT vengono rimossi in modo efficace. Se il valore in SQL_DESC_COUNT è impostato in modo esplicito su 0 e il campo si trova in un ARD, vengono rilasciati tutti i buffer di dati tranne una colonna di segnalibri associati.  
  
 Il numero di record in questo campo di un ARD non include una colonna di segnalibro associato. L'unico modo per annullare l'associazione di una colonna bookmark consiste nell'impostare il campo SQL_DESC_DATA_PTR su un puntatore null.  
  
 **SQL_DESC_ROWS_PROCESSED_PTR [descrittori di implementazione]**  
 In un IRD, questo \* campo di intestazione SQLULEN punta a un buffer contenente il numero di righe recuperate dopo una chiamata a **SQLFetch** o **SQLFetchScroll** o il numero di righe interessate in un'operazione bulk eseguita da una chiamata a **SQLBulkOperations** o **SQLSetPos**, incluse le righe di errore.  
  
 In un oggetto DIP, questo campo di intestazione SQLUINTEGER * punta a un buffer contenente il numero di set di parametri elaborati, inclusi i set di errori. Se questo è un puntatore null, non verrà restituito alcun numero.  
  
 SQL_DESC_ROWS_PROCESSED_PTR è valido solo dopo che è stato restituito SQL_SUCCESS o SQL_SUCCESS_WITH_INFO dopo una chiamata a **SQLFetch** o **SQLFetchScroll** (per un campo IRD) o a **SQLExecute**, **SQLEXECDIRECT** o **SQLParamData** (per un campo dpi). Se la chiamata che compila il buffer a cui punta questo campo non restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il contenuto del buffer non è definito, a meno che non restituisca SQL_NO_DATA, nel qual caso il valore nel buffer viene impostato su 0.  
  
 Questo campo in ARD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_ROWS_FETCHED_PTR. Questo campo nell'APD può essere impostato anche chiamando **SQLSetStmtAttr** con l'attributo SQL_ATTR_PARAMS_PROCESSED_PTR.  
  
 Il buffer a cui punta questo campo è allocato dall'applicazione. Si tratta di un buffer di output posticipato impostato dal driver. Per impostazione predefinita, viene impostato su un puntatore null.  
  
## <a name="record-fields"></a>Campi di record  
 Ogni descrittore contiene uno o più record costituiti da campi che definiscono i dati delle colonne o i parametri dinamici, a seconda del tipo di descrittore. Ogni record è una definizione completa di una singola colonna o parametro.  
  
 **SQL_DESC_AUTO_UNIQUE_VALUE [IRDs]**  
 Questo campo di record SQLINTEGER di sola lettura contiene SQL_TRUE se la colonna è una colonna a incremento automatico oppure SQL_FALSE se la colonna non è una colonna a incremento automatico. Questo campo è di sola lettura, ma la colonna di incremento automatico sottostante non è necessariamente di sola lettura.  
  
 **SQL_DESC_BASE_COLUMN_NAME [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il nome della colonna di base per la colonna del set di risultati. Se non esiste un nome di colonna di base (come nel caso di colonne che sono espressioni), questa variabile contiene una stringa vuota.  
  
 **SQL_DESC_BASE_TABLE_NAME [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il nome della tabella di base per la colonna del set di risultati. Se non è possibile definire un nome di tabella di base o se non è applicabile, questa variabile contiene una stringa vuota.  
  
 **SQL_DESC_CASE_SENSITIVE [descrittori di implementazione]**  
 Questo campo di record SQLINTEGER di sola lettura contiene SQL_TRUE se la colonna o il parametro viene considerato come distinzione tra maiuscole e minuscole per le regole di confronto e i confronti oppure SQL_FALSE se la colonna non viene considerata come distinzione tra maiuscole e minuscole per le regole di confronto e i confronti o se è una colonna non di tipo carattere.  
  
 **SQL_DESC_CATALOG_NAME [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il catalogo della tabella di base che contiene la colonna. Il valore restituito è dipendente dal driver se la colonna è un'espressione o se la colonna fa parte di una vista. Se l'origine dati non supporta i cataloghi o non è possibile determinare il catalogo, questa variabile contiene una stringa vuota.  
  
 **SQL_DESC_CONCISE_TYPE [tutti]**  
 Questo campo di intestazione SQLSMALLINT specifica il tipo di dati conciso per tutti i tipi di dati, inclusi i tipi di dati DateTime e Interval.  
  
 I valori nei campi SQL_DESC_CONCISE_TYPE, SQL_DESC_TYPE e SQL_DESC_DATETIME_INTERVAL_CODE sono interdipendenti. Ogni volta che viene impostato uno dei campi, è necessario impostare anche l'altro. SQL_DESC_CONCISE_TYPE possono essere impostate tramite una chiamata a **SQLBindCol** o **SQLBindParameter** o **SQLSetDescField**. SQL_DESC_TYPE possono essere impostate tramite una chiamata a **SQLSetDescField** o **SQLSetDescRec**.  
  
 Se SQL_DESC_CONCISE_TYPE è impostato su un tipo di dati conciso diverso da un intervallo o da un tipo di dati DateTime, il campo SQL_DESC_TYPE viene impostato sullo stesso valore e il campo SQL_DESC_DATETIME_INTERVAL_CODE è impostato su 0.  
  
 Se SQL_DESC_CONCISE_TYPE è impostato sul tipo di dati DateTime o Interval conciso, il campo SQL_DESC_TYPE viene impostato sul tipo dettagliato corrispondente (SQL_DATETIME o SQL_INTERVAL) e il campo SQL_DESC_DATETIME_INTERVAL_CODE è impostato sul sottocodice appropriato.  
  
 **SQL_DESC_DATA_PTR [descrittori dell'applicazione e IPD]**  
 Questo campo di record SQLPOINTER punta a una variabile che conterrà il valore del parametro (per APD) o il valore della colonna (per ARDs). Questo campo è un *campo posticipato*. Non viene utilizzato nel momento in cui viene impostato ma viene utilizzato in un secondo momento dal driver per recuperare i dati.  
  
 La colonna specificata dal campo SQL_DESC_DATA_PTR di ARD è Unbound se l'argomento *TargetValuePtr* in una chiamata a **SQLBindCol** è un puntatore null o se il campo SQL_DESC_DATA_PTR in ARD è impostato da una chiamata a **SQLSetDescField** o **SQLSetDescRec** a un puntatore null. Gli altri campi non sono interessati se il campo SQL_DESC_DATA_PTR è impostato su un puntatore null.  
  
 Se la chiamata a **SQLFetch** o **SQLFetchScroll** che compila il buffer a cui punta questo campo non ha restituito SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il contenuto del buffer non è definito.  
  
 Ogni volta che viene impostato il campo SQL_DESC_DATA_PTR di un oggetto APD, ARD o dpi, il driver verifica che il valore nel campo SQL_DESC_TYPE contenga uno dei tipi di dati ODBC C validi o un tipo di dati specifico del driver e che tutti gli altri campi che influiscono sui tipi di dati siano coerenti. La richiesta di verifica di coerenza è l'unico utilizzo del campo SQL_DESC_DATA_PTR di un oggetto dpi. In particolare, se un'applicazione imposta il campo SQL_DESC_DATA_PTR di un oggetto dpi e in un secondo momento chiama **SQLGetDescField** su questo campo, non viene necessariamente restituito il valore impostato. Per ulteriori informazioni, vedere l'argomento relativo alle verifiche di coerenza in [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 **SQL_DESC_DATETIME_INTERVAL_CODE [tutti]**  
 Questo campo di record SQLSMALLINT contiene il sottocodice per il tipo di dati DateTime o Interval specifico quando il campo SQL_DESC_TYPE è SQL_DATETIME o SQL_INTERVAL. Questo vale sia per i tipi di dati SQL che per i tipi di dati C. Il codice è costituito dal nome del tipo di dati con "codice" sostituito per "tipo" o "C_TYPE" (per i tipi DateTime) oppure "codice" sostituito da "INTERVAL" o "C_INTERVAL" (per i tipi di intervallo).  
  
 Se SQL_DESC_TYPE e SQL_DESC_CONCISE_TYPE in un descrittore dell'applicazione sono impostate su SQL_C_DEFAULT e il descrittore non è associato a un handle di istruzione, il contenuto di SQL_DESC_DATETIME_INTERVAL_CODE non è definito.  
  
 Questo campo può essere impostato per i tipi di dati DateTime elencati nella tabella seguente.  
  
|Tipi DateTime|DATETIME_INTERVAL_CODE|  
|--------------------|------------------------------|  
|SQL_TYPE_DATE/SQL_C_TYPE_DATE|SQL_CODE_DATE|  
|SQL_TYPE_TIME/SQL_C_TYPE_TIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP/SQL_C_TYPE_TIMESTAMP|SQL_CODE_TIMESTAMP|  
  
 Questo campo può essere impostato per i tipi di dati interval elencati nella tabella seguente.  
  
|Tipo di intervallo|DATETIME_INTERVAL_CODE|  
|-------------------|------------------------------|  
|SQL_INTERVAL_DAY/SQL_C_INTERVAL_DAY|SQL_CODE_DAY|  
|SQL_INTERVAL_DAY_TO_HOUR/SQL_C_INTERVAL_DAY_TO_HOUR|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE/SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_CODE_DAY_TO_MINUTE|  
|SQL_INTERVAL_DAY_TO_SECOND/SQL_C_INTERVAL_DAY_TO_SECOND|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR/SQL_C_INTERVAL_HOUR|SQL_CODE_HOUR|  
|SQL_INTERVAL_HOUR_TO_MINUTE/SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND/SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE/SQL_C_INTERVAL_MINUTE|SQL_CODE_MINUTE|  
|SQL_INTERVAL_MINUTE_TO_SECOND/SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_CODE_MINUTE_TO_SECOND|  
|SQL_INTERVAL_MONTH/SQL_C_INTERVAL_MONTH|SQL_CODE_MONTH|  
|SQL_INTERVAL_SECOND/SQL_C_INTERVAL_SECOND|SQL_CODE_SECOND|  
|SQL_INTERVAL_YEAR/SQL_C_INTERVAL_YEAR|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH/SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_CODE_YEAR_TO_MONTH|  
  
 Per ulteriori informazioni sugli intervalli di dati e su questo campo, vedere [identificatori e descrittori del tipo di dati](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md).  
  
 **SQL_DESC_DATETIME_INTERVAL_PRECISION [tutti]**  
 Questo campo record SQLINTEGER contiene la precisione principale dell'intervallo se il campo SQL_DESC_TYPE è SQL_INTERVAL. Quando il campo SQL_DESC_DATETIME_INTERVAL_CODE è impostato su un tipo di dati interval, questo campo viene impostato sulla precisione massima dell'intervallo predefinito.  
  
 **SQL_DESC_DISPLAY_SIZE [IRDs]**  
 Questo campo di record SQLINTEGER di sola lettura contiene il numero massimo di caratteri necessari per visualizzare i dati dalla colonna.  
  
 **SQL_DESC_FIXED_PREC_SCALE [descrittori di implementazione]**  
 Questo campo di record SQLSMALLINT di sola lettura è impostato su SQL_TRUE se la colonna è una colonna numerica esatta e presenta una precisione fissa e una scala diversa da zero oppure per SQL_FALSE se la colonna non è una colonna numerica esatta con una precisione e una scala fisse.  
  
 **SQL_DESC_INDICATOR_PTR [descrittori applicazione]**  
 In ARDs, questo campo di record SQLLEN * punta alla variabile indicatore. Questa variabile contiene SQL_NULL_DATA se il valore della colonna è NULL. Per APD, la variabile indicatore è impostata su SQL_NULL_DATA per specificare argomenti dinamici NULL. In caso contrario, la variabile è zero (a meno che i valori in SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR non siano lo stesso puntatore).  
  
 Se il SQL_DESC_INDICATOR_PTR campo in un ARD è un puntatore null, al driver viene impedito di restituire informazioni sulla presenza o meno di una colonna NULL. Se la colonna è NULL e SQL_DESC_INDICATOR_PTR è un puntatore null, viene restituito SQLSTATE 22002 (variabile indicatore obbligatoria ma non fornita) quando il driver tenta di popolare il buffer dopo una chiamata a **SQLFetch** o **SQLFetchScroll**. Se la chiamata a **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il contenuto del buffer non è definito.  
  
 Il campo SQL_DESC_INDICATOR_PTR determina se il campo a cui punta SQL_DESC_OCTET_LENGTH_PTR è impostato. Se il valore dei dati di una colonna è NULL, il driver imposta la variabile dell'indicatore su SQL_NULL_DATA. Il campo a cui punta SQL_DESC_OCTET_LENGTH_PTR non è impostato. Se non viene rilevato un valore NULL durante il recupero, il buffer a cui punta SQL_DESC_INDICATOR_PTR viene impostato su zero e il buffer a cui punta SQL_DESC_OCTET_LENGTH_PTR viene impostato sulla lunghezza dei dati.  
  
 Se il campo SQL_DESC_INDICATOR_PTR in un oggetto APD è un puntatore null, l'applicazione non può utilizzare questo record del descrittore per specificare argomenti NULL.  
  
 Questo campo è un *campo posticipato*: non viene usato nel momento in cui è impostato, ma viene usato in un secondo momento dal driver per indicare i valori null (per ARDs) o per determinare il supporto di valori null (per APD).  
  
 **SQL_DESC_LABEL [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene l'etichetta o il titolo della colonna. Se la colonna non dispone di un'etichetta, questa variabile contiene il nome della colonna. Se la colonna è senza nome e non è etichettata, questa variabile contiene una stringa vuota.  
  
 **SQL_DESC_LENGTH [tutti]**  
 Questo campo di record SQLULEN è la lunghezza massima o effettiva di una stringa di caratteri in caratteri o un tipo di dati binario in byte. È la lunghezza massima per un tipo di dati a lunghezza fissa o la lunghezza effettiva per un tipo di dati a lunghezza variabile. Il valore esclude sempre il carattere di terminazione null che termina la stringa di caratteri. Per i valori il cui tipo è SQL_TYPE_DATE, SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP o uno dei tipi di dati intervallo SQL, questo campo ha la lunghezza in caratteri della rappresentazione di stringa di caratteri del valore DateTime o Interval.  
  
 Il valore in questo campo può essere diverso dal valore di "length", come definito in ODBC 2 *. x*. Per ulteriori informazioni, vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_LITERAL_PREFIX [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il carattere o i caratteri riconosciuti dal driver come prefisso per un valore letterale di questo tipo di dati. Questa variabile contiene una stringa vuota per un tipo di dati per cui non è applicabile un prefisso letterale.  
  
 **SQL_DESC_LITERAL_SUFFIX [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il carattere o i caratteri riconosciuti dal driver come suffisso per un valore letterale di questo tipo di dati. Questa variabile contiene una stringa vuota per un tipo di dati per cui non è applicabile un suffisso letterale.  
  
 **SQL_DESC_LOCAL_TYPE_NAME [descrittori di implementazione]**  
 Questo campo di record SQLCHAR * contiene qualsiasi nome localizzato (lingua nativa) per il tipo di dati che può essere diverso dal nome regolare del tipo di dati. Se non è presente alcun nome localizzato, viene restituita una stringa vuota. Questo campo è solo a scopo di visualizzazione.  
  
 **SQL_DESC_NAME [descrittori di implementazione]**  
 Questo campo di record SQLCHAR * in un descrittore di riga contiene l'alias di colonna, se applicabile. Se l'alias di colonna non è applicabile, viene restituito il nome della colonna. In entrambi i casi, il driver imposta il campo SQL_DESC_UNNAMED su SQL_NAMED quando imposta il campo SQL_DESC_NAME. Se non è presente alcun nome di colonna o alias di colonna, il driver restituisce una stringa vuota nel campo SQL_DESC_NAME e imposta il campo SQL_DESC_UNNAMED su SQL_UNNAMED.  
  
 Un'applicazione può impostare il campo SQL_DESC_NAME di un oggetto dpi su un nome di parametro o un alias per specificare stored procedure parametri in base al nome. Per ulteriori informazioni, vedere [associazione di parametri in base al nome (parametri denominati)](../../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md). Il campo SQL_DESC_NAME di un IRD è un campo di sola lettura. Se un'applicazione tenta di impostarla, viene restituito SQLSTATE HY091 (identificatore del campo del descrittore non valido).  
  
 In IPD questo campo non è definito se il driver non supporta i parametri denominati. Se il driver supporta i parametri denominati ed è in grado di descrivere i parametri, il nome del parametro viene restituito in questo campo.  
  
 **SQL_DESC_NULLABLE [descrittori di implementazione]**  
 In IRDs, questo campo di record SQLSMALLINT di sola lettura è SQL_NULLABLE se la colonna può includere valori NULL, SQL_NO_NULLS se la colonna non contiene valori NULL oppure SQL_NULLABLE_UNKNOWN se non è noto se la colonna accetta valori NULL. Questo campo è relativo alla colonna del set di risultati, non alla colonna di base.  
  
 In IPD questo campo è sempre impostato su SQL_NULLABLE perché i parametri dinamici sono sempre nullable e non possono essere impostati da un'applicazione.  
  
 **SQL_DESC_NUM_PREC_RADIX [tutti]**  
 Questo campo SQLINTEGER contiene un valore pari a 2 Se il tipo di dati nel campo SQL_DESC_TYPE è un tipo di dati numerico approssimato, perché il SQL_DESC_PRECISION campo contiene il numero di bit. Questo campo contiene un valore pari a 10 se il tipo di dati nel campo SQL_DESC_TYPE è un tipo di dati numerico esatto, perché il SQL_DESC_PRECISION campo contiene il numero di cifre decimali. Questo campo è impostato su 0 per tutti i tipi di dati non numerici.  
  
 **SQL_DESC_OCTET_LENGTH [tutti]**  
 Questo campo di record SQLLEN contiene la lunghezza, in byte, di una stringa di caratteri o di un tipo di dati binario. Per i tipi di carattere a lunghezza fissa o binaria, questa è la lunghezza effettiva in byte. Per i tipi di carattere o binari a lunghezza variabile, si tratta della lunghezza massima in byte. Questo valore esclude sempre lo spazio per il carattere di terminazione null per i descrittori di implementazione e include sempre lo spazio per il carattere di terminazione null per i descrittori di applicazione. Per i dati dell'applicazione, questo campo contiene la dimensione del buffer. Per APD, questo campo viene definito solo per i parametri di output o di input/output.  
  
 **SQL_DESC_OCTET_LENGTH_PTR [descrittori applicazione]**  
 Questo campo di record SQLLEN * punta a una variabile che conterrà la lunghezza totale in byte di un argomento dinamico (per i descrittori di parametro) o di un valore di colonna associata (per i descrittori di riga).  
  
 Per un oggetto APD, questo valore viene ignorato per tutti gli argomenti eccetto la stringa di caratteri e il binario; Se questo campo punta a SQL_NTS, l'argomento dinamico deve essere con terminazione null. Per indicare che un parametro associato sarà un parametro data-at-execution, un'applicazione imposta questo campo nel record appropriato di APD su una variabile che, in fase di esecuzione, conterrà il valore SQL_DATA_AT_EXEC o il risultato della macro SQL_LEN_DATA_AT_EXEC. Se è presente più di un campo di questo tipo, SQL_DESC_DATA_PTR può essere impostato su un valore che identifica in modo univoco il parametro per consentire all'applicazione di determinare quale parametro è richiesto.  
  
 Se il OCTET_LENGTH_PTR campo di un ARD è un puntatore null, il driver non restituisce informazioni sulla lunghezza per la colonna. Se il campo SQL_DESC_OCTET_LENGTH_PTR di un oggetto APD è un puntatore null, il driver presuppone che le stringhe di caratteri e i valori binari siano con terminazione null. (I valori binari non devono essere con terminazione null, ma è necessario assegnare una lunghezza per evitare il troncamento).  
  
 Se la chiamata a **SQLFetch** o **SQLFetchScroll** che compila il buffer a cui punta questo campo non ha restituito SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il contenuto del buffer non è definito. Questo campo è un *campo posticipato*. Non viene usata nel momento in cui è impostata, ma viene usata in un secondo momento dal driver per determinare o indicare la lunghezza in ottetti dei dati.  
  
 **SQL_DESC_PARAMETER_TYPE [IPD]**  
 Questo campo di record SQLSMALLINT è impostato su SQL_PARAM_INPUT per un parametro di input, SQL_PARAM_INPUT_OUTPUT per un parametro di input/output, SQL_PARAM_OUTPUT per un parametro di output, SQL_PARAM_INPUT_OUTPUT_STREAM per un parametro di input/output trasmesso o SQL_PARAM_OUTPUT_STREAM per un parametro di output trasmesso. Per impostazione predefinita, è impostato su SQL_PARAM_INPUT.  
  
 Per un oggetto DIP, il campo è impostato su SQL_PARAM_INPUT per impostazione predefinita, se il valore DPI non viene popolato automaticamente dal driver (l'attributo SQL_ATTR_ENABLE_AUTO_IPD Statement è SQL_FALSE). Un'applicazione deve impostare questo campo in dpi per i parametri che non sono parametri di input.  
  
 **SQL_DESC_PRECISION [tutti]**  
 Questo campo di record SQLSMALLINT contiene il numero di cifre per un tipo numerico esatto, il numero di bit in mantissa (precisione binaria) per un tipo numerico approssimativo o il numero di cifre nel componente per i secondi frazionari per il tipo di dati SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP o SQL_INTERVAL_SECOND. Questo campo non è definito per tutti gli altri tipi di dati.  
  
 Il valore in questo campo può essere diverso dal valore di "Precision", come definito in ODBC 2 *. x*. Per ulteriori informazioni, vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_ROWVER [descrittori di implementazione]**  
 Questo campo SQLSMALLINTrecord indica se una colonna viene automaticamente modificata dal sistema DBMS quando viene aggiornata una riga (ad esempio, una colonna di tipo "timestamp" in SQL Server). Il valore di questo campo record è impostato su SQL_TRUE se la colonna è una colonna di controllo delle versioni delle righe e per SQL_FALSE in caso contrario. Questo attributo di colonna è simile alla chiamata di **SQLSpecialColumns** con IdentifierType di SQL_ROWVER per determinare se una colonna viene aggiornata automaticamente.  
  
 **SQL_DESC_SCALE [tutti]**  
 Questo campo di record SQLSMALLINT contiene la scala definita per i tipi di dati Decimal e numeric. Il campo non è definito per tutti gli altri tipi di dati.  
  
 Il valore in questo campo può essere diverso dal valore per "scalabilità", come definito in ODBC 2 *. x*. Per ulteriori informazioni, vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_SCHEMA_NAME [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il nome dello schema della tabella di base che contiene la colonna. Il valore restituito è dipendente dal driver se la colonna è un'espressione o se la colonna fa parte di una vista. Se l'origine dati non supporta schemi o non è possibile determinare il nome dello schema, questa variabile contiene una stringa vuota.  
  
 **SQL_DESC_SEARCHABLE [IRDs]**  
 Questo campo di record SQLSMALLINT di sola lettura è impostato su uno dei valori seguenti:  
  
-   SQL_PRED_NONE se la colonna non può essere utilizzata in una clausola **where** . (Corrisponde al valore SQL_UNSEARCHABLE in ODBC 2 *. x*).  
  
-   SQL_PRED_CHAR se la colonna può essere utilizzata in una clausola **where** , ma solo con il predicato **like** . (Corrisponde al valore SQL_LIKE_ONLY in ODBC 2 *. x*).  
  
-   SQL_PRED_BASIC se la colonna può essere utilizzata in una clausola **where** con tutti gli operatori di confronto, ad eccezione di **come**. (Corrisponde al valore SQL_EXCEPT_LIKE in ODBC 2 *. x*).  
  
-   SQL_PRED_SEARCHABLE se la colonna può essere utilizzata in una clausola **where** con qualsiasi operatore di confronto.  
  
 **SQL_DESC_TABLE_NAME [IRDs]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il nome della tabella di base che contiene la colonna. Il valore restituito è dipendente dal driver se la colonna è un'espressione o se la colonna fa parte di una vista.  
  
 **SQL_DESC_TYPE [tutti]**  
 Questo campo di record SQLSMALLINT specifica il tipo di dati SQL o C conciso per tutti i tipi di dati eccetto i tipi di dati DateTime e Interval. Per i tipi di dati DateTime e Interval, questo campo specifica il tipo di dati Verbose, che è SQL_DATETIME o SQL_INTERVAL.  
  
 Ogni volta che questo campo contiene SQL_DATETIME o SQL_INTERVAL, il campo SQL_DESC_DATETIME_INTERVAL_CODE deve contenere il sottocodice appropriato per il tipo conciso. Per i tipi di dati DateTime, SQL_DESC_TYPE contiene SQL_DATETIME e il campo SQL_DESC_DATETIME_INTERVAL_CODE contiene un sottocodice per il tipo di dati DateTime specifico. Per i tipi di dati intervallo, SQL_DESC_TYPE contiene SQL_INTERVAL e il campo SQL_DESC_DATETIME_INTERVAL_CODE contiene un sottocodice per il tipo di dati intervallo specifico.  
  
 I valori nei campi SQL_DESC_TYPE e SQL_DESC_CONCISE_TYPE sono interdipendenti. Ogni volta che viene impostato uno dei campi, è necessario impostare anche l'altro. SQL_DESC_TYPE possono essere impostate tramite una chiamata a **SQLSetDescField** o **SQLSetDescRec**. SQL_DESC_CONCISE_TYPE possono essere impostate tramite una chiamata a **SQLBindCol** o **SQLBindParameter** o **SQLSetDescField**.  
  
 Se SQL_DESC_TYPE è impostato su un tipo di dati conciso diverso da un intervallo o da un tipo di dati DateTime, il campo SQL_DESC_CONCISE_TYPE viene impostato sullo stesso valore e il campo SQL_DESC_DATETIME_INTERVAL_CODE è impostato su 0.  
  
 Se SQL_DESC_TYPE è impostato sul tipo di dati DateTime o Interval Verbose (SQL_DATETIME o SQL_INTERVAL) e il campo SQL_DESC_DATETIME_INTERVAL_CODE è impostato sul sottocodice appropriato, il campo tipo SQL_DESC_CONCISE viene impostato sul tipo conciso corrispondente. Il tentativo di impostare SQL_DESC_TYPE su uno dei tipi di DateTime o Interval concisi restituirà SQLSTATE HY021 (informazioni descrittore incoerenti).  
  
 Quando il campo SQL_DESC_TYPE viene impostato tramite una chiamata a **SQLBindCol**, **SQLBindParameter** o **SQLSetDescField**, i campi seguenti vengono impostati sui valori predefiniti seguenti, come illustrato nella tabella seguente. I valori dei campi rimanenti dello stesso record non sono definiti.  
  
|Valore di SQL_DESC_TYPE|Altri campi impostati in modo implicito|  
|------------------------------|---------------------------------|  
|SQL_CHAR, SQL_VARCHAR, SQL_C_CHAR, SQL_C_VARCHAR|SQL_DESC_LENGTH è impostato su 1. SQL_DESC_PRECISION è impostato su 0.|  
|SQL_DATETIME|Quando SQL_DESC_DATETIME_INTERVAL_CODE è impostato su SQL_CODE_DATE o SQL_CODE_TIME, SQL_DESC_PRECISION è impostato su 0. Quando è impostato su SQL_DESC_TIMESTAMP, SQL_DESC_PRECISION è impostato su 6.|  
|SQL_DECIMAL, SQL_NUMERIC, SQL_C_NUMERIC|SQL_DESC_SCALE è impostato su 0. SQL_DESC_PRECISION viene impostato sulla precisione definita dall'implementazione per il rispettivo tipo di dati.<br /><br /> Vedere da [SQL a C: Numeric](../../../odbc/reference/appendixes/sql-to-c-numeric.md) per informazioni su come associare manualmente un valore SQL_C_NUMERIC.|  
|SQL_FLOAT, SQL_C_FLOAT|SQL_DESC_PRECISION viene impostato sulla precisione predefinita definita dall'implementazione per SQL_FLOAT.|  
|SQL_INTERVAL|Quando SQL_DESC_DATETIME_INTERVAL_CODE è impostato su un tipo di dati interval, SQL_DESC_DATETIME_INTERVAL_PRECISION è impostato su 2 (la precisione principale dell'intervallo predefinita). Quando l'intervallo include un componente secondi, SQL_DESC_PRECISION viene impostato su 6 (la precisione dell'intervallo predefinito in secondi).|  
  
 Quando un'applicazione chiama **SQLSetDescField** per impostare campi di un descrittore anziché chiamare **SQLSetDescRec**, l'applicazione deve prima dichiarare il tipo di dati. In tal caso, gli altri campi indicati nella tabella precedente vengono impostati in modo implicito. Se uno dei valori impostati in modo implicito non è accettabile, l'applicazione può quindi chiamare **SQLSetDescField** o **SQLSetDescRec** per impostare in modo esplicito il valore non accettabile.  
  
 **SQL_DESC_TYPE_NAME [descrittori di implementazione]**  
 Questo campo di record SQLCHAR * di sola lettura contiene il nome del tipo dipendente dall'origine dati (ad esempio, "CHAR", "VARCHAR" e così via). Se il nome del tipo di dati è sconosciuto, questa variabile contiene una stringa vuota.  
  
 **SQL_DESC_UNNAMED [descrittori di implementazione]**  
 Questo campo di record SQLSMALLINT in un descrittore di riga viene impostato dal driver su SQL_NAMED o SQL_UNNAMED quando imposta il campo SQL_DESC_NAME. Se il campo SQL_DESC_NAME contiene un alias di colonna o se l'alias di colonna non è applicabile, il driver imposta il campo SQL_DESC_UNNAMED su SQL_NAMED. Se un'applicazione imposta il campo SQL_DESC_NAME di un oggetto DPI sul nome di un parametro o su un alias, il driver imposta il campo SQL_DESC_UNNAMED del valore dpi su SQL_NAMED. Se non è presente alcun nome di colonna o alias di colonna, il driver imposta il campo SQL_DESC_UNNAMED su SQL_UNNAMED.  
  
 Un'applicazione può impostare il campo SQL_DESC_UNNAMED di un DPI su SQL_UNNAMED. Un driver restituisce SQLSTATE HY091 (identificatore del campo del descrittore non valido) se un'applicazione tenta di impostare il campo SQL_DESC_UNNAMED di un oggetto dpi su SQL_NAMED. Il campo SQL_DESC_UNNAMED di un IRD è di sola lettura. Se un'applicazione tenta di impostarla, viene restituito SQLSTATE HY091 (identificatore del campo del descrittore non valido).  
  
 **SQL_DESC_UNSIGNED [descrittori di implementazione]**  
 Questo campo di record SQLSMALLINT di sola lettura è impostato su SQL_TRUE se il tipo di colonna è senza segno o non numerico oppure SQL_FALSE se il tipo di colonna è firmato.  
  
 **SQL_DESC_UPDATABLE [IRDs]**  
 Questo campo di record SQLSMALLINT di sola lettura è impostato su uno dei valori seguenti:  
  
-   SQL_ATTR_READ_ONLY se la colonna del set di risultati è di sola lettura.  
  
-   SQL_ATTR_WRITE se la colonna del set di risultati è di lettura/scrittura.  
  
-   SQL_ATTR_READWRITE_UNKNOWN se non è noto se la colonna del set di risultati è aggiornabile o meno.  
  
 SQL_DESC_UPDATABLE descrive aggiornabilità della colonna nel set di risultati, non della colonna nella tabella di base. Il valore di aggiornabilità della colonna nella tabella di base su cui si basa questa colonna del set di risultati può essere diverso da quello in questo campo. Il fatto che una colonna sia aggiornabile può essere basata sul tipo di dati, sui privilegi utente e sulla definizione del set di risultati stesso. Se non è chiaro se una colonna è aggiornabile, è necessario che venga restituito SQL_ATTR_READWRITE_UNKNOWN.  
  
## <a name="consistency-checks"></a>Verifiche coerenza  
 Una verifica della coerenza viene eseguita automaticamente dal driver ogni volta che un'applicazione passa un valore per il campo SQL_DESC_DATA_PTR di ARD, APD o dip. Se uno dei campi non è coerente con gli altri campi, **SQLSetDescField** restituirà SQLSTATE HY021 (informazioni descrittore incoerenti). Per ulteriori informazioni, vedere la sezione relativa alla verifica della coerenza in [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di una colonna|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Associazione di un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Recupero di un campo del descrittore|[Funzione SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Recupero di più campi di descrizione|[Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Impostazione di più campi di descrizione|[Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Informazioni di riferimento sull'API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)
