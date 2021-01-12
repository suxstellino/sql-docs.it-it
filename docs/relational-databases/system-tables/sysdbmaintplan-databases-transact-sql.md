---
description: sysdbmaintplan_databases (Transact-SQL)
title: sysdbmaintplan_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplan_databases
- sysdbmaintplan_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplan_databases system table
ms.assetid: f8413a44-8fcc-4899-84f2-b4afe0f8ec08
author: cawrites
ms.author: chadam
ms.openlocfilehash: 60cbb5bf6e533837696328f68b33821fbc8108ed
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096141"
---
# <a name="sysdbmaintplan_databases-transact-sql"></a>sysdbmaintplan_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa tabella è inclusa per mantenere le informazioni esistenti per le istanze aggiornate da una versione precedente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive non modificano il contenuto della tabella. Questa tabella è archiviata nel database **msdb** .  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**plan_id**|**Uniqueidentifier**|ID del piano di manutenzione.|  
|**database_name**|**sysname**|Nome del database associato al piano di manutenzione del database.|  
  
  
