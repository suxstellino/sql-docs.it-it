---
description: sp_repltrans (Transact-SQL)
title: sp_repltrans (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repltrans_TSQL
- sp_repltrans
helpviewer_keywords:
- sp_repltrans
ms.assetid: 738e2322-335b-44fa-820e-f31c02743978
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c408bc1563046bb300e7dd862074b87b59edbee2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211632"
---
# <a name="sp_repltrans-transact-sql"></a>sp_repltrans (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce un set di risultati che include tutte le transazioni del log delle transazioni del database di pubblicazione contrassegnate per la replica, ma non contrassegnate come distribuite. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_repltrans  
```  
  
## <a name="result-sets"></a>Set di risultati  
 **sp_repltrans** restituisce informazioni sul database di pubblicazione da cui viene eseguita, consentendo di visualizzare le transazioni attualmente non distribuite (le transazioni rimanenti nel log delle transazioni che non sono state inviate al server di distribuzione). Il set di risultati include i numeri di sequenza del file di log del primo e dell'ultimo record per ogni transazione. **sp_repltrans** è simile a [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) ma non restituisce i comandi per le transazioni.  
  
## <a name="remarks"></a>Commenti  
 **sp_repltrans** viene utilizzata nella replica transazionale.  
  
 **sp_repltrans** non è supportato per i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_repltrans**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_repldone &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
