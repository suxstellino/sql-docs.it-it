---
description: Funzioni di configurazione (Transact-SQL)
title: Funzioni di configurazione (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], configuration
- configuration options [SQL Server], functions
- current configuration information
- configuration functions [SQL Server]
ms.assetid: 066f15e7-3406-437e-93c4-3f247c529169
author: cawrites
ms.author: chadam
ms.openlocfilehash: 08684b28111619e1525232dbfab27a225c549d9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212239"
---
# <a name="configuration-functions-transact-sql"></a>Funzioni di configurazione (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Queste funzioni scalari restituiscono informazioni sulle impostazioni correnti delle opzioni di configurazione:
  
- [@@DATEFIRST](../../t-sql/functions/datefirst-transact-sql.md)
- [@@DBTS](../../t-sql/functions/dbts-transact-sql.md)
- [@@LANGID](../../t-sql/functions/langid-transact-sql.md)
- [@@LANGUAGE](../../t-sql/functions/language-transact-sql.md)
- [@@LOCK_TIMEOUT](../../t-sql/functions/lock-timeout-transact-sql.md)
- [@@MAX_CONNECTIONS](../../t-sql/functions/max-connections-transact-sql.md)
- [@@MAX_PRECISION](../../t-sql/functions/max-precision-transact-sql.md)
- [@@NESTLEVEL](../../t-sql/functions/nestlevel-transact-sql.md)
- [@@OPTIONS](../../t-sql/functions/options-transact-sql.md)
- [@@REMSERVER](../../t-sql/functions/remserver-transact-sql.md)
- [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md)
- [@@SERVICENAME](../../t-sql/functions/servicename-transact-sql.md)
- [@@SPID](../../t-sql/functions/spid-transact-sql.md)
- [@@TEXTSIZE](../../t-sql/functions/textsize-transact-sql.md)
- [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md)

Tutte le funzioni di configurazione operano in modo non deterministico. Questo significa che non restituiscono sempre gli stessi risultati ogni volta che vengono chiamate, anche se il set di valori di input è lo stesso. Per altre informazioni sul determinismo delle funzioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="see-also"></a>Vedere anche

[Funzioni &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)
