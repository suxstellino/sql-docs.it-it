---
description: sp_syscollector_delete_execution_log_tree (Transact-SQL)
title: sp_syscollector_delete_execution_log_tree (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_delete_execution_log_tree_TSQL
- sp_syscollector_delete_execution_log_tree
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_delete_execution_log_tree
- data collector [SQL Server], stored procedures
ms.assetid: 0a9a7c5b-c3cc-40ca-b524-e948a8cce4e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a25ff22212d08172d9fd34b6340329ba93cc6fd8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210223"
---
# <a name="sp_syscollector_delete_execution_log_tree-transact-sql"></a>sp_syscollector_delete_execution_log_tree (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina tutte le voci di log per l'esecuzione di un solo set di raccolta. Elimina anche le voci di log dalle tabelle [!INCLUDE[ssIS](../../includes/ssis-md.md)] per quell'esecuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syscollector_delete_execution_log_tree[ @log_id = ] log_id  
          , [ @from_collection_set = ] from_collection_set  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @log_id = ] log_id` Identificatore univoco del log del set di raccolta. *log_id* è di **tipo int**.  
  
`[ @from_collection_set = ] from_collection_set` Identificatore del set di raccolta. *from_collection_set* è di **bit = 1**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa procedura, è richiesta l'appartenenza al ruolo predefinito del database **dc_operator** (con autorizzazione Execute).  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
