---
description: sp_data_source_objects (Transact-SQL)
title: sp_data_source_objects | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_objects
helpviewer_keywords:
- PolyBase
ms.assetid: 48066431-fed2-4a8a-85af-ac704689e183
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f3aa43750cebfbbac77e3807845f1ed154af1ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067216"
---
# <a name="sp_data_source_objects-transact-sql"></a>sp_data_source_objects (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Restituisce l'elenco degli oggetti tabella disponibili per la virtualizzazione.

> [!NOTE]
> Questa procedura è stata introdotta in [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintassi  

```syntaxsql
sp_data_source_objects  
         [ @data_source = ] 'data_source'
     [ , [ @object_root_name = ] 'object_root_name' ]
     [ , [ @max_search_depth = ] max_search_depth ]
     [ , [ @search_options = ] 'search_options' ]
[ ; ]
```  
  
## <a name="arguments"></a>Argomenti  

`[ @data_source = ] 'data_source'`   
Nome dell'origine dati esterna da cui ottenere i metadati. `@data_source` è `sysname`.  

`[ @object_root_name = ] 'object_root_name'`   
Questo parametro è la radice del nome degli oggetti da cercare. `@object_root_name` è `nvarchar(max)` di e il valore predefinito è `NULL` .

Questa chiamata restituisce solo gli oggetti esterni che iniziano con il valore impostato per `@object_root_name` .

Se un'origine dati ODBC si connette a un sistema di gestione di database relazionali (RDBMS) che utilizza nomi in tre parti, `@object_root_name` non può contenere un nome di database parziale. In questi casi, il parametro `@object_root_name` deve contenere tutte e tre le parti, in cui la terza parte è il nome dell'oggetto in cui eseguire la ricerca.
> [!CAUTION]
> A causa delle differenze tra le piattaforme dati esterne, alcune piattaforme non restituiscono alcun risultato se viene specificato il valore predefinito di `NULL` . Alcuni considerano `NULL` come la mancanza di un filtro. Oracle RDMBS, ad esempio, non restituirà risultati se `NULL` viene fornito per `@object_root_name` .

`[ @max_search_depth = ] max_search_depth`   
Questo valore specifica la profondità massima (in parti) oltre la `@object_root_name` quale si vuole eseguire la ricerca. `@max_search_depth` è `int` di valore e il valore predefinito è 1.

Ad esempio, un valore `@max_search_depth` di 1, con un `@object_root_name` che corrisponde al nome di un database di SQL Server, restituirà schemi contenuto nel database.

Un oggetto `@max_search_depth` di `NULL` restituirà informazioni su `@object_root_name` se esiste e non è vuoto, nel caso di Catalog o schema.

`[ @search_options = ] 'search_options'`   
Il `search_options` parametro è di tipo nvarchar (max) e il valore predefinito è `NULL` .

Questo parametro non viene usato ma può essere implementato in futuro.

## <a name="result-sets"></a>Set di risultati

| Nome colonna | Tipo di dati | Descrizione |
|--|--|--|
| Object_Type | nvarchar(200) | Tipo dell'oggetto, ad esempio tabella o DATABASE. |
| OBJECT_NAME | nvarchar(max) | Nome completo dell'oggetto. Preceduto da un carattere di escape con virgolette specifiche del back-end. |
| OBJECT_LEAF_NAME | nvarchar(max) | Nome dell'oggetto non qualificato. |
| TABLE_LOCATION | nvarchar(max) | Stringa di percorso della tabella valida che può essere utilizzata per un'istruzione CREATE EXTERNAL TABLE. Sarà `NULL` se non è applicabile. |
  
## <a name="permissions"></a>Autorizzazioni

È necessaria l'autorizzazione ALTER ANY EXTERNAL DATA SOURCE.  

## <a name="remarks"></a>Commenti  

Per l'istanza di SQL Server deve essere installata la funzionalità di  [base](../../relational-databases/polybase/polybase-guide.md) .

Questo stored procedure supporta i connettori per:

- SQL Server
- Oracle
- Teradata
- MongoDB
- Cosmos DB

Il stored procedure non supporta i connettori di origine ODBC DTA generici.

Il concetto di confronto tra vuoto e non vuoto è correlato al comportamento del driver ODBC e della [ `SQLTables` funzione](../native-client-odbc-api/sqltables.md). Non Empty indica che un oggetto contiene tabelle, non righe. Uno schema vuoto, ad esempio, non contiene tabelle in SQL Server. Un database vuoto contiene senza tabelle all'interno di Teradata.

I tipi di oggetto sono determinati dal driver ODBC dell'origine dati esterna. Ogni origine dati esterna determina la qualifica di tabella. Questo può includere oggetti di database come funzioni in TeraData o sinonimi in Oracle. La funzione polibase non è in grado di connettersi ad alcuni oggetti ODBC come tabelle esterne e pertanto non ha un valore nella colonna TABLE_LOCATION. Nonostante ciò, la presenza di uno di questi oggetti può rendere un database o uno schema non vuoto.

Usare `sp_data_source_objects` e [`sp_data_source_table_columns`](sp-data-source-table-columns.md) per individuare gli oggetti esterni. Queste stored procedure di sistema restituiscono lo schema delle tabelle disponibili per la virtualizzazione. Azure Data Studio utilizza queste due stored procedure per supportare la [virtualizzazione dei dati](../../azure-data-studio/extensions/data-virtualization-extension.md). Utilizzare [sp_data_source_table_columns](sp-data-source-table-columns.md) per individuare gli schemi della tabella esterna rappresentati in SQL Server tipi di dati.

### <a name="data-source-type-specific-remarks"></a>Osservazioni specifiche del tipo di origine dati

* Teradata

   Le visualizzazioni di sistema Teradata non utilizzano la sicurezza a livello di riga, quindi gli utenti possono visualizzare l'esistenza di tabelle che non possono eseguire query.

* MongoDB

   Alcune versioni precedenti di MongoDB limitano la possibilità di elencare tutti i database per gli utenti di tipo amministratore. Gli utenti che non dispongono di questa autorizzazione possono ricevere errori di autenticazione che tentano di eseguire questa procedura con un valore null `@object_root_name` .

## <a name="examples"></a>Esempio  

### <a name="sql-server"></a>SQL Server

Nell'esempio seguente vengono restituiti tutti i database, schemi e le tabelle e le viste

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 3;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| SCHEMA | "database". dbo | dbo | NULL |
| TABLE | "database". dbo "." cliente | cliente | [database]. [dbo]. cliente |
| TABLE | "database". dbo "." elemento | item | [database]. [dbo]. elemento |
| TABLE | "database". dbo "." nazione | nazione | [database]. [dbo]. nazione |

Nell'esempio seguente vengono restituiti tutti i database

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | UserDatabase | UserDatabase | NULL |
| DATABASE | "master" | master | NULL |
| DATABASE | msdb | msdb | NULL |
| DATABASE | tempdb | tempdb | NULL |
| DATABASE | "database" | database | NULL |

Nell'esempio seguente vengono restituiti tutti i schemi in un database

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| SCHEMA | "database". dbo | dbo | NULL |
| SCHEMA | "database". INFORMATION_SCHEMA " | INFORMATION_SCHEMA | NULL |
| SCHEMA | "database". sys | sys | NULL |

Nell'esempio seguente vengono restituite tutte le tabelle nello schema 

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database].[dbo]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| TABLE | "database". dbo "." cliente | cliente | [database]. [dbo]. cliente |
| TABLE | "database". dbo "." elemento | item | [database]. [dbo]. elemento |
| TABLE | "database". dbo "." nazione | nazione | [database]. [dbo]. nazione |
| TABLE | "database". dbo "." ordini | orders | [database]. [dbo]. ordini |
| TABLE | "database". dbo "." parte | parte | [database]. [dbo]. parte |

### <a name="oracle"></a>Oracle

Nell'esempio seguente vengono restituiti i schemi completi e le tabelle, le funzioni, le visualizzazioni e così via.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[OracleObjectRoot]'; 
DECLARE @max_search_depth INT = 2; 
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| VIEW | "SYS". ALL_SQLSET_STATEMENTS " | ALL_SQLSET_STATEMENTS | [ORACLEOBJECTROOT]. [SYS]. [ALL_SQLSET_STATEMENTS] |
| SYSTEM TABLE | "SYS". BOOTSTRAP $ " | BOOTSTRAP $ | [ORACLEOBJECTROOT]. [SYS]. [BOOTSTRAP $] |
| SYNONYM | "PUBLIC". ALL_ALL_TABLES " | ALL_ALL_TABLES | NULL |
| SCHEMA | "database" | database | NULL |
| TABLE | "database". cliente | cliente | [ORACLEOBJECTROOT]. [database]. cliente |

### <a name="teradata"></a>Teradata

Nell'esempio seguente vengono restituiti tutti i database e le tabelle, le funzioni, le visualizzazioni e così via.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |  
|--|--|--|--|
| FUNCTION | "SYSLIB". " ExtractRoles" | ExtractRoles | NULL |  
| SYSTEM TABLE | "DBC". " UDTCast" | UDTCast | [DBC]. [UDTCast] |  
| TYPE | "SYSUDTLIB"." XML | XML | NULL |  
| DATABASE | "database" | database | NULL |
| TABLE | "database". cliente | cliente | [database]. cliente |  

### <a name="mongo-db"></a>MongoDB

Nell'esempio seguente vengono restituiti tutti i database e le tabelle.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| TABLE | "database". cliente | cliente | [database]. cliente |
| TABLE | "database". elemento | item | [database]. elemento |
| TABLE | "database". nazione | nazione | [database]. nazione |
| TABLE | "database". ordini | orders | [database]. ordini |

## <a name="see-also"></a>Vedi anche

- [sp_data_source_columns](./sp-data-source-table-columns.md)   
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
- [Estensione di virtualizzazione dei dati per Azure Data Studio](../../azure-data-studio/extensions/data-virtualization-extension.md)   
- [Introduzione a PolyBase](../polybase/polybase-guide.md)   
- [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)