---
description: sys.database_principals (Transact-SQL)
title: sys.database_principals (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_principals
- database_principals_TSQL
- sys.database_principals
- sys.database_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_principals catalog view
ms.assetid: 8cb239e9-eb8c-4109-9cec-0d35de95fa0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43fb4dff1730aa0d8e19d411838f76b965fbca01
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171993"
---
# <a name="sysdatabase_principals-transact-sql"></a>sys.database_principals (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Viene restituita una riga per ogni entità di sicurezza in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome dell'entità, univoco all'interno del database.|  
|**principal_id**|**int**|ID dell'entità, univoco all'interno del database.|  
|**type**|**char(1)**|Tipo di entità:<br /><br /> A = Ruolo applicazione<br /><br /> C = Utente sul quale è stato eseguito il mapping a un certificato<br /><br /> E = utente esterno da Azure Active Directory<br /><br /> G = Gruppo di Windows<br /><br /> K = Utente sul quale è stato eseguito il mapping a una chiave asimmetrica<br /><br /> R = Ruolo del database<br /><br /> S = Utente SQL<br /><br /> U = Utente di Windows<br /><br /> X = gruppo esterno da Azure Active Directory gruppo o applicazioni|  
|**type_desc**|**nvarchar(60)**|Descrizione del tipo dell'entità.<br /><br /> APPLICATION_ROLE<br /><br /> CERTIFICATE_MAPPED_USER<br /><br /> EXTERNAL_USER<br /><br /> WINDOWS_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_USER<br /><br /> DATABASE_ROLE<br /><br /> SQL_USER<br /><br /> WINDOWS_USER<br /><br /> EXTERNAL_GROUPS|  
|**default_schema_name**|**sysname**|Nome da utilizzare quando il nome SQL non specifica uno schema. Restituisce Null per entità non di tipo S, U o A.|  
|**create_date**|**datetime**|Ora di creazione dell'entità.|  
|**modify_date**|**datetime**|Ora dell'ultima modifica dell'entità.|  
|**owning_principal_id**|**int**|ID dell'entità proprietaria dell'entità corrente. Per impostazione predefinita, tutti i ruoli predefiniti del database sono di proprietà di **dbo** .|  
|**SID**|**varbinary(85)**|ID di sicurezza (SID) dell'entità.  NULL per SYS e INFORMATION SCHEMAS.|  
|**is_fixed_role**|**bit**|Se è 1, questa riga rappresenta una voce per uno dei ruoli predefiniti del database, ovvero db_owner, db_accessadmin, db_datareader, db_datawriter, db_ddladmin, db_securityadmin, db_backupoperator, db_denydatareader, db_denydatawriter.|  
|**authentication_type**|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Indica il tipo di autenticazione. Di seguito sono riportati i valori possibili e le relative descrizioni.<br /><br /> 0: nessuna autenticazione<br />1: autenticazione dell'istanza<br />2: autenticazione del database<br />3: autenticazione di Windows<br />4: autenticazione Azure Active Directory|  
|**authentication_type_desc**|**nvarchar(60)**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Descrizione del tipo di autenticazione. Di seguito sono riportati i valori possibili e le relative descrizioni.<br /><br /> NONE: nessuna autenticazione<br />ISTANZA: autenticazione dell'istanza<br />DATABASE: autenticazione del database<br />WINDOWS: autenticazione di Windows<br />ESTERNO: autenticazione Azure Active Directory|  
|**default_language_name**|**sysname**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Indica la lingua predefinita per questa entità.|  
|**default_language_lcid**|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Indica l'identificatore LCID predefinito per questa entità.|  
|**allow_encrypted_value_modifications**|**bit**|**Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Elimina i controlli sui metadati di crittografia nel server nelle operazioni di copia bulk. Ciò consente all'utente di eseguire la copia bulk dei dati crittografati tramite Always Encrypted, tra tabelle o database, senza decrittografare i dati. Il valore predefinito è OFF. |      
  
## <a name="remarks"></a>Commenti  
 Le proprietà di *PasswordLastSetTime* sono disponibili in tutte le configurazioni supportate di SQL Server, ma le altre proprietà sono disponibili solo quando SQL Server è in esecuzione in Windows Server 2003 o versioni successive e sono abilitati sia CHECK_POLICY che CHECK_EXPIRATION. Per ulteriori informazioni, vedere [criteri password](../../relational-databases/security/password-policy.md) .
I valori del principal_id possono essere riutilizzati nel caso in cui le entità siano state eliminate e pertanto non è sempre in aumento.
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi utente può visualizzare il proprio nome utente, gli utenti di sistema e i ruoli predefiniti del database. Per visualizzare altri utenti, è richiesta l'autorizzazione ALTER ANY USER o un'autorizzazione dell'utente. Per visualizzare i ruoli definiti dall'utente, è richiesta l'autorizzazione ALTER ANY ROLE o l'appartenenza al ruolo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A: elenco di tutte le autorizzazioni delle entità di database  
 Nella query seguente vengono elencate le autorizzazioni concesse o negate in modo esplicito alle entità di database.  
  
> [!IMPORTANT]  
>  Le autorizzazioni dei ruoli predefiniti del database non sono incluse in sys.database_permissions. Pertanto, le entità di database potrebbero contenere ulteriori autorizzazioni non presenti in questo elenco.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>B: elenco di autorizzazioni per gli oggetti dello schema all'interno di un database  
 Nella query seguente viene creato un join di sys.database_principals e sys.database_permissions con sys.objects e sys.schemas per elencare le autorizzazioni concesse o negate agli oggetti di uno schema specifico.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-listing-all-the-permissions-of-database-principals"></a>C: elenco di tutte le autorizzazioni delle entità di database  
 Nella query seguente vengono elencate le autorizzazioni concesse o negate in modo esplicito alle entità di database.  
  
> [!IMPORTANT]  
>  Le autorizzazioni dei ruoli predefiniti del database non vengono visualizzate in `sys.database_permissions` . Pertanto, le entità di database potrebbero contenere ulteriori autorizzazioni non presenti in questo elenco.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="d-listing-permissions-on-schema-objects-within-a-database"></a>D: elenco delle autorizzazioni per gli oggetti dello schema all'interno di un database  
 La query seguente unisce `sys.database_principals` e `sys.database_permissions` a `sys.objects` e `sys.schemas` per elencare le autorizzazioni concesse o negate a specifici oggetti dello schema.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md)   
 [Connettersi al Database SQL utilizzando l’autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)  
  
