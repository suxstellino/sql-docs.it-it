---
description: Tabelle di sistema (Transact-SQL)
title: Tabelle di sistema (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server]
- tables [SQL Server], system tables
- information retrieval [SQL Server]
- status information [SQL Server], system tables
- system information [SQL Server]
- system tables [SQL Server]
- system tables [SQL Server], about system tables
- system tables [SQL Server], retrieving information from
- retrieving system table information
ms.assetid: 56b8ad51-930c-4e5c-8d99-8c939d5b70ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: a6ab9da1443f2ce4e6b51e7d6bc995bbb9591a2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203589"
---
# <a name="system-tables-transact-sql"></a>Tabelle di sistema (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Negli argomenti di questa sezione vengono descritte le tabelle di sistema disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Gli utenti non devono modificare direttamente le tabelle di sistema, ad esempio con istruzioni DELETE, UPDATE o INSERT oppure con trigger definiti dall'utente.  
  
 È consentito fare riferimento alle colonne documentate delle tabelle di sistema. Molte colonne tuttavia non sono documentate. È pertanto consigliabile non creare applicazioni che eseguono query dirette nelle colonne non documentate. Per recuperare informazioni archiviate nelle tabelle di sistema, è necessario che le applicazioni utilizzino i componenti seguenti:  
  
-   Stored procedure di sistema  
  
-   Istruzioni e funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oggetti SMO (Management Objects)  
  
-   Oggetti RMO (Replication Management Objects)  
  
-   Funzioni del catalogo delle API per i database  
  
 Questi componenti compongono un'API pubblicata per il recupero delle informazioni sul sistema da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La compatibilità di tali componenti viene mantenuta nelle diverse versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Il formato delle tabelle di sistema dipende dalla struttura interna di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e può variare nelle diverse versioni. Può pertanto essere necessario apportare modifiche alle applicazioni che hanno accesso diretto a colonne non documentate delle tabelle di sistema in modo che possano accedere alle versioni successive di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 Gli argomenti relativi alle tabelle di sistema sono organizzati in base alle aree funzionali riportate di seguito.  

:::row:::
    :::column:::
        [Tabelle di backup e ripristino &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)

        [Tabelle Change Data Capture &#40;Transact-SQL&#41;](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)

        [Tabelle del piano di manutenzione del database &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/database-maintenance-plan-tables-transact-sql.md)

        [Tabelle degli eventi estesi di SQL Server &#40;Transact-SQL&#41;](../../relational-databases/extended-events/xevents-references-system-objects.md#system-tables)

        [Integration Services tabelle &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/integration-services-tables-transact-sql.md)
    :::column-end:::
    :::column:::
        [Tabelle di log shipping &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-tables-transact-sql.md)

        [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)

        [SQL Server Agent tabelle &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)

        [sys.sysoledbusers &#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysoledbusers-transact-sql.md)

        [systranschemas &#40;&#41;Transact-SQL ](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Viste di compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
