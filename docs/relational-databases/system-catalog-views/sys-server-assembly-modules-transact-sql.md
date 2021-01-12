---
description: sys.server_assembly_modules (Transact-SQL)
title: sys.server_assembly_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_assembly_modules_TSQL
- sys.server_assembly_modules
- server_assembly_modules
- sys.server_assembly_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_assembly_modules catalog view
ms.assetid: af799e38-2d16-49b2-bcf5-6f9199af899e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9449fe5719934222532a251426436415a26cd233
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100161"
---
# <a name="sysserver_assembly_modules-transact-sql"></a>sys.server_assembly_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni modulo in assembly per i trigger a livello di server di tipo TA. Questa vista esegue il mapping tra trigger di assembly e l'implementazione CLR sottostante. È possibile unire in join questa relazione con **sys.server_triggers**. L'assembly deve essere caricato nel database **Master** . La tuple (object_id) è la chiave della relazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Riferimento FOREIGN KEY all'oggetto in base a cui è stato definito questo modulo in assembly.|  
|**assembly_id**|**int**|ID dell'assembly da cui è stato creato questo modulo. L'assembly deve essere caricato nel database master.|  
|**assembly_class**|**sysname**|Nome della classe all'interno dell'assembly che definisce questo modulo.|  
|**assembly_method**|**sysname**|Nome del metodo all'interno della classe che definisce questo modulo. Questo valore è NULL per le funzioni di aggregazione.|  
|**execute_as_principal_id**|**int**|ID dell'entità server EXECUTE AS.<br /><br /> Questo valore è NULL per impostazione predefinita e se viene utilizzato EXECUTE AS CALLER.<br /><br /> ID dell'entità specificata se EXECUTE AS SELF EXECUTE AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
