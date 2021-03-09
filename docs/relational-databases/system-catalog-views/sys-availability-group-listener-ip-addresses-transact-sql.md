---
description: sys.availability_group_listener_ip_addresses (Transact-SQL)
title: sys.availability_group_listener_ip_addresses (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_group_listener_ip_addresses
- sys.availability_group_listener_ip_addresses
- availability_group_listener_ip_addresses_TSQL
- sys.availability_group_listener_ip_addresses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.availability_group_listener_ip_addresses catalog view
ms.assetid: e515fa6b-1354-4110-9b70-ab2e6164c992
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8c2f8d3db7be521b4c6350d3274b62e38126e851
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465068"
---
# <a name="sysavailability_group_listener_ip_addresses-transact-sql"></a>sys.availability_group_listener_ip_addresses (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni indirizzo IP associato a un listener del gruppo di disponibilità Always On nel cluster WSFC (Windows Server Failover Clustering).  
  
 Chiave primaria: **listener_id**  +  **ip_address**  +  **ip_sub_mask**  
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**listener_id**|**nvarchar (36)**|GUID della risorsa dal cluster WSFC (Windows Server Failover Clustering).|  
|**ip_address**|**nvarchar (48)**|Indirizzo IP virtuale configurato del listener del gruppo di disponibilità. Restituisce un indirizzo IPv4 o IPv6.|  
|**ip_subnet_mask**|**nvarchar (15)**|Subnet mask IP configurata per l'indirizzo IPv4, se presente, configurato per il listener del gruppo di disponibilità.<br /><br /> NULL = Subnet IPv6|  
|**is_dhcp**|**bit**|Se l'indirizzo IP è configurato tramite DHCP, uno di:<br /><br /> 0 = Indirizzo IP non configurato tramite DHCP.<br /><br /> 1 = Indirizzo IP configurato tramite DHCP|  
|**network_subnet_ip**|**nvarchar (48)**|Indirizzo IP della subnet di rete che specifica la subnet a cui appartiene l'indirizzo IP.|  
|**network_subnet_prefix_length**|**int**|Lunghezza del prefisso della subnet di rete a cui appartiene l'indirizzo IP.|  
|**network_subnet_ipv4_mask**|**nvarchar (45)**|Subnet mask di rete a cui appartiene l'indirizzo IP. **network_subnet_ipv4_mask** per specificare le opzioni dhcp <network_subnet_option> in una clausola with DHCP dell'istruzione [create Availability Group](../../t-sql/statements/create-availability-group-transact-sql.md) o [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .<br /><br /> NULL = Subnet IPv6|  
|**state**|**tinyint**|Stato ONLINE/OFFLINE della risorsa IP dal cluster WSFC, uno di:<br /><br /> 1 = Online. La risorsa IP è online.<br /><br /> 0 = Offline. La risorsa IP è offline.<br /><br /> 2 = Online in sospeso. La risorsa IP è offline ma verrà presto portata online.<br /><br /> 3 = Non completato. Il tentativo di portare online la risorsa IP non è riuscito.|  
|**state_desc**|**nvarchar(60)**|Descrizione dello **stato**, uno di:<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> ONLINE_PENDING<br /><br /> FAILED|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
