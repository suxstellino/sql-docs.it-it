---
title: Service Broker con i gruppi di disponibilità
description: Contiene informazioni sulla configurazione di Service Broker con i gruppi di disponibilità Always On di SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: 881c20e5-1c99-44eb-b393-09fc5ea0f122
author: cawrites
ms.author: chadam
ms.openlocfilehash: 521d66002a1ad3d2ead2a906dc40a87d22ae220a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642107"
---
# <a name="service-broker-with-always-on-availability-groups-sql-server"></a>Service Broker con i gruppi di disponibilità Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento sono contenute informazioni sulla configurazione di Service Broker per poter utilizzarlo con i [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
  
##  <a name="requirements-for-a-service-in-an-availability-group-to-receive-remote-messages"></a><a name="ReceiveRemoteMessages"></a> Requisiti necessari affinché i messaggi remoti vengano ricevuti da un servizio in un gruppo di disponibilità  
  
1.  **Assicurarsi che nel gruppo di disponibilità sia disponibile un listener.**  
  
     Per altre informazioni, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
2.  **Assicurarsi che l'endpoint di Service Broker sia presente e che sia configurato correttamente.**  
  
     In ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui è ospitata una replica di disponibilità per il gruppo di disponibilità, configurare l'endpoint di Service Broker come riportato di seguito:  
  
    -   Impostare LISTENER_IP su 'ALL'. Con questa impostazione vengono abilitate le connessioni in qualsiasi indirizzo IP valido associato al listener del gruppo di disponibilità.  
  
    -   Impostare la porta di Service Broker sullo stesso numero di porta in tutte le istanze del server host.  
  
        > [!TIP]  
        >  Per visualizzare il numero di porta dell'endpoint di Service Broker in un'istanza del server specifica, eseguire una query sulla colonna **port** della vista del catalogo [sys.tcp_endpoints](../../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) , dove **type_desc** = 'SERVICE_BROKER'.  
  
     Nell'esempio seguente viene creato un endpoint di Service Broker con autenticazione di Windows in cui viene utilizzata la porta predefinita di Service Broker (4022) e si è in ascolto di tutti gli indirizzi IP validi.  
  
    ```  
    CREATE ENDPOINT [SSBEndpoint]  
        STATE = STARTED  
        AS TCP  (LISTENER_PORT = 4022, LISTENER_IP = ALL )  
        FOR SERVICE_BROKER (AUTHENTICATION = WINDOWS)  
    ```  
  
     Per altre informazioni, vedere [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md).  

    > [!NOTE]  
    SQL Server Service Broker non è compatibile con le subnet multiple. Impostare `RegisterAllProvidersIP` su 0 e verificare che il cluster disponga delle autorizzazioni necessarie in DNS per l'uso di indirizzi IP statici. Per altre informazioni, vedere [Configurare il listener del gruppo di disponibilità](create-or-configure-an-availability-group-listener-sql-server.md). È possibile che Service Broker ritardi i messaggi con stato "CONVERSING" che tentano di usare un indirizzo IP non disabilitato.

3.  **Concedere l'autorizzazione CONNECT nell'endpoint.**  
  
     Concedere l'autorizzazione CONNECT per l'endpoint di Service Broker al ruolo PUBLIC o a un account di accesso.  
  
     Nell'esempio seguente viene concessa la connessione in un endpoint di Service Broker denominato `broker_endpoint` al ruolo PUBLIC.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::[broker_endpoint] TO [PUBLIC]  
    ```  
  
     Per altre informazioni, vedere [GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-transact-sql.md).  
  
4.  **Assicurarsi che msdb contenga una route AutoCreatedLocal o una route al servizio specifico.**  
  
    > [!NOTE]  
    >  Per impostazione predefinita, tutti i database utente, incluso **msdb**, contengono la route **AutoCreatedLocal**. Questa route corrisponde a qualsiasi nome di servizio e istanza di Service Broker e, tramite essa, viene specificato che il messaggio deve essere recapitato all'interno dell'istanza corrente. **AutoCreatedLocal** ha una priorità inferiore rispetto alle route che specifica esplicitamente un servizio specifico usato per comunicare con un'istanza remota.  
  
     Per informazioni sulla creazione di route, vedere [Esempi di routing di Service Broker](https://msdn.microsoft.com/library/ms166090\(SQL.105\).aspx) nella versione [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] della documentazione online e [CREATE ROUTE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-route-transact-sql.md).  
  
##  <a name="requirements-for-sending-messages-to-a-remote-service-in-an-availability-group"></a><a name="SendRemoteMessages"></a> Requisiti per inviare messaggi a un servizio remoto in un gruppo di disponibilità  
  
1.  **Creare una route al servizio di destinazione.**  
  
     Configurare la route come riportato di seguito:  
  
    -   Impostare ADDRESS sull'indirizzo IP del listener del gruppo di disponibilità in cui è ospitato il database del servizio.  
  
    -   Impostare PORT sulla porta specificata nell'endpoint di Service Broker di ogni istanza remota di SQL Server.  
  
     Nell'esempio seguente viene creata una route denominata `RouteToTargetService` per il servizio `ISBNLookupRequestService` . La route è destinata al listener del gruppo di disponibilità, `MyAgListener`, da cui viene utilizzata la porta 4022.  
  
    ```  
    CREATE ROUTE [RouteToTargetService] WITH   
    SERVICE_NAME = 'ISBNLookupRequestService',   
    ADDRESS = 'TCP://MyAgListener:4022';  
  
    ```  
  
     Per altre informazioni, vedere [CREATE ROUTE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-route-transact-sql.md).  
  
2.  **Assicurarsi che msdb contenga una route AutoCreatedLocal o una route al servizio specifico.** Per altre informazioni, vedere [Requisiti necessari affinché i messaggi remoti vengano ricevuti da un servizio in un gruppo di disponibilità](#ReceiveRemoteMessages)più indietro in questo argomento.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
-   [CREATE ROUTE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-route-transact-sql.md)  
  
-   [GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-transact-sql.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Configurare gli account di accesso per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [SQL Server Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
