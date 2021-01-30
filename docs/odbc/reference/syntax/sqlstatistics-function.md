---
description: Funzione SQLStatistics
title: Funzione SQLStatistics | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLStatistics
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLStatistics
helpviewer_keywords:
- SQLStatistics function [ODBC]
ms.assetid: 45210682-cfea-4e5d-9951-bcf1cbe10f41
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 711efa4797855e42b278e519106c38d62fda1817
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191748"
---
# <a name="sqlstatistics-function"></a>Funzione SQLStatistics
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLStatistics** recupera un elenco di statistiche relative a una singola tabella e gli indici associati alla tabella. Il driver restituisce le informazioni come un set di risultati.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLStatistics(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CatalogName,  
     SQLSMALLINT     NameLength1,  
     SQLCHAR *       SchemaName,  
     SQLSMALLINT     NameLength2,  
     SQLCHAR *       TableName,  
     SQLSMALLINT     NameLength3,  
     SQLUSMALLINT    Unique,  
     SQLUSMALLINT    Reserved);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *CatalogName*  
 Input Nome del catalogo. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non contengono cataloghi. *CatalogName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *CatalogName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *CatalogName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo. Per ulteriori informazioni, vedere [arguments in Catalog Functions](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Input Lunghezza in caratteri di **CatalogName*.  
  
 *SchemaName*  
 Input Nome dello schema. Se un driver supporta schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non dispongono di schemi. *SchemaName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *SchemaName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *SchemaName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength2*  
 Input Lunghezza in caratteri di **SchemaName*.  
  
 *TableName*  
 Input Nome della tabella. Questo argomento non può essere un puntatore null. *SchemaName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *TableName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *TableName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength3*  
 Input Lunghezza in caratteri di **TableName*.  
  
 *Univoco*  
 Input Tipo di indice: SQL_INDEX_UNIQUE o SQL_INDEX_ALL.  
  
 *Reserved*  
 Input Indica l'importanza delle colonne CARDINALity e PAGEs nel set di risultati. Le opzioni seguenti influiscono solo sulla restituzione delle colonne CARDINALity e PAGEs. le informazioni sugli indici vengono restituite anche se la CARDINALità e le pagine non vengono restituite.  
  
 SQL_ENSURE richiede che il driver recuperi le statistiche in modo incondizionato. I driver conformi esclusivamente allo standard di gruppo aperto e non supportano le estensioni ODBC non saranno in grado di supportare SQL_ENSURE.  
  
 SQL_QUICK richiede che il driver recuperi la CARDINALità e le pagine solo se sono prontamente disponibili dal server. In tal caso, il driver non garantisce che i valori siano aggiornati. (Le applicazioni scritte nello standard del gruppo aperto otterranno sempre SQL_QUICK comportamento dai driver conformi a ODBC *3. x*).  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLStatistics** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLStatistics** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Un cursore è stato aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* e quindi la funzione è stata chiamata nuovamente su *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|L'argomento *TableName* è un puntatore null.<br /><br /> L'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE, l'argomento *CatalogName* è un puntatore null e il SQL_CATALOG_NAME *InfoType* restituisce i nomi dei cataloghi supportati.<br /><br /> (DM) l'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE e l'argomento *SchemaName* era un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLStatistics** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore di uno degli argomenti della lunghezza del nome è minore di 0 ma non uguale a SQL_NTS.<br /><br /> Il valore di uno degli argomenti della lunghezza del nome supera il valore di lunghezza massima per il nome corrispondente.|  
|HY100|Tipo di opzione di univocità non compreso nell'intervallo|(DM) è stato specificato un valore *univoco* non valido.|  
|HY101|Tipo di opzione di accuratezza non compreso nell'intervallo|(DM) è stato specificato un valore *riservato* non valido.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|È stato specificato un catalogo e il driver o l'origine dati non supporta i cataloghi.<br /><br /> È stato specificato uno schema e il driver o l'origine dati non supporta gli schemi.<br /><br /> La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati richiesto. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLStatistics** restituisce informazioni su una singola tabella come set di risultati standard, ordinate per NON_UNIQUE, TYPE, INDEX_QUALIFIER, INDEX_NAME e ORDINAL_POSITION. Il set di risultati combina le informazioni sulle statistiche (nelle colonne CARDINALity e PAGEs del set di risultati) per la tabella con informazioni su ogni indice. Per informazioni sulle modalità di utilizzo di queste informazioni, vedere [utilizzi dei dati del catalogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Per determinare le lunghezze effettive delle colonne TABLE_CAT, TABLE_SCHEM, TABLE_NAME e COLUMN_NAME, un'applicazione può chiamare **SQLGetInfo** con le opzioni SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN e SQL_MAX_COLUMN_NAME_LEN.  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Le colonne seguenti sono state rinominate per ODBC *3. x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|Colonna ODBC *3. x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|SEQ_IN_INDEX|ORDINAL_POSITION|  
|COLLATION|ASC_OR_DESC|  
  
 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 13 (FILTER_CONDITION) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nome colonna|Numero di colonna|Tipo di dati|Commenti|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nome del catalogo della tabella a cui si applica la statistica o l'indice; NULL se non è applicabile all'origine dati. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le tabelle che non contengono cataloghi.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nome dello schema della tabella a cui si applica la statistica o l'indice. NULL se non è applicabile all'origine dati. Se un driver supporta schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le tabelle che non dispongono di schemi.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar NOT NULL|Nome della tabella a cui si applica la statistica o l'indice.|  
|NON_UNIQUE (ODBC 1,0)|4|Smallint|Indica se l'indice non consente valori duplicati:<br /><br /> SQL_TRUE se i valori dell'indice possono essere non univoci.<br /><br /> SQL_FALSE se i valori di indice devono essere univoci.<br /><br /> Se il tipo è SQL_TABLE_STAT, viene restituito NULL.|  
|INDEX_QUALIFIER (ODBC 1,0)|5|Varchar|Identificatore usato per qualificare il nome dell'indice che esegue una **Drop index**; Viene restituito NULL se un qualificatore di indice non è supportato dall'origine dati o se il tipo è SQL_TABLE_STAT. Se in questa colonna viene restituito un valore non null, è necessario utilizzarlo per qualificare il nome di indice in un'istruzione **Drop index** ; in caso contrario, è necessario utilizzare il TABLE_SCHEM per qualificare il nome dell'indice.|  
|INDEX_NAME (ODBC 1,0)|6|Varchar|Nome indice; Se il tipo è SQL_TABLE_STAT, viene restituito NULL.|  
|TIPO (ODBC 1,0)|7|Smallint non NULL|Tipo di informazioni restituite:<br /><br /> SQL_TABLE_STAT indica una statistica per la tabella (nella colonna CARDINALità o pagine).<br /><br /> SQL_INDEX_BTREE indica un indice albero B.<br /><br /> SQL_INDEX_CLUSTERED indica un indice cluster.<br /><br /> SQL_INDEX_CONTENT indica un indice di contenuto.<br /><br /> SQL_INDEX_HASHED indica un indice con hash.<br /><br /> SQL_INDEX_OTHER indica un altro tipo di indice.|  
|ORDINAL_POSITION (ODBC 1,0)|8|Smallint|Numero di sequenza della colonna nell'indice (a partire da 1); Se il tipo è SQL_TABLE_STAT, viene restituito NULL.|  
|COLUMN_NAME (ODBC 1,0)|9|Varchar|Nome colonna. Se la colonna è basata su un'espressione, ad esempio SALARY + BENEFITs, viene restituita l'espressione. Se non è possibile determinare l'espressione, viene restituita una stringa vuota. Se il tipo è SQL_TABLE_STAT, viene restituito NULL.|  
|ASC_OR_DESC (ODBC 1,0)|10|Char(1)|Sequenza di ordinamento per la colonna: "A" per l'ordine crescente; "D" per Descending; Viene restituito NULL se la sequenza di ordinamento della colonna non è supportata dall'origine dati o se il tipo è SQL_TABLE_STAT.|  
|CARDINALITÀ (ODBC 1,0)|11|Integer|Cardinalità della tabella o dell'indice; numero di righe nella tabella se il tipo è SQL_TABLE_STAT; numero di valori univoci nell'indice se il tipo non è SQL_TABLE_STAT; Viene restituito NULL se il valore non è disponibile dall'origine dati.|  
|PAGINE (ODBC 1,0)|12|Integer|Numero di pagine utilizzate per archiviare l'indice o la tabella; numero di pagine per la tabella se il tipo è SQL_TABLE_STAT; numero di pagine per l'indice se il tipo non è SQL_TABLE_STAT; Viene restituito NULL se il valore non è disponibile dall'origine dati o se non è applicabile all'origine dati.|  
|FILTER_CONDITION (ODBC 2,0)|13|Varchar|Se l'indice è filtrato, questa è la condizione di filtro, ad esempio SALARY > 30000; Se non è possibile determinare la condizione di filtro, si tratta di una stringa vuota.<br /><br /> NULL se l'indice non è filtrato, non è possibile determinare se l'indice è filtrato o se il tipo è SQL_TABLE_STAT.|  
  
 Se la riga del set di risultati corrisponde a una tabella, il driver imposta il tipo su SQL_TABLE_STAT e imposta NON_UNIQUE, INDEX_QUALIFIER, INDEX_NAME, ORDINAL_POSITION, COLUMN_NAME e ASC_OR_DESC su NULL. Se la CARDINALità o le pagine non sono disponibili dall'origine dati, il driver le imposta su NULL.  
  
## <a name="code-example"></a>Esempio di codice  
 Per un esempio di codice di una funzione simile, vedere [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di solo avanzamento.|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Restituzione di colonne di chiavi esterne|[Funzione SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|  
|Restituzione delle colonne di una chiave primaria|[SQLPrimaryKeys Function](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
