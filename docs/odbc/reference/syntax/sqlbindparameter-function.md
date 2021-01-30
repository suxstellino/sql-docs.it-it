---
description: Pagina relativa alla funzione SQLBindParameter
title: Funzione SQLBindParameter | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBindParameter
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindParameter
helpviewer_keywords:
- SQLBindParameter function [ODBC]
ms.assetid: 38349d4b-be03-46f9-9d6a-e50dd144e225
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d173d3cc38a0f07532f6b9d8a225d9bbf44a6a28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181080"
---
# <a name="sqlbindparameter-function"></a>Pagina relativa alla funzione SQLBindParameter

**Conformità**  
 Versione introdotta: ODBC 2,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLBindParameter** associa un buffer a un marcatore di parametro in un'istruzione SQL. **SQLBindParameter** supporta l'associazione a un tipo di dati C Unicode, anche se il driver sottostante non supporta i dati Unicode.  
  
> [!NOTE]  
>  Questa funzione sostituisce la funzione ODBC 1,0 **SQLSetParam**. Per ulteriori informazioni, vedere "Commenti".  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN SQLBindParameter(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT     InputOutputType,  
      SQLSMALLINT     ValueType,  
      SQLSMALLINT     ParameterType,  
      SQLULEN         ColumnSize,  
      SQLSMALLINT     DecimalDigits,  
      SQLPOINTER      ParameterValuePtr,  
      SQLLEN          BufferLength,  
      SQLLEN *        StrLen_or_IndPtr);  
```
  
## <a name="arguments"></a>Argomenti

 *StatementHandle*  
 Input Handle di istruzione.  
  
 *ParameterNumber*  
 Input Numero di parametro, ordinato sequenzialmente nell'ordine crescente del parametro, a partire da 1.  
  
 *InputOutputType*  
 Input Tipo di parametro. Per ulteriori informazioni, vedere "argomento *InputOutputType* " in "Commenti".  
  
 *ValueType*  
 Input Tipo di dati C del parametro. Per ulteriori informazioni, vedere "argomento *ValueType* " in "Commenti".  
  
 *ParameterType*  
 Input Tipo di dati SQL del parametro. Per ulteriori informazioni, vedere "argomento *ParameterType* " in "Commenti".  
  
 *ColumnSize*  
 Input Dimensione della colonna o dell'espressione del marcatore di parametro corrispondente. Per ulteriori informazioni, vedere "argomento *ColumnSize* " in "Commenti".  
  
 Se l'applicazione viene eseguita su un sistema operativo Windows a 64 bit, vedere [informazioni su ODBC 64-bit](../../../odbc/reference/odbc-64-bit-information.md).  
  
 *DecimalDigits*  
 Input Cifre decimali della colonna o dell'espressione del marcatore di parametro corrispondente. Per ulteriori informazioni sulle dimensioni delle colonne, vedere [dimensioni delle colonne, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *ParameterValuePtr*  
 [Input posticipato] Puntatore a un buffer per i dati del parametro. Per ulteriori informazioni, vedere "argomento *ParameterValuePtr* " in "Commenti".  
  
 *BufferLength*  
 [Input/output] Lunghezza del buffer *ParameterValuePtr* in byte. Per ulteriori informazioni, vedere "argomento *bufferLength* " in "Commenti".  
  
 Vedere [informazioni su ODBC 64 bit](../../../odbc/reference/odbc-64-bit-information.md), se l'applicazione viene eseguita su un sistema operativo a 64 bit.  
  
 *StrLen_or_IndPtr*  
 [Input posticipato] Puntatore a un buffer per la lunghezza del parametro. Per ulteriori informazioni, vedere "argomento *StrLen_or_IndPtr* " in "Commenti".  
  
## <a name="returns"></a>Restituisce

 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica

 Quando **SQLBindParameter** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLBindParameter** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  

|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Il tipo di dati identificato dall'argomento *ValueType* non può essere convertito nel tipo di dati identificato dall'argomento *ParameterType* . Si noti che questo errore può essere restituito da **SQLExecDirect**, **SQLExecute** o **SQLPutData** in fase di esecuzione, anziché da **SQLBindParameter**.|  
|07009|Indice del descrittore non valido|(DM) il valore specificato per l'argomento *ParameterNumber* è minore di 1.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer **MessageText* descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY003|Tipo di buffer dell'applicazione non valido|Il valore specificato dall'argomento *ValueType* non è un tipo di dati C o un SQL_C_DEFAULT valido.|  
|HY004|Tipo di dati SQL non valido|Il valore specificato per l'argomento *ParameterType* non è né un identificatore del tipo di dati ODBC SQL valido né un identificatore del tipo di dati SQL specifico del driver supportato dal driver.|  
|HY009|Valore argomento non valido|(DM) l'argomento *ParameterValuePtr* è un puntatore null, l'argomento *StrLen_or_IndPtr* è un puntatore null e l'argomento *InputOutputType* non è stato SQL_PARAM_OUTPUT.<br /><br /> (DM) SQL_PARAM_OUTPUT, dove l'argomento *ParameterValuePtr* è un puntatore null, il tipo C è char o binary e bufferLength (*cbValueMax*) è maggiore di 0.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stato chiamato **SQLBindParameter** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY021|Informazioni descrittore incoerenti|Le informazioni sul descrittore controllate durante una verifica di coerenza non erano coerenti. (Vedere la sezione "verifiche di coerenza" in **SQLSetDescField**).<br /><br /> Il valore specificato per l'argomento *DecimalDigits* non è compreso nell'intervallo di valori supportato dall'origine dati per una colonna del tipo di dati SQL specificato dall'argomento *ParameterType* .|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore in *bufferLength* è minore di 0. (Vedere la descrizione del campo SQL_DESC_DATA_PTR in **SQLSetDescField**).|  
|HY104|Valore di precisione o di scala non valido|Il valore specificato per l'argomento *ColumnSize* o *DecimalDigits* non è compreso nell'intervallo di valori supportato dall'origine dati per una colonna del tipo di dati SQL specificato dall'argomento *ParameterType* .|  
|HY105|Tipo di parametro non valido|(DM) il valore specificato per l'argomento *InputOutputType* non è valido. (Vedere "Commenti").|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il driver o l'origine dati non supporta la conversione specificata dalla combinazione del valore specificato per l'argomento *ValueType* e del valore specifico del driver specificato per l'argomento *ParameterType*.<br /><br /> Il valore specificato per l'argomento *ParameterType* era un identificatore del tipo di dati ODBC SQL valido per la versione di ODBC supportata dal driver ma non supportato dal driver o dall'origine dati.<br /><br /> Il driver supporta solo ODBC 2. *x* e l'argomento *ValueType* è uno dei seguenti:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> e tutti i tipi di dati intervallo C elencati nei tipi di dati [c](../../../odbc/reference/appendixes/c-data-types.md) in Appendice D: tipi di dati.<br /><br /> Il driver supporta solo le versioni ODBC precedenti alla 3,50 e l'argomento *ValueType* è stato SQL_C_GUID.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti

 Un'applicazione chiama **SQLBindParameter** per associare ogni marcatore di parametro in un'istruzione SQL. Le associazioni rimangono attive fino a quando l'applicazione chiama di nuovo **SQLBindParameter** , chiama **SQLFreeStmt** con l'opzione SQL_RESET_PARAMS o chiama **SQLSetDescField** per impostare il campo di intestazione SQL_DESC_COUNT di APD su 0.  
  
 Per ulteriori informazioni sui parametri, vedere [parametri di istruzione](../../../odbc/reference/develop-app/statement-parameters.md). Per altre informazioni sui tipi di dati dei parametri e i marcatori di parametro, vedere [tipi di dati dei](../../../odbc/reference/appendixes/parameter-data-types.md) parametri e [marcatori di parametro](../../../odbc/reference/appendixes/parameter-markers.md) nell'Appendice C: grammatica SQL.  
  
## <a name="parameternumber-argument"></a>Argomento ParameterNumber  
 Se *ParameterNumber* nella chiamata a **SQLBindParameter** è maggiore del valore di SQL_DESC_COUNT, viene chiamato **SQLSetDescField** per aumentare il valore di SQL_DESC_COUNT a *ParameterNumber*.  
  
## <a name="inputoutputtype-argument"></a>Argomento InputOutputType  
 L'argomento *InputOutputType* specifica il tipo di parametro. Questo argomento imposta il campo SQL_DESC_PARAMETER_TYPE del dip. Tutti i parametri nelle istruzioni SQL che non chiamano procedure, ad esempio istruzioni **Insert** , sono  *parametri* di input. I parametri nelle chiamate di routine possono essere parametri di input, di input/output o di output. Un'applicazione chiama **SQLProcedureColumns** per determinare il tipo di un parametro in una chiamata di routine. i parametri di cui non è possibile determinare il tipo sono considerati parametri di input.  
  
 L'argomento *InputOutputType* è rappresentato da uno dei valori seguenti:  
  
-   SQL_PARAM_INPUT. Il parametro contrassegna un parametro in un'istruzione SQL che non chiama una routine, ad esempio un'istruzione **Insert** , o contrassegna un parametro di input in una routine. Ad esempio, i parametri in **Insert into employee values (?,?,?)** sono parametri di input, mentre i parametri in **{call AddEmp (?,?,?)}** possono essere, ma non necessariamente, parametri di input.  
  
     Quando l'istruzione viene eseguita, il driver invia i dati per il parametro all'origine dati. il \* buffer *ParameterValuePtr* deve contenere un valore di input valido oppure il buffer **StrLen_or_IndPtr* deve contenere SQL_NULL_DATA, SQL_DATA_AT_EXEC o il risultato della macro SQL_LEN_DATA_AT_EXEC.  
  
     Se un'applicazione non è in grado di determinare il tipo di un parametro in una chiamata di routine, imposta *InputOutputType* su SQL_PARAM_INPUT; Se l'origine dati restituisce un valore per il parametro, il driver lo ignora.  
  
-   SQL_PARAM_INPUT_OUTPUT. Il parametro contrassegna un parametro di input/output in una procedura. Ad esempio, il parametro in **{Call GetEmpDept (?)}** è un parametro di input/output che accetta il nome di un dipendente e restituisce il nome del reparto del dipendente.  
  
     Quando l'istruzione viene eseguita, il driver invia i dati per il parametro all'origine dati. il \* buffer *ParameterValuePtr* deve contenere un valore di input valido oppure il \* buffer di *StrLen_or_IndPtr* deve contenere SQL_NULL_DATA, SQL_DATA_AT_EXEC o il risultato della macro SQL_LEN_DATA_AT_EXEC. Dopo l'esecuzione dell'istruzione, il driver restituisce i dati per il parametro all'applicazione; Se l'origine dati non restituisce un valore per un parametro di input/output, il driver imposta il buffer **StrLen_or_IndPtr* su SQL_NULL_DATA.  
  
    > [!NOTE]  
    >  Quando un'applicazione ODBC 1,0 chiama **SQLSetParam** in un driver ODBC 2,0, gestione driver lo converte in una chiamata a **SQLBindParameter** in cui l'argomento *InputOutputType* è impostato su SQL_PARAM_INPUT_OUTPUT.  
  
-   SQL_PARAM_OUTPUT. Il parametro contrassegna il valore restituito di una routine o di un parametro di output in una procedura. in entrambi i casi, questi sono noti come *parametri di output*. Ad esempio, il parametro in **{? = Call GetNextEmpID}** è un parametro di output che restituisce l'ID dipendente successivo.  
  
     Dopo l'esecuzione dell'istruzione, il driver restituisce i dati per il parametro all'applicazione, a meno che gli argomenti *ParameterValuePtr* e *StrLen_or_IndPtr* siano entrambi puntatori null, nel qual caso il driver ignora il valore di output. Se l'origine dati non restituisce un valore per un parametro di output, il driver imposta il buffer **StrLen_or_IndPtr* su SQL_NULL_DATA.  
  
-   SQL_PARAM_INPUT_OUTPUT_STREAM. Indica che un parametro di input/output deve essere trasmesso. **SQLGetData** può leggere i valori dei parametri in parti. *BufferLength* viene ignorato perché la lunghezza del buffer viene determinata alla chiamata di **SQLGetData**. Il valore del buffer di *StrLen_or_IndPtr* deve contenere SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_DATA_AT_EXEC o il risultato della macro SQL_LEN_DATA_AT_EXEC. Un parametro deve essere associato come parametro data-at-execution (DAE) in fase di input se verrà trasmesso all'output. *ParameterValuePtr* può essere qualsiasi valore puntatore non null che verrà restituito da **SQLParamData** come token definito dall'utente il cui valore è stato passato con *ParameterValuePtr* sia per l'input che per l'output. Per altre informazioni, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_PARAM_OUTPUT_STREAM. Uguale a SQL_PARAM_INPUT_OUTPUT_STREAM, per un parametro di output. **StrLen_or_IndPtr* viene ignorato nell'input.  
  
 La tabella seguente elenca diverse combinazioni di *InputOutputType* e **StrLen_or_IndPtr*:  
  
|*InputOutputType*|**StrLen_or_IndPtr*|Risultato|Contrassegna in ParameterValuePtr|  
|-----------------------|----------------------------|-------------|---------------------------------|  
|SQL_PARAM_INPUT|SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Input in parti|*ParameterValuePtr* può essere qualsiasi valore del puntatore che verrà restituito da **SQLParamData** come token definito dall'utente il cui valore è stato passato con *ParameterValuePtr*.|  
|SQL_PARAM_INPUT|Not SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Buffer con binding di input|*ParameterValuePtr* è l'indirizzo del buffer di input.|  
|SQL_PARAM_OUTPUT|Ignorato in corrispondenza dell'input.|Buffer associato di output|*ParameterValuePtr* è l'indirizzo del buffer di output.|  
|SQL_PARAM_OUTPUT_STREAM|Ignorato in corrispondenza dell'input.|Output trasmesso|*ParameterValuePtr* può essere qualsiasi valore del puntatore, che verrà restituito da **SQLParamData** come token definito dall'utente il cui valore è stato passato con *ParameterValuePtr*.|  
|SQL_PARAM_INPUT_OUTPUT|SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Input in parti e buffer associato di output|*ParameterValuePtr* è l'indirizzo del buffer di output, che verrà restituito anche da **SQLParamData** come token definito dall'utente il cui valore è stato passato con *ParameterValuePtr*.|  
|SQL_PARAM_INPUT_OUTPUT|Not SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Buffer con binding di input e buffer associato di output|*ParameterValuePtr* è l'indirizzo del buffer di input/output condiviso.|
|SQL_PARAM_INPUT_OUTPUT_STREAM|SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Input in parti e output trasmesso|*ParameterValuePtr* può essere un qualsiasi valore puntatore non null, che verrà restituito da **SQLParamData** come token definito dall'utente il cui valore è stato passato con *ParameterValuePtr* sia per l'input che per l'output.|  
  
> [!NOTE]  
>  Il driver deve decidere quali tipi SQL sono consentiti quando un'applicazione associa un parametro di output o di input-output come trasmesso. Gestione driver non genererà un errore per un tipo SQL non valido.  
  
## <a name="valuetype-argument"></a>ValueType (argomento)

 L'argomento *ValueType* specifica il tipo di dati C del parametro. Questo argomento imposta i campi SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE e SQL_DESC_DATETIME_INTERVAL_CODE di APD. Deve essere uno dei valori nella sezione tipi di [dati C](../../../odbc/reference/appendixes/c-data-types.md) di Appendice D: tipi di dati.  
  
 Se l'argomento *ValueType* è uno dei tipi di dati interval, il campo SQL_DESC_TYPE del record *ParameterNumber* di apd viene impostato su SQL_INTERVAL, il campo SQL_DESC_CONCISE_TYPE di APD viene impostato sul tipo di dati intervallo conciso e il campo SQL_DESC_DATETIME_INTERVAL_CODE del record *ParameterNumber* viene impostato su un sottocodice per il tipo di dati intervallo specifico. Vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md). Per i dati vengono utilizzati rispettivamente la precisione del valore predefinito per l'intervallo (2) e la precisione dei secondi di intervallo predefiniti (6), come impostato nei campi SQL_DESC_DATETIME_INTERVAL_PRECISION e SQL_DESC_PRECISION di APD. Se una precisione predefinita non è appropriata, l'applicazione deve impostare in modo esplicito il campo del descrittore tramite una chiamata a **SQLSetDescField** o **SQLSetDescRec**.  
  
 Se l'argomento *ValueType* è uno dei tipi di dati DateTime, il campo SQL_DESC_TYPE del record *ParameterNumber* di apd viene impostato su SQL_DATETIME, il campo SQL_DESC_CONCISE_TYPE del record *ParameterNumber* di APD viene impostato sul tipo di dati DateTime C conciso e il campo SQL_DESC_DATETIME_INTERVAL_CODE del record *ParameterNumber* viene impostato su un sottocodice per il tipo di dati DateTime specifico. Vedere [Appendice D: tipi di dati](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 Se l'argomento *ValueType* è un tipo di dati SQL_C_NUMERIC, per i dati viene utilizzata la precisione predefinita (definita dal driver) e la scala predefinita (0), come impostato nei campi SQL_DESC_PRECISION e SQL_DESC_SCALE di APD. Se la precisione o la scala predefinita non è appropriata, l'applicazione deve impostare in modo esplicito il campo del descrittore tramite una chiamata a **SQLSetDescField** o **SQLSetDescRec**.  
  
 SQL_C_DEFAULT specifica che il valore del parametro deve essere trasferito dal tipo di dati C predefinito per il tipo di dati SQL specificato con *ParameterType*.  
  
 È inoltre possibile specificare un tipo di dati C esteso. Per ulteriori informazioni, vedere [tipi di dati C in ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 Per ulteriori informazioni, vedere [tipi di dati c predefiniti](../../../odbc/reference/appendixes/default-c-data-types.md), [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)e [conversione di dati da SQL a tipi di dati c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) in Appendice D: tipi di dati.  
  
## <a name="parametertype-argument"></a>ParameterType (argomento)

 Deve essere uno dei valori elencati nella sezione tipi di [dati SQL](../../../odbc/reference/appendixes/sql-data-types.md) di Appendice D: tipi di dati oppure deve essere un valore specifico del driver. Questo argomento imposta i campi SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE e SQL_DESC_DATETIME_INTERVAL_CODE del dip.  
  
 Se l'argomento *ParameterType* è uno degli identificatori DateTime, il campo SQL_DESC_TYPE di dpi è impostato su SQL_DATETIME, il campo SQL_DESC_CONCISE_TYPE di dpi viene impostato sul tipo di dati DateTime SQL conciso e il campo SQL_DESC_DATETIME_INTERVAL_CODE è impostato sul valore del codice del sottocodice DateTime appropriato.  
  
 Se *ParameterType* è uno degli identificatori di intervallo, il campo SQL_DESC_TYPE di dpi è impostato su SQL_INTERVAL, il campo SQL_DESC_CONCISE_TYPE di dpi viene impostato sul tipo di dati intervallo SQL conciso e il campo SQL_DESC_DATETIME_INTERVAL_CODE del dip viene impostato sul sottocodice intervallo appropriato. Il campo SQL_DESC_DATETIME_INTERVAL_PRECISION di dpi è impostato sulla precisione principale dell'intervallo e il campo SQL_DESC_PRECISION è impostato sulla precisione di intervallo di secondi, se applicabile. Se il valore predefinito di SQL_DESC_DATETIME_INTERVAL_PRECISION o SQL_DESC_PRECISION non è appropriato, l'applicazione deve impostarla in modo esplicito chiamando **SQLSetDescField**. Per ulteriori informazioni su uno di questi campi, vedere [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Se l'argomento *ValueType* è un tipo di dati SQL_NUMERIC, per i dati viene utilizzata la precisione predefinita (definita dal driver) e la scala predefinita (0), come impostato nei campi SQL_DESC_PRECISION e SQL_DESC_SCALE del dip. Se la precisione o la scala predefinita non è appropriata, l'applicazione deve impostare in modo esplicito il campo del descrittore tramite una chiamata a **SQLSetDescField** o **SQLSetDescRec**.  
  
 Per informazioni sul modo in cui i dati vengono convertiti, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) e [conversione di dati da SQL in tipi di dati c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) in Appendice D: tipi di dati.  
  
## <a name="columnsize-argument"></a>Argomento ColumnSize

 L'argomento *ColumnSize* specifica le dimensioni della colonna o dell'espressione che corrisponde al marcatore di parametro, alla lunghezza dei dati o a entrambi. Questo argomento imposta campi diversi del tipo di dati dpi, a seconda del tipo di dati SQL (argomento *ParameterType* ). Al mapping si applicano le regole seguenti:  
  
-   Se *ParameterType* è SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY o uno dei tipi di dati concise SQL DateTime o Interval, il campo SQL_DESC_LENGTH di dpi viene impostato sul valore di *ColumnSize*. Per ulteriori informazioni, vedere la sezione [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.  
  
-   Se *ParameterType* è SQL_DECIMAL, SQL_NUMERIC, SQL_FLOAT, SQL_REAL o SQL_DOUBLE, il campo SQL_DESC_PRECISION di dpi viene impostato sul valore di *ColumnSize*.  
  
-   Per gli altri tipi di dati, l'argomento *ColumnSize* viene ignorato.  
  
 Per ulteriori informazioni, vedere "passaggio di valori dei parametri" e SQL_DATA_AT_EXEC in "argomento *StrLen_or_IndPtr* ".  
  
## <a name="decimaldigits-argument"></a>Argomento DecimalDigits

 Se *ParameterType* è SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP, SQL_INTERVAL_SECOND, SQL_INTERVAL_DAY_TO_SECOND, SQL_INTERVAL_HOUR_TO_SECOND o SQL_INTERVAL_MINUTE_TO_SECOND, il campo SQL_DESC_PRECISION del dip viene impostato su *DecimalDigits*. Se *ParameterType* è SQL_NUMERIC o SQL_DECIMAL, il campo SQL_DESC_SCALE di dpi viene impostato su *DecimalDigits*. Per tutti gli altri tipi di dati, l'argomento *DecimalDigits* viene ignorato.  
  
## <a name="parametervalueptr-argument"></a>Argomento ParameterValuePtr

 L'argomento *ParameterValuePtr* punta a un buffer che, quando viene chiamato **SQLExecute** o **SQLExecDirect** , contiene i dati effettivi per il parametro. I dati devono essere nel formato specificato dall'argomento *ValueType* . Questo argomento imposta il campo SQL_DESC_DATA_PTR di APD. Un'applicazione può impostare l'argomento *ParameterValuePtr* su un puntatore null, purché *\* StrLen_or_IndPtr* sia SQL_NULL_DATA o SQL_DATA_AT_EXEC. Questo vale solo per i parametri di input o di input/output.  
  
 Se \* *StrLen_or_IndPtr* è il risultato della macro SQL_LEN_DATA_AT_EXEC (*length*) o SQL_DATA_AT_EXEC, *ParameterValuePtr* è un valore del puntatore definito dall'applicazione associato al parametro. Viene restituito all'applicazione tramite **SQLParamData**. Ad esempio, *ParameterValuePtr* potrebbe essere un token diverso da zero, ad esempio un numero di parametro, un puntatore ai dati o un puntatore a una struttura usata dall'applicazione per associare i parametri di input. Si noti tuttavia che se il parametro è un parametro di input/output, *ParameterValuePtr* deve essere un puntatore a un buffer in cui verrà archiviato il valore di output. Se il valore nell'attributo dell'istruzione SQL_ATTR_PARAMSET_SIZE è maggiore di 1, l'applicazione può usare il valore a cui fa riferimento l'attributo SQL_ATTR_PARAMS_PROCESSED_PTR istruzione insieme all'argomento *ParameterValuePtr* . Ad esempio, *ParameterValuePtr* può puntare a una matrice di valori e l'applicazione può usare il valore a cui punta SQL_ATTR_PARAMS_PROCESSED_PTR per recuperare il valore corretto dalla matrice. Per ulteriori informazioni, vedere "passaggio di valori dei parametri" più avanti in questa sezione.  
  
 Se l'argomento *InputOutputType* è SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_OUTPUT, *ParameterValuePtr* punta a un buffer in cui il driver restituisce il valore di output. Se la procedura restituisce uno o più set di risultati, \* non è garantito l'impostazione del buffer *ParameterValuePtr* fino a quando non vengono elaborati tutti i set di risultati o i conteggi delle righe. Se il buffer non è impostato fino al completamento dell'elaborazione, i parametri di output e i valori restituiti non saranno disponibili fino a quando **SQLMoreResults** non restituisce SQL_NO_DATA. Se si chiama **SQLCloseCursor** o **SQLFreeStmt** con un'opzione di SQL_CLOSE, questi valori verranno eliminati.  
  
 Se il valore nell'attributo dell'istruzione SQL_ATTR_PARAMSET_SIZE è maggiore di 1, *ParameterValuePtr* punta a una matrice. Una singola istruzione SQL elabora la matrice completa dei valori di input per un parametro di input o di input/output e restituisce una matrice di valori di output per un parametro di input/output o output.  
  
## <a name="bufferlength-argument"></a>Argomento BufferLength

 Per i dati di tipo carattere e binario C, l'argomento *bufferLength* specifica la lunghezza del \* buffer *ParameterValuePtr* (se è un singolo elemento) o la lunghezza di un elemento nella \* matrice *ParameterValuePtr* (se il valore nell'attributo dell'istruzione SQL_ATTR_PARAMSET_SIZE è maggiore di 1). Questo argomento imposta il campo record SQL_DESC_OCTET_LENGTH di APD. Se l'applicazione specifica più valori, viene usato *bufferLength* per determinare la posizione dei valori nella matrice **ParameterValuePtr* , sia per l'input che per l'output. Per i parametri di input/output e di output, viene usato per determinare se troncare i dati di tipo carattere e binario C nell'output:  
  
-   Per i dati di tipo carattere C, se il numero di byte disponibili per restituire è maggiore o uguale a *bufferLength*, i dati in \* *ParameterValuePtr* vengono troncati a *bufferLength* meno la lunghezza di un carattere di terminazione null e terminano con null dal driver.  
  
-   Per i dati C binari, se il numero di byte disponibili per la restituzione è maggiore di *bufferLength*, i dati in \* *ParameterValuePtr* vengono troncati a *bufferLength* byte.  
  
 Per tutti gli altri tipi di dati C, l'argomento *bufferLength* viene ignorato. La lunghezza del buffer \* *ParameterValuePtr* (se è un singolo elemento) o la lunghezza di un elemento nella \* matrice *ParameterValuePtr* (se l'applicazione chiama **SQLSetStmtAttr** con un argomento *attribute* di SQL_ATTR_PARAMSET_SIZE per specificare più valori per ogni parametro) si presuppone che sia la lunghezza del tipo di dati C.  
  
 Per i parametri di output o di input/output trasmessi in flusso, l'argomento *bufferLength* viene ignorato perché la lunghezza del buffer è specificata in **SQLGetData**.  
  
> [!NOTE]  
>  Quando un'applicazione ODBC 1,0 chiama **SQLSetParam** in un ODBC 3. driver *x* , gestione driver converte questa operazione in una chiamata a **SQLBindParameter** in cui l'argomento *bufferLength* è sempre SQL_SETPARAM_VALUE_MAX. Poiché Gestione driver restituisce un errore se ODBC 3. *x* l'applicazione imposta *bufferLength* su SQL_SETPARAM_VALUE_MAX, un ODBC 3. il driver *x* può utilizzare questo metodo per determinare quando viene chiamato da un'applicazione ODBC 1,0.  
  
> [!NOTE]  
>  In **SQLSetParam**, il modo in cui un'applicazione specifica la lunghezza del buffer **ParameterValuePtr* , in modo che il driver possa restituire dati di tipo carattere o binario e il modo in cui un'applicazione invia una matrice di valori di parametro character o Binary al driver, è definito dal driver.  
  
## <a name="strlen_or_indptr-argument"></a>Argomento StrLen_or_IndPtr

 L'argomento *StrLen_or_IndPtr* punta a un buffer che, quando viene chiamato **SQLExecute** o **SQLExecDirect** , contiene uno dei seguenti elementi. Questo argomento imposta i campi SQL_DESC_OCTET_LENGTH_PTR e SQL_DESC_INDICATOR_PTR record dei puntatori del parametro dell'applicazione.  
  
-   Lunghezza del valore del parametro archiviato in **ParameterValuePtr*. Viene ignorato ad eccezione dei dati di tipo carattere o binario C.  
  
-   SQL_NTS. Il valore del parametro è una stringa con terminazione null.  
  
-   SQL_NULL_DATA. Il valore del parametro è NULL.  
  
-   SQL_DEFAULT_PARAM. Una procedura consiste nell'usare il valore predefinito di un parametro, anziché un valore recuperato dall'applicazione. Questo valore è valido solo in una procedura chiamata in sintassi canonica ODBC e quindi solo se l'argomento *InputOutputType* è SQL_PARAM_INPUT, SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_INPUT_OUTPUT_STREAM. Quando \* *StrLen_or_IndPtr* viene SQL_DEFAULT_PARAM, gli *argomenti ValueType*, *ParameterType*, *ColumnSize*, *DecimalDigits*, *bufferLength* e *ParameterValuePtr* vengono ignorati per i parametri di input e vengono usati solo per definire il valore del parametro di output per i parametri di input/output.  
  
-   Risultato della macro SQL_LEN_DATA_AT_EXEC (*length*). I dati per il parametro verranno inviati con **SQLPutData**. Se l'argomento *ParameterType* è SQL_LONGVARBINARY, SQL_LONGVARCHAR o un tipo di dati Long, specifico dell'origine dati e il driver restituisce "Y" per il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo**, *length* è il numero di byte di dati da inviare per il parametro. in caso contrario, la *lunghezza* deve essere un valore non negativo e viene ignorata. Per ulteriori informazioni, vedere "passaggio di valori dei parametri" più avanti in questa sezione.  
  
     Ad esempio, per specificare che 10.000 byte di dati verranno inviati con **SQLPutData** in una o più chiamate, per un parametro di SQL_LONGVARCHAR, un'applicazione imposta **StrLen_or_IndPtr* SQL_LEN_DATA_AT_EXEC (10000).  
  
-   SQL_DATA_AT_EXEC. I dati per il parametro verranno inviati con **SQLPutData**. Questo valore viene usato dalle applicazioni ODBC 1,0 quando chiamano ODBC 3. driver *x* . Per ulteriori informazioni, vedere "passaggio di valori dei parametri" più avanti in questa sezione.  
  
 Se *StrLen_or_IndPtr* è un puntatore null, il driver presuppone che tutti i valori dei parametri di input siano non null e che i dati di tipo carattere e binario siano con terminazione null. Se *InputOutputType* è SQL_PARAM_OUTPUT o SQL_PARAM_OUTPUT_STREAM e *ParameterValuePtr* e *StrLen_or_IndPtr* sono entrambi puntatori null, il driver ignora il valore di output.  
  
> [!NOTE]  
>  Gli sviluppatori di applicazioni sono fortemente sconsigliati di specificare un puntatore null per *StrLen_or_IndPtr* quando il tipo di dati del parametro è SQL_C_BINARY. Per assicurarsi che un driver non tronca inaspettatamente i dati SQL_C_BINARY, *StrLen_or_IndPtr* deve contenere un puntatore a un valore di lunghezza valido.  
  
 Se l'argomento *InputOutputType* è SQL_PARAM_INPUT_OUTPUT, SQL_PARAM_OUTPUT, SQL_PARAM_INPUT_OUTPUT_STREAM o SQL_PARAM_OUTPUT_STREAM, *StrLen_or_IndPtr* punta a un buffer in cui il driver restituisce SQL_NULL_DATA, il numero di byte disponibili per la restituzione in \* *ParameterValuePtr* (escluso il byte di terminazione null di dati di tipo carattere) o SQL_NO_TOTAL (se non è possibile determinare il numero di byte disponibili per la restituzione). Se la procedura restituisce uno o più set di risultati, non è garantita l'impostazione del buffer **StrLen_or_IndPtr* finché non vengono recuperati tutti i risultati.  
  
 Se il valore nell'attributo dell'istruzione SQL_ATTR_PARAMSET_SIZE è maggiore di 1, *StrLen_or_IndPtr* punta a una matrice di valori SQLLEN. Questi possono essere uno qualsiasi dei valori elencati in precedenza in questa sezione e vengono elaborati con una singola istruzione SQL.  
  
## <a name="passing-parameter-values"></a>Passaggio di valori di parametri

 Un'applicazione può passare il valore per un parametro nel \* buffer *ParameterValuePtr* o con una o più chiamate a **SQLPutData**. I parametri i cui dati vengono passati con **SQLPutData** sono noti come parametri *data-at-execution* . Vengono in genere utilizzati per inviare dati per SQL_LONGVARBINARY e SQL_LONGVARCHAR parametri e possono essere combinati con altri parametri.  
  
 Per passare i valori dei parametri, un'applicazione esegue la sequenza di passaggi seguente:  
  
1.  Chiama **SQLBindParameter** per ogni parametro per associare i buffer per il valore del parametro (argomento *ParameterValuePtr* ) e la lunghezza/indicatore (*StrLen_or_IndPtr* argomento). Per i parametri data-at-execution, *ParameterValuePtr* è un valore di puntatore definito dall'applicazione, ad esempio un numero di parametro o un puntatore ai dati. Il valore verrà restituito in un secondo momento e potrà essere usato per identificare il parametro.  
  
2.  Inserisce i valori per i parametri di input e di input/output nei \* buffer *ParameterValuePtr* e **StrLen_or_IndPtr* :  
  
    -   Per i parametri normali, l'applicazione inserisce il valore del parametro nel \* buffer *ParameterValuePtr* e la lunghezza del valore nel buffer **StrLen_or_IndPtr* . Per ulteriori informazioni, vedere [impostazione dei valori dei parametri](../../../odbc/reference/develop-app/setting-parameter-values.md).  
  
    -   Per i parametri data-at-execution, l'applicazione inserisce il risultato della macro SQL_LEN_DATA_AT_EXEC (*length*) (quando si chiama un driver ODBC 2,0) nel buffer **StrLen_or_IndPtr* .  
  
3.  Chiama **SQLExecute** o **SQLExecDirect** per eseguire l'istruzione SQL.  
  
    -   Se non sono presenti parametri data-at-execution, il processo è completo.  
  
    -   Se sono presenti parametri data-at-execution, la funzione restituisce SQL_NEED_DATA.  
  
4.  Chiama **SQLParamData** per recuperare il valore definito dall'applicazione specificato nell'argomento *ParameterValuePtr* di **SQLBindParameter** affinché il primo parametro data-at-execution venga elaborato. **SQLParamData** restituisce SQL_NEED_DATA.  
  
    > [!NOTE]  
    >  Sebbene i parametri data-at-execution siano simili alle colonne data-at-execution, il valore restituito da **SQLParamData** è diverso per ogni. I parametri data-at-execution sono parametri in un'istruzione SQL per cui i dati verranno inviati con **SQLPutData** quando l'istruzione viene eseguita con **SQLExecDirect** o **SQLExecute**. Sono associati a **SQLBindParameter**. Il valore restituito da **SQLParamData** è un valore del puntatore passato a **SQLBindParameter** nell'argomento *ParameterValuePtr* . Le colonne data-at-execution sono colonne di un set di righe per cui i dati verranno inviati con **SQLPutData** quando una riga viene aggiornata o aggiunta con **SQLBulkOperations** o aggiornata con **SQLSetPos**. Sono associati a **SQLBindCol**. Il valore restituito da **SQLParamData** è l'indirizzo della riga nel buffer **TargetValuePtr* (impostato da una chiamata a **SQLBindCol**) in fase di elaborazione.  
  
5.  Chiama **SQLPutData** una o più volte per inviare i dati per il parametro. Sono necessarie più chiamate se il valore dei dati è maggiore del \* buffer *ParameterValuePtr* specificato in **SQLPutData**. sono consentite più chiamate a **SQLPutData** per lo stesso parametro solo quando si inviano dati di tipo carattere c a una colonna con un tipo di dati carattere, binario o specifico dell'origine dati oppure quando si inviano dati c binari a una colonna con tipo di dati character, Binary o data source.  
  
6.  Chiama di nuovo **SQLParamData** per segnalare che tutti i dati sono stati inviati per il parametro.  
  
    -   Se sono presenti più parametri data-at-execution, **SQLParamData** restituisce SQL_NEED_DATA e il valore definito dall'applicazione per il successivo parametro data-at-execution da elaborare. L'applicazione ripete i passaggi 4 e 5.  
  
    -   Se non sono presenti altri parametri data-at-execution, il processo è completo. Se l'istruzione è stata eseguita correttamente, **SQLParamData** restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO; Se l'esecuzione ha esito negativo, restituisce SQL_ERROR. A questo punto, **SQLParamData** può restituire qualsiasi SQLSTATE che può essere restituito dalla funzione utilizzata per eseguire l'istruzione (**SQLExecDirect** o **SQLExecute**).  
  
         I valori di output per tutti i parametri di input/output o di output sono disponibili nei \* buffer *ParameterValuePtr* e **StrLen_or_IndPtr* dopo che l'applicazione recupera tutti i set di risultati generati dall'istruzione.  
  
 La chiamata a **SQLExecute** o **SQLExecDirect** inserisce l'istruzione in uno stato SQL_NEED_DATA. A questo punto, l'applicazione può chiamare solo **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** o **SQLPutData** con l'istruzione o l' *handle di connessione* associato all'istruzione. Se viene chiamata qualsiasi altra funzione con l'istruzione o la connessione associata all'istruzione, la funzione restituisce SQLSTATE HY010 (errore della sequenza di funzioni). L'istruzione lascia lo stato SQL_NEED_DATA quando **SQLParamData** o **SQLPutData** restituisce un errore, **SQLParamData** restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO oppure l'istruzione viene annullata.  
  
 Se l'applicazione chiama **SQLCancel** mentre il driver necessita ancora di dati per i parametri data-at-execution, il driver Annulla l'esecuzione dell'istruzione. l'applicazione può quindi chiamare di nuovo **SQLExecute** o **SQLExecDirect** .  
  
## <a name="retrieving-streamed-output-parameters"></a>Recupero dei parametri di output trasmessi

 Quando un'applicazione imposta *InputOutputType* su SQL_PARAM_INPUT_OUTPUT_STREAM o SQL_PARAM_OUTPUT_STREAM, il valore del parametro di output deve essere recuperato da una o più chiamate a **SQLGetData**. Quando il driver ha un valore del parametro di output trasmesso per tornare all'applicazione, restituirà SQL_PARAM_DATA_AVAILABLE in risposta a una chiamata alle funzioni seguenti: **SQLMoreResults**, **SQLExecute** e **SQLExecDirect**. Un'applicazione chiama **SQLParamData** per determinare il valore del parametro disponibile.  
  
 Per ulteriori informazioni sui parametri di output SQL_PARAM_DATA_AVAILABLE e trasmessi, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-arrays-of-parameters"></a>Uso delle matrici di parametri

 Quando un'applicazione prepara un'istruzione con marcatori di parametro e passa una matrice di parametri, questa operazione può essere eseguita in due modi diversi. Un modo consiste nel fare in modo che il driver si basi sulle funzionalità di elaborazione dell'array del back-end, nel qual caso l'intera istruzione con la matrice di parametri viene considerata come un'unità atomica. Oracle è un esempio di un'origine dati che supporta le funzionalità di elaborazione di matrici. Un altro modo per implementare questa funzionalità è il driver per generare un batch di istruzioni SQL, un'istruzione SQL per ogni set di parametri nella matrice di parametri ed eseguire il batch. Non è possibile usare matrici di parametri con un'istruzione **Update where current of** .  
  
 Quando viene elaborata una matrice di parametri, i singoli set di risultati/conteggi di righe (uno per ogni set di parametri) possono essere disponibili o il rollup dei set di risultati o delle righe può essere eseguito in uno. L'opzione SQL_PARAM_ARRAY_ROW_COUNTS in **SQLGetInfo** indica se i conteggi delle righe sono disponibili per ogni set di parametri (SQL_PARC_BATCH) o se è disponibile un solo conteggio delle righe (SQL_PARC_NO_BATCH).  
  
 L'opzione SQL_PARAM_ARRAY_SELECTS in **SQLGetInfo** indica se un set di risultati è disponibile per ogni set di parametri (SQL_PAS_BATCH) o se è disponibile un solo set di risultati (SQL_PAS_NO_BATCH). Se il driver non consente l'esecuzione di un'istruzione di generazione del set di risultati con una matrice di parametri, SQL_PARAM_ARRAY_SELECTS restituisce SQL_PAS_NO_SELECT.  
  
 Per altre informazioni, vedere [funzione SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 Per supportare matrici di parametri, l'attributo dell'istruzione SQL_ATTR_PARAMSET_SIZE è impostato in modo da specificare il numero di valori per ogni parametro. Se il campo è maggiore di 1, i campi SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR di APD devono puntare a matrici. La cardinalità di ogni matrice è uguale al valore di SQL_ATTR_PARAMSET_SIZE.  
  
 Il campo SQL_DESC_ROWS_PROCESSED_PTR di APD punta a un buffer contenente il numero di set di parametri elaborati, inclusi i set di errori. Quando ogni set di parametri viene elaborato, il driver archivia un nuovo valore nel buffer. Se questo è un puntatore null, non verrà restituito alcun numero. Quando vengono usate matrici di parametri, il valore a cui fa riferimento il campo SQL_DESC_ROWS_PROCESSED_PTR di APD viene popolato anche se SQL_ERROR viene restituito dalla funzione Setting. Se viene restituito SQL_NEED_DATA, il valore a cui fa riferimento il SQL_DESC_ROWS_PROCESSED_PTR campo di APD viene impostato sul set di parametri in fase di elaborazione.  
  
 Cosa accade quando viene associata una matrice di parametri e un aggiornamento in cui viene eseguita l'istruzione **Current of** è definito dal driver.  
  
## <a name="column-wise-parameter-binding"></a>Associazione di parametri Column-Wise

 Nell'associazione a livello di colonna, l'applicazione associa le matrici separate di parametri e lunghezze/indicatori a ogni parametro.  
  
 Per utilizzare l'associazione per colonna, l'applicazione imposta innanzitutto l'attributo dell'istruzione SQL_ATTR_PARAM_BIND_TYPE su SQL_PARAM_BIND_BY_COLUMN. (Impostazione predefinita). Per ogni colonna da associare, l'applicazione esegue i passaggi seguenti:  
  
1.  Alloca una matrice di buffer di parametri.  
  
2.  Alloca una matrice di buffer di lunghezza/indicatore.  
  
    > [!NOTE]  
    >  Se l'applicazione scrive direttamente nei descrittori quando viene utilizzata l'associazione per colonna, è possibile utilizzare matrici separate per i dati di lunghezza e indicatore.  
  
3.  Chiama **SQLBindParameter** con gli argomenti seguenti:  
  
    -   *ValueType* è il tipo C di un singolo elemento nella matrice del buffer dei parametri.  
  
    -   *ParameterType* è il tipo SQL del parametro.  
  
    -   *ParameterValuePtr* è l'indirizzo della matrice di buffer dei parametri.  
  
    -   *BufferLength* è la dimensione di un singolo elemento nella matrice del buffer dei parametri. L'argomento *bufferLength* viene ignorato quando i dati sono dati a lunghezza fissa.  
  
    -   *StrLen_or_IndPtr* è l'indirizzo della matrice di lunghezza/indicatore.  
  
 Per ulteriori informazioni sull'utilizzo di queste informazioni, vedere "argomento ParameterValuePtr" in "Commenti", più avanti in questa sezione. Per ulteriori informazioni sull'associazione a livello di colonna dei parametri, vedere [binding di matrici di parametri](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="row-wise-parameter-binding"></a>Associazione di parametri Row-Wise

 Nell'associazione per riga, l'applicazione definisce una struttura che contiene i parametri e i buffer di lunghezza/indicatore per ogni parametro da associare.  
  
 Per utilizzare l'associazione per riga, l'applicazione esegue i passaggi seguenti:  
  
1.  Definisce una struttura per includere un singolo set di parametri (inclusi i buffer dei parametri e di lunghezza/indicatore) e alloca una matrice di queste strutture.  
  
    > [!NOTE]  
    >  Se l'applicazione scrive direttamente nei descrittori quando viene utilizzata l'associazione per riga, è possibile utilizzare campi separati per i dati di lunghezza e indicatore.  
  
2.  Imposta l'attributo dell'istruzione SQL_ATTR_PARAM_BIND_TYPE sulla dimensione della struttura che contiene un singolo set di parametri o sulle dimensioni di un'istanza di un buffer in cui verranno associati i parametri. La lunghezza deve includere lo spazio per tutti i parametri associati, nonché qualsiasi riempimento della struttura o del buffer, per assicurarsi che quando l'indirizzo di un parametro associato venga incrementato con la lunghezza specificata, il risultato punterà all'inizio dello stesso parametro nella riga successiva. Quando si usa l'operatore *sizeof* in ANSI C, questo comportamento è garantito.  
  
3.  Chiama **SQLBindParameter** con gli argomenti seguenti per ogni parametro da associare:  
  
    -   *ValueType* è il tipo del membro del buffer del parametro da associare alla colonna.  
  
    -   *ParameterType* è il tipo SQL del parametro.  
  
    -   *ParameterValuePtr* è l'indirizzo del membro buffer del parametro nel primo elemento della matrice.  
  
    -   *BufferLength* è la dimensione del membro buffer del parametro.  
  
    -   *StrLen_or_IndPtr* è l'indirizzo del membro di lunghezza/indicatore da associare.  
  
 Per ulteriori informazioni sull'utilizzo di queste informazioni, vedere "argomento *ParameterValuePtr* ", più avanti in questa sezione. Per ulteriori informazioni sull'associazione di parametri per riga, vedere le [matrici di associazione dei parametri](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="error-information"></a>Informazioni sull'errore

 Se un driver non implementa matrici di parametri come batch (l'opzione SQL_PARAM_ARRAY_ROW_COUNTS è uguale a SQL_PARC_NO_BATCH), le situazioni di errore vengono gestite come se fosse stata eseguita un'istruzione. Se il driver implementa matrici di parametri come batch, un'applicazione può utilizzare il SQL_DESC_ARRAY_STATUS_PTR campo di intestazione di dpi per determinare quale parametro di un'istruzione SQL o quale parametro in una matrice di parametri ha causato la restituzione di un errore **SQLExecDirect** o **SQLExecute** . Questo campo contiene informazioni sullo stato per ogni riga di valori di parametro. Se il campo indica che si è verificato un errore, i campi nella struttura dei dati di diagnostica indicheranno la riga e il numero di parametro del parametro che ha avuto esito negativo. Il numero di elementi nella matrice verrà definito dal campo di intestazione SQL_DESC_ARRAY_SIZE in APD, che può essere impostato dall'attributo SQL_ATTR_PARAMSET_SIZE istruzione.  
  
> [!NOTE]  
>  Il SQL_DESC_ARRAY_STATUS_PTR campo di intestazione nell'APD viene usato per ignorare i parametri. Per ulteriori informazioni su come ignorare i parametri, vedere la sezione successiva "ignorare un set di parametri".  
  
 Quando **SQLExecute** o **SQLExecDirect** restituisce SQL_ERROR, gli elementi nella matrice a cui punta il campo SQL_DESC_ARRAY_STATUS_PTR nel elemento dpi conterranno SQL_PARAM_ERROR, SQL_PARAM_SUCCESS, SQL_PARAM_SUCCESS_WITH_INFO, SQL_PARAM_UNUSED o SQL_PARAM_DIAG_UNAVAILABLE.  
  
 Per ogni elemento di questa matrice, la struttura dei dati di diagnostica contiene uno o più record di stato. Il campo SQL_DIAG_ROW_NUMBER della struttura indica il numero di riga dei valori di parametro che hanno causato l'errore. Se è possibile determinare il parametro specifico in una riga di parametri che ha causato l'errore, il numero di parametro verrà immesso nel campo SQL_DIAG_COLUMN_NUMBER.  
  
 SQL_PARAM_UNUSED viene immesso quando un parametro non è stato utilizzato perché si è verificato un errore in un parametro precedente che ha forzato l'interruzione di **SQLExecute** o **SQLExecDirect** . Se ad esempio sono presenti 50 parametri e si è verificato un errore durante l'esecuzione del quarantesimo set di parametri che hanno causato l'interruzione di **SQLExecute** o **SQLExecDirect** , SQL_PARAM_UNUSED viene immesso nella matrice di stato per i parametri da 41 a 50.  
  
 SQL_PARAM_DIAG_UNAVAILABLE viene immesso quando il driver considera le matrici di parametri come unità monolitica, quindi non genera questo singolo livello di parametri delle informazioni sugli errori.  
  
 Alcuni errori nell'elaborazione di un singolo set di parametri provocano l'arresto dell'elaborazione dei set di parametri successivi nella matrice. Altri errori non influiscono sull'elaborazione dei parametri successivi. Gli errori che interrompono l'elaborazione sono definiti dal driver. Se l'elaborazione non viene arrestata, vengono elaborati tutti i parametri nella matrice, SQL_SUCCESS_WITH_INFO viene restituito come risultato dell'errore e il buffer definito da SQL_ATTR_PARAMS_PROCESSED_PTR viene impostato sul numero totale di set di parametri elaborati (come definito dall'attributo SQL_ATTR_PARAMSET_SIZE Statement), che include set di errori.  
  
> [!CAUTION]  
>  Il comportamento di ODBC quando si verifica un errore nell'elaborazione di una matrice di parametri è diverso in ODBC 3. *x* rispetto a ODBC 2. *x*. In ODBC 2. *x*, la funzione ha restituito SQL_ERROR e l'elaborazione è terminata. Il buffer a cui punta l'argomento *Pirow* di **SQLParamOptions** conteneva il numero della riga di errore. In ODBC 3. *x*, la funzione restituisce SQL_SUCCESS_WITH_INFO e l'elaborazione può arrestarsi o continuare. Se continua, il buffer specificato da SQL_ATTR_PARAMS_PROCESSED_PTR verrà impostato sul valore di tutti i parametri elaborati, inclusi quelli che hanno generato un errore. Questa modifica del comportamento può causare problemi per le applicazioni esistenti.  
  
 Quando **SQLExecute** o **SQLExecDirect** viene restituito prima di completare l'elaborazione di tutti i set di parametri in una matrice di parametri, ad esempio quando viene restituito SQL_ERROR o SQL_NEED_DATA, la matrice di stato contiene gli Stati per i parametri già elaborati. La posizione a cui punta il campo SQL_DESC_ROWS_PROCESSED_PTR nel file dpi contiene il numero di riga nella matrice di parametri che ha causato il codice di errore SQL_ERROR o SQL_NEED_DATA. Quando viene inviata una matrice di parametri a un'istruzione SELECT, la disponibilità dei valori della matrice di stato è definita dal driver; potrebbero essere disponibili dopo l'esecuzione dell'istruzione o il recupero dei set di risultati.  
  
## <a name="ignoring-a-set-of-parameters"></a>Ignorare un set di parametri

 Il SQL_DESC_ARRAY_STATUS_PTR campo di APD (impostato dall'attributo SQL_ATTR_PARAM_STATUS_PTR istruzione) può essere usato per indicare che un set di parametri associati in un'istruzione SQL deve essere ignorato. Per indicare al driver di ignorare uno o più set di parametri durante l'esecuzione, un'applicazione deve attenersi alla procedura seguente:  
  
1.  Chiamare **SQLSetDescField** per impostare il campo di intestazione SQL_DESC_ARRAY_STATUS_PTR di APD in modo che punti a una matrice di valori SQLUSMALLINT per contenere informazioni sullo stato. Questo campo può essere impostato anche chiamando **SQLSetStmtAttr** con un *attributo* di SQL_ATTR_PARAM_OPERATION_PTR, che consente a un'applicazione di impostare il campo senza ottenere un handle del descrittore.  
  
2.  Impostare ogni elemento della matrice definito dal campo SQL_DESC_ARRAY_STATUS_PTR di APD su uno dei due valori seguenti:  
  
    -   SQL_PARAM_IGNORE, per indicare che la riga è esclusa dall'esecuzione dell'istruzione.  
  
    -   SQL_PARAM_PROCEED, per indicare che la riga è inclusa nell'esecuzione dell'istruzione.  
  
3.  Chiamare **SQLExecDirect** o **SQLExecute** per eseguire l'istruzione preparata.  
  
 Le regole seguenti si applicano alla matrice definita dal campo SQL_DESC_ARRAY_STATUS_PTR di APD:  
  
-   Per impostazione predefinita, il puntatore è impostato su null.  
  
-   Se il puntatore è null, vengono utilizzati tutti i set di parametri, come se tutti gli elementi fossero impostati su SQL_ROW_PROCEED.  
  
-   L'impostazione di un elemento su SQL_PARAM_PROCEED non garantisce che l'operazione utilizzerà quel particolare set di parametri.  
  
-   SQL_PARAM_PROCEED è definito come 0 nel file di intestazione.  
  
 Un'applicazione può impostare il campo SQL_DESC_ARRAY_STATUS_PTR nell'APD in modo che punti alla stessa matrice a cui punta il campo SQL_DESC_ARRAY_STATUS_PTR in IRD. Questa operazione è utile quando si associano parametri ai dati delle righe. I parametri possono quindi essere ignorati in base allo stato dei dati della riga. Oltre ai SQL_PARAM_IGNORE, i codici seguenti causano l'ignoranza di un parametro in un'istruzione SQL: SQL_ROW_DELETED, SQL_ROW_UPDATED e SQL_ROW_ERROR. Oltre ai SQL_PARAM_PROCEED, i codici seguenti causano il proseguimento di un'istruzione SQL: SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO e SQL_ROW_ADDED.  
  
## <a name="rebinding-parameters"></a>Riassociazione di parametri

 Per modificare un'associazione, un'applicazione può eseguire una delle due operazioni seguenti:  
  
-   Chiamare **SQLBindParameter** per specificare una nuova associazione per una colonna già associata. Il driver sovrascrive il vecchio binding con quello nuovo.  
  
-   Specificare un offset da aggiungere all'indirizzo del buffer specificato dalla chiamata di binding a **SQLBindParameter**. Per ulteriori informazioni, vedere la sezione successiva "riassociazione con offset".  
  
## <a name="rebinding-with-offsets"></a>Riassociazione con offset

 Il riassociazione dei parametri è particolarmente utile quando un'applicazione dispone di una configurazione dell'area del buffer che può contenere molti parametri, ma una chiamata a **SQLExecDirect** o **SQLExecute** usa solo pochi parametri. Lo spazio rimanente nell'area del buffer può essere utilizzato per il set di parametri successivo modificando il binding esistente con un offset.  
  
 Il SQL_DESC_BIND_OFFSET_PTR campo di intestazione nell'oggetto APD punta all'offset dell'associazione. Se il campo è diverso da null, il driver dereferenzia il puntatore e, se nessuno dei valori nei campi SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR è un puntatore null, aggiunge il valore dereferenziato ai campi nei record del descrittore in fase di esecuzione. I nuovi valori dei puntatori vengono utilizzati quando vengono eseguite le istruzioni SQL. L'offset rimane valido dopo il riassociazione. Poiché SQL_DESC_BIND_OFFSET_PTR è un puntatore all'offset anziché all'offset stesso, un'applicazione può modificare l'offset direttamente, senza dover chiamare [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) o [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md) per modificare il campo del descrittore. Per impostazione predefinita, il puntatore è impostato su null. Il campo SQL_DESC_BIND_OFFSET_PTR di ARD può essere impostato da una chiamata a [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) o da una chiamata a [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)con *fAttribute* di SQL_ATTR_PARAM_BIND_OFFSET_PTR.  
  
 L'offset dell'associazione viene sempre aggiunto direttamente ai valori nei campi SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR. Se l'offset viene impostato su un valore diverso, il nuovo valore viene ancora aggiunto direttamente al valore in ogni campo del descrittore. Il nuovo offset non viene aggiunto alla somma del valore del campo e di eventuali offset precedenti.  
  
## <a name="descriptors"></a>Descrittori

 Il modo in cui un parametro è associato è determinato dai campi di APD e IPD. Gli argomenti in **SQLBindParameter** vengono usati per impostare i campi del descrittore. I campi possono essere impostati anche dalle funzioni **SQLSetDescField** , anche se **SQLBindParameter** è più efficiente da usare perché l'applicazione non deve ottenere un handle descrittore per chiamare **SQLBindParameter**.  
  
> [!CAUTION]  
>  La chiamata di **SQLBindParameter** per un'istruzione può influire su altre istruzioni. Questo errore si verifica quando il ARD associato all'istruzione viene allocato in modo esplicito ed è associato anche ad altre istruzioni. Poiché **SQLBindParameter** modifica i campi di APD, le modifiche si applicano a tutte le istruzioni a cui è associato questo descrittore. Se questo non è il comportamento necessario, l'applicazione deve annullare l'associazione di questo descrittore dalle altre istruzioni prima di chiamare **SQLBindParameter**.  
  
 A livello concettuale, **SQLBindParameter** esegue i passaggi seguenti in sequenza:  
  
1.  Chiama [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) per ottenere l'handle APD.  
  
2.  Chiama [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) per ottenere il campo SQL_DESC_COUNT di APD e, se il valore dell'argomento *ColumnNumber* supera il valore di SQL_DESC_COUNT, chiama **SQLSetDescField** per aumentare il valore di SQL_DESC_COUNT a *ColumnNumber*.  
  
3.  Chiama [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) più volte per assegnare valori ai campi seguenti di APD:  
  
    -   Imposta SQL_DESC_TYPE e SQL_DESC_CONCISE_TYPE sul valore di *ValueType*, ad eccezione del fatto che se *ValueType* è uno degli identificatori concisi di un sottotipo DateTime o Interval, imposta SQL_DESC_TYPE su SQL_DATETIME o SQL_INTERVAL, rispettivamente, imposta SQL_DESC_CONCISE_TYPE sull'identificatore conciso e imposta SQL_DESC_DATETIME_INTERVAL_CODE sul sottocodice DateTime o Interval corrispondente.  
  
    -   Imposta il campo SQL_DESC_OCTET_LENGTH sul valore di *bufferLength*.  
  
    -   Imposta il campo SQL_DESC_DATA_PTR sul valore di *ParameterValue*.  
  
    -   Imposta il campo SQL_DESC_OCTET_LENGTH_PTR sul valore di *StrLen_Or_Ind*.  
  
    -   Imposta il campo SQL_DESC_INDICATOR_PTR anche sul valore di *StrLen_Or_Ind*.  
  
     Il parametro *StrLen_Or_Ind* specifica le informazioni sull'indicatore e la lunghezza del valore del parametro.  
  
4.  Chiama [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) per ottenere l'handle DIP.  
  
5.  Chiama [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) per ottenere il campo SQL_DESC_COUNT di dpi e, se il valore dell'argomento *ColumnNumber* supera il valore di SQL_DESC_COUNT, chiama **SQLSetDescField** per aumentare il valore di SQL_DESC_COUNT a *ColumnNumber*.  
  
6.  Chiama [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) più volte per assegnare valori ai campi seguenti del valore dpi:  
  
    -   Imposta SQL_DESC_TYPE e SQL_DESC_CONCISE_TYPE sul valore di *ParameterType*, ad eccezione del fatto che se *ParameterType* è uno degli identificatori concisi di un sottotipo DateTime o Interval, imposta SQL_DESC_TYPE su SQL_DATETIME o SQL_INTERVAL, rispettivamente, imposta SQL_DESC_CONCISE_TYPE sull'identificatore conciso e imposta SQL_DESC_DATETIME_INTERVAL_CODE sul sottocodice DateTime o Interval corrispondente.  
  
    -   Imposta uno o più SQL_DESC_LENGTH, SQL_DESC_PRECISION e SQL_DESC_DATETIME_INTERVAL_PRECISION, in base alle esigenze di *ParameterType*.  
  
    -   Imposta SQL_DESC_SCALE sul valore di *DecimalDigits*.  
  
 Se la chiamata a **SQLBindParameter** ha esito negativo, il contenuto dei campi di descrizione impostati in APD non è definito e il campo SQL_DESC_COUNT di APD è invariato. Inoltre, i campi SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE e SQL_DESC_TYPE del record appropriato in dpi sono indefiniti e il campo SQL_DESC_COUNT del dip è invariato.  
  
## <a name="conversion-of-calls-to-and-from-sqlsetparam"></a>Conversione di chiamate da e verso SQLSetParam

 Quando un'applicazione ODBC 1,0 chiama **SQLSetParam** in un ODBC 3. driver *x* , ODBC 3. *x* Gestione driver esegue il mapping della chiamata, come illustrato nella tabella seguente.  
  
|Chiamata dall'applicazione ODBC 1,0|Chiamata a ODBC 3. driver *x*|  
|----------------------------------|-------------------------------|  
|SQLSetParam (StatementHandle, ParameterNumber, ValueType, ParameterType, LengthPrecision, ParameterScale, ParameterValuePtr, StrLen_or_IndPtr);|SQLBindParameter (StatementHandle, ParameterNumber, SQL_PARAM_INPUT_OUTPUT, ValueType, ParameterType,      *ColumnSize*,      *DecimalDigits*, ParameterValuePtr, SQL_SETPARAM_VALUE_MAX, StrLen_or_IndPtr);|  
  
## <a name="examples"></a>Esempi  

### <a name="a-use-sqlbindparameter-function"></a>R. Usa funzione SQLBindParameter

 Nell'esempio seguente un'applicazione prepara un'istruzione SQL per inserire i dati nella tabella ORDERs. Per ogni parametro nell'istruzione, l'applicazione chiama **SQLBindParameter** per specificare il tipo di dati C ODBC e il tipo di dati SQL del parametro e per associare un buffer a ogni parametro. Per ogni riga di dati, l'applicazione assegna i valori dei dati a ogni parametro e chiama **SQLExecute** per eseguire l'istruzione.  
  
 Nell'esempio seguente si presuppone l'esistenza di un'origine dati ODBC nel computer denominata Northwind associata al database Northwind.  
  
 Per altri esempi di codice, vedere funzione [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [funzione SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md), [funzione SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)e [funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp
// SQLBindParameter_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define EMPLOYEE_ID_LEN 10  
  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLSMALLINT sCustID;  
  
SQLCHAR szEmployeeID[EMPLOYEE_ID_LEN];  
SQL_DATE_STRUCT dsOrderDate;  
SQLINTEGER cbCustID = 0, cbOrderDate = 0, cbEmployeeID = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, EMPLOYEE_ID_LEN, 0, szEmployeeID, 0, &cbEmployeeID);  
   retcode = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_SSHORT, SQL_INTEGER, 0, 0, &sCustID, 0, &cbCustID);  
   retcode = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TIMESTAMP, sizeof(dsOrderDate), 0, &dsOrderDate, 0, &cbOrderDate);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"INSERT INTO Orders(CustomerID, EmployeeID, OrderDate) VALUES (?, ?, ?)", SQL_NTS);  
  
   strcpy_s((char*)szEmployeeID, _countof(szEmployeeID), "BERGS");  
   sCustID = 5;  
   dsOrderDate.year = 2006;  
   dsOrderDate.month = 3;  
   dsOrderDate.day = 17;  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
### <a name="b-execute-a-stored-procedure-using-a-named-parameter"></a>B. Eseguire una stored procedure usando un parametro denominato

 Nell'esempio seguente un'applicazione esegue un SQL Server stored procedure utilizzando un parametro denominato.  
  
```cpp
// SQLBindParameter_Function_2.cpp  
// compile with: ODBC32.lib  
// sample assumes the following stored procedure:  
// use northwind  
// DROP PROCEDURE SQLBindParameter  
// GO  
//   
// CREATE PROCEDURE SQLBindParameter @quote int  
// AS  
// delete from orders where OrderID >= @quote  
// GO  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
SQLHDESC hIpd = NULL;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLCHAR szQuote[50] = "100084";  
SQLINTEGER cbValue = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"{call SQLBindParameter(?)}", SQL_NTS);  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 50, 0, szQuote, 0, &cbValue);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_IMP_PARAM_DESC, &hIpd, 0, 0);  
   retcode = SQLSetDescField(hIpd, 1, SQL_DESC_NAME, "@quote", SQL_NTS);  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione di informazioni su un parametro in un'istruzione|[Funzione SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Rilascio di buffer di parametri nell'istruzione|[SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Restituzione del numero di parametri di istruzione|[Funzione SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|  
|Restituzione del parametro successivo per l'invio dei dati|[Funzione SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Specifica di più valori di parametro|[Funzione SQLParamOptions](../../../odbc/reference/syntax/sqlparamoptions-function.md)|  
|Invio di dati dei parametri in fase di esecuzione|[SQLPutData Function](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Vedere anche

 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
