---
title: Configurare l'accesso a Generatore report | Microsoft Docs
description: Configurare Generatore report, uno strumento di progettazione report da utilizzare con un server di report SQL Server Reporting Services. Viene usata la modalità nativa o la modalità di integrazione SharePoint.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.date: 03/07/2021
ms.openlocfilehash: 6940a6c5b46861ebb745fb3965167a4d19173446
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833854"
---
# <a name="configure-report-builder-access"></a>Configurare l'accesso a Generatore report

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

Microsoft Generatore report è uno strumento per la creazione di report ad hoc che può essere utilizzato con un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] server di report configurato per la modalità nativa o per l'integrazione con SharePoint.  

L'accesso a Generatore report dipende dai fattori seguenti:  

- Assegnazioni di ruolo o autorizzazioni tramite le quali Generatore report viene reso disponibile a singoli utenti o gruppi.  

- Impostazioni di autenticazione che determinano se le credenziali utente possono essere passate al server di report.

## <a name="prerequisites"></a>Prerequisiti

Nel computer client deve essere [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] installato 4.6.1 o versione successiva.

## <a name="enabling-and-disabling-report-builder"></a>Abilitazione e disabilitazione di Generatore report  

Il download di Generatore report tramite il portale è abilitato per impostazione predefinita. Gli amministratori del server di report hanno la possibilità di disabilitare il download Generatore report impostando la proprietà di sistema del server di report **ShowDownloadMenu** su **false**. Impostando questa proprietà, vengono disabilitati i download di Generatore report, Mobile Report Publisher, Power BI Desktop e Power BI mobile per il server di report.  

 Per impostare le proprietà di sistema del server di report, è possibile utilizzare Management Studio o uno script:   

 - Per usare Management Studio, connettersi al server di report e utilizzare la pagina relativa alle proprietà avanzate del server per impostare **ShowDownloadMenu** su **false**. Per altre informazioni su come aprire questa pagina, vedere [Impostare le proprietà di un server di report &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).      

 - Per visualizzare uno script di esempio che consente di impostare una proprietà del server di report, vedere [Creare script per le attività di distribuzione e di amministrazione](../../reporting-services/tools/script-deployment-and-administrative-tasks.md).  

## <a name="role-assignments-granting-report-builder-access-on-a-native-mode-report-server"></a>Assegnazioni di ruolo che concedono l'accesso a Generatore report in un server di report in modalità nativa  

In un server di report in modalità nativa creare assegnazioni di ruoli utente che includano attività per l'utilizzo di Generatore report. Per creare o modificare definizioni di ruolo e assegnazioni di ruolo sugli elementi e a livello del sito, è necessario essere assegnati ai ruoli Amministratore sistema e Gestione contenuto.  

Nelle istruzioni seguenti si presuppone che vengano utilizzati ruoli predefiniti. Se le definizioni di ruolo sono state modificate, controllare i ruoli per verificare che contengano le attività necessarie. Per altre informazioni sulla creazione delle assegnazioni di ruolo, vedere [Concedere l'accesso utente a un server di report](../../reporting-services/security/grant-user-access-to-a-report-server.md).

Dopo avere creato le assegnazioni di ruolo, gli utenti disporranno dell'autorizzazione per effettuare le operazioni seguenti:  

- Gli utenti assegnati ai ruoli Utente sistema e Visualizzazione possono visualizzare report di Generatore report pubblicati in un server di report, senza la necessità di avviare Generatore report.  

- Gli utenti assegnati ai ruoli Utente sistema e Generatore report possono generare modelli, avviare Generatore report per creare report e salvare report nel server di report.  

- Gli utenti assegnati ai ruoli Utente sistema e Pubblicazione possono pubblicare modelli da Progettazione modelli nel server di report. I modelli vengono utilizzati come origini dati in Generatore report.  

- Gli utenti assegnati ai ruoli Amministratore sistema e Gestione contenuto dispongono delle autorizzazioni complete per creare, visualizzare e gestire report di Generatore report.  

### <a name="to-verify-required-tasks-are-in-the-role-definitions"></a>Per verificare la presenza delle attività necessarie nelle definizioni di ruolo  

1. Avviare Management Studio e connettersi al server di report.  

2. Aprire la cartella **Sicurezza** .  

3. Aprire la cartella **Ruoli a livello di sistema** .  

4. Fare clic con il pulsante destro del mouse su **Amministratore sistema** e scegliere **Proprietà**.  

5. Selezionare **Esecuzione delle definizioni dei report** e clic su **OK**.  

6. Fare clic con il pulsante destro del mouse su **Utente sistema** e scegliere **Proprietà**.  

7. Selezionare **Esecuzione delle definizioni dei report** e clic su **OK**.  

8. Aprire la cartella **Ruoli** .  

9. Fare clic con il pulsante destro del mouse su **Visualizzazione** e scegliere **Proprietà**.  

10. Selezionare **Visualizzazione di modelli** e quindi fare clic cu **OK**.  

11. Fare clic con il pulsante destro del mouse su **Gestione contenuto** e scegliere **Proprietà**.  

12. Selezionare **Visualizzazione di modelli**, **Gestione di modelli** e **Utilizzo di report**, quindi fare clic su **OK**.  

13. Fare clic con il pulsante destro del mouse su **Pubblicazione** e scegliere **Proprietà**.  

14. Selezionare **Gestione di modelli** e quindi fare clic su **OK**.  

15. Creare il ruolo Generatore report se non esiste:  

    1. Aprire la cartella **Sicurezza** .  

    2. Fare clic con il pulsante destro del mouse su **Ruoli** e scegliere **Nuovo ruolo**.  

    3. In Nome digitare **Generatore report**.  

    4. In Descrizione immettere una descrizione per il ruolo in modo da indicare lo scopo del ruolo agli utenti del portale Web.  

    5. Aggiungere le attività seguenti: **Utilizzo di report**, **Visualizzazione di report**, **Visualizzazione di modelli**, **Visualizzazione di risorse**, **Visualizzazione di cartelle** e **Gestione di sottoscrizioni individuali**.  

    6. Fare clic su **OK** per salvare il ruolo.  

#### <a name="to-create-role-assignments-that-grant-access-to-report-builder"></a>Per creare assegnazioni di ruolo che concedono l'accesso a Generatore report  

1. Avviare il portale Web.  

2. Fare clic sull'icona a forma di ingranaggio nell'angolo in alto a destra della home page del portale Web e selezionare **Impostazioni sito** dal menu a discesa.  
![icona a forma di ingranaggio e menu del portale Web](../../reporting-services/report-builder/media/configure-report-builder-access/ssrswebportal-site-settings-gear-icon-and-menu.png)

3. Fare clic su **Security**.  

4. Se esiste già un'assegnazione di ruolo per l'utente o il gruppo per cui si vuole configurare l'accesso a Generatore report, fare clic su **Modifica**.  
In caso contrario, fare clic su **Nuova assegnazione ruolo**. In Gruppo o utente immettere un account utente o di gruppo di dominio Windows in questo formato: \<domain>\\<account\>. Se si utilizza l'autenticazione basata su form o la sicurezza personalizzata, specificare l'account utente o di gruppo nel formato corretto per la propria distribuzione.  

5. Selezionare **Utente sistema** e quindi fare clic su **OK**.  

6. Fare clic su **Home**.  

7. Fare clic sulla scheda **Impostazioni cartella** .  

8. Fare clic sulla scheda **Security** (Sicurezza).  

9. Se esiste già un'assegnazione di ruolo per l'utente o il gruppo per cui si vuole configurare l'accesso a Generatore report, fare clic su **Modifica**.  

    In caso contrario, fare clic su **Nuova assegnazione ruolo**. In Gruppo o utente immettere un account utente o di gruppo di dominio Windows in questo formato: \<domain>\\<account\>. Se si utilizza l'autenticazione basata su form o la sicurezza personalizzata, specificare l'account utente o di gruppo nel formato corretto per la propria distribuzione.  

10. Selezionare **Generatore report** e quindi fare clic su **Applica**.  

11. Ripetere le operazioni per creare o modificare le assegnazioni di ruolo per utenti o gruppi aggiuntivi.  

## <a name="permissions-granting-report-builder-access-on-a-sharepoint-integrated-mode-report-server"></a>Autorizzazioni che concedono l'accesso a Generatore report in un server di report in modalità integrata SharePoint  

In un server di report in modalità integrata SharePoint l'accesso a Generatore report viene concesso agli utenti di SharePoint che dispongono dei livelli di autorizzazione Collaborazione o Controllo completo.  

Se si utilizzano livelli di autorizzazione personalizzati, è necessario includere Aggiunta elementi e Modifica elementi al livello di autorizzazione. Per altre informazioni sull'accesso a Generatore report tramite i livelli di autorizzazione predefiniti, vedere [Usare la sicurezza predefinita di Windows SharePoint Services per gli elementi del server di report](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md). Per altre informazioni sui requisiti di autorizzazione per i livelli di autorizzazione personalizzati, vedere [Impostare le autorizzazioni per le operazioni del server di report in un'applicazione Web di SharePoint](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md).  

## <a name="authentication-considerations-and-credential-reuse"></a>Considerazioni sull'autenticazione e riutilizzo delle credenziali  

- Generatore report apre una connessione a un server di report. Se non si utilizza la sicurezza integrata di Windows con Single Sign-On, gli utenti devono specificare nuovamente le proprie credenziali per consentire a Generatore report di stabilire la connessione al server di report.  

## <a name="see-also"></a>Vedere anche  

- [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md)
- [Supporto browser per Reporting Services e Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
- [Avviare Generatore report](../../reporting-services/report-builder/start-report-builder.md)
- [Portale Web di un server di report (modalità nativa SSRS)](../web-portal-ssrs-native-mode.md)
- [Eseguire la connessione a un server di report in Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)
- [Proprietà di sistema del server di report](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md)
