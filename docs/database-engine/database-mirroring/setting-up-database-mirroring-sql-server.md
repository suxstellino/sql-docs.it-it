---
title: Impostazione del mirroring del database (SQL Server) | Microsoft Docs
description: Informazioni sui prerequisiti, le raccomandazioni e le procedure per la configurazione del mirroring del database in SQL Server, inclusa una panoramica di una sessione di mirroring del database.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
ms.assetid: da45efed-55eb-4c71-be34-ac2589dfce8d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c085a7e1571e85e4fc3f3f9267e033e17302e09e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352280"
---
# <a name="setting-up-database-mirroring-sql-server"></a>Impostazione del mirroring del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questa sezione vengono illustrati i prerequisiti, le indicazioni e la procedura per l'impostazione del mirroring del database. Per un'introduzione al mirroring del database, vedere [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!IMPORTANT]  
>  È consigliabile configurare il mirroring del database durante le fasce orarie di minore attività, in quanto la configurazione può influire sulle prestazioni.  
  
  
##  <a name="preparing-a-server-instance-to-host-a-mirror-server"></a><a name="PrepareInstances"></a> Preparazione di un'istanza del server a ospitare un server mirror  
 Per ogni sessione di mirroring del database:  
  
1.  È necessario che il server principale, il server mirror e il server di controllo, se presente, siano ospitati in istanze di server distinte, situate in sistemi host distinti. Per ogni istanza del server è necessario un endpoint del mirroring del database. Se è necessario creare un endpoint del mirroring di database, assicurarsi che sia accessibile alle altre istanze del server.  
  
     La forma di autenticazione utilizzata per il mirroring del database da un'istanza del server corrisponde a una proprietà dell'endpoint del mirroring del database dell'istanza. Per il mirroring del database sono disponibili due tipi di sicurezza del trasporto: autenticazione di Windows o autenticazione basata su certificati. Per altre informazioni, vedere [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     I requisiti per l'accesso alla rete sono specifici della forma di autenticazione, come segue:  
  
    -   **In caso di utilizzo dell'autenticazione di Windows**  
  
         Se le istanze del server sono in esecuzione in account utente di dominio diversi, per ciascuna istanza è necessario un account di accesso nel database **master** delle altre. Se l'account di accesso non esiste, è necessario crearlo. Per altre informazioni, vedere [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
    -   **In caso di utilizzo di certificati**  
  
         Per abilitare l'autenticazione del certificato per il mirroring del database in una determinata istanza del server, l'amministratore di sistema deve configurare ogni istanza del server per l'utilizzo dei certificati nelle connessioni in uscita e in ingresso. Le connessioni in uscita devono essere configurate per prime. Per ulteriori informazioni, vedere [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Verificare che nel server mirror siano presenti account di accesso per tutti gli utenti del database. Per altre informazioni, vedere [Configurare gli account di accesso per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
3.  Sull'istanza del server che ospiterà il database mirror, configurare il resto dell'ambiente richiesto per il database con mirroring. Per altre informazioni, vedere [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="overview-establishing-a-database-mirroring-session"></a><a name="EstablishUsingWinAuthentication"></a> Panoramica: apertura di una sessione di mirroring del database  
 I passaggi di base per stabilire una sessione di mirroring sono i seguenti:  
  
1.  Creare il database mirror ripristinando i backup seguenti, utilizzando RESTORE WITH NORECOVERY per ogni operazione di ripristino:  
  
    1.  Ripristinare un backup completo recente del database principale dopo aver verificato che il database principale utilizzava già il modello di recupero con registrazione completa al momento dell'esecuzione del backup. Il database mirror deve avere lo stesso nome del database principale.  
  
    2.  Se sono stati effettuati backup differenziali del database dopo il backup completo ripristinato, ripristinare il backup differenziale più recente.  
  
    3.  Ripristinare tutti i backup del log eseguiti dopo il backup completo o differenziale del database.  
  
     Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Completare i passaggi di installazione rimanenti non appena possibile dopo l'esecuzione del backup del database principale. Prima di poter avviare il mirroring nei partner, è consigliabile creare un backup del log corrente nel database originale e ripristinarlo nel futuro database mirror.  
  
2.  È possibile impostare il mirroring utilizzando [!INCLUDE[tsql](../../includes/tsql-md.md)] o la procedura guidata per il mirroring del database. Per ulteriori informazioni, vedere uno degli argomenti seguenti:  
  
    -   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
    -   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
3.  Per impostazione predefinita, una sessione è impostata su un livello di protezione delle transazioni completo (SAFETY è impostato su FULL), il che determina l'avvio della sessione in modalità sincrona a sicurezza elevata senza failover automatico. È possibile riconfigurare la sessione per l'esecuzione in modalità a sicurezza elevata con failover automatico o in modalità asincrona a prestazioni elevate, come riportato di seguito:  
  
    -   **Modalità a sicurezza elevata con failover automatico**  
  
         Se si desidera che una sessione in modalità a sicurezza elevata supporti il failover automatico, aggiungere un'istanza del server di controllo del mirroring.  
  
         **Per aggiungere un server di controllo del mirroring**  
  
        -   [Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
        -   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
        > [!NOTE]  
        >  Il proprietario del database può disattivare il server di controllo del mirroring di un database in qualsiasi momento. Un server di controllo del mirroring disattivato è praticamente assente e di conseguenza il failover automatico non può essere eseguito.  
  
    -   **Modalità a prestazioni elevate**  
  
         In alternativa, se si desidera evitare il failover automatico e si preferisce privilegiare le prestazioni rispetto alla disponibilità, disattivare la protezione delle transazioni. Per altre informazioni, vedere [Modifica della protezione delle transazioni in una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  In modalità a prestazioni elevate, WITNESS deve essere impostato su OFF. Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
> [!NOTE]  
>  Per un esempio dell'uso di [!INCLUDE[tsql](../../includes/tsql-md.md)] per impostare il mirroring del database tramite l'autenticazione di Microsoft Windows, vedere [Esempio: Impostazione del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
>   
>  Per un esempio dell'uso di [!INCLUDE[tsql](../../includes/tsql-md.md)] per impostare il mirroring del database tramite la sicurezza basata sui certificati, vedere [Esempio: Configurazione del mirroring del database tramite certificati &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> Contenuto della sezione  
 [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
 Include un riepilogo della procedura di creazione di un database mirror o di preparazione di un database mirror prima di riprendere una sessione sospesa. Include inoltre collegamenti ad altre procedure.  
  
 [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
 Descrive la sintassi di un indirizzo di rete del server, in che modo l'indirizzo identifica l'endpoint del mirroring del database dell'istanza del server e come individuare il nome di dominio completo di un sistema.  
  
 [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
 Viene descritto come utilizzare la Configurazione guidata sicurezza mirroring del database per avviare il mirroring di un database.  
  
 [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
 Viene descritta la procedura [!INCLUDE[tsql](../../includes/tsql-md.md)] per l'impostazione del mirroring del database.  
  
 [Esempio: Configurazione del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
 Include un esempio di tutte le fasi necessarie per creare una sessione di mirroring del database con un server di controllo del mirroring, utilizzando l'autenticazione di Windows.  
  
 [Esempio: Configurazione del mirroring del database tramite certificati &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
 Include un esempio di tutte le fasi necessarie per creare una sessione di mirroring del database con un server di controllo del mirroring, utilizzando l'autenticazione basata sui certificati.  
  
 [Configurare gli account di accesso per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
 Descrive la creazione di un account di accesso per un'istanza del server remota che utilizza un account diverso dall'istanza del server locale.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **SQL Server Management Studio**  
  
-   [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
 **Transact-SQL**  
  
-   [Concessione dell'accesso alla rete a un endpoint per il mirroring del database utilizzando l'autenticazione di Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in ingresso &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Impostazione di un database mirror per l'utilizzo della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
 **Transact-SQL/SQL Server Management Studio**  
  
-   [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)  
  
-   [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Risolvere i problemi relativi alla configurazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Mirroring del database: interoperabilità e coesistenza &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)   
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
  
