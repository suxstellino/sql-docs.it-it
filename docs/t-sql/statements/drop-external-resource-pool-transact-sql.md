---
description: DROP EXTERNAL RESOURCE POOL (Transact-SQL)
title: DROP EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- DROP EXTERNAL RESOURCE POOL
- DROP_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL RESOURCE POOL statement
ms.assetid: e2fa01bd-96ff-4ea9-bb08-6cb6b6adf68c
author: dphansen
ms.author: davidph
manager: cgronlund
ms.openlocfilehash: 70c4cb5ab7f6808e846bbd8a55ac1d8e75af4ab4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197571"
---
# <a name="drop-external-resource-pool-transact-sql"></a>DROP EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Elimina un pool di risorse esterne di Resource Governor usato per definire risorse per i processi esterni. 

::: moniker range="=sql-server-2016||>=sql-server-linux-ver15"
Per [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] il pool esterno gestisce `rterm.exe`, `BxlServer.exe` e altri processi derivati.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Per [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)], il pool esterno gestisce `rterm.exe`, `python.exe`, `BxlServer.exe` e altri processi derivati.
::: moniker-end

I pool di risorse esterne vengono creati mediante [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md) e modificati mediante [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP EXTERNAL RESOURCE POOL pool_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*pool_name*  
Nome del pool di risorse esterne da eliminare.  
  
## <a name="remarks"></a>Osservazioni

Non è possibile eliminare un pool di risorse esterne contenente gruppi del carico di lavoro.  

Non è possibile eliminare pool predefiniti o interni di Resource Governor.  

Per l'esecuzione di istruzioni DDL, è consigliabile avere familiarità con gli stati di Resource Governor. Per altre informazioni, vedere [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CONTROL SERVER`.  

## <a name="examples"></a>Esempi

Nell'esempio seguente viene eliminato il pool di risorse esterne denominato `ex_pool`.  

```sql
DROP EXTERNAL RESOURCE POOL ex_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Vedere anche

+ [Opzione di configurazione del server external scripts enabled](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)
+ [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)
