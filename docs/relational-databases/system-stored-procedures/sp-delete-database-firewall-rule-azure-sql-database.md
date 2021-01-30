---
description: sp_delete_database_firewall_rule (Database di SQL Azure)
title: sp_delete_database_firewall_rule (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 08/04/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_delete_database_firewall_rule
- sp_delete_database_firewall_rule_TSQL
- sys.sp_delete_database_firewall_rule
- sys.sp_delete_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_database_firewall_rule procedure
ms.assetid: ed295312-e586-4fc2-9e80-806b490ee7bd
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: 2206974640e6a2bb1d37c4b4853c8a74e37a1ceb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195493"
---
# <a name="sp_delete_database_firewall_rule-azure-sql-database"></a>sp_delete_database_firewall_rule (Database di SQL Azure)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Rimuove l'impostazione del firewall a livello di database dal [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] . Le regole del firewall del database possono essere configurate ed eliminate per il database master e per i database utente in [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] .   
  
 
## <a name="syntax"></a>Sintassi  
  
```    
sp_delete_database_firewall_rule [@name =] [N]'name'
[ ; ]  
```  
  
## <a name="arguments"></a>Argomenti  
 `[@name =] [N]'name'`  
 Nome dell'impostazione del firewall a livello di database che verrà rimossa. *Name* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito. L'identificatore Unicode `N` è facoltativo per [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] . 
  
## <a name="permissions"></a>Autorizzazioni  
 Solo l'account di accesso dell'entità di livello server creato dal processo di provisioning o un'entità Azure Active Directory assegnata come amministratore può eliminare le regole del firewall a livello di database.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene rimossa l'impostazione del firewall a livello di database denominata `Example DB Setting 1` .
  
```  
-- Remove database-level firewall setting  
EXECUTE sp_delete_database_firewall_rule N'Example DB Setting 1';  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Firewall del database SQL di Azure](/azure/azure-sql/database/firewall-configure)   
 [Procedura: configurare le impostazioni del firewall (database SQL di Azure)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sp_set_database_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)   
 [sys.database_firewall_rules &#40;database SQL di Azure&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
