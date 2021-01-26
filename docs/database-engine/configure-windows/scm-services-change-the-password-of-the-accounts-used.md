---
title: Gestione configurazione SQL Server - Modificare la password degli account | Microsoft Docs
description: Informazioni su come modificare la password degli account usati dal motore di database e da SQL Server Agent. Informazioni su quando è importante modificare la password.
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- expired password [SQL Server], SQL Server Agent
- passwords [SQL Server], SQL Server Agent service
- passwords [SQL Server], changing
- expired password [SQL Server], Database Engine
- passwords [SQL Server], SQL Server service
- Database Engine [SQL Server], passwords
- changing passwords used by SQL Server
- modifying passwords
ms.assetid: 5b6dcc03-6cae-45d3-acef-6f85ca6d615f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 05b7ec62778370e2acf0c5b4b4d90d3f1a742d87
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783103"
---
# <a name="scm-services---change-the-password-of-the-accounts-used"></a>Gestione configurazione SQL Server - Modificare la password degli account
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come modificare la password degli account utilizzati dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite Gestione configurazione SQL Server. Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent vengono eseguiti in un computer come servizi, utilizzando credenziali fornite inizialmente durante l'installazione. Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguita in un account di dominio e la password per tale account viene modificata, è necessario aggiornare la password utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostando la nuova password. Se la password non viene aggiornata, è possibile che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sia più in grado di accedere a determinate risorse di dominio e se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestato, il servizio non verrà riavviato fino all'aggiornamento della password.  
  
 Per modificare le password di autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Password scaduta](../../relational-databases/security/choose-an-authentication-mode.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è lo strumento progettato e autorizzato per la modifica delle impostazioni dei servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La modifica di un servizio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'applicazione Gestione controllo servizi di Windows (**services.msc**) non sempre determina la modifica di tutte le impostazioni necessarie e potrebbe impedire il corretto funzionamento del servizio. Tuttavia, in un ambiente cluster, dopo aver modificato la password nel nodo attivo tramite Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario modificare la password del nodo passivo utilizzando Gestione controllo servizi.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario essere l'amministratore del computer per modificare la password utilizzata da un servizio.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
#### <a name="to-change-the-password-used-by-the-sql-server-database-engine-service"></a>Per modificare la password utilizzata dal servizio SQL Server (Motore di database)  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, quindi fare clic su **Gestione configurazione SQL Server**.  
  
    > [!NOTE]  
    >  Poiché Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è uno snap-in per il programma [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console e non un programma autonomo, Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene visualizzato come applicazione nelle versioni più recenti di Windows.  
    >   
    >  -   **Windows 10**:  
    >          per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nella **pagina iniziale** digitare SQLServerManager13.msc (per [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). Per le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sostituire 13 con un numero inferiore. Se si fa clic su SQLServerManager13.msc, viene aperto Gestione configurazione. Per aggiungere Gestione configurazione alla pagina iniziale o alla barra delle applicazioni, fare clic con il pulsante destro del mouse su SQLServerManager13.msc e quindi scegliere **Apri percorso file**. In Esplora file di Windows fare clic con il pulsante destro del mouse su SQLServerManager13.msc e quindi scegliere **Aggiungi a Start** o **Aggiungi alla barra delle applicazioni**.  
    > -   **Windows 8**:  
    >          Per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nell'icona promemoria **Cerca** in **App** digitare **SQLServerManage\<version>.msc**, ad esempio **SQLServerManager13.msc**, quindi premere **INVIO**.  
  
2.  In Gestione configurazione SQL Server fare clic su **Servizi di SQL Server**.  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **SQL Server (** \<instancename> **)** e quindi scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **SQL Server (** \<instancename> **) Proprietà**, nella scheda Accesso, per l'account indicato nella casella **Nome account** digitare la nuova password nelle caselle **Password** e **Conferma password**, quindi fare clic su **OK**.  
  
     La password diventa immediatamente effettiva e non richiede il riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="to-change-the-password-used-by-the-sql-server-agent-service"></a>Per modificare la password utilizzata dal servizio SQL Server Agent  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, quindi fare clic su **Gestione configurazione SQL Server**.  
  
2.  In Gestione configurazione SQL Server fare clic su **Servizi di SQL Server**.  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **SQL Server Agent (** \<instancename> **)** e quindi scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **SQL Server Agent (** \<instancename> **) Proprietà**, nella scheda Accesso, per l'account indicato nella casella **Nome account** digitare la nuova password nelle caselle **Password** e **Conferma password**, quindi fare clic su **OK**.  
  
     In un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la password diventa effettiva immediatamente, senza riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In un'istanza cluster, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può portare offline la risorsa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e potrebbe essere necessario un riavvio.  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure per la gestione dei servizi &#40;Gestione configurazione SQL Server&#41;](./scm-services-connect-to-another-computer.md)  
  
