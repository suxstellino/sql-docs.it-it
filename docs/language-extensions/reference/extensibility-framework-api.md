---
title: API del framework di estendibilità
titleSuffix: SQL Server Language Extensions
description: È possibile usare il framework di estendibilità per scrivere estensioni dei linguaggi di programmazione per SQL Server. L'API del framework di estendibilità per Microsoft SQL Server è un'API che può essere usata dall'estensione di un linguaggio per interagire e scambiare dati con SQL Server.
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 21099123d8065fad63e1526a3377f1904aed87b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016527"
---
# <a name="extensibility-framework-api-for-sql-server"></a>API del framework di estendibilità per SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

È possibile usare il framework di estendibilità per scrivere estensioni dei linguaggi di programmazione per SQL Server. L'API del framework di estendibilità per Microsoft SQL Server è un'API che può essere usata dall'estensione di un linguaggio per interagire e scambiare dati con SQL Server.

Gli autori di estensioni di linguaggio possono usare questo documento di riferimento con le estensioni di linguaggio open source per capire come usare l'API per scrivere estensioni personalizzate. Il codice sorgente per le estensioni di linguaggio è disponibile all'indirizzo [aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions).

Nelle sezioni seguenti è possibile trovare informazioni a livello di sintassi e argomenti per tutte le funzioni API.

## <a name="return-value"></a>Valore restituito

Tutte le funzioni restituiscono un parametro *SQLRETURN*. Se il valore è diverso da *SQL_SUCCESS*, viene considerato come un errore e l'esecuzione dello script si interrompe.

## <a name="standard-output"></a>Output standard

Eventuali output dell'estensione all'output standard o flussi di errore verranno tracciati nel file di log della sessione e ricondotti a SQL Server (analogamente agli elementi visualizzati nella scheda dei messaggi SSMS).


## <a name="init"></a>Init

Questa funzione viene chiamata una sola volta ed è usata per inizializzare il runtime per l'esecuzione. 

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>Argomenti

*ExtensionParams*  
\[Input\] Stringa con terminazione Null contenente il valore `PARAMETERS` fornito durante la fase [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) o [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md).

*ExtensionParamsLength*  
\[Input\] Lunghezza, espressa in byte, di *ExtensionParams* (escluso il carattere con terminazione Null).

*ExtensionPath*  
\[Input\] Stringa UTF-8 con terminazione Null contenente il percorso assoluto della directory di installazione dell'estensione.

*ExtensionPathLength*  
\[Input\] Lunghezza, espressa in byte, di *ExtensionPath* (escluso il carattere con terminazione Null).

*PublicLibraryPath*  
\[Input\] Stringa UTF-8 con terminazione Null contenente il percorso assoluto della directory delle librerie esterne pubbliche per questo linguaggio esterno.

*PublicLibraryPathLength*  
\[Input\] Lunghezza, espressa in byte, di *PublicLibraryPath* (escluso il carattere con terminazione Null).

*PrivateLibraryPath*  
\[Input\] Stringa UTF-8 con terminazione Null contenente il percorso assoluto della directory delle librerie esterne private per questo utente e questo linguaggio esterno.

*PrivateLibraryPathLength*  
\[Input\] Lunghezza, espressa in byte, di *PrivateLibraryPath* (escluso il carattere con terminazione Null).

## <a name="initsession"></a>InitSession

Questa funzione viene chiamata una volta per sessione e inizializza le impostazioni specifiche della sessione.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*Script*  
\[Input\] Stringa UTF-8 con terminazione Null contenente l'oggetto `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ScriptLength*  
\[Input\] Lunghezza, espressa in byte, di *ScriptScript* (escluso il carattere con terminazione Null).

*InputSchemaColumnsNumber*  
\[Input\] Numero di colonne nel set di risultati di `@input_data_1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ParametersNumber*  
\[Input\] Numero di parametri di input di `@params` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataName*  
\[Input\] Stringa UTF-8 con terminazione Null contenente l'oggetto `@input_data_1_name` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataNameLength*  
\[Input\] Lunghezza, espressa in byte, di *InputDataName* (escluso il carattere con terminazione Null).

*OutputDataName*  
\[Input\] Stringa UTF-8 con terminazione Null contenente l'oggetto `@output_data_1_name` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*OutputDataNameLength*  
\[Input\] Lunghezza, espressa in byte, di *OutputDataName* (escluso il carattere con terminazione Null).

## <a name="initcolumn"></a>InitColumn

Consente di inizializzare le informazioni relative a una determinata colonna in una sessione specifica.

Questa funzione viene chiamata per ogni colonna del set di risultati di `@input_data_1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Con *schema di input* si indicherà la struttura di colonna di questo set di risultati.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*ColumnNumber*  
\[Input\] Numero intero che identifica l'indice di questa colonna nello schema di input. Le colonne sono numerate in ordine sequenziale crescente a partire da 0.

*ColumnName*  
\[Input\] Stringa UTF-8 con terminazione Null contenente il nome della colonna.

*ColumnNameLength*  
\[Input\] Lunghezza, espressa in byte, di *ColumnName* (escluso il carattere con terminazione Null).

*DataType*  
\[Input\] Tipo ODBC C che identifica il tipo di dati della colonna.

*ColumnSize*  
\[Input\] Dimensione massima, espressa in byte, dei dati sottostanti in questa colonna.

Per i tipi di dati SQL_C_CHAR, SQL_C_WCHAR e SQL_C_BINARY, i valori superiori a 8000 indicano che questa colonna rappresenta un oggetto LOB con dimensioni superiori a 2 GB.

*DecimalDigits*  
\[Input\] Cifre decimali dei dati sottostanti in questa colonna, in base a quanto definito da [Cifre decimali](../../odbc/reference/appendixes/decimal-digits.md).

*Ammette i valori Null*  
\[Input\] Valore che indica se questa colonna può contenere valori NULL. Valori possibili:

- SQL_NO_NULLS: La colonna non può contenere valori NULL.
- SQL_NULLABLE: La colonna può contenere valori NULL.

*PartitionByNumber*  
\[Input\] Valore che indica l'indice di questa colonna nella sequenza `@input_data_1_partition_by_columns` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Le colonne sono numerate in ordine sequenziale crescente a partire da 0. Se questa colonna non è inclusa nella sequenza, il valore è -1.

*OrderByNumber*  
\[Input\] Valore che indica l'indice di questa colonna nella sequenza `@input_data_1_order_by_columns` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Le colonne sono numerate in ordine sequenziale crescente a partire da 0. Se questa colonna non è inclusa nella sequenza, il valore è -1.

## <a name="initparam"></a>InitParam

Consente di inizializzare le informazioni relative a un determinato parametro di input in una sessione specifica.

Questa funzione viene chiamata per ogni parametro di `@params` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*ParamNumber*  
\[Input\] Numero intero che identifica l'indice di questo parametro. I parametri sono numerati in ordine sequenziale crescente a partire da 0.

*ParamName*  
\[Input\] Stringa UTF-8 con terminazione Null contenente il nome del parametro.

*ParamNameLength*  
\[Input\] Lunghezza, espressa in byte, di *ParamName* (escluso il carattere con terminazione Null).

*DataType*  
\[Input\] Tipo ODBC C che identifica il tipo di dati del parametro.

*ParamSize*  
\[Input\] Dimensione massima, espressa in byte, dei dati sottostanti in questo parametro.

Per i tipi di dati SQL_C_CHAR, SQL_C_WCHAR e SQL_C_BINARY, i valori superiori a 8000 indicano che questo parametro rappresenta un oggetto LOB con dimensioni superiori a 2 GB.

*DecimalDigits*  
\[Input\] Cifre decimali dei dati sottostanti in questo parametro, in base a quanto definito da [Cifre decimali](../../odbc/reference/appendixes/decimal-digits.md).

*ParamValue*  
\[Input\] Puntatore a un buffer contenente il valore del parametro.

*StrLen_or_Ind*  
\[Input\] Valore intero che indica la lunghezza in byte di *ParamValue*, oppure SQL_NULL_DATA, per indicare che i dati sono NULL.

È possibile ignorare StrLen_or_Ind\[col\] se una colonna non ammette valori Null e non rappresenta uno dei tipi di dati seguenti: SQL_C_CHAR, SQL_C_WCHAR e SQL_C_BINARY, SQL_C_NUMERIC o SQL_C_TYPE_TIMESTAMP. In caso contrario, punta a una matrice valida con elementi \[RowsNumber\], in cui ogni elemento contiene dati dell'indicatore di lunghezza o Null.

*InputOutputType*  
\[Input\] Tipo di parametro. L'argomento *InputOutputType* è rappresentato da uno dei valori seguenti:

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>Execute

Consente di eseguire `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Questa funzione può essere chiamata più volte, una per ogni blocco di flusso e per ogni partizione nell'oggetto `@input_data_1_partition_by_columns` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*RowsNumber*  
\[Input\] Numero di righe in *Data*.

*Dati*  
\[Input\] Matrice bidimensionale contenente il set di risultati di `@input_data_1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Il numero totale di colonne è indicato dall'oggetto *InputSchemaColumnsNumber* ricevuto nella chiamata [InitSession](#initsession). Ogni colonna contiene elementi *RowsNumber* che devono essere interpretati in base al tipo di colonna restituito da [InitColumn](#initcolumn).

Gli elementi indicati come NULL in *StrLen_or_Ind* potrebbero non essere validi e devono essere ignorati.

*StrLen_or_Ind*  
\[Input\] Matrice bidimensionale contenente l'indicatore di lunghezza/NULL per ogni valore di *Data*. Possibili valori di ogni cella:

- n, dive n > 0. Indica la lunghezza dei dati in byte.
- SQL_NULL_DATA, che indica un valore NULL.

Il numero totale di colonne è indicato dall'oggetto *InputSchemaColumnsNumber* ricevuto nella chiamata [InitSession](#initsession). Ogni colonna contiene elementi *RowsNumber* che devono essere interpretati in base al tipo di colonna restituito da [InitColumn](#initcolumn).

È possibile ignorare StrLen_or_Ind\[col\] se una colonna non ammette valori Null e non rappresenta uno dei tipi di dati seguenti: SQL_C_CHAR, SQL_C_WCHAR e SQL_C_BINARY, SQL_C_NUMERIC o SQL_C_TYPE_TIMESTAMP. In caso contrario, punta a una matrice valida con elementi *RowsNumber*, in cui ogni elemento contiene dati dell'indicatore di lunghezza o Null.

*OutputSchemaColumnsNumber*  
\[Output\] Puntatore a un buffer in cui viene restituito il numero di colonne nel set di risultati previsto di `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="getresultcolumn"></a>GetResultColumn

Consente di recuperare le informazioni relative a una determinata colonna di output in una sessione specifica.

Questa funzione viene chiamata per ogni colonna del set di risultati di `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
Con "schema di output" si indicherà la struttura di colonna di questo set di risultati.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*ColumnNumber*  
\[Input\] Numero intero che identifica l'indice di questa colonna nello schema di output. Le colonne sono numerate in ordine sequenziale crescente a partire da 0.

*DataType*  
\[Output\] Puntatore al buffer contenente il tipo ODBC C che identifica il tipo di dati della colonna.

*ColumnSize*  
\[Output\] Puntatore a un buffer contenente la dimensione massima, espressa in byte, dei dati sottostanti in questa colonna.

*DecimalDigits*  
\[Output\] Puntatore a un buffer contenente le cifre decimali dei dati sottostanti in questa colonna, in base a quanto definito da [Cifre decimali](../../odbc/reference/appendixes/decimal-digits.md). Se il numero di cifre decimali non può essere stabilito o non è applicabile, il valore viene ignorato.

*Ammette i valori Null*  
\[Output\] Puntatore a un buffer contenente un valore che indica se la colonna può contenere valori NULL. Valori possibili:

- SQL_NO_NULLS: La colonna non può contenere valori NULL.
- SQL_NULLABLE: La colonna può contenere valori NULL.

Se vengono passati altri valori, l'esecuzione viene interrotta.

## <a name="getresults"></a>GetResults

Consente di recuperare il set di risultati dell'esecuzione di `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Questa funzione può essere chiamata più volte, una per ogni blocco di flusso e per ogni partizione nell'oggetto `@input_data_1_partition_by_columns` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER***   StrLen_or_Ind
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*RowsNumber*  
\[Output\] Puntatore a un buffer contenente il numero di righe in *Data*.

*Dati*  
\[Output\] Puntatore a una matrice bidimensionale allocata dall'estensione, contenente il set di risultati di `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Il numero totale di colonne deve essere *OutputSchemaColumnsNumber*, valore recuperato nella chiamata [Execute](#execute). Ogni colonna deve contenere elementi *RowsNumber* che devono essere interpretati in base al tipo di colonna restituito da [GetResultColumn](#getresultcolumn).

*StrLen_or_Ind*  
\[Output\] Puntatore a una matrice bidimensionale allocata dall'estensione, contenente l'indicatore di lunghezza/NULL per ogni valore in *Data*. Possibili valori di ogni cella:

- n, dive n > 0. Indica la lunghezza dei dati in byte.
- SQL_NULL_DATA, che indica un valore NULL.

Il numero totale di colonne deve essere *OutputSchemaColumnsNumber*, valore ricevuto nella chiamata [Execute](#execute). Ogni colonna contiene elementi *RowsNumber* che devono essere interpretati in base al tipo di colonna restituito da [GetResultColumn](#getresultcolumn).

È possibile ignorare StrLen_or_Ind\[col\] se una colonna non ammette valori Null e non rappresenta uno dei tipi di dati seguenti: SQL_C_CHAR, SQL_C_WCHAR e SQL_C_BINARY [aggiungere date]. In caso contrario, punta a una matrice valida con elementi *RowsNumber*, in cui ogni elemento contiene dati dell'indicatore di lunghezza o Null.

## <a name="getoutputparam"></a>GetOutputParam

Consente di recuperare le informazioni relative a un determinato parametro di output in una sessione specifica.

Questa funzione viene chiamata per ogni parametro di `@params` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) contrassegnato con OUTPUT.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*ParamValue*  
\[Output\] Puntatore a un buffer contenente il valore del parametro.

*StrLen_or_Ind* \[Output\] Puntatore a un buffer contenente un valore intero che indica la lunghezza in byte di *ParamValue*, oppure SQL_NULL_DATA, per indicare che i dati sono NULL.

## <a name="getinterfaceversion"></a>GetInterfaceVersion

Consente di recuperare la versione dell'interfaccia.
Questa funzione restituisce un numero intero che rappresenta la versione dell'interfaccia dell'estensione. Valori supportati:
1. La versione 1 corrisponde alla versione iniziale dell'API, supportata in SQL Server 2019 RTM.
1. Nella versione 2 è stato aggiunto il supporto per le API InstallExternalLibrary e UninstallExternalLibrary e il supporto di SQL Server 2019 CU3.                            

### <a name="syntax"></a>Sintassi

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

Consente di pulire le informazioni relative a ogni sessione.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

## <a name="cleanup"></a>Pulizia

Consente di pulire informazioni condivise a livello globale (ad esempio, JVM).

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

Consente di recuperare dati di telemetria (coppie chiave-valore) dall'estensione. Questa funzione è facoltativa e non deve essere necessariamente implementata. I dati di telemetria vengono esposti dalla vista a gestione dinamica (DMV) `dm_db_external_script_execution_stats`.

È presente un contatore denominato script_executions, inviato dal framework. L'estensione non deve tuttavia usare questo nome.

Ogni voce di telemetria è una coppia chiave-valore, in cui le chiavi sono stringhe e i valori sono valori interi a 64 bit (contatori). L'output è costituito quindi da due matrici logiche: i nomi e i contatori corrispondenti. Ogni matrice è un output.

La lunghezza di ogni matrice è indicata da *RowsNumber*, che costituisce un output. Il primo output logico contiene puntatori a stringhe ed è quindi rappresentato da due matrici: *CounterNames* (i dati stringa effettivi) e *CounterNamesLength* (la lunghezza di ogni stringa). Il secondo output logico viene archiviato nel puntatore *CounterValues*.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>Argomenti

*SessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*TaskId*  
\[Input\] Numero intero che identifica in modo univoco questo processo di esecuzione.

Se `@parallel = 1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), questo valore è compreso tra 0 e il grado di parallelismo della query.

*RowsNumber*  
\[Output\] Numero di coppie chiave-valore.

*CounterNames*  
\[Output\] Dati stringa contenenti le chiavi.

*CounterNamesLength*  
\[Output\] Lunghezza di ogni stringa chiave.

*CounterValues*  
\[Output\] Numeri interi a 64 bit contenenti valori.

## <a name="installexternallibrary"></a>InstallExternalLibrary

Consente di installare una libreria. Questa funzione è facoltativa e non deve essere necessariamente implementata. L'implementazione predefinita prevede di copiare il contenuto della libreria (vedere [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)) in un file disponibile nel percorso appropriato. Il nome del file è il nome della libreria.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Argomenti

*SetupSessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*LibraryName*  
\[Input\] Nome della libreria.

*LibraryNameLength*  
\[Input\] Lunghezza del nome della libreria.

*LibraryFile*  
\[Input\] Percorso (sotto forma di stringa) al file della libreria contenente il contenuto binario specificato da [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

*LibraryFileLength*  
\[Input\] Lunghezza della stringa LibraryFile.

*LibraryInstallDirectory:*  
\[Input\] Directory radice in cui installare la libreria.

*LibraryInstallDirectoryLength*  
\[Input\] Lunghezza della stringa LibraryInstallDirectory.

*LibraryError*  
\[Output\] Parametro di output facoltativo. Se si verificasse un errore durante l'installazione della libreria, LibraryError indirizzerebbe a una stringa che descrive l'errore.

*LibraryErrorLength*  
\[Output\] Lunghezza della stringa LibraryError.

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

Disinstalla una libreria. Questa funzione è facoltativa e non deve essere necessariamente implementata. L'implementazione predefinita prevede di annullare le attività eseguite dall'implementazione predefinita di InstallExternalLibrary. L'implementazione predefinita elimina il contenuto del file *LibraryName* contenuto in *LibraryInstallDirectory*.

### <a name="syntax"></a>Sintassi

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Argomenti

*SetupSessionId*  
\[Input\] GUID che identifica in modo univoco questa sessione di script.

*LibraryName*  
\[Input\] Nome della libreria

*LibraryNameLength*  
\[Input\] Lunghezza del nome della libreria

*LibraryFile*  
\[Input\] Percorso (sotto forma di stringa) al file della libreria contenente il contenuto binario specificato da CREATE EXTERNAL LIBRARY

*LibraryFileLength*  
\[Input\] Lunghezza della stringa LibraryFile

*LibraryInstallDirectory*  
\[Input\] Directory radice in cui installare la libreria

*LibraryInstallDirectoryLength*  
\[Input\] Lunghezza della stringa LibraryInstallDirectory.

*LibraryError*  
\[Output\] Stringa di errore della libreria.

*LibraryErrorLength*  
\[Output\] Lunghezza della stringa LibraryError.

## <a name="next-steps"></a>Passaggi successivi

- [Microsoft Extensibility SDK per Java per SQL Server](../how-to/extensibility-sdk-java-sql-server.md)
- [Runtime personalizzato di Python](../../machine-learning/install/custom-runtime-python.md)
- [Runtime personalizzato di R](../../machine-learning/install/custom-runtime-r.md)