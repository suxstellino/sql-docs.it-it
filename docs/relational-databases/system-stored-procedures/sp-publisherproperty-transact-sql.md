---
description: sp_publisherproperty (Transact-SQL)
title: sp_publisherproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_publisherproperty
- sp_publisherproperty_TSQL
helpviewer_keywords:
- sp_publisherproperty
ms.assetid: 0ed1ebc1-a1bd-4aed-9f46-615c5cf07827
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 80e2e9eda3d6a4f900c4ab9f8e48dc783d23cd2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210043"
---
# <a name="sp_publisherproperty-transact-sql"></a>sp_publisherproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Consente di visualizzare o modificare le proprietà del server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. Questa stored procedure viene eseguita nel database di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_publisherproperty [ @publisher = ] 'publisher'   
   [ , [ @propertyname = ] 'propertyname' ]   
   [ , [ @propertyvalue = ] 'propertyvalue' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione eterogeneo. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @propertyname = ] 'propertyname'` Nome della proprietà da impostare. *PropertyName* è di **tipo sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**xactsetbatching**|Indica se le transazioni nel server di pubblicazione sono raggruppate in set con consistenza transazionale per elaborazioni successive, noti come Xactset. Il valore **Enabled** indica che è possibile creare Xactset, che è l'impostazione predefinita. Il valore **disabled** indica che i Xactset esistenti vengono elaborati da non vengono creati nuovi Xactset.|  
|**xactsetjob**|Indica se è attivo il processo Xactset per la creazione di Xactset. Il valore **Enabled** indica che il processo Xactset viene eseguito periodicamente per creare Xactset nel server di pubblicazione. Il valore **disabled** indica che i Xactset vengono creati solo dal agente di lettura log quando esegue il polling del server di pubblicazione per le modifiche.|  
|**xactsetjobinterval**|Intervallo tra le esecuzioni del processo Xactset, espresso in minuti.|  
  
 Quando *PropertyName* viene omesso, vengono restituite tutte le proprietà impostabili.  
  
 `[ @propertyvalue = ] 'propertyvalue'`  
 Nuovo valore per la proprietà. *PropertyValue* è di **tipo sysname** e il valore predefinito è null. Quando *PropertyValue* viene omesso, viene restituita l'impostazione corrente per la proprietà.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**propertyname**|**sysname**|Restituisce le proprietà delle pubblicazioni seguenti che è possibile impostare:<br /><br /> **xactsetbatching**<br /><br /> **xactsetjob**<br /><br /> **xactsetjobinterval**|  
|**PropertyValue**|**sysname**|Impostazione corrente della proprietà nella colonna **PropertyName** .|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_publisherproperty** viene utilizzata per la replica transazionale per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non.  
  
 Quando viene specificato solo *Publisher* , il set di risultati include le impostazioni correnti per tutte le proprietà che è possibile impostare.  
  
 Quando viene specificato *PropertyName* , nel set di risultati viene visualizzata solo la proprietà denominata.  
  
 Se si specificano tutti i parametri, la proprietà viene modificata e non viene restituito alcun set di risultati.  
  
 Quando si modifica la proprietà **xactsetjobinterval** per un processo in esecuzione, è necessario riavviare il processo per rendere effettivo il nuovo intervallo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione possono eseguire **sp_publisherproperty**.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare il processo del set di transazioni per un server di pubblicazione Oracle &#40;programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
