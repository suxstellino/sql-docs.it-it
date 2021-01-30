---
description: sp_ivindexhasnullcols (Transact-SQL)
title: sp_ivindexhasnullcols (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_ivindexhasnullcols
- sp_ivindexhasnullcols_TSQL
helpviewer_keywords:
- sp_ivindexhasnullcols
ms.assetid: ed2cde63-37e1-43cf-b6ba-3b6114a0f797
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9fc065d30bc8fac8195a3a389de1b316645bcd57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183400"
---
# <a name="sp_ivindexhasnullcols-transact-sql"></a>sp_ivindexhasnullcols (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verifica che l'indice cluster della vista indicizzata sia univoco e non includa colonne che ammettono valori Null se la vista indicizzata verrà utilizzata per la creazione di una pubblicazione transazionale. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_ivindexhasnullcols [ @viewname = ] 'view_name'  
        , [ @fhasnullcols= ] field_has_null_columns OUTPUT  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @viewname = ] 'view_name'` Nome della visualizzazione da verificare. *view_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @fhasnullcols = ] field_has_null_columns OUTPUT` Flag che indica se l'indice della vista include colonne che ammettono valori NULL. *view_name* è di **tipo sysname** e non prevede alcun valore predefinito. Restituisce un valore pari a **1** se l'indice della vista include colonne che ammettono valori null. Restituisce un valore pari a **0** se la vista non contiene colonne che ammettono valori null.  
  
> [!NOTE]  
>  Se il stored procedure stesso restituisce un codice restituito **1**, ovvero l'esecuzione del stored procedure ha avuto un errore, questo valore è **0** e deve essere ignorato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_ivindexhasnullcols** viene utilizzata dalla replica transazionale.  
  
 Per impostazione predefinita, gli articoli di vista indicizzata di una pubblicazione vengono creati sotto forma di tabella nei Sottoscrittori. Quando tuttavia la colonna indicizzata ammette valori Null, la vista indicizzata viene creata come vista indicizzata anziché come tabella nel Sottoscrittore. Tramite l'esecuzione di questa stored procedure è possibile verificare se nella vista indicizzata corrente esiste o meno questo problema.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_ivindexhasnullcols**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
