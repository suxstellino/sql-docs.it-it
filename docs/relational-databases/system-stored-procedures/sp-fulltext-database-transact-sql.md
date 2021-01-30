---
description: sp_fulltext_database (Transact-SQL)
title: sp_fulltext_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_database_TSQL
- sp_fulltext_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_database
ms.assetid: eeb1e151-eb00-484c-8fd1-5641e621ffc6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f180dceedb5473ec3b7518a03f6ea86f0659f8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185433"
---
# <a name="sp_fulltext_database-transact-sql"></a>sp_fulltext_database (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Non ha alcun effetto sui cataloghi full-text in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive ed è supportata solo per la compatibilità con le versioni precedenti. **sp_fulltext_database** non disabilita il motore di Full-Text per un determinato database. Tutti i database creati dall'utente in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] sono sempre abilitati per l'indicizzazione full-text.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] In alternativa, usare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_fulltext_database [@action=] 'action'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @action = ] 'action'` Azione da eseguire. **Action** è di tipo **varchar (20)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**enable**|Supportato unicamente per compatibilità con le versioni precedenti. Non produce alcun effetto sui cataloghi full-text in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|  
|**disable**|Supportato unicamente per compatibilità con le versioni precedenti. Non produce alcun effetto sui cataloghi full-text in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, non è possibile disabilitare l'indicizzazione full-text. La disabilitazione dell'indicizzazione full-text non comporta la rimozione di righe da **sysfulltextcatalogs** e non indica che le tabelle abilitate per la funzionalità full-text non sono più contrassegnate per l'indicizzazione full-text. Tutte le definizioni di metadati full-text vengono conservate nelle tabelle di sistema.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** e del ruolo predefinito del database **db_owner** possono eseguire **sp_fulltext_database**.  
  
## <a name="see-also"></a>Vedere anche  
 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
