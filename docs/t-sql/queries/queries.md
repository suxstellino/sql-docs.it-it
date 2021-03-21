---
description: Query
title: Query | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5ff02a32-e8d3-479c-ae8b-07581e41f5f8
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8652e15ae1116fda061bc357e3c3feac492ef79d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754481"
---
# <a name="queries"></a>Query

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Data Manipulation Language (DML) è un vocabolario usato per recuperare e utilizzare i dati in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] e nel database SQL. Questo linguaggio è per la maggior parte utilizzabile anche in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] (vedere ogni singola istruzione per i dettagli). Utilizzare queste istruzioni per operazioni di aggiunta, modifica, esecuzione di query o rimozione dei dati di un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 Nella tabella seguente vengono elencate le istruzioni DML utilizzate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

:::row:::
    :::column:::
        [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)
    :::column-end:::
    :::column:::
        [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
    :::column-end:::
    :::column:::
        [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
    :::column-end:::
    :::column:::
        [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)
    :::column-end:::
    :::column:::
        [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

 Nella tabella seguente vengono elencate le clausole utilizzate in più istruzioni o clausole DML.  
  
|Clausola|Può essere utilizzata in queste istruzioni|  
|------------|-------------------------------------|  
|[FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)|DELETE, SELECT, UPDATE|  
|[Hint &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)|DELETE, INSERT, SELECT, UPDATE|  
|[Clausola OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md)|DELETE, SELECT, UPDATE|  
|[Clausola OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)|DELETE, INSERT, MERGE, UPDATE|  
|[Condizione di ricerca &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md)|DELETE, MERGE, SELECT, UPDATE|  
|[Costruttore di valori di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md)|FROM, INSERT, MERGE|  
|[TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)|DELETE, INSERT, MERGE, SELECT, UPDATE|  
|[WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)|DELETE, SELECT, UPDATE, MATCH|  
|[WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)|DELETE, INSERT, MERGE, SELECT, UPDATE|  
| &nbsp; | &nbsp; |
