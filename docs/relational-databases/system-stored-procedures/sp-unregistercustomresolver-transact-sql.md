---
description: sp_unregistercustomresolver (Transact-SQL)
title: sp_unregistercustomresolver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_unregistercustomresolver_TSQL
- sp_unregistercustomresolver
helpviewer_keywords:
- sp_unregistercustomresolver
ms.assetid: 08bd20c8-c6be-4be2-be9f-2b5e1d7bee43
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1a8bb2c7384312d9782bdb6169d17ace6eae7c6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202340"
---
# <a name="sp_unregistercustomresolver-transact-sql"></a>sp_unregistercustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Annulla la registrazione di un modulo di logica di business precedentemente registrato. La logica di business può essere un componente COM o un assembly [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Questa stored procedure viene eseguita nel server di distribuzione in cui è stata registrata la logica di business personalizzata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_unregistercustomresolver [ @article_resolver = ] 'article_resolver'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @article_resolver = ] 'article_resolver'` Specifica il nome della logica di business personalizzata di cui è in corso l'annullamento della registrazione. *article_resolver* è di **tipo nvarchar (255)** e non prevede alcun valore predefinito. Se la logica di business da rimuovere è un componente COM, questo parametro corrisponde al nome descrittivo del componente. Se la logica di business è un assembly .NET Framework, il parametro corrisponde al nome dell'assembly.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_unregistercustomresolver** viene utilizzata nella replica di tipo merge.  
  
 Utilizzare [sp_enumcustomresolvers](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) in qualsiasi server della topologia di replica per restituire l'elenco dei moduli della logica di business o dei resolver com personalizzati registrati disponibili per la topologia.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_unregistercustomresolver**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_lookupcustomresolver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_registercustomresolver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
