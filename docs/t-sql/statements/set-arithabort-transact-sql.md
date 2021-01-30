---
description: SET ARITHABORT (Transact-SQL)
title: SET ARITHABORT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ARITHABORT_TSQL
- ARITHABORT
- SET ARITHABORT
- SET_ARITHABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- terminating queries
- queries [SQL Server], terminating
- overflow errors [SQL Server]
- ARITHABORT option
- divide-by-zero errors
- SET ARITHABORT statement
- ending queries [SQL Server]
- stopping queries
ms.assetid: f938a666-fdd1-4233-b97f-719f27b1a0e6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43ad8593836d262db63540de34d86e04ebdf7ab4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190426"
---
# <a name="set-arithabort-transact-sql"></a>SET ARITHABORT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Interrompe una query quando si verifica un errore di divisione per zero o di overflow durante l'esecuzione della query stessa.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintassi per [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] e [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ARITHABORT { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintassi per [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] e [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ARITHABORT ON
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
Impostare sempre ARITHABORT su ON nelle sessioni di accesso. L'impostazione di ARITHABORT su OFF può influire negativamente sull'ottimizzazione delle query causando problemi di prestazioni.  
  
> [!WARNING]  
>  L'impostazione ARITHABORT predefinita per [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è ON. Le applicazioni client che impostano l'opzione ARITHABORT su OFF potrebbero ricevere piani di query diversi e, di conseguenza, la risoluzione di errori di query con prestazioni scarse risulta difficile. In altre parole, la stessa query potrebbe essere eseguita rapidamente in Management Studio, ma lentamente nell'applicazione. Per la risoluzione dei problemi relativi alle query con [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], far sempre corrispondere l'impostazione ARITHABORT del client.  
  
Quando SET ARITHABORT e SET ANSI WARNINGS sono impostate su ON, queste condizioni di errore provocano l'interruzione della query.  
  
Quando SET ARITHABORT è impostata su ON e SET ANSI WARNINGS su OFF, queste condizioni di errore provocano l'interruzione del batch. Se gli errori si verificano in una transazione, viene eseguito il rollback della transazione. Quando SET ARITHABORT è impostata su OFF e si verifica uno di questi errori, viene visualizzato un messaggio di avviso e il risultato dell'espressione aritmetica è NULL.  
  
Quando SET ARITHABORT e SET ANSI WARNINGS sono impostate su OFF e si verifica uno di questi errori, viene visualizzato un messaggio di avviso e il risultato dell'operazione aritmetica è NULL.  
  
> [!NOTE]  
>  Se entrambe le opzioni SET ARITHABORT e SET ARITHIGNORE non sono impostate su ON, dopo l'esecuzione della query [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce NULL e un messaggio di avviso.  
  
Quando ANSI_WARNINGS ha un valore ON e il livello di compatibilità del database è impostato su 90 o su un valore superiore, ARITHABORT è implicitamente ON indipendentemente dall'impostazione del valore. Se il livello di compatibilità del database è impostato su 80 o su un valore inferiore, l'opzione ARITHABORT deve essere impostata esplicitamente su ON.  
  
Per la valutazione dell'espressione, se SET ARITHABORT è impostata su OFF e un'istruzione INSERT, UPDATE o DELETE rileva un errore aritmetico, di overflow, di divisione per zero o di dominio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inserisce o aggiorna un valore NULL. Se la colonna di destinazione non ammette valori Null, l'operazione di inserimento o aggiornamento ha esito negativo e viene generato un errore per l'utente.  
  
Quando SET ARITHABORT o SET ARITHIGNORE è impostata su OFF e SET ANSI_WARNINGS è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce comunque un messaggio di errore quando si verificano errori di divisione per zero o di overflow.  
  
Quando SET ARITHABORT è OFF e si verifica un errore di interruzione durante la valutazione della condizione booleana di un'istruzione IF, viene eseguito il segmento di codice associato alla valutazione FALSE.
  
È necessario che l'opzione SET ARITHABORT sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate. Se l'opzione è impostata su OFF, le istruzioni CREATE, UPDATE, INSERT e DELETE eseguite sulle tabelle che includono indici in colonne calcolate o viste indicizzate hanno esito negativo.
  
L'opzione SET ARITHABORT viene impostata in fase di esecuzione, non in fase di analisi.  
  
Per visualizzare l'impostazione corrente per SET ARITHABORT, eseguire la query riportata di seguito:
  
```sql  
DECLARE @ARITHABORT VARCHAR(3) = 'OFF';  
IF ( (64 & @@OPTIONS) = 64 ) SET @ARITHABORT = 'ON';  
SELECT @ARITHABORT AS ARITHABORT;  
  
```  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente vengono illustrati gli errori di divisione per zero e gli errori di overflow con impostazioni `SET ARITHABORT`.  
  
```sql  
-- SET ARITHABORT  
-------------------------------------------------------------------------------  
-- Create tables t1 and t2 and insert data values.  
CREATE TABLE t1 (  
   a TINYINT,   
   b TINYINT  
);  
CREATE TABLE t2 (  
   a TINYINT  
);  
GO  
INSERT INTO t1   
VALUES (1, 0);  
INSERT INTO t1   
VALUES (255, 1);  
GO  
  
PRINT '**_ SET ARITHABORT ON';  
GO  
-- SET ARITHABORT ON and testing.  
SET ARITHABORT ON;  
GO  
  
PRINT '_*_ Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab   
FROM t1;  
GO  
  
PRINT '_*_ Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
  
PRINT '_*_ Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '_*_ Resulting data - should be no data';  
GO  
SELECT _   
FROM t2;  
GO  
  
-- Truncate table t2.  
TRUNCATE TABLE t2;  
GO  
  
-- SET ARITHABORT OFF and testing.  
PRINT '*** SET ARITHABORT OFF';  
GO  
SET ARITHABORT OFF;  
GO  
  
-- This works properly.  
PRINT '**_ Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab    
FROM t1;  
GO  
  
-- This works as if SET ARITHABORT was ON.  
PRINT '_*_ Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
PRINT '_*_ Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '_*_ Resulting data - should be 0 rows';  
GO  
SELECT _   
FROM t2;  
GO  
  
-- Drop tables t1 and t2.  
DROP TABLE t1;  
DROP TABLE t2;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ARITHIGNORE &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithignore-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
  
