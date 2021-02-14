---
description: Funzionalità del motore di database deprecate in [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Funzionalità del motore di database deprecate in SQL Server 2019 | Microsoft Docs
titleSuffix: SQL Server 2019
ms.custom: seo-lt-2019
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated changes 2019 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: fe188076c467f4ed08fedadefe847afb797c5893
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489485"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>Funzionalità del motore di database deprecate in SQL Server 2019 (15. x)

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] non depreca le funzionalità oltre a quelle deprecate nelle versioni precedenti:

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-pollicy"></a>Deprecation pollicy

Quando una funzionalità è contrassegnata come deprecata significa che:

- La funzionalità è solo in modalità manutenzione. Non verranno apportate nuove modifiche, incluse quelle correlate all'interoperabilità con le nuove funzionalità.
- Microsoft si impegna a non rimuovere una funzionalità deprecata dalle versioni future per semplificare gli aggiornamenti. Tuttavia, in rare situazioni, una funzionalità potrebbe essere rimossa in modo permanente da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se ne limita le innovazioni future.
- Per i nuovi progetti di sviluppo, non è consigliabile usare funzionalità deprecate.      

È possibile monitorare l'utilizzo delle funzionalità deprecate tramite il contatore delle prestazioni [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :Funzionalità deprecate e gli eventi di traccia. Per altre informazioni, vedere [Usare oggetti di SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

## <a name="query-deprecated-features"></a>Eseguire query sulle funzionalità deprecate

I valori di questi contatori sono disponibili anche tramite l'istruzione seguente:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

### <a name="see-also"></a>Vedi anche

- [Modifiche che causano interruzioni apportate alle funzionalità del motore di database in SQL Server 2019](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Funzionalità del motore di database non più disponibili in SQL Server](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)
- [Compatibilità con le versioni precedenti del motore di database di SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)