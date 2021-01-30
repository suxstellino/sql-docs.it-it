---
description: sp_pdw_log_user_data_masking (analisi delle sinapsi di Azure)
title: sp_pdw_log_user_data_masking (Azure sinapsi Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 43c63b42-03cb-4fb5-8362-ec3b7e22a590
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 8c02aa65df689ab147d3c7312d8c1e84a513aa88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196065"
---
# <a name="sp_pdw_log_user_data_masking-azure-synapse-analytics"></a>sp_pdw_log_user_data_masking (analisi delle sinapsi di Azure)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Usare **sp_pdw_log_user_data_masking** per abilitare la maschera dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività. La maschera dati utente influiscono sulle istruzioni in tutti i database nel dispositivo.  
  
> [!IMPORTANT]  
>  I [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività interessati dal **sp_pdw_log_user_data_masking** sono determinati [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività. **sp_pdw_log_user_data_masking** non influisce sui log delle transazioni del database o sui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] log degli errori.  
  
 **Sfondo:** Nei log attività di configurazione predefiniti [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] contengono [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni complete e in alcuni casi possono includere dati utente contenuti in operazioni quali le istruzioni **Insert**, **Update** e **Select** . In caso di problemi nell'appliance, questo consente di analizzare le condizioni che hanno causato il problema senza dover riprodurre il problema. Per evitare che i dati utente vengano scritti nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività, i clienti possono scegliere di attivare la maschera dati utente utilizzando questo stored procedure. Le istruzioni verranno comunque scritte [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nei log attività, ma tutti i valori letterali nelle istruzioni che potrebbero contenere dati utente verranno mascherati, sostituiti con alcuni valori costanti predefiniti.  
  
 Quando Transparent Data Encryption è abilitato nell'appliance, la maschera dei dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività viene attivata automaticamente.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_log_user_data_masking [ [ @masking_mode = ] value ] ;  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

#### <a name="parameters"></a>Parametri  
`[ @masking_mode = ] masking_mode` Determina se la maschera dati utente di log Transparent Data Encryption è abilitata. *masking_mode* è di **tipo int**. i possibili valori sono i seguenti:  
  
-   0 = disabilitato, i dati utente vengono visualizzati nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività.  
  
-   1 = abilitato, le istruzioni dati utente vengono visualizzate nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività, ma i dati utente sono mascherati.  
  
-   2 = le istruzioni che contengono dati utente non vengono scritte nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività.  
  
 L'esecuzione di **sp_pdw_ log_user_data_masking** senza parametri restituisce lo stato corrente della maschera dati utente del log di Transparent Data Mask nell'appliance come set di risultati scalare.  
  
## <a name="remarks"></a>Commenti  
 La maschera dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività consente la sostituzione di valori letterali con valori costanti predefiniti nelle istruzioni **SELECT** e DML, perché possono contenere dati utente. L'impostazione di *masking_mode* su 1 non maschera i metadati, ad esempio nomi di colonna o di tabella. Impostando *masking_mode* su 2 vengono rimosse le istruzioni con metadati, ad esempio nomi di colonna o di tabella.  
  
 La maschera dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività viene implementata nel modo seguente:  
  
-   I dati Transparent Data Encryption e utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività sono disattivati per impostazione predefinita. Le istruzioni non verranno automaticamente mascherate se la crittografia del database non è abilitata nel dispositivo.  
  
-   L'abilitazione di Transparent Data Encryption nell'appliance attiva automaticamente la maschera dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività.  
  
-   La disabilitazione di Transparent Data Encryption non influisce sulla maschera dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività.  
  
-   È possibile abilitare in modo esplicito la maschera dati utente nei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] log attività usando la procedura **sp_pdw_log_user_data_masking** .  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **sysadmin** o all'autorizzazione **Control Server** .  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene abilitata la maschera dati utente del log di Transparent nell'appliance.  
  
```sql  
EXEC sp_pdw_log_user_data_masking 1;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_pdw_database_encryption &#40;Azure sinapsi Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_database_encryption_regenerate_system_keys &#40;Azure sinapsi Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
  
  
