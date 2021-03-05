---
title: Funzionalità del motore di database deprecate in SQL Server 2017 | Microsoft Docs
titleSuffix: SQL Server 2019
description: Informazioni sulle funzionalità del motore di database deprecate ancora disponibili in SQL Server 2017 (14.x), ma che non devono essere usate nelle nuove applicazioni.
ms.custom: seo-lt-2019
ms.date: 03/03/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: e3b27292b817d0ec0bf22afcc1e8eaab61864e82
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186444"
---
# <a name="deprecated-database-engine-features-in-sql-server-2017"></a>Funzionalità del motore di database deprecate in SQL Server 2017

[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]

  Questo articolo descrive le funzionalità deprecate [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ancora disponibili in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] . È consigliabile non usare le funzionalità deprecate nelle nuove applicazioni.  
  
Quando una funzionalità è contrassegnata come deprecata significa che:
- La funzionalità è solo in modalità manutenzione. Non verrà apportata alcuna nuova modifica, inclusi quelli relativi all'interoperabilità tra le nuove funzionalità.
- Microsoft si impegna a non rimuovere una funzionalità deprecata dalle versioni future per semplificare gli aggiornamenti. In rare situazioni, tuttavia, è possibile scegliere di interrompere definitivamente (rimuovere) la funzionalità da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se limita le innovazioni future.
- Per le nuove attività di sviluppo, non usare le funzionalità deprecate. Per le applicazioni esistenti, pianificare la modifica delle applicazioni che attualmente utilizzano queste funzionalità il prima possibile.     

È possibile monitorare l'utilizzo delle funzionalità deprecate tramite il contatore delle prestazioni [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :Funzionalità deprecate e gli eventi di traccia. Per altre informazioni, vedere [Usare oggetti di SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

I valori di questi contatori sono disponibili anche tramite l'istruzione seguente:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

> [!NOTE]
> Questo elenco è identico all'elenco [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]. Non sono presenti funzionalità nuove o deprecate del motore di database annunciate per [!INCLUDE[sssql17-md](../includes/sssql17-md.md)].

## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>Funzionalità deprecate nella prossima versione di SQL Server
Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] funzionalità seguenti non saranno supportate in una versione futura di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Non usare queste funzionalità in un nuovo progetto di sviluppo e modificare non appena possibile le applicazioni in cui sono attualmente implementate. Il valore **Nome funzionalità** viene visualizzato negli eventi di traccia come ObjectName e nei contatori delle prestazioni e in `sys.dm_os_performance_counters` come nome dell'istanza. Il valore **ID funzionalità** viene visualizzato negli eventi di traccia come ID dell'oggetto.

### <a name="back-up-and-restore"></a>Backup e ripristino

| Funzionalità deprecata | Sostituzione | Nome funzionalità | ID funzionalità |
|--------------------|-------------|--------------|------------|
| RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD continua a essere deprecata.<br /><br />BACKUP { DATABASE &#124; LOG } WITH PASSWORD e BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD non sono più disponibili. | No. | BACKUP DATABASE o LOG WITH PASSWORD<br /><br />BACKUP DATABASE o LOG WITH MEDIAPASSWORD | 104<br /><br /> 103 |

### <a name="compatibility-levels"></a>Livelli di compatibilità

| Funzionalità deprecata | Sostituzione | Nome funzionalità | ID funzionalità |
|--------------------|-------------|--------------|------------|
Aggiornamento dalla versione 100 ([!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]). | Quando una [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] versione non è più [supportata](/lifecycle/products/?products=sql-server), il livello di compatibilità del database associato è contrassegnato come deprecato. Microsoft continua tuttavia a supportare per quanto possibile le applicazioni certificate in qualsiasi livello di compatibilità del database supportato al fine di facilitare gli aggiornamenti. Per informazioni sui livelli di compatibilità supportati, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Livello di compatibilità del database: 100 | 108 |

### <a name="database-objects"></a>Oggetti di database

| Funzionalità deprecata | Sostituzione | Nome funzionalità | ID funzionalità |
|--------------------|-------------|--------------|------------|
| Possibilità di restituire set di risultati dai trigger | nessuno | Restituzione di risultati da un trigger | 12 |

### <a name="encryption"></a>Crittografia

| Funzionalità deprecata | Sostituzione | Nome funzionalità | ID funzionalità |
|--------------------|-------------|--------------|------------|
| Crittografia tramite RC4 o RC4_128 deprecata. Rimozione pianificata nella prossima versione. Decrittografia RC4 e RC4_128 non deprecate. | Utilizzare un'altra crittografia, ad esempio AES. | Algoritmo di crittografia deprecata | 253 |
| L'uso di MD2, MD4, MD5, SHA e SHA1 è deprecato. | Usare invece SHA2_256 o SHA2_512. Gli algoritmi precedenti continuano a funzionare, ma generano un evento Deprecation. |Algoritmo hash deprecato | nessuno |

### <a name="remote-servers"></a>Server remoti

| Funzionalità deprecata | Sostituzione | Nome funzionalità | ID funzionalità |
|--------------------|-------------|--------------|------------|
| sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br /> sp_remoteoption|Sostituire i server remoti utilizzando server collegati. sp_addserver può essere usata solo con l'opzione locale. | sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br > sp_remoteoption | 70 <br /><br /> 69 <br /><br /> 71 <br /><br /> 72 <br /><br /> 73 |
| \@\@remserver | Sostituire i server remoti utilizzando server collegati. | nessuno | nessuno |
| SET REMOTE_PROC_TRANSACTIONS|Sostituire i server remoti utilizzando server collegati. | SET REMOTE_PROC_TRANSACTIONS | 110 |

### <a name="transact-sql"></a>Transact-SQL

| Funzionalità deprecata | Sostituzione | Nome funzionalità | ID funzionalità |
|--------------------|-------------|--------------|------------|
| **SET ROWCOUNT** per istruzioni **INSERT**, **UPDATE** e **DELETE** | Parola chiave TOP | SET ROWCOUNT | 109 |
| Hint di tabella HOLDLOCK senza parentesi | Utilizzare HOLDLOCK con parentesi. | Hint di tabella HOLDLOCK senza parentesi | 167 |

## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>Funzionalità deprecate in una versione futura di SQL Server

Le funzionalità seguenti del motore di database di SQL Server sono supportate nella versione successiva di SQL Server. La versione specifica di SQL Server non è stata determinata.

### <a name="back-up-and-restore"></a>Backup e ripristino

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| BACKUP { DATABASE &#124; LOG } TO TAPE <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*|BACKUP { DATABASE &#124; LOG } TO DISK <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk* | BACKUP DATABASE o LOG TO TAPE |
| sp_addumpdevice '**tape**' | sp_addumpdevice '**disk**' | ADDING TAPE DEVICE |
| sp_helpdevice | sys.backup_devices | sp_helpdevice |

### <a name="compatibility-levels"></a>Livelli di compatibilità

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_dbcmptlevel|ALTER DATABASE ... SET COMPATIBILITY_LEVEL. Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | sp_dbcmptlevel |
| Livello di compatibilità 110 e 120 del database. | Pianificare l'aggiornamento del database e dell'applicazione per una versione successiva. Microsoft continua tuttavia a supportare per quanto possibile le applicazioni certificate in qualsiasi livello di compatibilità del database supportato al fine di facilitare gli aggiornamenti. Per informazioni sui livelli di compatibilità supportati, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Livello di compatibilità 110 del database <br /><br /> Livello di compatibilità 120 del database |

### <a name="collations"></a>Regole di confronto

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS | No. Queste regole di confronto sono presenti in SQL Server 2005 (9.x), ma non è possibile visualizzarle tramite fn_helpcollations. | Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS |
| Hindi <br /><br /> Macedone | Queste regole di confronto sono presenti in SQL Server 2005 (9.x) e versioni successive, ma non è possibile visualizzarle tramite fn_helpcollations. Utilizzare Macedonian_FYROM_90 e Indic_General_90.|Hindi <br /><br /> Macedone |
| Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 | Azeri_Latin_100 <br /><br /> Azeri_Cyrilllic_100 | Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 |

### <a name="data-types"></a>Tipi di dati

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_addtype <br /><br /> sp_droptype|CREATE TYPE<br /><br /> DROP TYPE | sp_addtype<br /><br /> sp_droptype |
| Sintassi **timestamp** per il tipo di dati **rowversion** | Sintassi del tipo di dati **rowversion** | timestamp |
| Possibilità di inserire valori Null in colonne di tipo **timestamp** . | Utilizzare DEFAULT. | INSERT NULL in colonne TIMESTAMP |
| Opzione di tabella 'text in row'|Usare i tipi di dati **varchar(max)** , **nvarchar(max)** e **varbinary(max)** . Per altre informazioni, vedere [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|Opzione di tabella text in row |
| Tipi di dati:<br /><br /> **text**<br /><br /> **ntext**<br /><br /> **image**|Usare i tipi di dati **varchar(max)** , **nvarchar(max)** e **varbinary(max)** .|Tipi di dati: **text**, **ntext** o **image** |

### <a name="database-management"></a>Gestione di database

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_attach_db <br /><br /> sp_attach_single_file_db|Istruzione CREATE DATABASE con l'opzione FOR ATTACH. Per ricompilare più file di log in caso di nuovo percorso di uno o più di questi file, utilizzare l'opzione FOR ATTACH_REBUILD_LOG. | sp_attach_db <br /><br /> sp_attach_single_file_db |
| sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable |
| sp_dbremove | DROP DATABASE | sp_dbremove |
| sp_renamedb | MODIFY NAME in ALTER DATABASE | sp_renamedb |

### <a name="database-objects"></a>Oggetti di database

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|Parola chiave DEFAULT in CREATE TABLE e ALTER TABLE|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault |
| CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule | Parola chiave CHECK in CREATE TABLE e ALTER TABLE | CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule |
| sp_change_users_login | Utilizzare ALTER USER. | sp_change_users_login |
| sp_depends | sys.dm_sql_referencing_entities e sys.dm_sql_referenced_entities | sp_depends |
| sp_getbindtoken | Utilizzare MARS o transazioni distribuite. | sp_getbindtoken |

### <a name="database-options"></a>Opzioni di database

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_bindsession | Utilizzare MARS o transazioni distribuite. | sp_bindsession |
| sp_resetstatus | ALTER DATABASE SET { ONLINE &#124; EMERGENCY } | sp_resetstatus
| Opzione TORN_PAGE_DETECTION di ALTER DATABASE|Opzione PAGE_VERIFY TORN_PAGE_DETECTION di ALTER DATABASE | ALTER DATABASE WITH TORN_PAGE_DETECTION |

### <a name="dbcc"></a>DBCC

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| DBCC DBREINDEX|Opzione REBUILD di ALTER INDEX | DBCC DBREINDEX |
| DBCC INDEXDEFRAG|Opzione REORGANIZE di ALTER INDEX | DBCC INDEXDEFRAG |
| DBCC SHOWCONTIG|sys.dm_db_index_physical_stats | DBCC SHOWCONTIG |
| DBCC PINTABLE<br /><br /> DBCC UNPINTABLE | Non ha alcun effetto. | DBCC [UN]PINTABLE |

### <a name="extended-properties"></a>Proprietà estese

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Level0type = 'type' e Level0type = 'USER' per l'aggiunta di proprietà estese a oggetti Type di livello 1 o 2. | Utilizzare Level0type = 'USER' soltanto per aggiungere una proprietà estesa direttamente a un utente o un ruolo.<br /><br /> Utilizzare Level0type = 'SCHEMA' per aggiungere una proprietà estesa a tipi di livello 1, ad esempio TABLE o VIEW, oppure a tipi di livello 2, ad esempio COLUMN o TRIGGER. Per altre informazioni, vedere [sp_addextendedproperty &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md). | EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER |

### <a name="extended-stored-procedures"></a>Stored procedure estese

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Utilizzare CREATE_LOGIN<br /><br /> Utilizzare l'argomento DROP LOGIN IsIntegratedSecurityOnly di SERVERPROPERTY.|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig |

### <a name="extended-stored-procedures-programming"></a>Programmazione di stored procedure estese

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg | Utilizzare invece la funzionalità di integrazione CLR. | XP_API |
| sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc | Utilizzare invece la funzionalità di integrazione CLR. | sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc |
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Utilizzare CREATE_LOGIN<br /><br /> Utilizzare l'argomento DROP LOGIN IsIntegratedSecurityOnly di SERVERPROPERTY. | xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig |

### <a name="high-availability"></a>Disponibilità elevata

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| mirroring del database | Gruppi di disponibilità AlwaysOn<br /><br /> Se l'edizione di SQL Server non supporta Gruppi di disponibilità Always On, usare il log shipping. | DATABASE_MIRRORING |

### <a name="index-options"></a>Opzioni per indici

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_indexoption | ALTER INDEX | sp_indexoption |
| Sintassi di CREATE TABLE, ALTER TABLE o CREATE INDEX senza parentesi per racchiudere le opzioni. | Riscrivere le istruzioni in modo che utilizzino la sintassi corrente. | INDEX_OPTION |

### <a name="instance-options"></a>Opzioni di istanza

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Opzione di sp_configure 'allow updates'|Le tabelle di sistema non sono più aggiornabili. L'impostazione non ha alcun effetto. | sp_configure 'allow updates' |
| Opzioni di sp_configure: <br /><br /> 'locks' <br /><br /> 'open objects'<br /><br /> 'set working set size' | Configurata automaticamente. L'impostazione non ha alcun effetto. | sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size' |
| Opzione di sp_configure 'priority boost' | Le tabelle di sistema non sono più aggiornabili. L'impostazione non ha alcun effetto. In alternativa, usare l'opzione start /high ... program.exe di Windows. | sp_configure 'priority boost' |
| Opzione di sp_configure 'remote proc trans' | Le tabelle di sistema non sono più aggiornabili. L'impostazione non ha alcun effetto. | sp_configure 'remote proc trans' |

### <a name="linked-servers"></a>Server collegati

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Specifica del provider SQLOLEDB per i server collegati. | SQL Server Native Client (SQLNCLI) | SQLOLEDB per server collegati |

### <a name="metadata"></a>Metadati

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| FILE_ID<br /><br /> INDEXKEY_PROPERTY | FILE_IDEX<br /><br /> sys.index_columns | FILE_ID<br /><br /> INDEXKEY_PROPERTY |

### <a name="native-xml-web-services"></a>Servizi Web XML nativi

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Istruzione CREATE o ALTER ENDPOINT con l'opzione FOR SOAP.<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|Usare Windows Communications Foundation (WCF) o ASP.NET. | CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints |

### <a name="other"></a>Altri

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| DB-Library<br /><br />Embedded SQL for C|Nonostante supporti connessioni da applicazioni esistenti che usano le API DB-Library ed Embedded SQL, il motore di database non include la documentazione o i file necessari per svolgere attività di programmazione per applicazioni che usano tali API. In una versione futura del motore di database di SQL Server verrà eliminato il supporto per le connessioni da applicazioni DB-Library o Embedded SQL. Non utilizzare pertanto DB-Library o Embedded SQL per sviluppare nuove applicazioni. Quando si modificano applicazioni esistenti, rimuovere tutte le dipendenze da DB-Library o Embedded SQL. Invece di queste API, usare lo spazio dei nomi SQLClient o un'API, ad esempio ODBC. SQL Server 2019 (15.x) non include la DLL DB-Library necessaria per eseguire queste applicazioni. Per eseguire applicazioni DB-Library o Embedded SQL, deve essere disponibile la DLL DB-Library di SQL Server versione 6.5, SQL Server 7.0 o SQL Server 2000 (8.x). | nessuno |

### <a name="security"></a>Sicurezza

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Sintassi di ALTER LOGIN WITH SET CREDENTIAL | Nuova sintassi di ALTER LOGIN ADD e DROP CREDENTIAL | ALTER LOGIN WITH SET CREDENTIAL |
| sp_addapprole<br /><br /> sp_dropapprole | CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE | sp_addapprole<br /><br /> sp_dropapprole |
| sp_addlogin<br /><br /> sp_droplogin | CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin |
| sp_adduser<br /><br /> sp_dropuser | CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser |
| sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess |
| sp_addrole<br /><br /> sp_droprole | CREATE ROLE<br /><br /> DROP ROLE|sp_addrole<br /><br /> sp_droprole |
| sp_approlepassword<br /><br /> sp_password | ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password |
| sp_changedbowner|ALTER AUTHORIZATION | sp_changedbowner |
| sp_changeobjectowner|ALTER SCHEMA o ALTER AUTHORIZATION | sp_changeobjectowner |
| sp_control_dbmasterkey_password | La chiave master è obbligatoria e la password deve essere corretta.|sp_control_dbmasterkey_password |
| sp_defaultdb<br /><br /> sp_defaultlanguage | ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage |
| sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin |
| USER_ID|DATABASE_PRINCIPAL_ID | USER_ID |
| sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|Le informazioni restituite da queste stored procedure risultano corrette in [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]. L'output non riflette le modifiche apportate alla gerarchia di autorizzazioni implementata in SQL Server 2008. Per ulteriori informazioni, vedere [Autorizzazioni dei ruoli predefiniti del server](https://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx). | sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission |
| GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|Autorizzazioni specifiche di GRANT, DENY e REVOKE.|Autorizzazione ALL |
| Funzione intrinseca PERMISSIONS | Eseguire una query su sys.fn_my_permissions. | PERMISSIONS |
| SETUSER | EXECUTE AS | SETUSER |
| Algoritmi di crittografia RC4 e DESX|Utilizzare un altro algoritmo, ad esempio AES. | Algoritmo DESX |

### <a name="server-configuration-options"></a>Opzioni di configurazione del server

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Opzione c2 audit, opzione default trace enabled<br /><br /> default trace enabled - opzione | [Opzione di configurazione del server common criteria compliance enabled](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [Eventi estesi](../relational-databases/extended-events/extended-events.md) | sp_configure 'c2 audit mode'<br /><br />sp_configure 'default trace enabled' |

### <a name="smo-classes"></a>Classi SMO

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Classe *Microsoft.SQLServer. Management.Smo.Information*<br /><br />Classe *Microsoft.SQLServer. Management.Smo.Settings*<br /><br />Classe *Microsoft.SQLServer.Management. Smo.DatabaseOptions*<br /><br />Proprietà *Microsoft.SqlServer.Management.Smo. DatabaseDdlTrigger.NotForReplication* | Classe *Microsoft.SqlServer.  Management.Smo.Server*<br /><br />Classe **Microsoft.SqlServer.  Management.Smo.Server* Classe<br /><br />* Microsoft.SqlServer. Management.Smo.Database*<br /><br />nessuno | nessuno |

### <a name="sql-server-agent"></a>SQL Server Agent

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Notifica **net send**<br /><br />Notifica tramite cercapersone | Notifica di posta elettronica<br /><br />Notifica di posta elettronica | nessuno |

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Integrazione di Esplora soluzioni in SQL Server Management Studio | | nessuno |

### <a name="system-stored-procedures-and-functions"></a>Stored procedure e funzioni di sistema

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_db_increased_partitions | No. Il supporto per l'estensione del numero di partizioni è disponibile per impostazione predefinita in SQL Server 2019 (15.x). | sp_db_increased_partitions |
| fn_virtualservernodes<br /><br />fn_servershareddrives | sys.dm_os_cluster_nodes<br /><br />sys.dm_io_cluster_shared_drives | fn_virtualservernodes<br /><br /> fn_servershareddrives |
| fn_get_sql | sys.dm_exec_sql_text | fn_get_sql |
| sp_lock | sys.dm_tran_locks | sp_lock |

### <a name="system-tables"></a>Tabelle di sistema

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers|Viste di compatibilità. Per altre informazioni, vedere [Viste di compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md).<br /><br />**Importante:** nelle viste di compatibilità non vengono esposti metadati per le funzionalità introdotte in SQL Server 2005 (9.x). È consigliabile aggiornare le applicazioni per l'utilizzo delle viste del catalogo. Per altre informazioni, vedere [Viste del catalogo &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/catalog-views-transact-sql.md). | sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers |
| sys.numbered_procedures<br /><br />sys.numbered_procedure_parameters | nessuno | numbered_procedures<br /><br />numbered_procedure_parameters |

### <a name="sql-trace-stored-procedures-functions-and-catalog-views"></a>Stored procedure, funzioni e viste del catalogo della traccia SQL

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values|[Eventi estesi](../relational-databases/extended-events/extended-events.md) | sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values |

### <a name="system-views"></a>Viste di sistema

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies |

### <a name="table-compression"></a>Compressione di tabelle

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Utilizzo del formato di archiviazione vardecimal. | Il formato di archiviazione vardecimal è deprecato. Con la compressione dei dati di SQL Server 2019 (15.x) vengono compressi sia i valori decimali che altri tipi di dati. È consigliabile utilizzare la compressione dei dati anziché il formato di archiviazione vardecimal. | Formato di archiviazione vardecimal |
| Utilizzo della procedura sp_db_vardecimal_storage_format.|Il formato di archiviazione vardecimal è deprecato. Con la compressione dei dati di SQL Server 2019 (15.x) vengono compressi sia i valori decimali che altri tipi di dati. È consigliabile utilizzare la compressione dei dati anziché il formato di archiviazione vardecimal. | sp_db_vardecimal_storage_format |
| Utilizzo della procedura sp_estimated_rowsize_reduction_for_vardecimal.|Utilizzare la compressione dei dati e la procedura sp_estimate_data_compression_savings. |sp_estimated_rowsize_reduction_for_vardecimal |

### <a name="text-pointers"></a>Puntatori di testo

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| WRITETEXT<br /><br />UPDATETEXT<br /><br />READTEXT|nessuno|UPDATETEXT o WRITETEXT<br /><br />READTEXT |
| TEXTPTR()<br /><br />TEXTVALID() | nessuno | TEXTPTR<br /><br />TEXTVALID |

### <a name="transact-sql"></a>Transact-SQL

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Sequenza di chiamata di funzioni :: | Sostituita da SELECT *column_list* FROM sys.\<*function_name*>().<br /><br />Sostituire, ad esempio, `SELECT * FROM ::fn_virtualfilestats(2,1)`con `SELECT * FROM sys.fn_virtualfilestats(2,1)`. | Sintassi per la chiamata di funzioni '::' |
| Riferimenti a colonne in 3 e 4 parti. | Il funzionamento conforme allo standard prevede nomi in 2 parti.|Nome di colonna in più di due parti |
| Stringa racchiusa tra virgolette utilizzata come alias di colonna per un'espressione in un elenco SELECT:<br /><br />'*string_alias*' = *expression* | *expression* [AS] *column_alias*<br /><br />*expression* [AS] [*column_alias*]<br /><br />*expression* [AS] "*column_alias*"<br /><br />*expression* [AS] '*column_alias*'<br /><br />*column_alias* = *expression* | Valori letterali stringa come alias di colonna |
| Procedure numerate | No. Non usare. | ProcNums |
| Sintassi *table_name.index_name* nell'istruzione DROP INDEX|Sintassi *index_name* ON *table_name* nell'istruzione DROP INDEX.|DROP INDEX con nome in due parti |
| Istruzioni Transact-SQL che non terminano con un punto e virgola.|Terminare le istruzioni Transact-SQL con un punto e virgola (;). | nessuno |
| GROUP BY ALL|Utilizzare una soluzione personalizzata caso per caso con UNION o una tabella derivata. | GROUP BY ALL |
| ROWGUIDCOL come nome di colonna nelle istruzioni DML.|Utilizzare $rowguid.|ROWGUIDCOL |
| IDENTITYCOL come nome di colonna nelle istruzioni DML.|Utilizzare $identity.|IDENTITYCOL |
| Utilizzo di # e ## come nomi di tabelle e di stored procedure temporanee. | Usare almeno un carattere aggiuntivo.|'#' e '##' come nomi di tabelle e stored procedure temporanee
| Uso di \@, \@\@ o \@\@ come identificatori Transact-SQL. | Non usare come identificatori \@ o \@\@ o nomi che iniziano con \@\@. | '\@' e nomi che iniziano con '\@\@' come identificatori Transact-SQL |
| Utilizzo della parola chiave DEFAULT come valore predefinito.|Non utilizzare la parola DEFAULT come valore predefinito. | Parola chiave DEFAULT come valore predefinito |
| Utilizzo di uno spazio come separatore tra gli hint di tabella.|Per separare gli hint di tabella, utilizzare la virgola. | Più hint di tabella senza virgola |
| L'elenco di selezione di una vista indicizzata aggregata deve contenere COUNT_BIG (\*) in modalità compatibilità 90 | Usare COUNT_BIG (\*). | Elenco di selezioni di una vista indicizzata senza COUNT_BIG(\*) |
| Applicazione indiretta di hint di tabella a una chiamata di una funzione con valori di tabella composta da più istruzioni tramite una vista.|No.|Hint di funzione con valori di tabella indiretti |
| Sintassi di ALTER DATABASE:<br /><br />MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE | MODIFY FILEGROUP READ_ONLY<br /><br />MODIFY FILEGROUP READ_WRITE | MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE |
| SET ANSI_NULLS OFF e opzione di database ANSI_NULLS OFF<br /><br />SET ANSI_PADDING OFF e opzione di database ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF e opzione di database CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS | No. <br /><br /> ANSI_NULLS, ANSI_PADDING e CONCAT_NULLS_YIELDS_NULL sono sempre impostate su ON. SET OFFSETS non è disponibile. | SET ANSI_NULLS OFF <br /><br /> SET ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS<br /><br />ALTER DATABASE SET ANSI_NULLS OFF<br /><br />ALTER DATABASE SET ANSI_PADDING OFF <br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF |
| SET FMTONLY | [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md), [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md), [sp_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) e [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md). | SET FMTONLY |
| Specifica di NOLOCK o READUNCOMMITTED nella clausola FROM di un'istruzione UPDATE o DELETE. | Rimuovere l'hint di tabella NOLOCK o READUNCOMMITTED dalla clausola FROM. | NOLOCK o READUNCOMMITTED in UPDATE o DELETE |
| Specifica di hint di tabella senza utilizzare la parola chiave WITH. | Utilizzare WITH. | Hint di tabella senza WITH |
| INSERT_HINTS | | INSERT_HINTS |

### <a name="tools"></a>Strumenti

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| SQL Server Profiler per l'acquisizione della traccia | Utilizzare il profile degli eventi estesi incorporato in SQL Server Management Studio.|SQL Server Profiler |
| SQL Server Profiler per la riproduzione della traccia | [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md) |

### <a name="trace-management-objects"></a>Trace Management Objects

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Spazio dei nomi Microsoft.SqlServer.Management.Trace (contiene le API per gli oggetti Trace and Replay di SQL Server)|Configurazione della traccia: <xref:Microsoft.SqlServer.Management.XEvent><br /><br />Lettura della traccia: <xref:Microsoft.SqlServer.XEvent.Linq><br /><br />Riproduzione della traccia: nessuno | |

### <a name="xml"></a>XML

| Funzionalità deprecata | Sostituzione | Nome funzionalità |
|--------------------|-------------|--------------|
| Generazione schema XDR inline | La direttiva XMLDATA all'opzione FOR XML è deprecata. Utilizzare la generazione XSD in caso di modalità RAW e AUTO. Non sono disponibili sostituzioni per la direttiva XMLDATA in modalità EXPLICIT. | XMLDATA |

> [!NOTE]
> Il parametro **OUTPUT** del cookie per **sp_setapprole** è attualmente documentato come **varbinary(8000)** che rappresenta la lunghezza massima corretta. Tuttavia, l'implementazione corrente restituisce **varbinary(50)** . Se gli sviluppatori hanno allocato **varbinary(50)** , potrebbe essere necessario apportare modifiche all'applicazione qualora le dimensioni restituite dal cookie aumentino in una versione successiva. Sebbene non si tratti di un problema relativo a elementi deprecati, questo aspetto viene riportato in quanto le modifiche all'applicazione sono simili. Per altre informazioni, vedere [sp_setapprole &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Funzionalità del motore di database non più usate in SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)  
