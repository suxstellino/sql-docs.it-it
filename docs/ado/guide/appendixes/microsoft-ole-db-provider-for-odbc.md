---
description: Panoramica di Microsoft OLE DB provider per ODBC
title: Provider OLE DB Microsoft per ODBC | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for ODBC [ADO]
- providers [ADO], OLE DB provider for ODBC
ms.assetid: 2dc0372d-e74d-4d0f-9c8c-04e5a168c148
author: rothja
ms.author: jroth
ms.openlocfilehash: 5a6cfdda6072f8ed51d55569ed68b113a4148642
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029265"
---
# <a name="microsoft-ole-db-provider-for-odbc-overview"></a>Panoramica di Microsoft OLE DB provider per ODBC
A un programmatore ADO o RDS, un mondo ideale è quello in cui ogni origine dati espone un'interfaccia di OLE DB, in modo che ADO possa chiamare direttamente nell'origine dati. Sebbene sempre più fornitori di database implementino OLE DB interfacce, alcune origini dati non sono ancora esposte in questo modo. È tuttavia possibile accedere alla maggior parte dei sistemi DBMS attualmente in uso tramite ODBC.

 I driver ODBC sono disponibili per tutti i principali sistemi DBMS attualmente in uso, tra cui Microsoft SQL Server, Microsoft Access (motore di database Microsoft Jet) e Microsoft FoxPro, oltre ai prodotti di database non Microsoft, ad esempio Oracle.

 Il provider ODBC Microsoft, tuttavia, consente a ADO di connettersi a qualsiasi origine dati ODBC. Il provider è a thread libero e il formato Unicode è abilitato.

 Il provider supporta le transazioni, sebbene diversi motori DBMS offrano tipi diversi di supporto delle transazioni. Ad esempio, Microsoft Access supporta le transazioni nidificate fino a cinque livelli di profondità.

 Si tratta del provider predefinito per ADO e tutte le proprietà e i metodi ADO dipendenti dal provider sono supportati.

## <a name="connection-string-parameters"></a>Parametri della stringa di connessione
 Per connettersi a questo provider, impostare l'argomento **provider =** della proprietà [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) su:

```
MSDASQL
```

 La lettura della proprietà del [provider](../../reference/ado-api/provider-property-ado.md) restituirà anche questa stringa.

## <a name="typical-connection-string"></a>Stringa di connessione tipica
 Una stringa di connessione tipica per questo provider è la seguente:

```
"Provider=MSDASQL;DSN=dsnName;UID=MyUserID;PWD=MyPassword;"
```

 La stringa è costituita dalle parole chiave seguenti:

|Parola chiave|Descrizione|
|-------------|-----------------|
|**Provider**|Specifica il provider di OLE DB per ODBC.|
|**DSN**|Specifica il nome dell'origine dati.|
|**UID**|Specifica il nome utente.|
|**PWD**|Specifica la password dell'utente.|
|**URL**|Specifica l'URL di un file o di una directory pubblicata in una cartella Web.|

 Poiché si tratta del provider predefinito per ADO, se si omette il parametro **provider =** dalla stringa di connessione, ADO tenterà di stabilire una connessione a questo provider.

> [!NOTE]
>  Se ci si connette a un provider dell'origine dati che supporta l'autenticazione di Windows, è necessario specificare **Trusted_Connection = Yes** o **Integrated Security = SSPI** anziché le informazioni relative a ID utente e password nella stringa di connessione.

 Il provider non supporta i parametri di connessione specifici, oltre a quelli definiti da ADO. Tuttavia, il provider passerà tutti i parametri di connessione non ADO a gestione driver ODBC.

 Poiché è possibile omettere il parametro **provider** , è quindi possibile comporre una stringa di connessione ADO identica a una stringa di connessione ODBC per la stessa origine dati. Utilizzare gli stessi nomi di parametro (**driver =**, **database =**, **DSN =** e così via), i valori e la sintassi come si farebbe quando si compone una stringa di connessione ODBC. È possibile connettersi con o senza un nome origine dati (DSN) o FileDSN predefinito.

## <a name="syntax-with-a-dsn-or-filedsn"></a>Sintassi con DSN o FileDSN:

```
"[Provider=MSDASQL;] { DSN=name | FileDSN=filename } ;
[DATABASE=database;] UID=user; PWD=password"
```

## <a name="syntax-without-a-dsn-dsn-less-connection"></a>Sintassi senza DSN (connessione senza DSN):

```
"[Provider=MSDASQL;] DRIVER=driver; SERVER=server;
DATABASE=database; UID=MyUserID; PWD=MyPassword"
```

## <a name="remarks"></a>Commenti
 Se si utilizza un **DSN** o **FileDSN**, è necessario che venga definito tramite Amministrazione origine dati ODBC nel pannello di controllo di Windows. In Microsoft Windows 2000, l'amministratore ODBC si trova in strumenti di amministrazione. Nelle versioni precedenti di Windows, l'icona dell'amministratore ODBC è denominata **ODBC a 32 bit** o solo **ODBC**.

 In alternativa all'impostazione di un **DSN**, è possibile specificare il driver ODBC (**driver =**), ad esempio "SQL Server;" il nome del server (**server =**); e il nome del database (**database =**).

 È inoltre possibile specificare un nome di account utente (**UID =**) e la password per l'account utente (**pwd =**) nei parametri specifici di ODBC o nei parametri di *utente* e *password* standard definiti da ADO.

 Sebbene una definizione **DSN** specifichi già un database, è possibile specificare *un* parametro di *database* oltre a un **DSN** per connettersi a un database diverso. Quando si utilizza un **DSN**, è consigliabile includere sempre *il parametro del* *database* . In questo modo sarà possibile connettersi al database corretto se un altro utente ha modificato il parametro di database predefinito dall'ultima volta che è stata controllata la definizione **DSN** .

## <a name="provider-specific-connection-properties"></a>Proprietà di connessione Provider-Specific
 Il provider OLE DB per ODBC aggiunge diverse proprietà alla raccolta [Properties](../../reference/ado-api/properties-collection-ado.md) dell'oggetto **Connection** . Nella tabella seguente sono elencate le proprietà con il nome della proprietà OLE DB corrispondente tra parentesi.

|Nome proprietà|Descrizione|
|-------------------|-----------------|
|Procedure accessibili (KAGPROP_ACCESSIBLEPROCEDURES)|Indica se l'utente ha accesso alle stored procedure.|
|Tabelle accessibili (KAGPROP_ACCESSIBLETABLES)|Indica se l'utente dispone delle autorizzazioni per eseguire le istruzioni SELECT sulle tabelle del database.|
|Istruzioni attive (KAGPROP_ACTIVESTATEMENTS)|Indica il numero di handle che un driver ODBC può supportare in una connessione.|
|Nome driver (KAGPROP_DRIVERNAME)|Indica il nome file del driver ODBC.|
|Versione ODBC del driver (KAGPROP_DRIVERODBCVER)|Indica la versione di ODBC supportata dal driver.|
|Utilizzo file (KAGPROP_FILEUSAGE)|Indica il modo in cui il driver considera un file in un'origine dati. come tabella o come catalogo.|
|Clausola like Escape (KAGPROP_LIKEESCAPECLAUSE)|Indica se il driver supporta la definizione e l'utilizzo di un carattere di escape per il carattere di percentuale (%) e carattere di sottolineatura (_) nel predicato LIKE di una clausola WHERE.|
|Numero massimo di colonne in Group by (KAGPROP_MAXCOLUMNSINGROUPBY)|Indica il numero massimo di colonne che possono essere elencate nella clausola GROUP BY di un'istruzione SELECT.|
|Numero massimo di colonne nell'indice (KAGPROP_MAXCOLUMNSININDEX)|Indica il numero massimo di colonne che possono essere incluse in un indice.|
|Numero massimo di colonne in order by (KAGPROP_MAXCOLUMNSINORDERBY)|Indica il numero massimo di colonne che possono essere elencate nella clausola ORDER BY di un'istruzione SELECT.|
|Numero massimo di colonne in Select (KAGPROP_MAXCOLUMNSINSELECT)|Indica il numero massimo di colonne che è possibile elencare nella parte SELECT di un'istruzione SELECT.|
|Numero massimo di colonne nella tabella (KAGPROP_MAXCOLUMNSINTABLE)|Indica il numero massimo di colonne consentite in una tabella.|
|Funzioni numeriche (KAGPROP_NUMERICFUNCTIONS)|Indica quali funzioni numeriche sono supportate dal driver ODBC. Per un elenco di nomi di funzione e i valori associati utilizzati in questa maschera di maschera, vedere [Appendice E: funzioni scalari](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), nella documentazione di ODBC.|
|Funzionalità outer join (KAGPROP_OJCAPABILITY)|Indica i tipi di OUTER JOIN supportati dal provider.|
|Outer join (KAGPROP_OUTERJOINS)|Indica se il provider supporta OUTER JOIN.|
|Caratteri speciali (KAGPROP_SPECIALCHARACTERS)|Indica quali caratteri hanno un significato speciale per il driver ODBC.|
|Stored procedure (KAGPROP_PROCEDURES)|Indica se le stored procedure sono disponibili per l'utilizzo con questo driver ODBC.|
|Funzioni stringa (KAGPROP_STRINGFUNCTIONS)|Indica quali funzioni stringa sono supportate dal driver ODBC. Per un elenco di nomi di funzione e i valori associati utilizzati in questa maschera di maschera, vedere [Appendice E: funzioni scalari](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), nella documentazione di ODBC.|
|Funzioni di sistema (KAGPROP_SYSTEMFUNCTIONS)|Indica quali funzioni di sistema sono supportate dal driver ODBC. Per un elenco di nomi di funzione e i valori associati utilizzati in questa maschera di maschera, vedere [Appendice E: funzioni scalari](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), nella documentazione di ODBC.|
|Funzioni di data/ora (KAGPROP_TIMEDATEFUNCTIONS)|Indica quali funzioni di data e ora sono supportate dal driver ODBC. Per un elenco di nomi di funzione e i valori associati utilizzati in questa maschera di maschera, vedere [Appendice E: funzioni scalari](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), nella documentazione di ODBC.|
|Supporto della grammatica SQL (KAGPROP_ODBCSQLCONFORMANCE)|Indica la grammatica SQL supportata dal driver ODBC.|

## <a name="provider-specific-recordset-and-command-properties"></a>Proprietà Provider-Specific recordset e comando
 Il provider OLE DB per ODBC aggiunge diverse proprietà alla raccolta **Properties** degli oggetti **Recordset** e **Command** . Nella tabella seguente sono elencate le proprietà con il nome della proprietà OLE DB corrispondente tra parentesi.

|Nome proprietà|Descrizione|
|-------------------|-----------------|
|Aggiornamenti/eliminazioni/inserimenti basati su query (KAGPROP_QUERYBASEDUPDATES)|Indica se è possibile eseguire aggiornamenti, eliminazioni e inserimenti tramite query SQL.|
|Tipo di concorrenza ODBC (KAGPROP_CONCURRENCY)|Indica il metodo utilizzato per ridurre i potenziali problemi causati da due utenti che tentano di accedere contemporaneamente agli stessi dati dall'origine dati.|
|Accessibilità BLOB su Forward-Only cursore (KAGPROP_BLOBSONFOCURSOR)|Indica se è possibile accedere ai **campi** BLOB quando si utilizza un cursore di sola trasmissione.|
|Includi SQL_FLOAT, SQL_DOUBLE e SQL_REAL nelle clausole WHERE di QBU (KAGPROP_INCLUDENONEXACT)|Indica se i valori SQL_FLOAT, SQL_DOUBLE e SQL_REAL possono essere inclusi in una clausola WHERE QBU.|
|Posizione sull'ultima riga dopo l'inserimento (KAGPROP_POSITIONONNEWROW)|Indica che dopo l'inserimento di un nuovo record in una tabella, l'ultima riga nella tabella sarà la riga corrente.|
|IRowsetChangeExtInfo (KAGPROP_IROWSETCHANGEEXTINFO)|Indica se l'interfaccia **IRowsetChange** fornisce supporto per le informazioni estese.|
|Tipo di cursore ODBC (KAGPROP_CURSOR)|Indica il tipo di cursore utilizzato dal **Recordset**.|
|Genera un set di righe di cui è possibile effettuare il marshalling (KAGPROP_MARSHALLABLE)|Indica che il driver ODBC genera un recordset di cui è possibile effettuare il marshalling|

## <a name="command-text"></a>Testo comando
 Il modo in cui si usa l'oggetto [Command](../../reference/ado-api/command-object-ado.md) in gran parte dipende dall'origine dati e dal tipo di query o dell'istruzione di comando che accetterà.

 ODBC fornisce una sintassi specifica per la chiamata di stored procedure. Per la [proprietà CommandText](../../reference/ado-api/commandtext-property-ado.md) di un oggetto **Command** , l'argomento *CommandText* per il **metodo Execute** su un oggetto [Connection](../../reference/ado-api/connection-object-ado.md) o l'argomento di *origine* al metodo **Open** su un oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) , passa una stringa con questa sintassi:

```
"{ [ ? = ] call procedure [ ( ? [, ? [ , ... ]] ) ] }"
```

 Ogni **?** fa riferimento a un oggetto nella raccolta [Parameters](../../reference/ado-api/parameters-collection-ado.md) . Il primo **?** fa riferimento ai **parametri**(0), a quello successivo **?** fa riferimento ai **parametri**(1) e così via.

 I riferimenti ai parametri sono facoltativi e dipendono dalla struttura del stored procedure. Se si vuole chiamare un stored procedure che non definisce parametri, la stringa avrà un aspetto simile al seguente:

```
"{ call procedure }"
```

 Se si dispone di due parametri di query, la stringa sarà simile alla seguente:

```
"{ call procedure ( ?, ? ) }"
```

 Se il stored procedure restituirà un valore, il valore restituito viene considerato come un altro parametro. Se non si dispone di parametri di query ma si ha un valore restituito, la stringa sarà simile alla seguente:

```
"{ ? = call procedure }"
```

 Infine, se si dispone di un valore restituito e di due parametri di query, la stringa sarà simile alla seguente:

```
"{ ? = call procedure ( ?, ? ) }"
```

## <a name="recordset-behavior"></a>Comportamento del recordset
 Nelle tabelle seguenti sono elencati i metodi e le proprietà ADO standard disponibili in un oggetto **Recordset** aperto con questo provider.

 Per informazioni più dettagliate sul comportamento del **Recordset** per la configurazione del provider, eseguire il metodo [Supports](../../reference/ado-api/supports-method.md) ed enumerare la raccolta **Properties** del **Recordset** per determinare se sono presenti proprietà dinamiche specifiche del provider.

 Disponibilità delle proprietà del **Recordset** ADO standard:

|Proprietà|ForwardOnly|Dynamic|Keyset|Static|
|--------------|-----------------|-------------|------------|------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|non disponibile|non disponibile|lettura/scrittura|lettura/scrittura|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|non disponibile|non disponibile|lettura/scrittura|lettura/scrittura|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[BOF](../../reference/ado-api/bof-eof-properties-ado.md)|Sola lettura|Sola lettura|Sola lettura|Sola lettura|
|[Segnalibro](../../reference/ado-api/bookmark-property-ado.md)|non disponibile|non disponibile|lettura/scrittura|lettura/scrittura|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[EditMode](../../reference/ado-api/editmode-property.md)|Sola lettura|Sola lettura|Sola lettura|Sola lettura|
|[Filter](../../reference/ado-api/filter-property.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[PageCount](../../reference/ado-api/pagecount-property-ado.md)|lettura/scrittura|non disponibile|Sola lettura|Sola lettura|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|lettura/scrittura|non disponibile|Sola lettura|Sola lettura|
|[Origine](../../reference/ado-api/source-property-ado-recordset.md)|lettura/scrittura|lettura/scrittura|lettura/scrittura|lettura/scrittura|
|[State](../../reference/ado-api/state-property-ado.md)|Sola lettura|Sola lettura|Sola lettura|Sola lettura|
|[Status](../../reference/ado-api/status-property-ado-recordset.md)|Sola lettura|Sola lettura|Sola lettura|Sola lettura|

 Le proprietà [AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md) e [AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md) sono di sola scrittura quando ADO viene utilizzato con la versione 1,0 del provider Microsoft OLE DB per ODBC.

 Disponibilità dei metodi **Recordset** ADO standard:

|Metodo|ForwardOnly|Dynamic|Keyset|Static|
|------------|-----------------|-------------|------------|------------|
|[AddNew](../../reference/ado-api/addnew-method-ado.md)|Sì|Sì|Sì|Sì|
|[Annulla](../../reference/ado-api/cancel-method-ado.md)|Sì|Sì|Sì|Sì|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|Sì|Sì|Sì|Sì|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|Sì|Sì|Sì|Sì|
|[Clone](../../reference/ado-api/clone-method-ado.md)|No|No|Sì|Sì|
|[Close](../../reference/ado-api/close-method-ado.md)|Sì|Sì|Sì|Sì|
|[Elimina](../../reference/ado-api/delete-method-ado-recordset.md)|Sì|Sì|Sì|Sì|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|Sì|Sì|Sì|Sì|
|[Sposta](../../reference/ado-api/move-method-ado.md)|Sì|Sì|Sì|Sì|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Sì|Sì|Sì|Sì|
|[MoveLast](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|No|Sì|Sì|Sì|
|[MoveNext](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Sì|Sì|Sì|Sì|
|[MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|No|Sì|Sì|Sì|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)*|Sì|Sì|Sì|Sì|
|[Apri](../../reference/ado-api/open-method-ado-recordset.md)|Sì|Sì|Sì|Sì|
|[Requery](../../reference/ado-api/requery-method.md)|Sì|Sì|Sì|Sì|
|[Risincronizza](../../reference/ado-api/resync-method.md)|No|No|Sì|Sì|
|[Supporti](../../reference/ado-api/supports-method.md)|Sì|Sì|Sì|Sì|
|[Aggiornamento](../../reference/ado-api/update-method.md)|Sì|Sì|Sì|Sì|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|Sì|Sì|Sì|Sì|

 * Non supportato per i database di Microsoft Access.

## <a name="dynamic-properties"></a>Proprietà dinamiche
 Il provider di OLE DB Microsoft per ODBC inserisce diverse proprietà dinamiche nella raccolta **Properties** degli oggetti [Connection](../../reference/ado-api/connection-object-ado.md), [Recordset](../../reference/ado-api/recordset-object-ado.md)e [Command](../../reference/ado-api/command-object-ado.md) non aperti.

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
|Location|DBPROP_INIT_LOCATION|
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
|Servizi di OLE DB|DBPROP_INIT_OLEDBSERVICES|
|Versione OLE DB|DBPROP_PROVIDEROLEDBVER|
|Supporto per oggetti OLE|DBPROP_OLEOBJECTS|
|Supporto per set di righe aperto|DBPROP_OPENROWSETSUPPORT|
|Colonne ORDER BY nell'elenco di selezione|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilità parametro di output|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Password|DBPROP_AUTH_PASSWORD|
|Passa per funzioni di accesso Ref|DBPROP_BYREFACCESSORS|
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
|IRowsetResynch||
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
|Righe univoche|DBPROP_UNIQUEROWS|
|Aggiornabilità|DBPROP_UPDATABILITY|
|Usare i segnalibri|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Proprietà dinamiche del comando
 Le proprietà seguenti vengono aggiunte alla raccolta **Properties** dell'oggetto **Command** .

|Nome della proprietà ADO|Nome della proprietà OLE DB|
|-----------------------|--------------------------|
|Ordine di accesso|DBPROP_ACCESSORDER|
|Blocco di oggetti di archiviazione|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo di segnalibro|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|Modifica righe inserite|DBPROP_CHANGEINSERTEDROWS|
|Privilegi di colonna|DBPROP_COLUMNRESTRICT|
|Notifica set di colonne|DBPROP_NOTIFYCOLUMNSET|
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
|IRowsetResynch||
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
|Ignora segnalibri eliminati|DBPROP_BOOKMARKSKIP|
|Identità riga forte|DBPROP_STRONGIDENTITY|
|Aggiornabilità|DBPROP_UPDATABILITY|
|Usare i segnalibri|DBPROP_BOOKMARKS|

 Per informazioni dettagliate sull'implementazione specifica e sulle informazioni funzionali sul provider Microsoft OLE DB per ODBC, vedere la Guida [di riferimento per programmatori OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)) oppure visitare il sito Web di Data Access and Storage Developer Center su MSDN.

## <a name="see-also"></a>Vedere anche
 [Oggetto Command (ADO)](../../reference/ado-api/command-object-ado.md) [COMMANDTEXT Property (ADO)](../../reference/ado-api/commandtext-property-ado.md) [Connection Object (](../../reference/ado-api/connection-object-ado.md) ADO) [ConnectionString Property (ADO)](../../reference/ado-api/connectionstring-property-ado.md) [Execute Method (comando ADO)](../../reference/ado-api/execute-method-ado-command.md) [metodo Open (recordset ADO)](../../reference/ado-api/open-method-ado-recordset.md) [Parameters Collection (](../../reference/ado-api/parameters-collection-ado.md) ADO) [Proprietà Collection (](../../reference/ado-api/properties-collection-ado.md) ADO) [provider Property](../../reference/ado-api/provider-property-ado.md) (ADO) [Recordset Object (](../../reference/ado-api/recordset-object-ado.md) ADO) [supporta il metodo](../../reference/ado-api/supports-method.md)