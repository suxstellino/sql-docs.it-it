---
description: SQLSetStmtAttr
title: SQLSetStmtAttr | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetStmtAttr function
ms.assetid: 799c80fd-c561-4912-8562-9229076dfd19
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9005450f9dcb700a08b70820e75ddc8d3f83d90
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754861"
---
# <a name="sqlsetstmtattr"></a>SQLSetStmtAttr
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client non supporta il modello di cursore misto (keyset/dinamico). I tentativi di impostare la dimensione del keyset utilizzando SQL_ATTR_KEYSET_SIZE non riescono se il set di valori non è uguale a 0.  
  
 L'applicazione imposta SQL_ATTR_ROW_ARRAY_SIZE in tutte le istruzioni per dichiarare il numero di righe restituite in una chiamata di funzione **SQLFetch** o [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) . Nelle istruzioni che indicano un cursore del server il driver utilizza SQL_ATTR_ROW_ARRAY_SIZE per determinare la dimensione del blocco di righe che il server genera per soddisfare una richiesta di recupero dal cursore. Nelle dimensioni del blocco di un cursore dinamico, l'ordinamento e l'appartenenza delle righe sono fissi se il livello di isolamento della transazione è sufficiente per assicurare letture ripetibili delle transazioni di cui è stato eseguito il commit. Il cursore è completamente dinamico al di fuori del blocco indicato da questo valore. Le dimensioni del blocco del cursore del server sono completamente dinamiche e possono essere modificate in qualsiasi momento durante l'elaborazione del recupero.  
  
## <a name="sqlsetstmtattr-and-table-valued-parameters"></a>SQLSetStmtAttr e parametri con valori di tabella  
 SQLSetStmtAttr può essere usato per impostare SQL_SOPT_SS_PARAM_FOCUS nel descrittore del parametro dell'applicazione (APD) prima di accedere ai campi del descrittore per le colonne dei parametri con valori di tabella.  
  
 Se viene effettuato un tentativo di impostare SQL_SOPT_SS_PARAM_FOCUS sull'ordinale di un parametro che non è un parametro con valori di tabella, SQLSetStmtAttr restituisce SQL_ERROR e viene creato un record di diagnostica con SQLSTATE = HY024 e il messaggio "valore attributo non valido". SQL_SOPT_SS_PARAM_FOCUS non viene modificato al momento della restituzione di SQL_ERROR.  
  
 L'impostazione di SQL_SOPT_SS_PARAM_FOCUS su 0 ripristina l'accesso ai record del descrittore per i parametri.  
  
 SQLSetStmtAttr può essere usato anche per impostare SQL_SOPT_SS_NAME_SCOPE. Per ulteriori informazioni, vedere la sezione SQL_SOPT_SS_NAME_SCOPE più avanti in questo argomento.  
  
 Per ulteriori informazioni, vedere [metadati dei parametri con valori di tabella per le istruzioni preparate](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md).  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlsetstmtattr-support-for-sparse-columns"></a>Supporto di SQLSetStmtAttr per colonne di tipo sparse  
 SQLSetStmtAttr può essere utilizzato per impostare SQL_SOPT_SS_NAME_SCOPE. Per ulteriori informazioni, vedere la sezione SQL_SOPT_SS_NAME_SCOPE più avanti in questo argomento. Per ulteriori informazioni sulle colonne di tipo sparse, vedere [supporto di colonne di tipo sparse &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="statement-attributes"></a>Attributi di istruzione  
 Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client supporta anche gli attributi di istruzione seguenti specifici del driver.  
  
### <a name="sql_sopt_ss_cursor_options"></a>SQL_SOPT_SS_CURSOR_OPTIONS  
 L'attributo SQL_SOPT_SS_CURSOR specifica se il driver utilizzerà opzioni delle prestazioni specifiche del driver sui cursori. [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) non è consentito quando queste opzioni sono impostate. L'impostazione predefinita è SQL_CO_OFF. Il valore *ValuePtr* è di tipo SQLLEN.  
  
|Valore *ValuePtr*|Descrizione|  
|----------------------|-----------------|  
|SQL_CO_OFF|Valore predefinito. Disabilita i cursori fast-only, di sola lettura e di recupero automatico, Abilita **SQLGetData** su cursori di sola lettura e di sola lettura. Quando SQL_SOPT_SS_CURSOR_OPTIONS è impostato su SQL_CO_OFF, il tipo di cursore non cambia. Ciò significa che il cursore fast forward-only resterà tale. Per modificare il tipo di cursore, l'applicazione deve ora impostare un tipo di cursore diverso utilizzando **SQLSetStmtAttr**/SQL_ATTR_CURSOR_TYPE.|  
|SQL_CO_FFO|Consente cursori fast-only di sola lettura, Disabilita **SQLGetData** sui cursori di sola lettura di sola lettura.|  
|SQL_CO_AF|Abilita l'opzione per il recupero automatico su qualsiasi tipo di cursore. Quando questa opzione è impostata per un handle di istruzione, **SQLExecute** o **SQLExecDirect** genererà un **SQLFetchScroll** implicito (SQL_FIRST). Il cursore è aperto e il primo batch di righe viene restituito in un solo round trip al server.|  
|SQL_CO_FFO_AF|Abilita i cursori fast forward-only con l'opzione di recupero automatico. Produce gli stessi risultati della specifica contemporanea di SQL_CO_AF e SQL_CO_FFO.|  
  
 Quando queste opzioni sono impostate, il server chiude automaticamente il cursore quando rileva che l'ultima riga è stata recuperata. L'applicazione deve comunque chiamare [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) (SQL_CLOSE) o [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), ma il driver non deve inviare la notifica di chiusura al server.  
  
 Se l'elenco di selezione contiene una colonna di tipo **Text**, **ntext** o **Image** , il cursore fast-only viene convertito in un cursore dinamico e **SQLGetData** è consentito.  
  
### <a name="sql_sopt_ss_defer_prepare"></a>SQL_SOPT_SS_DEFER_PREPARE  
 L'attributo SQL_SOPT_SS_DEFER_PREPARE determina se l'istruzione viene preparata immediatamente o rinviata fino a quando non viene eseguito **SQLExecute**, [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) o [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md) . In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 e versioni precedenti questa proprietà viene ignorata (nessuna preparazione posticipata). Il valore *ValuePtr* è di tipo SQLLEN.  
  
|Valore *ValuePtr*|Descrizione|  
|----------------------|-----------------|  
|SQL_DP_ON|Valore predefinito. Dopo la chiamata della [funzione SQLPrepare](../../odbc/reference/syntax/sqlprepare-function.md), la preparazione dell'istruzione viene posticipata fino alla chiamata di **SQLExecute** o all'esecuzione dell'operazione di metaproprietà (**SQLDescribeCol** o **SQLDescribeParam**).|  
|SQL_DP_OFF|L'istruzione viene preparata non appena viene eseguito **SQLPrepare** .|  
  
### <a name="sql_sopt_ss_regionalize"></a>SQL_SOPT_SS_REGIONALIZE  
 L'attributo SQL_SOPT_SS_REGIONALIZE viene utilizzato per determinare la conversione dei dati a livello di istruzione. L'attributo fa sì che il driver rispetti l'impostazione locale del client durante la conversione dei valori di data, ora e valuta nelle stringhe di caratteri. La conversione viene effettuata dai tipi di dati nativi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo in stringhe di caratteri.  
  
 Il valore *ValuePtr* è di tipo SQLLEN.  
  
|Valore *ValuePtr*|Descrizione|  
|----------------------|-----------------|  
|SQL_RE_OFF|Valore predefinito. Il driver non converte i dati di tipo data, ora e valuta in stringhe di caratteri mediante l'impostazione locale del client.|  
|SQL_RE_ON|Il driver utilizza l'impostazione locale del client durante la conversione dei dati di tipo data, ora e valuta in stringhe di caratteri.|  
  
 Ai dati di tipo valuta, numerico, data e ora vengono applicate le impostazioni di conversione internazionali. L'impostazione di conversione è applicabile solo alle conversioni di output quando i valori di valuta, data, ora o numerici vengono convertiti in stringhe di caratteri.  
  
> [!NOTE]  
>  Quando l'opzione di istruzione SQL_SOPT_SS_REGIONALIZE è impostata, il driver utilizza le impostazioni locali del Registro di sistema per l'utente corrente. Il driver non rispetta le impostazioni locali del thread corrente se l'applicazione la imposta, ad esempio, chiamando **SetThreadLocale**.  
  
 La modifica del comportamento internazionale di un'origine dati può generare un errore nell'applicazione. Un'applicazione che analizza le stringhe relative alla data e ne prevede la visualizzazione in base a quanto definito da ODBC, potrebbe essere influenzata negativamente dalla modifica di questo valore.  
  
### <a name="sql_sopt_ss_textptr_logging"></a>SQL_SOPT_SS_TEXTPTR_LOGGING  
 L'attributo SQL_SOPT_SS_TEXTPTR_LOGGING attiva la registrazione delle operazioni sulle colonne contenenti dati di **testo** o di **immagine** . Il valore *ValuePtr* è di tipo SQLLEN.  
  
|Valore *ValuePtr*|Descrizione|  
|----------------------|-----------------|  
|SQL_TL_OFF|Disabilita la registrazione di operazioni eseguite su dati di **testo** e di **immagine** .|  
|SQL_TL_ON|Valore predefinito. Abilita la registrazione delle operazioni eseguite sui dati di **testo** e di **immagine** .|  
  
### <a name="sql_sopt_ss_hidden_columns"></a>SQL_SOPT_SS_HIDDEN_COLUMNS  
 L'attributo SQL_SOPT_SS_HIDDEN_COLUMNS espone nel set di risultati le colonne nascoste in un'istruzione SELECT FOR BROWSE di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per impostazione predefinita, il driver non espone queste colonne. Il valore *ValuePtr* è di tipo SQLLEN.  
  
|Valore *ValuePtr*|Descrizione|  
|----------------------|-----------------|  
|SQL_HC_OFF|Valore predefinito. Le colonne FOR BROWSE sono nascoste dal set di risultati.|  
|SQL_HC_ON|Espone colonne FOR BROWSE.|  
  
### <a name="sql_sopt_ss_querynotification_msgtext"></a>SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT  
 L'attributo SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT restituisce il testo del messaggio per la richiesta di notifica di query.  
  
### <a name="sql_sopt_ss_querynotification_options"></a>SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS  
 L'attributo SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS specifica le opzioni utilizzate per la richiesta di notifica di query. Tali opzioni vengono specificate in una stringa con la sintassi `name=value` come indicato di seguito. L'applicazione è responsabile della creazione del servizio e della lettura delle notifiche all'esterno della coda.  
  
 La sintassi delle opzioni delle notifiche delle query è la seguente:  
  
 `service=<service-name>[;(local database=<database>|broker instance=<broker instance>)]`  
  
 Ad esempio:  
  
 `service=mySSBService;local database=mydb`  
  
### <a name="sql_sopt_ss_querynotification_timeout"></a>SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT  
 L'attributo SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT specifica per quanti secondi la notifica di query deve rimanere attiva. Il valore predefinito è 432000 secondi (5 giorni). Il valore *ValuePtr* è di tipo SQLLEN.  
  
### <a name="sql_sopt_ss_param_focus"></a>SQL_SOPT_SS_PARAM_FOCUS  
 L'attributo SQL_SOPT_SS_PARAM_FOCUS specifica lo stato attivo per le chiamate successive a SQLBindParameter, SQLGetDescField, SQLSetDescField, SQLGetDescRec e SQLSetDescRec.  
  
 Il tipo per SQL_SOPT_SS_PARAM_FOCUS è SQLULEN.  
  
 L'impostazione predefinita è 0 e indica che queste chiamate riguardano i parametri che corrispondono a marcatori di parametro nell'istruzione SQL. Se l'impostazione corrisponde al numero di un parametro con valori di tabella, queste chiamate riguardano le colonne del parametro con valori di tabella. Se l'impostazione corrisponde a un valore diverso dal numero un parametro con valori di tabella, queste chiamate restituiscono l'errore IM020: "Lo stato attivo del parametro non fa riferimento a un parametro con valori di tabella".  
  
### <a name="sql_sopt_ss_name_scope"></a>SQL_SOPT_SS_NAME_SCOPE  
 L'attributo SQL_SOPT_SS_NAME_SCOPE specifica l'ambito del nome per le chiamate di funzione di catalogo successive. Il set di risultati restituito da SQLColumns dipende dall'impostazione di SQL_SOPT_SS_NAME_SCOPE.  
  
 Il tipo per SQL_SOPT_SS_NAME_SCOPE è SQLULEN.  
  
|Valore *ValuePtr*|Descrizione|  
|----------------------|-----------------|  
|SQL_SS_NAME_SCOPE_TABLE|Valore predefinito.<br /><br /> In caso di utilizzo di parametri con valori di tabella, indica che è necessario che vengano restituiti i metadati per le tabelle effettive.<br /><br /> Quando si utilizza la funzionalità colonne di tipo sparse, SQLColumns restituirà solo le colonne che non sono membri del **column_set** di tipo sparse.|  
|SQL_SS_NAME_SCOPE_TABLE_TYPE|Indica che l'applicazione richiede metadati per un tipo di tabella, anziché una tabella effettiva. Le funzioni di catalogo devono restituire metadati per i tipi di tabella. L'applicazione passa quindi il TYPE_NAME del parametro con valori di tabella come parametro *TableName* .|  
|SQL_SS_NAME_SCOPE_EXTENDED|Quando si utilizza la funzionalità colonne di tipo sparse, SQLColumns restituisce tutte le colonne, indipendentemente dall'appartenenza **column_set** .|  
|SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET|Quando si utilizza la funzionalità colonne di tipo sparse, SQLColumns restituisce solo le colonne che sono membri del **column_set** di tipo sparse.|  
|SQL_SS_NAME_SCOPE_DEFAULT|Equivale a SQL_SS_NAME_SCOPE_TABLE.|  
  
 Per identificare il catalogo e lo schema per il parametro con valori di tabella, SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMA_NAME vengono utilizzati rispettivamente con i parametri *CatalogName* e *SchemaName* . Quando un'applicazione ha completato il recupero dei metadati per i parametri con valori di tabella, deve impostare nuovamente SQL_SOPT_SS_NAME_SCOPE sul valore predefinito di SQL_SS_NAME_SCOPE_TABLE.  
  
 Quando SQL_SOPT_SS_NAME_SCOPE è impostato su SQL_SS_NAME_SCOPE_TABLE, le query ai server collegati non riescono. Le chiamate a SQLColumns o SQLPrimaryKeys con un catalogo contenente un componente server avranno esito negativo.  
  
 Se si tenta di impostare SQL_SOPT_SS_NAME_SCOPE su un valore non valido, viene restituito SQL_ERROR e un record di diagnostica viene generato con SQLSTATE HY024 e il messaggio "Valore attributo non valido".  
  
 Se una funzione di catalogo, ovvero SQLTables, SQLColumns o SQLPrimaryKeys, viene chiamata quando SQL_SOPT_SS_NAME_SCOPE dispone di un valore diverso da SQL_SS_NAME_SCOPE_TABLE, viene restituito SQL_ERROR. Viene generato un record di diagnostica con SQLSTATE HY010 e il messaggio "Errore nella sequenza della funzione (SQL_SOPT_SS_NAME_SCOPE non è impostato su SQL_SS_NAME_SCOPE_TABLE)".  
  
## <a name="see-also"></a>Vedere anche  
 [SQLGetStmtAttr (funzione)](../../odbc/reference/syntax/sqlgetstmtattr-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
