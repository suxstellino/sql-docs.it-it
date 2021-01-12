---
description: sys.firewall_rules (Database SQL di Azure)
title: sys.firewall_rules (database SQL di Azure) | Microsoft Docs
ms.date: 03/26/2019
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.firewall_rules
- firewall_rules
- sys.firewall_rules_TSQL
- firewall_rules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- firewall_rules
- sys.firewall_rules
ms.assetid: 140d2cd8-9aa1-4cc5-870d-e1dbc873b3fe
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 351ca530c49051b61b6f44b8dc2fbff83623cf8f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097991"
---
# <a name="sysfirewall_rules-azure-sql-database"></a>sys.firewall_rules (Database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce informazioni sulle impostazioni del firewall a livello di server associate al [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
 La vista `sys.firewall_rules` contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|id|**INT**|Identificatore dell'impostazione del firewall a livello di server.|  
|name|**NVARCHAR (128)**|Nome scelto per descrivere e distinguere l'impostazione del firewall a livello di server.|  
|start_ip_address|**VARCHAR (45)**|L'indirizzo IP più basso nell'intervallo dell'impostazione del firewall a livello di server. Gli indirizzi IP uguali o maggiori di questo possono tentare la connessione al server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L'indirizzo IP più basso possibile è `0.0.0.0`.|  
|end_ip_address|**VARCHAR (45)**|L'indirizzo IP più alto nell'intervallo dell'impostazione del firewall a livello di server. Gli indirizzi IP uguali o minori di questo possono tentare la connessione al server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L'indirizzo IP più alto possibile è `255.255.255.255`.<br /><br /> Nota: i tentativi di connessione di Azure sono consentiti quando sia questo campo che il campo **start_ip_address** è uguale a `0.0.0.0` .|  
|create_date|**DATETIME**|Data e ora UTC in cui è stata creata l'impostazione del firewall a livello di server.<br /><br /> Nota: UTC è un acronimo di Coordinated Universal Time.|  
|modify_date|**DATETIME**|Data e ora UTC in cui è stata modificata per l'ultima volta l'impostazione del firewall a livello di server.|  
  
## <a name="remarks"></a>Osservazioni

 Per restituire informazioni sulle impostazioni del firewall a livello di database associate alla database SQL di Microsoft Azure, usare [sys.database_firewall_rules &#40;&#41;del database SQL di Azure ](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md).  
  
## <a name="permissions"></a>Autorizzazioni

 L'accesso in sola lettura a questa vista è disponibile per tutti gli utenti con l'autorizzazione per la connessione al database **Master** .  
  
## <a name="see-also"></a>Vedere anche

[sp_set_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
[sp_delete_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database.md)   
[sp_set_database_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
[sp_delete_database_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)  
[sys.database_firewall_rules &#40;database SQL di Azure&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
[Configurare un Windows Firewall per l'accesso motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)     
[Configurare un firewall per l'accesso FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)  
[Configurare un firewall per l'accesso al server di report](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md) 
