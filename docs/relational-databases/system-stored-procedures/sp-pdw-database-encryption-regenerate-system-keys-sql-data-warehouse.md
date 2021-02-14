---
title: sp_pdw_database_encryption_regenerate_system_keys (Azure sinapsi Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: bb13e323-a984-4462-8b6d-6019c38ddd9d
author: ronortloff
ms.author: rortloff
ms.reviewer: ''
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 3d7a4efb9a629938b6cfc7d48c990351bc2a5cbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271778"
---
# <a name="sp_pdw_database_encryption_regenerate_system_keys-azure-synapse-analytics"></a>sp_pdw_database_encryption_regenerate_system_keys (analisi delle sinapsi di Azure)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Usare **sp_pdw_database_encryption_regenerate_system_keys** per ruotare il certificato e la chiave di crittografia del database per i database interni crittografati quando è abilitata la funzionalità Transparent Data Encryption nell'appliance. incluso `tempdb`. Questa operazione avrà esito positivo solo se si abilita Transparent Data Encryption.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption_regenerate_system_keys  ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 La routine non dispone di parametri.  
  
 Questa procedura deve essere usata quando il traffico nell'appliance è basso.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **sysadmin** o all'autorizzazione **Control Server** .  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente vengono rigenerate le chiavi di crittografia del database.  
  
```sql  
EXEC sys.sp_pdw_database_encryption_regenerate_system_keys;  
```  
  
## <a name="see-also"></a>Vedi anche  
 [sp_pdw_database_encryption &#40;Azure sinapsi Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40;Azure sinapsi Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
