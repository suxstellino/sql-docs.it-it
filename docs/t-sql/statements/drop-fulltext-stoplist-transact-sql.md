---
description: DROP FULLTEXT STOPLIST (Transact-SQL)
title: DROP FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_FULLTEXT_STOPLIST_TSQL
- DROP FULLTEXT STOPLIST
dev_langs:
- TSQL
helpviewer_keywords:
- DROP FULLTEXT STOPLIST statement
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 3ee2a2bb-1dfb-4e7c-90e9-9d917cd84a15
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: edd5eb9f8d47561d1ea0c97b47bcfdcb83885585
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179619"
---
# <a name="drop-fulltext-stoplist-transact-sql"></a>DROP FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Elimina un elenco di parole non significative full-text dal database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!IMPORTANT]  
>  CREATE FULLTEXT STOPLIST è supportato solo per il livello di compatibilità 100 e superiore. Per i livelli di compatibilità 80 e 90, l'elenco di parole non significative di sistema viene sempre assegnato al database.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP FULLTEXT STOPLIST stoplist_name  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *stoplist_name*  
 Nome dell'elenco di parole non significative full-text da eliminare dal database.  
  
## <a name="remarks"></a>Commenti  
 DROP FULLTEXT STOPLIST ha esito negativo se qualsiasi indice full-text fa riferimento all'elenco di parole non significative full-text da eliminare.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eliminare un elenco di parole non significative è necessaria l'autorizzazione DROP per tale elenco o l'appartenenza al ruolo predefinito del database **db_owner** o **db_ddladmin**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene eliminato un elenco di parole non significative full-text denominato `myStoplist`.  
  
```sql 
DROP FULLTEXT STOPLIST myStoplist;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)   
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)  
  
  
