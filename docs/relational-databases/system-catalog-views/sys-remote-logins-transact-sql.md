---
description: sys.remote_logins (Transact-SQL)
title: sys.remote_logins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.remote_logins_TSQL
- remote_logins
- remote_logins_TSQL
- sys.remote_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_logins catalog view
ms.assetid: 38477e91-d084-4df7-b1de-b930c5580189
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b66a0dc17660d4ff5c379dbedc9ebd629aa8c371
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206852"
---
# <a name="sysremote_logins-transact-sql"></a>sys.remote_logins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni mapping di account di accesso remoto. Questa vista del catalogo viene utilizzata per eseguire il mapping tra gli account di accesso locali in ingresso che risultano provenire da un server corrispondente e un account di accesso locale effettivo.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|ID del server in **sys. Servers**. Questo nome viene fornito dalla connessione proveniente dal server "remoto".|  
|**remote_name**|**sysname**|Nome dell'account di accesso fornito dalla connessione per il mapping. Se è NULL, viene utilizzato il nome dell'account di accesso specificato nella connessione.|  
|**local_principal_id**|**int**|ID dell'entità server a cui viene eseguito il mapping dell'account di accesso. Se è 0, sull'account di accesso remoto viene eseguito il mapping all'account di accesso con lo stesso nome.|  
|**modify_date**|**datetime**|Data dell'ultima modifica dell'account di accesso collegato.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di server collegati &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
