---
description: sp_helpmergearticlecolumn (Transact-SQL)
title: sp_helpmergearticlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergearticlecolumn
- sp_helpmergearticlecolumn_TSQL
helpviewer_keywords:
- sp_helpmergearticlecolumn
ms.assetid: 651c017b-9e9a-48f2-a0bd-6fc896eab334
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d781adddce0db83791614012b8583f20af1d2e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179277"
---
# <a name="sp_helpmergearticlecolumn-transact-sql"></a>sp_helpmergearticlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce l'elenco di colonne dell'articolo della tabella o vista specificato per una pubblicazione di tipo merge. Poiché le stored procedure non includono colonne, questa procedura restituisce un errore se, come articolo, viene specificata una stored procedure. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergearticlecolumn [ @publication = ] 'publication' ]  
        , [ @article= ] 'article' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome di una tabella o di una vista che rappresenta l'articolo su cui recuperare informazioni. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**column_id**|**sysname**|Identificatore della colonna.|  
|**column_name**|**sysname**|Nome della colonna di una tabella o vista.|  
|**Pubblicato**|**bit**|Specifica se pubblicare il nome della colonna.<br /><br /> **1** specifica che la colonna è in fase di pubblicazione.<br /><br /> **0** indica che non è pubblicato.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergearticlecolumn** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_helpmergearticlecolumn** solo i membri del ruolo predefinito del database **replmonitor** nel database di distribuzione o l'elenco di accesso alla pubblicazione per la pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
