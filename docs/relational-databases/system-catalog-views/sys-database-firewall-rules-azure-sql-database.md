---
description: sys.database_firewall_rules (Database di SQL Azure)
title: sys.database_firewall_rules (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.database_firewall_rules_TSQL
- database_firewall_rules_TSQL
- sys.database_firewall_rules
- database_firewall_rules
dev_langs:
- TSQL
helpviewer_keywords:
- database_firewall_rules
- sys.database_firewall_rules
ms.assetid: 2e821593-3b9f-43d6-a99b-1ceffe177faf
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: 6e09c2043494b2d01adb3781b7b3c4336a3b9a1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195876"
---
# <a name="sysdatabase_firewall_rules-azure-sql-database"></a>sys.database_firewall_rules (Database di SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce informazioni sulle impostazioni del firewall a livello di database associate al [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] . Le impostazioni del firewall a livello di database sono particolarmente utili quando si usano utenti del database indipendente. Per altre informazioni, vedere [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
 La vista `sys.database_firewall_rules` contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|id|**INTEGER**|Identificatore dell'impostazione del firewall a livello di database.|  
|name|**NVARCHAR (128)**|Il nome scelto per descrivere e distinguere l'impostazione del firewall a livello di database.|  
|start_ip_address|**VARCHAR (45)**|L'indirizzo IP più basso nell'intervallo dell'impostazione del firewall a livello di database. Gli indirizzi IP uguali o maggiori di questo possono tentare la connessione all'istanza del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L'indirizzo IP più basso possibile è `0.0.0.0`.|  
|end_ip_address|**VARCHAR (45)**|L'indirizzo IP più alto nell'intervallo dell'impostazione del firewall. Gli indirizzi IP uguali o minori di questo possono tentare la connessione all'istanza del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L'indirizzo IP più alto possibile è `255.255.255.255`.<br /><br /> Nota: i tentativi di connessione di Azure sono consentiti quando sia questo campo che il campo **start_ip_address** è uguale a `0.0.0.0` .|  
|create_date|**DATETIME**|Data e ora UTC in cui è stata creata l'impostazione del firewall a livello di database.|  
|modify_date|**DATETIME**|Data e ora UTC in cui è stata modificata per l'ultima volta l'impostazione del firewall a livello di database.|  
  
## <a name="remarks"></a>Commenti  
 Per restituire informazioni sulle impostazioni del firewall a livello di server associate alla database SQL di Microsoft Azure, usare [sys.firewall_rules (database SQL di Azure)](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa vista è disponibile nel database **Master** e in ogni database utente. L'accesso di sola lettura a questa vista è disponibile per tutti gli utenti che dispongono dell'autorizzazione per connettersi al database.  
  
## <a name="see-also"></a>Vedere anche
[sp_set_database_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
[sp_delete_database_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)  
[sp_set_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
[sp_delete_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database.md)   
[sys.firewall_rules &#40;database SQL di Azure&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)  
[Configurare un Windows Firewall per l'accesso motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)     
[Configurare un firewall per l'accesso FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)  
[Configurare un firewall per l'accesso al server di report](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
