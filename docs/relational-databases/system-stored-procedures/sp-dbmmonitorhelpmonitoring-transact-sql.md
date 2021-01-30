---
description: sp_dbmmonitorhelpmonitoring (Transact-SQL)
title: sp_dbmmonitorhelpmonitoring (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorhelpmonitoring
- sp_dbmmonitorhelpmonitoring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorhelpmonitoring
- database mirroring [SQL Server], monitoring
ms.assetid: a085cf87-269f-454a-a146-21f80a113b72
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3ea98cea7184683921c98bf4d4fd736c9730924e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212365"
---
# <a name="sp_dbmmonitorhelpmonitoring-transact-sql"></a>sp_dbmmonitorhelpmonitoring (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il periodo di aggiornamento corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dbmmonitorhelpmonitoring   
```  
  
## <a name="arguments"></a>Argomenti  
 nessuno  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 Restituisce il periodo di aggiornamento corrente, ovvero il numero di minuti tra gli aggiornamenti della tabella di stato di mirroring del database. Il valore è compreso tra 1 e 120 minuti.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituito il periodo di aggiornamento corrente.  
  
```  
EXEC sp_dbmmonitorhelpmonitoring;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
