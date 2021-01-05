---
description: DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)
title: DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: b3cfbc2117d6c3dd3599443a6f5b6e71a2d3186e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637435"
---
# <a name="dbcc-showresultcachespaceused-transact-sql"></a>DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Mostra lo spazio di archiviazione usato per la memorizzazione nella cache dei set di risultati per un database [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] di Azure.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DBCC SHOWRESULTCACHESPACEUSED  
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="remarks"></a>Osservazioni

Il comando `DBCC SHOWRESULTCACHESPACEUSED` non accetta parametri e restituisce lo spazio usato dal database in cui viene eseguito il comando.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione VIEW SERVER STATE.
  
## <a name="result-sets"></a>Set di risultati  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|reserved_space|bigint|Spazio totale usato per il database, in KB. Questo numero cambierà con l'aumento del set di risultati memorizzato nella cache.|  
|data_space|bigint|Spazio usato per i dati, in KB.|  
|index_space|bigint|Spazio usato per gli indici, in KB.|  
|unused_space|bigint|Spazio che è parte dello spazio riservato e non usato, in KB.|  

## <a name="see-also"></a>Vedere anche

[Ottimizzazione delle prestazioni con memorizzazione nella cache dei set di risultati](/azure/sql-data-warehouse/performance-tuning-result-set-caching)</br>
[Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../statements/alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)</br>
[ALTER DATABASE &#40;Transact-SQL&#41;](../statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)</br>
[SET RESULT SET CACHING &#40;Transact-SQL&#41;](../statements/set-result-set-caching-transact-sql.md)</br>
[DBCC DROPRESULTSETCACHE  &#40;Transact-SQL&#41;](./dbcc-dropresultsetcache-transact-sql.md)