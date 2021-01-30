---
description: sys.security_policies (Transact-SQL)
title: sys.security_policies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_POLICIES_TSQL
- SECURITY_POLICIES_TSQL
- SYS.SECURITY_POLICIES
- SECURITY_POLICIES
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_policies catalog view
- security_policies catalog view
ms.assetid: 35362f5b-e601-4049-9e1d-c5307e823831
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c33e75e803b7b5d9972d373f4cc5738601197860
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182497"
---
# <a name="syssecurity_policies-transact-sql"></a>sys.security_policies (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Restituisce una riga per ogni criterio di sicurezza nel database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nome del criterio di sicurezza, univoco all'interno del database.|  
|object_id|**int**|ID del criterio di sicurezza.|  
|principal_id|**int**|ID del proprietario del criterio di sicurezza registrato nel database. NULL se il proprietario viene determinato tramite lo schema.|  
|schema_id|**int**|ID dello schema in cui si trova l'oggetto.|  
|parent_object_id|**int**|ID dell'oggetto a cui appartiene il criterio. Deve essere 0.|  
|tipo|**vachar (2)**|Deve essere **SP**.|  
|type_desc|**nvarchar(60)**|**SECURITY_POLICY**.|  
|create_date|**datetime**|Data UTC di creazione del criterio di sicurezza.|  
|modify_date|**datetime**|Data UTC dell'ultima modifica del criterio di sicurezza.|  
|is_ms_shipped|**bit**|Sempre false.|  
|is_enabled|**bit**|Stato della specifica del criterio di sicurezza:<br /><br /> 0 = disabilitato<br /><br /> 1 = abilitato|  
|is_not_for_replication|**bit**|Il criterio è stato creato con l'opzione NOT FOR REPLICATION.|  
|uses_database_collation|**bit**|Usa le stesse regole di confronto del database.|  
|is_schemabinding_enabled|**bit**|Stato schema per i criteri di sicurezza:<br /><br /> 0 o NULL = abilitato<br /><br /> 1 = disabilitato|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le entità con l'autorizzazione **ALTER ANY Security Policy** possono accedere a tutti gli oggetti in questa vista del catalogo, nonché a qualsiasi utente con la **definizione della vista** sull'oggetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md)   
 [sys.security_predicates &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
