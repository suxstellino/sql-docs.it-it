---
title: sp_data_source_table_columns
description: sp_data_source_table_columns (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_table_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
author: MikeRayMSFT
ms.author: mikeray
ms.custom: ''
ms.date: 11/10/2020
ms.openlocfilehash: 31b0dedb8544cc6752eac8a9934aa92d74d424ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081872"
---
# <a name="sp_data_source_table_columns-transact-sql"></a>sp_data_source_table_columns (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Restituisce un elenco di colonne nella tabella di origine dati esterna.
  
> [!NOTE]
> Questa procedura è stata introdotta in [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sqlsyntax
sp_data_source_table_columns
         [ @data_source = ] 'data_source'
       , [ @table_location = ] 'table_location'
[ ; ]
```  

## <a name="arguments"></a>Argomenti

`[ @data_source = ] 'data_source'`   
Nome dell'origine dati esterna da cui ottenere i metadati. Il tipo è `sysname` .

`[ @table_location = ] 'table_location'`   
Stringa del percorso della tabella che identifica la tabella. `table_location` il tipo è `nvarchar(max)` .

## <a name="returns"></a>Restituisce

Il stored procedure restituisce le informazioni seguenti:

|Nome colonna |Tipo di dati |Descrizione|
|---|---|---|
|NAME|nvarchar(max)|Nome della colonna.
|TYPE|nvarchar(200)|Nome del tipo di SQL Server
|LENGTH|INT|Lunghezza della colonna
|PRECISION|INT|Precisione della colonna
|SCALE|INT|Scala della colonna
|COLLATION|nvarchar(200)|SQL Server regole di confronto della colonna
|IS_NULLABLE|bit|Questa colonna ammette valori null.
|SOURCE_TYPE_NAME|nvarchar(max)|Nome del tipo specifico del back-end. Utilizzato principalmente per il debug. Per le origini ODBC questo corrisponderà alla colonna del risultato TYPE_NAME per SQLColumns ().
|REMARKS|nvarchar(max)|Commenti generali o descrizione della colonna. Attualmente sempre NULL.|

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

Il concetto di confronto tra vuoto e non vuoto è correlato al comportamento del driver ODBC e della [ `SQLTables` funzione](../native-client-odbc-api/sqltables.md). Non Empty indica che un oggetto contiene tabelle, non righe. Uno schema vuoto, ad esempio, non contiene tabelle in SQL Server. Un database vuoto contiene senza tabelle all'interno di Teradata. i risultati sono una rappresentazione SQL Server dello schema back-end come interpretato dal connettore di base per il back-end. Questa distinzione consiste nel fatto che anziché semplicemente passare i risultati della chiamata ODBC al back-end, i risultati sono basati sul risultato del codice di mapping del tipo di base.

Usare [`sp_data_source_objects`](sp-data-source-objects.md) e `sp_data_source_table_columns` per individuare gli oggetti esterni. Queste stored procedure di sistema restituiscono lo schema delle tabelle disponibili per la virtualizzazione. Azure Data Studio utilizza queste due stored procedure per supportare la [virtualizzazione dei dati](../../azure-data-studio/extensions/data-virtualization-extension.md). Utilizzare `sp_data_source_table_columns` per individuare gli schemi della tabella esterna rappresentati in SQL Server tipi di dati.

A causa delle differenze tra le regole di confronto nei dati di origine di Hadoop e le regole di confronto supportate in SQL Server 2019, le lunghezze dei tipi di dati consigliati per le colonne con tipo di dati varchar in tabelle esterne possono essere molto più grandi del previsto. Questo si verifica per motivi strutturali.

## <a name="example"></a>Esempio  

Nell'esempio seguente vengono restituite le colonne della tabella per una tabella esterna in un SQL Server denominato `server` , appartenenti a uno schema denominato `schema` .
  
```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @table_location NVARCHAR(400) = N'[database].[schema].[table]';
EXEC sp_data_source_table_columns @data_source, @table_location
```  
  
## <a name="see-also"></a>Vedi anche

- [Introduzione a PolyBase](../polybase/polybase-guide.md)
- [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)