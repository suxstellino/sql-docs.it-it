---
title: Configurare un server di report per l'amministrazione remota | Microsoft Docs
description: Informazioni su come configurare istanze del server di report di Reporting Services per la configurazione locale o remota tramite lo strumento di configurazione o la scrittura di codice personalizzato.
ms.date: 09/14/2015
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services Configuration tool
- WMI provider [Reporting Services], remote configuration
- configuration management [WMI]
- report servers [Reporting Services], configuring
- remote server administration [Reporting Services]
ms.assetid: 8c7f145f-3ac2-4203-8cd6-2a4694395d09
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7232e6b5d89ae9c73b81df16a03ebcd00859d7ee
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597056"
---
# <a name="configure-a-report-server-for-remote-administration"></a>Configurare un server di report per l'amministrazione remota
  In [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]è possibile configurare istanze del server di report in modalità locale o remota. Per configurare un'istanza remota del server di report, è possibile usare lo strumento di configurazione di Reporting Services oppure scrivere codice personalizzato che usi il provider WMI (Windows Management Instrumentation) per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Lo strumento Gestione configurazione Reporting Services offre un'interfaccia grafica al provider WMI, per consentire di configurare un server di report senza dover scrivere codice. Quando si avvia lo strumento, è possibile specificare un server remoto a cui connettersi.  
  
 Prima che sia possibile utilizzare lo strumento per configurare un server di report remoto, è necessario seguire le indicazioni contenute in questo argomento per abilitare le porte in Windows Firewall, consentire le connessioni remote e abilitare le richieste WMI remote.  
  
 Una configurazione appropriata consente di evitare l'errore seguente:  
  
 `The machine could not be found.`  
  
 `"The RPC server is unavailable. (Exception from HRESULT: 0x800706BA)".`  
  
## <a name="prerequisites"></a>Prerequisiti  
 A tale scopo, è necessario accedere al sistema localmente ed essere membri del gruppo Administrators locale. Non è possibile modificare le impostazioni di Windows Firewall di un computer remoto tramite una connessione remota.  
  
 Se si desidera abilitare l'amministrazione remota per un utente non amministratore, è necessario concedere all'account le autorizzazioni per l'attivazione remota DCOM (Distributed Component Object Model). In questo argomento vengono fornite le istruzioni per la configurazione del server per l'accesso da parte di utenti non amministratori.  
  
 In alcune organizzazioni sono presenti criteri di gruppo che impediscono l'amministrazione remota del server per alcuni sistemi operativi o utenti. Prima di iniziare a modificare le impostazioni del firewall, rivolgersi all'amministratore di rete per verificare se vi sono limitazioni all'amministrazione remota.  
  
 Per ulteriori informazioni, vedere [Connecting Through Windows Firewall](/windows/win32/wmisdk/connecting-to-wmi-remotely-with-vbscript) nella documentazione di Platform SDK nel sito Web MSDN.  
  
## <a name="tasks"></a>Attività  
 Tra le attività che consentono di configurare un server di report remoto sono incluse le seguenti:  
  
-   Abilitare le porte in Windows Firewall per consentire le richieste sulle porte utilizzate dal server di report e dall'istanza del Motore di database di SQL Server.  Vedere [Configurare un firewall per l'accesso al server di report](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md) e [Configurazione di Windows Firewall per l'accesso al Motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
-   Consentire le connessioni remote all'istanza del Motore di database che ospita il database del server di report. Una connessione remota è necessaria per la configurazione della connessione al database del server di report e per la gestione delle chiavi di crittografia.  
  
-   Consentire il passaggio delle richieste WMI remote attraverso [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Firewall.  
  
-   Se si configura un server di report remoto per l'amministrazione da parte di un utente non amministratore, è necessario impostare autorizzazioni DCOM per consentire l'accesso WMI remoto a un account utente di Windows standard. Poiché WMI utilizza DCOM per il trasporto delle chiamate remote, è necessario impostare le autorizzazioni DCOM per consentire agli utenti che non hanno eseguito l'accesso come amministratore locale di configurare il server.  
  
-   Se si configura un server di report remoto per l'amministrazione da parte di un utente non amministratore, è inoltre necessario impostare autorizzazioni WMI nello spazio dei nomi WMI del server di report. Per impostazione predefinita, tutti i membri del gruppo Administrators locale dispongono di accesso allo spazio dei nomi WMI del server di report. Per concedere l'accesso a utenti non amministratori, è necessario impostare le autorizzazioni.  
  
 Le indicazioni per l'esecuzione di queste attività sono disponibili in questo argomento.  
  
### <a name="to-configure-remote-connections-to-the-report-server-database"></a>Per configurare connessioni remote al database del server di report  
  
1.  Fare clic sul pulsante **Start**, scegliere **Programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, quindi **Gestione configurazione SQL Server**.  
  
2.  Nel riquadro a sinistra espandere **Configurazione di rete SQL Server**, quindi fare clic su **Protocolli** per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Nel riquadro dei dettagli abilitare i protocolli TCP/IP e Named Pipes, quindi riavviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="to-enable-remote-administration-in-windows-firewall"></a>Per abilitare l'amministrazione remota in Windows Firewall  
  
1.  Accedere come amministratore locale al computer per il quale si desidera abilitare l'amministrazione remota.  
  
2.  Aprire un prompt dei comandi con privilegi di amministratore.  
  
3.  Eseguire il comando seguente:  
  
    ```  
    netsh.exe firewall set service type=REMOTEADMIN mode=ENABLE scope=ALL  
    ```  
  
     È possibile specificare opzioni diverse per Scope. Per ulteriori informazioni, vedere la documentazione di Windows Firewall.  
  
4.  Verificare che l'amministrazione remota sia abilitata. Per visualizzare lo stato, è possibile eseguire il comando seguente:  
  
    ```  
    netsh.exe firewall show state  
    ```  
  
5.  Riavviare il computer.  
  
### <a name="to-set-dcom-permissions-to-enable-remote-wmi-access-for-non-administrators"></a>Per impostare autorizzazioni DCOM per consentire l'accesso WMI remoto a utenti non amministratori  
  
1.  Nel menu Start, scegliere **Strumenti di amministrazione** e quindi **Servizi componenti**.  
  
     Per Windows Vista, nel menu Start, scegliere **Tutti i programmi**, **Esegui**, quindi immettere **mmc comexp.msc**.  
  
2.  Aprire la cartella Component Services.  
  
3.  Aprire la cartella Computer.  
  
4.  Selezionare Risorse del computer.  
  
5.  Scegliere **Proprietà** dal menu **Azione**.  
  
6.  Fare clic su **Sicurezza COM**.  
  
7.  In **Autorizzazioni di esecuzione e attivazione** fare clic su **Modifica limiti**.  
  
8.  Se il proprio nome non è visualizzato in **Autorizzazioni di avvio** fare clic su **Aggiungi**.  
  
9. Digitare il nome del proprio account utente e quindi fare clic su **OK**.  
  
10. Nella colonna **Consenti** in **Autorizzazioni per \<User or Group>** , selezionare **Avvio remoto** e **Attivazione remota** e quindi fare clic su **OK**.  
  
### <a name="to-set-permissions-on-the-report-server-wmi-namespace-for-non-administrators"></a>Per impostare autorizzazioni nello spazio dei nomi WMI del server di report per utenti non amministratori  
  
1.  Nel menu Start, scegliere **Strumenti di amministrazione** e quindi **Gestione computer**.  
  
2.  Aprire la cartella Servizi e applicazioni.  
  
3.  Fare clic con il pulsante destro del mouse su **Controllo WMI** e scegliere **Proprietà**.  
  
4.  Fare clic su **Security**.  
  
5.  Aprire la cartella Root.  
  
6.  Aprire la cartella Microsoft.  
  
7.  Aprire la cartella SQLServer.  
  
8.  Aprire la cartella ReportServer.  
  
9. Aprire la cartella dell'istanza. Se è stata installata l'istanza predefinita, il nome della cartella è MSSQLSERVER.  
  
10. Aprire la cartella v10.  
  
11. Selezionare la cartella Admin e quindi fare clic su **Sicurezza**.  
  
12. Fare clic su **Aggiungi** e quindi digitare l'account utente che si desidera utilizzare per gestire il server.  
  
13. Nella colonna **Consenti** selezionare **Abilita account**, **Abilita remoto** e **Sicurezza da lettura**, quindi scegliere **OK**.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  
