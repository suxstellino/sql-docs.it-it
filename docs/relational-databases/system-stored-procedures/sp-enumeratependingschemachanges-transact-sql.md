---
description: sp_enumeratependingschemachanges (Transact-SQL)
title: sp_enumeratependingschemachanges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumeratependingschemachanges
- sp_enumeratependingschemachanges_TSQL
helpviewer_keywords:
- sp_enumeratependingschemachanges
ms.assetid: df169b21-d10a-41df-b3a1-654cfb58bc21
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7a75e22f94688d4d8819b22b1b478bd3a3a66ce1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193572"
---
# <a name="sp_enumeratependingschemachanges-transact-sql"></a>sp_enumeratependingschemachanges (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco di tutte le modifiche dello schema in sospeso. Questo stored procedure può essere utilizzato con [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md), che consente a un amministratore di ignorare le modifiche dello schema in sospeso selezionate in modo che non vengano replicate. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_enumeratependingschemachanges [ @publication = ] 'publication'   
    [ , [ @starting_schemaversion = ] starting_schemaversion ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @starting_schemaversion = ] starting_schemaversion` Modifica dello schema con il numero più basso da includere nel set di risultati.  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**article_name**|**sysname**|Nome dell'articolo a cui si applica la modifica dello schema oppure a **livello di pubblicazione** per le modifiche dello schema valide per l'intera pubblicazione.|  
|**SchemaVersion**|**int**|Numero della modifica dello schema in sospeso.|  
|**schematype**|**sysname**|Valore di testo che rappresenta il tipo di modifica dello schema.|  
|**schematext**|**nvarchar(max)**|[!INCLUDE[tsql](../../includes/tsql-md.md)] che descrive la modifica dello schema.|  
|**schemastatus**|**nvarchar (10)**|Specifica se è in sospeso una modifica dello schema per l'articolo. I possibili valori sono i seguenti:<br /><br /> **attivo** = modifica dello schema in sospeso<br /><br /> **inactive** = modifica dello schema inattiva<br /><br /> **Skip** = la modifica dello schema non viene replicata|  
|**schemaguid**|**uniqueidentifier**|Identifica la modifica dello schema.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_enumeratependingschemachanges** viene utilizzata nella replica di tipo merge.  
  
 **sp_enumeratependingschemachanges**, utilizzato con [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md), è concepito per supportare la replica di tipo merge e deve essere utilizzato solo quando altre azioni correttive, ad esempio la reinizializzazione, non riescono a correggere la situazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_enumeratependingschemachanges**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sysmergeschemachange &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
