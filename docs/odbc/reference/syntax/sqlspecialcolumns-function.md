---
description: Funzione SQLSpecialColumns
title: Funzione SQLSpecialColumns | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSpecialColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSpecialColumns
helpviewer_keywords:
- SQLSpecialColumns function [ODBC]
ms.assetid: bb2d9f21-bda0-4e50-a8be-f710db660034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cf4f810499c4a3409d134806ead739096c5b1b89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191753"
---
# <a name="sqlspecialcolumns-function"></a>Funzione SQLSpecialColumns
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: Open Group  
  
 **Summary**  
 **SQLSpecialColumns** recupera le informazioni seguenti sulle colonne all'interno di una tabella specificata:  
  
-   Set ottimale di colonne che identifica in modo univoco una riga nella tabella.  
  
-   Colonne che vengono aggiornate automaticamente quando un valore della riga viene aggiornato da una transazione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLSpecialColumns(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   IdentifierType,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLSMALLINT   Scope,  
     SQLSMALLINT   Nullable);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *IdentifierType*  
 Input Tipo di colonna da restituire. Deve essere uno dei valori seguenti:   
  
 SQL_BEST_ROWID: restituisce la colonna o il set di colonne ottimale che, tramite il recupero di valori dalla colonna o dalle colonne, consente di identificare in modo univoco tutte le righe della tabella specificata. Una colonna può essere una pseudo-colonna progettata appositamente per questo scopo (come in Oracle ROWID o Ingres TID) oppure la colonna o le colonne di qualsiasi indice univoco per la tabella.  
  
 SQL_ROWVER: restituisce la colonna o le colonne nella tabella specificata, se presente, che vengono aggiornate automaticamente dall'origine dati quando un valore nella riga viene aggiornato da qualsiasi transazione (come in SQLBase ROWID o Sybase TIMESTAMP).  
  
 *CatalogName*  
 Input Nome del catalogo per la tabella. Se un driver supporta i cataloghi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non contengono cataloghi. *CatalogName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *CatalogName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *CatalogName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo. Per ulteriori informazioni, vedere [arguments in Catalog Functions](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Input Lunghezza in caratteri di **CatalogName*.  
  
 *SchemaName*  
 Input Nome dello schema per la tabella. Se un driver supporta gli schemi per alcune tabelle ma non per altri, ad esempio quando il driver recupera dati da DBMS diversi, una stringa vuota ("") indica le tabelle che non contengono schemi. *SchemaName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *SchemaName* viene considerato come un identificatore e il case non è significativo. Se è SQL_FALSE, *SchemaName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength2*  
 Input Lunghezza in caratteri di **SchemaName*.  
  
 *TableName*  
 Input Nome della tabella. Questo argomento non può essere un puntatore null. *TableName* non può contenere un criterio di ricerca di stringhe.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, *TableName* viene considerato come un identificatore e il relativo case non è significativo. Se è SQL_FALSE, *TableName* è un argomento normale; viene trattato letteralmente e il suo caso è significativo.  
  
 *NameLength3*  
 Input Lunghezza in caratteri di **TableName*.  
  
 *Scope*  
 Input Ambito minimo obbligatorio del ROWID. Il ROWID restituito può essere di un ambito maggiore. I possibili valori sono i seguenti:  
  
 SQL_SCOPE_CURROW: ROWID è sicuramente valido solo se posizionato in corrispondenza di tale riga. Una nuova selezione successiva con ROWID non può restituire una riga se la riga è stata aggiornata o eliminata da un'altra transazione.  
  
 SQL_SCOPE_TRANSACTION: è garantita la validità della ROWID per la durata della transazione corrente.  
  
 SQL_SCOPE_SESSION: è garantito che ROWID sia valido per la durata della sessione (oltre i limiti delle transazioni).  
  
 *Ammette i valori Null*  
 Input Determina se restituire colonne speciali che possono avere un valore NULL. I possibili valori sono i seguenti:  
  
 SQL_NO_NULLS: escludere colonne speciali che possono avere valori NULL. Alcuni driver non supportano SQL_NO_NULLS e questi driver restituiranno un set di risultati vuoto se è stato specificato SQL_NO_NULLS. Per questo caso è necessario preparare le applicazioni e richiedere SQL_NO_NULLS solo se è assolutamente necessario.  
  
 SQL_NULLABLE: restituisce colonne speciali anche se possono avere valori NULL.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSpecialColumns** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLSpecialColumns** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
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
|HY009|Uso non valido del puntatore null|L'argomento *TableName* è un puntatore null.<br /><br /> L'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE, l'argomento *CatalogName* è un puntatore null e il SQL_CATALOG_NAME *InfoType* restituisce i nomi dei cataloghi supportati.<br /><br /> (DM) l'attributo SQL_ATTR_METADATA_ID Statement è stato impostato su SQL_TRUE e l'argomento *SchemaName* era un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione era ancora in esecuzione quando è stato chiamato **SQLSpecialColumns** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore di uno degli argomenti di lunghezza è minore di 0 ma non uguale a SQL_NTS.<br /><br /> Il valore di uno degli argomenti di lunghezza ha superato il valore di lunghezza massima per il nome corrispondente. La lunghezza massima di ogni nome può essere ottenuta chiamando **SQLGetInfo** con i valori *InfoType* : SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN o SQL_MAX_TABLE_NAME_LEN.|  
|HY097|Tipo di colonna non compreso nell'intervallo|(DM) è stato specificato un valore *IdentifierType* non valido.|  
|HY098|Tipo di ambito non compreso nell'intervallo|(DM) è stato specificato un valore di *ambito* non valido.|  
|HY099|Tipo nullable non compreso nell'intervallo|(DM) è stato specificato un valore *Nullable* non valido.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|È stato specificato un catalogo e il driver o l'origine dati non supporta i cataloghi.<br /><br /> È stato specificato uno schema e il driver o l'origine dati non supporta gli schemi.<br /><br /> La combinazione delle impostazioni correnti degli attributi SQL_ATTR_CONCURRENCY e SQL_ATTR_CURSOR_TYPE istruzione non è supportata dal driver o dall'origine dati.<br /><br /> L'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_VARIABLE e l'attributo SQL_ATTR_CURSOR_TYPE Statement è stato impostato su un tipo di cursore per il quale il driver non supporta i segnalibri.|  
|HYT00|Timeout|Il periodo di timeout della query è scaduto prima che l'origine dati restituisse il set di risultati richiesto. Il periodo di timeout viene impostato tramite **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
## <a name="comments"></a>Commenti  
 Quando l'argomento *IdentifierType* è SQL_BEST_ROWID, **SQLSpecialColumns** restituisce la colonna o le colonne che identificano in modo univoco ogni riga della tabella. Queste colonne possono essere sempre utilizzate in una clausola *select-list* o **where** . **SQLColumns**, utilizzato per restituire una serie di informazioni sulle colonne di una tabella, non restituisce necessariamente le colonne che identificano in modo univoco ogni riga o le colonne che vengono aggiornate automaticamente quando un valore della riga viene aggiornato da una transazione. **SQLColumns** , ad esempio, potrebbe non restituire la pseudo-colonna Oracle ROWID. Questo è il motivo per cui viene usato **SQLSpecialColumns** per restituire queste colonne. Per ulteriori informazioni, vedere [utilizzi dei dati del catalogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Per ulteriori informazioni sull'utilizzo generale, sugli argomenti e sui dati restituiti delle funzioni del catalogo ODBC, vedere [funzioni di catalogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Se non sono presenti colonne che identificano in modo univoco ogni riga della tabella, **SQLSpecialColumns** restituisce un set di righe senza righe; una chiamata successiva a **SQLFetch** o **SQLFetchScroll** nell'istruzione restituisce SQL_NO_DATA.  
  
 Se gli argomenti *IdentifierType*, *scope* o *Nullable* specificano caratteristiche che non sono supportate dall'origine dati, **SQLSpecialColumns** restituisce un set di risultati vuoto.  
  
 Se l'attributo SQL_ATTR_METADATA_ID Statement è impostato su SQL_TRUE, gli argomenti *CatalogName*, *SchemaName* e *TableName* vengono considerati come identificatori, quindi non possono essere impostati su un puntatore null in determinate situazioni. Per ulteriori informazioni, vedere [argomenti nelle funzioni di catalogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 **SQLSpecialColumns** restituisce i risultati come set di risultati standard, ordinati in base all'ambito.  
  
 Le colonne seguenti sono state rinominate per ODBC *3. x*. Le modifiche al nome di colonna non influiscono sulla compatibilità con le versioni precedenti perché le applicazioni vengono associate per numero di colonna  
  
|ODBC 2,0-colonna|Colonna ODBC *3. x*|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
  
 Per determinare la lunghezza effettiva della colonna COLUMN_NAME, un'applicazione può chiamare **SQLGetInfo** con l'opzione SQL_MAX_COLUMN_NAME_LEN.  
  
 Nella tabella seguente sono elencate le colonne del set di risultati. È possibile definire colonne aggiuntive oltre la colonna 8 (PSEUDO_COLUMN) dal driver. Un'applicazione deve ottenere l'accesso alle colonne specifiche del driver eseguendo il conteggio a discesa dalla fine del set di risultati anziché specificare una posizione ordinale esplicita. Per ulteriori informazioni, vedere [dati restituiti da funzioni di catalogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nome colonna|Numero di colonna|Tipo di dati|Commenti|  
|-----------------|-------------------|---------------|--------------|  
|AMBITO (ODBC 1,0)|1|Smallint|Ambito effettivo del ROWID. Contiene uno dei valori seguenti:<br /><br /> SQL_SCOPE_CURROW SQL_SCOPE_TRANSACTION SQL_SCOPE_SESSION<br /><br /> Quando *IdentifierType* è SQL_ROWVER, viene restituito null. Per una descrizione di ogni valore, vedere la descrizione dell' *ambito* in "Syntax", più indietro in questa sezione.|  
|COLUMN_NAME (ODBC 1,0)|2|Varchar NOT NULL|Nome colonna. Il driver restituisce una stringa vuota per una colonna che non dispone di un nome.|  
|DATA_TYPE (ODBC 1,0)|3|Smallint non NULL|Tipo di dati SQL. Può trattarsi di un tipo di dati SQL ODBC o di un tipo di dati SQL specifico del driver. Per un elenco dei tipi di dati ODBC SQL validi, vedere [tipi di dati SQL](../../../odbc/reference/appendixes/sql-data-types.md). Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.|  
|TYPE_NAME (ODBC 1,0)|4|Varchar NOT NULL|Nome del tipo di dati dipendente dall'origine dati; ad esempio, "CHAR", "VARCHAR", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA".|  
|COLUMN_SIZE (ODBC 1,0)|5|Integer|Dimensione della colonna nell'origine dati. Per ulteriori informazioni sulle dimensioni delle colonne, vedere [dimensioni delle colonne, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|BUFFER_LENGTH (ODBC 1,0)|6|Integer|Lunghezza in byte dei dati trasferiti in un'operazione **SQLGetData** o **SQLFetch** se viene specificato SQL_C_DEFAULT. Per i dati numerici, questa dimensione può essere diversa da quella dei dati archiviati nell'origine dati. Questo valore corrisponde alla colonna COLUMN_SIZE per i dati di tipo carattere o binario. Per altre informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|DECIMAL_DIGITS (ODBC 1,0)|7|Smallint|Cifre decimali della colonna nell'origine dati. Viene restituito NULL per i tipi di dati in cui non sono applicabili cifre decimali. Per ulteriori informazioni sulle cifre decimali, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|PSEUDO_COLUMN (ODBC 2,0)|8|Smallint|Indica se la colonna è una pseudo-colonna, ad esempio Oracle ROWID:<br /><br /> SQL_PC_UNKNOWN SQL_PC_NOT_PSEUDO SQL_PC_PSEUDO **Nota:**  per la massima interoperabilità, le pseudo-colonne non devono essere racchiuse tra virgolette con il carattere virgoletta identificatore restituito da **SQLGetInfo**.|  
  
 Quando l'applicazione recupera i valori per SQL_BEST_ROWID, l'applicazione può usare questi valori per riselezionare la riga all'interno dell'ambito definito. L'istruzione **Select** non restituisce né righe né una riga.  
  
 Se un'applicazione riseleziona una riga in base alla colonna o alle colonne ROWID e la riga non viene trovata, l'applicazione può presumere che la riga sia stata eliminata o che le colonne ROWID siano state modificate. Il contrario non è vero: anche se ROWID non è stato modificato, è possibile che le altre colonne della riga siano state modificate.  
  
 Le colonne restituite per il tipo di colonna SQL_BEST_ROWID sono utili per le applicazioni che devono scorrere in alto e indietro in un set di risultati per recuperare i dati più recenti da un set di righe. La colonna o le colonne di ROWID non vengono modificate mentre sono posizionate nella riga.  
  
 La colonna o le colonne di ROWID possono rimanere valide anche quando il cursore non è posizionato sulla riga; l'applicazione può determinare questo problema controllando la colonna ambito nel set di risultati.  
  
 Le colonne restituite per il tipo di colonna SQL_ROWVER sono utili per le applicazioni che richiedono la possibilità di controllare se le colonne in una determinata riga sono state aggiornate mentre la riga è stata riselezionata utilizzando ROWID. Ad esempio, dopo aver riselezionato una riga usando ROWID, l'applicazione può confrontare i valori precedenti nelle colonne SQL_ROWVER a quelli appena recuperati. Se il valore in una colonna SQL_ROWVER differisce dal valore precedente, l'applicazione può avvertire l'utente che i dati visualizzati nella visualizzazione sono stati modificati.  
  
## <a name="code-example"></a>Esempio di codice  
 Per un esempio di codice di una funzione simile, vedere [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione delle colonne di una tabella o di tabelle|[Funzione SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Recupero di una singola riga o di un blocco di dati in una direzione di sola trasmissione|[Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Recupero di un blocco di dati o scorrimento di un set di risultati|[Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Restituzione delle colonne di una chiave primaria|[SQLPrimaryKeys Function](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
