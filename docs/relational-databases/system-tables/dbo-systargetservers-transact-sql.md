---
description: dbo.systargetservers (Transact-SQL)
title: dbo.systargetServers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.systargetservers_TSQL
- dbo.systargetservers
- systargetservers_TSQL
- systargetservers
dev_langs:
- TSQL
helpviewer_keywords:
- systargetservers system table
ms.assetid: 479d1314-be37-4d19-ac9c-419fc9110e53
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6c89bf252a4e06db5f0398f9553d1f8190071c12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207261"
---
# <a name="dbosystargetservers-transact-sql"></a>dbo.systargetservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra i server di destinazione integrati nel dominio multiserver specificato.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|ID del server.|  
|**server_name**|**sysname**|Nome del server.|  
|**location**|**nvarchar(200)**|Posizione del server di destinazione specificato.|  
|**time_zone_adjustment**|**int**|Regolazione del fuso orario in ore rispetto all'ora di Greenwich (GMT).|  
|**enlist_date**|**datetime**|Data e ora in cui il server di destinazione specificato è stato integrato.|  
|**last_poll_date**|**datetime**|Data e ora dell'ultimo polling del server di destinazione specificato nella tabella di sistema **sysdownloadlist** multiserver per l'esecuzione dei processi.|  
|**Stato**|**int**|Stato del server di destinazione:<br /><br /> **1** = normale<br /><br /> **2** = risincronizzazione in sospeso<br /><br /> **4** = sospetto offline|  
|**local_time_at_last_poll**|**datetime**|Data e ora dell'ultimo polling del server di destinazione per operazioni di processo.|  
|**enlisted_by_nt_user**|**nvarchar (100)**|Nome utente della persona che esegue **sp_msx_enlist** nel server di destinazione.|  
|**poll_internal**|**int**|Numero di secondi che deve trascorrere prima che il server di destinazione esegua il polling del server master per individuare nuove istruzioni di download.|  
  
  
