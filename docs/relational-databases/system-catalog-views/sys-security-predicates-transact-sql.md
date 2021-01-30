---
description: sys.security_predicates (Transact-SQL)
title: sys.security_predicates (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_PREDICATES
- SECURITY_PREDICATES
- SECURITY_PREDICATES_TSQL
- SYS.SECURITY_PREDICATES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_predicates catalog view
- security_predicates catalog view
ms.assetid: c7a2f28c-98da-463d-8b8a-8e5619e2c6a6
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8220ca6cefc82682201e3b06244cbed5a7f27db1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182493"
---
# <a name="syssecurity_predicates-transact-sql"></a>sys.security_predicates (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Restituisce una riga per ogni predicato di sicurezza nel database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID del criterio di sicurezza che contiene il predicato.|  
|security_predicate_id|**int**|ID predicato all'interno del criterio di sicurezza.|  
|target_object_id|**int**|ID dell'oggetto a cui è associato il predicato di sicurezza.|  
|predicate_definition|**nvarchar(max)**|Nome completo della funzione che verrà usata come predicato di sicurezza, inclusi gli argomenti. Si noti che il nome `schema.function` può essere normalizzato (vale a dire preceduto da un carattere di escape) come qualsiasi altro elemento nel testo per la coerenza. Ad esempio:<br /><br /> `[dbo].[fn_securitypredicate]([wing], [startTime], [endTime])`|  
|predicate_type|**int**|Tipo di predicato utilizzato dai criteri di sicurezza:<br /><br /> 0 = PREDICATO DEL FILTRO<br /><br /> 1 = PREDICATO DI BLOCCO|  
|predicate_type_desc|**nvarchar(60)**|Tipo di predicato utilizzato dai criteri di sicurezza:<br /><br /> FILTER<br /><br /> BLOCCA|  
|operation|**int**|Tipo di operazione specificata per il predicato:<br /><br /> NULL = tutte le operazioni applicabili<br /><br /> 1 = DOPO L'INSERIMENTO<br /><br /> 2 = DOPO L'AGGIORNAMENTO<br /><br /> 3 = PRIMA DELL'AGGIORNAMENTO<br /><br /> 4 = PRIMA DELL'ELIMINAZIONE|  
|operation_desc|**nvarchar(60)**|Tipo di operazione specificata per il predicato:<br /><br /> NULL<br /><br /> DOPO L'INSERIMENTO<br /><br /> AFTER UPDATE<br /><br /> PRIMA DELL'AGGIORNAMENTO<br /><br /> PRIMA DELL'ELIMINAZIONE|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le entità con l'autorizzazione **ALTER ANY Security Policy** possono accedere a tutti gli oggetti in questa vista del catalogo, nonché a qualsiasi utente con la **definizione della vista** sull'oggetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
