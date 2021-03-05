---
title: Scrittura di eventi di controllo di SQL Server nel registro di sicurezza | Microsoft Docs
description: Informazioni su come registrare eventi di controllo di SQL Server nel registro di sicurezza di Windows. Informazioni sulle limitazioni e le restrizioni relative all'uso del log.
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], Security Log
- server audit [SQL Server]
- audits [SQL Server], writing to Security Log
- security logs [SQL Server]
ms.assetid: 6fabeea3-7a42-4769-a0f3-7e04daada314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abdd25c6166b40436584a1260d4b1b09d40900c4
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186065"
---
# <a name="write-sql-server-audit-events-to-the-security-log"></a>Scrittura di eventi di controllo di SQL Server nel registro di sicurezza  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In un ambiente con sicurezza elevata il registro di sicurezza di Windows rappresenta la posizione appropriata per la scrittura di eventi che registrano l'accesso agli oggetti. Sono supportati altre posizioni di controllo che tuttavia sono più soggette alla manomissione.  
  
 Per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] la scrittura dei controlli del server nel registro di sicurezza di Windows sono necessari tre requisiti principali:  
  
-   È necessario configurare l'impostazione di controllo dell'accesso agli oggetti per l'acquisizione degli eventi. Lo strumento dei criteri di controllo (`auditpol.exe`) espone varie impostazioni di criteri secondari nella categoria **controllo dell'accesso agli oggetti** . Per consentire il controllo dell'accesso agli oggetti in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , configurare l'impostazione **generata dall'applicazione** .  
-   L'account in cui viene eseguito il servizio di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve disporre dell'autorizzazione **generazione controlli di sicurezza** per scrivere nel registro di sicurezza di Windows. Per impostazione predefinita, gli account LOCAL SERVICE e NETWORK SERVICE dispongono di questa autorizzazione. Questo passaggio non è obbligatorio se [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene eseguito in uno di questi account.  
-   Concedere l'autorizzazione completa per l'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]all'hive del Registro di sistema `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security`.  

  > [!IMPORTANT]  
  > [!INCLUDE[ssnoteregistry-md](../../../includes/ssnoteregistry-md.md)]   
  
I criteri di controllo di Windows possono influire sul controllo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se sono configurati per scrivere nel registro di sicurezza di Windows. Se i criteri di controllo non sono configurati in modo corretto, potrebbe verificarsi la perdita di eventi. In genere il registro di sicurezza di Windows è impostato in modo che gli eventi meno recenti vengano sovrascritti e vengano mantenuti quelli più recenti. Tuttavia, se il registro di sicurezza di Windows è impostato in modo diverso e il registro di sicurezza è pieno, verrà generato l'evento di Windows 1104 che indica che il registro è pieno. A questo punto si verificano le situazioni seguenti:  
-   Non verranno registrati ulteriori eventi di sicurezza  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non potrà rilevare che il sistema non è in grado di registrare gli eventi nel registro di sicurezza, causando una perdita potenziale di eventi di controllo  
-   Dopo che l'amministratore ha apportato correzioni al registro di sicurezza, il comportamento relativo alla registrazione tornerà normale.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Gli amministratori del computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] devono comprendere che le impostazioni locali relative al registro di sicurezza possono essere sovrascritte da criteri del dominio. In questo caso i criteri del dominio potrebbero sovrascrivere l'impostazione della sottocategoria (**auditpol/get/subcategory:"application generated"** ). Questa condizione può influire sulla possibilità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di registrare eventi senza disporre di alcuna modalità per rilevare che gli eventi che [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sta tentando di controllare non verranno registrati.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario essere amministratore di Windows per configurare queste impostazioni.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-auditpol"></a><a name="auditpolAccess"></a> Per configurare l'impostazione di controllo dell'accesso agli oggetti in Windows mediante auditpol  
  
1.  Aprire un prompt dei comandi con autorizzazioni amministrative.  
  
    1.  Nel menu **Start** scegliere **Tutti i programmi**, **Accessori**, fare clic con il pulsante destro del mouse su **Prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.  
  
    2.  Se viene visualizzata la finestra di dialogo **Controllo account utente** , fare clic su **Continua**.  
  
2.  Eseguire l'istruzione seguente per abilitare il controllo da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    ```  
    auditpol /set /subcategory:"application generated" /success:enable /failure:enable  
    ```  
  
3.  Chiudere la finestra del prompt dei comandi.  
  
##  <a name="to-grant-the-generate-security-audits-permission-to-an-account-using-secpol"></a><a name="secpolAccess"></a> Per concedere l'autorizzazione di generazione dei controlli di sicurezza a un account mediante secpol  
  
1.  Per qualsiasi sistema operativo Windows, scegliere **Esegui** dal menu **Start**.  
  
2.  Digitare **secpol.msc** , quindi fare clic su **OK**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , fare clic su **Continua**.  
  
3.  Nello strumento Criteri di sicurezza locale espandere **Impostazioni di sicurezza**, **Criteri locali**, quindi fare clic su **Assegnazione diritti utente**.  
  
4.  Nel riquadro dei risultati fare doppio clic su **Generazione di controlli di sicurezza**.  
  
5.  Nella scheda **Impostazioni di sicurezza locali** fare clic su **Aggiungi utente o gruppo**.  
  
6.  Nella finestra di dialogo **Seleziona utenti, computer o gruppi** digitare il nome dell'account utente, ad esempio **dominio1\utente1** , quindi fare clic su **OK** oppure su **Avanzate** e cercare l'account.  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
8.  Chiudere lo strumento Criteri di sicurezza.  
  
9. Riavviare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per abilitare questa impostazione.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-secpol"></a><a name="secpolPermission"></a> Per configurare l'impostazione di controllo dell'accesso agli oggetti in Windows mediante secpol  
  
1.  Se si utilizza un sistema operativo precedente a [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] o Windows Server 2008, scegliere **Esegui** dal menu **Start**.  
  
2.  Digitare **secpol.msc** , quindi fare clic su **OK**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , fare clic su **Continua**.  
  
3.  Nello strumento Criteri di sicurezza locale espandere **Impostazioni di sicurezza**, **Criteri locali**, quindi fare clic su **Criteri di controllo**.  
  
4.  Nel riquadro dei risultati fare doppio clic su **Controlla accesso agli oggetti**.  
  
5.  Nella scheda **Impostazioni di sicurezza locali** , nell'area **Controlla i seguenti tentativi** selezionare sia **Esito positivo** sia **Esito negativo**.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  Chiudere lo strumento Criteri di sicurezza.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Audit &#40;Database Engine&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)  
  
  
