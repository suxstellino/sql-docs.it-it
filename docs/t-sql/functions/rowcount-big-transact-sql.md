---
description: ROWCOUNT_BIG (Transact-SQL)
title: ROWCOUNT_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROWCOUNT_BIG
- ROWCOUNT_BIG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ROWCOUNT_BIG function
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 6e18a0eb-bb36-4348-90d9-8b1ecf095064
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: fb0770aba26099fbc10aebfdd59d6094007dc076
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183514"
---
# <a name="rowcount_big-transact-sql"></a>ROWCOUNT_BIG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il numero di righe modificate dall'ultima istruzione eseguita. Questa funzione opera esattamente come [@@ROWCOUNT](../../t-sql/functions/rowcount-transact-sql.md), con la sola differenza che il valore restituito da ROWCOUNT_BIG è di tipo **bigint**.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ROWCOUNT_BIG ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **bigint**  
  
## <a name="remarks"></a>Osservazioni  
 Se segue l'istruzione SELECT, questa funzione restituisce il numero di righe restituite dall'istruzione SELECT.  
  
 Se eseguita dopo le istruzioni INSERT, UPDATE o DELETE, questa funzione restituisce il numero di righe modificate dall'istruzione di modifica dei dati.  
  
 Se viene eseguita dopo istruzioni che non restituiscono alcuna riga, ad esempio un'istruzione IF, questa funzione restituisce zero (0).  
  
## <a name="see-also"></a>Vedere anche  
 [COUNT_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/count-big-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
