---
description: sys.dm_tcp_listener_states (Transact-SQL)
title: sys.dm_tcp_listener_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tcp_listener_states
- dm_tcp_listener_states
- sys.dm_tcp_listener_states_TSQL
- dm_tcp_listener_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.dm_tcp_listener_states dynamic management view
ms.assetid: 9997ffed-a4c1-428f-8bac-3b9e4b16d7cf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d3b155d84f499c4dd2ba8ed9333932364c7957ea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093961"
---
# <a name="sysdm_tcp_listener_states-transact-sql"></a>sys.dm_tcp_listener_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga contenente informazioni sullo stato dinamico per ogni listener TCP.  
  
> [!NOTE]
> Il listener del gruppo di disponibilità può rimanere in attesa sulla stessa porta del listener dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questo caso, i listener vengono elencati separatamente, proprio come un listener di Service Broker.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**listener_id**|**int**|ID interno del listener. Non ammette i valori Null.<br /><br /> Chiave primaria.|  
|**ip_address**|**nvarchar (48)**|L'indirizzo IP del listener online e attualmente in attesa. È consentito IPv4 o IPv6. Se un listener dispone di entrambi gli indirizzi, vengono elencati separatamente. Un carattere jolly IPv4 viene visualizzato come "0.0.0.0". Un carattere jolly IPv6 viene visualizzato come "::".<br /><br /> Non ammette i valori Null.|  
|**is_ipv4**|**bit**|Tipo di indirizzo IP<br /><br /> 1 = IPv4<br /><br /> 0 = IPv6|  
|**port**|**int**|Numero della porta su cui il listener rimane in attesa. Non ammette i valori Null.|  
|**type**|**tinyint**|Tipo di listener, uno di:<br /><br /> 0 = [!INCLUDE[tsql](../../includes/tsql-md.md)]<br /><br /> 1 = Service Broker<br /><br /> 2 = Mirroring del database<br /><br /> Non ammette i valori Null.|  
|**type_desc**|**nvarchar (20)**|Descrizione del **tipo**, uno di:<br /><br /> TSQL<br /><br /> SERVICE_BROKER<br /><br /> DATABASE_MIRRORING<br /><br /> Non ammette i valori Null.|  
|**state**|**tinyint**|Stato del listener del gruppo di disponibilità, uno di:<br /><br /> 1 = Online. Il listener è in attesa ed elabora le richieste.<br /><br /> 2 = Riavvio in sospeso. Il listener è offline, con il riavvio in sospeso.<br /><br /> Se il listener del gruppo di disponibilità è in attesa sulla stessa porta dell'istanza del server, questi due listener si trovano sempre nello stesso stato.<br /><br /> Non ammette i valori Null.<br /><br /> Nota: i valori di questa colonna provengono dall'oggetto TSD_listener. La colonna non supporta uno stato offline perché quando TDS_listener è offline, non è possibile eseguire query sullo stato.|  
|**state_desc**|**nvarchar (16)**|Descrizione dello **stato**, uno di:<br /><br /> ONLINE<br /><br /> PENDING_RESTART<br /><br /> Non ammette i valori Null.|  
|**start_time**|**datetime**|Timestamp che indica l'avvio del listener. Non ammette i valori Null.|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Viste del catalogo dei gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Funzioni e DMV di Gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  
