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
ms.openlocfilehash: 4e322ff478e9ccdc031882e7c3b5b18fd8506e42
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186468"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>Funzionalità del motore di database deprecate in SQL Server 2019 (15. x)

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] non depreca le funzionalità oltre a quelle deprecate nelle versioni precedenti:

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-guidelines"></a>Linee guida per la deprecazione

Quando una funzionalità è contrassegnata come deprecata significa che:

- La funzionalità è solo in modalità manutenzione. Non verrà apportata alcuna nuova modifica, inclusi quelli relativi all'interoperabilità tra le nuove funzionalità.
- Microsoft si impegna a non rimuovere una funzionalità deprecata dalle versioni future per semplificare gli aggiornamenti. In rare situazioni, tuttavia, è possibile scegliere di interrompere definitivamente (rimuovere) la funzionalità da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se limita le innovazioni future.
- Per le nuove attività di sviluppo, non usare le funzionalità deprecate. Per aplications esistenti, pianificare la modifica delle applicazioni che attualmente usano queste funzionalità il prima possibile.     

È possibile monitorare l'utilizzo delle funzionalità deprecate tramite il [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contatore delle prestazioni oggetto funzionalità deprecate oppure gli `deprecation_announcement` `deprecation_final_support` eventi estesi ed. Per altre informazioni, vedere [Usare oggetti di SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

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
