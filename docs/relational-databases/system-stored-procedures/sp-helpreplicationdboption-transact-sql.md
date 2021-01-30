---
description: sp_helpreplicationdboption (Transact-SQL)
title: sp_helpreplicationdboption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplicationdboption_TSQL
- sp_helpreplicationdboption
helpviewer_keywords:
- sp_helpreplicationdboption
ms.assetid: 143ce689-108b-49d7-9892-fd3a86897f38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17c47d247e154ed80f0daeb8bacdb4984864fa03
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210809"
---
# <a name="sp_helpreplicationdboption-transact-sql"></a>sp_helpreplicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Indica se i database nel server di pubblicazione sono abilitati per la replica. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione. *Questa proprietà non è supportata per server di pubblicazione Oracle.*  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpreplicationdboption [ [ @dbname =] 'dbname' ]  
    [ , [ @type = ] 'type' ]  
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Nome del database. *dbname* è di **tipo sysname** e il valore predefinito è **%** . Se **%** , il set di risultati contiene tutti i database nel server di pubblicazione; in caso contrario, vengono restituite solo le informazioni sul database specificato. Non vengono restituite informazioni per gli eventuali database per cui l'utente non dispone delle autorizzazioni appropriate, come indicato di seguito.  
  
`[ @type = ] 'type'` Limita il set di risultati in modo che contenga solo i database in cui è stato abilitato il valore del *tipo* di opzione di replica specificato. *Type* è di tipo **sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**pubblicare**|È consentita la replica transazionale.|  
|**pubblicazione di tipo merge**|È consentita la replica di tipo merge.|  
|**replica consentita** (impostazione predefinita)|È consentita la replica transazionale o la replica di tipo merge.|  
  
`[ @reserved = ] reserved` Specifica se vengono restituite informazioni sulle pubblicazioni e sulle sottoscrizioni esistenti. *riservato* è di **bit** e il valore predefinito è 0. Se è **1**, il set di risultati include informazioni sulla presenza o meno di pubblicazioni o sottoscrizioni esistenti nel database specificato.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del database.|  
|**id**|**int**|Identificatore del database.|  
|**transpublish**|**bit**|Se il database è stato abilitato per la pubblicazione snapshot o transazionale; dove il valore **1** indica che la pubblicazione snapshot o transazionale è abilitata.|  
|**mergepublish**|**bit**|Se il database è stato abilitato per la pubblicazione di tipo merge; dove il valore **1** indica che la pubblicazione di tipo merge è abilitata.|  
|**dbowner**|**bit**|Se l'utente è un membro del ruolo predefinito del database **db_owner** ; dove il valore **1** indica che l'utente è un membro di questo ruolo.|  
|**dbreadonly**|**bit**|Indica se il database è contrassegnato come di sola lettura. dove il valore **1** indica che il database è di sola lettura.|  
|**haspublications**|**bit**|Indica se nel database sono presenti pubblicazioni esistenti. dove il valore **1** indica che sono presenti pubblicazioni esistenti.|  
|**haspullsubscriptions**|**bit**|Indica se nel database sono presenti sottoscrizioni pull esistenti. dove il valore **1** indica che sono presenti sottoscrizioni pull esistenti.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpreplicationdboption** viene utilizzata per la replica snapshot, transazionale e di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 I membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_helpreplicationdboption** per qualsiasi database. I membri del ruolo predefinito del database **db_owner** possono eseguire **sp_helpreplicationdboption** per quel database.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_replicationdboption &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
