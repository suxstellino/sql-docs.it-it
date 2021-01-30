---
description: CREATE EXTERNAL DATA SOURCE (Transact-SQL)
title: CREATE EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL DATA SOURCE
- CREATE_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, data source
- PolyBase, create data source
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b4f4280509d2717f00090b4941c540444c184cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186809"
---
# <a name="create-external-data-source-transact-sql"></a>CREATE EXTERNAL DATA SOURCE (Transact-SQL)

Crea un'origine dati esterna per l'esecuzione di query tramite SQL Server, il database SQL, Azure Synapse Analytics o la piattaforma di strumenti analitici (Parallel Data Warehouse o PDW).

Questo articolo fornisce la sintassi, gli argomenti, la sezione Osservazioni, le autorizzazioni ed esempi per qualsiasi prodotto SQL scelto.

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Database SQL](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Panoramica: SQL Server

Crea un'origine dati esterna per le query PolyBase. Le origini dati esterne vengono usate per stabilire la connettività e supportano questi casi d'uso principali:

- Virtualizzazione dati e caricamento dati con [PolyBase][intro_pb]
- Operazioni di caricamento bulk con `BULK INSERT` o `OPENROWSET`

**Si applica a**: A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CONNECTION_OPTIONS = '<name_value_pairs>']
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] PUSHDOWN = { ON | OFF } ]
    [ [ , ] TYPE = { HADOOP | BLOB_STORAGE } ]
    [ [ , ] RESOURCE_MANAGER_LOCATION = '<resource_manager>[:<port>]' )
[ ; ]
```

## <a name="arguments"></a>Argomenti

### <a name="data_source_name"></a>data_source_name

Specifica il nome definito dall'utente per l'origine dati. Il nome deve essere univoco all'interno del database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Fornisce il protocollo di connettività e il percorso dell'origine dati esterna.

| Origine dati esterna    | Prefisso della posizione | Percorso                                         | Posizioni supportate per prodotto/servizio |
| ----------------------- | --------------- | ----------------------------------------------------- | ---------------------------------------- |
| Cloudera o Hortonworks | `hdfs`          | `<Namenode>[:port]`                                   | A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]                       |
| Account di archiviazione di Azure (V2) | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` | A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]         Spazio dei nomi gerarchico **non** supportato |
| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]              | `sqlserver`     | `<server_name>[\<instance_name>][:port]`              | A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]                       |
| Oracle                  | `oracle`        | `<server_name>[:port]`                                | A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]                       |
| Teradata                | `teradata`      | `<server_name>[:port]`                                | A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]                       |
| MongoDB o CosmosDB     | `mongodb`       | `<server_name>[:port]`                                | A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]                       |
| ODBC                    | `odbc`          | `<server_name>[:port]`                                | A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] - Solo Windows        |
| Operazioni bulk         | `https`         | `<storage_account>.blob.core.windows.net/<container>` | A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]                        |
| Hub di Edge         | `edgehub`         | Non applicabile | EdgeHub è sempre locale nell'istanza di [SQL Edge di Azure](/azure/azure-sql-edge/overview/). Non è necessario, quindi, specificare un percorso o un valore di porta. Disponibile solo in SQL Edge di Azure.                      |
| Kafka        | `kafka`         | `<Kafka IP Address>[:port]` | Disponibile solo in SQL Edge di Azure.                      |

Percorso:

- `<`Namenode`>` = nome del computer, URI del servizio dei nomi o indirizzo IP di `Namenode` nel cluster Hadoop. PolyBase deve risolvere tutti i nomi DNS usati dal cluster Hadoop. <!-- For highly available Hadoop configurations, provide the Nameservice ID as the `LOCATION`. -->
- `port` = porta su cui è in ascolto l'origine dati esterna. Per trovare la porta in Hadoop, si usa il parametro di configurazione `fs.defaultFS`. L'impostazione predefinita è 8020.
- `<container>` = contenitore dell'account di archiviazione che include i dati. I contenitori radice sono di sola lettura, di conseguenza i dati non possono essere riscritti nel contenitore.
- `<storage_account>` = nome dell'account di archiviazione della risorsa di Azure.
- `<server_name>` = nome host.
- `<instance_name>` = nome dell'istanza denominata di SQL Server. Usato se il servizio SQL Server Browser Service è in esecuzione nell'istanza di destinazione.

Note aggiuntive e indicazioni utili per l'impostazione della posizione:

- Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] non verifica l'esistenza dell'origine dati esterna quando viene creato l'oggetto. Per eseguire la convalida, creare una tabella esterna usando l'origine dati esterna.
- Per garantire una semantica di esecuzione di query coerente, usare la stessa origine dati esterna per tutte le tabelle quando si eseguono query su Hadoop.
- È possibile usare il prefisso di posizione `sqlserver` per connettere [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] a un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] o ad Azure Synapse Analytics.
- Per la connessione tramite `ODBC` specificare `Driver={<Name of Driver>}`.
- `wasbs` è facoltativo ma consigliato per l'accesso agli account di Archiviazione di Azure perché i dati verranno inviati tramite una connessione TLS/SSL sicura.
- Le API `abfs` o `abfss` non sono supportate quando si accede agli account di Archiviazione di Azure.
- L'opzione relativa allo spazio dei nomi gerarchico per gli account di Archiviazione di Azure (v2) non è supportata. Verificare che questa opzione rimanga **disabilitata**.
- Per garantire la corretta esecuzione delle query di PolyBase durante un failover di `Namenode` di Hadoop, provare a usare un indirizzo IP virtuale per l'istanza di `Namenode` del cluster Hadoop. In caso contrario, eseguire un comando [ALTER EXTERNAL DATA SOURCE][alter_eds] in modo che punti alla nuova posizione.

### <a name="connection_options--key_value_pair"></a>CONNECTION_OPTIONS = *key_value_pair*

Specifica le opzioni aggiuntive quando si esegue la connessione a un'origine dati esterna tramite `ODBC`. Per usare più opzioni di connessione, separarle con un punto e virgola.


Il nome del driver è un requisito minimo, ma sono disponibili altre opzioni, ad esempio `APP='<your_application_name>'` o `ApplicationIntent= ReadOnly|ReadWrite`, che può essere utile impostare e possono essere usate per la risoluzione dei problemi.

Vedere la documentazione del prodotto `ODBC` per un elenco di valori consentiti per [CONNECTION_OPTIONS][connection_options]

### <a name="pushdown--on--off"></a>PUSHDOWN = *ON | OFF*

Indica se è possibile eseguire il pushdown del calcolo nell'origine dati esterna. È attivato per impostazione predefinita.

`PUSHDOWN` è supportato per la connessione a SQL Server, Oracle, Teradata, MongoDB oppure ODBC a livello di origine dati esterna.

Per abilitare o disabilitare il pushdown a livello di query, si usa un [hint][hint_pb].

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Specifica una credenziale con ambito database per l'autenticazione nell'origine dati esterna.

Note aggiuntive e indicazioni utili per la creazione delle credenziali:

- `CREDENTIAL` è obbligatorio solo se i dati sono stati protetti. `CREDENTIAL` non è obbligatorio per i set di dati che consentono l'accesso anonimo.
- Se `TYPE` = `BLOB_STORAGE`, è necessario specificare `SHARED ACCESS SIGNATURE` come identità per la creazione delle credenziali. È inoltre necessario configurare il token di firma di accesso condiviso nel modo seguente:
  - Escludere il carattere `?` iniziale quando viene configurato come segreto
  - Disporre almeno dell'autorizzazione di lettura per il file da caricare, ad esempio `srt=o&sp=r`
  - Usare un periodo di scadenza valido (tutte le date sono espresse in formato UTC).

Per un esempio d'uso di `CREDENTIAL` con `SHARED ACCESS SIGNATURE` e `TYPE` = `BLOB_STORAGE`, vedere [Creare un'origine dati esterna per eseguire operazioni bulk e recuperare dati da Archiviazione di Azure nel database SQL](#i-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage)

Per creare credenziali con ambito database, vedere [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc].

### <a name="type---hadoop--blob_storage-"></a>TYPE = *[ HADOOP | BLOB_STORAGE ]*

Specifica il tipo dell'origine dati esterna da configurare. Questo parametro non è sempre obbligatorio.

- Usare HADOOP quando l'origine dati esterna è Cloudera, Hortonworks o un account di Archiviazione di Azure.
- Usare BLOB_STORAGE durante l'esecuzione di operazioni bulk da un account di Archiviazione di Azure usando [BULK INSERT][bulk_insert] o [OPENROWSET][openrowset] con [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].

> [!IMPORTANT]
> Non impostare `TYPE` se si usa qualsiasi altra origine dati esterna.

Per un esempio d'uso di `TYPE` = `HADOOP` per caricare dati da un account di Archiviazione di Azure, vedere [Creare un'origine dati esterna per accedere ai dati in Archiviazione di Azure usando l'interfaccia wasb://](#e-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface) <!--[Create external data source to reference Azure Storage](#e-create-external-data-source-to-reference-azure-storage).-->

### <a name="resource_manager_location--resourcemanager_uriport"></a>RESOURCE_MANAGER_LOCATION = *'ResourceManager_URI[:port]'*

Configurare questo valore facoltativo per la connessione a Hortonworks o Cloudera.

Dopo aver definito `RESOURCE_MANAGER_LOCATION`, Query Optimizer prenderà una decisione basata sui costi per migliorare le prestazioni. È possibile usare un processo MapReduce per eseguire il pushdown del calcolo in Hadoop. Specificando il parametro `RESOURCE_MANAGER_LOCATION`, è possibile ridurre significativamente il volume dei dati trasferiti tra Hadoop e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quindi migliorare le prestazioni delle query.

Se non si specifica tale parametro, il push del calcolo in Hadoop è disabilitato per le query PolyBase.

Se la porta non è specificata, per la scelta del valore predefinito si usa l'impostazione corrente della configurazione 'hadoop connectivity'.

| Connettività Hadoop | Porta di gestione risorse predefinita |
| ------------------- | ----------------------------- |
| 1                   | 50300                         |
| 2                   | 50300                         |
| 3                   | 8021                          |
| 4                   | 8032                          |
| 5                   | 8050                          |
| 6                   | 8032                          |
| 7                   | 8050                          |

Per un elenco completo delle versioni di Hadoop supportate, vedere [Configurazione della connettività di PolyBase (Transact-SQL)][connectivity_pb].

> [!IMPORTANT]  
> Il valore RESOURCE_MANAGER_LOCATION non viene convalidato quando si crea l'origine dati esterna. Se si immette un valore errato, potrebbe verificarsi un errore di query in fase di esecuzione quando si prova a usare il pushdown perché non è possibile risolvere il valore specificato.

In [Creare un'origine dati esterna per fare riferimento a Hadoop con il pushdown abilitato](#c-create-external-data-source-to-reference-hadoop-with-push-down-enabled) viene fornito un esempio concreto, oltre a ulteriori indicazioni.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL` per il database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="locking"></a>Blocco

Acquisisce un blocco condiviso per l'oggetto `EXTERNAL DATA SOURCE`.

## <a name="security"></a>Sicurezza

PolyBase supporta l'autenticazione basata su proxy per la maggior parte delle origini dati esterne. Creare credenziali con ambito database per creare l'account proxy.

Quando ci si connette al pool di archiviazione o al pool di dati in un cluster di Big Data di SQL Server, le credenziali dell'utente vengono passate tramite il sistema back-end. Creare gli account di accesso direttamente nel pool di dati per abilitare l'autenticazione pass-through.

Il token di firma di accesso condiviso di tipo `HADOOP` non è attualmente supportato. È supportato solo con una chiave di accesso dell'account di archiviazione. Il tentativo di creare un'origine dati esterna di tipo `HADOOP` e le credenziali di firma di accesso condiviso potrebbe non riuscire e potrebbe essere visualizzato l'errore:

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples-starting-with-sssql15"></a>Esempi (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)])

> [!IMPORTANT]
> Per informazioni su come installare e abilitare PolyBase, vedere [Installare PolyBase in Windows](../../relational-databases/polybase/polybase-installation.md)

### <a name="a-create-external-data-source-in-sql-server-2019-to-reference-oracle"></a>R. Creare un'origine dati esterna in SQL Server 2019 per fare riferimento a Oracle

Per creare un'origine dati esterna che fa riferimento a Oracle, assicurarsi di disporre di credenziali con ambito database. È anche possibile abilitare o disabilitare il pushdown del calcolo su questa origine dati.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '!MyC0mpl3xP@ssw0rd!' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL OracleProxyAccount
WITH
     IDENTITY = 'oracle_username',
     SECRET = 'oracle_password' ;

CREATE EXTERNAL DATA SOURCE MyOracleServer
WITH
  ( LOCATION = 'oracle://145.145.145.145:1521',
    CREDENTIAL = OracleProxyAccount,
    PUSHDOWN = ON
  ) ;
```

Per altri esempi relativi a origini dati diverse, come MongoDB, vedere [Configurare PolyBase per l'accesso a dati esterni in MongoDB][mongodb_pb]

### <a name="b-create-external-data-source-to-reference-hadoop"></a>B. Creare un'origine dati esterna per fare riferimento a Hadoop

Per creare un'origine dati esterna per fare riferimento al cluster Hortonworks o Cloudera Hadoop, specificare il nome del computer o l'indirizzo IP di `Namenode` Hadoop e la porta. <!-- Provide the Nameservice ID as the `LOCATION` for highly available configurations. -->

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    TYPE = HADOOP
  ) ;
```

### <a name="c-create-external-data-source-to-reference-hadoop-with-push-down-enabled"></a>C. Creare un'origine dati esterna per fare riferimento a Hadoop con il pushdown abilitato

Specificare l'opzione `RESOURCE_MANAGER_LOCATION` per abilitare il pushdown del calcolo in Hadoop per le query PolyBase. Dopo l'abilitazione, PolyBase prende una decisione basata sui costi per determinare se eseguire il push del calcolo delle query in Hadoop.

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8020' ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  ) ;
```

### <a name="d-create-external-data-source-to-reference-kerberos-secured-hadoop"></a>D. Creare un'origine dati esterna per fare riferimento a Hadoop con protezione Kerberos

Per verificare se il cluster Hadoop è protetto tramite Kerberos, controllare il valore della proprietà hadoop.security.authentication nel file core-site.xml di Hadoop. Per fare riferimento a un cluster Hadoop protetto tramite Kerberos, è necessario specificare una credenziale con ambito database che contiene il nome utente e la password di Kerberos. La chiave master del database viene usata per crittografare il segreto della credenziale con ambito database.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
WITH
     IDENTITY = '<hadoop_user_name>',
     SECRET = '<hadoop_password>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    CREDENTIAL = HadoopUser1 ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  );
```

### <a name="e-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>E. Creare un'origine dati esterna per accedere ai dati in Archiviazione di Azure usando l'interfaccia wasb://
In questo esempio, l'origine dati esterna è un account di Archiviazione di Azure V2 denominato `logs`. il nome del contenitore è `daily`. L'origine dati esterna di Archiviazione di Azure è destinata al solo trasferimento dei dati e non supporta il pushdown dei predicati. Gli spazi dei nomi gerarchici non sono supportati quando si accede ai dati tramite l'interfaccia `wasb://`.

Questo esempio illustra come creare le credenziali con ambito di database per l'autenticazione in un account di Archiviazione di Azure V2. Specificare la chiave dell'account di Archiviazione di Azure nel segreto della credenziale di database. È possibile specificare qualsiasi stringa nell'identità della credenziale con ambito database perché non viene usata durante l'autenticazione in Archiviazione di Azure.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>' ,
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/' ,
    CREDENTIAL = AzureStorageCredential ,
    TYPE = HADOOP
  ) ;
```

### <a name="f-create-external-data-source-to-reference-a-sql-server-named-instance-via-polybase-connectivity-sql-server-2019"></a>F. Creare un'origine dati esterna per fare riferimento a un'istanza denominata di SQL Server tramite la connettività PolyBase ([!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

Per creare un'origine dati esterna che faccia riferimento a un'istanza denominata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile usare CONNECTION_OPTIONS per specificare il nome dell'istanza. Nell'esempio seguente `WINSQL2019` è il nome host e `SQL2019` è il nome dell'istanza.

```sql
CREATE EXTERNAL DATA SOURCE SQLServerInstance2
WITH (
  LOCATION = 'sqlserver://WINSQL2019' ,
  CONNECTION_OPTIONS = 'Server=%s\SQL2019' ,
  CREDENTIAL = SQLServerCredentials
) ;
```

In alternativa, è possibile usare una porta per connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

```sql
CREATE EXTERNAL DATA SOURCE SQLServerInstance2
WITH (
  LOCATION = 'sqlserver://WINSQL2019:58137' ,
  CREDENTIAL = SQLServerCredentials
) ;
```

### <a name="g-create-external-data-source-to-reference-kafka"></a>G. Creare un'origine dati esterna per fare riferimento a Kafka

In questo esempio, l'origine dati esterna è un server Kafka con indirizzo IP xxx.xxx.xxx.xxx e in ascolto sulla porta 1900. L'origine dati esterna Kafka viene usata solo per lo streaming dei dati e non supporta la distribuzione dei predicati.

```sql
-- Create an External Data Source for Kafka
CREATE EXTERNAL DATA SOURCE MyKafkaServer WITH (
    LOCATION = 'kafka://xxx.xxx.xxx.xxx:1900'
)
go
```

### <a name="h-create-external-data-source-to-reference-edgehub"></a>H. Creare un'origine dati esterna per fare riferimento a EdgeHub

In questo esempio, l'origine dati esterna è un EdgeHub in esecuzione nello stesso dispositivo perimetrale di SQL Edge di Azure. L'origine dati esterna EdgeHub viene usata solo per lo streaming dei dati e non supporta la distribuzione dei predicati.

```sql
-- Create an External Data Source for Kafka
CREATE EXTERNAL DATA SOURCE MyEdgeHub WITH (
    LOCATION = 'edgehub://'
)
go
```

## <a name="examples-bulk-operations"></a>Esempi: Operazioni bulk

> [!IMPORTANT]
> Non aggiungere un carattere **/** finale, un nome file o parametri di firma per l'accesso condiviso alla fine dell'URL `LOCATION` quando si configura un'origine dati esterne per le operazioni bulk.

### <a name="i-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage"></a>I. Creare un'origine dati esterna per le operazioni bulk che recuperano i dati da Archiviazione di Azure

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].
Usare l'origine dati seguente per le operazioni bulk che usano [BULK INSERT][bulk_insert] o [OPENROWSET][openrowset]. Le credenziali devono impostare `SHARED ACCESS SIGNATURE` come identità, non devono includere il carattere `?` iniziale nel token di firma di accesso condiviso, devono avere almeno un'autorizzazione di lettura per il file da caricare (ad esempio `srt=o&sp=r`). Inoltre il periodo di scadenza deve essere valido (tutte le date sono in formato UTC). Per altre informazioni sulle firme di accesso condiviso, vedere [Uso delle firme di accesso condiviso][sas_token].

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
WITH
  IDENTITY = 'SHARED ACCESS SIGNATURE',
  -- Remove ? from the beginning of the SAS token
  SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
WITH
  ( LOCATION = 'https://newinvoices.blob.core.windows.net/week3' ,
    CREDENTIAL = AccessAzureInvoices ,
    TYPE = BLOB_STORAGE
  ) ;
```

Per un esempio pratico, vedere l'esempio di [BULK INSERT][bulk_insert_example].

## <a name="see-also"></a>Vedere anche

- [ALTER EXTERNAL DATA SOURCE (Transact-SQL)][alter_eds]
- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Uso delle firme di accesso condiviso][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: ./create-external-table-transact-sql.md
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest&preserve-view=true

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown

<!-- Azure Docs -->
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Database SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Panoramica: database SQL di Azure

Crea un'origine dati esterna per le query elastiche. Le origini dati esterne vengono usate per stabilire la connettività e supportano questi casi d'uso principali:

- Operazioni di caricamento bulk con `BULK INSERT` o `OPENROWSET`
- Esecuzione di query su istanze remote del database SQL o di Azure Synapse con il database SQL con [query elastica][remote_eq]
- Esecuzione di query su un database SQL di Azure partizionato con [query elastica][sharded_eq]

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = { BLOB_STORAGE | RDBMS | SHARD_MAP_MANAGER } ]
    [ [ , ] DATABASE_NAME = '<database_name>' ]
    [ [ , ] SHARD_MAP_NAME = '<shard_map_manager>' ] )
[ ; ]
```

## <a name="arguments"></a>Argomenti

### <a name="data_source_name"></a>data_source_name

Specifica il nome definito dall'utente per l'origine dati. Il nome deve essere univoco all'interno del database nel database SQL.

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Fornisce il protocollo di connettività e il percorso dell'origine dati esterna.

| Origine dati esterna   | Prefisso della posizione | Percorso                                         |
| ---------------------- | --------------- | ----------------------------------------------------- |
| Operazioni bulk        | `https`         | `<storage_account>.blob.core.windows.net/<container>` |
| Query elastica (partizione)  | Facoltativo    | `<shard_map_server_name>.database.windows.net`        |
| Query elastica (remoto) | Facoltativo    | `<remote_server_name>.database.windows.net`           |

Percorso:

- `<shard_map_server_name>` = nome del server logico in Azure che ospita il gestore mappe partizioni. L'argomento `DATABASE_NAME` fornisce il database usato per ospitare la mappa partizioni, mentre `SHARD_MAP_NAME` viene usato per la mappa partizioni stessa.
- `<remote_server_name>` = nome del server logico di destinazione per la query elastica. Per specificare il nome del database, si usa l'argomento `DATABASE_NAME`.

Note aggiuntive e indicazioni utili per l'impostazione della posizione:

- Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] non verifica l'esistenza dell'origine dati esterna quando viene creato l'oggetto. Per eseguire la convalida, creare una tabella esterna usando l'origine dati esterna.

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Specifica una credenziale con ambito database per l'autenticazione nell'origine dati esterna.

Note aggiuntive e indicazioni utili per la creazione delle credenziali:

- Per caricare i dati da Archiviazione di Azure in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], usare una firma di accesso condiviso (token SAS).
- `CREDENTIAL` è obbligatorio solo se i dati sono stati protetti. `CREDENTIAL` non è obbligatorio per i set di dati che consentono l'accesso anonimo.
- Quando `TYPE` = `BLOB_STORAGE`, è necessario specificare `SHARED ACCESS SIGNATURE` come identità per la creazione delle credenziali. È inoltre necessario configurare il token di firma di accesso condiviso nel modo seguente:
  - Escludere il carattere `?` iniziale quando viene configurato come segreto
  - Disporre almeno dell'autorizzazione di lettura per il file da caricare, ad esempio `srt=o&sp=r`
  - Usare un periodo di scadenza valido (tutte le date sono espresse in formato UTC).

Per un esempio d'uso di `CREDENTIAL` con `SHARED ACCESS SIGNATURE` e `TYPE` = `BLOB_STORAGE`, vedere [Creare un'origine dati esterna per eseguire operazioni bulk e recuperare dati da Archiviazione di Azure nel database SQL](#c-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage)

Per creare credenziali con ambito database, vedere [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc].

### <a name="type---blob_storage--rdbms--shard_map_manager"></a>TYPE = *[ BLOB_STORAGE | RDBMS | SHARD_MAP_MANAGER]*

Specifica il tipo dell'origine dati esterna da configurare. Questo parametro non è sempre obbligatorio.

- Usare `RDBMS` per le query tra database usando una query elastica dal database SQL.
- Usare `SHARD_MAP_MANAGER` durante la creazione di un'origine dati esterna per la connessione a un database SQL partizionato.
- Usare `BLOB_STORAGE` durante l'esecuzione di operazioni bulk con [BULK INSERT][bulk_insert] o [OPENROWSET][openrowset].

> [!IMPORTANT]
> Non impostare `TYPE` se si usa qualsiasi altra origine dati esterna.

### <a name="database_name--database_name"></a>DATABASE_NAME = *database_name*

Configurare questo argomento quando `TYPE` è impostato su `RDBMS` o `SHARD_MAP_MANAGER`.

| TYPE              | Valore di DATABASE_NAME                                       |
| ----------------- | ------------------------------------------------------------ |
| RDBMS             | Nome del database remoto sul server specificando usando `LOCATION` |
| SHARD_MAP_MANAGER | Nome del database che funge da gestore mappe partizioni      |

Per un esempio relativo alla creazione di un'origine dati esterna in cui `TYPE` = `RDBMS`, vedere [Creare un'origine dati esterna RDBMS](#b-create-an-rdbms-external-data-source)

### <a name="shard_map_name--shard_map_name"></a>SHARD_MAP_NAME = *shard_map_name*

Usato quando l'argomento `TYPE` è impostato su `SHARD_MAP_MANAGER` solo per impostare il nome della mappa partizioni.

Per un esempio relativo alla creazione di un'origine dati esterna in cui `TYPE` = `SHARD_MAP_MANAGER`, vedere [Creare un'origine dati esterna del gestore mappe partizioni](#a-create-a-shard-map-manager-external-data-source)

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL` per il database in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

## <a name="locking"></a>Blocco

Acquisisce un blocco condiviso per l'oggetto `EXTERNAL DATA SOURCE`.

## <a name="examples"></a>Esempi:

### <a name="a-create-a-shard-map-manager-external-data-source"></a>R. Creare un'origine dati esterna del gestore mappe partizioni

Per creare un'origine dati esterna per fare riferimento a SHARD_MAP_MANAGER, specificare il nome del server di database SQL che ospita il gestore mappe partizioni nel database SQL in un database di SQL Server in una macchina virtuale.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
WITH
  IDENTITY = '<username>',
  SECRET = '<password>' ;

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc
WITH
  ( TYPE = SHARD_MAP_MANAGER ,
    LOCATION = '<server_name>.database.windows.net' ,
    DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb' ,
    CREDENTIAL = ElasticDBQueryCred ,
    SHARD_MAP_NAME = 'CustomerIDShardMap'
  ) ;
```

Per un'esercitazione dettagliata, vedere [Getting started with elastic queries for sharding (horizontal partitioning)][sharded_eq_tutorial] (Introduzione alle query di database elastico per il partizionamento orizzontale).

### <a name="b-create-an-rdbms-external-data-source"></a>B. Creare un'origine dati esterna RDBMS

Per creare un'origine dati esterna per fare riferimento a un RDBMS, specificare il nome del server di database SQL del database remoto nel database SQL.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL SQL_Credential
WITH
  IDENTITY = '<username>' ,
  SECRET = '<password>' ;

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc
WITH
  ( TYPE = RDBMS ,
    LOCATION = '<server_name>.database.windows.net' ,
    DATABASE_NAME = 'Customers' ,
    CREDENTIAL = SQL_Credential
  ) ;
```

Per un'esercitazione dettagliata su RDBMS, vedere [Introduzione alle query tra database (partizionamento verticale)][remote_eq_tutorial].

## <a name="examples-bulk-operations"></a>Esempi: Operazioni bulk

> [!IMPORTANT]
> Non aggiungere un carattere **/** finale, un nome file o parametri di firma per l'accesso condiviso alla fine dell'URL `LOCATION` quando si configura un'origine dati esterne per le operazioni bulk.

### <a name="c-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage"></a>C. Creare un'origine dati esterna per le operazioni bulk che recuperano i dati da Archiviazione di Azure

Usare l'origine dati seguente per le operazioni bulk che usano [BULK INSERT][bulk_insert] o [OPENROWSET][openrowset]. Le credenziali devono impostare `SHARED ACCESS SIGNATURE` come identità, non devono includere il carattere `?` iniziale nel token di firma di accesso condiviso, devono avere almeno un'autorizzazione di lettura per il file da caricare (ad esempio `srt=o&sp=r`). Inoltre il periodo di scadenza deve essere valido (tutte le date sono in formato UTC). Per altre informazioni sulle firme di accesso condiviso, vedere [Uso delle firme di accesso condiviso][sas_token].

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
WITH
  IDENTITY = 'SHARED ACCESS SIGNATURE',
  -- Remove ? from the beginning of the SAS token
  SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
WITH
  ( LOCATION = 'https://newinvoices.blob.core.windows.net/week3' ,
    CREDENTIAL = AccessAzureInvoices ,
    TYPE = BLOB_STORAGE
  ) ;
```

Per visualizzare l'esempio di utilizzo, vedere [BULK INSERT][bulk_insert_example].

## <a name="see-also"></a>Vedere anche

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Uso delle firme di accesso condiviso][sas_token]
- [Introduzione alla query elastica][intro_eq]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md
[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[alter_eds]: ./alter-external-data-source-transact-sql.md
[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Panoramica: Azure Synapse Analytics

Crea un'origine dati esterna per PolyBase. Le origini dati esterne vengono usate per stabilire la connettività e supportano il caso d'uso principale seguente: Virtualizzazione dati e caricamento dati con [PolyBase][intro_pb]

> [!IMPORTANT]  
> Per creare un'origine dati esterna per l'esecuzione di query su una risorsa SQL Analytics che usa il database SQL di Azure con [query elastica][remote_eq], vedere [Database SQL](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true).

## <a name="syntax"></a>Sintassi

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = HADOOP ]
[ ; ]
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>  
WITH
(    LOCATION = '<prefix>://<path>[:<port>]'
) 
[;]
```
---

## <a name="arguments"></a>Argomenti

### <a name="data_source_name"></a>data_source_name

Specifica il nome definito dall'utente per l'origine dati. Il nome deve essere univoco all'interno di [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Fornisce il protocollo di connettività e il percorso dell'origine dati esterna.

| Origine dati esterna        | Prefisso della posizione | Percorso                                         |
| --------------------------- | --------------- | ----------------------------------------------------- |
| Azure Data Lake Store Gen 1 | `adl`           | `<storage_account>.azuredatalake.net`                 |
| Azure Data Lake Store Gen 2 | `abfs[s]`       | `<container>@<storage_account>.dfs.core.windows.net`  |
| Account di archiviazione di Azure V2    | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |

Percorso:

- `<container>` = contenitore dell'account di archiviazione che include i dati. I contenitori radice sono di sola lettura, di conseguenza i dati non possono essere riscritti nel contenitore.
- `<storage_account>` = nome dell'account di archiviazione della risorsa di Azure.

Note aggiuntive e indicazioni utili per l'impostazione della posizione:

- L'opzione predefinita consiste nell'usare `enable secure SSL connections` quando si effettua il provisioning di Azure Data Lake Storage Gen 2. Quando questa opzione è abilitata, è necessario usare `abfss` quando viene selezionata una connessione TLS/SSL protetta. Si noti che `abfss` funziona anche per le connessioni TLS senza protezione.
- Azure Synapse non verifica l'esistenza dell'origine dati esterna quando viene creato l'oggetto. . Per eseguire la convalida, creare una tabella esterna usando l'origine dati esterna.
- Per garantire una semantica di esecuzione di query coerente, usare la stessa origine dati esterna per tutte le tabelle quando si eseguono query su Hadoop.
- `wasbs` è consigliato quando i dati vengono inviati usando una connessione TLS protetta
- Gli spazi dei nomi gerarchici non sono supportati con gli account di Archiviazione di Azure v2 quando si accede ai dati tramite PolyBase usando l'interfaccia wasb://.

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Specifica una credenziale con ambito database per l'autenticazione nell'origine dati esterna.

Note aggiuntive e indicazioni utili per la creazione delle credenziali:

- Per caricare i dati da Archiviazione di Azure o Azure Data Lake Store (ADLS) Gen 2 in Azure Synapse Analytics, usare una chiave di archiviazione di Azure.
- `CREDENTIAL` è obbligatorio solo se i dati sono stati protetti. `CREDENTIAL` non è obbligatorio per i set di dati che consentono l'accesso anonimo.

Per creare credenziali con ambito database, vedere [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc].

### <a name="type--hadoop"></a>TYPE = *HADOOP*

Specifica il tipo dell'origine dati esterna da configurare. Questo parametro non è sempre obbligatorio.

- Usare HADOOP quando l'origine dati esterna è Archiviazione di Azure, ADLS Gen 1 o ADLS Gen 2.

Per un esempio d'uso di `TYPE` = `HADOOP` per caricare i dati da Archiviazione di Azure, vedere [Creare un'origine dati esterna per fare riferimento ad Azure Data Lake Store Gen 1 o 2 usando un'entità servizio](#b-create-external-data-source-to-reference-azure-data-lake-store-gen-1-or-2-using-a-service-principal).

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL` per il database.

## <a name="locking"></a>Blocco

Acquisisce un blocco condiviso per l'oggetto `EXTERNAL DATA SOURCE`.

## <a name="security"></a>Sicurezza

PolyBase supporta l'autenticazione basata su proxy per la maggior parte delle origini dati esterne. Creare credenziali con ambito database per creare l'account proxy.

Quando ci si connette al pool di archiviazione o al pool di dati in un cluster di Big Data di SQL Server, le credenziali dell'utente vengono passate tramite il sistema back-end. Creare gli account di accesso direttamente nel pool di dati per abilitare l'autenticazione pass-through.

Il token di firma di accesso condiviso di tipo `HADOOP` non è attualmente supportato. È supportato solo con una chiave di accesso dell'account di archiviazione. Il tentativo di creare un'origine dati esterna di tipo `HADOOP` e le credenziali di firma di accesso condiviso potrebbe non riuscire e potrebbe essere visualizzato l'errore:

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples"></a>Esempi:

### <a name="a-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>R. Creare un'origine dati esterna per accedere ai dati in Archiviazione di Azure usando l'interfaccia wasb://
In questo esempio, l'origine dati esterna è un account di Archiviazione di Azure V2 denominato `logs`. il nome del contenitore è `daily`. L'origine dati esterna di Archiviazione di Azure è destinata al solo trasferimento dei dati e non supporta il pushdown dei predicati. Gli spazi dei nomi gerarchici non sono supportati quando si accede ai dati tramite l'interfaccia `wasb://`.

Questo esempio illustra come creare la credenziale con ambito database per l'autenticazione in Archiviazione di Azure. Specificare la chiave dell'account di Archiviazione di Azure nel segreto della credenziale di database. È possibile specificare qualsiasi stringa nell'identità della credenziale con ambito database perché non viene usata durante l'autenticazione nell'archiviazione di Azure.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>',
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/' ,
    CREDENTIAL = AzureStorageCredential ,
    TYPE = HADOOP
  ) ;
```

### <a name="b-create-external-data-source-to-reference-azure-data-lake-store-gen-1-or-2-using-a-service-principal"></a>B. Creare un'origine dati esterna per fare riferimento ad Azure Data Lake Storage Gen 1 o 2 usando un'entità servizio

La connettività di Azure Data Lake Store può essere basata sull'URI ADLS e sull'entità servizio dell'applicazione di Azure Active Directory. La documentazione relativa alla creazione di questa applicazione è disponibile in [Autenticazione da servizio a servizio con Data Lake Store tramite Azure Active Directory][azure_ad].

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

-- These values come from your Azure Active Directory Application used to authenticate to ADLS
CREATE DATABASE SCOPED CREDENTIAL ADLS_credential
WITH
  -- IDENTITY = '<clientID>@<OAuth2.0TokenEndPoint>' ,
  IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token' ,
  -- SECRET = '<KEY>'
  SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI=' 
;

-- For Gen 1 - Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen 1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  ( LOCATION = 'adl://newyorktaxidataset.azuredatalakestore.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;

-- For Gen 2 - Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen 2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  -- Please note the abfss endpoint when your account has secure transfer enabled
  ( LOCATION = 'abfss://data@newyorktaxidataset.dfs.core.windows.net' , 
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

### <a name="c-create-external-data-source-to-reference-azure-data-lake-store-gen-2-using-the-storage-account-key"></a>C. Creare un'origine dati esterna per fare riferimento ad Azure Data Lake Storage Gen 2 usando la chiave dell'account di archiviazione

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL ADLS_credential
WITH
-- IDENTITY = '<storage_account_name>' ,
  IDENTITY = 'newyorktaxidata' ,
-- SECRET = '<storage_account_key>'
  SECRET = 'yz5N4+bxSb89McdiysJAzo+9hgEHcJRJuXbF/uC3mhbezES/oe00vXnZEl14U0lN3vxrFKsphKov16C0w6aiTQ=='
;

-- Note this example uses a Gen 2 secured endpoint (abfss)
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( LOCATION = 'abfss://2013@newyorktaxidataset.dfs.core.windows.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

### <a name="d-create-external-data-source-to-reference-polybase-connectivity-to-azure-data-lake-store-gen-2-using-abfs"></a>D. Creare un'origine dati esterna per fare riferimento alla connettività Polybase ad Azure Data Lake Store Gen2 usando abfs://

Non è necessario specificare SECRET quando ci si connette a un account Azure Data Lake Store Gen2 con il meccanismo [Identità gestita](/azure/active-directory/managed-identities-azure-resources/overview
).

```sql
-- If you do not have a Master Key on your DW you will need to create one
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

--Create database scoped credential with **IDENTITY = 'Managed Service Identity'**

CREATE DATABASE SCOPED CREDENTIAL msi_cred 
WITH IDENTITY = 'Managed Service Identity' ;

--Create external data source with abfss:// scheme for connecting to your Azure Data Lake Store Gen2 account

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss 
WITH 
  ( TYPE = HADOOP , 
    LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net' , 
    CREDENTIAL = msi_cred
  ) ;
```

## <a name="see-also"></a>Vedere anche

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [CREATE EXTERNAL TABLE AS SELECT (Azure Synapse Analytics)][create_etb_as_sel]
- [CREATE TABLE AS SELECT (Azure Synapse Analytics)][create_tbl_as_sel]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Uso delle firme di accesso condiviso][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest&preserve-view=true

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Piattaforma di strumenti<br />analitici (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Panoramica: Sistema della piattaforma di analisi

Crea un'origine dati esterna per le query PolyBase. Le origini dati esterne vengono usate per stabilire la connettività e supportano il caso d'uso seguente: Virtualizzazione dati e caricamento dati con [PolyBase][intro_pb]

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = HADOOP ]
    [ [ , ] RESOURCE_MANAGER_LOCATION = '<resource_manager>[:<port>]' )
[ ; ]
```

## <a name="arguments"></a>Argomenti

### <a name="data_source_name"></a>data_source_name

Specifica il nome definito dall'utente per l'origine dati. Il nome deve essere univoco nel server in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Fornisce il protocollo di connettività e il percorso dell'origine dati esterna.

| Origine dati esterna    | Prefisso della posizione | Percorso                                         |
| ----------------------- | --------------- | ----------------------------------------------------- |
| Cloudera o Hortonworks | `hdfs`          | `<Namenode>[:port]`                                   |
| Account di archiviazione di Azure   | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |

Percorso:

- `<Namenode>` = nome del computer, URI del servizio dei nomi o indirizzo IP di `Namenode` nel cluster Hadoop. PolyBase deve risolvere tutti i nomi DNS usati dal cluster Hadoop. <!-- For highly available Hadoop configurations, provide the Nameservice ID as the `LOCATION`. -->
- `port` = porta su cui è in ascolto l'origine dati esterna. Per trovare la porta in Hadoop, si usa il parametro di configurazione `fs.defaultFS`. L'impostazione predefinita è 8020.
- `<container>` = contenitore dell'account di archiviazione che include i dati. I contenitori radice sono di sola lettura, di conseguenza i dati non possono essere riscritti nel contenitore.
- `<storage_account>` = nome dell'account di archiviazione della risorsa di Azure.

Note aggiuntive e indicazioni utili per l'impostazione della posizione:

- Il motore PDW non verifica l'esistenza dell'origine dati esterna quando viene creato l'oggetto. Per eseguire la convalida, creare una tabella esterna usando l'origine dati esterna.
- Per garantire una semantica di esecuzione di query coerente, usare la stessa origine dati esterna per tutte le tabelle quando si eseguono query su Hadoop.
- `wasbs` è consigliato quando i dati vengono inviati usando una connessione TLS protetta.
- Gli spazi dei nomi gerarchici non sono supportati se vengono usati con account di Archiviazione di Azure tramite wasb://.
- Per garantire la corretta esecuzione delle query di PolyBase durante un failover di `Namenode` di Hadoop, provare a usare un indirizzo IP virtuale per l'istanza di `Namenode` del cluster Hadoop. In caso contrario, eseguire un comando [ALTER EXTERNAL DATA SOURCE][alter_eds] in modo che punti alla nuova posizione.

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Specifica una credenziale con ambito database per l'autenticazione nell'origine dati esterna.

Note aggiuntive e indicazioni utili per la creazione delle credenziali:

- Per caricare i dati da Archiviazione di Azure in Azure Synapse o PDW, usare una chiave di Archiviazione di Azure.
- `CREDENTIAL` è obbligatorio solo se i dati sono stati protetti. `CREDENTIAL` non è obbligatorio per i set di dati che consentono l'accesso anonimo.

### <a name="type---hadoop-"></a>TYPE = *[ HADOOP ]*

Specifica il tipo dell'origine dati esterna da configurare. Questo parametro non è sempre obbligatorio.

- Usare HADOOP quando l'origine dati esterna è Cloudera, Hortonworks o Archiviazione di Azure.

Per un esempio d'uso di `TYPE` = `HADOOP` per caricare i dati da Archiviazione di Azure, vedere [Creare un'origine dati esterna per fare riferimento ad Hadoop](#a-create-external-data-source-to-reference-hadoop).

### <a name="resource_manager_location--resourcemanager_uriport"></a>RESOURCE_MANAGER_LOCATION = *'ResourceManager_URI[:port]'*

Configurare questo valore facoltativo per la connessione a Hortonworks o Cloudera.

Dopo aver definito `RESOURCE_MANAGER_LOCATION`, Query Optimizer prenderà una decisione basata sui costi per migliorare le prestazioni. È possibile usare un processo MapReduce per eseguire il pushdown del calcolo in Hadoop. Specificando il parametro `RESOURCE_MANAGER_LOCATION`, è possibile ridurre significativamente il volume dei dati trasferiti tra Hadoop e SQL e quindi migliorare le prestazioni delle query.

Se non si specifica tale parametro, il push del calcolo in Hadoop è disabilitato per le query PolyBase.

Se la porta non è specificata, per la scelta del valore predefinito si usa l'impostazione corrente della configurazione 'hadoop connectivity'.

| Connettività Hadoop | Porta di gestione risorse predefinita |
| ------------------- | ----------------------------- |
| 1                   | 50300                         |
| 2                   | 50300                         |
| 3                   | 8021                          |
| 4                   | 8032                          |
| 5                   | 8050                          |
| 6                   | 8032                          |
| 7                   | 8050                          |

Per un elenco completo delle versioni di Hadoop supportate, vedere [Configurazione della connettività di PolyBase (Transact-SQL)][connectivity_pb].

> [!IMPORTANT]
> Il valore `RESOURCE_MANAGER_LOCATION` non viene convalidato quando si crea l'origine dati esterna. Se si immette un valore errato, potrebbe verificarsi un errore di query in fase di esecuzione quando si prova a usare il pushdown perché non è possibile risolvere il valore specificato.

In [Creare un'origine dati esterna per fare riferimento a Hadoop con il pushdown abilitato](#b-create-external-data-source-to-reference-hadoop-with-push-down-enabled) viene fornito un esempio concreto, oltre a ulteriori indicazioni.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL` per il database in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

> [!NOTE]
> Nelle versioni precedenti di PDW creare le autorizzazioni `ALTER ANY EXTERNAL DATA SOURCE` richieste dell'origine dati esterna.

## <a name="locking"></a>Blocco

Acquisisce un blocco condiviso per l'oggetto `EXTERNAL DATA SOURCE`.

## <a name="security"></a>Sicurezza

PolyBase supporta l'autenticazione basata su proxy per la maggior parte delle origini dati esterne. Creare credenziali con ambito database per creare l'account proxy.

Il token di firma di accesso condiviso di tipo `HADOOP` non è attualmente supportato. È supportato solo con una chiave di accesso dell'account di archiviazione. Il tentativo di creare un'origine dati esterna di tipo `HADOOP` e le credenziali di firma di accesso condiviso potrebbe non riuscire e potrebbe essere visualizzato l'errore:

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples"></a>Esempi:

### <a name="a-create-external-data-source-to-reference-hadoop"></a>R. Creare un'origine dati esterna per fare riferimento a Hadoop

Per creare un'origine dati esterna per fare riferimento al cluster Hortonworks o Cloudera Hadoop, specificare il nome del computer o l'indirizzo IP di `Namenode` Hadoop e la porta. <!-- Provide the Nameservice ID as the `LOCATION` for highly available configurations. -->

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    TYPE = HADOOP
  ) ;
```

### <a name="b-create-external-data-source-to-reference-hadoop-with-push-down-enabled"></a>B. Creare un'origine dati esterna per fare riferimento a Hadoop con il pushdown abilitato

Specificare l'opzione `RESOURCE_MANAGER_LOCATION` per abilitare il pushdown del calcolo in Hadoop per le query PolyBase. Dopo l'abilitazione, PolyBase prende una decisione basata sui costi per determinare se eseguire il push del calcolo delle query in Hadoop.

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8020'
    TYPE = HADOOP
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
) ;
```

### <a name="c-create-external-data-source-to-reference-kerberos-secured-hadoop"></a>C. Creare un'origine dati esterna per fare riferimento a Hadoop con protezione Kerberos

Per verificare se il cluster Hadoop è protetto tramite Kerberos, controllare il valore della proprietà hadoop.security.authentication nel file core-site.xml di Hadoop. Per fare riferimento a un cluster Hadoop protetto tramite Kerberos, è necessario specificare una credenziale con ambito database che contiene il nome utente e la password di Kerberos. La chiave master del database viene usata per crittografare il segreto della credenziale con ambito database.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
WITH
  IDENTITY = '<hadoop_user_name>' ,
  SECRET = '<hadoop_password>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    CREDENTIAL = HadoopUser1 ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  ) ;
```

### <a name="d-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>D. Creare un'origine dati esterna per accedere ai dati in Archiviazione di Azure usando l'interfaccia wasb://

In questo esempio, l'origine dati esterna è un account di Archiviazione di Azure V2 denominato `logs`. il nome del contenitore è `daily`. L'origine dati esterna di Archiviazione di Azure è destinata al solo trasferimento dei dati e non supporta il pushdown dei predicati. Gli spazi dei nomi gerarchici non sono supportati quando si accede ai dati tramite l'interfaccia `wasb://`.

Questo esempio illustra come creare la credenziale con ambito database per l'autenticazione nell'archiviazione di Azure. Specificare la chiave dell'account di archiviazione di Azure nel segreto della credenziale di database. È possibile specificare qualsiasi stringa nell'identità della credenziale con ambito database perché non viene usata durante l'autenticazione nell'archiviazione di Azure.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>' ,
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/'
    CREDENTIAL = AzureStorageCredential
    TYPE = HADOOP
  ) ;
```


## <a name="see-also"></a>Vedere anche

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Uso delle firme di accesso condiviso][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest&preserve-view=true

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
