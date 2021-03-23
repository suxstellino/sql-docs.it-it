---
description: Configurazione di SQL Server Agent
title: Configurazione di SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, configuring
- accounts [SQL Server], SQL Server Agent
- SQL Server Agent, permissions
- security [SQL Server], SQL Server Agent
ms.assetid: 2e361a62-9e92-4fcd-80d7-d6960f127900
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1de20e6f195c9ad38a66eeec53f21c64cafb6cbf
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890767"
---
# <a name="configure-sql-server-agent"></a>Configurazione di SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. L'abilitazione e la disabilitazione di SQL Server Agent non sono attualmente supportate nell'istanza gestita di SQL. SQL Agent è sempre in esecuzione. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene descritto come specificare alcune opzioni di configurazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il set completo di opzioni di configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è disponibile solo in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects (SMO) o nelle stored procedure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni
  
-   Selezionare **SQL Server Agent** in Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per amministrare processi, operatori, avvisi e il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio Agent. Tuttavia, in Esplora oggetti viene visualizzato il nodo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent solo se si dispone dell'autorizzazione a utilizzarlo.
  
-   Non è possibile abilitare il riavvio automatico per il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent nelle istanze del cluster di failover.
  
### <a name="security"></a><a name="Security"></a>Sicurezza
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni
Per la corretta esecuzione delle funzioni, è necessario che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia configurato per utilizzare le credenziali di un account membro del ruolo predefinito del server **sysadmin** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'account deve disporre delle autorizzazioni di Windows seguenti:  
  
-   Accesso come servizio (SeServiceLogonRight)
  
-   Sostituzione di token a livello di processo (SeAssignPrimaryTokenPrivilege)
  
-   Ignorare controllo incrociato (SeChangeNotifyPrivilege)
  
-   Regolazione quote di memoria per un processo (SeIncreaseQuotaPrivilege)
  
Per altre informazioni sulle autorizzazioni di Windows necessarie per l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, vedere [Selezionare un account per il servizio SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) e [Impostazione di account di servizio Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).
  
## <a name="to-configure-sql-server-agent"></a>Per configurare SQL Server Agent

1. Selezionare il pulsante **Start** , quindi scegliere **Pannello di controllo** dal menu **Start** .  
2. Nel pannello di controllo selezionare **sistema e sicurezza**, selezionare **strumenti di amministrazione**, quindi **criteri di sicurezza locali**.
3. In criteri di sicurezza locali selezionare la freccia di espansione per espandere la cartella **criteri locali** , quindi selezionare la cartella **assegnazione diritti utente** .
4. Fare clic con il pulsante destro del mouse su un'autorizzazione da configurare per l'uso con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e selezionare **Proprietà**.
5. Nella finestra di dialogo delle proprietà dell'autorizzazione, verificare che sia elencato l'account con cui viene eseguito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. In caso contrario, selezionare **Aggiungi utente o gruppo**, immettere l'account con cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito Agent nella finestra di dialogo **Seleziona utenti, computer, account di servizio o gruppi** e quindi fare clic su **OK**.
6. Ripetere per ogni autorizzazione da aggiungere per l'esecuzione con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Al termine, selezionare **OK**.
