---
description: sys.routes (Transact-SQL)
title: sys. Routes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- routes
- routes_TSQL
- sys.routes
- sys.routes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.routes catalog view
ms.assetid: 8fc65915-8bd6-425b-95d9-6a8468cb1e48
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ad4662aa66798e18aa3ebb1eede49333ba4a08f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97411820"
---
# <a name="sysroutes-transact-sql"></a>sys.routes (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Questa vista del catalogo contiene una riga per route. Le route vengono utilizzate da Service Broker per individuare l'indirizzo di rete per un servizio.   

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome della route, univoco all'interno del database. Non ammette i valori Null.|  
|**route_id**|**int**|Identificatore della route. Non ammette i valori Null.|  
|**principal_id**|**int**|Identificatore dell'entità di database proprietaria della route. Ammette valori Null.|  
|**remote_service_name**|**nvarchar(256)**|Nome del servizio remoto. Ammette valori Null.|  
|**broker_instance**|**nvarchar(128)**|Identificatore dell'istanza di Service Broker che ospita il servizio remoto. Ammette valori Null.|  
|**vita**|**datetime**|Data e ora di scadenza della route. Questo valore non utilizza il fuso orario locale, bensì corrisponde all'ora di scadenza per UTC. Ammette valori Null.|  
|**address**|**nvarchar(256)**|Indirizzo di rete a cui Service Broker invia messaggi per il servizio remoto. Ammette valori Null. Per SQL Istanza gestita, l'indirizzo deve essere locale.|  
|**mirror_address**|**nvarchar(256)**|Indirizzo di rete del partner per il mirroring per il server specificato nell'indirizzo. Ammette valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
