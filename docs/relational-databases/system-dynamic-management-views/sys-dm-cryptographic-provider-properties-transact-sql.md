---
description: sys.dm_cryptographic_provider_properties (Transact-SQL)
title: sys.dm_cryptographic_provider_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_cryptographic_provider_properties_TSQL
- sys.dm_cryptographic_provider_properties
- sys.dm_cryptographic_provider_properties_TSQL
- dm_cryptographic_provider_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_properties dynamic management view
ms.assetid: 024b0095-6766-4189-a39a-d316c5ec2874
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 419793e94cc85a8dba1e29f4b79694820518eaad
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384667"
---
# <a name="sysdm_cryptographic_provider_properties-transact-sql"></a>sys.dm_cryptographic_provider_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui provider di crittografia registrati.  
  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|provider_id|**int**|Numero di identificazione del provider di crittografia.|  
|guid|**uniqueidentifier**|GUID univoco del provider.|  
|provider_version|**nvarchar(256)**|Versione del provider nel formato ' *AA.BB.cccc.dd* '.|  
|sqlcrypt_version|**nvarchar(256)**|Versione principale dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] API di crittografia nel formato ' *AA.BB.cccc.dd* '.|  
|friendly_name|**nvarchar(2048)**|Nome fornito dal provider.|  
|authentication_type|**nvarchar(256)**|WINDOWS, BASIC o OTHER.|  
|symmetric_key_support|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|symmetric_key_export|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|symmetric_key_import|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|symmetric_key_persistance|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|asymmetric_key_support|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|asymmetric_key_export|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|symmetric_key_import|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
|symmetric_key_persistance|**tinyint**|0 (non supportato)<br /><br /> 1 (supportato)|  
  
## <a name="permissions"></a>Autorizzazioni  
 La visualizzazione **sys.dm_cryptographic_provider_properties** è visibile al pubblico.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [Funzioni e viste a gestione dinamica relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
