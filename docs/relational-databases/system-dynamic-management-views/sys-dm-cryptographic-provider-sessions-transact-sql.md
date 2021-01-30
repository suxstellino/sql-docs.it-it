---
description: sys.dm_cryptographic_provider_sessions (Transact-SQL)
title: sys.dm_cryptographic_provider_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_cryptographic_provider_sessions
- dm_cryptographic_provider_sessions_TSQL
- sys.dm_cryptographic_provider_sessions_TSQL
- dm_cryptographic_provider_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_sessions dynamic management function
ms.assetid: 9a4de02b-1a07-4850-979a-0861fddb7f9d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8af82a9be0181aa5668ee59dc3c82e20233023d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195116"
---
# <a name="sysdm_cryptographic_provider_sessions-transact-sql"></a>sys.dm_cryptographic_provider_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulle sessioni aperte per un provider di crittografia.  
 
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_cryptographic_provider_sessions(session_identifier)  
```  
  
## <a name="arguments"></a>Argomenti  
 *session_identifier*  
 Un numero intero che indica le sessioni da restituire.  
  
 0 = solo connessione corrente  
  
 1 = tutte le connessioni di crittografia  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**provider_id**|**int**|Numero di identificazione del provider di crittografia.|  
|**session_handle**|**varbytes (8)**|Handle della sessione di crittografia.|  
|**identity**|**nvarchar(128)**|Identità utilizzata per l'autenticazione con il provider di crittografia.|  
|**spid**|**short**|ID sessione (SPID) della connessione. Per altre informazioni, vedere [@@SPID &#40;Transact-SQL&#41;](../../t-sql/functions/spid-transact-sql.md).|  
  
## <a name="permissions"></a>Autorizzazioni  
 I membri del ruolo del server public possono utilizzare **sys.dm_cryptographic_provider_sessions** per restituire informazioni sulla connessione corrente. Per visualizzare tutte le connessioni crittografiche, è necessaria l'autorizzazione **Control** server.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
