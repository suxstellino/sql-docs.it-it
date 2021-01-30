---
description: managed_backup managed_backup.fn_is_master_switch_on (Transact-SQL)
title: managed_backup managed_backup.fn_is_master_switch_on (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_is_master_switch_on
- fn_is_master_switch_on_TSQL
- smart_admin.fn_is_master_switch_on
- smart_admin.fn_is_master_switch_on_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_is_master_switch_on
- fn_is_master_switch_on
ms.assetid: e8c2108d-b104-46cb-9645-a15f46112c86
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 32395235a4d644814c7ff090fd74abc9030b2eda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207380"
---
# <a name="managed_backupfn_is_master_switch_on-transact-sql"></a>managed_backup managed_backup.fn_is_master_switch_on (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Restituisce lo stato delle operazioni del [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] nell'istanza di SQL Server.  
  
 Utilizzare questa funzione per ottenere lo stato corrente del [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
managed_backup.fn_is_master_switch_on ()  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 nessuno  
  
## <a name="return-type"></a>Tipo restituito  
 **BIT**  
  
 1 = [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è attivo, 0 = [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è sospeso.  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Sono richieste le autorizzazioni SELECT per la funzione.  
  
## <a name="see-also"></a>Vedere anche  
 [Backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
