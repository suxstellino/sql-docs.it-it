---
description: sys.dm_audit_class_type_map (Transact-SQL)
title: sys.dm_audit_class_type_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_audit_class_type_map
- sys.dm_audit_class_type_map_TSQL
- dm_audit_class_type_map
- dm_audit_class_type_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_audit_class_type_map dynamic management view
ms.assetid: e10b5431-1bb0-47ca-8fd0-c04bd73a4410
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10d3cba8aef1ec2720d195ed52a5ee677d07ec3a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202432"
---
# <a name="sysdm_audit_class_type_map-transact-sql"></a>sys.dm_audit_class_type_map (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Restituisce una tabella che esegue il mapping del campo class_type nel log di controllo al campo class_desc in sys.dm_audit_actions. Per ulteriori informazioni sul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controllo, vedere [SQL Server audit &#40;motore di database&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**class_type**|**char(2)**|Tipo di classe dell'entità controllata. Esegue il mapping all'class_type scritta nel log di controllo e restituita dalla funzione **get_audit_file ()** . Non ammette i valori Null.|  
|**class_type_desc**|**nvarchar(120)**|Nome dell'entità controllabile. Non ammette i valori Null.|  
|**securable_class_desc**|**nvarchar(120)**|Oggetto a protezione diretta che esegue il mapping al campo class_type controllato. Il valore è Null se class_type non esegue il mapping a un oggetto a protezione diretta. Può essere correlato a class_desc in sys.dm_audit_actions.|  
  
## <a name="permissions"></a>Autorizzazioni  
Questa vista è visibile al pubblico.  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_class_type_map](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Creazione di un controllo del server e di una specifica del controllo del server](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
