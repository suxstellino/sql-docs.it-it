---
description: sysarticlecolumns (vista di sistema) (Transact-SQL)
title: sysarticlecolumns (vista di sistema) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticlecolumns
- sysarticlecolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticlecolumns view
ms.assetid: a8dd8d13-c827-45c4-87ba-802725301382
author: stevestein
ms.author: sstein
ms.openlocfilehash: 1de338795532eb470db25a4510f75dc3d6bf4239
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160348"
---
# <a name="sysarticlecolumns-system-view-transact-sql"></a>sysarticlecolumns (vista di sistema) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **sysarticlecolumns** espone informazioni aggiuntive sulle colonne negli articoli pubblicati. Questa vista è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Identifica un articolo.|  
|**colid**|**int**|Identifica una colonna di un articolo.|  
|**is_udt**|**int**|Indica se il tipo di dati della colonna è un tipo definito dall'utente (UDT). Il valore **1** indica una colonna con tipo definito dall'utente.|  
|**is_xml**|**int**|Indica se la colonna è una colonna **XML** . Il valore **1** indica una colonna **XML** .|  
|**is_max**|**int**|Indica se la colonna è una colonna con tipo di dati con valori di grandi dimensioni (**varchar (max)**, **nvarchar (max)** o **varbinary (max)**). Il valore **1** indica una colonna con valori di grandi dimensioni.|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
