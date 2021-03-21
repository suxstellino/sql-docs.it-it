---
description: sys.foreign_keys (Transact-SQL)
title: sys.foreign_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- foreign_keys
- sys.foreign_keys
- sys.foreign_keys_TSQL
- foreign_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_keys catalog view
ms.assetid: e960df1a-13fc-43ee-ba91-34c1b719ac2c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbdacfd88d1e807461bd168a4df41ddbed4f49b0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745241"
---
# <a name="sysforeign_keys-transact-sql"></a>sys.foreign_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni oggetto che rappresenta un vincolo FOREIGN KEY, con **sys. Object. Type** = F.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Per un elenco di colonne ereditate da questa vista, vedere [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**referenced_object_id**|**int**|ID dell'oggetto a cui viene fatto riferimento.|  
|**key_index_id**|**int**|ID della chiave di indice all'interno dell'oggetto a cui viene fatto riferimento.|  
|**is_disabled**|**bit**|Il vincolo FOREIGN KEY è disabilitato.|  
|**is_not_for_replication**|**bit**|Il vincolo FOREIGN KEY è stato creato con l'opzione NOT FOR REPLICATION.|  
|**is_not_trusted**|**bit**|Il vincolo FOREIGN KEY non è stato verificato dal sistema.|  
|**delete_referential_action**|**tinyint**|Operazione referenziale dichiarata per il vincolo FOREIGN KEY in caso di eliminazione.<br /><br /> 0 = Nessuna operazione<br /><br /> 1 = Propagazione<br /><br /> 2 = Impostazione su Null<br /><br /> 3 = Impostazione del valore predefinito|  
|**delete_referential_action_desc**|**nvarchar(60)**|Descrizione dell'operazione referenziale dichiarata per il vincolo FOREIGN KEY in caso di eliminazione:<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**update_referential_action**|**tinyint**|Operazione referenziale dichiarata per il vincolo FOREIGN KEY in caso di aggiornamento.<br /><br /> 0 = Nessuna operazione<br /><br /> 1 = Propagazione<br /><br /> 2 = Impostazione su Null<br /><br /> 3 = Impostazione del valore predefinito|  
|**update_referential_action_desc**|**nvarchar(60)**|Descrizione dell'operazione referenziale dichiarata per il vincolo FOREIGN KEY in caso di aggiornamento:<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**is_system_named**|**bit**|1 = Nome generato dal sistema.<br /><br /> 0 = Nome specificato dall'utente.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
