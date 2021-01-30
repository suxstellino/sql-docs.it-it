---
description: sys.server_principals (Transact-SQL)
title: sys.server_principals (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_principals
- sys.server_principals_TSQL
- sys.server_principals
- server_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_principals catalog view
ms.assetid: c5dbe0d8-a1c8-4dc4-b9b1-22af20effd37
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d2059bf6c20da387bab7f0ea0e07ab2ad7ced79c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204855"
---
# <a name="sysserver_principals-transact-sql"></a>sys.server_principals (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Contiene una riga per ogni entità a livello di server.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome dell'entità. Univoco all'interno di un server.|  
|**principal_id**|**int**|ID dell'entità. Univoco all'interno di un server.|  
|**SID**|**varbinary(85)**|ID di sicurezza (SID) dell'entità. Per le entità di Windows, corrisponde al SID di Windows.|  
|**type**|**char(1)**|Tipo di entità:<br /><br /> S = Account di accesso di SQL<br /><br /> U = Account di accesso di Windows<br /><br /> G = Gruppo di Windows<br /><br /> R = Ruolo del server<br /><br /> C = Account di accesso sul quale è stato eseguito il mapping a un certificato<br /><br /> E = accesso esterno da Azure Active Directory<br /><br /> X = gruppo esterno da Azure Active Directory gruppo o applicazioni<br /><br /> K = Account di accesso sul quale è stato eseguito il mapping a una chiave asimmetrica|  
|**type_desc**|**nvarchar(60)**|Descrizione del tipo di entità:<br /><br /> SQL_LOGIN<br /><br /> WINDOWS_LOGIN<br /><br /> WINDOWS_GROUP<br /><br /> SERVER_ROLE<br /><br /> CERTIFICATE_MAPPED_LOGIN<br /><br /> EXTERNAL_LOGIN<br /><br /> EXTERNAL_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_LOGIN|  
|**is_disabled**|**int**|1 = L'account di accesso è disabilitato.|  
|**create_date**|**datetime**|Ora di creazione dell'entità.|  
|**modify_date**|**datetime**|Ora dell'ultima modifica della definizione dell'entità.|  
|**default_database_name**|**sysname**|Database principale per l'entità.|  
|**default_language_name**|**sysname**|Lingua predefinita per l'entità.|  
|**credential_id**|**int**|ID di una credenziale associata all'entità. Se all'entità non è associata alcuna credenziale, credential_id è NULL.|  
|**owning_principal_id**|**int**|**Principal_id** del proprietario di un ruolo del server. NULL se l'entità non è un ruolo del server.|  
|**is_fixed_role**|**bit**|Restituisce 1 se l'entità è uno dei ruoli predefiniti del server con autorizzazioni fisse. Per altre informazioni, vedere [Ruoli a livello di server](../../relational-databases/security/authentication-access/server-level-roles.md).|  
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi account di accesso consente di visualizzare il proprio nome dell'account di accesso, gli account di accesso di sistema e i ruoli predefiniti del server. Per visualizzare altri account di accesso, è richiesta l'autorizzazione ALTER ANY LOGIN o un'autorizzazione per l'account di accesso. Per visualizzare i ruoli del server definiti dall'utente, è richiesta l'autorizzazione ALTER ANY SERVER ROLE o l'appartenenza al ruolo.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempio  
 Nella query seguente vengono elencate le autorizzazioni concesse o negate in modo esplicito alle entità del server.  
  
> [!IMPORTANT]  
>  Le autorizzazioni dei ruoli predefiniti del server (diverse da Public) non vengono visualizzate in sys.server_permissions. Pertanto, le entità del server potrebbero contenere ulteriori autorizzazioni non presenti in questo elenco.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pe.state_desc, pe.permission_name   
FROM sys.server_principals AS pr   
JOIN sys.server_permissions AS pe   
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Gerarchia delle autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
  
  
