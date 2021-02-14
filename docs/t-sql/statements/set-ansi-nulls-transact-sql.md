---
description: SET ANSI_NULLS (Transact-SQL)
title: SET ANSI_NULLS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/24/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current || azuresqldb-current'
ms.openlocfilehash: 5aded31238cbcfa5abc33b981531796d1b910e13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342110"
---
# <a name="set-ansi_nulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Specifica il comportamento conforme allo standard ISO degli operatori di confronto Uguale a (=) e Diverso da (<>) quando vengono usati con valori Null in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintassi

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintassi per [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] e [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_NULLS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintassi per [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] e [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_NULLS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
Quando l'opzione ANSI_NULLS è impostata su ON, un'istruzione SELECT che usa WHERE *column_name* = **NULL** non restituisce alcuna riga anche se *column_name* include valori Null. Un'istruzione SELECT che usa WHERE *column_name* <> **NULL** non restituisce alcuna riga anche se *column_name* include valori non Null.  
  
Quando l'opzione ANSI_NULLS è impostata su OFF, gli operatori uguale a (=) e diverso da (<>) non seguono lo standard ISO. Un'istruzione SELECT che usa WHERE *column_name* = **NULL** restituisce le righe con valori Null in *column_name*. Un'istruzione SELECT che usa WHERE *column_name* <> **NULL** restituisce le righe con valori non Null nella colonna. Inoltre un'istruzione SELECT che usa WHERE *column_name* <> *XYZ_value* restituisce tutte le righe che non sono *XYZ_value* e non sono NULL.  
  
Quando l'opzione ANSI_NULLS è impostata su ON, tutti i confronti con un valore Null restituiscono UNKNOWN. Quando l'opzione SET ANSI_NULLS è impostata su OFF, i confronti di tutti i dati con un valore Null restituiscono TRUE se il valore dei dati è NULL. In caso di omissione di SET ANSI_NULLS, viene applicata l'impostazione dell'opzione ANSI_NULLS del database corrente. Per altre informazioni sull'opzione di database ANSI_NULLS, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  

La tabella seguente illustra come l'impostazione di ANSI_NULLS influisce sui risultati di un certo numero di espressioni booleane che usano valori NULL e non NULL.  
  
|Espressione booleana|SET ANSI_NULLS ON|SET ANSI_NULLS OFF|  
|---------------|---------------|------------|  
|NULL = NULL|UNKNOWN|TRUE|  
|1 = NULL|UNKNOWN|FALSE|  
|NULL <> NULL|UNKNOWN|FALSE|  
|1 <> NULL|UNKNOWN|TRUE|  
|NULL > NULL|UNKNOWN|UNKNOWN|  
|1 > NULL|UNKNOWN|UNKNOWN|  
|NULL IS NULL|TRUE|TRUE|  
|1 IS NULL|FALSE|FALSE|  
|NULL IS NOT NULL|FALSE|FALSE|  
|1 IS NOT NULL|TRUE|TRUE|  

SET ANSI_NULLS ON influisce su un confronto solo se uno degli operandi del confronto è una variabile NULL o un valore letterale NULL. Se entrambi gli operandi del confronto sono colonne o espressioni composte, l'impostazione non ha effetto sul confronto.  
  
Affinché uno script presenti il funzionamento previsto indipendentemente dall'opzione di database ANSI_NULLS o dall'impostazione di SET ANSI_NULLS, utilizzare IS NULL e IS NOT NULL nei confronti che potrebbero includere valori Null.  
  
Per l'esecuzione di query distribuite, è necessario impostare l'opzione ANSI_NULLS su ON.  
  
È necessario che l'opzione ANSI_NULLS sia impostata su ON anche per la creazione o la modifica di indici su colonne calcolate o viste indicizzate. Se l'opzione SET ANSI_NULLS è impostata su OFF, qualsiasi istruzione CREATE, UPDATE, INSERT e DELETE eseguita in tabelle con indici su colonne calcolate o viste indicizzate avrà esito negativo. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore in cui sono elencate tutte le opzioni SET che violano i valori obbligatori. Se inoltre l'opzione SET ANSI_NULLS è impostata su OFF durante l'esecuzione di un'istruzione SELECT, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignora i valori di indice delle colonne calcolate o delle viste e l'operazione di selezione viene risolta come se le tabelle o le viste non includessero tali indici.  
  
> [!NOTE]  
> ANSI_NULLS è una delle sette opzioni SET che è necessario impostare sui valori richiesti quando si utilizzano colonne calcolate o viste indicizzate. Anche le opzioni `ANSI_PADDING`, `ANSI_WARNINGS`, `ARITHABORT`, `QUOTED_IDENTIFIER` e `CONCAT_NULL_YIELDS_NULL` devono essere impostate su ON e `NUMERIC_ROUNDABORT` deve essere impostata su OFF.  
  
 Il driver ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client e il provider OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostano automaticamente l'opzione ANSI_DEFAULTS su ON al momento della connessione. È possibile configurare questa impostazione nelle origini dei dati ODBC, negli attributi di connessione ODBC o nelle proprietà di connessione OLE DB impostate nell'applicazione prima della connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore predefinito per SET ANSI_NULLS è OFF.  
  
Quando l'opzione ANSI_DEFAULTS è impostata su ON, l'opzione ANSI_NULLS è abilitata.  
  
L'opzione ANSI_NULLS viene definita in fase di esecuzione, non in fase di analisi.  
  
Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito:
  
```sql  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;   
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono usati gli operatori di confronto Uguale a (`=`) e Diverso da (`<>`) per confrontare i valori `NULL` e non Null in una tabella. L'esempio evidenzia anche il fatto che l'impostazione dell'opzione `SET ANSI_NULLS` non influisce su `IS NULL`.  
  
```sql  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO 
```

Impostare ora ANSI_DEFAULTS su ON e testare.

```sql
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
```

Impostare ora ANSI_DEFAULTS su OFF e testare.  

```sql
PRINT 'Testing ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= &#40;Equals&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [&#60;&#62; &#40;Not Equal To&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
