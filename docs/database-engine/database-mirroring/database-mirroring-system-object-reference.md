---
title: Informazioni di riferimento sugli oggetti di sistema per il mirroring del database | Microsoft Docs
description: 'Informazioni sugli oggetti di sistema per il mirroring del database: viste del catalogo di sistema, viste a gestione dinamica del sistema e tabelle di sistema.'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3e5bc078ee4ee8817f8884d5c8e6f638ce432e3e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643715"
---
# <a name="database-mirroring-system-object-reference"></a>Informazioni di riferimento sugli oggetti di sistema per il mirroring del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="system-catalog-views"></a>Viste del catalogo di sistema

| Vista del catalogo di sistema | Descrizione|
| :------ | :----------------------------- |
| [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   | Contiene una riga per ogni ruolo del server di controllo del mirroring che un server riveste in una relazione di mirroring del database. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Viste a gestione dinamica (DMV) di sistema

| Vista a gestione dinamica (DMV) di sistema | Descrizione|
| :------ | :----------------------------- |
| [sys.dm_db_mirroring_auto_page_repair](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)   | Viene restituita una riga per ogni tentativo di correzione automatica della pagina in qualsiasi database con mirroring nell'istanza del server.  |
| [sys.dm_db_mirroring_connections](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)    | Restituisce una riga per ogni connessione stabilita per il mirroring del database. |
| &nbsp; | &nbsp; |

## <a name="system-tables"></a>Tabelle di sistema

| Tabella di sistema | Descrizione|
| :------ | :----------------------------- |
| [sysdbmaintplan_databases](../../relational-databases/system-tables/sysdbmaintplan-databases-transact-sql.md)   | Restituisce informazioni sui piani di manutenzione di mirroring del database. |
| [sysdbmaintplan_history](../../relational-databases/system-tables/sysdbmaintplan-history-transact-sql.md)    | Restituisce informazioni sulla cronologia dei piani di manutenzione di mirroring del database. |
| [sysdbmaintplan_jobs](../../relational-databases/system-tables/sysdbmaintplan-jobs-transact-sql.md)    |Restituisce informazioni sui processi dei piani manutenzione di mirroring del database.  |
| [sysdbmaintplans](../../relational-databases/system-tables/sysdbmaintplans-transact-sql.md)    | Restituisce informazioni sui piani di mirroring del database.  |
| &nbsp; | &nbsp; |


## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   

  
  
