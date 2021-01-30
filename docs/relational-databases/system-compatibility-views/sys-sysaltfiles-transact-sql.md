---
description: sys.sysaltfiles (Transact-SQL)
title: sys.sysaltfiles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysaltfiles_TSQL
- sys.sysaltfiles
- sysaltfiles_TSQL
- sysaltfiles
dev_langs:
- TSQL
helpviewer_keywords:
- sysaltfiles system table
- sys.sysaltfiles compatibility view
ms.assetid: 698dec23-5336-4108-87a5-f8e407f8da09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1482980471675563417943366eef233396f6f902
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180732"
---
# <a name="syssysaltfiles-transact-sql"></a>sys.sysaltfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In casi particolari contiene le righe corrispondenti ai file di un database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**fileid**|**smallint**|Numero di identificazione del file. Questo valore è univoco per ogni database.|  
|**GroupID**|**smallint**|Numero di identificazione del filegroup.|  
|**size**|**int**|Dimensioni del file espresse in pagine da 8 KB.|  
|**MaxSize**|**int**|Dimensioni massime del file espresse in pagine da 8 KB.<br /><br /> 0 = Nessun aumento.<br /><br /> -1 = La dimensione del file aumenterà finché il disco è pieno.<br /><br /> 268435456 = La dimensione del file di log aumenterà fino al valore massimo di 2 TB.<br /><br /> Nota: i database aggiornati con una dimensione di file di log illimitata segnaleranno-1 per le dimensioni massime del file di log.|  
|**crescita**|**int**|Aumento delle dimensioni del database.<br /><br /> 0 = Nessun aumento. I possibili valori sono un numero di pagine o una percentuale delle dimensioni del file, a seconda del valore di status. Se **status** è 0x100000, **Growth** è la percentuale delle dimensioni del file; in caso contrario, è il numero di pagine.|  
|**Stato**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**perf**|**int**|Riservato.|  
|**dbid**|**smallint**|Numero di identificazione del database a cui appartiene il file.|  
|**nome**|**sysname**|Nome logico del file.|  
|**filename**|**nvarchar(260)**|Nome del dispositivo fisico, incluso il percorso completo del file.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
