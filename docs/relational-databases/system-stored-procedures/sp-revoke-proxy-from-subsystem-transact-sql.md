---
description: sp_revoke_proxy_from_subsystem (Transact-SQL)
title: sp_revoke_proxy_from_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revoke_login_from_subsystem
- sp_revoke_login_from_subsystem_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_proxy_from_subsystem
ms.assetid: b87bc8ba-3ea8-4aed-b54b-32c3d82d9d2a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9457a1d9dd0d9fd01a7418cd56e772902d593b6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211860"
---
# <a name="sp_revoke_proxy_from_subsystem-transact-sql"></a>sp_revoke_proxy_from_subsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca l'accesso a un sottosistema da un proxy.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_revoke_proxy_from_subsystem   
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @proxy_id = ] id` Numero di identificazione del proxy da cui revocare l'accesso. Il *proxy_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *proxy_id* o *proxy_name* , ma non è possibile specificarli entrambi.  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy da cui revocare l'accesso. Il *proxy_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *proxy_id* o *proxy_name* , ma non è possibile specificarli entrambi.  
  
`[ @subsystem_id = ] id` Numero ID del sottosistema a cui revocare l'accesso. Il *subsystem_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *subsystem_id* o *subsystem_name* , ma non è possibile specificarli entrambi. Nella tabella seguente vengono elencati i valori disponibili per ogni sottosistema.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**2**|Script ActiveX<br /><br /> Importante il sottosistema di scripting ActiveX verrà rimosso da Agent in una versione futura di **\* . \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.|  
|**3**|Sistema operativo (CmdExec)|  
|**4**|Agente snapshot repliche|  
|**5**|Agente lettura log repliche|  
|**6**|Agente distribuzione repliche|  
|**7**|Agente merge repliche|  
|**8**|Agente di lettura coda repliche|  
|**9**|Comando di Analysis Services|  
|**10**|Query di Analysis Services|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] esecuzione del pacchetto|  
|**12**|Script di PowerShell|  
  
`[ @subsystem_name = ] 'subsystem_name'` Nome del sottosistema a cui revocare l'accesso. Il *subsystem_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *subsystem_id* o *subsystem_name* , ma non è possibile specificarli entrambi. Nella tabella seguente vengono elencati i valori disponibili per ogni sottosistema.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|ActiveScripting|Script ActiveX|  
|CmdExec|Sistema operativo (CmdExec)|  
|Snapshot|Agente snapshot repliche|  
|LogReader|Agente lettura log repliche|  
|Distribuzione|Agente distribuzione repliche|  
|Unione|Agente merge repliche|  
|QueueReader|Agente di lettura coda repliche|  
|ANALYSISQUERY|Comando di Analysis Services|  
|ANALYSISCOMMAND|Query di Analysis Services|  
|Dts|[!INCLUDE[ssIS](../../includes/ssis-md.md)] esecuzione del pacchetto|  
|PowerShell|Script di PowerShell|  
  
## <a name="remarks"></a>Commenti  
 La revoca dell'accesso a un sottosistema non modifica le autorizzazione per l'entità specificata nel proxy.  
  
> [!NOTE]  
>  Per determinare i passaggi del processo che fanno riferimento a un proxy, fare clic con il pulsante destro del mouse sul nodo **proxy** in **SQL Server Agent** in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , quindi scegliere **Proprietà**. Nella finestra di dialogo **Proprietà account proxy** selezionare la pagina **riferimenti** per visualizzare tutti i passaggi del processo che fanno riferimento a questo proxy.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_revoke_proxy_from_subsystem**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene revocato l'accesso al sottosistema [!INCLUDE[ssIS](../../includes/ssis-md.md)] per il proxy `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_proxy_from_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_name = N'Dts';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implementare SQL Server Agent sicurezza](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_grant_proxy_to_subsystem &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
