---
title: Eseguire la connessione a un server di report in Management Studio | Microsoft Docs
description: Informazioni su come connettersi a qualsiasi server della famiglia SQL Server e visualizzarne graficamente il contenuto usando Esplora oggetti in SQL Server Management Studio.
ms.date: 05/07/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttors.connectionproperties.f1
- sql13.swb.connecttors.login.f1
- sql13.swb.connection.login.reportserver.f1
helpviewer_keywords:
- report servers [Reporting Services], connections
- connections [Reporting Services], report server
- registering report servers
- report servers [Reporting Services], registering
- Connect to Server dialog box, Reporting Services
ms.assetid: c875ff87-ee7d-443a-a702-bdb4b6c27c6e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 735c7805d1deba9861a6e51249acf9928d97db7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076782"
---
# <a name="connect-to-a-report-server-in-management-studio"></a>Eseguire la connessione a un server di report in Management Studio

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fornisce Esplora oggetti, che consente di connettersi a qualsiasi server della famiglia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di visualizzarne graficamente il contenuto. Per Reporting Services, è possibile usare Esplora oggetti per eseguire queste attività:

- Abilitare le funzionalità del server di report.

- Impostare i valori predefiniti del server e configurare le definizioni di ruolo.

- Gestire i processi in esecuzione.

- Gestire le pianificazioni dei processi.

 È possibile connettersi a un server di report in modalità nativa o in modalità integrata SharePoint. La sintassi della connessione e i tipi di operazioni che è possibile eseguire dipendono dalla modalità del server di report e dalle autorizzazioni. Se non si riesce a connettersi al server di report o si riscontrano problemi nell'esecuzione di attività specifiche, probabilmente le autorizzazioni disponibili non sono sufficienti oppure il nome del server di report non è stato specificato correttamente. Per altre informazioni sulle autorizzazioni e sulla sintassi della connessione, vedere la tabella alla fine di questo articolo.

 Non è possibile usare Esplora oggetti per visualizzare o gestire il contenuto del server di report. La gestione del contenuto viene eseguita tramite il portale Web in caso di esecuzione del server di report in modalità nativa e tramite un sito di SharePoint in caso di esecuzione del server di report in modalità integrata SharePoint.

 Esplora oggetti consente di aprire connessioni verso più istanze del server nella stessa area di lavoro, purché i server siano registrati nello stesso gruppo. Prima che sia possibile connettersi a un'istanza del server di report in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], è necessario registrare il server. Se il server di report si già è registrato, ignorare questo passaggio. Le istruzioni per la registrazione dei server di report sono riportate alla fine di questo articolo.

### <a name="to-connect-to-a-native-mode-report-server"></a>Per connettersi a un server di report in modalità nativa

1. Se Esplora oggetti non è già aperto, selezionarlo nel menu **Visualizza**.

2. Selezionare **Connetti** per visualizzare l'elenco dei tipi di server e quindi **Reporting Services**.

3. Nella finestra di dialogo **Connetti al server** immettere il nome dell'istanza del server di report. I nomi delle istanze del server di report si basano sui nomi delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per impostazione predefinita, il nome di un'istanza locale del server di report è il nome del computer. Se il server di report è installato come istanza denominata, usare la sintassi seguente per specificare il server: *\<servername>[\\<nomeistanza\>]* .

4. Selezionare il **tipo di autenticazione**. Se si usa l'autenticazione di Windows, connettersi con le proprie credenziali. Se è stata selezionata l'autenticazione di base o quella basata su form, digitare l'account e la password.  
  
5. Selezionare **Connetti**. Il server di report verrà visualizzato in Esplora oggetti.  

6. Fare clic con il pulsante destro del mouse sul nodo del server per impostare le proprietà di sistema e valori predefiniti per il server. Per altre informazioni, vedere [Impostare le proprietà di un server di report &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).

### <a name="to-connect-to-a-sharepoint-integrated-mode-report-server"></a>Per connettersi a un server di report in modalità integrata SharePoint  

1. Se Esplora oggetti non è già aperto, selezionarlo nel menu **Visualizza**.

2. Selezionare **Connetti** per visualizzare l'elenco dei tipi di server e quindi **Reporting Services**.

3. Nella finestra di dialogo **Connetti al server** , immettere un URL a un sito di SharePoint. Nell'esempio seguente viene illustrata la sintassi: `https://<web server>/sites/<site>`.

4. Selezionare il **tipo di autenticazione**. Se si utilizza l'autenticazione di Windows, connettersi utilizzando le proprie credenziali. Se è stata selezionata l'autenticazione di base o quella basata su form, digitare l'account e la password.

5. Selezionare **Connetti**. Il server di report verrà visualizzato in Esplora oggetti.

6. Fare clic con il pulsante destro del mouse sul nodo del server per impostare le proprietà di sistema e valori predefiniti per il server. Per altre informazioni, vedere [Impostare le proprietà di un server di report &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).

### <a name="to-register-a-report-server"></a>Per registrare un server di report

1. Se non si riesce a connettersi a un server di report, non si è autorizzati ad accedere oppure il server non è registrato. Per registrare il server, scegliere **Server registrati** dal menu **Visualizza**.

2. Selezionare l'icona di **Reporting Services**.

3. Fare clic con il pulsante destro del mouse su **Reporting Services** e quindi scegliere **Nuovo** > **Registrazione server**. Verrà visualizzata la finestra di dialogo **Nuova registrazione server** .

4. In **Nome server** immettere un valore. Il valore da specificare dipende dalla modalità del server:

    - Per un server di report in modalità nativa, digitare il nome dell'istanza relativa. I nomi delle istanze del server di report si basano sui nomi delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per impostazione predefinita, il nome di un'istanza locale del server di report è il nome del computer. Se il server di report è installato come istanza denominata, usare la sintassi seguente per specificare il server: *\<servername>[\\<nomeistanza\>]* .

    - Per un server di report eseguito in modalità integrata SharePoint, il server cui connettersi è il sito di SharePoint cui il server di report è connesso. Connettersi al sito di SharePoint per poter visualizzare i livelli di autorizzazione. L'accesso al contenuto e alle operazioni del server di report è controllato dalle autorizzazioni. È possibile specificare qualsiasi sito nella raccolta siti. Nell'esempio seguente viene illustrata la sintassi: `https://mysharepointsite`.

5. Per **Autenticazione** selezionare la modalità di autenticazione già in uso nel server di report.

   - Se si usano le impostazioni di sicurezza predefinite, scegliere **Autenticazione di Windows**.
   - Se è stata installata e distribuita un'estensione di sicurezza personalizzata, scegliere **Autenticazione basata su form**.
   - Se il server di report è stato configurato per usare l'autenticazione di base, selezionare **Autenticazione di base**.
   - Se il server di report è configurato per la modalità integrata SharePoint, scegliere **Autenticazione di Windows**.

6. Selezionare **Test** per verificare la connessione.

7. Quando richiesto, selezionare **OK** e quindi **Salva**.

## <a name="connection-syntax-and-permissions"></a>Sintassi della connessione e autorizzazioni

 La tabella seguente offre un riepilogo della sintassi della connessione, dei passaggi e delle autorizzazioni necessari per eseguire attività specifiche.

 Quando si specifica [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] come tipo di server nella finestra di dialogo **Connetti al server** , è possibile specificare un nome del server di report o un endpoint al servizio Web.

|Server di report di connessione|   Attività   |   Autorizzazioni   |
|----------|-----------|-----------------|  
|Server di report in modalità nativa, connesso come istanza predefinita o denominata:<br /><br /> \<server name>\<_instance><br /><br /> La connessione al server di report viene eseguita mediante il provider WMI del server di report.|Visualizzazione e impostazione delle proprietà e dei valori predefiniti del server.<br /><br /> Visualizzazione e annullamento di processi.<br /><br /> Creazione e gestione di pianificazioni condivise.<br /><br /> Creazione, modifica o eliminazione di definizioni di ruolo.|Assegnato al ruolo di Amministratore sistema.|  
|Server di report in modalità nativa, connesso come istanza predefinita o denominata tramite l'endpoint al servizio Web ReportServer:<br /><br /> `https://<servername>/reportserver`<br /><br /> La specifica di un URL per server di report fornisce una modalità alternativa per connettersi al server di report.|Visualizzazione e impostazione delle proprietà e dei valori predefiniti del server.<br /><br /> Visualizzazione e annullamento di processi.<br /><br /> Creazione e gestione di pianificazioni condivise.<br /><br /> Creazione, modifica o eliminazione di definizioni di ruolo.|Assegnato al ruolo di Amministratore sistema.|  
|Server di report in modalità integrata SharePoint, connesso mediante il sito di SharePoint:<br /><br /> `https://<webserver>/<SharePointSite>`|Visualizzazione e impostazione delle proprietà e dei valori predefiniti del server.<br /><br /> Visualizzazione e annullamento di processi.<br /><br /> Creazione e gestione di pianificazioni condivise definite per il sito a cui si è connessi.<br /><br /> Visualizzazione dei livelli di autorizzazione definiti per il sito a cui si è connessi.|Livello di autorizzazione Controllo completo sul sito di SharePoint a cui si è connessi.|
|Server di report in modalità integrata SharePoint, connesso mediante il nome dell'istanza del server di report:<br /><br /> \<server name>\<_instance>|Visualizzazione e impostazione delle proprietà e dei valori predefiniti del server.<br /><br /> Visualizzazione e annullamento di processi.|Livello di autorizzazione Controllo completo sul sito di SharePoint integrato con il server di report.<br /><br /> Si noti che in caso di connessione al server di report, anziché al sito di SharePoint, il numero di attività che è possibile eseguire si riduce. Ciò è dovuto al fatto che il server di report può restituire solo dati dell'applicazione archiviati o gestiti nel database del server di report e non nei database di configurazione e del contenuto di SharePoint.|

## <a name="see-also"></a>Vedere anche

 [Configurare una connessione del database del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 [Reporting Services in SQL Server Management Studio &#40;SSRS&#41;](../../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)
