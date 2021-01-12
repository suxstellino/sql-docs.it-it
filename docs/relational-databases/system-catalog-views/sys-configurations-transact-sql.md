---
description: sys.configurations (Transact-SQL)
title: sys.configurations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10956e1c3738c64829ec18d92a05b9f427b1833d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100681"
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ciascun valore di un'opzione di configurazione valida per l'intero server impostata nel sistema.  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|ID univoco del valore di configurazione.|  
|**nome**|**nvarchar(35)**|Nome dell'opzione di configurazione.|  
|**value**|**sql_variant**|Valore configurato per l'opzione.|  
|**minimum**|**sql_variant**|Valore minimo dell'opzione di configurazione.|  
|**maximum**|**sql_variant**|Valore massimo dell'opzione di configurazione.|  
|**value_in_use**|**sql_variant**|Valore corrente dell'opzione.|  
|**description**|**nvarchar(255)**|Descrizione dell'opzione di configurazione.|  
|**is_dynamic**|**bit**|1 = La variabile viene applicata quando viene eseguita l'istruzione RECONFIGURE.|  
|**is_advanced**|**bit**|1 = la variabile viene visualizzata solo quando è impostato il **advancedoption di visualizzazione** .|  
  
 ## <a name="remarks"></a>Osservazioni
  Per un elenco di tutte le opzioni di configurazione del server, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
> [!NOTE]  
>  Per le opzioni di configurazione a livello di database, vedere [ALTER database scoped configuration &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Per configurare Soft-NUMA, vedere [&#41;Soft-numa &#40;SQL Server ](../../database-engine/configure-windows/soft-numa-sql-server.md).  
 
La vista del catalogo sys.configurations può essere utilizzata per determinare l'config_value (colonna valore), il run_value (la colonna value_in_use) e se l'opzione di configurazione è dinamica (non richiede un riavvio del motore Server o la colonna is_dynamic).

> [!NOTE]
> Il config_value nel set di risultati di sp_configure è equivalente alla colonna **sys.configurations. Value** . Il **run_value** è equivalente alla colonna **sys.configurations.value_in_use** .

La query seguente può essere utilizzata per determinare se i valori configurati non sono stati installati:

```SQL
select * from sys.configurations where value != value_in_use
```

Se il valore è uguale alla modifica per l'opzione di configurazione eseguita, ma la **value_in_use** non è la stessa, il comando RECONFIGURE non è stato eseguito o ha avuto esito negativo oppure è necessario riavviare il motore Server.

Sono disponibili opzioni di configurazione in cui il valore e value_in_use potrebbero non essere uguali e si tratta di un comportamento previsto. Esempio:

"max server memory (MB)": il valore configurato predefinito 0 viene visualizzato come **value_in_use** = 2147483647<br>

"min server memory (MB)": il valore configurato predefinito 0 può essere visualizzato come **value_in_use** = 8 (a 32 bit) o 16 (a 64 bit). In alcuni casi, il **value_in_use** è 0. In questa situazione, il **value_in_use** "true" è 8 (a 32 bit) o 16 (a 64 bit).


È possibile utilizzare la colonna **is_dynamic** per determinare se l'opzione di configurazione richiede un riavvio. is_dynamic = 1 indica che, quando viene eseguito il Esegui di RECONFIGURE (T-SQL), il nuovo valore diverrà effettivo "immediatamente" (in alcuni casi, il motore del server potrebbe non valutare immediatamente il nuovo valore, ma eseguirlo nel corso normale della relativa esecuzione). is_dynamic = 0 significa che il valore di configurazione modificato non verrà applicato finché il server non viene riavviato anche se è stato eseguito il comando RECONFIGURE (T-SQL).

Per un'opzione di configurazione non dinamica non è possibile stabilire se è stato eseguito il comando RECONFIGURE (T-SQL) per eseguire il primo passaggio dell'installazione della modifica della configurazione. Prima di riavviare SQL Server per installare una modifica della configurazione, eseguire il comando RECONFIGURE (T-SQL) per assicurarsi che tutte le modifiche alla configurazione abbiano effetto dopo il riavvio di un SQL Server. 
 
 
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di configurazione a livello di server &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
