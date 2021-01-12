---
description: sys.filetables (Transact-SQL)
title: sys. FileTables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- filetables
- filetables_TSQL
- sys.filetables
- sys.filetables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filetables catalog view
ms.assetid: a740be59-cd52-4707-9ad2-5203669a63ac
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 990965f5f3af485437cd2e97d5eb952a53b157cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093151"
---
# <a name="sysfiletables-transact-sql"></a>sys.filetables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene restituita una riga per ogni oggetto FileTable in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Per altre informazioni sugli oggetti FileTable, vedere [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).    
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**||Numero di identificazione dell'oggetto. Valore univoco all'interno di un database.<br /><br /> Per ulteriori informazioni, [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_enabled**|**bit**|1 = Lo stato dell'oggetto FileTable è "abilitato".|  
|**directory_name**|**varchar(255)**|Nome della directory radice per un oggetto FileTable.|  
|**filename_collation_id**||Identificatore delle regole di confronto definito per l'oggetto FileTable.|  
|**filename_collation_name**||Nome delle regole di confronto definito per l'oggetto FileTable.|  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire tabelle FileTable](../../relational-databases/blob/manage-filetables.md)   
 [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md)  
  
  
