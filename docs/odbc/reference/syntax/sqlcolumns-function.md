---
description: Funzione SQLColumns
title: Funzione SQLColumns | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumns
helpviewer_keywords:
- SQLColumns function [ODBC]
ms.assetid: 4a3618b7-d2b8-43c6-a1fd-7a4e6fa8c7d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51a72000e071d89fc3f189b00c6d8491db1e6e9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201975"
---
# <a name="sqlcolumns-function"></a>Funzione SQLColumns
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: Open Group  
  
 **Summary**  
 **SQLColumns** restituisce l'elenco dei nomi di colonna nelle tabelle specificate. Il driver restituisce queste informazioni come un set di risultati nel *statementHandle* specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLColumns(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      ColumnName,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *CatalogName*  
 Input Nome del catalogo. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non contengono cataloghi. *CatalogName* non può contenere un criterio di ricerca di stringhe.  
  
> [!NOTE]  
>  Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *CatalogName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *CatalogName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo. Per ulteriori informazioni, vedere [arguments in Catalog Functions](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Input Lunghezza in caratteri di **CatalogName*.  
  
 *SchemaName*  
 Input Modello di ricerca di stringhe per i nomi degli schemi. Se un driver supporta schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non dispongono di schemi.  
  
> [!NOTE]  
>  Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *SchemaName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *SchemaName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength2*  
 Input Lunghezza in caratteri di **SchemaName*.  
  
 *TableName*  
 Input Modello di ricerca di stringhe per i nomi di tabella.  
  
> [!NOTE]  
>  Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *TableName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *TableName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength3*  
 Input Lunghezza in caratteri di **TableName*.  
  
 *ColumnName*  
 Input Modello di ricerca di stringhe per i nomi di colonna.  
  
> [!NOTE]  
>  Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *ColumnName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *ColumnName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength4*  
 Input Lunghezza in caratteri di **ColumnName*.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLColumns** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLColumns** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Un cursore è stato aperto in *statementHandle* , ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|L'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE, l'argomento *CatalogName* è un puntatore null e il SQL_CATALOG_NAME *InfoType* restituisce i nomi dei cataloghi supportati.<br /><br /> (DM) l'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE e l'argomento *SchemaName*, *TableName* o *ColumnName* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLColumns** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore di uno degli argomenti della lunghezza del nome è minore di 0 ma non uguale a SQL_NTS.|  
|||Il valore di uno degli argomenti della lunghezza del nome supera il valore di lunghezza massima per il nome o il catalogo corrispondente. La lunghezza massima di ogni catalogo o nome può essere ottenuta chiamando **SQLGetInfo** con i valori *InfoType* . (Vedere "Commenti").|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|È stato specificato un nome di catalogo e il driver o l'origine dati non supporta i cataloghi.<br /><br /> È stato specificato un nome di schema e il driver o l'origine dati non supporta gli schemi.<br /><br /> È stato specificato un criterio di ricerca di stringhe per il nome dello schema, il nome della tabella o il nome della colonna e l'origine dati non supporta i criteri di ricerca per uno o più di questi argomenti.<br /><br /> La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 Questa funzione viene in genere utilizzata prima dell'esecuzione dell'istruzione per recuperare informazioni sulle colonne di una tabella o di tabelle dal catalogo dell'origine dati. **SQLColumns** può essere utilizzato per recuperare i dati per tutti i tipi di elementi restituiti da **SQLTables**. Oltre alle tabelle di base, può includere viste, sinonimi, tabelle di sistema e così via. Al contrario, le funzioni **SQLColAttribute** e **SQLDescribeCol** descrivono le colonne in un set di risultati e la funzione **SQLNumResultCols** restituisce il numero di colonne in un set di risultati. Per ulteriori informazioni, vedere [utilizzi dei dati del catalogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLColumns** restituisce i risultati come set di risultati standard, ordinati in base TABLE_CAT, TABLE_SCHEM, TABLE_NAME e ORDINAL_POSITION.  
  
> [!NOTE]  
>  Quando un'applicazione funziona con ODBC 2. driver *x* , nessuna colonna ORDINAL_POSITION viene restituita nel set di risultati. Di conseguenza, quando si utilizza ODBC 2. i driver *x* , l'ordine delle colonne nell'elenco di colonne restituito da **SQLColumns** non corrisponde necessariamente all'ordine delle colonne restituito quando l'applicazione esegue un'istruzione SELECT su tutte le colonne della tabella.  
  
> [!NOTE]  
>  **SQLColumns** non può restituire tutte le colonne. Un driver, ad esempio, potrebbe non restituire informazioni sulle pseudo-colonne, ad esempio Oracle ROWID. Le applicazioni possono utilizzare qualsiasi colonna valida, indipendentemente dal fatto che venga restituita da **SQLColumns**.  
>   
>  Alcune colonne che possono essere restituite da **SQLStatistics** non vengono restituite da **SQLColumns**. **SQLColumns** , ad esempio, non restituisce le colonne in un indice creato tramite un'espressione o un filtro, ad esempio salary + benefits o Dept = 0012.  
  
 Le lunghezze delle colonne VARCHAR non vengono visualizzate nella tabella. le lunghezze effettive dipendono dall'origine dati. Per determinare le lunghezze effettive delle colonne TABLE_CAT, TABLE_SCHEM, TABLE_NAME e COLUMN_NAME, un'applicazione può chiamare **SQLGetInfo** con le opzioni SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN e SQL_MAX_COLUMN_NAME_LEN.  
  
 Le colonne seguenti sono state rinominate per ODBC 3. *x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|ODBC 3. colonna *x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Le colonne seguenti sono state aggiunte al set di risultati restituito da **SQLColumns** per ODBC 3. *x*:  

:::row:::
    :::column:::
        CHAR_OCTET_LENGTH  
        COLUMN_DEF  
    :::column-end:::
    :::column:::
        IS_NULLABLE  
        ORDINAL_POSITION  
    :::column-end:::
    :::column:::
        SQL_DATA_TYPE  
        SQL_DATETIME_SUB  
    :::column-end:::
:::row-end:::

 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 18 (IS_NULLABLE) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nome colonna|Colonna<br /><br /> d'acquisto|Tipo di dati|Commenti|  
|-----------------|-----------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nome catalogo; NULL se non è applicabile all'origine dati. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le tabelle che non contengono cataloghi.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nome schema; NULL se non è applicabile all'origine dati. Se un driver supporta schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le tabelle che non dispongono di schemi.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar NOT NULL|Nome della tabella.|  
|COLUMN_NAME (ODBC 1,0)|4|Varchar NOT NULL|Nome colonna. Il driver restituisce una stringa vuota per una colonna che non dispone di un nome.|  
|DATA_TYPE (ODBC 1,0)|5|Smallint non NULL|Tipo di dati SQL. Può trattarsi di un tipo di dati SQL ODBC o di un tipo di dati SQL specifico del driver. Per i tipi di dati DateTime e Interval, questa colonna restituisce il tipo di dati conciso, ad esempio SQL_TYPE_DATE o SQL_INTERVAL_YEAR_TO_MONTH, anziché il tipo di dati non conciso, ad esempio SQL_DATETIME o SQL_INTERVAL. Per un elenco dei tipi di dati ODBC SQL validi, vedere tipi di dati [SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.<br /><br /> Tipi di dati restituiti per ODBC 3. *x* e ODBC 2. le applicazioni *x* potrebbero essere diverse. Per altre informazioni, vedere [compatibilità con le versioni precedenti e conformità agli standard](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|TYPE_NAME (ODBC 1,0)|6|Varchar NOT NULL|Nome del tipo di dati dipendente dall'origine dati; ad esempio, "CHAR", "VARCHAR", "MONEY", "LONG VARBINAR" o "CHAR () FOR BIT DATA".|  
|COLUMN_SIZE (ODBC 1,0)|7|Integer|Se DATA_TYPE è SQL_CHAR o SQL_VARCHAR, questa colonna contiene la lunghezza massima in caratteri della colonna. Per i tipi di dati DateTime, questo è il numero totale di caratteri necessari per visualizzare il valore quando viene convertito in caratteri. Per i tipi di dati numerici, indica il numero totale di cifre o il numero totale di bit consentiti nella colonna in base alla colonna NUM_PREC_RADIX. Per i tipi di dati intervallo, questo è il numero di caratteri nella rappresentazione di caratteri del valore letterale intervallo (come definito dalla precisione principale dell'intervallo, vedere [lunghezza del tipo di dati intervallo](../../../odbc/reference/appendixes/interval-data-type-length.md) in Appendice D: tipi di dati). Per ulteriori informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|BUFFER_LENGTH (ODBC 1,0)|8|Integer|Lunghezza in byte dei dati trasferiti in un'operazione SQLGetData, SQLFetch o SQLFetchScroll se si specifica SQL_C_DEFAULT. Per i dati numerici, le dimensioni possono essere diverse da quelle dei dati archiviati nell'origine dati. Questo valore potrebbe essere diverso da quello COLUMN_SIZE colonna per i dati di tipo carattere. Per ulteriori informazioni sulla lunghezza, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.|  
|DECIMAL_DIGITS (ODBC 1,0)|9|Smallint|Numero totale di cifre significative a destra della virgola decimale. Per SQL_TYPE_TIME e SQL_TYPE_TIMESTAMP, questa colonna contiene il numero di cifre nel componente frazioni di secondo. Per gli altri tipi di dati, corrisponde alle cifre decimali della colonna nell'origine dati. Per i tipi di dati intervallo che contengono un componente ora, questa colonna contiene il numero di cifre a destra del separatore decimale (secondi frazionari). Per i tipi di dati intervallo che non contengono un componente ora, questa colonna è 0. Per ulteriori informazioni sulle cifre decimali, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati. Viene restituito NULL per i tipi di dati in cui DECIMAL_DIGITS non è applicabile.|  
|NUM_PREC_RADIX (ODBC 1,0)|10|Smallint|Per i tipi di dati numerici, 10 o 2. Se è 10, i valori in COLUMN_SIZE e DECIMAL_DIGITS indicano il numero di cifre decimali consentito per la colonna. Una colonna DECIMAL (12, 5) restituisce ad esempio un NUM_PREC_RADIX di 10, un COLUMN_SIZE di 12 e un DECIMAL_DIGITS di 5. una colonna FLOAT può restituire un NUM_PREC_RADIX di 10, un COLUMN_SIZE di 15 e un DECIMAL_DIGITS NULL.<br /><br /> Se è 2, i valori in COLUMN_SIZE e DECIMAL_DIGITS forniscono il numero di bit consentiti nella colonna. Una colonna FLOAT, ad esempio, può restituire una radice di 2, una COLUMN_SIZE di 53 e una DECIMAL_DIGITS di valore NULL.<br /><br /> Viene restituito NULL per i tipi di dati in cui NUM_PREC_RADIX non è applicabile.|  
|NULLABLE (ODBC 1,0)|11|Smallint non NULL|SQL_NO_NULLS se la colonna non può includere valori NULL.<br /><br /> SQL_NULLABLE se la colonna accetta valori NULL.<br /><br /> SQL_NULLABLE_UNKNOWN se non è noto se la colonna accetta valori NULL.<br /><br /> Il valore restituito per questa colonna è diverso dal valore restituito per la colonna IS_NULLABLE. La colonna NULLABLE indica con certezza che una colonna può accettare valori NULL, ma non può indicare con certezza che una colonna non accetta valori NULL. La colonna IS_NULLABLE indica con certezza che una colonna non può accettare valori NULL, ma non può indicare con certezza che una colonna accetta valori NULL.|  
|OSSERVAZIONI (ODBC 1,0)|12|Varchar|Descrizione della colonna.|  
|COLUMN_DEF (ODBC 3,0)|13|Varchar|Valore predefinito della colonna. Il valore in questa colonna deve essere interpretato come stringa se è racchiuso tra virgolette.<br /><br /> Se è stato specificato NULL come valore predefinito, questa colonna è la parola NULL, non racchiusa tra virgolette. Se il valore predefinito non può essere rappresentato senza troncamento, la colonna contiene TRONCAta senza racchiudere le virgolette singole. Se non è stato specificato alcun valore predefinito, questa colonna è NULL.<br /><br /> Il valore di COLUMN_DEF può essere utilizzato per generare una nuova definizione di colonna, tranne quando contiene il valore TRONCAto.|  
|SQL_DATA_TYPE (ODBC 3,0)|14|Smallint non NULL|Tipo di dati SQL, così come viene visualizzato nel campo SQL_DESC_TYPE record in IRD. Può trattarsi di un tipo di dati SQL ODBC o di un tipo di dati SQL specifico del driver. Questa colonna corrisponde alla colonna DATA_TYPE, ad eccezione dei tipi di dati DateTime e Interval. Questa colonna restituisce il tipo di dati non conciso, ad esempio SQL_DATETIME o SQL_INTERVAL, anziché il tipo di dati conciso, ad esempio SQL_TYPE_DATE o SQL_INTERVAL_YEAR_TO_MONTH, per i tipi di dati DateTime e Interval. Se questa colonna restituisce SQL_DATETIME o SQL_INTERVAL, è possibile determinare il tipo di dati specifico dalla colonna SQL_DATETIME_SUB. Per un elenco dei tipi di dati ODBC SQL validi, vedere tipi di dati [SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.<br /><br /> Tipi di dati restituiti per ODBC 3. *x* e ODBC 2. le applicazioni *x* potrebbero essere diverse. Per altre informazioni, vedere [compatibilità con le versioni precedenti e conformità agli standard](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|SQL_DATETIME_SUB (ODBC 3,0)|15|Smallint|Codice del sottotipo per i tipi di dati DateTime e Interval. Per gli altri tipi di dati in questa colonna viene restituito NULL. Per ulteriori informazioni sui sottocodici DateTime e Interval, vedere "SQL_DESC_DATETIME_INTERVAL_CODE" in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).|  
|CHAR_OCTET_LENGTH (ODBC 3,0)|16|Integer|Lunghezza massima in byte di una colonna con tipo di dati character o Binary. Per gli tutti gli altri tipi di dati, il valore di questa colonna è NULL.|  
|ORDINAL_POSITION (ODBC 3,0)|17|Integer non NULL|Posizione ordinale della colonna nella tabella. La prima colonna della tabella è il numero 1.|  
|IS_NULLABLE (ODBC 3,0)|18|Varchar|"NO" se la colonna non include valori NULL.<br /><br /> "YES" se la colonna può includere valori NULL.<br /><br /> Quando non è noto se i valori Null sono supportati, in questa colonna viene restituita una stringa di lunghezza zero.<br /><br /> Per determinare il supporto di valori Null vengono seguite le regole ISO. In un sistema DBMS conforme a ISO SQL non vengono restituite stringhe vuote.<br /><br /> Il valore restituito per questa colonna è diverso dal valore restituito per la colonna NULLABLE. (Vedere la descrizione della colonna NULLABLE).|  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente un'applicazione dichiara i buffer per il set di risultati restituito da **SQLColumns**. Chiama **SQLColumns** per restituire un set di risultati che descrive ogni colonna della tabella Employee. Chiama quindi **SQLBindCol** per associare i buffer alle colonne del set di risultati. Infine, l'applicazione recupera ogni riga di dati con **SQLFetch** e la elabora.  
  
```cpp  
// SQLColumns_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#define STR_LEN 128 + 1  
#define REM_LEN 254 + 1  
  
// Declare buffers for result set data  
SQLCHAR szSchema[STR_LEN];  
SQLCHAR szCatalog[STR_LEN];  
SQLCHAR szColumnName[STR_LEN];  
SQLCHAR szTableName[STR_LEN];  
SQLCHAR szTypeName[STR_LEN];  
SQLCHAR szRemarks[REM_LEN];  
SQLCHAR szColumnDefault[STR_LEN];  
SQLCHAR szIsNullable[STR_LEN];  
  
SQLINTEGER ColumnSize;  
SQLINTEGER BufferLength;  
SQLINTEGER CharOctetLength;  
SQLINTEGER OrdinalPosition;  
  
SQLSMALLINT DataType;  
SQLSMALLINT DecimalDigits;  
SQLSMALLINT NumPrecRadix;  
SQLSMALLINT Nullable;  
SQLSMALLINT SQLDataType;  
SQLSMALLINT DatetimeSubtypeCode;  
  
SQLHSTMT hstmt = NULL;  
  
// Declare buffers for bytes available to return  
SQLINTEGER cbCatalog;  
SQLINTEGER cbSchema;  
SQLINTEGER cbTableName;  
SQLINTEGER cbColumnName;  
SQLINTEGER cbDataType;  
SQLINTEGER cbTypeName;  
SQLINTEGER cbColumnSize;  
SQLLEN cbBufferLength;  
SQLINTEGER cbDecimalDigits;  
SQLINTEGER cbNumPrecRadix;  
SQLINTEGER cbNullable;  
SQLINTEGER cbRemarks;  
SQLINTEGER cbColumnDefault;  
SQLINTEGER cbSQLDataType;  
SQLINTEGER cbDatetimeSubtypeCode;  
SQLINTEGER cbCharOctetLength;  
SQLINTEGER cbOrdinalPosition;  
SQLINTEGER cbIsNullable;  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retcode = SQLColumns(hstmt, NULL, 0, NULL, 0, (SQLCHAR*)"CUSTOMERS", SQL_NTS, NULL, 0);  
  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      // Bind columns in result set to buffers  
      SQLBindCol(hstmt, 1, SQL_C_CHAR, szCatalog, STR_LEN,&cbCatalog);  
      SQLBindCol(hstmt, 2, SQL_C_CHAR, szSchema, STR_LEN, &cbSchema);  
      SQLBindCol(hstmt, 3, SQL_C_CHAR, szTableName, STR_LEN,&cbTableName);  
      SQLBindCol(hstmt, 4, SQL_C_CHAR, szColumnName, STR_LEN, &cbColumnName);  
      SQLBindCol(hstmt, 5, SQL_C_SSHORT, &DataType, 0, &cbDataType);  
      SQLBindCol(hstmt, 6, SQL_C_CHAR, szTypeName, STR_LEN, &cbTypeName);  
      SQLBindCol(hstmt, 7, SQL_C_SLONG, &ColumnSize, 0, &cbColumnSize);  
      SQLBindCol(hstmt, 8, SQL_C_SLONG, &BufferLength, 0, &cbBufferLength);  
      SQLBindCol(hstmt, 9, SQL_C_SSHORT, &DecimalDigits, 0, &cbDecimalDigits);  
      SQLBindCol(hstmt, 10, SQL_C_SSHORT, &NumPrecRadix, 0, &cbNumPrecRadix);  
      SQLBindCol(hstmt, 11, SQL_C_SSHORT, &Nullable, 0, &cbNullable);  
      SQLBindCol(hstmt, 12, SQL_C_CHAR, szRemarks, REM_LEN, &cbRemarks);  
      SQLBindCol(hstmt, 13, SQL_C_CHAR, szColumnDefault, STR_LEN, &cbColumnDefault);  
      SQLBindCol(hstmt, 14, SQL_C_SSHORT, &SQLDataType, 0, &cbSQLDataType);  
      SQLBindCol(hstmt, 15, SQL_C_SSHORT, &DatetimeSubtypeCode, 0, &cbDatetimeSubtypeCode);  
      SQLBindCol(hstmt, 16, SQL_C_SLONG, &CharOctetLength, 0, &cbCharOctetLength);  
      SQLBindCol(hstmt, 17, SQL_C_SLONG, &OrdinalPosition, 0, &cbOrdinalPosition);  
      SQLBindCol(hstmt, 18, SQL_C_CHAR, szIsNullable, STR_LEN, &cbIsNullable);  
  
      while (SQL_SUCCESS == retcode) {  
         retcode = SQLFetch(hstmt);  
         /*  
         if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // show_error();  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // Process fetched data  
         else  
            break;  
        */  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di privilegi per una o più colonne|[SQLColumnPrivileges Function](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di più righe di dati|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Restituzione di colonne che identificano in modo univoco una riga o colonne aggiornate automaticamente da una transazione|[Funzione SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|  
|Restituzione di statistiche e indici tabella|[Funzione SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|Restituzione di un elenco di tabelle in un'origine dati|[Funzione SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|  
|Restituzione di privilegi per una tabella o tabelle|[Funzione SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
