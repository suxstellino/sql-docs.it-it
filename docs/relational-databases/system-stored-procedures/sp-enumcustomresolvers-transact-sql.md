---
description: sp_enumcustomresolvers (Transact-SQL)
title: sp_enumcustomresolvers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumcustomresolvers
- sp_enumcustomresolvers_TSQL
helpviewer_keywords:
- sp_enumcustomresolvers
ms.assetid: 81bd0d3a-48dc-42b1-b662-c630f61fc630
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8972aa1aaa4e567c60efb9653a75e6fa196211b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99156779"
---
# <a name="sp_enumcustomresolvers-transact-sql"></a>sp_enumcustomresolvers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce l'elenco di tutti i gestori della logica di business e i sistemi di risoluzione personalizzati disponibili registrati nel server di distribuzione. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_enumcustomresolvers [ [ @distributor =] 'distributor']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @distributor = ] 'distributor'` Nome del server di distribuzione in cui si trova il sistema di risoluzione personalizzato. *Distributor* è di **tipo sysname** e il valore predefinito è null. *Questo parametro è deprecato e verrà rimosso da una delle prossime versioni.*  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**article_resolver**|**nvarchar(255)**|Nome descrittivo del gestore della logica di business o del sistema di risoluzione dei conflitti.|  
|**resolver_clsid**|**nvarchar(50)**|ID classe (CLSID) del sistema di risoluzione basato su COM. Per un gestore della logica di business, questa colonna restituisce un valore di CLSID pari a zero.|  
|**is_dotnet_assembly**|**bit**|Indica se la registrazione viene eseguita per un gestore della logica di business.<br /><br /> **0** = sistema di risoluzione dei conflitti basato su com<br /><br /> **1** = gestore della logica di business|  
|**dotnet_assembly_name**|**nvarchar(255)**|Nome dell'assembly [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework che implementa il gestore della logica di business.|  
|**dotnet_class_name**|**nvarchar(255)**|Nome della classe che sostituisce <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> per implementare il gestore della logica di business.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_enumcustomresolvers** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** e del ruolo predefinito del database **db_owner** possono eseguire **sp_enumcustomresolvers**.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementare un gestore della logica di business per un articolo di merge](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Implementare un sistema di risoluzione dei conflitti personalizzato per un articolo di merge](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
