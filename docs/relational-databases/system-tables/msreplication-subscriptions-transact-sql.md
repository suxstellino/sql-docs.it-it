---
description: MSreplication_subscriptions (Transact-SQL)
title: MSreplication_subscriptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_subscriptions
- MSreplication_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_subscriptions system table
ms.assetid: fd0c5843-4e9b-4448-8bfb-0a4067d1d8d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8d5d0b8b450102483c844ec53472ff509b75cd0d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092380"
---
# <a name="msreplication_subscriptions-transact-sql"></a>MSreplication_subscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSreplication_subscriptions** contiene una riga di informazioni di replica per ogni agente di distribuzione di manutenzione del database Sottoscrittore locale. Questa tabella è archiviata nel database di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**independent_agent**|**bit**|Indica se per questa pubblicazione è disponibile un agente di distribuzione autonomo.|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> 0 = push<br /><br /> 1 = pull<br /><br /> 2 = anonima|  
|**distribution_agent**|**sysname**|Nome dell'agente di distribuzione.|  
|**Time**|**smalldatetime**|Ora dell'ultimo aggiornamento eseguito dall'agente di distribuzione.|  
|**description**|**nvarchar(255)**|Descrizione della sottoscrizione.|  
|**transaction_timestamp**|**varbinary(16)**|Solo per uso interno.|  
|**update_mode**|**tinyint**|Tipo di aggiornamento.|  
|**agent_id**|**binary(16)**|ID dell'agente.|  
|**subscription_guid**|**binary(16)**|Identificatore globale della versione della sottoscrizione associata alla pubblicazione.|  
|**subid**|**binary(16)**|Identificatore globale di una sottoscrizione anonima.|  
|**immediate_sync**|**bit**|Indica se i file di sincronizzazione vengono creati o aggiornati a ogni esecuzione dell'agente snapshot.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
