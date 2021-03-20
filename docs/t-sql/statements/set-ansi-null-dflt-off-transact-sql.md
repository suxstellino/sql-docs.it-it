---
description: SET ANSI_NULL_DFLT_OFF (Transact-SQL)
title: SET ANSI_NULL_DFLT_OFF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ANSI_NULL_DFLT_OFF_TSQL
- ANSI_NULL_DFLT_OFF
- SET ANSI_NULL_DFLT_OFF
- SET_ANSI_NULL_DFLT_OFF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- default nullability
- ANSI_NULL_DFLT_OFF option
- null values [SQL Server], overriding
- overriding default nullability
- SET ANSI_NULL_DFLT_OFF statement
ms.assetid: 8ed5c512-f5de-4741-a18a-de85a3041295
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1a9d7351c0d69e714ca50453e8f38eb578ed30d5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755951"
---
# <a name="set-ansi_null_dflt_off-transact-sql"></a>SET ANSI_NULL_DFLT_OFF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica il comportamento della sessione in modo da eseguire l'override dell'impostazione predefinita relativa al supporto di valori Null delle nuove colonne quando l'opzione di database ANSI null default è **true**. Per altre informazioni sull'impostazione del valore per ANSI null default, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET ANSI_NULL_DFLT_OFF { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_OFF OFF
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 Questa impostazione ha effetto solo sul supporto di valori Null per le nuove colonne quando tale supporto non è specificato nelle istruzioni CREATE TABLE e ALTER TABLE. Quando l'opzione SET ANSI_NULL_DFLT_OFF è impostata su ON, per impostazione predefinita le nuove colonne create tramite le istruzioni ALTER TABLE e CREATE TABLE sono NOT NULL se il supporto di valori Null per la colonna non è stato specificato in modo esplicito. SET ANSI_NULL_DFLT_OFF non ha effetto sulle colonne create tramite un NULL o un NOT NULL esplicito.  
  
 Non è possibile impostare contemporaneamente su ON entrambe le opzioni SET ANSI_NULL_DFLT_OFF e SET ANSI_NULL_DFLT_ON. Se un'opzione è impostata su ON, l'altra deve essere impostata su OFF. In altri termini, è possibile impostare su ON una delle due opzioni oppure impostare entrambe le opzioni su OFF. Se si imposta su ON una delle due opzioni SET ANSI_NULL_DFLT_OFF e SET ANSI_NULL_DFLT_ON, tale opzione risulta attivata. Se entrambe le opzioni sono impostate su OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa il valore della colonna is_ansi_null_default_on nella vista di catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Per ottenere un'affidabilità maggiore dell'operazione degli script [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizzabili in database con impostazioni per il supporto di valori Null diverse, nelle istruzioni CREATE TABLE e ALTER TABLE è consigliabile specificare sempre la parola chiave NULL o NOT NULL.  
  
 L'opzione SET ANSI_NULL_DFLT_OFF viene impostata in fase di esecuzione, non in fase di analisi.  
  
 Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito.  
  
```sql  
DECLARE @ANSI_NULL_DFLT_OFF VARCHAR(3) = 'OFF';  
IF ( (2048 & @@OPTIONS) = 2048 ) SET @ANSI_NULL_DFLT_OFF = 'ON';  
SELECT @ANSI_NULL_DFLT_OFF AS ANSI_NULL_DFLT_OFF;  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono illustrati gli effetti di `SET ANSI_NULL_DFLT_OFF` con entrambe le impostazioni per l'opzione di database relativa all'impostazione predefinita per il supporto di valori Null ANSI.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- Set the 'ANSI null default' database option to true by executing   
-- ALTER DATABASE.  
GO  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT);  
GO  
-- NULL INSERT should succeed.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to ON and create table t2.  
SET ANSI_NULL_DFLT_OFF ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL INSERT should fail.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to OFF and create table t3.  
SET ANSI_NULL_DFLT_OFF OFF;  
GO  
CREATE TABLE t3 (a TINYINT) ;  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- This illustrates the effect of having both the database  
-- option and SET option disabled.  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to ON and create table t5.  
SET ANSI_NULL_DFLT_OFF ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL insert should fail.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to OFF and create table t6.  
SET ANSI_NULL_DFLT_OFF OFF;  
GO  
CREATE TABLE t6 (a TINYINT);   
GO   
-- NULL insert should fail.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1, t2, t3, t4, t5, t6;  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)  
  
  
