---
description: Panoramica di Microsoft OLE DB provider per Microsoft Jet
title: Provider Microsoft OLE DB per Microsoft Jet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Jet provider for OLE DB [ADO]
- providers [ADO], OLE DB provider for Microsoft Jet
- OLE DB provider for Microsoft Jet [ADO]
ms.assetid: fd956da1-5203-40af-aa7e-fc13a6c6581f
author: rothja
ms.author: jroth
ms.openlocfilehash: 87d9e159a9a97a0a929bec72d03118a927aad56e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029225"
---
# <a name="microsoft-ole-db-provider-for-microsoft-jet-overview"></a>Panoramica di Microsoft OLE DB provider per Microsoft Jet
Il provider OLE DB per Microsoft Jet consente a ADO di accedere ai database Microsoft Jet.

## <a name="connection-string-parameters"></a>Parametri della stringa di connessione
 Per connettersi a questo provider, impostare l'argomento del *provider* della proprietà [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) sulla proprietà seguente:

```vb
Microsoft.Jet.OLEDB.4.0
```

 La lettura della proprietà del [provider](../../reference/ado-api/provider-property-ado.md) restituirà anche questa stringa.

## <a name="typical-connection-string"></a>Stringa di connessione tipica
 Una stringa di connessione tipica per questo provider è la seguente:

```vb
"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=databaseName;User ID=MyUserID;Password=MyPassword;"
```

 La stringa è costituita dalle parole chiave seguenti:

|Parola chiave|Descrizione|
|-------------|-----------------|
|**Provider**|Specifica il provider di OLE DB per Microsoft Jet.|
|**Origine dati**|Specifica il percorso e il nome file del database (ad esempio, `c:\Northwind.mdb` ).|
|**ID utente**|Specifica il nome utente. Se questa parola chiave non viene specificata, `admin` per impostazione predefinita viene usata la stringa "".|
|**Password**|Specifica la password dell'utente. Se questa parola chiave non viene specificata, per impostazione predefinita viene usata la stringa vuota ("").|

> [!NOTE]
>  Se ci si connette a un provider dell'origine dati che supporta l'autenticazione di Windows, è necessario specificare **Trusted_Connection = Yes** o **Integrated Security = SSPI** anziché le informazioni relative a ID utente e password nella stringa di connessione.

## <a name="provider-specific-connection-parameters"></a>Parametri di connessione Provider-Specific
 Il provider OLE DB per Microsoft Jet supporta diverse proprietà dinamiche specifiche del provider, oltre a quelle definite da ADO. Come per tutti gli altri parametri di **connessione** , possono essere impostati tramite la raccolta **Properties** dell'oggetto **Connection** o come parte della stringa di connessione.

 Nella tabella seguente sono elencate queste proprietà insieme al nome della proprietà OLE DB corrispondente tra parentesi.

|Parametro|Descrizione|
|---------------|-----------------|
|Jet OLEDB: importo spazio recuperato compatto (DBPROP_JETOLEDB_COMPACTFREESPACESIZE)|Indica una stima della quantità di spazio, in byte, che può essere recuperata dalla compattazione del database. Questo valore è valido solo dopo che è stata stabilita una connessione al database.|
|Jet OLEDB: controllo della connessione (DBPROP_JETOLEDB_CONNECTIONCONTROL)|Indica se gli utenti possono connettersi al database.|
|Jet OLEDB: Crea database di sistema (DBPROP_JETOLEDB_CREATESYSTEMDATABASE)|Indica se è necessario creare un database di sistema quando si crea una nuova origine dati.|
|Jet OLEDB: modalità di blocco del database (DBPROP_JETOLEDB_DATABASELOCKMODE)|Indica la modalità di blocco per il database. Il primo utente che apre il database determina la modalità utilizzata durante l'apertura del database.|
|Jet OLEDB: password database (DBPROP_JETOLEDB_DATABASEPASSWORD)|Indica la password del database.|
|Jet OLEDB: non copiare le impostazioni locali in Compact (DBPROP_JETOLEDB_COMPACT_DONTCOPYLOCALE)|Indica se Jet deve copiare le informazioni sulle impostazioni locali durante la compattazione di un database.|
|Jet OLEDB: Crittografa database (DBPROP_JETOLEDB_ENCRYPTDATABASE)|Indica se è necessario crittografare un database compresso. Se questa proprietà non è impostata, il database compresso verrà crittografato se anche il database originale è stato crittografato.|
|Jet OLEDB: tipo di motore (DBPROP_JETOLEDB_ENGINE)|Indica il motore di archiviazione utilizzato per accedere all'archivio dati corrente.|
|Jet OLEDB: ritardo asincrono esclusivo (DBPROP_JETOLEDB_EXCLUSIVEASYNCDELAY)|Indica il periodo di tempo massimo, in millisecondi, per cui Jet può ritardare le scritture asincrone su disco quando il database viene aperto in modo esclusivo.<br /><br /> Questa proprietà viene ignorata a meno che **Jet OLEDB: Flush Transaction Timeout** non sia impostato su 0.|
|Jet OLEDB: timeout di svuotamento transazioni (DBPROP_JETOLEDB_FLUSHTRANSACTIONTIMEOUT)|Indica la quantità di tempo di attesa prima che i dati archiviati in una cache per la scrittura asincrona vengano scritti su disco. Questa impostazione esegue l'override dei valori per **Jet OLEDB: ritardo asincrono condiviso** e **Jet OLEDB: ritardo asincrono esclusivo**.|
|Jet OLEDB: transazioni bulk globali (DBPROP_JETOLEDB_GLOBALBULKNOTRANSACTIONS)|Indica se le transazioni bulk SQL sono transazionali.|
|Jet OLEDB: operazioni bulk parziali globali (DBPROP_JETOLEDB_GLOBALBULKPARTIAL)|Indica la password utilizzata per aprire il database.|
|Jet OLEDB: sincronizzazione commit implicita (DBPROP_JETOLEDB_IMPLICITCOMMITSYNC)|Indica se le modifiche apportate alle transazioni implicite interne vengono scritte in modalità sincrona o asincrona.|
|Jet OLEDB: ritardo blocco (DBPROP_JETOLEDB_LOCKDELAY)|Indica il numero di millisecondi di attesa prima di tentare di acquisire un blocco dopo che un tentativo precedente non è riuscito.|
|Jet OLEDB: nuovo tentativo di blocco (DBPROP_JETOLEDB_LOCKRETRY)|Indica il numero di volte in cui un tentativo di accedere a una pagina bloccata viene ripetuto.|
|Jet OLEDB: dimensioni massime del buffer (DBPROP_JETOLEDB_MAXBUFFERSIZE)|Indica la quantità massima di memoria, in kilobyte, che può essere utilizzata da Jet prima di avviare lo scaricamento delle modifiche su disco.|
|Jet OLEDB: numero massimo di blocchi per file (DBPROP_JETOLEDB_MAXLOCKSPERFILE)|Indica il numero massimo di blocchi che Jet può inserire in un database. Il valore predefinito è 9500.|
|Jet OLEDB: nuova password del database (DBPROP_JETOLEDB_NEWDATABASEPASSWORD)|Indica la nuova password da impostare per il database. La vecchia password viene archiviata in **Jet OLEDB: database password**.|
|Jet OLEDB: timeout del comando ODBC (DBPROP_JETOLEDB_ODBCCOMMANDTIMEOUT)|Indica il numero di millisecondi prima del timeout di una query ODBC remota di Jet.|
|Jet OLEDB: blocchi di pagina al blocco della tabella (DBPROP_JETOLEDB_PAGELOCKSTOTABLELOCK)|Indica il numero di pagine che devono essere bloccate all'interno di una transazione prima che Jet tenti di innalzare di livello il blocco a un blocco di tabella. Se questo valore è 0, il blocco non viene mai promosso.|
|Jet OLEDB: timeout pagina (DBPROP_JETOLEDB_PAGETIMEOUT)|Indica il numero di millisecondi di attesa prima di controllare se la cache non è aggiornata con il file di database.|
|Jet OLEDB: riciclo Long-Valued pagine (DBPROP_JETOLEDB_RECYCLELONGVALUEPAGES)|Indica se Jet deve tentare di recuperare le pagine BLOB in modo aggressivo quando vengono liberate.|
|Jet OLEDB: percorso del registro di sistema (DBPROP_JETOLEDB_REGPATH)|Indica la chiave del registro di sistema di Windows che contiene i valori per il motore di database Jet.|
|Jet OLEDB: Reimposta statistiche ISAM (DBPROP_JETOLEDB_RESETISAMSTATS)|Indica se il **Recordset** dello schema DBSCHEMA_JETOLEDB_ISAMSTATS deve reimpostare i contatori delle prestazioni dopo avere restituito le informazioni sulle prestazioni.|
|Jet OLEDB: ritardo asincrono condiviso (DBPROP_JETOLEDB_SHAREDASYNCDELAY)|Indica la quantità massima di tempo, in millisecondi, con cui Jet può ritardare le scritture asincrone su disco quando il database viene aperto in modalità multiutente.|
|Jet OLEDB: database di sistema (DBPROP_JETOLEDB_SYSDBPATH)|Indica il percorso e il nome file per il file di informazioni sul gruppo di lavoro (database di sistema).|
|Jet OLEDB: modalità commit transazioni (DBPROP_JETOLEDB_TXNCOMMITMODE)|Indica se Jet scrive i dati su disco in modo sincrono o asincrono quando viene eseguito il commit di una transazione.|
|Jet OLEDB: sincronizzazione commit utente (DBPROP_JETOLEDB_USERCOMMITSYNC)|Indica se le modifiche apportate alle transazioni vengono scritte in modalità sincrona o asincrona.|

## <a name="provider-specific-recordset-and-command-properties"></a>Proprietà Provider-Specific recordset e comando
 Il provider Jet supporta inoltre diverse proprietà del **comando** e del **Recordset** specifiche del provider. Queste proprietà sono accessibili e impostate tramite la raccolta **Properties** dell'oggetto **Recordset** o **Command** . La tabella elenca il nome della proprietà ADO e il nome della proprietà OLE DB corrispondente tra parentesi.

|Nome proprietà|Descrizione|
|-------------------|-----------------|
|Jet OLEDB: transazioni bulk (DBPROP_JETOLEDB_BULKNOTRANSACTIONS)|Indica se le operazioni bulk SQL vengono sottoposte a transazione. Le operazioni bulk di grandi dimensioni potrebbero avere esito negativo se sottoposte a transazione, a causa di ritardi|
|Jet OLEDB: Abilita cursori FAT (DBPROP_JETOLEDB_ENABLEFATCURSOR)|Indica se Jet deve memorizzare nella cache più righe durante la compilazione di un recordset per le origini di riga remote.|
|Jet OLEDB: dimensioni cache cursore FAT (DBPROP_JETOLEDB_FATCURSORMAXROWS)|Indica il numero di righe da memorizzare nella cache quando si utilizza la memorizzazione nella cache delle righe dell'archivio dati remoto. Questo valore viene ignorato a meno che **Jet OLEDB: Abilita cursori Fat** è true.|
|Jet OLEDB: incoerente (DBPROP_JETOLEDB_INCONSISTENT)|Indica se i risultati della query consentono aggiornamenti non coerenti.|
|Jet OLEDB: granularità del blocco (DBPROP_JETOLEDB_LOCKGRANULARITY)|Indica se una tabella viene aperta utilizzando il blocco a livello di riga.|
|Jet OLEDB: istruzione ODBC Pass-Through (DBPROP_JETOLEDB_ODBCPASSTHROUGH)|Indica che Jet deve passare il testo SQL in un oggetto **Command** al back-end invariato.|
|Jet OLEDB: operazioni bulk parziali (DBPROP_JETOLEDB_BULKPARTIAL)|Indica il comportamento di Jet quando le operazioni DML SQL hanno esito negativo.|
|Jet OLEDB: Bulk-Op di query pass-through (DBPROP_JETOLEDB_PASSTHROUGHBULKOP)|Indica se le query che non restituiscono un **Recordset** vengono passate senza modifiche all'origine dati.|
|Jet OLEDB: stringa di connessione della query pass-through (DBPROP_JETOLEDB_ODBCPASSTHROUGHCONNECTSTRING)|Indica la stringa di connessione Jet utilizzata per la connessione a un archivio dati remoto. Questo valore viene ignorato a meno che **Jet OLEDB: ODBC Pass-Through istruzione** è true.|
|Jet OLEDB: query archiviata (DBPROP_JETOLEDB_STOREDQUERY)|Indica se il testo del comando deve essere interpretato come una query archiviata anziché come comando SQL.|
|Jet OLEDB: convalidare le regole sul set (DBPROP_JETOLEDB_VALIDATEONSET)|Indica se le regole di convalida Jet vengono valutate quando vengono impostati i dati della colonna o quando viene eseguito il commit delle modifiche nel database.|

 Per impostazione predefinita, il provider di OLE DB per Microsoft Jet apre i database Microsoft Jet in modalità di lettura/scrittura. Per aprire un database in modalità di sola lettura, impostare la proprietà [mode](../../reference/ado-api/mode-property-ado.md) dell'oggetto **connessione** ADO su **adModeRead**.

## <a name="command-object-usage"></a>Utilizzo oggetto comando
 Il testo del comando nell'oggetto [comando](../../reference/ado-api/command-object-ado.md) usa il dialetto Microsoft Jet SQL. Nel testo del comando è possibile specificare query di restituzione di righe, query di azione e nomi di tabella. Tuttavia, le stored procedure non sono supportate e non devono essere specificate.

## <a name="recordset-behavior"></a>Comportamento del recordset
 Il motore di database di Microsoft Jet non supporta i cursori dinamici. Pertanto, il provider di OLE DB per Microsoft Jet non supporta il tipo di cursore **adLockDynamic** . Quando viene richiesto un cursore dinamico, il provider restituisce un cursore keyset e reimposta la proprietà [CursorType](../../reference/ado-api/cursortype-property-ado.md) per indicare il tipo di [Recordset](../../reference/ado-api/recordset-object-ado.md) restituito. Inoltre, se viene richiesto **un recordset** aggiornabile (**LockType** è **adLockOptimistic**, **adLockBatchOptimistic** o **adLockPessimistic**), il provider restituirà anche un cursore keyset e reimposta la proprietà **CursorType** .

## <a name="dynamic-properties"></a>Proprietà dinamiche
 Il provider OLE DB per Microsoft Jet inserisce diverse proprietà dinamiche nella raccolta **Properties** degli oggetti [Connection](../../reference/ado-api/connection-object-ado.md), [Recordset](../../reference/ado-api/recordset-object-ado.md)e [Command](../../reference/ado-api/command-object-ado.md) non aperti.

 Le tabelle seguenti sono un indice incrociato dei nomi ADO e OLE DB per ogni proprietà dinamica. Il riferimento del programmatore OLE DB fa riferimento a un nome di proprietà ADO in base al termine "Description". Per ulteriori informazioni su queste proprietà, vedere OLE DB Programmer ' s Reference.

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
|Catalogo corrente|DBPROP_CURRENTCATALOG|
|origine dati|DBPROP_INIT_DATASOURCE|
|Data Source Name|DBPROP_DATASOURCENAME|
|Modello di threading dell'oggetto origine dati|DBPROP_DSOTHREADMODEL|
|Nome DBMS|DBPROP_DBMSNAME|
|Versione DBMS|DBPROP_DBMSVER|
|Supporto per GROUP BY|DBPROP_GROUPBY|
|Supporto tabelle eterogenee|DBPROP_HETEROGENEOUSTABLES|
|Distinzione maiuscole/minuscole identificatore|DBPROP_IDENTIFIERCASE|
|Livelli di isolamento|DBPROP_SUPPORTEDTXNISOLEVELS|
|Conservazione isolamento|DBPROP_SUPPORTEDTXNISORETAIN|
|Locale Identifier|DBPROP_INIT_LCID|
|Dimensioni massime indice|DBPROP_MAXINDEXSIZE|
|Dimensioni massime riga|DBPROP_MAXROWSIZE|
|Dimensioni massime righe includono BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Numero massimo di tabelle in SELECT|DBPROP_MAXTABLESINSELECT|
|Modalità|DBPROP_INIT_MODE|
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
|Set di righe Append-Only|DBPROP_APPENDONLY|
|Blocco di oggetti di archiviazione|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo di segnalibro|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|Segnalibri ordinati|DBPROP_ORDEREDBOOKMARKS|
|Memorizza nella cache le colonne posticipate|DBPROP_CACHEDEFERRED|
|Modifica righe inserite|DBPROP_CHANGEINSERTEDROWS|
|Privilegi di colonna|DBPROP_COLUMNRESTRICT|
|Notifica set di colonne|DBPROP_NOTIFYCOLUMNSET|
|Colonna scrivibile|DBPROP_MAYWRITECOLUMN|
|Rinvia colonna|DBPROP_DEFERRED|
|Ritardare gli aggiornamenti degli oggetti di archiviazione|DBPROP_DELAYSTORAGEOBJECTS|
|Recupera all'indietro|DBPROP_CANFETCHBACKWARDS|
|Mantieni righe|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Righe non mobili|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Segnalibri letterali|DBPROP_LITERALBOOKMARKS|
|Identità di riga letterale|DBPROP_LITERALIDENTITY|
|Numero massimo di righe aperte|DBPROP_MAXOPENROWS|
|Numero massimo di righe in sospeso|DBPROP_MAXPENDINGROWS|
|Numero massimo di righe|DBPROP_MAXROWS|
|Utilizzo memoria|DBPROP_MEMORYUSAGE|
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
|Ignora segnalibri eliminati|DBPROP_BOOKMARKSKIPPED|
|Identità riga forte|DBPROP_STRONGITDENTITY|
|Aggiornabilità|DBPROP_UPDATABILITY|
|Usare i segnalibri|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Proprietà dinamiche del comando
 Le proprietà seguenti vengono aggiunte alla raccolta **Properties** dell'oggetto **Command** .

|Nome della proprietà ADO|Nome della proprietà OLE DB|
|-----------------------|--------------------------|
|Ordine di accesso|DBPROP_ACCESSORDER|
|Set di righe Append-Only|DBPROP_APPENDONLY|
|Blocco di oggetti di archiviazione|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo di segnalibro|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|Modifica righe inserite|DBPROP_CHANGEINSERTEDROWS|
|Privilegi di colonna|DBPROP_COLUMNRESTRICT|
|Notifica set di colonne|DBPROP_NOTIFYCOLUMNSET|
|Rinvia colonna|DBPROP_DEFERRED|
|Ritardare gli aggiornamenti degli oggetti di archiviazione|DBPROP_DELAYSTORAGEOBJECTS|
|Recupera all'indietro|DBPROP_CANFETCHBACKWARDS|
|Mantieni righe|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Righe non mobili|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
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
|Dati server all'inserimento|DBPROP_SERVERDATAONINSERT|
|Ignora segnalibri eliminati|DBPROP_BOOKMARKSKIP|
|Identità riga forte|DBPROP_STRONGIDENTITY|
|Aggiornabilità|DBPROP_UPDATABILITY|
|Usare i segnalibri|DBPROP_BOOKMARKS|

 Per dettagli specifici sull'implementazione e informazioni funzionali sul provider OLE DB per Microsoft Jet, vedere [provider Jet](/previous-versions/windows/desktop/ms722791(v=vs.85)) nella documentazione di OLE DB.
