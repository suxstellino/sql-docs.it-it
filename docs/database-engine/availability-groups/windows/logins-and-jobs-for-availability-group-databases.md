---
title: Gestire gli account di accesso dei processi per i database in un gruppo di disponibilità
description: Descrizione delle modalità di gestione degli account di accesso per i processi che usano database che fanno parte di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: d7da14d3-848c-44d4-8e49-d536a1158a61
author: cawrites
ms.author: chadam
ms.openlocfilehash: 80f55802e887e5f5064a454d3f59532e86ac144b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644210"
---
# <a name="manage-logins-for-jobs-using-databases-in-an-always-on-availability-group"></a>Gestire gli account di accesso per i processi che usano i database di un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  È necessario gestire periodicamente lo stesso set di account di accesso utente e processi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent in ogni database primario di un gruppo di disponibilità AlwaysOn e nei database secondari corrispondenti. Gli account di accesso e i processi devono essere riprodotti in ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui è ospitata una replica di disponibilità per il gruppo di disponibilità.  
  
-   **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Processi agente**  
  
     È necessario copiare manualmente i processi rilevanti dall'istanza del server in cui è ospitata la replica primaria originale alle istanze del server in cui sono ospitate le repliche secondarie originali. Per tutti i database è necessario aggiungere logica all'inizio di ogni processo rilevante affinché il processo venga eseguito solo nel database primario, ovvero solo se la replica locale è la replica primaria del database.  
  
     Le istanze del server che ospitano le repliche di disponibilità di un gruppo di disponibilità potrebbero essere configurate in modo diverso, con lettere di unità nastro diverse e così via. I processi per ogni replica di disponibilità devono supportare eventuali differenze di questo tipo.  
  
     I processi di backup possono usare la funzione [sys.fn_hadr_is_preferred_backup_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) per identificare se la replica locale è quella preferita per i backup, in base alle preferenze di backup del gruppo di disponibilità. I processi di backup creati tramite la [Creazione guidata piano di manutenzione](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md) a livello nativo usano questa funzione. Per altri processi di backup, è consigliabile utilizzare questa funzione come condizione nei processi di backup, pertanto vengono eseguiti solo nella replica preferita. Per altre informazioni, vedere [Repliche secondarie attive: Backup in repliche secondarie &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
-   **Account di accesso**  
  
     Se si utilizzano database indipendenti, è possibile configurare utenti indipendenti nei database per i quali non è necessario creare account di accesso nelle istanze del server che ospitano una replica secondaria. Per un database di disponibilità non indipendente, sarà necessario creare utenti per gli account di accesso nelle istanze del server che ospitano le repliche di disponibilità. Per altre informazioni, vedere [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md).  
  
     Se una o più applicazioni usano l'autenticazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o un account Windows locale, vedere [Account di accesso di applicazioni in cui viene utilizzata l'autenticazione di SQL Server o un account di accesso di Windows locale](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md#SSauthentication), più avanti in questo argomento.  
  
    > [!NOTE]  
    >  Un utente del database il cui account di accesso di SQL Server non è definito o è definito in modo errato in un'istanza del server non potrà accedere a tale istanza. Questo utente viene definito *utente orfano* del database nell'istanza del server. Se un utente è isolato in una determinata istanza del server, è possibile impostare account di accesso utente in qualsiasi momento. Per altre informazioni, vedere [Risolvere i problemi relativi agli utenti isolati &#40;SQL Server&#41;](../../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md).  
  
-   **Metadati aggiuntivi**  
  
     Gli account di accesso e i processi non sono le uniche informazioni che è necessario ricreare in ogni istanza del server in cui è ospitata una replica secondaria per uno specifico gruppo di disponibilità. Potrebbe ad esempio essere necessario ricreare le impostazioni di configurazione del server, credenziali, dati crittografati, autorizzazioni, impostazioni di replica, applicazioni di Service Broker, trigger (a livello di server) e così via. Per altre informazioni, vedere [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="logins-of-applications-that-use-sql-server-authentication-or-a-local-windows-login"></a><a name="SSauthentication"></a> Account di accesso di applicazioni in cui viene utilizzata l'autenticazione di SQL Server o un account di accesso di Windows locale  
 Se in un'applicazione viene utilizzata l'autenticazione di SQL Server o un account di accesso di Windows locale, i SID non corrispondenti possono impedire la risoluzione in un'istanza remota di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]da parte dell'account di accesso dell'applicazione. In caso di SID non corrispondenti, l'account di accesso diventa un utente orfano nell'istanza del server remoto. Questo problema si può verificare quando tramite un'applicazione si effettua la connessione a un database di log shipping o con mirroring dopo un failover o a un database Sottoscrittore di replica inizializzato da un backup.  
  
 Per evitare questo problema, è consigliabile intraprendere misure preventive quando si configura un'applicazione di questo tipo per utilizzare un database ospitato da un'istanza remota di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La prevenzione comporta il trasferimento degli account di accesso e delle password dall'istanza locale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] all'istanza remota di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per altre informazioni su come evitare questo problema, vedere l'articolo della Knowledge Base 918992 relativo alla [modalità di trasferimento degli account di accesso e delle password tra le istanze di SQL Server](https://support.microsoft.com/kb/918992/).  
  
> [!NOTE]  
>  Questo problema influisce sugli account di Windows locali in computer diversi. Tuttavia, non si verifica in caso di account di dominio, dal momento che il SID è identico in ogni computer.  
  
 Per altre informazioni, vedere la pagina relativa agli [utenti orfani con log shipping e mirroring del database](/archive/blogs/sqlserverfaq/orphaned-users-with-database-mirroring-and-log-shipping) (blog del motore di database).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creare un account di accesso](../../../relational-databases/security/authentication-access/create-a-login.md)  
  
-   [Creare un utente del database](../../../relational-databases/security/authentication-access/create-a-database-user.md).  
  
-   [Creazione di un processo](../../../ssms/agent/create-a-job.md)  
  
-   [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Database indipendenti](../../../relational-databases/databases/contained-databases.md)   
 [Crea processi](../../../ssms/agent/create-jobs.md)  
  
