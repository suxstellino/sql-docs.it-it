---
description: Funzione SQLColAttribute
title: Funzione SQLColAttribute | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43980caf7a592284aba87d57725c30ae9fe8e949
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151246"
---
# <a name="sqlcolattribute-function"></a>Funzione SQLColAttribute
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLColAttribute** restituisce informazioni sul descrittore per una colonna in un set di risultati. Le informazioni sul descrittore vengono restituite come una stringa di caratteri, un valore dipendente dal descrittore o un valore integer.  
  
> [!NOTE]  
>  Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando ODBC 3. l'applicazione *x* funziona con ODBC 2. driver *x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *ColumnNumber*  
 Input Il numero del record in IRD da cui deve essere recuperato il valore del campo. Questo argomento corrisponde al numero di colonna di dati dei risultati, ordinati in sequenza in ordine crescente di colonne, a partire da 1. Le colonne possono essere descritte in qualsiasi ordine.  
  
 È possibile specificare la colonna 0 in questo argomento, ma tutti i valori eccetto SQL_DESC_TYPE e SQL_DESC_OCTET_LENGTH restituiranno valori non definiti.  
  
 *FieldIdentifier*  
 Input Handle del descrittore. Questo handle definisce il campo in IRD su cui deve essere eseguita una query, ad esempio SQL_COLUMN_TABLE_NAME.  
  
 *CharacterAttributePtr*  
 Output Puntatore a un buffer in cui restituire il valore nel campo *FieldIdentifier* della riga *ColumnNumber* di IRD, se il campo è una stringa di caratteri. In caso contrario, il campo non viene utilizzato.  
  
 Se *CharacterAttributePtr* è null, *StringLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *CharacterAttributePtr*.  
  
 *BufferLength*  
 Input Se *FieldIdentifier* è un campo definito da ODBC e *CharacterAttributePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di \* *CharacterAttributePtr*. Se *FieldIdentifier* è un campo definito da ODBC e \* *CharacterAttribute* ptr è un numero intero, questo campo viene ignorato. Se *\* CharacterAttributePtr* è una stringa Unicode (quando si chiama **SQLColAttributeW**), l'argomento *bufferLength* deve essere un numero pari. Se *FieldIdentifier* è un campo definito dal driver, l'applicazione indica la natura del campo per Gestione driver impostando l'argomento *bufferLength* . *BufferLength* può avere i valori seguenti:  
  
-   Se *CharacterAttributePtr* è un puntatore a un puntatore, il valore di *BufferLength* deve essere SQL_IS_POINTER.  
  
-   Se *CharacterAttributePtr* è un puntatore a una stringa di caratteri, *bufferLength* è la lunghezza del buffer.  
  
-   Se *CharacterAttributePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *bufferLength*. Questo inserisce un valore negativo in *bufferLength*.  
  
-   Se *CharacterAttributePtr* è un puntatore a un tipo di dati a lunghezza fissa, *bufferLength* deve essere uno dei seguenti: SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT.  
  
 *StringLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di byte, escluso il byte di terminazione null per i dati di tipo carattere, disponibile per restituire in **CharacterAttributePtr*.  
  
 Per i dati di tipo carattere, se il numero di byte disponibili per restituire è maggiore o uguale a *bufferLength*, le informazioni sul descrittore in \* *CharacterAttributePtr* vengono troncate a *bufferLength* meno la lunghezza di un carattere di terminazione null e con terminazione null dal driver.  
  
 Per tutti gli altri tipi di dati, il valore di *bufferLength* viene ignorato e il driver presuppone che la dimensione di **CharacterAttributePtr* sia 32 bit.  
  
 *NumericAttributePtr*  
 Output Puntatore a un buffer Integer in cui restituire il valore nel campo *FieldIdentifier* della riga *ColumnNumber* di IRD, se il campo è un tipo di descrittore numerico, ad esempio SQL_DESC_COLUMN_LENGTH. In caso contrario, il campo non viene utilizzato. Si noti che alcuni driver possono scrivere solo la versione inferiore a 32 bit o a 16 bit di un buffer e lasciare invariato il bit di ordine superiore. Pertanto, le applicazioni devono inizializzare il valore su 0 prima di chiamare questa funzione.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLColAttribute** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLColAttribute** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il buffer \* *CharacterAttributePtr* non era sufficientemente grande da restituire l'intero valore stringa, quindi il valore stringa è stato troncato. La lunghezza del valore stringa untruncated viene restituita in **StringLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07005|L'istruzione preparata non è una *specifica del cursore*|L'istruzione associata a *statementHandle* non ha restituito un set di risultati e *FieldIdentifier* non è stato SQL_DESC_COUNT. Nessuna colonna da descrivere.|  
|07009|Indice del descrittore non valido|(DM) il valore specificato per *ColumnNumber* è uguale a 0 e l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARKS è stato SQL_UB_OFF.<br /><br /> Il valore specificato per l'argomento *ColumnNumber* è maggiore del numero di colonne nel set di risultati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagField** dalla struttura dei dati di diagnostica descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione Aynchronous era ancora in esecuzione quando è stato chiamato SQLColAttribute.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) la funzione è stata chiamata prima di chiamare **SQLPrepare**, **SQLExecDirect** o una funzione di catalogo per *statementHandle*.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) *\* CharacterAttributePtr* è una stringa di caratteri e *bufferLength* è minore di 0 ma non uguale a SQL_NTS.|  
|HY091|Identificatore del campo del descrittore non valido|Il valore specificato per l'argomento *FieldIdentifier* non è uno dei valori definiti e non è un valore definito dall'implementazione.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Driver non in grado di supportare|Il valore specificato per l'argomento *FieldIdentifier* non è supportato dal driver.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
 Quando viene chiamato dopo **SQLPrepare** e prima di **SQLExecute**, **SQLColAttribute** può restituire qualsiasi SQLSTATE che può essere restituito da **SQLPrepare** o **SQLExecute**, a seconda del momento in cui l'origine dati valuta l'istruzione SQL associata a *statementHandle*.  
  
 Per motivi di prestazioni, un'applicazione non deve chiamare **SQLColAttribute** prima di eseguire un'istruzione.  
  
## <a name="comments"></a>Commenti  
 Per informazioni sul modo in cui le applicazioni utilizzano le informazioni restituite da **SQLColAttribute**, vedere [metadati del set di risultati](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLColAttribute** restituisce informazioni in \* *NumericAttributePtr* o in \* *CharacterAttributePtr*. Le informazioni integer vengono restituite in \* *NumericAttributePtr* come valore SQLLEN; tutti gli altri formati di informazioni vengono restituiti in \* *CharacterAttributePtr*. Quando vengono restituite informazioni in \* *NumericAttributePtr*, il driver ignora *CharacterAttributePtr*, *bufferLength* e *StringLengthPtr*. Quando vengono restituite informazioni in \* *CharacterAttributePtr*, il driver ignora *NumericAttributePtr*.  
  
 **SQLColAttribute** restituisce i valori dei campi del descrittore di IRD. La funzione viene chiamata con un handle di istruzione anziché un handle del descrittore. I valori restituiti da **SQLColAttribute** per i valori *FieldIdentifier* elencati più avanti in questa sezione possono anche essere recuperati chiamando **SQLGetDescField** con l'handle IRD appropriato.  
  
 I campi del descrittore attualmente definiti, la versione di ODBC in cui sono stati introdotti e gli argomenti in cui vengono restituite le informazioni vengono illustrati più avanti in questa sezione. più tipi di descrittori possono essere definiti dai driver per sfruttare le diverse origini dati.  
  
 ODBC 3. il driver *x* deve restituire un valore per ogni campo del descrittore. Se un campo del descrittore non è applicabile a un driver o a un'origine dati e se non diversamente specificato, il driver restituisce 0 in \* *StringLengthPtr* o una stringa vuota in **CharacterAttributePtr*.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 ODBC 3. la funzione *x* **SQLColAttribute** sostituisce la deprecata ODBC 2. **SQLColAttributes** funzione *x* . Quando si esegue il mapping di **SQLColAttributes** a **SQLCOLATTRIBUTE** (quando ODBC 2.** l'applicazione x funziona con ODBC 3. driver *x* ) o mapping di **SQLColAttribute** a **SQLColAttributes** (quando ODBC 3.** l'applicazione x funziona con ODBC 2. driver *x* ), la gestione driver passa il valore di *FieldIdentifier* tramite, ne esegue il mapping a un nuovo valore o restituisce un errore, come indicato di seguito:  
  
> [!NOTE]  
>  Prefisso utilizzato nei valori *FieldIdentifier* in ODBC 3. *x* è stato modificato rispetto a quello utilizzato in ODBC 2. *x*. Il nuovo prefisso è "SQL_DESC"; il prefisso precedente è "SQL_COLUMN".  
  
-   Se il valore **#define** di ODBC 2. *x* *FieldIdentifier* è uguale al valore **#define** di ODBC 3. *x* *FieldIdentifier*, il valore nella chiamata di funzione viene appena passato.  
  
-   Valori **#define** di ODBC 2. *x* *FieldIdentifiers* SQL_COLUMN_LENGTH, SQL_COLUMN_PRECISION e SQL_COLUMN_SCALE sono diversi dai valori **#define** di ODBC 3. *x* *FieldIdentifiers* SQL_DESC_PRECISION, SQL_DESC_SCALE e SQL_DESC_LENGTH. ODBC 2. il driver *x* deve supportare solo ODBC 2. valori *x* . ODBC 3. il driver *x* deve supportare sia i valori "SQL_COLUMN" che "SQL_DESC" per questi tre *FieldIdentifiers*. Questi valori sono diversi perché la precisione, la scala e la lunghezza sono definite in modo diverso in ODBC 3. *x* rispetto a ODBC 2. *x*. Per altre informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
-   Se il valore **#define** di ODBC 2. *x* *FieldIdentifier* è diverso dal valore **#define** di ODBC 3. *x* *FieldIdentifier*, come avviene con il conteggio, il nome e i valori nullable, il valore nella chiamata di funzione viene mappato al valore corrispondente. Ad esempio, SQL_COLUMN_COUNT viene mappato a SQL_DESC_COUNT e SQL_DESC_COUNT viene mappato a SQL_COLUMN_COUNT, a seconda della direzione del mapping.  
  
-   Se *FieldIdentifier* è un nuovo valore in ODBC 3. *x*, per cui non esiste alcun valore corrispondente in ODBC 2. *x*, non verrà mappato quando ODBC 3. l'applicazione *x* la utilizza in una chiamata a **SQLColAttribute** in ODBC 2. driver *x* e la chiamata restituirà SQLSTATE HY091 (identificatore del campo del descrittore non valido).  
  
 Nella tabella seguente sono elencati i tipi di descrittori restituiti da **SQLColAttribute**. Il tipo per i valori *NumericAttributePtr* è ** \* SQLLEN* _.  
  
|_FieldIdentifier *|Informazioni<br /><br /> restituito in|Descrizione|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE se la colonna è una colonna a incremento automatico.<br /><br /> SQL_FALSE se la colonna non è una colonna a incremento automatico o non è numerica.<br /><br /> Questo campo è valido solo per le colonne con tipi di dati numerici. Un'applicazione può inserire valori in una riga contenente una colonna AutoIncrement, ma in genere non può aggiornare i valori nella colonna.<br /><br /> Quando viene eseguita un'istruzione INSERT in una colonna AutoIncrement, viene inserito un valore univoco nella colonna in fase di inserimento. L'incremento non è definito, ma è specifico dell'origine dati. Un'applicazione non deve presupporre che una colonna AutoIncrement venga avviata in un determinato punto o incrementi di un determinato valore.|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3,0)|*CharacterAttributePtr*|Nome della colonna di base per la colonna del set di risultati. Se non esiste un nome di colonna di base (come nel caso di colonne che sono espressioni), questa variabile contiene una stringa vuota.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_BASE_COLUMN_NAME record di IRD, che è un campo di sola lettura.|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Nome della tabella di base che contiene la colonna. Se il nome della tabella di base non può essere definito o non è applicabile, questa variabile contiene una stringa vuota.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_BASE_TABLE_NAME record di IRD, che è un campo di sola lettura.|  
|SQL_DESC_CASE_SENSITIVE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE se la colonna viene considerata come distinzione tra maiuscole e minuscole per le regole di confronto e i confronti.<br /><br /> SQL_FALSE se la colonna non viene considerata come distinzione tra maiuscole e minuscole per le regole di confronto e i confronti oppure non è di tipo carattere.|  
|SQL_DESC_CATALOG_NAME (ODBC 2,0)|*CharacterAttributePtr*|Catalogo della tabella contenente la colonna. Il valore restituito è definito dall'implementazione se la colonna è un'espressione o se la colonna fa parte di una vista. Se l'origine dati non supporta i cataloghi o non è possibile determinare il nome del catalogo, viene restituita una stringa vuota. Questo campo di record VARCHAR non è limitato a 128 caratteri.|  
|SQL_DESC_CONCISE_TYPE (ODBC 1,0)|*NumericAttributePtr*|Tipo di dati conciso.<br /><br /> Per i tipi di dati DateTime e Interval, questo campo restituisce il tipo di dati conciso; ad esempio, SQL_TYPE_TIME o SQL_INTERVAL_YEAR. Per ulteriori informazioni, vedere [identificatori e descrittori di tipi di dati](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) in Appendice D: tipi di dati.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_CONCISE_TYPE record del IRD.|  
|SQL_DESC_COUNT (ODBC 1,0)|*NumericAttributePtr*|Numero di colonne disponibili nel set di risultati. Restituisce 0 se non sono presenti colonne nel set di risultati. Il valore nell'argomento *ColumnNumber* viene ignorato.<br /><br /> Queste informazioni vengono restituite dal campo di intestazione SQL_DESC_COUNT di IRD.|  
|SQL_DESC_DISPLAY_SIZE (ODBC 1,0)|*NumericAttributePtr*|Numero massimo di caratteri necessari per visualizzare i dati della colonna. Per ulteriori informazioni sulle dimensioni di visualizzazione, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE se la colonna ha una precisione fissa e una scala diversa da zero che sono specifiche dell'origine dati.<br /><br /> SQL_FALSE se la colonna non ha una precisione fissa e una scala diversa da zero che sono specifiche dell'origine dati.|  
|SQL_DESC_LABEL (ODBC 2,0)|*CharacterAttributePtr*|Etichetta o titolo della colonna. Ad esempio, una colonna denominata EmpName potrebbe essere etichettata come nome del dipendente o potrebbe essere etichettata con un alias.<br /><br /> Se una colonna non dispone di un'etichetta, viene restituito il nome della colonna. Se la colonna è senza etichetta e non è denominata, viene restituita una stringa vuota.|  
|SQL_DESC_LENGTH (ODBC 3,0)|*NumericAttributePtr*|Valore numerico che rappresenta la lunghezza di caratteri massima o effettiva di una stringa di caratteri o di un tipo di dati binario. Corrisponde alla lunghezza massima dei caratteri per un tipo di dati a lunghezza fissa o alla lunghezza effettiva dei caratteri per un tipo di dati a lunghezza variabile. Il valore esclude sempre il byte di terminazione null che termina la stringa di caratteri.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_LENGTH record del IRD.<br /><br /> Per ulteriori informazioni sulla lunghezza, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3,0)|*CharacterAttributePtr*|Questo campo di record VARCHAR (128) contiene il carattere o i caratteri riconosciuti dal driver come prefisso per un valore letterale di questo tipo di dati. Questo campo contiene una stringa vuota per un tipo di dati per il quale non è applicabile un prefisso letterale. Per ulteriori informazioni, vedere [prefissi e suffissi letterali](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3,0)|*CharacterAttributePtr*|Questo campo di record VARCHAR (128) contiene il carattere o i caratteri riconosciuti dal driver come suffisso per un valore letterale di questo tipo di dati. Questo campo contiene una stringa vuota per un tipo di dati per il quale non è applicabile un suffisso letterale. Per ulteriori informazioni, vedere [prefissi e suffissi letterali](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Questo campo di record VARCHAR (128) contiene qualsiasi nome localizzato (lingua nativa) per il tipo di dati che può essere diverso dal nome regolare del tipo di dati. Se non è presente alcun nome localizzato, viene restituita una stringa vuota. Questo campo è solo a scopo di visualizzazione. Il set di caratteri della stringa dipende dalle impostazioni locali ed è in genere il set di caratteri predefinito del server.|  
|SQL_DESC_NAME (ODBC 3,0)|*CharacterAttributePtr*|Alias di colonna, se applicabile. Se l'alias di colonna non è applicabile, viene restituito il nome della colonna. In entrambi i casi, SQL_DESC_UNNAMED è impostato su SQL_NAMED. Se non è presente alcun nome di colonna o alias di colonna, viene restituita una stringa vuota e SQL_DESC_UNNAMED è impostata su SQL_UNNAMED.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_NAME record del IRD.|  
|SQL_DESC_NULLABLE (ODBC 3,0)|*NumericAttributePtr*|SQL_ NULLABLE se la colonna può includere valori NULL; SQL_NO_NULLS se la colonna non contiene valori NULL. oppure SQL_NULLABLE_UNKNOWN se non è noto se la colonna accetta valori NULL.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_NULLABLE record del IRD.|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3,0)|*NumericAttributePtr*|Se il tipo di dati nel campo SQL_DESC_TYPE è un tipo di dati numerico approssimato, questo campo SQLINTEGER contiene il valore 2 perché il campo SQL_DESC_PRECISION contiene il numero di bit. Se il tipo di dati nel campo SQL_DESC_TYPE è un tipo di dati numerico esatto, questo campo contiene un valore pari a 10 perché il campo SQL_DESC_PRECISION contiene il numero di cifre decimali. Questo campo è impostato su 0 per tutti i tipi di dati non numerici.|  
|SQL_DESC_OCTET_LENGTH (ODBC 3,0)|*NumericAttributePtr*|Lunghezza, in byte, di una stringa di caratteri o di un tipo di dati binario. Per i tipi di carattere a lunghezza fissa o binaria, questa è la lunghezza effettiva in byte. Per i tipi di carattere o binari a lunghezza variabile, si tratta della lunghezza massima in byte. Questo valore non include il carattere di terminazione null.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_OCTET_LENGTH record del IRD.<br /><br /> Per ulteriori informazioni sulla lunghezza, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|SQL_DESC_PRECISION (ODBC 3,0)|*NumericAttributePtr*|Un valore numerico che per un tipo di dati numerico denota la precisione applicabile. Per i tipi di dati SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP e tutti i tipi di dati intervallo che rappresentano un intervallo di tempo, il relativo valore è la precisione applicabile del componente secondi frazionari.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_PRECISION record del IRD.|  
|SQL_DESC_SCALE (ODBC 3,0)|*NumericAttributePtr*|Valore numerico che rappresenta la scala applicabile per un tipo di dati numerico. Per i tipi di dati DECIMAL e NUMERIC, si tratta della scala definita. Non è definito per tutti gli altri tipi di dati.<br /><br /> Queste informazioni vengono restituite dal campo del record di SCALAbilità di IRD.|  
|SQL_DESC_SCHEMA_NAME (ODBC 2,0)|*CharacterAttributePtr*|Schema della tabella che contiene la colonna. Il valore restituito è definito dall'implementazione se la colonna è un'espressione o se la colonna fa parte di una vista. Se l'origine dati non supporta schemi o non è possibile determinare il nome dello schema, viene restituita una stringa vuota. Questo campo di record VARCHAR non è limitato a 128 caratteri.|  
|SQL_DESC_SEARCHABLE (ODBC 1,0)|*NumericAttributePtr*|SQL_PRED_NONE se la colonna non può essere utilizzata in una clausola WHERE. (Corrisponde al valore SQL_UNSEARCHABLE in ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR se la colonna può essere utilizzata in una clausola WHERE, ma solo con il predicato LIKE. (Corrisponde al valore SQL_LIKE_ONLY in ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC se la colonna può essere utilizzata in una clausola WHERE con tutti gli operatori di confronto, ad eccezione di come. (Corrisponde al valore SQL_EXCEPT_LIKE in ODBC 2. *x*.)<br /><br /> SQL_PRED_SEARCHABLE se la colonna può essere utilizzata in una clausola WHERE con qualsiasi operatore di confronto.<br /><br /> Le colonne di tipo SQL_LONGVARCHAR e SQL_LONGVARBINARY in genere restituiscono SQL_PRED_CHAR.|  
|SQL_DESC_TABLE_NAME (ODBC 2,0)|*CharacterAttributePtr*|Nome della tabella che contiene la colonna. Il valore restituito è definito dall'implementazione se la colonna è un'espressione o se la colonna fa parte di una vista.<br /><br /> Se non è possibile determinare il nome della tabella, viene restituita una stringa vuota.|  
|SQL_DESC_TYPE (ODBC 3,0)|*NumericAttributePtr*|Valore numerico che specifica il tipo di dati SQL.<br /><br /> Quando *ColumnNumber* è uguale a 0, viene restituito SQL_BINARY per i segnalibri a lunghezza variabile e viene restituito SQL_INTEGER per i segnalibri a lunghezza fissa.<br /><br /> Per i tipi di dati DateTime e Interval, questo campo restituisce il tipo di dati verbose: SQL_DATETIME o SQL_INTERVAL. Per ulteriori informazioni, vedere [identificatori e descrittori del tipo di dati](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) in Appendice D: tipi di dati.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_TYPE record del IRD. **Nota:**  Per utilizzare ODBC 2. per i driver *x* , usare invece SQL_DESC_CONCISE_TYPE.|  
|SQL_DESC_TYPE_NAME (ODBC 1,0)|*CharacterAttributePtr*|Nome del tipo di dati dipendente dall'origine dati; ad esempio, "CHAR", "VARCHAR", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA".<br /><br /> Se il tipo è sconosciuto, viene restituita una stringa vuota.|  
|SQL_DESC_UNNAMED (ODBC 3,0)|*NumericAttributePtr*|SQL_NAMED o SQL_UNNAMED. Se il SQL_DESC_NAME campo di IRD contiene un alias di colonna o un nome di colonna, viene restituito SQL_NAMED. Se non è presente alcun nome di colonna o alias di colonna, viene restituito SQL_UNNAMED.<br /><br /> Queste informazioni vengono restituite dal campo SQL_DESC_UNNAMED record del IRD.|  
|SQL_DESC_UNSIGNED (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE se la colonna non è firmata (o non è numerica).<br /><br /> SQL_FALSE se la colonna è firmata.|  
|SQL_DESC_UPDATABLE (ODBC 1,0)|*NumericAttributePtr*|La colonna è descritta dai valori per le costanti definite:<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE descrive aggiornabilità della colonna nel set di risultati, non della colonna nella tabella di base. Il valore di aggiornabilità della colonna di base su cui si basa la colonna del set di risultati può essere diverso da quello in questo campo. Il fatto che una colonna sia aggiornabile può essere basata sul tipo di dati, sui privilegi utente e sulla definizione del set di risultati stesso. Se non è chiaro se una colonna è aggiornabile, è necessario che venga restituito SQL_ATTR_READWRITE_UNKNOWN.|  
  
 **SQLColAttribute** è un'alternativa estendibile a **SQLDescribeCol**. **SQLDescribeCol** restituisce un set fisso di informazioni sul descrittore in base a SQL ANSI-89. **SQLColAttribute** consente l'accesso a un set più completo di informazioni sul descrittore disponibili in ANSI SQL-92 e DBMS Vendor Extensions.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su una colonna in un set di risultati|[Funzione SQLDescribeCol ](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di più righe di dati|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>Esempio  
 Nel codice di esempio seguente non sono disponibili handle e connessioni. Vedere la [funzione SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md), il [programma ODBC di esempio](../../../odbc/reference/sample-odbc-program.md)e la [funzione SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md) per gli esempi di codice per liberare handle e istruzioni.  
  
```cpp  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;
   
   retCode = SQLNumResultCols(hstmt, &numColumn);  
   
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Programma di esempio ODBC](../../../odbc/reference/sample-odbc-program.md)
