---
title: Scegliere una modalità di autenticazione | Microsoft Docs
description: Scegliere tra la modalità di autenticazione di Windows e l'autenticazione in modalità mista per il motore di database di SQL Server al momento dell'installazione.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.ins.instwizard.authenticationmode.f1
- sql13.swb.passwordexpired.f1
helpviewer_keywords:
- sa account
- authentication modes
- trusted connection
- SQL Server Installation Wizard, Authentication Mode page
- choose authentication mode
- authentication [SQL Server], choosing a mode
- Windows authentication [SQL Server]
- mixed mode authentication
- mixed authentication mode
- SQL authentication mode
- Password Expired dialog box
ms.assetid: ff7a6a48-3d38-4209-aa0f-7d6c0a8c64ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ebcf99ad025477a562f2a050eb1c2c3f3f63e12f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344399"
---
# <a name="choose-an-authentication-mode"></a>Scegliere una modalità di autenticazione
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Durante l'installazione è necessario selezionare una modalità di autenticazione per il [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Esistono due possibili modalità: modalità di autenticazione di Windows e modalità mista. La modalità di autenticazione di Windows abilita l'autenticazione di Windows e disabilita quella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La modalità mista abilita sia l'autenticazione di Windows sia quella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'autenticazione di Windows è sempre disponibile e non può essere disabilitata.  
  
## <a name="configuring-the-authentication-mode"></a>Configurazione della modalità di autenticazione  
 Se si seleziona l'autenticazione in modalità mista durante l'installazione, è necessario specificare e confermare una password complessa per l'account amministratore di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] predefinito denominato sa. L'account sa consente di connettersi utilizzando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Se si seleziona l'autenticazione di Windows durante l'installazione, viene creato l'account sa per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , che rimane però disabilitato. Se successivamente si passa all'autenticazione in modalità mista e si desidera utilizzare l'account sa, sarà necessario abilitarlo. Qualsiasi account di Windows o di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere configurato come amministratore di sistema. Poiché l'account sa è un account noto ed è spesso preso di mira dagli utenti malintenzionati, si consiglia di abilitarlo solo se richiesto dall'applicazione. Non impostare mai password vuote o vulnerabili per l'account sa. Per passare dalla modalità di autenticazione di Windows all'autenticazione in modalità mista e usare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Modificare la modalità di autenticazione del server](../../database-engine/configure-windows/change-server-authentication-mode.md).  
  
## <a name="connecting-through-windows-authentication"></a>Connessione tramite l'autenticazione di Windows  
 Quando un utente si connette utilizzando un account utente di Windows, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il nome di account e la password vengono convalidati tramite il token dell'entità di Windows nel sistema operativo. L'identità utente viene pertanto verificata da Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non richiede la password e non esegue la convalida dell'identità. L'autenticazione di Windows è la modalità di autenticazione predefinita e garantisce maggiore sicurezza rispetto all'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'autenticazione di Windows, per la quale viene utilizzato il protocollo di sicurezza Kerberos, garantisce l'applicazione dei criteri password mediante convalida della complessità delle password complesse, offre il supporto per il blocco dell'account e la scadenza delle password. In alcuni casi, una connessione effettuata con l'autenticazione di Windows viene denominata connessione trusted, poiché in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono considerate attendibili le credenziali fornite da Windows.  
  
 Utilizzando l'autenticazione di Windows, è possibile creare i gruppi di Windows a livello di dominio e un account di accesso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'intero gruppo. Gestire l'accesso a livello di dominio può semplificare l'amministrazione di account.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="connecting-through-sql-server-authentication"></a>Connessione tramite l'autenticazione di SQL Server  
 Quando si utilizza l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono creati account di accesso non basati su account utente di Windows. Il nome utente e la password vengono creati tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli utenti che si connettono tramite l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devono fornire le proprie credenziali (account di accesso e password) a ogni tentativo di connessione. Se si utilizza l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario impostare password complesse per tutti gli account di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per consultare le linee guida per la creazione di password complesse, vedere [Password complesse](../../relational-databases/security/strong-passwords.md).  
  
 Sono disponibili tre criteri password facoltativi per gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Richiedi modifica della password all'accesso successivo  
  
     Viene richiesto di modificare la password alla connessione successiva dell'utente. La possibilità di modificare la password è resa disponibile da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Se viene selezionata questa opzione, gli sviluppatori software di terze parti devono fornire questa funzionalità.  
  
-   Imponi scadenza password  
  
     Per gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono applicati i criteri di durata massima delle password del computer.  
  
-   Applica criteri password  
  
     Per gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono applicati i criteri password di Windows del computer, tra cui quelli relativi alla lunghezza e alla complessità delle password. Questa funzionalità dipende dall'API `NetValidatePasswordPolicy` , disponibile unicamente in [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] e versioni successive.  
  
#### <a name="to-determine-the-password-policies-of-the-local-computer"></a>Per determinare i criteri password del computer locale  
  
1.  Fare clic sul menu **Start** e scegliere **Esegui**.  
  
2.  Nella finestra di dialogo **Esegui** digitare **secpol.msc**, quindi fare clic su **OK**.  
  
3.  Nell'applicazione **Impostazioni sicurezza locale** espandere **Impostazioni di sicurezza** e **Criteri account**, quindi fare clic su **Criteri password**.  

     I criteri password sono descritti nel riquadro dei risultati.  
  
### <a name="disadvantages-of-sql-server-authentication"></a>Svantaggi dell'autenticazione di SQL Server  
  
-   Se l'utente è un utente di dominio Windows con un account di accesso e una password per Windows, deve comunque specificare un altro account di accesso ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) e un'altra password per connettersi. Tenere traccia di più nomi e password è un'operazione problematica per molti utenti. La necessità di fornire le credenziali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ogni volta che ci si connette al database può risultare fastidiosa.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L'autenticazione non supporta l'utilizzo del protocollo di sicurezza Kerberos.  
  
-   In Windows sono presenti criteri password aggiuntivi non disponibili per gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   È necessario che la password di accesso crittografata per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] venga trasmessa in rete al momento della connessione. In alcune applicazioni che si connettono automaticamente, la password verrà archiviata nel client, creando punti di attacco aggiuntivi.  
  
### <a name="advantages-of-sql-server-authentication"></a>Vantaggi dell'autenticazione di SQL Server  
  
-   Consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di supportare applicazioni meno recenti e applicazioni di terze parti per le quali è necessaria l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di supportare ambienti con sistemi operativi misti, in cui tutti gli utenti non sono autenticati da un dominio Windows.  
  
-   Consente agli utenti di connettersi da domini sconosciuti o non attendibili. ad esempio un'applicazione in cui clienti specifici si connettono con account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegnati per verificare lo stato dei loro ordini.  
  
-   Consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di supportare applicazioni Web in cui gli utenti creano le proprie identità.  
  
-   Consente agli sviluppatori software di distribuire le loro applicazioni tramite una gerarchia di autorizzazioni complessa basata su account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] noti e preimpostati.  
  
    > [!NOTE]  
    >  L'utilizzo dell'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non limita le autorizzazioni degli amministratori locali per il computer in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza per un'installazione di SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
  
