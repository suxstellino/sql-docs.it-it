---
description: Implementazione della sicurezza di SQL Server Agent
title: Implementazione della sicurezza di SQL Server Agent
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, security
- security [SQL Server Agent], about security
- security [SQL Server Agent]
- security [SQL Server], SQL Server Agent
ms.assetid: d770d35c-c8de-4e00-9a85-7d03f45a0f0d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7bd95527dd51981c3ba3b5d44e4beb2be6a1db80
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342669"
---
# <a name="implement-sql-server-agent-security"></a>Implementazione della sicurezza di SQL Server Agent
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent consente all'amministratore del database di eseguire ciascun passaggio di un processo in un contesto di sicurezza in cui sono disponibili solo le autorizzazioni necessarie all'esecuzione di tale passaggio e che viene determinato da un proxy di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per impostare le autorizzazioni per un particolare passaggio di un processo, è necessario creare un proxy che disponga delle autorizzazioni necessarie e quindi assegnare questo proxy al passaggio del processo. È possibile specificare un proxy per più di un passaggio di processo. Per i passaggi che richiedono le medesime autorizzazioni, viene utilizzato lo stesso proxy.  
  
Nella sezione che segue viene spiegato quale ruolo di database concedere agli utenti affinché possano creare o eseguire processi utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="granting-access-to-sql-server-agent"></a>Concessione dell'accesso a SQL Server Agent  
Per poter utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, l'utente deve essere membro di uno o più dei seguenti ruoli predefiniti di database:  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
Questi ruoli sono archiviati nel database **msdb** . Per impostazione predefinita, nessun utente è membro di questi ruoli di database. L'appartenenza a questi ruoli deve essere concessa esplicitamente. Gli utenti membri del ruolo di server predefinito **sysadmin** hanno accesso completo a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e non hanno bisogno di essere membri di questi ruoli di database predefiniti per poter usare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Se un utente non è membro di uno di questi ruoli di database o del ruolo **sysadmin** , il nodo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non è disponibile quando l'utente si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
I membri di questi ruoli di database possono visualizzare ed eseguire processi di cui sono proprietari e possono creare passaggi di processo eseguibili come un account proxy esistente. Per altre informazioni sulle autorizzazioni specifiche associate a ognuno di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
I membri del ruolo predefinito del server **sysadmin** sono autorizzati a creare, modificare ed eliminare account proxy. I membri del ruolo **sysadmin** sono autorizzati a creare passaggi di processo che non specificano un proxy ma che invece vengono eseguiti come account di servizio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, vale a dire l'account usato per avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="guidelines"></a>Indicazioni  
Per migliorare la sicurezza dell'implementazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, attenersi alle seguenti indicazioni generali:  
  
-   Creare appositi account utente dedicati per i proxy e utilizzare questi account solo per eseguire passaggi di processo.  
  
-   Concedere le autorizzazioni necessarie solo agli account utente proxy. Concedere solo le autorizzazioni effettivamente richieste per eseguire i passaggi di processo assegnati a un particolare account proxy.  
  
-   Non eseguire il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent usando un account Microsoft Windows che è membro del gruppo **Administrators** di Windows.  
  
-   I proxy hanno lo stesso livello di sicurezza dell'archivio credenziali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Se le operazioni di scrittura dell'utente possono scrivere nel registro eventi NT, possono generare avvisi tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
-   Non specificare l'account amministratore NT come account di servizio o account proxy.  
  
-   Notare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dispongono dell'accesso reciproco alle proprie risorse. I due servizi condividono un unico spazio di elaborazione e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è un sysadmin sul servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Quando un processo TSX viene integrato con un processo MSX, sysadmins di MSX assume il controllo totale dell'istanza TSX di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Il processo ACE è un'estensione e non può richiamare se stesso. ACE viene richiamato da Chainer ScenarioEngine.exe, anche noto come Microsoft.SqlServer.Chainer.Setup.exe, o può essere richiamato da un altro processo host.  
  
-   ACE dipende dalle DLL di configurazione seguenti di proprietà di SSDP, perché le API delle DLL vengono richiamate da ACE:  
  
    -   **SCO**: Microsoft.SqlServer.Configuration.Sco.dll, incluse le nuove convalide SCO per gli account virtuali  
  
    -   **Cluster**: Microsoft.SqlServer.Configuration.Cluster.dll  
  
    -   **SFC**: Microsoft.SqlServer.Configuration.SqlConfigBase.dll  
  
    -   **Extension**: Microsoft.SqlServer.Configuration.ConfigExtension.dll  
  
## <a name="see-also"></a>Vedere anche  
[Utilizzo dei ruoli predefiniti](../../reporting-services/security/role-definitions-predefined-roles.md)  
[sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
[sp_droprolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)  
[Sicurezza e protezione (Motore di database)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
