---
title: 'Esempio: Mirroring del database con certificati (T-SQL)'
description: Questo articolo contiene un esempio di configurazione del mirroring di database di SQL Server con l'autenticazione basata su certificati usando Transact-SQL in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- certificates [SQL Server], database mirroring
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: df489ecd-deee-465c-a26a-6d1bef6d7b66
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 12459a76f612ba2e9acc915acf5189792512b2ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342341"
---
# <a name="example-setting-up-database-mirroring-using-certificates-transact-sql"></a>Esempio: Configurazione del mirroring del database tramite certificati (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo esempio vengono illustrati tutti i passaggi necessari per la creazione di una sessione di mirroring del database tramite l'autenticazione basata sui certificati. Negli esempi di questo argomento viene utilizzato [!INCLUDE[tsql](../../includes/tsql-md.md)]. A meno che la sicurezza della rete in uso non sia già garantita, è consigliabile utilizzare la crittografia per le connessioni per il mirroring del database.  
  
 Quando si copia un certificato in un altro sistema, utilizzare un metodo di copia sicuro. È estremamente importante garantire la protezione di tutti i certificati.  
  
##  <a name="example"></a><a name="ExampleH2"></a>Esempio  
 Nell'esempio seguente vengono descritte le operazioni necessarie in un partner che risiede in HOST_A. Nell'esempio i due partner sono le istanze predefinite del server in tre sistemi. Le due istanze server vengono eseguite in domini Windows non trusted, quindi è necessaria l'autenticazione basata sui certificati.  
  
 Il ruolo principale viene inizialmente assunto da HOST_A e il ruolo di server mirror da HOST_B.  
  
 La configurazione del mirroring del database eseguita con i certificati è costituita da quattro fasi generali. Le fasi 1, 2 e 4 vengono illustrate in questo esempio. Le fasi sono le seguenti:  
  
1.  [Configurazione delle connessioni in uscita](#ConfiguringOutboundConnections)  
  
     Nell'esempio riportato di seguito vengono illustrate le fasi seguenti:  
  
    1.  Configurazione di Host_A per le connessioni in uscita.  
  
    2.  Configurazione di Host_B per le connessioni in uscita.  
  
     Per informazioni su questa fase di configurazione del mirroring del database, vedere [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  [Configurazione delle connessioni in ingresso](#ConfigureInboundConnections)  
  
     Nell'esempio riportato di seguito vengono illustrate le fasi seguenti:  
  
    1.  Configurazione di Host_A per le connessioni in ingresso.  
  
    2.  Configurazione di Host_B per le connessioni in ingresso.  
  
     Per informazioni su questa fase di configurazione del mirroring del database, vedere [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in ingresso &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
3.  Creazione del database mirror  
  
     Per informazioni su come creare un database mirror, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
4.  [Configurazione dei partner del mirroring](#ConfigureMirroringPartners)  
  
###  <a name="configuring-outbound-connections"></a><a name="ConfiguringOutboundConnections"></a> Configurazione delle connessioni in uscita  
 **Per configurare Host_A per le connessioni in uscita**  
  
1.  Nel database master creare la chiave master del database, se necessaria.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
2.  Creare un certificato per questa istanza del server.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate';  
    GO  
    ```  
  
3.  Creare un endpoint del mirroring per l'istanza del server che utilizza il certificato.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Eseguire il backup del certificato di HOST_A e copiarlo nell'altro sistema, HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
5.  Utilizzando un metodo di copia sicuro, copiare C:\HOST_A_cert.cer in HOST_B.  
  
 **Per configurare Host_B per le connessioni in uscita**  
  
1.  Nel database master creare la chiave master del database, se necessaria.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
    GO  
    ```  
  
2.  Creare un certificato per l'istanza del server in HOST_B.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert   
       WITH SUBJECT = 'HOST_B certificate for database mirroring';  
    GO  
    ```  
  
3.  Creare un endpoint del mirroring per l'istanza del server in HOST_B.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_B_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Eseguire il backup del certificato di HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
    GO   
    ```  
  
5.  Utilizzando un metodo di copia sicuro, copiare C:\HOST_B_cert.cer in HOST_A.  
  
 Per altre informazioni, vedere [Impostare l'endpoint del mirroring del database per l'uso di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 [&#91;Inizio dell'esempio &#93;](#ExampleH2)  
  
###  <a name="configuring-inbound-connections"></a><a name="ConfigureInboundConnections"></a> Configurazione delle connessioni in ingresso  
 **Per configurare Host_A per le connessioni in ingresso**  
  
1.  In HOST_A creare un account di accesso per HOST_B.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
2.  --Creare un utente per tale account di accesso.  
  
    ```  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
3.  --Associare il certificato all'utente.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
4.  Concedere l'autorizzazione CONNECT per l'account di accesso per l'endpoint del mirroring remoto.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
 **Per configurare Host_B per le connessioni in ingresso**  
  
1.  In HOST_B creare un account di accesso per HOST_A.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_A_login WITH PASSWORD = '=Sample#2_Strong_Password2';  
    GO  
    ```  
  
2.  Creare un utente per tale account di accesso.  
  
    ```  
    CREATE USER HOST_A_user FOR LOGIN HOST_A_login;  
    GO  
    ```  
  
3.  Associare il certificato all'utente.  
  
    ```  
    CREATE CERTIFICATE HOST_A_cert  
       AUTHORIZATION HOST_A_user  
       FROM FILE = 'C:\HOST_A_cert.cer'  
    GO  
    ```  
  
4.  Concedere l'autorizzazione CONNECT per l'account di accesso per l'endpoint del mirroring remoto.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_A_login];  
    GO  
    ```  
  
> [!IMPORTANT]  
>  Se si prevede di utilizzare la modalità a sicurezza elevata con failover automatico, è necessario ripetere gli stessi passaggi di impostazione per configurare il server di controllo del mirroring per connessioni in uscita e in ingresso. Per configurare le connessioni in ingresso per un server di controllo del mirroring è necessario impostare account di accesso e utenti per il server di controllo del mirroring in entrambi i partner e per entrambi i partner nel server di controllo del mirroring.  
  
 Per altre informazioni, vedere [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in ingresso &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 [&#91;Inizio dell'esempio &#93;](#ExampleH2)  
  
### <a name="creating-the-mirror-database"></a>Creazione del database mirror  
 Per informazioni su come creare un database mirror, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
###  <a name="configuring-the-mirroring-partners"></a><a name="ConfigureMirroringPartners"></a> Configurazione dei partner del mirroring  
  
1.  Nell'istanza del server mirror in HOST_B, impostare l'istanza del server in HOST_A come partner (rendendola l'istanza iniziale del server principale). Specificare un indirizzo di rete valido per `TCP://HOST_A.Mydomain.Corp.Adventure-Works``.com:7024`. Per altre informazioni, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
    ```  
    --At HOST_B, set server instance on HOST_A as partner (principal server):  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_A.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
2.  Nell'istanza del server principale in HOST_A, impostare l'istanza del server in HOST_B come partner (rendendola l'istanza iniziale del server mirror). Specificare un indirizzo di rete valido per `TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024`.  
  
    ```  
    --At HOST_A, set server instance on HOST_B as partner (mirror server).  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
3.  In questo esempio si presuppone che la sessione verrà eseguita nella modalità a prestazioni elevate. Per configurare la sessione per la modalità a prestazioni elevate, nell'istanza del server principale (in HOST_A), impostare la protezione delle transazioni su OFF.  
  
    ```  
    --Change to high-performance mode by turning off transacton safety.  
    ALTER DATABASE AdventureWorks   
        SET PARTNER SAFETY OFF  
    GO  
    ```  
  
    > [!NOTE]  
    >  Se si prevede di usare la modalità a sicurezza elevata con failover automatico, lasciare la sicurezza delle transazioni impostata su FULL (impostazione predefinita) e aggiungere il server di controllo del mirroring non appena possibile dopo l'esecuzione della seconda istruzione SET PARTNER **'** _partner_server_ **'** . Si osservi che è necessario configurare prima il server di controllo del mirroring per le connessioni in uscita e in ingresso.  
  
 [&#91;Inizio dell'esempio &#93;](#ExampleH2)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in ingresso &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Gestione di account di accesso e di processi dopo un cambio di ruolo &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
-   [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md) (SQL Server)  
  
-   [Risolvere i problemi relativi alla configurazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Usare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Centro sicurezza per il motore di Database di SQL Server e il Database SQL di Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
