---
description: SET ANSI_NULL_DFLT_ON (Transact-SQL)
title: SET ANSI_NULL_DFLT_ON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ANSI_NULL_DFLT_ON
- ANSI_NULL_DFLT_ON_TSQL
- SET ANSI_NULL_DFLT_ON
- SET_ANSI_NULL_DFLT_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULL_DFLT_ON statement
- ANSI_NULL_DFLT_ON option
- default nullability
- null values [SQL Server], overriding
- overriding default nullability
ms.assetid: 8c925924-a466-4c8b-aeb2-7e0d341f32db
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39854417675cc4a7ee767c27918ae16c070c9ef5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100774"
---
# <a name="set-ansi_null_dflt_on-transact-sql"></a>SET ANSI_NULL_DFLT_ON (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica il comportamento della sessione in modo da eseguire l'override dell'impostazione predefinita relativa al supporto di valori Null delle nuove colonne, quando l'opzione di database **ANSI null default** è **false**. Per altre informazioni sull'impostazione del valore per **ANSI null default**, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database

SET ANSI_NULL_DFLT_ON {ON | OFF}
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_ON ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 Questa impostazione ha effetto solo sul supporto di valori Null per le nuove colonne quando tale supporto non è specificato nelle istruzioni CREATE TABLE e ALTER TABLE. Quando l'opzione SET ANSI_NULL_DFLT_ON è impostata su ON, le nuove colonne create con le istruzioni ALTER TABLE e CREATE TABLE supportano i valori Null se il supporto di valori Null per la colonna non è stato specificato in modo esplicito. SET ANSI_NULL_DFLT_OFF non ha effetto sulle colonne create tramite un NULL o un NOT NULL esplicito.  
  
 Non è possibile impostare contemporaneamente su ON entrambe le opzioni SET ANSI_NULL_DFLT_OFF e SET ANSI_NULL_DFLT_ON. Se un'opzione è impostata su ON, l'altra deve essere impostata su OFF. In altri termini, è possibile impostare su ON una delle due opzioni oppure impostare entrambe le opzioni su OFF. Se si imposta su ON una delle due opzioni SET ANSI_NULL_DFLT_OFF e SET ANSI_NULL_DFLT_ON, tale opzione risulta attivata. Se entrambe le opzioni sono impostate su OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa il valore della colonna **is_ansi_null_default_on** nella vista di catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Per ottenere un'affidabilità maggiore dell'operazione degli script [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizzabili in database con impostazione per il supporto di valori Null diversa, nelle istruzioni CREATE TABLE e ALTER TABLE è consigliabile specificare sempre la parola chiave NULL o NOT NULL.  
  
 Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client e il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostano automaticamente l'opzione ANSI_NULL_DFLT_ON su ON al momento della connessione. L'opzione predefinita per SET ANSI_NULL_DFLT_ON è OFF per le connessioni di applicazioni DB-Library.  
  
 Quando l'opzione SET ANSI_DEFAULTS è impostata su ON, l'opzione SET ANSI_NULL_DFLT_ON risulta abilitata.  
  
 L'opzione SET ANSI_NULL_DFLT_ON viene impostata in fase di esecuzione, non in fase di analisi.  
  
 L'impostazione di SET ANSI_NULL_DFLT_ON non viene applicata quando le tabelle vengono create utilizzando l'istruzione SELECT INTO.  
  
 Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito.  
  
```sql  
DECLARE @ANSI_NULL_DFLT_ON VARCHAR(3) = 'OFF';  
IF ( (1024 & @@OPTIONS) = 1024 ) SET @ANSI_NULL_DFLT_ON = 'ON';  
SELECT @ANSI_NULL_DFLT_ON AS ANSI_NULL_DFLT_ON;  
  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 In questo esempio vengono illustrati gli effetti dell'opzione `SET ANSI_NULL_DFLT_ON` con entrambe le impostazioni dell'opzione di database **ANSI null default**.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON  
-- has an effect when the 'ANSI null default' for the database is false.  
-- Set the 'ANSI null default' database option to false by executing  
-- ALTER DATABASE.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t2.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL insert should succeed.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t3.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t3 (a TINYINT);  
GO  
-- NULL insert should fail.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON   
-- has no effect when the 'ANSI null default' for the database is true.  
-- Set the 'ANSI null default' database option to true.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON  
GO  
  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t5.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t6.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t6 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1,t2,t3,t4,t5,t6;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [SET ANSI_NULL_DFLT_OFF &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)  
  
  
