---
description: sys.tcp_endpoints (Transact-SQL)
title: sys.tcp_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.tcp_endpoints
- sys.tcp_endpoints_TSQL
- tcp_endpoints
- tcp_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tcp_endpoints catalog view
ms.assetid: 43cc3afa-cced-4463-8e97-fbfdaf2e4fa8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5717389606bb3b73dc95c6a7eaedd6e03312cfd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094426"
---
# <a name="systcp_endpoints-transact-sql"></a>sys.tcp_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni endpoint TCP nel sistema. Gli endpoint descritti da **sys.tcp_endpoints** forniscono un oggetto per concedere e revocare il privilegio di connessione. Le informazioni visualizzate relative alle porte e agli indirizzi IP non vengono utilizzate per configurare i protocolli e potrebbero non corrispondere alla configurazione effettiva del protocollo. Per visualizzare e configurare i protocolli, utilizzare Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**< colonne ereditate>**||Eredita le colonne da [sys. Endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**port**|INT|Numero della porta su cui è in attesa l'endpoint. Non ammette i valori Null.|  
|**is_dynamic_port**|bit|1 = Il numero della porta è stato assegnato dinamicamente.<br /><br /> Non ammette i valori Null.|  
|**ip_address**|**nvarchar (45)**|Indirizzo IP del listener specificato dalla clausola LISTENER_IP. Ammette i valori Null.|  
  
## <a name="remarks"></a>Osservazioni  
 Eseguire la query seguente per raccogliere informazioni sugli endpoint e sulle connessioni. Gli endpoint senza connessioni correnti o TCP verranno visualizzati con valori NULL. Aggiungere la  clausola WHERE `WHERE des.session_id = @@SPID` per restituire informazioni sulla connessione corrente.  
  
```  
SELECT des.login_name, des.host_name, program_name,  dec.net_transport, des.login_time,   
e.name AS endpoint_name, e.protocol_desc, e.state_desc, e.is_admin_endpoint,   
t.port, t.is_dynamic_port, dec.local_net_address, dec.local_tcp_port   
FROM sys.endpoints AS e  
LEFT JOIN sys.tcp_endpoints AS t  
   ON e.endpoint_id = t.endpoint_id  
LEFT JOIN sys.dm_exec_sessions AS des  
   ON e.endpoint_id = des.endpoint_id  
LEFT JOIN sys.dm_exec_connections AS dec  
   ON des.session_id = dec.session_id;  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo degli endpoint &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)  
  
  
