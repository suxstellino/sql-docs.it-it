---
description: DROP TABLE (Transact-SQL)
title: DROP TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_TABLE_TSQL
- DROP TABLE
dev_langs:
- TSQL
helpviewer_keywords:
- removing indexes
- table removal [SQL Server]
- deleting indexes
- dropping data
- deleting tables
- dropping indexes
- removing constraints
- removing permissions
- DROP TABLE statement
- removing tables
- deleting triggers
- removing data
- dropping tables
- deleting permissions
- dropping triggers
- deleting constraints
- removing triggers
- deleting data
- dropping constraints
- dropping permissions
ms.assetid: 0b6f2b6f-3aa3-4767-943f-43df3c3c5cfd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 01343ee8d31f086781f09be35b81155f5b9090eb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236992"
---
# <a name="drop-table-transact-sql"></a>DROP TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Rimuove una o più definizioni di tabella e tutti i dati, gli indici, i trigger, i vincoli e le specifiche di autorizzazione associate alle tabelle in questione. Le viste e stored procedure che fanno riferimento alla tabella eliminata devono essere eliminate in modo esplicito tramite l'istruzione [DROP VIEW](../../t-sql/statements/drop-view-transact-sql.md) o [DROP PROCEDURE](../../t-sql/statements/drop-procedure-transact-sql.md). Per indicare le dipendenze da una tabella, usare [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP TABLE [ IF EXISTS ] { database_name.schema_name.table_name | schema_name.table_name | table_name } [ ,...n ]  
[ ; ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *database_name*  
 Nome del database in cui è stata creata la tabella.  
  
 Il database SQL di Azure supporta il formato del nome in tre parti, nome_database.[nome_schema].nome_oggetto, quando nome_database è il database corrente oppure nome_database è tempdb e nome_oggetto inizia con #. Il database SQL di Azure non supporta i nomi composti da quattro parti.  
  
 *IF EXISTS*  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Rimuove in modo condizionale la tabella solo se esiste già.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la tabella.  
  
 *table_name*  
 Nome della tabella da rimuovere.  
  
## <a name="remarks"></a>Commenti  
 Non è possibile utilizzare l'istruzione DROP TABLE per eliminare una tabella a cui fa riferimento un vincolo FOREIGN KEY. È prima necessario eliminare il vincolo FOREIGN KEY o la tabella di riferimento. Se con la stessa istruzione DROP TABLE si eliminano sia la tabella di riferimento che la tabella che contiene la chiave primaria, è necessario indicare la tabella di riferimento per prima nell'elenco.  
  
 Si possono eliminare più tabelle in qualsiasi database. Se una tabella da eliminare fa riferimento alla chiave primaria di un'altra tabella, anch'essa da eliminare, è necessario indicare la tabella di riferimento con la chiave esterna prima della tabella contenente la chiave primaria a cui fa riferimento la prima tabella.  
  
 Con l'eliminazione di una tabella, le regole o i valori predefiniti della tabella vengono disassociati e i vincoli o trigger associati alla tabella vengono eliminati automaticamente. Se la tabella viene ricreata, è necessario associare nuovamente le regole e i valori predefiniti appropriati, ricreare eventuali trigger e aggiungere tutti i vincoli necessari.  
  
 Se si eliminano tutte le righe di una tabella con DELETE *tablename* oppure si esegue l'istruzione TRUNCATE TABLE, la tabella continua a esistere fino a quando non viene eliminata.  
  
 Le tabelle e gli indici di grandi dimensioni che utilizzano più di 128 extent vengono eliminati in due fasi distinte: logica e fisica. Nella fase logica, le unità di allocazione esistenti utilizzate dalla tabella vengono contrassegnate per la deallocazione e bloccate fino al completamento del commit della transazione. Nella fase fisica, le pagine IAM contrassegnate per la deallocazione vengono eliminate fisicamente in più batch.  
  
 Se si elimina una tabella che contiene una colonna VARBINARY (MAX) con l'attributo FILESTREAM, non verrà rimosso alcun dato archiviato nel file system.  
  
> [!IMPORTANT]  
>  DROP TABLE e CREATE TABLE non devono essere eseguiti nella stessa tabella nello stesso batch. In caso contrario, è possibile che si verifichi un errore imprevisto.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER per lo schema a cui appartiene la tabella, l'autorizzazione CONTROL per la tabella o l'appartenenza al ruolo predefinito del database **db_ddladmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-a-table-in-the-current-database"></a>R. Eliminazione di una tabella dal database corrente  
 Nell'esempio seguente vengono eliminati la tabella `ProductVendor1` e i dati e gli indici corrispondenti dal database corrente.  
  
```sql  
DROP TABLE ProductVendor1 ;  
```  
  
### <a name="b-dropping-a-table-in-another-database"></a>B. Eliminazione di una tabella da un database diverso da quello corrente  
 Nell'esempio seguente viene eliminata la tabella `SalesPerson2` nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. È possibile eseguire questo esempio da qualsiasi database nell'istanza del server.  
  
```sql  
DROP TABLE AdventureWorks2012.dbo.SalesPerson2 ;  
```  
  
### <a name="c-dropping-a-temporary-table"></a>C. Eliminazione di una tabella temporanea  
 Nell'esempio seguente viene creata una tabella temporanea, ne viene controllata l'esistenza, quindi la tabella viene eliminata e viene eseguito un ulteriore controllo dell'esistenza. Questo esempio non usa la sintassi **IF EXISTS**, disponibile a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
```sql  
CREATE TABLE #temptable (col1 INT);  
GO  
INSERT INTO #temptable  
VALUES (10);  
GO  
SELECT * FROM #temptable;  
GO  
IF OBJECT_ID(N'tempdb..#temptable', N'U') IS NOT NULL   
DROP TABLE #temptable;  
GO  
--Test the drop.  
SELECT * FROM #temptable;  
```  
  
### <a name="d-dropping-a-table-using-if-exists"></a>D. Eliminazione di una tabella usando IF EXISTS  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Nell'esempio seguente viene creato un pool di risorse denominato T1. Quindi la seconda istruzione elimina la tabella. La terza istruzione non esegue alcuna azione perché la tabella è già stata eliminata, ma non produce un errore.  
  
```sql  
CREATE TABLE T1 (Col1 INT);  
GO  
DROP TABLE IF EXISTS T1;  
GO  
DROP TABLE IF EXISTS T1;  
```  
  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)