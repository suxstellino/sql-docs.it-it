---
description: SQLColumnPrivileges Function
title: Funzione SQLColumnPrivileges | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColumnPrivileges
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumnPrivileges
helpviewer_keywords:
- SQLColumnPrivileges function [ODBC]
ms.assetid: ef233d9a-6ed5-4986-9d42-5e0b1a79fb6e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33bf7f6289ec7c7f3167372cbdd658d3db32f28e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208430"
---
# <a name="sqlcolumnprivileges-function"></a>SQLColumnPrivileges Function
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLColumnPrivileges** restituisce un elenco di colonne e i privilegi associati per la tabella specificata. Il driver restituisce le informazioni come un set di risultati nel *statementHandle* specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLColumnPrivileges(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *CatalogName*  
 Input Nome del catalogo. Se un driver supporta nomi per alcuni cataloghi ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica i cataloghi che non dispongono di nomi. *CatalogName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *CatalogName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *CatalogName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo. Per ulteriori informazioni, vedere [arguments in Catalog Functions](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Input Lunghezza in caratteri di **CatalogName*.  
  
 *SchemaName*  
 Input Nome dello schema. Se un driver supporta gli schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non contengono schemi. *SchemaName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *SchemaName* viene considerato come un identificatore. Se è SQL_FALSE, *SchemaName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength2*  
 Input Lunghezza in caratteri di **SchemaName*.  
  
 *TableName*  
 Input Nome della tabella. Questo argomento non può essere un puntatore null. *TableName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *TableName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *TableName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength3*  
 Input Lunghezza in caratteri di **TableName*.  
  
 *ColumnName*  
 Input Modello di ricerca di stringhe per i nomi di colonna.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *ColumnName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *ColumnName* è un argomento del valore del modello; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength4*  
 Input Lunghezza in caratteri di **ColumnName*.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLColumnPrivileges** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLColumnPrivileges** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|24000|Stato del cursore non valido|Un cursore è stato aperto in *statementHandle* e **SQLFetch** o **SQLFetchScroll** è stato chiamato. Questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.<br /><br /> Un cursore è stato aperto in *statementHandle*, ma non è stato chiamato **SQLFetch** o **SQLFetchScroll** .|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|L'argomento *TableName* è un puntatore null.<br /><br /> L'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE, l'argomento *CatalogName* è un puntatore null e il SQL_CATALOG_NAME *InfoType* restituisce i nomi dei cataloghi supportati.<br /><br /> (DM) l'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE e l'argomento *SchemaName* o *ColumnName* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore di uno degli argomenti della lunghezza del nome è minore di 0 ma non uguale a SQL_NTS.|  
|||Il valore di uno degli argomenti della lunghezza del nome supera il valore di lunghezza massima per il nome corrispondente. (Vedere "Commenti").|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|È stato specificato un nome di catalogo e il driver o l'origine dati non supporta i cataloghi.<br /><br /> È stato specificato un nome di schema e il driver o l'origine dati non supporta gli schemi.<br /><br /> È stato specificato un criterio di ricerca di stringhe per il nome della colonna e l'origine dati non supporta i criteri di ricerca per tale argomento.<br /><br /> La combinazione delle impostazioni correnti degli attributi SQL_CONCURRENCY e SQL_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 **SQLColumnPrivileges** restituisce i risultati come set di risultati standard, ordinati per TABLE_CAT, TABLE_SCHEM, TABLE_NAME, column_name e Privilege.  
  
> [!NOTE]  
>  **SQLColumnPrivileges** potrebbe non restituire i privilegi per tutte le colonne. Un driver, ad esempio, potrebbe non restituire informazioni sui privilegi per le pseudo-colonne, ad esempio Oracle ROWID. Le applicazioni possono utilizzare qualsiasi colonna valida, indipendentemente dal fatto che venga restituita da **SQLColumnPrivileges**.  
  
 Le lunghezze delle colonne VARCHAR non vengono visualizzate nella tabella. le lunghezze effettive dipendono dall'origine dati. Per determinare le lunghezze effettive delle colonne CATALOG_NAME, SCHEMA_NAME, TABLE_NAME e COLUMN_NAME, un'applicazione può chiamare **SQLGetInfo** con le opzioni SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN e SQL_MAX_COLUMN_NAME_LEN.  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Le colonne seguenti sono state rinominate per ODBC 3. *x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|ODBC 3. colonna *x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
  
 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 8 (IS_GRANTABLE) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nome colonna|Numero di colonna|Tipo di dati|Commenti|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Identificatore del catalogo; NULL se non è applicabile all'origine dati. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le tabelle che non contengono cataloghi.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Identificatore dello schema; NULL se non è applicabile all'origine dati. Se un driver supporta schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, restituisce una stringa vuota ("") per le tabelle che non dispongono di schemi.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar NOT NULL|Identificatore di tabella.|  
|COLUMN_NAME (ODBC 1,0)|4|Varchar NOT NULL|Nome colonna. Il driver restituisce una stringa vuota per una colonna che non dispone di un nome.|  
|UTENTE CHE CONCEDE LE AUTORIZZAZIONI (ODBC 1,0)|5|Varchar|Nome dell'utente che ha concesso il privilegio; NULL se non è applicabile all'origine dati.<br /><br /> Per tutte le righe in cui il valore nella colonna utente autorizzato è il proprietario dell'oggetto, la colonna di concessione sarà "_SYSTEM".|  
|UTENTE AUTORIZZATO (ODBC 1,0)|6|Varchar NOT NULL|Nome dell'utente a cui è stato concesso il privilegio.|  
|PRIVILEGIO (ODBC 1,0)|7|Varchar NOT NULL|Identifica il privilegio della colonna. Può essere uno dei seguenti (o altri supportati dall'origine dati in fase di implementazione):<br /><br /> SELECT: l'utente autorizzato può recuperare i dati per la colonna.<br /><br /> INSERT: l'utente autorizzato ha la possibilità di fornire dati per la colonna nelle nuove righe inserite nella tabella associata.<br /><br /> AGGIORNAMENTO: l'utente autorizzato può aggiornare i dati nella colonna.<br /><br /> RIFERIMENTI: l'utente autorizzato può fare riferimento alla colonna all'interno di un vincolo, ad esempio un vincolo UNIQUE, referenziale o check di tabella.|  
|IS_GRANTABLE (ODBC 1,0)|8|Varchar|Indica se l'utente autorizzato può concedere il privilegio ad altri utenti. "Sì", "NO" o "NULL" se sconosciuta o non applicabile all'origine dati.<br /><br /> Un privilegio può essere concesso o non consentita, ma non entrambi. Il set di risultati restituito da **SQLColumnPrivileges** non conterrà mai due righe per le quali tutte le colonne ad eccezione della colonna IS_GRANTABLE contengono lo stesso valore.|  
  
## <a name="code-example"></a>Esempio di codice  
 Per un esempio di codice di una funzione simile, vedere [funzione SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione delle colonne di una tabella o di tabelle|[Funzione SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Recupero di più righe di dati|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Restituzione di privilegi per una tabella o tabelle|[Funzione SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
|Restituzione di un elenco di tabelle in un'origine dati|[Funzione SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
