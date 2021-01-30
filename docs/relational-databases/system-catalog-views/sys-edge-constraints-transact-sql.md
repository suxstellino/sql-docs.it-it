---
description: sys.edge_constraints (Transact-SQL)
title: sys.edge_constraints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.edge_constraints
- edge_constraints
- SQL Graph
- edge_constraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.edge_constraints catalog view
ms.assetid: 0f782d2f-7126-46ab-85b7-bcba44862231
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b7aa12c8f062244e206985db4c2283011a08686
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203912"
---
# <a name="sysedge_constraints-transact-sql"></a>sys.edge_constraints (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Contiene una riga per ogni oggetto che rappresenta un vincolo Edge. 
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Per un elenco di colonne ereditate da questa vista, vedere [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_disabled**|**bit**|1 = il vincolo Edge viene disattivato.<br /><br /> 0 = il vincolo Edge è abilitato.|  
|**is_not_trusted**|**bit**|1 = il vincolo Edge non è stato verificato dal sistema.<br /><br /> 0 = il vincolo Edge è stato verificato dal sistema.|  
|**delete_referential_action**|**tinyint**|Azione referenziale definita in questo vincolo Edge.<br /><br />0 = nessuna azione.|  
|**delete_referential_action_desc**|**nvarchar(60)**|Descrizione dell'azione referenziale definita in questo vincolo Edge.<br /><br />NO_ACTION|  
|**is_system_named**|**bit**|1 = il nome del vincolo Edge è stato generato dal sistema.<br /><br />0 = il nome del vincolo Edge è stato fornito dall'utente.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
