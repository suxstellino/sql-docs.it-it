---
title: Configurare il mirroring del database
description: Procedura per configurare una relazione di mirroring del database tra un principale e un mirror usando l'autenticazione di Windows.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 143c68a5-589f-4e7f-be59-02707e1a430a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7584e4611d957f48efcb73d12b5965f0a4727b45
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642031"
---
# <a name="configure-database-mirroring"></a>Configurare il mirroring del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 Dopo la preparazione del database mirror, illustrata in [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md), è possibile stabilire una sessione di mirroring del database. È necessario che le istanze del server principale, del server mirror e del server di controllo del mirroring siano istanze di server distinte, situate in sistemi host distinti.  
  
> [!IMPORTANT]
>  È consigliabile configurare il mirroring del database durante le fasce orarie di minore attività, dato che tale configurazione può influire sulle prestazioni.  
> 
> [!NOTE]
>  Una determinata istanza del server può prendere parte a più sessioni di mirroring del database simultanee con lo stesso partner o con partner diversi. Una determinata istanza del server può essere partner in alcune sessioni e server di controllo del mirroring in altre. L'istanza del server mirror deve eseguire la stessa edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come istanza del server principale. Il mirroring del database non è disponibile in ogni edizione di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). È inoltre consigliabile che vengano eseguite in sistemi simili in grado di gestire carichi di lavoro identici.  
  
### <a name="to-establish-a-database-mirroring-session"></a>Per stabilire una sessione di mirroring del database  
  
1.  Creare il database mirror. Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
2.  Impostare la sicurezza in ogni istanza del server.  
  
     Per ogni istanza del server in una sessione di mirroring del database è necessario un endpoint di mirroring del database. Se l'endpoint non esiste, è necessario crearlo.  
  
    > [!NOTE]  
    >  La forma di autenticazione utilizzata per il mirroring del database da un'istanza del server corrisponde a una proprietà dell'endpoint del mirroring del database dell'istanza. Per il mirroring del database sono disponibili due tipi di sicurezza del trasporto: autenticazione di Windows o autenticazione basata su certificati. Per altre informazioni, vedere [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Assicurarsi che in ogni server partner server sia disponibile un endpoint per il mirroring del database. Indipendentemente dal numero di sessioni di mirroring da supportare, nell'istanza del server è consentito un solo endpoint del mirroring del database. Se si desidera usare questa istanza del server esclusivamente per i partner di sessioni di mirroring del database, è possibile assegnare il ruolo di partner all'endpoint (ROLE **=** PARTNER). Se si desidera utilizzare questo server anche per il server di controllo del mirroring in altre sessioni di mirroring del database, assegnare il ruolo dell'endpoint come ALL.  
  
     Per eseguire un'istruzione SET PARTNER, l'opzione STATE degli endpoint di entrambi i partner deve essere impostata su STARTED.  
  
     Per verificare se un'istanza del server dispone di un endpoint del mirroring del database e per informazioni sul ruolo e sullo stato relativi, in quell'istanza, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Non riconfigurare un endpoint del mirroring del database in uso. Se un endpoint di mirroring del database è presente e già in uso, è consigliabile utilizzarlo per ogni sessione sull'istanza del server. L'eliminazione di un endpoint in uso può determinare il riavvio dell'endpoint, interrompendo le connessioni delle sessioni esistenti, cosa che ad altre istanze del server potrebbe apparire come un errore. Questa indicazione è particolarmente importante in modalità a sicurezza elevata con failover automatico, nella quale la riconfigurazione dell'endpoint in un partner potrebbe causare il verificarsi di un failover. Se un server di controllo del mirroring è stato impostato per una sessione, l'eliminazione dell'endpoint del mirroring del database può inoltre determinare la perdita del quorum da parte del server principale della sessione. Se si verifica questa situazione, il database viene portato offline e i suoi utenti vengono disconnessi. Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Se un partner manca di un endpoint, vedere [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
3.  Se le istanze del server sono in esecuzione in account utente di dominio diversi, per ciascuna istanza è necessario un account di accesso nel database **master** delle altre. Se l'account di accesso non esiste, è necessario crearlo. Per altre informazioni, vedere [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
4.  Per impostare il server principale come partner sul database mirror, connettersi al server mirror ed eseguire l'istruzione seguente:  
  
     ALTER DATABASE *\<database_name\>* SET PARTNER **=** _\<server\_network\_address\>_  
  
     dove _\<database\_name\>_ è il nome del database di cui eseguire il mirroring (tale nome è lo stesso per entrambi i partner) e _\<server\_network\_address\>_ è l'indirizzo di rete del server principale.  
  
     La sintassi per un indirizzo di rete del server presenta la seguente struttura:  
  
     TCP <b>\://</b> _\<system-address\>_ <b>\:</b> _\<port\>_  
  
     dove _\<system-address>_ è una stringa che identifica in maniera univoca il computer di destinazione e _\<port>_ è il numero di porta usato dall'endpoint del mirroring dell'istanza del server partner. Per altre informazioni, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Ad esempio, sull'istanza del server mirror, l'istruzione ALTER DATABASE seguente imposta il partner come istanza del server principale originale. Il nome del database è **AdventureWorks**, l'indirizzo del sistema è DBSERVER1, ovvero il nome del sistema partner, e il numero della porta usata dall'endpoint del mirroring del database del partner è 7022:  
  
    ```  
    ALTER DATABASE AdventureWorks   
       SET PARTNER = 'TCP://DBSERVER1:7022'  
    ```  
  
     Questa istruzione prepara il server mirror per creare una sessione quando viene contattato dal server principale.  
  
5.  Per impostare il server mirror come partner sul database principale, connettersi al server principale ed eseguire l'istruzione seguente:  
  
     ALTER DATABASE _\<database\_name\>_ SET PARTNER **=** _\<server\_network\_address\>_  
  
     Per ulteriori informazioni, vedere il passaggio 4.  
  
     Ad esempio, sull'istanza del server principale, l'istruzione ALTER DATABASE seguente imposta il partner come istanza del server mirror originale. Il nome del database è **AdventureWorks**, l'indirizzo del sistema è DBSERVER2, ovvero il nome del sistema partner, e il numero della porta usata dall'endpoint del mirroring del database del partner è 7025:  
  
    ```  
    ALTER DATABASE AdventureWorks SET PARTNER = 'TCP://DBSERVER2:7022'  
    ```  
  
     L'immissione di questa istruzione sul server principale avvia la sessione di mirroring del database.  
  
6.  Per impostazione predefinita, una sessione è impostata su un livello di protezione delle transazioni completo (SAFETY è impostato su FULL), il che determina l'avvio della sessione in modalità sincrona a sicurezza elevata senza failover automatico. È possibile riconfigurare la sessione per l'esecuzione in modalità a sicurezza elevata con failover automatico o in modalità asincrona a prestazioni elevate, come riportato di seguito:  
  
    -   **Modalità a sicurezza elevata con failover automatico**  
  
         Se si desidera che una sessione in modalità a sicurezza elevata supporti il failover automatico, aggiungere un'istanza del server di controllo del mirroring. Per altre informazioni, vedere [Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
    -   **Modalità a prestazioni elevate**  
  
         In alternativa, se si desidera evitare il failover automatico e si preferisce privilegiare le prestazioni rispetto alla disponibilità, disattivare la protezione delle transazioni. Per altre informazioni, vedere [Modifica della protezione delle transazioni in una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  In modalità a prestazioni elevate, WITNESS deve essere impostato su OFF. Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
## <a name="example"></a>Esempio  
  
> [!NOTE]  
>  Nell'esempio seguente viene creata una sessione di mirroring del database tra i partner per un database mirror esistente. Per informazioni sulla creazione di un database mirror, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Nell'esempio viene illustrata la procedura di base per la creazione di una sessione di mirroring del database senza un server di controllo del mirroring. I due partner sono le istanze del server predefinite sui due sistemi di computer (PARTNERHOST1 e PARTNERHOST5). Le due istanze dei partner vengono eseguite tramite lo stesso account utente di dominio di Windows (MYDOMAIN\dbousername).  
  
1.  Nell'istanza del server principale (istanza predefinita in PARTNERHOST1) creare un endpoint che supporti tutti i ruoli utilizzando la porta 7022:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
    > [!NOTE]  
    >  Per un esempio di configurazione di un account di accesso, vedere [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
2.  Nell'istanza del server mirror (istanza predefinita in PARTNERHOST5) creare un endpoint che supporti tutti i ruoli utilizzando la porta 7022:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
3.  Nell'istanza del server principale (in PARTNERHOST1), eseguire il backup del database:  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdvWorks_dbmirror.bak'   
        WITH FORMAT  
    GO  
    ```  
  
4.  Sull'istanza del server mirror (in `PARTNERHOST5`), eseguire il ripristino del database:  
  
    ```  
    RESTORE DATABASE AdventureWorks   
        FROM DISK = 'Z:\AdvWorks_dbmirror.bak'   
        WITH NORECOVERY  
    GO  
    ```  
  
5.  Dopo aver creato il backup completo del database, è necessario creare un backup del log sul database principale. Ad esempio, l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente esegue il backup del log nello stesso file utilizzato dal backup del database precedente:  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Prima di avviare il mirroring, è necessario applicare il backup del log richiesto ed eventuali backup del log successivi.  
  
     Ad esempio, l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente ripristina il primo log da C:\AdventureWorks.bak:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\ AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Nell'istanza del server mirror impostare l'istanza del server in PARTNERHOST1 come partner, rendendola il server principale iniziale:  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1:7022'  
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  Per impostazione predefinita, una sessione di mirroring del database viene eseguita in modalità sincrona, che dipende da un livello di sicurezza delle transazioni completo (SAFETY impostato su FULL). Per fare in modo che una sessione venga eseguita in modalità asincrona a prestazioni elevate impostare SAFETY su OFF. Per altre informazioni, vedere [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
8.  Nell'istanza del server principale impostare l'istanza del server in `PARTNERHOST5` come partner, rendendola l'istanza del server mirror iniziale:  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5:7022'  
    GO  
    ```  
  
9. Facoltativamente, se si desidera utilizzare la modalità a sicurezza elevata con failover automatico, impostare l'istanza del server di controllo del mirroring. Per altre informazioni, vedere [Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
> [!NOTE]  
>  Per un esempio completo che illustri le impostazioni relative alla sicurezza e ai partner, nonché l'aggiunta di un server di controllo del mirroring, vedere [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Mirroring del database e log shipping &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Mirroring e replica del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  

