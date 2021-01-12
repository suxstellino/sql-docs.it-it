---
description: SET QUOTED_IDENTIFIER (Transact-SQL)
title: SET QUOTED_IDENTIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- QUOTED_IDENTIFIER_TSQL
- SET_QUOTED_IDENTIFIER_TSQL
- SET QUOTED_IDENTIFIER
- QUOTED_IDENTIFIER
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- identifiers [SQL Server], delimited
- QUOTED_IDENTIFIER option
- quoted identifiers
- ISO delimited identifiers rules
- SET QUOTED_IDENTIFIER statement
ms.assetid: 10f66b71-9241-4a3a-9292-455ae7252565
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 232c287564593eee0deab4ae37dc13c36d2d689b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100759"
---
# <a name="set-quoted_identifier-transact-sql"></a>SET QUOTED_IDENTIFIER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Impone in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la conformità alle regole ISO relative all'utilizzo delle virgolette per delimitare identificatori e stringhe letterali. Gli identificatori delimitati da virgolette doppie possono essere parole chiave riservate [!INCLUDE[tsql](../../includes/tsql-md.md)] o possono includere caratteri normalmente non consentiti dalle regole della sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and serverless SQL pool in Azure Synapse Analytics

SET QUOTED_IDENTIFIER { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET QUOTED_IDENTIFIER ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
Quando `SET QUOTED_IDENTIFIER` è ON (impostazione predefinita), gli identificatori possono essere racchiusi tra virgolette doppie (" "), mentre i valori letterali devono essere racchiusi tra virgolette singole (' '). Tutte le stringhe delimitate da virgolette doppie vengono interpretate come identificatori di oggetto. Gli identificatori delimitati pertanto non devono necessariamente essere conformi alle regole per gli identificatori di [!INCLUDE[tsql](../../includes/tsql-md.md)]. Possono essere parole chiave riservate e includere caratteri normalmente non consentiti negli identificatori [!INCLUDE[tsql](../../includes/tsql-md.md)]. Non è possibile utilizzare le virgolette doppie per delimitare espressioni di stringhe letterali. Inoltre le stringhe letterali devono essere racchiuse tra virgolette singole. Se la stringa letterale contiene una virgoletta singola ('), questa può essere rappresentata da due virgolette singole (''). Quando si usano parole chiave riservate per nomi di oggetti nel database, è necessario che l'opzione `SET QUOTED_IDENTIFIER` sia impostata su ON.

Quando l'opzione `SET QUOTED_IDENTIFIER` è impostata su OFF, non è possibile racchiudere tra virgolette gli identificatori, i quali devono essere conformi a tutte le regole per gli identificatori di [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md). È possibile delimitare i valori letterali con virgolette singole o doppie. Se una stringa letterale è delimitata da virgolette doppie, la stringa può includere virgolette singole, ad esempio gli apostrofi.

> [!NOTE]
> QUOTED_IDENTIFIER non influisce sugli identificatori delimitati racchiusi tra parentesi quadre ([ ]).

È necessario che l'opzione `SET QUOTED_IDENTIFIER` sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate. Se l'opzione `SET QUOTED_IDENTIFIER` è impostata su OFF, le istruzioni CREATE, UPDATE, INSERT e DELETE eseguite su tabelle con indici in colonne calcolate o con viste indicizzate hanno esito negativo. Per altre informazioni sulle impostazioni dell'opzione SET necessarie per viste indicizzate e indici in colonne calcolate, vedere [Considerazioni sull'uso delle istruzioni SET](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).

`SET QUOTED_IDENTIFIER` deve essere ON quando si crea un indice filtrato.

`SET QUOTED_IDENTIFIER` deve essere ON quando si richiamano metodi con tipo di dati XML.

Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client e il provider OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostano automaticamente l'opzione QUOTED_IDENTIFIER su ON al momento della connessione. È possibile configurare questa impostazione nelle origini dei dati ODBC, negli attributi di connessione ODBC o nelle proprietà di connessione OLE DB. L'impostazione predefinita dell'opzione SET QUOTED_IDENTIFIER è OFF per le connessioni dalle applicazioni DB-Library.

Durante la creazione di una tabella l'opzione QUOTED IDENTIFIER viene sempre archiviata impostata su ON nei metadati della tabella se l'opzione è stata precedentemente impostata su OFF.

Quando viene creata una stored procedure, le impostazioni delle opzioni SET QUOTED_IDENTIFIER e SET ANSI_NULLS vengono acquisite e utilizzate per le successive chiamate della stored procedure.

Quando viene eseguita all'interno di una stored procedure, l'impostazione dell'opzione SET QUOTED_IDENTIFIER non viene modificata.

Quando `SET ANSI_DEFAULTS` è ON, anche QUOTED_IDENTIFIER è ON.

`SET QUOTED_IDENTIFIER` corrisponde anche all'impostazione QUOTED_IDENTIFIER di [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

`SET QUOTED_IDENTIFIER` diventa effettiva in fase di analisi di [!INCLUDE[tsql](../../includes/tsql-md.md)] e influisce solo sull'analisi, non sull'ottimizzazione o l'esecuzione delle query.

Per un batch ad hoc di livello superiore l'analisi viene avviata usando l'impostazione corrente della sessione per QUOTED_IDENTIFIER. Durante l'analisi del batch, le occorrenze di `SET QUOTED_IDENTIFIER` modificheranno il comportamento di analisi da quel punto in poi e questa impostazione verrà salvata per la sessione. Al termine dell'analisi e dell'esecuzione del batch, l'impostazione QUOTED_IDENTIFIER della sessione verrà pertanto impostata in base all'ultima occorrenza di `SET QUOTED_IDENTIFIER` nel batch.

Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] statiche in una stored procedure vengono analizzate usando l'impostazione QUOTED_IDENTIFIER in uso per il batch che ha creato o modificato la stored procedure. `SET QUOTED_IDENTIFIER` non produce alcun effetto quando si trova nel corpo di una stored procedure come istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] statica.

Per un batch annidato che usa `sp_executesql` o `exec()` l'analisi viene avviata tramite l'impostazione QUOTED_IDENTIFIER della sessione. Se il batch annidato si trova all'interno di una stored procedure, l'analisi viene avviata tramite l'impostazione QUOTED_IDENTIFIER della stored procedure. Durante l'analisi del batch annidato, le occorrenze di `SET QUOTED_IDENTIFIER` modificheranno il comportamento di analisi da quel punto in poi, ma l'impostazione QUOTED_IDENTIFIER della sessione non verrà aggiornata.

Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito:

```sql
DECLARE @QUOTED_IDENTIFIER VARCHAR(3) = 'OFF';
IF ( (256 & @@OPTIONS) = 256 ) 
SET @QUOTED_IDENTIFIER = 'ON';

SELECT @QUOTED_IDENTIFIER AS QUOTED_IDENTIFIER;
```

## <a name="permissions"></a>Autorizzazioni
Richiede l'appartenenza al ruolo `PUBLIC`.

## <a name="examples"></a>Esempi

### <a name="a-using-the-quoted-identifier-setting-and-reserved-word-object-names"></a>R. Utilizzo dell'impostazione per l'identificatore delimitato e di nomi di oggetto corrispondenti a parole chiave riservate

Nell'esempio seguente viene illustrato come, per poter creare e utilizzare oggetti con nomi che corrispondono a parole chiave riservate, sia necessario impostare l'opzione `SET QUOTED_IDENTIFIER` su `ON` e racchiudere tra virgolette doppie le parole chiave nei nomi di tabella.

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- Create statement fails.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Create statement succeeds.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SELECT "identity","order"
FROM "select"
ORDER BY "order";
GO

DROP TABLE "SELECT";
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

### <a name="b-using-the-quoted-identifier-setting-with-single-and-double-quotation-marks"></a>B. Utilizzo dell'opzione per l'identificatore delimitato con virgolette singole e doppie

 Nell'esempio seguente viene illustrato l'utilizzo di virgolette singole e doppie in espressioni stringa con l'opzione `SET QUOTED_IDENTIFIER` impostata su `ON` e `OFF`.

```sql
SET QUOTED_IDENTIFIER OFF;
GO

USE AdventureWorks2012;
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
    WHERE TABLE_NAME = 'Test')
    DROP TABLE dbo.Test;
GO
USE AdventureWorks2012;
CREATE TABLE dbo.Test (ID INT, String VARCHAR(30)) ;
GO

-- Literal strings can be in single or double quotation marks.
INSERT INTO dbo.Test VALUES (1, "'Text in single quotes'");
INSERT INTO dbo.Test VALUES (2, '''Text in single quotes''');
INSERT INTO dbo.Test VALUES (3, 'Text with 2 '''' single quotes');
INSERT INTO dbo.Test VALUES (4, '"Text in double quotes"');
INSERT INTO dbo.Test VALUES (5, """Text in double quotes""");
INSERT INTO dbo.Test VALUES (6, "Text with 2 """" double quotes");
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Strings inside double quotation marks are now treated
-- as object names, so they cannot be used for literals.
INSERT INTO dbo."Test" VALUES (7, 'Text with a single '' quote');
GO

-- Object identifiers do not have to be in double quotation marks
-- if they are not reserved keywords.
SELECT ID, String
FROM dbo.Test;
GO

DROP TABLE dbo.Test;
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
 ID          String
 ----------- ------------------------------
 1           'Text in single quotes'
 2           'Text in single quotes'
 3           Text with 2 '' single quotes
 4           "Text in double quotes"
 5           "Text in double quotes"
 6           Text with 2 "" double quotes
 7           Text with a single ' quote
 ```

## <a name="see-also"></a>Vedere anche
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)    
[CREATE DEFAULT](../../t-sql/statements/create-default-transact-sql.md)    
[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)    
[CREATE RULE](../../t-sql/statements/create-rule-transact-sql.md)    
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)    
[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)    
[Tipi di dati](../../t-sql/data-types/data-types-transact-sql.md)    
[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)    
[SELECT](../../t-sql/queries/select-transact-sql.md)    
[Istruzioni SET](../../t-sql/statements/set-statements-transact-sql.md)    
[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)    
[sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)    
[Identificatori del database](../../relational-databases/databases/database-identifiers.md)
