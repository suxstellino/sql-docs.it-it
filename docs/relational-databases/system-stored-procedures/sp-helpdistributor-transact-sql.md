---
description: sp_helpdistributor (Transact-SQL)
title: sp_helpdistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributor_TSQL
- sp_helpdistributor
helpviewer_keywords:
- sp_helpdistributor
ms.assetid: 37b0983e-3b69-4f0f-977e-20efce0a0b97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c23613a7b79ea7bfa418425a32b6d2ee9bf62699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186985"
---
# <a name="sp_helpdistributor-transact-sql"></a>sp_helpdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Elenca le informazioni sul server di distribuzione, il database di distribuzione, la directory di lavoro e l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account utente di Agent. Questa stored procedure viene eseguita nel database di pubblicazione o in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdistributor [ [ @distributor= ] 'distributor' OUTPUT ]  
    [ , [ @distribdb= ] 'distribdb' OUTPUT ]  
    [ , [ @directory= ] 'directory' OUTPUT ]  
    [ , [ @account= ] 'account' OUTPUT ]  
    [ , [ @min_distretention= ] min_distretention OUTPUT ]  
    [ , [ @max_distretention= ] max_distretention OUTPUT ]  
    [ , [ @history_retention= ] history_retention OUTPUT ]  
    [ , [ @history_cleanupagent= ] 'history_cleanupagent' OUTPUT ]  
    [ , [ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @local = ] 'local' ]  
    [ , [ @rpcsrvname= ] 'rpcsrvname' OUTPUT ]  
    [ , [ @publisher_type = ] 'publisher_type' OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @distributor = ] 'distributor' OUTPUT` Nome del server di distribuzione. Distributor è di **tipo sysname** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @distribdb = ] 'distribdb' OUTPUT` Nome del database di distribuzione. *distribdb* è di **tipo sysname** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @directory = ] 'directory' OUTPUT` È la directory di lavoro. la *directory* è di **tipo nvarchar (255)** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @account = ] 'account' OUTPUT`[!INCLUDE[msCoName](../../includes/msconame-md.md)]Account utente di Windows. l' *account* è di **tipo nvarchar (255)** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @min_distretention = ] _min_distretentionOUTPUT` Periodo di memorizzazione minimo per la distribuzione, in ore. *min_distretention* è di **tipo int** e il valore predefinito è **-1**.  
  
`[ @max_distretention = ] _max_distretentionOUTPUT` Periodo di memorizzazione massimo per la distribuzione, in ore. *max_distretention* è di **tipo int** e il valore predefinito è **-1**.  
  
`[ @history_retention = ] _history_retentionOUTPUT` Periodo di memorizzazione della cronologia, in ore. *history_retention* è di **tipo int** e il valore predefinito è **-1**.  
  
`[ @history_cleanupagent = ] 'history_cleanupagent' OUTPUT` Nome dell'agente di pulizia della cronologia. *history_cleanupagent* è di **tipo nvarchar (100)** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT` Nome dell'agente di pulizia della distribuzione. *distrib_cleanupagent* è di **tipo nvarchar (100)** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @local = ] 'local'` Indica se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve ottenere i valori del server locale. *local* è di **tipo nvarchar (5)** e il valore predefinito è null.  
  
`[ @rpcsrvname = ] 'rpcsrvname' OUTPUT` Nome del server che esegue chiamate di procedure remote. *rpcsrvname* è di **tipo sysname** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
`[ @publisher_type = ] 'publisher_type' OUTPUT` Tipo di server di pubblicazione del server di pubblicazione. *publisher_type* è di **tipo sysname** e **%** il valore predefinito è, che è l'unico valore che restituisce un set di risultati.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**distribuzione**|**sysname**|Nome del server di distribuzione.|  
|**database di distribuzione**|**sysname**|Nome del database di distribuzione.|  
|**Directory**|**nvarchar(255)**|Nome della directory di lavoro.|  
|**account**|**nvarchar(255)**|Nome dell'account utente di Windows.|  
|**min distrib retention**|**int**|Periodo di memorizzazione minimo per la distribuzione.|  
|**max distrib retention**|**int**|Periodo di memorizzazione massimo per la distribuzione.|  
|**history retention**|**int**|Periodo di memorizzazione per la cronologia.|  
|**history cleanup agent**|**nvarchar (100)**|Nome dell'agente di pulizia del contenuto della cronologia.|  
|**distribution cleanup agent**|**nvarchar (100)**|Nome dell'agente di pulizia dei riferimenti alla distribuzione.|  
|**rpc server name**|**sysname**|Nome del server di distribuzione remoto o locale.|  
|**rpc login name**|**sysname**|Account di accesso utilizzato per le chiamate di procedure remote al server di distribuzione remoto.|  
|**tipo di server di pubblicazione**|**sysname**|Tipo di server di pubblicazione. Può essere uno dei tipi seguenti:<br /><br /> **MSSQLSERVER**<br /><br /> **ORACLE**<br /><br /> **ORACLE GATEWAY**|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpdistributor** viene utilizzato in tutti i tipi di replica.  
  
 Se vengono specificati uno o più parametri di output durante l'esecuzione di **sp_helpdistributor**, a tutti i parametri di output impostati su null vengono assegnati valori all'uscita e non viene restituito alcun set di risultati. Se non viene specificato alcun parametro di output, viene restituito un set di risultati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le colonne del set di risultati o i parametri di output seguenti vengono restituiti ai membri del ruolo predefinito del server **sysadmin** nel server di pubblicazione e al ruolo predefinito del database **db_owner** nel database di pubblicazione:  
  
|Colonna del set di risultati|Parametro di output|  
|-----------------------|----------------------|  
|account|**\@account**|  
|min distrib retention|**\@min_distretention**|  
|max distrib retention|**\@max_distretention**|  
|history retention|**\@history_retention**|  
|history cleanup agent|**\@history_cleanupagent**|  
|distribution cleanup agent|**\@distrib_cleanupagent**|  
|rpc login name|Nessuno|  
  
 La colonna del set dei risultati seguente viene restituita agli utenti nell'elenco di accesso alla pubblicazione per una pubblicazione nel server di distribuzione:  
  
-   directory  
  
 Le seguenti colonne del set di risultati vengono restituite a tutti gli utenti.  
  
|Colonna del set di risultati|Parametro di output|  
|-----------------------|----------------------|  
|distributor|**\@distribuzione**|  
|database di distribuzione|**\@distribdb**|  
|rpc server name|**\@rpcsrvname**|  
|publisher type|**\@publisher_type**|  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
