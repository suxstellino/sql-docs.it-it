---
title: Usare certificati per le connessioni in entrata
description: Illustra la procedura per configurare le istanze del server in modo da usare i certificati per l'autenticazione delle connessioni in ingresso per il mirroring del database.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- inbound connections
- database mirroring [SQL Server], security
ms.assetid: 5d48bb98-61f0-4b99-8f1a-b53f831d63d0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c1ff8414ef765459bea59af5d158a0d4bdcd6cc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349221"
---
# <a name="database-mirroring---use-certificates-for-inbound-connections"></a>Mirroring del database: utilizzo di certificati per le connessioni in entrata
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrata la procedura per configurare le istanze del server in modo da utilizzare i certificati per l'autenticazione delle connessioni in ingresso per il mirroring del database. Prima di impostare le connessioni in entrata, è necessario configurare le connessioni in uscita in ogni istanza del server. Per altre informazioni, vedere [Impostare l'endpoint del mirroring del database per l'uso di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 Il processo di configurazione delle connessioni in entrata include i passaggi generali seguenti:  
  
1.  Creare un account di accesso per l'altro sistema.  
  
2.  Creare un utente per tale account di accesso.  
  
3.  Ottenere il certificato per l'endpoint del mirroring dell'altra istanza del server.  
  
4.  Associare il certificato all'utente creato nel passaggio 2.  
  
5.  Concedere l'autorizzazione CONNECT all'account di accesso per l'endpoint del mirroring.  
  
 Se è disponibile un server di controllo del mirroring, è necessario configurare le connessioni in ingresso anche per tale server. Questa operazione prevede la configurazione di account di accesso, utenti e certificati per il server di controllo del mirroring in entrambi i partner, e viceversa.  
  
 Questa procedura descrive i vari passaggi, Per ogni passaggio la procedura fornisce un esempio di configurazione di un istanza di server in un sistema denominato HOST_A. Nella sezione di esempio corrispondente vengono indicati gli stessi passaggi per un'altra istanza di server in un sistema denominato HOST_B.  
  
### <a name="to-configure-server-instances-for-inbound-mirroring-connections-on-host_a"></a>Per configurare le istanze del server per le connessioni per il mirroring in ingresso (on HOST_A)  
  
1.  Creare un account di accesso per l'altro sistema.  
  
     Nell'esempio seguente viene creato un account di accesso per il sistema, HOST_B, nel database **master** dell'istanza del server in HOST_A. Tale account di accesso è denominato `HOST_B_login`. Sostituire la password di esempio con una password personalizzata.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login   
       WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
     Per altre informazioni, vedere [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
     Per visualizzare gli account di accesso in questa istanza del server, è possibile utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```  
    SELECT * FROM sys.server_principals  
    ```  
  
     Per altre informazioni, vedere [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
2.  Creare un utente per tale account di accesso.  
  
     Nell'esempio seguente viene creato l'utente `HOST_B_user` per l'account di accesso creato nel passaggio precedente.  
  
    ```  
    USE master;  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
     Per altre informazioni, vedere [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md).  
  
     Per visualizzare gli utenti in questa istanza del server, è possibile utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```  
    SELECT * FROM sys.sysusers;  
    ```  
  
     Per altre informazioni, vedere [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md).  
  
3.  Ottenere il certificato per l'endpoint del mirroring dell'altra istanza del server.  
  
     Se questa operazione non è già stata eseguita durante la configurazione delle connessioni in uscita, ottenere una copia del certificato per l'endpoint del mirroring dell'istanza del server remoto. A tale scopo, eseguire il backup del certificato in tale istanza del server, come illustrato in [Impostare l'endpoint del mirroring del database per l'uso di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). Quando si copia un certificato in un altro sistema, utilizzare un metodo di copia sicuro. È estremamente importante garantire la protezione di tutti i certificati.  
  
     Per altre informazioni, vedere [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
4.  Associare il certificato all'utente creato nel passaggio 2.  
  
     Nell'esempio seguente, il certificato di HOST_B viene associato al relativo utente in HOST_A.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
     Per altre informazioni, vedere [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Per visualizzare i certificati in questa istanza del server, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```  
    SELECT * FROM sys.certificates  
    ```  
  
     Per altre informazioni, vedere [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
5.  Concedere l'autorizzazione CONNECT per l'account di accesso per l'endpoint del mirroring remoto.  
  
     Per concedere l'autorizzazione in HOST_A all'istanza del server remoto in HOST_B per la connessione al relativo account di accesso locale `HOST_B_login`, ad esempio, usare le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti:  
  
    ```  
    USE master;  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
     Per altre informazioni, vedere [GRANT - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 A questo punto, è stata completata la configurazione dell'autenticazione tramite un certificato per la connessione di HOST_B a HOST_A.  
  
 È ora necessario eseguire i rispettivi passaggi in ingresso per HOST_A in HOST_B. Tali passaggi sono illustrati nella parte relativa alle connessioni in ingresso della sezione di esempio riportata più avanti.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene illustrata la configurazione di HOST_B per le connessioni in ingresso.  
  
> [!NOTE]  
>  L'esempio usa un file di certificato contenente il certificato di HOST_A creato da un frammento di codice in [Impostare l'endpoint del mirroring del database per l'uso di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
```  
USE master;  
--On HOST_B, create a login for HOST_A.  
CREATE LOGIN HOST_A_login WITH PASSWORD = 'AStrongPassword!@#';  
GO  
--Create a user, HOST_A_user, for that login.  
CREATE USER HOST_A_user FOR LOGIN HOST_A_login  
GO  
--Obtain HOST_A certificate. (See the note   
--   preceding this example.)  
--Asscociate this certificate with the user, HOST_A_user.  
CREATE CERTIFICATE HOST_A_cert  
   AUTHORIZATION HOST_A_user  
   FROM FILE = 'C:\HOST_A_cert.cer';  
GO  
--Grant CONNECT permission for the server instance on HOST_A.  
GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO HOST_A_login  
GO  
```  
  
 Se si prevede di utilizzare la modalità a sicurezza elevata con failover automatico, è necessario ripetere gli stessi passaggi di impostazione per configurare il server di controllo del mirroring per connessioni in uscita e in ingresso.  
  
 Per informazioni sulla creazione di un database mirror e un esempio di Transact-SQL, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Per un esempio di Transact-SQL per stabilire una sessione in modalità a prestazioni elevate, vedere [Esempio: Impostazione del mirroring del database tramite certificati &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>Sicurezza di .NET Framework  
 Quando si copia un certificato in un altro sistema, utilizzare un metodo di copia sicuro. È estremamente importante garantire la protezione di tutti i certificati.  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [GRANT Endpoint Permissions &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Impostare un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Risolvere i problemi relativi alla configurazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
