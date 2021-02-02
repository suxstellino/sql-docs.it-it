---
title: Configurazione delle opzioni di avvio del server (Gestione configurazione SQL Server) | Microsoft Docs
description: Informazioni su come impostare le opzioni usate dal motore di database di SQL Server all'avvio. Visualizzare limitazioni e restrizioni per apportare modifiche ai parametri di avvio.
ms.custom: ''
ms.date: 11/23/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], startup options
- SQL Server, startup options
- SQL Server, startup parameters
- single-user mode [SQL Server], starting in
- startup options [SQL Server]
- startup parameters [SQL Server]
- SQL Server services, setting startup options
- SQL Server services, setting startup parameters
ms.assetid: 7a94643c-6460-4baf-bb31-0cb99eaf970d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13240827e460c1bb64f3ae41e648d4dd44842105
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236668"
---
# <a name="scm-services---configure-server-startup-options"></a>Configurazione delle opzioni di avvio del server (Gestione configurazione SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come configurare le opzioni di avvio da usare ogni volta che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene avviato in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager. Per un elenco delle opzioni di avvio, vedere [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , i parametri di avvio vengono scritti nel Registro di sistema e vengono applicati al successivo avvio del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 In un cluster le modifiche devono essere eseguite sul server attivo quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è online e verranno applicate al riavvio di [!INCLUDE[ssDE](../../includes/ssde-md.md)] . L'aggiornamento delle opzioni di avvio nel Registro di sistema per l'altro nodo verrà eseguito al successivo failover.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 La configurazione delle opzioni di avvio del server è limitata a utenti che possono modificare le voci correlate nel Registro di sistema. Sono inclusi gli utenti indicati di seguito.  
  
-   Membri del gruppo Administrators locale.  
  
-   Account di dominio utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] è configurato per essere in esecuzione in un account di dominio.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
#### <a name="to-configure-startup-options"></a>Per configurare le opzioni di avvio  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, quindi fare clic su **Gestione configurazione SQL Server**.  
  
    > [!NOTE]  
    >  Poiché Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è uno snap-in per il programma [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console e non un programma autonomo, Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene visualizzato come applicazione nelle versioni più recenti di Windows.  
    >   
    >  -   **Windows 10**:  
    >          per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nella **pagina iniziale** digitare SQLServerManager13.msc (per [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]). Per le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sostituire 13 con un numero inferiore. Se si fa clic su SQLServerManager13.msc, viene aperto Gestione configurazione. Per aggiungere Gestione configurazione alla pagina iniziale o alla barra delle applicazioni, fare clic con il pulsante destro del mouse su SQLServerManager13.msc e quindi scegliere **Apri percorso file**. In Esplora file di Windows fare clic con il pulsante destro del mouse su SQLServerManager13.msc e quindi scegliere **Aggiungi a Start** o **Aggiungi alla barra delle applicazioni**.  
    >  -   **Windows 8**:  
    >          Per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nell'icona promemoria **Cerca** in **App** digitare **SQLServerManager\<version>.msc**, ad esempio **SQLServerManager13.msc**, quindi premere **INVIO**.  
  
2.  In Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fare clic su **Servizi di SQL Server**.  
  
3.  Nel riquadro destro fare clic con il pulsante destro del mouse su **SQL Server(** _<nome_istanza>_ **)** e quindi scegliere **Proprietà**.  
  
4.  Nella scheda **Parametri di avvio** della casella **Specificare un parametro di avvio** digitare il parametro e quindi fare clic su **Aggiungi**.  
  
     Per l'avvio in modalità utente singolo, digitare **-m** nella casella **Specificare un parametro di avvio** e quindi fare clic su **Aggiungi**. Al riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità utente singolo, arrestare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. In caso contrario, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent potrebbe eseguire per primo la connessione impedendo la connessione come secondo utente.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Riavviare il [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
    > [!WARNING]  
    >  Quando si finisce di usare la modalità utente singolo, selezionare il parametro **-m** nella casella **Parametri esistenti** della casella Parametri di avvio e quindi fare clic su **Rimuovi**. Riavviare [!INCLUDE[ssDE](../../includes/ssde-md.md)] per ripristinare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nella modalità tipica multiutente.  
  
## <a name="see-also"></a>Vedere anche  
 [Avvio di SQL Server in modalità utente singolo](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)   
 [Connettersi a SQL Server se gli amministratori di sistema sono bloccati](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)   
 [Avvio, arresto o sospensione del servizio SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
 [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md) 
  
