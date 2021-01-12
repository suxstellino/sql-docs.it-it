---
description: SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)
title: SET CONCAT_NULL_YIELDS_NULL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONCAT_NULL_YIELDS_NULL_TSQL
- SET CONCAT_NULL_YIELDS_NULL
- CONCAT_NULL_YIELDS_NULL
- SET_CONCAT_NULL_YIELDS_NULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT_NULL_YIELDS_NULL option
- null values [SQL Server], concatenation results
- concatenation [SQL Server]
- SET CONCAT_NULL_YIELDS_NULL statement
ms.assetid: 3091b71c-6518-4eb4-88ab-acae49102bc5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f97aaed0dbd0cef996f015c5498505b1b81dd176
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091881"
---
# <a name="set-concat_null_yields_null-transact-sql"></a>SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Determina se i risultati della concatenazione di valori Null vengono gestiti come valori Null o valori di stringa vuota.  
  
> [!IMPORTANT]  
>  In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'opzione CONCAT_NULL_YIELDS_NULL sarà sempre impostata su ON e qualsiasi applicazione che la imposta in modo esplicito su OFF restituirà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server  
    
SET CONCAT_NULL_YIELDS_NULL { ON | OFF }   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
SET CONCAT_NULL_YIELDS_NULL ON    
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 Quando l'opzione SET CONCAT_NULL_YIELDS_NULL è impostata su ON, la concatenazione di un valore Null con una stringa restituisce NULL. Ad esempio, `SELECT 'abc' + NULL` restituisce `NULL`. Quando l'opzione SET CONCAT_NULL_YIELDS_NULL è impostata su OFF, la concatenazione di un valore Null con una stringa restituisce la stringa stessa (il valore Null viene considerato una stringa vuota). Ad esempio, `SELECT 'abc' + NULL` restituisce `abc`.  
  
 Se non si specifica SET CONCAT_NULL_YIELDS_NULL, si applica l'impostazione dell'opzione di database **CONCAT_NULL_YIELDS_NULL**.  
  
> [!NOTE]  
>  L'impostazione dell'opzione SET CONCAT_NULL_YIELDS_NULL è uguale all'impostazione di CONCAT_NULL_YIELDS_NULL di ALTER DATABASE.  
  
 L'opzione SET CONCAT_NULL_YIELDS_NULL viene impostata in fase di esecuzione, non in fase di analisi.  

È necessario che l'opzione SET CONCAT_NULL_YIELDS_NULL sia impostata su **ON** durante la creazione o la modifica di viste indicizzate, indici in colonne calcolate, indici filtrati o indici spaziali. Se l'opzione SET CONCAT_NULL_YIELDS_NULL è impostata su **OFF**, qualsiasi istruzione CREATE, UPDATE, INSERT e DELETE eseguita in tabelle con indici su colonne calcolate, indici filtrati, indici spaziali o viste indicizzate avrà esito negativo. Per altre informazioni sulle impostazioni dell'opzione SET necessarie per viste indicizzate e indici nelle colonne calcolate, vedere "Considerazioni sull'uso delle istruzioni SET" nell'argomento [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).
  
 Quando l'opzione CONCAT_NULL_YIELDS_NULL è impostata su OFF, la concatenazione delle stringhe in più server non può verificarsi.  
  
 Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito.  
  
```sql
DECLARE @CONCAT_SETTING VARCHAR(3) = 'OFF';  
IF ( (4096 & @@OPTIONS) = 4096 ) SET @CONCAT_SETTING = 'ON';  
SELECT @CONCAT_SETTING AS CONCAT_NULL_YIELDS_NULL; 
```  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato l'utilizzo di entrambe le impostazioni `SET CONCAT_NULL_YIELDS_NULL`.  
  
```sql
PRINT 'Setting CONCAT_NULL_YIELDS_NULL ON';  
GO  
-- SET CONCAT_NULL_YIELDS_NULL ON and testing.  
SET CONCAT_NULL_YIELDS_NULL ON;  
GO  
SELECT 'abc' + NULL ;  
GO  
  
-- SET CONCAT_NULL_YIELDS_NULL OFF and testing.  
SET CONCAT_NULL_YIELDS_NULL OFF;  
GO  
SELECT 'abc' + NULL;   
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
  
