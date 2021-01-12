---
description: dbo.syssessions (Transact-SQL)
title: Sessioni di dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/30/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.syssessions_TSQL
- dbo.syssessions
- syssessions_TSQL
- syssessions
dev_langs:
- TSQL
helpviewer_keywords:
- syssessions system table
ms.assetid: 187819b6-c7f4-4a26-b74c-0a89e96695cf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 67e34792baf2dbcb49677ea13746dd9cd53034a6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098293"
---
# <a name="dbosyssessions-transact-sql"></a>dbo.syssessions (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Crea una nuova sessione a ogni avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent utilizza le sessioni per mantenere lo stato dei processi quando il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene riavviato o arrestato in modo imprevisto. Ogni riga della tabella **syssessions** contiene informazioni su una sessione. Usare la tabella **sysjobactivity** per visualizzare lo stato del processo alla fine di ogni sessione.  
  
 Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID di una sessione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Questo session_id non è lo SPID per la sessione, bensì un valore IDENTITY all'interno di questa tabella di sistema.|  
|**agent_start_date**|**datetime**|Data e ora di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per la sessione corrente.|  
  
## <a name="remarks"></a>Osservazioni  
 Solo gli utenti membri del ruolo predefinito del server **sysadmin** possono accedere a questa tabella.  
  
## <a name="see-also"></a>Vedere anche  
 [dbo.sysjobactivity &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysjobactivity-transact-sql.md)  
  
  
