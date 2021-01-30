---
description: sp_helpmergefilter (Transact-SQL)
title: sp_helpmergefilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergefilter
- sp_helpmergefilter_TSQL
helpviewer_keywords:
- sp_helpmergefilter
ms.assetid: f133a094-0009-4771-b93b-e86a5c01e40b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 862f23713da4bcd611310149b509e34658766006
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179241"
---
# <a name="sp_helpmergefilter-transact-sql"></a>sp_helpmergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui filtri di merge. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergefilter [ @publication= ] 'publication'   
    [ , [ @article= ] 'article']  
    [ , [ @filtername= ] 'filtername']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e il valore predefinito è **%** , che restituisce i nomi di tutti gli articoli.  
  
`[ @filtername = ] 'filtername'` Nome del filtro su cui si desidera ottenere informazioni. *FilterName* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i filtri definiti per l'articolo o la pubblicazione.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**join_filterid**|**int**|ID del filtro join.|  
|**NomeFiltro**|**sysname**|Nome del filtro.|  
|**join article name**|**sysname**|Nome dell'articolo di join.|  
|**join_filterclause**|**nvarchar (2000)**|Clausola di filtro che qualifica il join.|  
|**join_unique_key**|**int**|Indica se il join è basato su una chiave univoca.|  
|**base table owner**|**sysname**|Nome del proprietario della tabella di base.|  
|**base table name**|**sysname**|Nome della tabella di base.|  
|**join table owner**|**sysname**|Nome del proprietario della tabella che si desidera unire in join alla tabella di base.|  
|**join table name**|**sysname**|Nome della tabella che si desidera unire in join alla tabella di base.|  
|**article name**|**sysname**|Nome dell'articolo di tabella che si desidera unire in join alla tabella di base.|  
|**filter_type**|**tinyint**|Tipo di filtro di merge. I possibili valori sono i seguenti:<br /><br /> **1** = solo filtro di join<br /><br /> **2** = relazione tra record logici<br /><br /> **3** = entrambi|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergefilter** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** e del ruolo predefinito del database **db_owner** possono eseguire **sp_helpmergefilter**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
