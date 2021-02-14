---
title: Endpoint del mirroring del database (SQL Server) | Microsoft Docs
description: Informazioni su un endpoint di mirroring del database dedicato, necessario affinché ogni server partecipi a gruppi di disponibilità Always On o al mirroring dei database.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], AlwaysOn Availability Groups
- endpoints [SQL Server], database mirroring
- Availability Groups [SQL Server], endpoint
ms.assetid: 39332dc5-678e-4650-9217-6aa3cdc41635
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ae513dd7def7acdf43198e2dbea9c4438ed74159
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352208"
---
# <a name="the-database-mirroring-endpoint-sql-server"></a>Endpoint del mirroring del database (SQL Server)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Per fare parte del mirroring del database e di [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] , un'istanza del server richiede un *endpoint del mirroring del database* dedicato. Si tratta di un endpoint speciale utilizzato solo per ricevere connessioni da altre istanze del server. In un'istanza del server specificata, ogni connessione del mirroring del database o di [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] a qualsiasi altra istanza del server utilizza un solo endpoint del mirroring di database.  
  
 Gli endpoint del mirroring del database utilizzano il protocollo TCP (Transmission Control Protocol) per inviare e ricevere messaggi tra istanze del server che fanno parte di sessioni di mirroring del database o ospitano repliche di disponibilità. L'endpoint del mirroring del database è in attesa su un numero di porta TCP univoco.  
  
> [!NOTE]  
>  Per le connessioni client a un server principale o una replica primaria non viene utilizzato l'endpoint del mirroring del database.  
  
> [!NOTE]  
>  La funzionalità del mirroring di database verrà rimossa in una delle prossime versioni di Microsoft SQL Server. Evitare di utilizzare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata in modo da poter utilizzare [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .  
  
  
##  <a name="server-network-address"></a><a name="ServerNetworkAddress"></a> Indirizzo di rete del server  
 L'indirizzo di rete di un'istanza del server, ovvero l' *indirizzo di rete del server* o l' *URL endpoint*, contiene il numero di porta del relativo endpoint e il nome di sistema e di dominio del relativo computer host. Il numero di porta identifica in modo univoco un'istanza del server specifica.  
  
 Nella figura seguente viene illustrato come sia possibile identificare in modo univoco due istanze nello stesso server. L'indirizzo di rete del server di entrambe le istanze contiene lo stesso nome di sistema, `MYSYSTEM`, e lo stesso nome di dominio, `Adventure-Works.MyDomain.com`. Per consentire al sistema di eseguire il routing delle connessioni a un'istanza del server, un indirizzo di rete del server include il numero di porta associato all'endpoint del mirroring di un'istanza del server specifica.  
  
 ![Indirizzi di rete del server per un'istanza predefinita](../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Indirizzi di rete del server per un'istanza predefinita")  
  
 Per impostazione predefinita, un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non contiene endpoint del mirroring del database. Tali endpoint devono essere creati manualmente durante l'impostazione di una sessione di mirroring del database. L'amministratore di sistema dovrà creare un endpoint separato per ogni istanza del server che deve partecipare al mirroring del database. Se in un computer specifico più istanze del server richiedono un endpoint del mirroring di database, specificare un numero di porta diverso per ogni endpoint.  
  
> [!IMPORTANT]  
>  Se nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è presente un firewall, esso dovrà essere configurato in modo da consentire sia le connessioni in ingresso che quelle in uscita per la porta specificata nell'endpoint.  
  
 Per il mirroring del database e [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], l'autenticazione e la crittografia devono essere configurate nell'endpoint. Per altre informazioni, vedere [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
> [!IMPORTANT]  
>  Non riconfigurare un endpoint del mirroring del database in uso. Le istanze del server utilizzano reciprocamente i propri endpoint per apprendere lo stato degli altri sistemi. La riconfigurazione dell'endpoint potrebbe determinarne il riavvio, situazione che alle altre istanze del server potrebbe apparire come un errore. Questa indicazione è particolarmente importante per la modalità con failover automatico, nella quale la riconfigurazione dell'endpoint in un partner potrebbe causare il verificarsi di un failover.  
  
  
##  <a name="determining-the-authentication-type-for-a-database-mirroring-endpoint"></a><a name="EndpointAuthenticationTypes"></a> Determinazione del tipo di autenticazione per un endpoint del mirroring del database  
 È importante comprendere che gli account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] delle istanze del server determinano il tipo di autenticazione che è possibile utilizzare per gli endpoint del mirroring del database, come indicato di seguito:  
  
-   Se ogni istanza del server è in esecuzione con un account di servizio del dominio, è possibile utilizzare l'autenticazione di Windows per gli endpoint del mirroring del database. Se tutte le istanze del server vengono eseguite con lo stesso account utente di dominio, gli account di accesso utente corretti saranno disponibili automaticamente in entrambi i database **master** . Questa scelta semplifica la configurazione della sicurezza per i database di disponibilità ed è quella consigliata.  
  
     Se ogni istanza del server che ospita le repliche di disponibilità per un gruppo di disponibilità viene eseguita con account diversi, è necessario creare l'accesso per ogni account nel database **master** dell'altra istanza del server. È necessario che l'account di accesso disponga dell'autorizzazione CONNECT per connettersi all'endpoint del mirroring del database dell'istanza del server. Per altre informazioni, vedere [Configurare gli account di accesso per il mirroring del database o i gruppi di disponibilità AlwaysOn (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
     Se le istanze del server utilizzano l'autenticazione di Windows, è possibile creare gli endpoint del mirroring di database tramite [!INCLUDE[tsql](../../includes/tsql-md.md)], PowerShell o la Creazione guidata Gruppo di disponibilità.  
  
    > [!NOTE]  
    >  Se un'istanza del server che deve ospitare una replica di disponibilità non dispone di un endpoint del mirroring del database, è possibile utilizzare la Creazione guidata Gruppo di disponibilità per creare automaticamente un endpoint del mirroring del database che utilizza l'autenticazione di Windows. Per altre informazioni, vedere [Utilizzare la Creazione guidata Gruppo di disponibilità (SQL Server Management Studio)](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
-   Se un'istanza del server viene eseguita con un account predefinito, ad esempio Sistema locale, Servizio locale o Servizio di rete, oppure con un account non di dominio, è necessario utilizzare certificati per l'autenticazione dell'endpoint. Se si utilizzano certificati per gli endpoint del mirroring del database, l'amministratore di sistema deve configurare ogni istanza del server per l'utilizzo dei certificati sia nelle connessioni in uscita che in quelle in ingresso.  
  
     Non è disponibile alcun metodo automatico per la configurazione della sicurezza del mirroring del database tramite certificati. È necessario usare l'istruzione di [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE ENDPOINT o il cmdlet di PowerShell **New-SqlHadrEndpoint** . Per altre informazioni, vedere [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md). Per informazioni su come abilitare l'autenticazione del certificato in un'istanza del server, vedere [Utilizzare certificati per un endpoint del mirroring del database (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare un endpoint del mirroring del database**  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Utilizzare certificati per un endpoint del mirroring del database (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in entrata (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Specificare un indirizzo di rete del server (Mirroring del database)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità (SQL Server)](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Usare la Creazione guidata Gruppo di disponibilità (SQL Server Management Studio)](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
 **Per visualizzare informazioni sull'endpoint del mirroring del database**  
  
-   [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Risolvere i problemi relativi alla configurazione del mirroring del database (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [sys.dm_hadr_availability_replica_states (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)   
 [sys.dm_db_mirroring_connections (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)  
  
  
