---
title: Configurare il mirroring del database tramite l'autenticazione di Windows (T-SQL)
description: Questo articolo contiene un esempio di creazione di una sessione di mirroring del database con un server di controllo del mirroring usando l'autenticazione di Windows con Transact-SQL in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: 35800769-aede-4aac-b077-0e0e487e302f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 50b27862389c0977abcfe5072534043458bd5b3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342308"
---
# <a name="example-configure-database-mirroring-using-windows-authentication-transact-sql"></a>Esempio: Configurare il mirroring del database usando l'autenticazione di Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo esempio vengono illustrate tutte le fasi necessarie per creare una sessione di mirroring del database con un server di controllo del mirroring con l'autenticazione di Windows. Negli esempi di questo argomento viene utilizzato [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si osservi che per impostare il mirroring del database è possibile utilizzare, in alternativa alle procedure di [!INCLUDE[tsql](../../includes/tsql-md.md)], la Configurazione guidata sicurezza mirroring del database. Per altre informazioni, vedere [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
## <a name="prerequisite"></a>Prerequisito  
 Nell'esempio viene utilizzato il database di esempio **AdventureWorks** , in cui, per impostazione predefinita, viene utilizzato il modello di recupero con registrazione minima. Per utilizzare il mirroring del database con questo database, è necessario modificarlo in modo che venga utilizzato il modello di recupero con registrazione completa. Per eseguire questa operazione in [!INCLUDE[tsql](../../includes/tsql-md.md)], utilizzare l'istruzione ALTER DATABASE, come segue:  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks   
SET RECOVERY FULL;  
GO  
```  
  
 Per informazioni sulla modifica del modello di recupero in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vedere [Visualizzazione o modifica del modello di recupero di un database &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database e l'autorizzazione CREATE ENDPOINT o l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="example"></a>Esempio  
 In questo esempio i due partner e il server di controllo del mirroring sono le istanze predefinite del server in tre sistemi. Le tre istanze del server sono eseguite nello stesso dominio Windows, ma per l'istanza del server di controllo del mirroring l'account utente (utilizzato come account del servizio di avvio) è diverso.  
  
 Nella tabella seguente sono riepilogati i valori utilizzati nell'esempio.  
  
|Ruolo di mirroring iniziale|Sistema host|Account utente di dominio|  
|----------------------------|-----------------|-------------------------|  
|Server principale|PARTNERHOST1|*\<Mydomain>\\<dbousername\>*|  
|Mirror|PARTNERHOST5|*\<Mydomain>\\<dbousername\>*|  
|Controllo|WITNESSHOST4|*\<Somedomain>\\<witnessuser\>*|  
  
1.  Creare un endpoint nell'istanza del server principale, ovvero l'istanza predefinita in PARTNERHOST1.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=PARTNER)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    -- Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
2.  Creare un endpoint nell'istanza del server mirror, ovvero l'istanza predefinita in PARTNERHOST5.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
3.  Creare un endpoint nell'istanza del server di controllo del mirroring, ovvero l'istanza predefinita in WITNESSHOST4.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    --Create a login for the partner server instances,  
    --which are both running as Mydomain\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [Mydomain\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
4.  Creare il database mirror. Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
5.  Nell'istanza del server mirror in PARTNERHOST5 impostare l'istanza del server in PARTNERHOST1 come partner, rendendola l'istanza del server principale iniziale.  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1.COM:7022'  
    GO  
    ```  
  
6.  Nell'istanza del server principale in PARTNERHOST1 impostare l'istanza del server in PARTNERHOST5 come partner, rendendola l'istanza del server mirror iniziale.  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5.COM:7022'  
    GO  
    ```  
  
7.  Nel server principale impostare il server di controllo del mirroring, che si trova in WITNESSHOST4.  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4.COM:7022'  
    GO  
    ```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Impostazione di un database mirror per l'utilizzo della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in ingresso &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Esempio: Configurazione del mirroring del database tramite certificati &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Centro sicurezza per il motore di Database di SQL Server e il Database SQL di Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
