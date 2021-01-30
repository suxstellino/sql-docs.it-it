---
description: managed_backup managed_backup.fn_backup_instance_config (Transact-SQL)
title: managed_backup managed_backup.fn_backup_instance_config (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_backup_instance_config
- smart_admin.fn_backup_instance_config_TSQL
- fn_backup_instance_config_TSQL
- smart_admin.fn_backup_instance_config
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_instance_config
- fn_backup_instance_config
ms.assetid: 2382a547-c0c9-4e1d-87c9-d8526192eb5a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 534b1417536b233cafd0c7f3499e6ff801f0fa42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207421"
---
# <a name="managed_backupfn_backup_instance_config-transact-sql"></a>managed_backup managed_backup.fn_backup_instance_config (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Restituisce una riga con le impostazioni di configurazione predefinite del [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per l'istanza di SQL Server.  
  
 Utilizzare questa stored procedure per controllare o determinare le impostazioni di configurazione correnti del [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per un'istanza di SQL Server.  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
managed_backup.fn_backup_db_config ()  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 nessuno  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|is_smart_backup_enabled|INT|Visualizza 1 quando il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è abilitato e 0 quando il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è disabilitato.|  
|credential_name|SYSNAME|Credenziali SQL predefinite utilizzate per l'autenticazione per l'archiviazione.|  
|retention_days|INT|Periodo di memorizzazione predefinito impostato a livello di istanza.|  
|storage_url|NVARCHAR (1024)|URL dell'account di archiviazione predefinito impostato a livello di istanza.|  
|encryption_algorithm|SYSNAME|Nome dell'algoritmo di crittografia. Se la crittografia non è specificata, è impostato su NULL.|  
|encryptor_type|NVARCHAR (32)|Tipo di componente di crittografia usato: certificato o chiave asimmetrica. Se nessun componente di crittografia è specificato, è impostato su NULL.|  
|encryptor_name|SYSNAME|Nome del certificato o della chiave asimmetrica. Se nessun nome è specificato, è impostato su NULL.|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo del database **db_backupoperator** con autorizzazioni **ALTER ANY CREDENTIAL** . All'utente non devono essere negate le autorizzazioni **View any Definition** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite le impostazioni di configurazione predefinite del [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] per l'istanza in cui viene eseguito.  
  
```  
Use msdb;  
GO  
SELECT * FROM managed_backup.fn_backup_instance_config ();  
  
```  
  
  
