---
title: Oggetti di sistema correlati a XEvent
description: Queste risorse sono correlate agli eventi estesi, ad esempio come gli oggetti di sistema li supportano, come SQL Server li usa e aspetti specifici del database SQL di Azure.
ms.date: 03/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jukoesma
ms.technology: xevents
ms.topic: reference
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a2d56121276e0a485d0f7a95f45c16c2a87d866
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481282"
---
# <a name="system-objects-that-support-extended-events"></a>Oggetti di sistema che supportano eventi estesi

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il presente articolo include collegamenti ad altri articoli relativi agli eventi estesi. Si tratta di articoli che descrivono gli argomenti seguenti:

- Oggetti di sistema che forniscono il supporto per la funzionalità degli eventi estesi.
- Parti di SQL Server che usano gli eventi estesi.
- Aspetti degli eventi estesi specifici per il database SQL di Azure nel cloud.

Gli elenchi non sono necessariamente completi.

## <a name="system-tables"></a>Tabelle di sistema

- [Tabelle eventi estesi - trace_xe_action_map](../system-tables/extended-events-tables-trace-xe-action-map.md)

- [Tabelle eventi estesi - trace_xe_event_map](../system-tables/extended-events-tables-trace-xe-event-map.md)

## <a name="system-catalog-views"></a>Viste del catalogo di sistema

- [Viste del catalogo degli eventi estesi (Transact-SQL)](../system-catalog-views/extended-events-catalog-views-transact-sql.md)

- [sys.server_event_sessions (Transact-SQL)](../system-catalog-views/sys-server-event-sessions-transact-sql.md)

- [sys.server_event_session_actions (Transact-SQL)](../system-catalog-views/sys-server-event-session-actions-transact-sql.md)

- [sys.server_event_session_events (Transact-SQL)](../system-catalog-views/sys-server-event-session-events-transact-sql.md)

- [sys.server_event_session_fields (Transact-SQL)](../system-catalog-views/sys-server-event-session-fields-transact-sql.md)

- [sys.server_event_session_targets (Transact-SQL)](../system-catalog-views/sys-server-event-session-targets-transact-sql.md)

## <a name="other-system-objects"></a>Altri oggetti di sistema

- [Viste a gestione dinamica degli eventi estesi](../system-dynamic-management-views/extended-events-dynamic-management-views.md)

## <a name="uses-of-extended-events-by-sql-server-itself"></a>Usi degli eventi estesi per SQL Server

Questo elenco non intende essere completo.

- [Accesso alle informazioni di diagnostica nel log degli eventi estesi](../native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)

- [Configurare eventi estesi per i gruppi di disponibilità Always On](../../database-engine/availability-groups/windows/always-on-extended-events.md)

- [Eventi estesi per Stretch Database](../../sql-server/stretch-database/extended-events-for-stretch-database.md)

## <a name="azure-sql-database-and-extended-events"></a>Database SQL di Azure ed eventi estesi

- [Eventi estesi nel database SQL di Azure](/azure/sql-database/sql-database-xevent-db-diff-from-svr)

- [sys.database_event_session_targets (database SQL di Azure)](../system-catalog-views/sys-database-event-session-targets-azure-sql-database.md)

- [sys.database_event_session_fields (database SQL di Azure)](../system-catalog-views/sys-database-event-session-fields-azure-sql-database.md)

- [sys.database_event_session_events (database SQL di Azure)](../system-catalog-views/sys-database-event-session-events-azure-sql-database.md)

- [sys.database_event_session_actions (database SQL di Azure)](../system-catalog-views/sys-database-event-session-actions-azure-sql-database.md)
