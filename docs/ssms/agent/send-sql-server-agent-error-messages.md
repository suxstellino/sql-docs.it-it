---
description: Send SQL Server Agent Error Messages
title: Send SQL Server Agent Error Messages
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- messages [SQL Server], SQL Server Agent
- sending messages
- SQL Server Agent, errors
- errors [SQL Server Agent]
ms.assetid: 2597d0d7-951a-48cf-989f-abb67b9fdb36
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7504f16114b73e40824a088368ab7647b35861ff
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251316"
---
# <a name="send-sql-server-agent-error-messages"></a>Send SQL Server Agent Error Messages
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come configurare [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per l'invio di messaggi di errore tramite Net Send in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
  
-   In Esplora oggetti viene visualizzato il nodo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent solo se si dispone dell'autorizzazione per utilizzarlo.  
  
-   Per ricevere eventi Net Send, è necessario che il servizio [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Messenger sia in esecuzione.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per la corretta esecuzione delle funzioni, è necessario che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia configurato per utilizzare le credenziali di un account membro del ruolo predefinito del server **sysadmin** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'account deve disporre delle autorizzazioni di Windows seguenti:  
  
-   Accesso come servizio (SeServiceLogonRight)  
  
-   Sostituzione di token a livello di processo (SeAssignPrimaryTokenPrivilege)  
  
-   Ignorare controllo incrociato (SeChangeNotifyPrivilege)  
  
-   Regolazione quote di memoria per un processo (SeIncreaseQuotaPrivilege)  
  
Per altre informazioni sulle autorizzazioni di Windows necessarie per l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, vedere [Selezionare un account per il servizio SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) e [Impostazione di account di servizio Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-send-sql-server-agent-error-messages"></a>Per inviare messaggi di errore di SQL Server Agent  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server contenente il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent da cui si desidera inviare i messaggi di errore tramite Net Send.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent** e scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà SQL Server Agent -** _nome\_server_ digitare in **Log degli errori** nella pagina **Generale** il nome utente o il nome computer a cui inviare i messaggi di errore nella casella **Destinatario Net Send**.  
  
4.  Fare clic su **OK**.  
