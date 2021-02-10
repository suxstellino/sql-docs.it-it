---
description: Panoramica del provider Microsoft OLE DB per SQL Server
title: Provider di OLE DB Microsoft per SQL Server | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB provider for SQL Server
- OLE DB provider for SQL Server [ADO]
- SQLOLEDB [ADO]
ms.assetid: 99bc40c4-9181-4ca1-a06f-9a1a914a0b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b6a62e8946415cc9c1e869a173de43b98ba14f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029255"
---
# <a name="microsoft-ole-db-provider-for-sql-server-overview"></a>Panoramica del provider Microsoft OLE DB per SQL Server
Il provider di OLE DB Microsoft per SQL Server, SQLOLEDB, consente a ADO di accedere a Microsoft SQL Server.

> [!IMPORTANT]
> Il provider di OLE DB Microsoft per SQL Server (SQLOLEDB) rimane deprecato e non è consigliabile utilizzarlo per un nuovo lavoro di sviluppo. Usare invece il nuovo [Microsoft OLE DB Driver per SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) che verrà aggiornato con le funzionalità server più recenti.

## <a name="connection-string-parameters"></a>Parametri della stringa di connessione
 Per connettersi a questo provider, impostare l'argomento del *provider* sulla proprietà [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) su:

```vb
SQLOLEDB
```

 Questo valore può essere impostato o letto anche usando la proprietà del [provider](../../reference/ado-api/provider-property-ado.md) .

## <a name="typical-connection-string"></a>Stringa di connessione tipica
 Una stringa di connessione tipica per questo provider è la seguente:

```vb
"Provider=SQLOLEDB;Data Source=serverName;"
Initial Catalog=databaseName;
User ID=MyUserID;Password=MyPassword;"
```

 La stringa è costituita dalle parole chiave seguenti:

|Parola chiave|Descrizione|
|-------------|-----------------|
|**Provider**|Specifica il provider di OLE DB per SQL Server.|
|**Origine dati** o **Server**|Specifica il nome di un server.|
|**Catalogo iniziale** o **database**|Specifica il nome di un database nel server.|
|**ID utente** o **UID**|Specifica il nome utente (per l'autenticazione SQL Server).|
|**Password** o **pwd**|Specifica la password utente (per l'autenticazione SQL Server).|

> [!NOTE]
>  Se ci si connette a un provider dell'origine dati che supporta l'autenticazione di Windows, è necessario specificare **Trusted_Connection = Yes** o **Integrated Security = SSPI** anziché le informazioni relative a ID utente e password nella stringa di connessione.

## <a name="provider-specific-connection-parameters"></a>Parametri di connessione Provider-Specific
 Il provider supporta diversi parametri di connessione specifici del provider, oltre a quelli definiti da ADO. Come per le proprietà di connessione ADO, queste proprietà specifiche del provider possono essere impostate tramite la raccolta [Properties](../../reference/ado-api/properties-collection-ado.md) di una [connessione](../../reference/ado-api/connection-object-ado.md) oppure possono essere impostate come parte di **ConnectionString**.

|Parametro|Descrizione|
|---------------|-----------------|
|Trusted_Connection|Indica la modalità di autenticazione utente. Questo può essere impostato su **Yes** o **No**. Il valore predefinito è **No**. Se questa proprietà è impostata su **Sì**, SQLOLEDB usa la modalità di autenticazione di Microsoft Windows NT per autorizzare l'accesso utente al database SQL Server specificato dai valori della proprietà **location** e [DataSource](../../reference/ado-api/datasource-property-ado.md) . Se questa proprietà è impostata su **No**, SQLOLEDB utilizza la modalità mista per autorizzare l'accesso utente al database SQL Server. Il SQL Server account di accesso e la password sono specificati nelle proprietà **ID utente** e **password** .|
|Lingua corrente|Indica un nome di lingua SQL Server. Identifica la lingua utilizzata per la selezione e la formattazione dei messaggi di sistema. Il linguaggio deve essere installato nel SQL Server. in caso contrario, l'apertura della connessione avrà esito negativo.|
|Indirizzo di rete|Indica l'indirizzo di rete dell'SQL Server specificato dalla proprietà **location** .|
|Libreria di rete|Indica il nome della libreria di rete (DLL) utilizzata per comunicare con il SQL Server. Il nome non deve include il percorso o l'estensione di file DLL. Il valore predefinito è fornito dalla configurazione del client SQL Server.|
|Usare la procedura per la preparazione|Determina se SQL Server crea stored procedure temporanee quando i comandi vengono preparati (mediante la proprietà **preparata** ).|
|Conversione automatica|Indica se i caratteri OEM/ANSI vengono convertiti. Questa proprietà può essere impostata su **true** o **false**. Il valore predefinito è **True**. Se questa proprietà è impostata su **true**, SQLOLEDB esegue la conversione di caratteri OEM/ANSI quando le stringhe di caratteri multibyte vengono recuperate da o inviate a, il SQL Server. Se questa proprietà è impostata su **false**, SQLOLEDB non esegue la conversione di caratteri OEM/ANSI nei dati stringa di caratteri multibyte.|
|Dimensione pacchetti|Indica una dimensione del pacchetto di rete in byte. Il valore della proprietà dimensioni pacchetto deve essere compreso tra 512 e 32767. Le dimensioni predefinite del pacchetto di rete SQLOLEDB sono pari a 4096.|
|Nome dell'applicazione|Indica il nome dell'applicazione client.|
|ID workstation|Stringa che identifica la workstation.|

## <a name="command-object-usage"></a>Utilizzo oggetto comando
 SQLOLEDB accetta un amalgama di ODBC, ANSI e Transact-SQL specifico di SQL Server come sintassi valida. L'istruzione SQL seguente, ad esempio, utilizza una sequenza di escape ODBC SQL per specificare la funzione per i valori stringa LCASE:

```sql
SELECT customerid={fn LCASE(CustomerID)} FROM Customers

```

 LCASE restituisce una stringa di caratteri, convertendo tutti i caratteri maiuscoli nei rispettivi equivalenti minuscoli. La funzione stringa SQL ANSI LOWER esegue la stessa operazione, pertanto l'istruzione SQL seguente è un equivalente ANSI dell'istruzione ODBC presentata in precedenza:

```sql
SELECT customerid=LOWER(CustomerID) FROM Customers

```

 SQLOLEDB elabora correttamente una delle forme dell'istruzione quando viene specificato come testo per un comando.

## <a name="stored-procedures"></a>Stored procedure
 Quando si esegue un SQL Server stored procedure utilizzando un comando SQLOLEDB, utilizzare la sequenza di escape ODBC RPC Call nel testo del comando. SQLOLEDB usa quindi il meccanismo RPC (Remote Procedure Call) di SQL Server per ottimizzare l'elaborazione dei comandi. Ad esempio, l'istruzione SQL ODBC seguente rappresenta il testo del comando preferito sul modulo Transact-SQL:

## <a name="odbc-sql"></a>ODBC SQL

```vb
{call SalesByCategory('Produce', '1995')}

```

## <a name="transact-sql"></a>Transact-SQL

```sql
EXECUTE SalesByCategory 'Produce', '1995'

```

## <a name="sql-server-features"></a>Funzionalità di SQL Server
 Con SQL Server, ADO può utilizzare XML per l'input del **comando** e recuperare i risultati in formato flusso XML anziché negli oggetti **Recordset** . Per altre informazioni, vedere [uso dei flussi per l'input del comando](../data/command-streams.md) e [recupero di set di risultati nei flussi](../data/retrieving-resultsets-into-streams.md).

### <a name="accessing-sql_variant-data-using-mdac-27-mdac-28-or-windows-dac-60"></a>Accesso ai dati di sql_variant utilizzando MDAC 2,7, MDAC 2,8 o Windows DAC 6,0
 Microsoft SQL Server dispone di un tipo di dati denominato **sql_variant**. Analogamente al **DBTYPE_VARIANT** di OLE DB, il tipo di dati **sql_variant** può archiviare dati di tipi diversi. Tuttavia, esistono alcune differenze fondamentali tra **DBTYPE_VARIANT** e **sql_variant**. ADO gestisce anche i dati archiviati come valore di **sql_variant** in modo diverso rispetto alla gestione di altri tipi di dati. Nell'elenco seguente vengono descritti i problemi da considerare quando si accede a SQL Server dati archiviati in colonne di tipo **sql_variant**.

-   In MDAC 2,7, MDAC 2,8 e Windows Data Access Components (Windows DAC) 6,0, il provider di OLE DB per SQL Server supporta il tipo di **sql_variant** . Il provider di OLE DB per ODBC non lo è.

-   Il tipo di **sql_variant** non corrisponde esattamente al tipo di dati **DBTYPE_VARIANT** .  Il tipo di **sql_variant** supporta alcuni nuovi sottotipi non supportati da **DBTYPE_VARIANT,** tra cui **GUID**, stringhe **ANSI** (non Unicode) e **bigint**. L'utilizzo di sottotipi diversi da quelli elencati in precedenza funzionerà correttamente.

-   Il **sql_variant** **numeric** del sottotipo non corrisponde alle dimensioni del **DBTYPE_DECIMAL** .

-   Più coercizioni dei tipi di dati determineranno tipi che non corrispondono. Ad esempio, se si assegna un **sql_variant** con un sottotipo di **GUID** a una **DBTYPE_VARIANT** , viene generato un sottotipo di **SAFEARRAY**(byte). La conversione di questo tipo di nuovo in un **sql_variant** comporterà un nuovo sottotipo di **Array**(byte).

-   I campi del **Recordset** che contengono **sql_variant** dati possono essere in modalità remota (con marshalling) o mantenuti solo se il **sql_variant** contiene sottotipi specifici. Se si tenta di eseguire la modalità remota o di rendere persistenti i dati con i sottotipi non supportati seguenti, verrà generato un errore di run-time (conversione non supportata) dal provider di persistenza Microsoft (MSPersist): **VT_VARIANT**, **VT_RECORD**, **VT_ILLEGAL**, **VT_UNKNOWN**, **VT_BSTR** e **VT_DISPATCH.**

-   Il provider OLE DB per SQL Server in MDAC 2,7, MDAC 2,8 e Windows DAC 6,0 include una proprietà dinamica denominata **Consenti varianti native** che, come suggerisce il nome, consentono agli sviluppatori di accedere al **sql_variant** nel formato nativo invece che a una **DBTYPE_VARIANT**. Se questa proprietà è impostata e un **Recordset** viene aperto con il motore di cursori client (**adUseClient**), la chiamata di **Recordset. Open** avrà esito negativo. Se questa proprietà è impostata e viene aperto un **Recordset** con cursori del server (**adUseServer come**), la chiamata di **Recordset. Open** avrà esito positivo, ma l'accesso a colonne di tipo **sql_variant** genererà un errore.

-   Nelle applicazioni client che utilizzano MDAC 2,5, **sql_variant** dati possono essere utilizzati con query su Microsoft SQL Server. Tuttavia, i valori dei dati **sql_variant** vengono considerati come stringhe. Tali applicazioni client devono essere aggiornate a MDAC 2,7, MDAC 2,8 o Windows DAC 6,0.

## <a name="recordset-behavior"></a>Comportamento del recordset
 SQLOLEDB non può utilizzare SQL Server cursori per supportare il multiplo risultato generato da molti comandi. Se un consumer richiede un recordset che richiede SQL Server supporto del cursore, si verifica un errore se il testo del comando utilizzato genera più di un singolo recordset come risultato.

 I set di record SQLOLEDB scorrevoli sono supportati da SQL Server cursori. SQL Server impone limitazioni sui cursori che sono sensibili alle modifiche apportate da altri utenti del database. In particolare, le righe in alcuni cursori non possono essere ordinate e il tentativo di creare un recordset utilizzando un comando contenente una clausola SQL ORDER BY potrebbe non riuscire.

## <a name="dynamic-properties"></a>Proprietà dinamiche
 Il provider di OLE DB Microsoft per SQL Server inserisce diverse proprietà dinamiche nella raccolta **Properties** degli oggetti [Connection](../../reference/ado-api/connection-object-ado.md), [Recordset](../../reference/ado-api/recordset-object-ado.md)e [Command](../../reference/ado-api/command-object-ado.md) non aperti.

 Le tabelle seguenti sono un indice incrociato dei nomi ADO e OLE DB per ogni proprietà dinamica. Il riferimento del programmatore OLE DB fa riferimento a un nome di proprietà ADO in base al termine "Description". Per ulteriori informazioni su queste proprietà, vedere OLE DB Programmer ' s Reference. Cercare il nome della proprietà OLE DB nell'indice o vedere [Appendice C: OLE DB proprietà](/previous-versions/windows/desktop/ms723130(v=vs.85)).

## <a name="connection-dynamic-properties"></a>Proprietà dinamiche della connessione
 Le proprietà seguenti vengono aggiunte alla raccolta **Properties** dell'oggetto **Connection** .

|Nome della proprietà ADO|Nome della proprietà OLE DB|
|-----------------------|--------------------------|
|Sessioni attive|DBPROP_ACTIVESESSIONS|
|Interruzione asincrona|DBPROP_ASYNCTXNABORT|
|Commit asincrona|DBPROP_ASYNCTNXCOMMIT|
|Livelli di isolamento autocommit|DBPROP_SESS_AUTOCOMMITISOLEVELS|
|Percorso catalogo|DBPROP_CATALOGLOCATION|
|Termine Catalogo|DBPROP_CATALOGTERM|
|Definizione di colonna|DBPROP_COLUMNDEFINITION|
|Timeout di connessione|DBPROP_INIT_TIMEOUT|
|Catalogo corrente|DBPROP_CURRENTCATALOG|
|origine dati|DBPROP_INIT_DATASOURCE|
|Data Source Name|DBPROP_DATASOURCENAME|
|Modello di threading dell'oggetto origine dati|DBPROP_DSOTHREADMODEL|
|Nome DBMS|DBPROP_DBMSNAME|
|Versione DBMS|DBPROP_DBMSVER|
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|
|Supporto per GROUP BY|DBPROP_GROUPBY|
|Supporto tabelle eterogenee|DBPROP_HETEROGENEOUSTABLES|
|Distinzione maiuscole/minuscole identificatore|DBPROP_IDENTIFIERCASE|
|Catalogo iniziale|DBPROP_INIT_CATALOG|
|Livelli di isolamento|DBPROP_SUPPORTEDTXNISOLEVELS|
|Conservazione isolamento|DBPROP_SUPPORTEDTXNISORETAIN|
|Locale Identifier|DBPROP_INIT_LCID|
|Dimensioni massime indice|DBPROP_MAXINDEXSIZE|
|Dimensioni massime riga|DBPROP_MAXROWSIZE|
|Dimensioni massime righe includono BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Numero massimo di tabelle in SELECT|DBPROP_MAXTABLESINSELECT|
|Set di parametri multipli|DBPROP_MULTIPLEPARAMSETS|
|Risultati multipli|DBPROP_MULTIPLERESULTS|
|Più oggetti di archiviazione|DBPROP_MULTIPLESTORAGEOBJECTS|
|Aggiornamento di più tabelle|DBPROP_MULTITABLEUPDATE|
|Ordinamento regole di confronto NULL|DBPROP_NULLCOLLATION|
|Comportamento concatenazione NULL|DBPROP_CONCATNULLBEHAVIOR|
|Versione OLE DB|DBPROP_PROVIDEROLEDBVER|
|Supporto per oggetti OLE|DBPROP_OLEOBJECTS|
|Supporto per set di righe aperto|DBPROP_OPENROWSETSUPPORT|
|Colonne ORDER BY nell'elenco di selezione|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilità parametro di output|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Passa per funzioni di accesso Ref|DBPROP_BYREFACCESSORS|
|Password|DBPROP_AUTH_PASSWORD|
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|
|Tipo ID persistente|DBPROP_PERSISTENTIDTYPE|
|Comportamento preparazione interruzione|DBPROP_PREPAREABORTBEHAVIOR|
|Comportamento preparazione commit|DBPROP_PREPARECOMMITBEHAVIOR|
|Termine procedura|DBPROP_PROCEDURETERM|
|Prompt|DBPROP_INIT_PROMPT|
|Nome descrittivo provider|DBPROP_PROVIDERFRIENDLYNAME|
|Provider Name|DBPROP_PROVIDERFILENAME|
|Versione del provider|DBPROP_PROVIDERVER|
|Read-Only origine dati|DBPROP_DATASOURCEREADONLY|
|Conversioni di set di righe nel comando|DBPROP_ROWSETCONVERSIONSONCOMMAND|
|Termine schema|DBPROP_SCHEMATERM|
|Utilizzo dello schema|DBPROP_SCHEMAUSAGE|
|Supporto di SQL|DBPROP_SQLSUPPORT|
|Archiviazione strutturata|DBPROP_STRUCTUREDSTORAGE|
|Supporto sottoquery|DBPROP_SUBQUERIES|
|Termine tabella|DBPROP_TABLETERM|
|DDL transazione|DBPROP_SUPPORTEDTXNDDL|
|ID utente|DBPROP_AUTH_USERID|
|Nome utente|DBPROP_USERNAME|
|Handle finestra|DBPROP_INIT_HWND|

## <a name="recordset-dynamic-properties"></a>Proprietà dinamiche del recordset
 Le proprietà seguenti vengono aggiunte alla raccolta **Properties** dell'oggetto **Recordset** .

|Nome della proprietà ADO|Nome della proprietà OLE DB|
|-----------------------|--------------------------|
|Ordine di accesso|DBPROP_ACCESSORDER|
|Blocco di oggetti di archiviazione|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo di segnalibro|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|Modifica righe inserite|DBPROP_CHANGEINSERTEDROWS|
|Privilegi di colonna|DBPROP_COLUMNRESTRICT|
|Notifica set di colonne|DBPROP_NOTIFYCOLUMNSET|
|Timeout del comando|DBPROP_COMMANDTIMEOUT|
|Rinvia colonna|DBPROP_DEFERRED|
|Ritardare gli aggiornamenti degli oggetti di archiviazione|DBPROP_DELAYSTORAGEOBJECTS|
|Recupera all'indietro|DBPROP_CANFETCHBACKWARDS|
|Mantieni righe|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Righe non mobili|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Segnalibri letterali|DBPROP_LITERALBOOKMARKS|
|Identità di riga letterale|DBPROP_LITERALIDENTITY|
|Numero massimo di righe aperte|DBPROP_MAXOPENROWS|
|Numero massimo di righe in sospeso|DBPROP_MAXPENDINGROWS|
|Numero massimo di righe|DBPROP_MAXROWS|
|Granularità delle notifiche|DBPROP_NOTIFICATIONGRANULARITY|
|Fasi di notifica|DBPROP_NOTIFICATIONPHASES|
|Oggetti sottoposti a transazione|DBPROP_TRANSACTEDOBJECT|
|Modifiche di altri utenti visibili|DBPROP_OTHERUPDATEDELETE|
|Inserimenti di altri utenti visibili|DBPROP_OTHERINSERT|
|Modifiche personalizzate visibili|DBPROP_OWNUPDATEDELETE|
|Inserimenti personali visibili|DBPROP_OWNINSERT|
|Mantieni in interruzione|DBPROP_ABORTPRESERVE|
|Mantieni al commit|DBPROP_COMMITPRESERVE|
|Riavvio rapido|DBPROP_QUICKRESTART|
|Eventi rientranti|DBPROP_REENTRANTEVENTS|
|Rimuovi righe eliminate|DBPROP_REMOVEDELETED|
|Segnala più modifiche|DBPROP_REPORTMULTIPLECHANGES|
|Restituisci inserimenti in sospeso|DBPROP_RETURNPENDINGINSERTS|
|Notifica eliminazione riga|DBPROP_NOTIFYROWDELETE|
|Notifica di modifica prima riga|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notifica inserimento riga|DBPROP_NOTIFYROWINSERT|
|Privilegi di riga|DBPROP_ROWRESTRICT|
|Notifica della risincronizzazione delle righe|DBPROP_NOTIFYROWRESYNCH|
|Modello di threading delle righe|DBPROP_ROWTHREADMODEL|
|Notifica modifiche annullamento riga|DBPROP_NOTIFYROWUNDOCHANGE|
|Notifica annullamento eliminazione riga|DBPROP_NOTIFYROWUNDODELETE|
|Notifica annullamento inserimento riga|DBPROP_NOTIFYROWUNDOINSERT|
|Notifica aggiornamento riga|DBPROP_NOTIFYROWUPDATE|
|Notifica di modifica posizione recupero set di righe|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|Notifica di rilascio del set di righe|DBPROP_NOTIFYROWSETRELEASE|
|Scorri indietro|DBPROP_CANSCROLLBACKWARDS|
|Cursore server|DBPROP_SERVERCURSOR|
|Ignora segnalibri eliminati|DBPROP_BOOKMARKSKIPPED|
|Identità riga forte|DBPROP_STRONGITDENTITY|
|Righe univoche|DBPROP_UNIQUEROWS|
|Aggiornabilità|DBPROP_UPDATABILITY|
|Usare i segnalibri|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Proprietà dinamiche del comando
 Le proprietà seguenti vengono aggiunte alla raccolta **Properties** dell'oggetto **Command** .

|Nome della proprietà ADO|Nome della proprietà OLE DB|
|-----------------------|--------------------------|
|Ordine di accesso|DBPROP_ACCESSORDER|
|Percorso di base|SSPROP_STREAM_BASEPATH|
|Blocco di oggetti di archiviazione|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo di segnalibro|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|Modifica righe inserite|DBPROP_CHANGEINSERTEDROWS|
|Privilegi di colonna|DBPROP_COLUMNRESTRICT|
|Notifica set di colonne|DBPROP_NOTIFYCOLUMNSET|
|Tipo di contenuto|SSPROP_STREAM_CONTENTTYPE|
|Recupero automatico del cursore|SSPROP_CURSORAUTOFETCH|
|Rinvia colonna|DBPROP_DEFERRED|
|Posticipa comandi prepare|SSPROP_DEFERPREPARE|
|Ritardare gli aggiornamenti degli oggetti di archiviazione|DBPROP_DELAYSTORAGEOBJECTS|
|Recupera all'indietro|DBPROP_CANFETCHBACKWARDS|
|Mantieni righe|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Righe non mobili|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch|DBPROP_IRowsetResynch|
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Segnalibri letterali|DBPROP_LITERALBOOKMARKS|
|Identità di riga letterale|DBPROP_LITERALIDENTITY|
|Modalità di blocco|DBPROP_LOCKMODE|
|Numero massimo di righe aperte|DBPROP_MAXOPENROWS|
|Numero massimo di righe in sospeso|DBPROP_MAXPENDINGROWS|
|Numero massimo di righe|DBPROP_MAXROWS|
|Granularità delle notifiche|DBPROP_NOTIFICATIONGRANULARITY|
|Fasi di notifica|DBPROP_NOTIFICATIONPHASES|
|Oggetti sottoposti a transazione|DBPROP_TRANSACTEDOBJECT|
|Modifiche di altri utenti visibili|DBPROP_OTHERUPDATEDELETE|
|Inserimenti di altri utenti visibili|DBPROP_OTHERINSERT|
|Proprietà di codifica di output|DBPROP_OUTPUTENCODING|
|Proprietà del flusso di output|DBPROP_OUTPUTSTREAM|
|Modifiche personalizzate visibili|DBPROP_OWNUPDATEDELETE|
|Inserimenti personali visibili|DBPROP_OWNINSERT|
|Mantieni in interruzione|DBPROP_ABORTPRESERVE|
|Mantieni al commit|DBPROP_COMMITPRESERVE|
|Riavvio rapido|DBPROP_QUICKRESTART|
|Eventi rientranti|DBPROP_REENTRANTEVENTS|
|Rimuovi righe eliminate|DBPROP_REMOVEDELETED|
|Segnala più modifiche|DBPROP_REPORTMULTIPLECHANGES|
|Restituisci inserimenti in sospeso|DBPROP_RETURNPENDINGINSERTS|
|Notifica eliminazione riga|DBPROP_NOTIFYROWDELETE|
|Notifica di modifica prima riga|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notifica inserimento riga|DBPROP_NOTIFYROWINSERT|
|Privilegi di riga|DBPROP_ROWRESTRICT|
|Notifica della risincronizzazione delle righe|DBPROP_NOTIFYROWRESYNCH|
|Modello di threading delle righe|DBPROP_ROWTHREADMODEL|
|Notifica modifiche annullamento riga|DBPROP_NOTIFYROWUNDOCHANGE|
|Notifica annullamento eliminazione riga|DBPROP_NOTIFYROWUNDODELETE|
|Notifica annullamento inserimento riga|DBPROP_NOTIFYROWUNDOINSERT|
|Notifica aggiornamento riga|DBPROP_NOTIFYROWUPDATE|
|Notifica di modifica posizione recupero set di righe|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|
|Notifica di rilascio del set di righe|DBPROP_NOTIFYROWSETRELEASE|
|Scorri indietro|DBPROP_CANSCROLLBACKWARDS|
|Cursore server|DBPROP_SERVERCURSOR|
|Dati server all'inserimento|DBPROP_SERVERDATAONINSERT|
|Ignora segnalibri eliminati|DBPROP_BOOKMARKSKIP|
|Identità riga forte|DBPROP_STRONGIDENTITY|
|Aggiornabilità|DBPROP_UPDATABILITY|
|Usare i segnalibri|DBPROP_BOOKMARKS|
|Radice XML|SSPROP_STREAM_XMLROOT|
|XSL|SSPROP_STREAM_XSL|

 Per dettagli specifici sull'implementazione e informazioni funzionali sul provider di OLE DB Microsoft SQL Server, vedere il [provider di SQL Server](/previous-versions/windows/desktop/ms720897(v=vs.85)).

## <a name="see-also"></a>Vedere anche
 Proprietà [ConnectionString (](../../reference/ado-api/connectionstring-property-ado.md) ADO) [provider Property](../../reference/ado-api/provider-property-ado.md) (ADO) [Recordset Object (ADO)](../../reference/ado-api/recordset-object-ado.md)