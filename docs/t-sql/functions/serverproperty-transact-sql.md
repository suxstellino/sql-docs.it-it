---
description: SERVERPROPERTY (Transact-SQL)
title: SERVERPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SERVERPROPERTY_TSQL
- SERVERPROPERTY
- sql13.swb.serverpropeties.connections.f1
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SERVERPROPERTY function
- server instance property information [SQL Server]
- IsHadrEnabled server property
- instances of SQL Server, property information
- server properties [SQL Server]
ms.assetid: 11e166fa-3dd2-42d8-ac4b-04f18c612c4a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aed106ed3915c779c558ff919128320f97174c8d
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241892"
---
# <a name="serverproperty-transact-sql"></a>SERVERPROPERTY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce informazioni sulle proprietà dell'istanza del server.  

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
SERVERPROPERTY ( 'propertyname' )  
```  

> [!IMPORTANT]
> I numeri di versione di [!INCLUDE[ssde_md](../../includes/ssde_md.md)] per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non sono confrontabili tra loro e rappresentano numeri di build interni per i prodotti separati. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] per [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è basato sulla stessa codebase di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. In particolare, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ha sempre i bit SQL [!INCLUDE[ssde_md](../../includes/ssde_md.md)] più recenti. La versione 12 di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è più recente della versione 15 di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*propertyname*  
Espressione che contiene le informazioni sulle proprietà da restituire per il server. *propertyname* può essere uno dei valori seguenti.  
  
|Proprietà|Valori di codice restituiti|  
|--------------|---------------------|  
|BuildClrVersion|Versione di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) di [!INCLUDE[msCoName](../../includes/msconame-md.md)] usata durante la compilazione dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|Regole di confronto|Nome delle regole di confronto predefinite per il server.<br /><br /> NULL = Input non valido o errore.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|CollationID|ID delle regole di confronto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Tipo di dati di base: **int**|  
|ComparisonStyle|Stile di confronto di Windows per le regole di confronto.<br /><br /> Tipo di dati di base: **int**|  
|ComputerNamePhysicalNetBIOS|Nome NetBIOS del computer locale in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Per un'istanza cluster di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un cluster di failover, questo valore cambia in caso di failover dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su altri nodi del cluster di failover.<br /><br /> In un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questo valore rimane costante e restituisce lo stesso valore della proprietà MachineName.<br /><br /> **Nota:** Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è inclusa in un cluster di failover e si vuole ottenere il nome dell'istanza del cluster di failover, usare la proprietà MachineName.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|Edizione|Edizione del prodotto installata per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Usare il valore di questa proprietà per determinare le funzionalità e i limiti, come ad esempio in [Limiti della capacità di calcolo per edizione di SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md). Nelle versioni a 64 bit del [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene aggiunta la dicitura (64 bit) al nome della versione stessa.<br /><br /> Restituisce:<br /><br /> 'Enterprise Edition'<br /><br /> 'Enterprise Edition: Core-based Licensing'<br /><br /> 'Enterprise Evaluation Edition'<br /><br /> 'Business Intelligence Edition'<br /><br /> 'Developer Edition'<br /><br /> 'Express Edition'<br /><br /> 'Express Edition with Advanced Services'<br /><br /> 'Standard Edition'<br /><br /> 'Web Edition'<br /><br /> 'SQL Azure' indica [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 'Azure SQL Edge Developer' indica l'edizione di solo sviluppo per SQL Edge di Azure<br /><br /> 'SQL Edge di Azure' indica l'edizione a pagamento per SQL Edge di Azure<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|EditionID|Rappresenta l'edizione del prodotto installata per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Usare il valore di questa proprietà per determinare le funzionalità e i limiti, come ad esempio in [Limiti della capacità di calcolo per edizione di SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).<br /><br /> 1804890536 = Enterprise<br /><br /> 1872460670 = Enterprise Edition: Core-based Licensing<br /><br /> 610778273 = Enterprise Evaluation<br /><br /> 284895786 = Business Intelligence<br /><br /> -2117995310 = Developer<br /><br /> -1592396055 = Express<br /><br /> -133711905= Express with Advanced Services<br /><br /> -1534726760 = Standard<br /><br /> 1293598313 = Web<br /><br /> 1674378470 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]<br /><br /> -1461570097 = SQL Edge di Azure Developer <br /><br /> 1994083197 = SQL Edge di Azure <br /><br />Tipo di dati di base: **bigint**|  
|EngineEdition|Edizione del [!INCLUDE[ssDE](../../includes/ssde-md.md)] dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installata nel server.<br /><br /> 1 = Personal o Desktop Engine: non disponibile in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive.<br /><br /> 2 = Standard: valore restituito per le edizioni Standard, Web e Business Intelligence.<br /><br /> 3 = Enterprise: valore restituito per le edizioni Evaluation, Developer ed Enterprise.<br /><br /> 4 = Express: valore restituito per le edizioni Express, Express with Tools ed Express with Advanced Services.<br /><br /> 5 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<br /><br /> 6 = [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 8 = [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]<br /><br /> 9 = SQL Edge di Azure (restituito per tutte le edizioni di SQL Edge di Azure)<br /><br /> 11 = Pool SQL serverless di Azure Synapse<br /><br /> Tipo di dati di base: **int**|  
|FilestreamConfiguredLevel|Livello di accesso di FILESTREAM configurato. Per altre informazioni, vedere [Livello di accesso FILESTREAM](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).<br /><br /> Tipo di dati di base: **int**|  
|FilestreamEffectiveLevel|Livello di accesso di FILESTREAM effettivo. Questo valore può essere diverso da FilestreamConfiguredLevel se il livello è stato modificato e vi è un'operazione di riavvio dell'istanza o del computer in sospeso. Per altre informazioni, vedere [Livello di accesso FILESTREAM](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).<br /><br /> Tipo di dati di base: **int**|  
|FilestreamShareName|Nome della condivisione utilizzata da FILESTREAM.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**| 
|HadrManagerStatus|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Indica se Gestione [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] è stata avviata.<br /><br /> 0 = non avviata, comunicazione in sospeso.<br /><br /> 1 = Avviata e in esecuzione.<br /><br /> 2 = Non avviata e non riuscita.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|InstanceDefaultBackupPath|**Si applica a**: [!INCLUDE[ssSQL2019](../../includes/sssqlv15-md.md)] e versioni successive.<br /><br /> Nome del percorso predefinito per i file di backup dell'istanza.|  
|InstanceDefaultDataPath|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> Nome del percorso predefinito per i file di dati dell'istanza.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|InstanceDefaultLogPath|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> Nome del percorso predefinito per i file di log dell'istanza.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|InstanceName|Nome dell'istanza a cui è connesso l'utente.<br /><br /> Restituisce NULL se il nome dell'istanza corrisponde all'istanza predefinita oppure in caso di input non valido o di errore.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|IsAdvancedAnalyticsInstalled|Restituisce 1 se la funzionalità di Advanced Analytics è stata installata durante l'installazione; 0 se Advanced Analytics non è stato installato.<br /><br /> Tipo di dati di base: **int**|  
|IsBigDataCluster| Introdotta in [!INCLUDE[ssSQL2019](../../includes/sssqlv15-md.md)] a partire da CU4.<br /><br />Restituisce 1 se l'istanza è un cluster Big Data di SQL Server; 0 in caso contrario.<br /><br /> Tipo di dati di base: **int**|  
|IsClustered|L'istanza del server è configurata in un cluster di failover.<br /><br /> 1 = Cluster.<br /><br /> 0 = Non cluster.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|IsFullTextInstalled|Indica se i componenti di indicizzazione full-text e semantica sono installati nell'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = i componenti di indicizzazione full-text e semantica sono installati.<br /><br /> 0 = i componenti di indicizzazione full-text e semantica non sono installati.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|IsHadrEnabled|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] è abilitato in questa istanza del server.<br /><br /> 0 = la funzionalità [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] è disabilitata.<br /><br /> 1 = la funzionalità [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] è abilitata.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**<br /><br /> Affinché sia possibile creare ed eseguire repliche di disponibilità in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] deve essere abilitato nell'istanza del server. Per altre informazioni, vedere [Abilitare e disabilitare la funzionalità Gruppi di disponibilità Always On (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).<br /><br /> **Nota:** la proprietà IsHadrEnabled è applicabile solo a [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Altre funzionalità a disponibilità elevata o di ripristino di emergenza, quali il mirroring del database o il log shipping, non sono interessate da questa proprietà server.|  
|IsIntegratedSecurityOnly|Indica se il server è in modalità di sicurezza integrata.<br /><br /> 1 = Sicurezza integrata (autenticazione di Windows)<br /><br /> 0 = Sicurezza integrata non attivata. Sia l'autenticazione di Windows sia l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|IsLocalDB|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Il server è un'istanza del database locale di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|IsPolyBaseInstalled|**Si applica a**: [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Determina se nell'istanza del server è installata la funzionalità PolyBase.<br /><br /> 0 = PolyBase non è installato.<br /><br /> 1 = PolyBase è installato.<br /><br /> Tipo di dati di base: **int**|  
|IsSingleUser|Indica se nel server è attivata o meno la modalità utente singolo.<br /><br /> 1 = Modalità utente singolo attivata.<br /><br /> 0 = Modalità utente singolo non attivata.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|IsTempDbMetadataMemoryOptimized|**Si applica a**: [!INCLUDE[ssSQL2019](../../includes/sssqlv15-md.md)] e versioni successive.<br /><br />Restituisce 1 se tempdb è stato abilitato per l'uso di tabelle ottimizzate per la memoria per i metadati. 0 se tempdb usa tabelle regolari basate su disco per i metadati. Per altre informazioni, vedere [tempdb Database](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).<br /><br /> Tipo di dati di base: **int**|  
|IsXTPSupported|**Si applica a**: SQL Server ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive), [!INCLUDE[ssSDS](../../includes/sssds-md.md)].<br /><br /> Il server supporta il motore OLTP in memoria.<br /><br /> 1= Il server supporta il motore OLTP in memoria.<br /><br /> 0= Il server non supporta il motore OLTP in memoria.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|LCID|Identificatore delle impostazioni locali (LCID) di Windows per le regole di confronto.<br /><br /> Tipo di dati di base: **int**|  
|LicenseType|Non utilizzato. Le informazioni sulla licenza non sono mantenute o non sono gestite dal prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Restituisce sempre DISABLED.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|MachineName|Nome del computer Windows in cui è in esecuzione l'istanza del server.<br /><br /> Nel caso di un'istanza cluster, un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguita in un server virtuale con Microsoft Cluster Services restituisce il nome del server virtuale.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|NumLicenses|Non utilizzato. Le informazioni sulla licenza non sono mantenute o non sono gestite dal prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Viene restituito sempre NULL.<br /><br /> Tipo di dati di base: **int**|  
|ProcessID|ID di processo del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. ProcessID è utile per identificare quale Sqlservr.exe appartiene all'istanza.<br /><br /> NULL = input non valido, errore o non applicabile.<br /><br /> Tipo di dati di base: **int**|  
|ProductBuild|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a partire da ottobre 2015.<br /><br /> Numero di build.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductBuildType|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> Tipo di build della build corrente.<br /><br /> Restituisce una delle operazioni seguenti:<br /><br /> OD = versione su richiesta di un cliente specifico.<br /><br /> GDR = distribuzione generale rilasciata tramite Windows Update.<br /><br /> NULL = non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductLevel|Livello della versione dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Restituisce una delle operazioni seguenti:<br /><br /> 'RTM' = Versione originale<br /><br /> 'SP *n*' = Versione Service Pack<br /><br /> 'CTP *n*' = versione Community Technology Preview<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductMajorVersion|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> La versione principale.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductMinorVersion|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> La versione secondaria.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductUpdateLevel|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> Livello di aggiornamento della build corrente. CU indica un aggiornamento cumulativo.<br /><br /> Restituisce una delle operazioni seguenti:<br /><br /> CU *n* = Aggiornamento cumulativo<br /><br /> NULL = non applicabile.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductUpdateReference|**Si applica a**: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] alla versione corrente con aggiornamenti a partire da fine 2015.<br /><br /> Articolo della Knowledge Base per la versione.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ProductVersion|Versione dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel formato "*principale.secondaria.build.revisione*".<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ResourceLastUpdateDateTime|Restituisce la data e l'ora dell'ultimo aggiornamento del database Resource.<br /><br /> Tipo di dati di base: **datetime**|  
|ResourceVersion|Restituisce la versione del database Resource.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ServerName|Informazioni relative sia al server Windows che all'istanza associate all'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = Input non valido o errore.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|SqlCharSet|ID del set di caratteri SQL dall'ID delle regole di confronto.<br /><br /> Tipo di dati di base: **tinyint**|  
|SqlCharSetName|Nome del set di caratteri SQL dalle regole di confronto.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|SqlSortOrder|ID del tipo di ordinamento SQL dalle regole di confronto.<br /><br /> Tipo di dati di base: **tinyint**|  
|SqlSortOrderName|Nome dell'ordinamento SQL dalle regole di confronto.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
  
## <a name="return-types"></a>Tipi restituiti  

**sql_variant**
  
## <a name="remarks"></a>Osservazioni  
  
### <a name="servername-property"></a>Proprietà ServerName

La proprietà `ServerName` della funzione `SERVERPROPERTY` e [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) restituiscono informazioni simili. La proprietà `ServerName` restituisce il nome del server Windows e il nome dell'istanza che insieme compongono il nome univoco dell'istanza del server. [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) restituisce il nome del server locale attualmente configurato.  
  
La proprietà `ServerName` e [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) restituiscono le stesse informazioni se il nome del server impostato come predefinito al momento dell'installazione non è stato modificato. Per configurare il nome del server locale eseguire l'istruzione seguente:  
  
```sql
EXEC sp_dropserver 'current_server_name';  
GO  
EXEC sp_addserver 'new_server_name', 'local';  
GO  
```  
  
Se il nome del server locale è stato modificato rispetto al nome del server impostato come predefinito al momento dell'installazione, [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) restituisce il nuovo nome.  
  
### <a name="version-properties"></a>Proprietà della versione

La funzione `SERVERPROPERTY` restituisce proprietà singole che si riferiscono alle informazioni sulla versione, mentre la funzione [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md) combina l'output in una stringa. Se l'applicazione richiede stringhe della proprietà singole, è possibile usare la funzione `SERVERPROPERTY` per restituirle anziché analizzare i risultati di [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md).  

## <a name="permissions"></a>Autorizzazioni

Tutti gli utenti possono eseguire una query sulle proprietà del server.
  
## <a name="examples"></a>Esempi

Nell'esempio seguente viene usata la funzione `SERVERPROPERTY`in un'istruzione `SELECT` per restituire informazioni sull'istanza corrente di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
  
```sql
SELECT  
  SERVERPROPERTY('MachineName') AS ComputerName,
  SERVERPROPERTY('ServerName') AS InstanceName,  
  SERVERPROPERTY('Edition') AS Edition,
  SERVERPROPERTY('ProductVersion') AS ProductVersion,  
  SERVERPROPERTY('ProductLevel') AS ProductLevel;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche

[Edizioni e componenti di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  
