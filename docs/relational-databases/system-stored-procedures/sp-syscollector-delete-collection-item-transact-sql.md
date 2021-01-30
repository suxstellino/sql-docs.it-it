---
description: sp_syscollector_delete_collection_item (Transact-SQL)
title: sp_syscollector_delete_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_delete_collection_item
- sp_syscollector_delete_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_delete_collecton_item
- data collector [SQL Server], stored procedures
ms.assetid: 9c2b0990-1d3d-4a59-94a0-3cca6fef4681
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ae900632edd8ccce0e31c87bd7814c41f02da291
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204324"
---
# <a name="sp_syscollector_delete_collection_item-transact-sql"></a>sp_syscollector_delete_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina un elemento della raccolta da un set di raccolta  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syscollector_delete_collection_item [[ @collection_item_id = ] collection_item_id ]  
    , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Argomenti  
 [ @collection_item_id =] *collection_item_id*  
 Identificatore univoco per l'elemento della raccolta. *collection_item_id* è di **tipo int** e il valore predefinito è null. *collection_item_id* deve avere un valore se il *nome* è null.  
  
 [ @name =]'*nome*'  
 Nome dell'elemento della raccolta. *Name* è di **tipo sysname** e il valore predefinito è null. Se *collection_item_id* è null, il *nome* deve avere un valore.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syscollector_delete_collection_item nel contesto del database di sistema msdb. Non è possibile eliminare elementi della raccolta dai set di raccolta di sistema.  
  
 Durante questa operazione, il set di raccolta che contiene l'elemento della raccolta viene arrestato e riavviato.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa procedura, è richiesta l'appartenenza al ruolo predefinito del database dc_admin (con autorizzazione EXECUTE) .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene eliminato un elemento della raccolta denominato `MyCollectionItem1`.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_delete_collection_item @name = 'MyCollectionItem1';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_create_collection_item &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)   
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
