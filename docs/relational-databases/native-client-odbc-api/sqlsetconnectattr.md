---
title: SQLSetConnectAttr | Microsoft Docs
description: Informazioni sugli attributi di connessione in SQLSetConnectAttr, incluse le impostazioni per l'impostazione e i valori possibili nell'SQL Server Native Client driver ODBC.
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetConnectAttr function
ms.assetid: d21b5cf1-3724-43f7-bc96-5097df0677b4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 30893a308be9cb7634f3c0ceb755572e0e5b274b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754831"
---
# <a name="sqlsetconnectattr"></a>SQLSetConnectAttr

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ignora l'impostazione di SQL_ATTR_CONNECTION_TIMEOUT.  
  
 SQL_ATTR_TRANSLATE_LIB viene inoltre ignorato. La specifica di un'altra libreria di conversione non è supportata. Per consentire alle applicazioni di utilizzare in modo semplice un driver Microsoft ODBC per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], qualsiasi valore impostato con SQL_ATTR_TRANSLATE_LIB verrà copiato all'interno e all'esterno di un buffer da Gestione driver.  
  
 Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client implementa l'isolamento delle transazioni Repeatable Read come serializzabile.  
  
 In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è stato introdotto il supporto per un nuovo attributo di isolamento delle transazioni: SQL_COPT_SS_TXN_ISOLATION. Impostando SQL_COPT_SS_TXN_ISOLATION su SQL_TXN_SS_SNAPSHOT si indica che la transazione si verificherà con il livello di isolamento dello snapshot.  
  
> [!NOTE]  
> SQL_ATTR_TXN_ISOLATION può essere utilizzato per impostare tutti gli altri livelli di isolamento ad eccezione di SQL_TXN_SS_SNAPSHOT. Se si desidera utilizzare l'isolamento dello snapshot, è necessario impostare SQL_TXN_SS_SNAPSHOT tramite SQL_COPT_SS_TXN_ISOLATION. Tuttavia, è possibile recuperare il livello di isolamento tramite SQL_ATTR_TXN_ISOLATION oppure SQL_COPT_SS_TXN_ISOLATION.  
  
 La promozione di attributi di istruzione ODBC ad attributi di connessione può comportare conseguenze impreviste. Gli attributi di istruzione che richiedono cursori del server per l'elaborazione dei set di risultati possono essere promossi ad attributi di connessione. L'impostazione, ad esempio, dell'attributo di istruzione ODBC SQL_ATTR_CONCURRENCY su un valore più restrittivo del valore predefinito SQL_CONCUR_READ_ONLY indica al driver di utilizzare cursori dinamici per tutte le istruzioni eseguite nella connessione. L'esecuzione di una funzione di catalogo ODBC in un'istruzione nella connessione restituisce SQL_SUCCESS_WITH_INFO e un record di diagnostica indicante che il comportamento del cursore è stato modificato e impostato come di sola lettura. Un tentativo di esecuzione di un'istruzione Transact-SQL SELECT contenente una clausola COMPUTE nella stessa connessione avrà esito negativo.  
  
 Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client supporta diverse estensioni specifiche del driver ad attributi di connessione ODBC definiti in sqlncli.h. Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client può richiedere che l'attributo venga impostato prima della connessione o può ignorare l'attributo se è già impostato. Nella tabella seguente sono incluse le restrizioni.  
  
|Attributo di SQL Server|Impostazione prima o dopo la connessione al server|  
|--------------------------|----------------------------------------------|  
|SQL_COPT_SS_ANSI_NPW|Prima|  
|SQL_COPT_SS_APPLICATION_INTENT|Prima|  
|SQL_COPT_SS_ATTACHDBFILENAME|Prima|  
|SQL_COPT_SS_BCP|Prima|  
|SQL_COPT_SS_BROWSE_CONNECT|Prima|  
|SQL_COPT_SS_BROWSE_SERVER|Prima|  
|SQL_COPT_SS_CONCAT_NULL|Prima|  
|SQL_COPT_SS_CONNECTION_DEAD|After|  
|SQL_COPT_SS_ENCRYPT|Prima|  
|SQL_COPT_SS_ENLIST_IN_DTC|After|  
|SQL_COPT_SS_ENLIST_IN_XA|After|  
|SQL_COPT_SS_FALLBACK_CONNECT|Prima|  
|SQL_COPT_SS_FAILOVER_PARTNER|Prima|  
|SQL_COPT_SS_INTEGRATED_SECURITY|Prima|  
|SQL_COPT_SS_MARS_ENABLED|Prima|  
|SQL_COPT_SS_MULTISUBNET_FAILOVER|Prima|  
|SQL_COPT_SS_OLDPWD|Prima|  
|SQL_COPT_SS_PERF_DATA|After|  
|SQL_COPT_SS_PERF_DATA_LOG|After|  
|SQL_COPT_SS_PERF_DATA_LOG_NOW|After|  
|SQL_COPT_SS_PERF_QUERY|After|  
|SQL_COPT_SS_PERF_QUERY_INTERVAL|After|  
|SQL_COPT_SS_PERF_QUERY_LOG|After|  
|SQL_COPT_SS_PRESERVE_CURSORS|Prima|  
|SQL_COPT_SS_QUOTED_IDENT|Prima o dopo|  
|SQL_COPT_SS_TRANSLATE|Prima o dopo|  
|SQL_COPT_SS_TRUST_SERVER_CERTIFICATE|Prima|  
|SQL_COPT_SS_TXN_ISOLATION|Prima o dopo|  
|SQL_COPT_SS_USE_PROC_FOR_PREP|Prima o dopo|  
|SQL_COPT_SS_USER_DATA|Prima o dopo|  
|SQL_COPT_SS_WARN_ON_CP_ERROR|Prima|  
  
 L'utilizzo di un attributo di pre-connessione e del comando [!INCLUDE[tsql](../../includes/tsql-md.md)] equivalente per la stessa sessione, del database o dello stato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può generare un comportamento imprevisto. Ad esempio,  
  
```  
SQLSetConnectAttr(SQL_COPT_SS_QUOTED_IDENT, SQL_QI_ON) // turn ON via attribute  
SQLDriverConnect(...);  
SQLExecDirect("SET QUOTED_IDENTIFIER OFF") // turn OFF via Transact-SQL  
SQLSetConnectAttr(SQL_ATTR_CURRENT_CATALOG, ...) // restores to pre-connect attribute value  
```  

<a name="sqlcoptssansinpw"></a>
## <a name="sql_copt_ss_ansi_npw"></a>SQL_COPT_SS_ANSI_NPW  
 SQL_COPT_SS_ANSI_NPW abilita o disabilita l'utilizzo della gestione ISO di valori NULL nei confronti e nella concatenazione, nella spaziatura dei tipi di dati character e negli avvisi. Per ulteriori informazioni, vedere SET ANSI_NULLS, SET ANSI_PADDING, SET ANSI_WARNINGS e SET CONCAT_NULL_YIELDS_NULL.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_AD_ON|Valore predefinito. La connessione utilizza il comportamento ANSI predefinito per la gestione dei confronti di valori NULL, per il riempimento, gli avvisi e le concatenazioni NULL.|  
|SQL_AD_OFF|La connessione utilizza la gestione di valori NULL, la spaziatura dei tipi di dati character e gli avvisi definiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 Se si usa il pool di connessioni, è necessario impostare SQL_COPT_SS_ANSI_NPW nella stringa di connessione, invece che con SQLSetConnectAttr. Una volta stabilita una connessione, qualsiasi tentativo di modifica di questo attributo avrà esito negativo senza generare alcun avviso quando si utilizza un pool di connessioni.  

<a name="sqlcoptssapplicationintent"></a>
## <a name="sql_copt_ss_application_intent"></a>SQL_COPT_SS_APPLICATION_INTENT  
 Dichiara il tipo di carico di lavoro dell'applicazione in caso di connessione a un server. I valori possibili sono **ReadOnly** e **ReadWrite**. Ad esempio:  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_APPLICATION_INTENT, TEXT("Readonly"), SQL_NTS)  
```  
  
 Il valore predefinito è **ReadWrite**. Per ulteriori informazioni sul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporto di Native Client per [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] AGS, vedere [SQL Server Native client supporto per la disponibilità elevata e il ripristino di emergenza](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  

<a name="sqlcoptssattachdbfilename"></a>
## <a name="sql_copt_ss_attachdbfilename"></a>SQL_COPT_SS_ATTACHDBFILENAME  
 SQL_COPT_SS_ATTACHDBFILENAME specifica il nome del file primario di un database collegabile. Questo database viene collegato e diventa il database predefinito per la connessione. Per utilizzare SQL_COPT_SS_ATTACHDBFILENAME è necessario specificare il nome del database come valore dell'attributo di connessione SQL_ATTR_CURRENT_CATALOG o nel parametro DATABASE = di un [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md). Se il database è stato collegato in precedenza, non viene ricollegato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQLPOINTER a una stringa di caratteri|La stringa contiene il nome del file primario per il database da collegare. Includere il percorso completo del file.|  

<a name="sqlcoptssbcp"></a>
## <a name="sql_copt_ss_bcp"></a>SQL_COPT_SS_BCP  
 SQL_COPT_SS_BCP consente funzioni di copia bulk in una connessione. Per ulteriori informazioni, vedere la pagina relativa alle [funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md).  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_BCP_OFF|Valore predefinito. Le funzioni di copia bulk non sono disponibili nella connessione.|  
|SQL_BCP_ON|Le funzioni di copia bulk sono disponibili nella connessione.|  

<a name="sqlcoptssbrowseconnect"></a>
## <a name="sql_copt_ss_browse_connect"></a>SQL_COPT_SS_BROWSE_CONNECT  
 Questo attributo viene utilizzato per personalizzare il set di risultati restituito da [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md). SQL_COPT_SS_BROWSE_CONNECT abilita o disabilita la restituzione di informazioni aggiuntive da un'istanza enumerata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tali informazioni possono includere l'indicazione che il server è o non è un cluster, i nomi di diverse istanze e il numero di versione.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_MORE_INFO_NO|Valore predefinito. Restituisce un elenco di server.|  
|SQL_MORE_INFO_YES|**SQLBrowseConnect** restituisce una stringa estesa di proprietà del server.|  

<a name="sqlcoptssbrowseserver"></a>
## <a name="sql_copt_ss_browse_server"></a>SQL_COPT_SS_BROWSE_SERVER  
 Questo attributo viene utilizzato per personalizzare il set di risultati restituito da **SQLBrowseConnect**. SQL_COPT_SS_BROWSE_SERVER specifica il nome del server per il quale **SQLBrowseConnect** restituisce le informazioni.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|nomecomputer|**SQLBrowseConnect** restituisce un elenco di istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer specificato. Le barre rovesciate doppie ( \\ \\ ) non devono essere usate per il nome del server (ad esempio, invece di \\ \MyServer, è necessario usare MyServer).|  
|NULL|Valore predefinito. **SQLBrowseConnect** restituisce informazioni per tutti i server nel dominio.|  

<a name="sqlcoptssconcatnull"></a>
## <a name="sql_copt_ss_concat_null"></a>SQL_COPT_SS_CONCAT_NULL  
 SQL_COPT_SS_CONCAT_NULL abilita o disabilita l'utilizzo della gestione ISO di valori NULL per la concatenazione delle stringhe. Per ulteriori informazioni, vedere SET CONCAT_NULL_YIELDS_NULL.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_CN_ON|Valore predefinito. La connessione utilizza il comportamento predefinito ISO per la gestione di valori NULL per la concatenazione delle stringhe.|  
|SQL_CN_OFF|La connessione utilizza il comportamento definito in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la gestione dei valori NULL per la concatenazione delle stringhe.|  

<a name="sqlcoptssencrypt"></a>
## <a name="sql_copt_ss_encrypt"></a>SQL_COPT_SS_ENCRYPT  
 Controlla la crittografia per una connessione.  
  
 Per la crittografia viene utilizzato il certificato del server. Tale certificato deve essere verificato da un autorità di certificazione, a meno che l'attributo di connessione SQL_COPT_SS_TRUST_SERVER_CERTIFICATE venga impostato su SQL_TRUST_SERVER_CERTIFICATE_YES o la stringa di connessione contenga "TrustServerCertificate=yes". Se si verifica una di queste condizioni, un certificato generato e firmato dal server può essere utilizzato per crittografare la connessione se nel server non è presente alcun certificato.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_EN_ON|La connessione verrà crittografata.|  
|SQL_EN_OFF|La connessione non verrà crittografata. Questo è il valore predefinito.|  

<a name="sqlcoptssenlistindtc"></a>
## <a name="sql_copt_ss_enlist_in_dtc"></a>SQL_COPT_SS_ENLIST_IN_DTC  
 Il client chiama il metodo Microsoft Distributed Transaction Coordinator (MS DTC) OLE DB **ITransactionDispenser:: BeginTransaction** per iniziare una transazione MS DTC e creare un oggetto transazione MS DTC che rappresenta la transazione. L'applicazione chiama quindi **SQLSetConnectAttr** con l'opzione SQL_COPT_SS_ENLIST_IN_DTC per associare l'oggetto transazione alla connessione ODBC. Tutte le attività del database correlate verranno eseguite sotto la protezione della transazione MS DTC. L'applicazione chiama **SQLSetConnectAttr** con SQL_DTC_DONE per terminare l'associazione DTC della connessione.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|Oggetto DTC*|L'oggetto transazione OLE di MS DTC che specifica la transazione da esportare in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SQL_DTC_DONE|Delimita la fine di una transazione DTC.|  

<a name="sqlcoptssenlistinxa"></a>
## <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA  
 Per iniziare una transazione XA con un processore di transazioni conforme a XA (TP), il client chiama la funzione Open Group **tx_begin** . L'applicazione chiama quindi **SQLSetConnectAttr** con un parametro SQL_COPT_SS_ENLIST_IN_XA true per associare la transazione XA alla connessione ODBC. Tutte le attività del database correlate verranno eseguite sotto la protezione della transazione XA. Per terminare un'associazione XA con una connessione ODBC, il client deve chiamare **SQLSetConnectAttr** con un parametro SQL_COPT_SS_ENLIST_IN_XA false. Per ulteriori informazioni, vedere la documentazione di Microsoft Distributed Transaction Coordinator.  
  
## <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT  
 Questo attributo non è più supportato.  

<a name="sqlcoptssfailoverpartner"></a>
## <a name="sql_copt_ss_failover_partner"></a>SQL_COPT_SS_FAILOVER_PARTNER  
 Consente di specificare o recuperare il nome del partner di failover utilizzato per il mirroring del database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si tratta di una stringa di caratteri con terminazione Null che deve essere impostata prima della connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Dopo aver effettuato la connessione, l'applicazione può eseguire una query su questo attributo utilizzando [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) per determinare l'identità del partner di failover. Se il server primario non dispone di partner di failover, la proprietà restituirà una stringa vuota. In questo modo, un'applicazione smart può memorizzare nella cache il server di backup determinato più di recente, benché con tali applicazioni sia necessario tenere conto del fatto che le informazioni vengono aggiornate solo quando la connessione viene stabilita per la prima volta (o reimpostata, se in pool) e possono diventare obsolete nel caso di connessioni prolungate.  
  
 Per altre informazioni, vedere [Uso del mirroring del database](../../relational-databases/native-client/features/using-database-mirroring.md).  

<a name="sqlcoptssintegratedsecurity"></a>
## <a name="sql_copt_ss_integrated_security"></a>SQL_COPT_SS_INTEGRATED_SECURITY  
 SQL_COPT_SS_INTEGRATED_SECURITY forza l'utilizzo dell'autenticazione di Windows per la convalida dell'accesso in fase di connessione al server. Quando si usa l'autenticazione di Windows, il driver ignora i valori dell'identificatore utente e della password forniti come parte dell'elaborazione di **SQLConnect**, [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)o [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) .  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_IS_OFF|Valore predefinito. L'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene utilizzata per convalidare ID utente e password al momento dell'accesso.|  
|SQL_IS_ON|Viene utilizzata la modalità di autenticazione di Windows per convalidare i diritti di accesso di un utente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  

<a name="sqlcoptssmarsenabled"></a>
## <a name="sql_copt_ss_mars_enabled"></a>SQL_COPT_SS_MARS_ENABLED  
 Questo attributo abilita o disabilita MARS (Multiple Active Result Set). Per impostazione predefinita, MARS è disabilitato. Questo attributo deve essere impostato prima di stabilire una connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Una volta aperta la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il servizio MARS rimarrà abilitato o disabilitato per tutta la durata della connessione.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_MARS_ENABLED_NO|Valore predefinito. MARS è disabilitato.|  
|SQL_MARS_ENABLED_YES|MARS è abilitato.|  
  
 Per altre informazioni su MARS, vedere [uso di più set di risultati attivi &#40;&#41;Mars ](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  

<a name="sqlcoptssmultisubnetfailover"></a>
## <a name="sql_copt_ss_multisubnet_failover"></a>SQL_COPT_SS_MULTISUBNET_FAILOVER  
 Se l'applicazione si connette a un gruppo di disponibilità [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] in subnet diverse, questa proprietà di connessione consente di configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per garantire una maggiore velocità di rilevamento e connessione al server attualmente attivo. Ad esempio:  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_MULTISUBNET_FAILOVER, SQL_IS_ON, SQL_IS_INTEGER)  
```  
  
 Per ulteriori informazioni sul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporto di Native Client per [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] AGS, vedere [SQL Server Native client supporto per la disponibilità elevata e il ripristino di emergenza](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_IS_ON|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client consente una riconnessione più veloce in caso di failover.|  
|SQL_IS_OFF|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client non consente una riconnessione più veloce in caso di failover.|  

<a name="sqlcoptssoldpwd"></a>
## <a name="sql_copt_ss_oldpwd"></a>SQL_COPT_SS_OLDPWD  
 In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è stata introdotta la scadenza della password per l'autenticazione di SQL Server. L'attributo SQL_COPT_SS_OLDPWD è stato aggiunto per consentire al client di fornire sia la vecchia password che la nuova per la connessione. Quando questa proprietà è impostata, il provider non utilizzerà il pool di connessioni per la prima connessione o per le connessioni successive, in quanto la stringa di connessione conterrà la password precedente che è stata modificata.  
  
 Per altre informazioni, vedere [Modifica delle password a livello di programmazione](../../relational-databases/native-client/features/changing-passwords-programmatically.md).  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_COPT_SS_OLD_PASSWORD|SQLPOINTER a una stringa di caratteri contenente la password precedente. Questo valore è di sola scrittura e deve essere impostato prima della connessione al server.|  

<a name="sqlcoptssperfdata"></a>
## <a name="sql_copt_ss_perf_data"></a>SQL_COPT_SS_PERF_DATA  
 SQL_COPT_SS_PERF_DATA avvia o arresta la registrazione dei dati relativi alle prestazioni. Il nome del file di log deve essere impostato prima di avviare la registrazione dei dati. Vedere SQL_COPT_SS_PERF_DATA_LOG di seguito.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_PERF_START|Avvia il driver che esegue il campionamento dei dati relativi alle prestazioni.|  
|SQL_PERF_STOP|Arresta il campionamento dei dati relativi alle prestazioni da parte dei contatori.|  
  
 Per ulteriori informazioni, vedere [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  

<a name="sqlcoptssperfdatalog"></a>
## <a name="sql_copt_ss_perf_data_log"></a>SQL_COPT_SS_PERF_DATA_LOG  
 SQL_COPT_SS_PERF_DATA_LOG assegna il nome del file di log utilizzato per registrare i dati relativi alle prestazioni. Il nome del file di log è una stringa ANSI o Unicode con terminazione Null che dipende dalla compilazione dell'applicazione. L'argomento *StringLength* deve essere SQL_NTS.  

<a name="sqlcoptssperfdatalognow"></a>
## <a name="sql_copt_ss_perf_data_log_now"></a>SQL_COPT_SS_PERF_DATA_LOG_NOW  
 SQL_COPT_SS_PERF_DATA_LOG_NOW indica al driver di scrivere una voce del log delle statistiche sul disco. L'argomento *StringLength* deve essere SQL_NTS.  

<a name="sqlcoptssperfquery"></a>
## <a name="sql_copt_ss_perf_query"></a>SQL_COPT_SS_PERF_QUERY  
 SQL_COPT_SS_PERF_QUERY avvia o arresta la registrazione di query con esecuzione prolungata. Il nome del file di log delle query deve essere specificato prima di avviare la registrazione. L'applicazione può definire l'esecuzione prolungata impostando l'intervallo per la registrazione.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_PERF_START|Avvia la registrazione di query con esecuzione prolungata.|  
|SQL_PERF_STOP|Arresta la registrazione di query con esecuzione prolungata.|  
  
 Per ulteriori informazioni, vedere [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  

<a name="sqlcoptssperfqueryinterval"></a>
## <a name="sql_copt_ss_perf_query_interval"></a>SQL_COPT_SS_PERF_QUERY_INTERVAL  
 SQL_COPT_SS_PERF_QUERY_INTERVAL imposta la soglia di registrazione delle query in millisecondi. Le query che non vengono risolte entro la soglia vengono registrate nel file di log delle query con esecuzione prolungata. Non sussiste alcun limite per la soglia massima di query. Un valore soglia pari a zero comporta la registrazione di tutte le query.  

<a name="sqlcoptssperfquerylog"></a>
## <a name="sql_copt_ss_perf_query_log"></a>SQL_COPT_SS_PERF_QUERY_LOG  
 SQL_COPT_SS_PERF_QUERY_LOG assegna il nome di un file di log per la registrazione dei dati delle query con esecuzione prolungata. Il nome del file di log è una stringa ANSI o Unicode con terminazione Null che dipende dalla compilazione dell'applicazione. L'argomento *StringLength* deve essere SQL_NTS o la lunghezza della stringa in byte.  

<a name="sqlcoptsspreservecursors"></a>
## <a name="sql_copt_ss_preserve_cursors"></a>SQL_COPT_SS_PRESERVE_CURSORS  
 Questo attributo consente di eseguire query per determinare se la connessione manterrà i cursori durante il commit o il rollback di una transazione e di impostare o meno tale comportamento. L'impostazione è SQL_PC_ON o SQL_PC_OFF. Il valore predefinito è SQL_PC_OFF. Questa impostazione determina se il driver chiuderà o meno i cursori quando si chiama [SQLEndTran](../../relational-databases/native-client-odbc-api/sqlendtran.md) (o SQLTransact).  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_PC_OFF|Valore predefinito. I cursori vengono chiusi quando viene eseguito il commit o il rollback della transazione utilizzando **SQLEndTran**.|  
|SQL_PC_ON|I cursori non vengono chiusi quando viene eseguito il commit o il rollback della transazione utilizzando **SQLEndTran**, tranne quando si utilizza un cursore statico o keyset in modalità asincrona. Se si esegue un rollback quando il popolamento del cursore non è completo, il cursore viene chiuso.|  

<a name="sqlcoptssquotedident"></a>
## <a name="sql_copt_ss_quoted_ident"></a>SQL_COPT_SS_QUOTED_IDENT  
 SQL_COPT_SS_QUOTED_IDENT consente la presenza di identificatori tra virgolette in istruzioni ODBC e Transact-SQL eseguite nella connessione. Tramite l'immissione di identificatori tra virgolette, il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client consente la presenza di nomi di oggetto altrimenti non validi, ad esempio "Tabella personale", che contengono un carattere di spazio nell'identificatore. Per ulteriori informazioni, vedere SET QUOTED_IDENTIFIER.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_QI_OFF|La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non consente la presenza di identificatori tra virgolette in istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite.|  
|SQL_QI_ON|Valore predefinito. La connessione consente la presenza di identificatori tra virgolette in istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite.|  

<a name="sqlcoptsstranslate"></a>
## <a name="sql_copt_ss_translate"></a>SQL_COPT_SS_TRANSLATE  
 SQL_COPT_SS_TRANSLATE fa in modo che il driver converta caratteri tra le tabelle codici del client e del server durante lo scambio di dati MBCS. L'attributo influiscono solo sui dati archiviati nelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonne **char**, **varchar** e **Text** .  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_XL_OFF|Il driver non converte i caratteri da una tabella codici all'altra nei dati di tipo carattere scambiati tra il client e il server.|  
|SQL_XL_ON|Valore predefinito. Il driver converte caratteri da una tabella codici a un'altra in dati di tipo carattere scambiati tra il client e il server. Il driver configura automaticamente la conversione dei caratteri, determinando la tabella codici installata nel server e quella utilizzata dal client.|  

<a name="sqlcoptsstrustservercertificate"></a>
## <a name="sql_copt_ss_trust_server_certificate"></a>SQL_COPT_SS_TRUST_SERVER_CERTIFICATE  
 SQL_COPT_SS_TRUST_SERVER_CERTIFICATE fa in modo che il driver abiliti o disabiliti la convalida del certificato quando si utilizza la crittografia. Questo attributo è un valore di lettura/scrittura, ma non ha alcun effetto se lo si imposta una volta stabilita una connessione.  
  
 Le applicazioni client possono eseguire query su questa proprietà dopo l'apertura di una connessione per determinare le effettive impostazioni di crittografia e convalida in uso.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_TRUST_SERVER_CERTIFICATE_NO|Valore predefinito. La crittografia senza convalida del certificato non è abilitata.|  
|SQL_TRUST_SERVER_CERTIFICATE_YES|La crittografia senza convalida del certificato è abilitata.|  

<a name="sqlcoptsstxnisolation"></a>
## <a name="sql_copt_ss_txn_isolation"></a>SQL_COPT_SS_TXN_ISOLATION  
 SQL_COPT_SS_TXN_ISOLATION imposta l'attributo di isolamento dello snapshot specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è possibile impostare l'isolamento dello snapshot utilizzando SQL_ATTR_TXN_ISOLATION, in quanto il valore è specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tale valore può tuttavia essere recuperato utilizzando SQL_ATTR_TXN_ISOLATION o SQL_COPT_SS_TXN_ISOLATION.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_TXN_SS_SNAPSHOT|Indica che non è possibile visualizzare da una transazione le modifiche apportate in altre transazioni e che ciò non è possibile neanche ripetendo la query.|  
  
 Per ulteriori informazioni sull'isolamento dello snapshot, vedere [utilizzo dell'isolamento dello snapshot](../../relational-databases/native-client/features/working-with-snapshot-isolation.md).  
  
## <a name="sql_copt_ss_use_proc_for_prep"></a>SQL_COPT_SS_USE_PROC_FOR_PREP

 Questo attributo non è più supportato.  

<a name="sqlcoptssuserdata"></a>
## <a name="sql_copt_ss_user_data"></a>SQL_COPT_SS_USER_DATA  
 SQL_COPT_SS_USER_DATA consente di impostare il puntatore ai dati dell'utente. I dati dell'utente vengono registrati nella memoria di proprietà del client per ogni connessione.  
  
 Per ulteriori informazioni, vedere [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  

<a name="sqlcoptsswarnoncperror"></a>
## <a name="sql_copt_ss_warn_on_cp_error"></a>SQL_COPT_SS_WARN_ON_CP_ERROR  
 Questo attributo determina la visualizzazione o meno di un avviso in caso di perdita di dati durante la conversione di una tabella codici. Si applica solo ai dati provenienti dal server.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SQL_WARN_YES|Genera avvisi quando si verifica una perdita di dati durante la conversione della tabella codici.|  
|SQL_WARN_NO|(Impostazione predefinita) Non genera avvisi quando si verifica una perdita di dati durante la conversione della tabella codici.|  
  
## <a name="sqlsetconnectattr-support-for-service-principal-names-spns"></a>Supporto di SQLSetConnectAttr per nomi SPN (Service Principal Name)

 SQLSetConnectAttr può essere utilizzato per impostare il valore dei nuovi attributi di connessione SQL_COPT_SS_SERVER_SPN e SQL_COPT_SS_FAILOVER_PARTNER_SPN. Tali attributi non possono essere impostati quando una connessione è aperta; se si tenta di impostare questi attributi quando una connessione è aperta, viene restituito l'errore HY011 con il messaggio "Operazione correntemente non valida". Per impostare questi valori, è anche possibile usare SQLSetConnectOption.  
  
 Per ulteriori informazioni sui nomi SPN, vedere [nomi dell'entità servizio &#40;spn&#41; nelle connessioni Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md).  

<a name="sqlcoptssconnectiondead"></a>
## <a name="sql_copt_ss_connection_dead"></a>SQL_COPT_SS_CONNECTION_DEAD  
 Attributo di sola lettura.  
  
 Per ulteriori informazioni su SQL_COPT_SS_CONNECTION_DEAD, vedere [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) e [connessione a un'origine dati &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-communication/connecting-to-a-data-source-odbc.md).  
  
## <a name="example"></a>Esempio

 In questo esempio vengono registrati i dati relativi alle prestazioni.  
  
```  
SQLPERF*     pSQLPERF;  
SQLINTEGER   nValue;  
  
// See if you are already logging. SQLPERF* will be NULL if not.  
SQLGetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA, &pSQLPERF,  
    sizeof(SQLPERF*), &nValue);  
  
if (pSQLPERF == NULL)  
    {  
    // Set the performance log file name.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG,  
        (SQLPOINTER) "\\My LogDirectory\\MyServerLog.txt", SQL_NTS);  
  
    // Start logging...  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
        (SQLPOINTER) SQL_PERF_START, SQL_IS_INTEGER);  
    }  
else  
    {  
    // Take a snapshot now so that your performance statistics are discernible.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
    }  
  
    // ...perform some action...  
  
// ...take a performance data snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
    // ...perform more actions...  
  
// ...take another snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
// ...and disable logging.  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
    (SQLPOINTER) SQL_PERF_STOP, SQL_IS_INTEGER);  
  
// Continue on...  
```  
  
## <a name="see-also"></a>Vedere anche

 [Funzione SQLSetConnectAttr](../../odbc/reference/syntax/sqlsetconnectattr-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)   
 [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md)   
 [SQLPrepare (funzione)](../../odbc/reference/syntax/sqlprepare-function.md)   
 [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md)  
  
