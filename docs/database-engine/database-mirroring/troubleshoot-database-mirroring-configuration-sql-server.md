---
title: Problemi comuni durante la configurazione del mirroring del database
description: Contiene informazioni sulla risoluzione dei problemi relativi alla configurazione di una sessione di mirroring del database.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- endpoints [SQL Server], database mirroring
- database mirroring [SQL Server], troubleshooting
- troubleshooting [SQL Server], database mirroring
ms.assetid: 87d3801b-dc52-419e-9316-8b1f1490946c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2b50f06d3830a8f723d9d6ecc570125ecafb136a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641540"
---
# <a name="troubleshoot-database-mirroring-configuration-sql-server"></a>Risolvere i problemi relativi alla configurazione del mirroring del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi all'impostazione di una sessione di mirroring del database.  
  
> [!NOTE]  
>  Assicurarsi che vengano soddisfatti tutti i [prerequisiti per il mirroring del database](../../database-engine/database-mirroring/prerequisites-restrictions-and-recommendations-for-database-mirroring.md).  
  
|Problema|Summary|  
|-----------|-------------|  
|Messaggio di errore 1418|Questo messaggio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indica che l'indirizzo di rete del server non è raggiungibile o non esiste, pertanto si consiglia di controllare il nome dell'indirizzo di rete e quindi eseguire nuovamente il comando. |  
|[Accounts](#Accounts)|Illustra i requisiti per la corretta configurazione degli account in cui viene eseguito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Endpoints](#Endpoints)|Illustra i requisiti per la corretta configurazione dell'endpoint di mirroring del database di ogni istanza del server.|  
|[SystemAddress](#SystemAddress)|Riepiloga le alternative per la specifica del nome di sistema di un'istanza del server in una configurazione di mirroring del database.|  
|[Accesso alla rete](#NetworkAccess)|Documenta il requisito in base a cui ogni istanza del server deve essere in grado di accedere alle porte dell'altra istanza o delle altre istanze tramite TCP.|  
|[Preparazione del database mirror](#MirrorDbPrep)|Riepiloga i requisiti per la preparazione del database mirror per consentire l'avvio del mirroring.|  
|[Operazione di creazione file non riuscita](#FailedCreateFileOp)|Descrive le attività da eseguire in seguito a un'operazione di creazione file non riuscita.|  
|[Avvio del mirroring mediante Transact-SQL](#StartDbm)|Descrive l'ordine richiesto per le istruzioni ALTER DATABASE *_database* SET PARTNER **='** __partner_ **'** .|  
|[Transazioni tra database](#CrossDbTxns)|Un failover automatico può portare a una risoluzione automatica e talvolta errata delle transazioni in dubbio. Per questo motivo, il mirroring del database non supporta transazioni tra database.|  
  
##  <a name="accounts"></a><a name="Accounts"></a> Accounts  
 È necessario configurare correttamente gli account utilizzati per l'esecuzione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
1.  Autorizzazioni corrette per gli account  
  
    1.  Per ridurre l'eventualità di una configurazione non corretta, utilizzare account eseguiti negli stessi account di dominio.  
  
    2.  Se gli account vengono eseguiti in domini diversi oppure non sono account di dominio, è necessario che un account di accesso sia creato nel database **master** dell'altro computer e disponga delle autorizzazioni CONNECT per l'endpoint. Per altre informazioni, vedere [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md). Queste informazioni si riferiscono anche all'account Servizio di rete.  
  
2.  Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito come servizio con l'account di sistema locale, è necessario utilizzare i certificati per l'autenticazione. Per ulteriori informazioni, vedere [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 È necessario configurare correttamente gli endpoint.  
  
1.  Verificare che per ogni istanza del server (server principale, server mirror e server di controllo del mirroring, se presente) sia disponibile un endpoint di mirroring del database. Per altre informazioni, vedere [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) e, a seconda della forma di autenticazione, [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) oppure [Usare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Verificare che i numeri di porta siano corretti.  
  
     Per trovare la porta attualmente associata all'endpoint del mirroring del database per un'istanza del server, usare le viste del catalogo [sys.database_mirroring_endpoints](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) e [sys.tcp_endpoints](../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) .  
  
3.  Per i problemi di impostazione del mirroring del database che sono difficili diagnosticare, è consigliabile controllare ogni istanza del server per verificare che sia in attesa sulle porte corrette.   
  
4.  Verificare che gli endpoint siano stati avviati (STATE=STARTED). A tale scopo, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente su ogni istanza del server:  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Per altre informazioni sulla colonna **state_desc**, vedere [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Per avviare un endpoint, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Per altre informazioni, vedere [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Verificare che ROLE sia corretto. A tale scopo, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] su ogni istanza del server.  
  
    ```  
    SELECT role FROM sys.database_mirroring_endpoints;  
    GO  
    ```  
  
     Per altre informazioni, vedere [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
6.  Per l'account di accesso per l'account del servizio dell'altra istanza del server è richiesta l'autorizzazione CONNECT. Verificare che l'account di accesso dell'altro server disponga dell'autorizzazione CONNECT. Per individuare gli account che dispongono dell'autorizzazione CONNECT per un endpoint, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente su ogni istanza del server:  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-address"></a><a name="SystemAddress"></a> Indirizzo di sistema  
 Quale nome di sistema di un'istanza del server in una configurazione di mirroring del database è possibile utilizzare qualsiasi nome che identifichi univocamente il sistema. L'indirizzo del server può essere un nome di sistema (se i sistemi si trovano nello stesso dominio), un nome di dominio completo o un indirizzo IP (preferibilmente un indirizzo IP statico). L'utilizzo del nome di dominio completo è una soluzione efficace. Per altre informazioni, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Ogni istanza del server deve essere in grado di accedere alle porte dell'altra istanza o delle altre istanze tramite TCP. Questo requisito è particolarmente importante quando le istanze del server appartengono a domini diversi non trusted. il che limita buona parte delle comunicazioni tra le istanze del server.  
  
##  <a name="mirror-database-preparation"></a><a name="MirrorDbPrep"></a> Mirror Database Preparation  
 Sia che si avvii il mirroring per la prima volta o che lo si avvii nuovamente dopo averlo rimosso, verificare che il database mirror sia predisposto per il mirroring.  
  
 Quando si crea il database mirror sul server mirror, assicurarsi di ripristinare il backup del database principale specificando lo stesso nome di database con l'opzione WITH NORECOVERY. È inoltre necessario applicare tutti i backup di log creati dopo l'esecuzione di tale backup, sempre tramite WITH NORECOVERY.  
  
 Se possibile, è inoltre consigliabile che il percorso del database mirror, inclusa la lettera di unità, sia identico a quello del database principale. Se i percorsi dei file sono diversi, ad esempio il database principale è disponibile nell'unità F: e tale unità non è presente nel database mirror, è necessario includere l'opzione MOVE nell'istruzione RESTORE.  
  
> [!IMPORTANT]  
>  Se durante la creazione del database mirror i file del database vengono spostati, potrebbe essere impossibile aggiungere successivamente file al database senza sospendere il mirroring.  
  
 Se il mirroring di database è stato arrestato, prima di poter riavviare il mirroring è necessario che tutti i backup di log successivi eseguiti sul database vengano applicati al database.  
  
 Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
##  <a name="failed-create-file-operation"></a><a name="FailedCreateFileOp"></a> Failed Create-File Operation  
 Per aggiungere un file senza conseguenze per la sessione di mirroring, è necessario che il percorso del file esista in entrambi i server. Pertanto, se durante la creazione del database mirror i file del database vengono spostati, potrebbe essere impossibile aggiungere successivamente file al database mirror senza sospendere il mirroring.  
  
 Per risolvere il problema:  
  
1.  Il proprietario del database deve rimuovere la sessione di mirroring e deve ripristinare un backup completo del filegroup contenente il file aggiunto.  
  
2.  Il proprietario deve quindi eseguire il backup del log contenente l'operazione di aggiunta file nel server principale e ripristinare manualmente il backup del log nel database mirror utilizzando le opzioni WITH NORECOVERY e WITH MOVE. L'esecuzione di questa operazione consente di creare il percorso del file specificato nel server mirror e di ripristinare il nuovo file in tale percorso.  
  
3.  Per preparare il database per una nuova sessione di mirroring, il proprietario deve inoltre ripristinare con l'opzione WITH NO RECOVERY tutti gli altri backup del log in sospeso dal server principale.  
  
 Per altre informazioni, vedere [Rimozione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md), [Preparare un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md), [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md), [Usare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md) oppure [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="starting-mirroring-by-using-transact-sql"></a><a name="StartDbm"></a> Avvio del mirroring mediante Transact-SQL  
 L'ordine con cui vengono rilasciate le istruzioni ALTER DATABASE *_database* SET PARTNER **='** _server_partner_ **'** è molto importante.  
  
1.  La prima istruzione deve essere eseguita sul server mirror. Quando viene eseguita questa istruzione, il server mirror non tenta di contattare altre istanze del server e indica invece al database di attendere che il server mirror venga contattato dal server principale.  
  
2.  La seconda istruzione ALTER DATABASE deve essere eseguita sul server principale e fa in modo che il server principale tenti di connettersi al server mirror. Dopo che è stata creata la connessione, il server mirror tenta di connettersi al server principale tramite un'altra connessione.  
  
 Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
> [!NOTE]  
>  Per altre informazioni sull'uso di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vedere [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="cross-database-transactions"></a><a name="CrossDbTxns"></a> Transazioni tra database  
 Quando viene eseguito il mirroring di un database in modalità a protezione elevata con failover automatico, un failover automatico può portare a una risoluzione automatica e talvolta errata delle transazioni in dubbio. Se si verifica un failover automatico su uno dei database mentre è stato eseguito il commit di una transazione tra database, possono verificarsi inconsistenze logiche tra i database.  
  
 I tipi di transazioni tra database che possono essere interessati da un failover automatico includono:  
  
-   Una transazione che aggiorna più database nella stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Le transazioni che utilizzano [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC).  
  
 Per altre informazioni, vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)  
  
  


