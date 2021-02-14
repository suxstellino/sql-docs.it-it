---
title: Configurare un server di controllo del mirroring
description: "Descrive come configurare un server di controllo del mirroring del database con l'autenticazione di Windows tramite Transact-SQL. "
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- Windows authentication [SQL Server]
- database mirroring [SQL Server], witness
ms.assetid: bf5e87df-91a4-49f9-ae88-2a6dcf644510
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a5702769994efd65abeeb330205a2a83362fee6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353255"
---
# <a name="add-a-database-mirroring-witness-using-windows-authentication-transact-sql"></a>Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per installare un server di controllo per un database, il proprietario del database assegna un'istanza di Motore di database al ruolo di server di controllo. L'istanza del server di controllo può essere eseguita sullo stesso computer dell'istanza del server principale o mirror, ma questo riduce in modo significativo l'affidabilità del failover automatico.  
  
 È fortemente consigliabile che il server di controllo risieda su un computer separato. Un determinato server può prendere parte a più sessioni di mirroring del database simultanee con lo stesso o diversi partner. Un determinato server può essere partner in alcune sessioni e server di controllo in altre.  
  
 Il server di controllo del mirroring è destinato unicamente alla modalità a sicurezza elevata con failover automatico. Prima di impostare un server di controllo del mirroring, è consigliabile verificare che la proprietà SAFETY sia impostata su FULL.  
  
> [!IMPORTANT]  
>  È consigliabile configurare il mirroring del database durante le fasce orarie di minore attività, in quanto la configurazione può influire sulle prestazioni.  
  
## <a name="establish-a-witness"></a>Creare un server di controllo  
  
1.  Sull'istanza del server di controllo, assicurarsi che sia presente un endpoint per il mirroring del database. Indipendentemente dal numero di sessioni di mirroring da supportare, è necessario che l'istanza del server disponga di un unico endpoint di mirroring del database. Se si desidera usare l'istanza del server esclusivamente come server di controllo nelle sessioni di mirroring del database, assegnare il ruolo di server di controllo all'endpoint (ROLE **=** WITNESS). Se si desidera utilizzare l'istanza del server come partner in una o più sessioni di mirroring del database, assegnare il ruolo di server dell'endpoint come ALL.  
  
     Per eseguire un'istruzione SET WITNESS, è necessario che la sessione di mirroring del database sia già iniziata (tra i partner) e che il valore STATE dell'endpoint del server di controllo sia impostato su STARTED.  
  
     Per informazioni sulle situazioni in cui l'istanza del server di controllo del mirroring dispone di un endpoint del mirroring del database e sul suo ruolo e stato, eseguire l'istruzione Transact-SQL seguente sull'istanza:  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Se un endpoint di mirroring del database è presente e già in uso, è consigliabile utilizzarlo per ogni sessione sull'istanza del server. L'eliminazione di un endpoint in uso determina la chiusura di tutte le connessioni delle sessioni esistenti. Se un server di controllo del mirroring è stato impostato per una sessione, l'eliminazione dell'endpoint del mirroring del database può determinare la perdita del quorum da parte del server principale della sessione. Se questo si verifica, il database viene portato offline e i suoi utenti vengono disconnessi. Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Se il server di controllo manca di un endpoint, vedere [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
2.  Se le istanze dei partner sono in esecuzione con account utente di dominio diversi, creare un account di accesso per i diversi account sul database master di ogni istanza. Per altre informazioni, vedere [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
3.  Connettersi al server principale ed eseguire la seguente istruzione:  
  
     ALTER DATABASE *<database_name>* SET WITNESS **=** _<server_network_address>_  
  
     dove *<database_name>* è il nome del database di cui eseguire il mirroring (tale nome è lo stesso per entrambi i partner) e *<server_network_address>* è l'indirizzo di rete dell'istanza del server di controllo del mirroring.  
  
     La sintassi per un indirizzo di rete del server presenta la seguente struttura:  
  
     TCP <b>://</b> _\<system-address>_ <b>:</b> _\<port>_  
  
     dove \<*system-address>* è una stringa che identifica in maniera univoca il computer di destinazione e \<*port>* è il numero di porta usato dall'endpoint del mirroring dell'istanza del server partner. Per altre informazioni, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Ad esempio, sull'istanza del server principale l'istruzione ALTER DATABASE seguente imposta il server di controllo del mirroring. Il nome del database è **AdventureWorks**, l'indirizzo del sistema è DBSERVER3, ovvero il nome del server di controllo del mirroring, e la porta usata dall'endpoint del mirroring del database del server di controllo del mirroring è `7022`:  
  
    ```  
    ALTER DATABASE AdventureWorks   
      SET WITNESS = 'TCP://DBSERVER3:7022'  
    ```  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene creato un server di controllo del mirroring dei dati. Sull'istanza del server di controllo (istanza predefinita in `WITNESSHOST4`):  
  
1.  Creare un endpoint per questa istanza del server solo per il ruolo WITNESS utilizzando la porta `7022`.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    ```  
  
2.  Creare un account di accesso per l'account utente di dominio delle istanze dei partner, se diverso. Ad esempio, supporre che il server di controllo del mirroring sia in esecuzione come `SOMEDOMAIN\witnessuser`, ma che i partner siano in esecuzione come `MYDOMAIN\dbousername`. Creare un account di accesso per i partner, come segue:  
  
    ```  
    --Create a login for the partner server instances,  
    --which are both running as MYDOMAIN\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [MYDOMAIN\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [MYDOMAIN\dbousername];  
    GO  
    ```  
  
3.  Su ciascuna delle istanze del server partner creare un account di accesso per l'istanza del server di controllo:  
  
    ```  
    --Create a login for the witness server instance,  
    --which is running as SOMEDOMAIN\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [SOMEDOMAIN\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [SOMEDOMAIN\witnessuser];  
    GO  
    ```  
  
4.  Sul server principale impostare il server di controllo del mirroring, che si trova in `WITNESSHOST4`:  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4:7022'  
    GO  
    ```  
  
> [!NOTE]  
>  L'indirizzo di rete del server indica l'istanza del server di destinazione attraverso il numero di porta, che esegue il mapping dell'endpoint di mirroring dell'istanza.  
  
 Per un esempio completo che illustri le impostazioni relative alla sicurezza e ai partner, nonché l'aggiunta di un server di controllo del mirroring, vedere [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)   
 [Rimuovere il server di controllo del mirroring da una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)   
 [Server di controllo del mirroring del database](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
