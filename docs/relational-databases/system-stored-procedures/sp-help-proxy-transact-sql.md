---
description: sp_help_proxy (Transact-SQL)
title: sp_help_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_proxy
- sp_help_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_proxy
ms.assetid: a2fce164-2b64-40c2-8f35-6eeb7844abf1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a598d3fef67af2e2d9f4874b9e8f63770b762bb7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199622"
---
# <a name="sp_help_proxy-transact-sql"></a>sp_help_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza le informazioni per uno o più proxy.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @subsystem_name = ] 'subsystem_name' ,  
    [ @name = ] 'name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @proxy_id = ] id` Numero di identificazione del proxy per cui elencare le informazioni. Il *proxy_id* è di **tipo int** e il valore predefinito è null. È possibile specificare l' *ID* o il *proxy_name* .  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy per cui elencare le informazioni. Il *proxy_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare l' *ID* o il *proxy_name* .  
  
`[ @subsystem_name = ] 'subsystem_name'` Nome del sottosistema per cui elencare i proxy. Il *subsystem_name* è di **tipo sysname** e il valore predefinito è null. Quando si specifica *subsystem_name* , è necessario specificare anche il *nome* .  
  
 Nella tabella seguente vengono elencati i valori disponibili per ogni sottosistema.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|ActiveScripting|Script ActiveX|  
|CmdExec|Sistema operativo (CmdExec)|  
|Snapshot|Agente snapshot repliche|  
|LogReader|Agente lettura log repliche|  
|Distribuzione|agente di distribuzione di replica|  
|Unione|Agente merge repliche|  
|QueueReader|Agente di lettura coda repliche|  
|ANALYSISQUERY|Comando di Analysis Services|  
|ANALYSISCOMMAND|Query di Analysis Services|  
|Dts|Esecuzione pacchetti SSIS|  
|PowerShell|Script di PowerShell|  
  
`[ @name = ] 'name'` Nome di un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso per cui elencare i proxy. Il nome è di **tipo nvarchar (256)** e il valore predefinito è null. Quando si specifica *Name* , è necessario specificare anche *subsystem_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Numero di identificazione del proxy.|  
|**nome**|**sysname**|Nome del proxy.|  
|**credential_identity**|**sysname**|Nome utente e del dominio Microsoft Windows per le credenziali associate al proxy.|  
|**abilitato**|**tinyint**|Indica se il proxy è attivato. { **0** = non abilitato, **1** = abilitato}|  
|**description**|**nvarchar(1024)**|Descrizione del proxy.|  
|**user_sid**|**varbinary(85)**|ID di sicurezza (SID) di Windows dell'utente di Windows per questo proxy.|  
|**credential_id**|**int**|Identificatore per le credenziali associate a questo proxy.|  
|**credential_identity_exists**|**int**|Indica se credential_identity esiste. { 0 = non esiste, 1 = esiste }|  
  
## <a name="remarks"></a>Commenti  
 Se non vengono specificati parametri, **sp_help_proxy** elenca le informazioni per tutti i proxy nell'istanza di.  
  
 Per determinare quali proxy possono essere utilizzati da un account di accesso per un determinato sottosistema, specificare *Name* e *subsystem_name*. Quando vengono forniti questi argomenti, **sp_help_proxy** elenca i proxy a cui l'account di accesso specificato può accedere e che possono essere utilizzati per il sottosistema specificato.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono appartenere al ruolo predefinito del database **SQLAgentOperatorRole** nel database **msdb** .  
  
 Per informazioni dettagliate su **SQLAgentOperatorRole**, vedere [SQL Server Agent ruoli](../../ssms/agent/sql-server-agent-fixed-database-roles.md)predefiniti del database.  
  
> [!NOTE]  
>  Le colonne **credential_identity** e **user_sid** vengono restituite nel set di risultati solo quando i membri di **sysadmin** eseguono questa stored procedure.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-information-for-all-proxies"></a>R. Visualizzazione di un elenco di informazioni per tutti i proxy  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per tutti i proxy nell'istanza.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-proxy"></a>B. Visualizzazione di un elenco di informazioni per un proxy specifico  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per il proxy denominato `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)  
  
  
