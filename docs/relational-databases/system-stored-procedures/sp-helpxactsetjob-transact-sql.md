---
description: sp_helpxactsetjob (Transact-SQL)
title: sp_helpxactsetjob (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpxactsetjob
- sp_helpxactsetjob_TSQL
helpviewer_keywords:
- sp_helpxactsetjob
ms.assetid: 242cea3e-e6ac-4f84-a072-b003b920eb33
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39092ea96ed0f143af7322fea3f04d38d3e4eef0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194730"
---
# <a name="sp_helpxactsetjob-transact-sql"></a>sp_helpxactsetjob (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza informazioni sul processo Xactset per un server di pubblicazione Oracle. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpxactsetjob [ @publisher = ] 'publisher'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione non a cui appartiene il processo. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**jobnumber**|**int**|Numero di processo Oracle.|  
|**LASTDATE**|**varchar (22)**|Data dell'ultima esecuzione del processo.|  
|**thisdate**|**varchar (22)**|Ora della modifica|  
|**nextdate**|**varchar (22)**|Data della successiva esecuzione del processo.|  
|**broken**|**varchar(1)**|Flag che indica se il processo è interrotto.|  
|**intervallo**|**varchar (200)**|Intervallo del processo.|  
|**fallimenti**|**int**|Numero di errori per il processo.|  
|**xactsetjobwhat**|**varchar (200)**|Nome della procedura eseguita dal processo.|  
|**xactsetjob**|**varchar(1)**|Stato del processo. I possibili valori sono i seguenti:<br /><br /> **1** -il processo è abilitato.<br /><br /> **0** : il processo è disabilitato.|  
|**xactsetlonginterval**|**int**|Intervallo lungo per il processo.|  
|**xactsetlongthreshold**|**int**|valore soglia lungo per il processo.|  
|**xactsetshortinterval**|**int**|Intervallo breve per il processo.|  
|**xactsetshortthreshold**|**int**|valore soglia breve per il processo.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpxactsetjob** viene utilizzata nella replica snapshot e nella replica transazionale per i Publisher Oracle.  
  
 **sp_helpxactsetjob** restituisce sempre le impostazioni correnti per il processo del Xactset (HREPL_XactSetJob) nel server di pubblicazione. Se il processo Xactset è attualmente nella coda dei processi, restituisce anche gli attributi del processo dalla vista del dizionario dei dati USER_JOB creata nell'ambito dell'account di amministratore nel server di pubblicazione Oracle.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_helpxactsetjob** solo un membro del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare il processo del set di transazioni per un server di pubblicazione Oracle &#40;programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [sp_publisherproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-publisherproperty-transact-sql.md)  
  
  
