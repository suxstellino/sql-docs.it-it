---
description: Configurare l'autenticazione di Windows nel server di report.
title: Configurare l'autenticazione di Windows nel server di report | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [Reporting Services]
- Reporting Services, configuration
ms.assetid: 4de9c3dd-0ee7-49b3-88bb-209465ca9d86
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 22a9134ed3caf220dbe895d7e4de8b9d52290e08
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536332"
---
# <a name="configure-windows-authentication-on-the-report-server"></a>Configurare l'autenticazione di Windows nel server di report.
  Per impostazione predefinita, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] accetta richieste che specificano l'autenticazione con negoziazione o NTLM. Se nella distribuzione sono incluse applicazioni client e browser che utilizzano tali provider di sicurezza, è possibile utilizzare i valori predefiniti senza alcuna configurazione aggiuntiva. Se si desidera utilizzare un provider di sicurezza diverso per la sicurezza integrata di Windows, ad esempio se si desidera utilizzare direttamente l'autenticazione Kerberos, o se i valori predefiniti sono stati modificati e si desidera ripristinare le impostazioni originali, è possibile utilizzare le informazioni contenute in questo argomento per specificare le impostazioni di autenticazione nel server di report.  
  
 Per utilizzare la sicurezza integrata di Windows, ogni utente che richiede l'accesso a un server di report deve disporre di un account utente di dominio o locale di Windows valido o essere membro di un account di gruppo di dominio o locale di Windows. È possibile includere account di altri domini, purché tali domini siano di tipo trusted. Per accedere a operazioni specifiche del server di report, gli account devono disporre dell'accesso al computer relativo e devono quindi essere assegnati a ruoli.  
  
 È necessario inoltre che siano soddisfatti i seguenti requisiti aggiuntivi:  
  
-   I file RSReportServer.config devono avere **AuthenticationType** impostato su **RSWindowsNegotiate**, **RSWindowsKerberos** o **RSWindowsNTLM**. Per impostazione predefinita, il file RSReportServer.config include l'impostazione **RSWindowsNegotiate** se l'account del servizio del server di report è NetworkService o LocalSystem; in caso contrario, viene usata l'impostazione **RSWindowsNTLM** . Se sono presenti applicazioni che usano solo l'autenticazione Kerberos, è possibile aggiungere **RSWindowsKerberos** .  
  
    > [!IMPORTANT]  
    >  L'uso di **RSWindowsNegotiate** comporterà un errore di autenticazione Kerberos se il servizio del server di report è stato configurato per essere eseguito con un account utente di dominio e non è stato registrato un nome SPN per l'account. Per altre informazioni, vedere [Risoluzione di errori di autenticazione Kerberos durante la connessione a un server di report](#proxyfirewallRSWindowsNegotiate) in questo argomento.  
  
-   [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] deve essere configurato per usare l'autenticazione di Windows. Per impostazione predefinita, i file Web.config per il servizio Web ReportServer includono l'impostazione \<authentication mode="Windows">. Se l'impostazione viene modificata in \<authentication mode="Forms">, l'autenticazione di Windows per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] avrà esito negativo.  
  
-   Nei file Web.config per il servizio Web ReportServer deve essere presente l'impostazione \<identity impersonate= "true" />.  
  
-   Nell'applicazione client o nel browser deve essere supportata la sicurezza integrata di Windows.  

- Per [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] non è necessaria alcuna configurazione aggiuntiva.
  
 Per modificare le impostazioni di autenticazione del server di report, modificare gli elementi XML e i valori nel file RSReportServer.config. È possibile copiare e incollare gli esempi disponibili in questo argomento per implementare combinazioni specifiche.  
  
 Le impostazioni predefinite funzionano in modo ottimale se tutti i computer client e server si trovano nello stesso dominio o in un dominio di tipo trusted e se il server di report è distribuito per l'accesso Intranet attraverso un firewall aziendale. I domini singoli e di tipo trusted sono un requisito per il passaggio delle credenziali di Windows. Le credenziali possono essere passate più volte se si abilita il protocollo Kerberos versione 5 per i server. In caso contrario, le credenziali possono essere passate solo una volta prima che scadano. Per altre informazioni sulla configurazione delle credenziali per più connessioni del computer, vedere [Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
 Le istruzioni seguenti sono relative a un server di report in modalità nativa. Se il server di report è distribuito in modalità integrata SharePoint, è necessario utilizzare le impostazioni di autenticazione predefinite che specificano la sicurezza integrata di Windows. Per supportare server di report in modalità integrata SharePoint, il server di report utilizza caratteristiche interne nell'estensione di autenticazione di Windows predefinita.  
  
## <a name="extended-protection-for-authentication"></a>Protezione estesa per l'autenticazione  
 A partire da [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], è disponibile il supporto per la protezione estesa per l'autenticazione. La caratteristica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta l'uso del binding di canale e dell'associazione al servizio per migliorare la protezione dell'autenticazione. Le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] devono essere utilizzate con un sistema operativo che supporti la protezione estesa. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] La configurazione per la protezione estesa è determinata dalle impostazioni presenti nel file RSReportServer.config. È possibile aggiornare il file modificandolo o utilizzando le API di WMI. Per altre informazioni, vedere [Protezione estesa per l'autenticazione con Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md).  
  
### <a name="to-configure-a-report-server-to-use-windows-integrated-security"></a>Per configurare un server di report per l'utilizzo della sicurezza integrata di Windows  
  
1.  Aprire RSReportServer.config in un editor di testo.  
  
2.  Individuare \<**Authentication**>.  
  
3.  Tra le strutture XML seguenti, copiare quella che corrisponde meglio alle proprie esigenze. È possibile specificare **RSWindowsNegotiate**, **RSWindowsNTLM** e **RSWindowsKerberos** in qualsiasi ordine. Se si desidera autenticare la connessione anziché ogni singola richiesta, è necessario abilitare la persistenza dell'autenticazione in modo che tutte le richieste per cui è necessaria l'autenticazione verranno consentite per la durata della connessione.  
  
     La prima struttura XML è la configurazione predefinita quando l'account del servizio del server di report è NetworkService o LocalSystem:  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
     La seconda struttura XML è la configurazione predefinita quando l'account del servizio del server di report non è NetworkService o LocalSystem:  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    ```  
  
     \</Authentication>  
  
     La terza struttura XML specifica tutti i pacchetti di sicurezza utilizzati nella sicurezza integrata di Windows:  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
                 <RSWindowsKerberos />  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
     La quarta struttura XML specifica l'autenticazione NTLM solo per distribuzioni che non supportano l'autenticazione Kerberos o per risolvere errori di autenticazione Kerberos:  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
4.  Incollare la struttura sulle voci esistenti per \<**Authentication**>.  
  
     Si noti che non è possibile usare **Personalizzata** con i tipi **RSWindows** .  
  
5.  Modificare le impostazioni per la protezione estesa nel modo appropriato. La protezione estesa è disabilitata per impostazione predefinita.  Se queste voci non sono presenti, è possibile che sul computer corrente non sia in esecuzione una versione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che supporta la protezione estesa. Per altre informazioni, vedere [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
    ```  
          <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
          <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
    ```  
  
6.  Salvare il file.  
  
7.  Se è stata configurata una distribuzione con scalabilità orizzontale, ripetere questi passaggi per gli altri server di report presenti nella distribuzione.  
  
8.  Riavviare il server di report per cancellare qualsiasi sessione attualmente aperta.  
  
##  <a name="resolving-kerberos-authentication-errors-when-connecting-to-a-report-server"></a><a name="proxyfirewallRSWindowsNegotiate"></a> Risoluzione degli errori di autenticazione Kerberos durante la connessione a un Server di Report  
 In un server di report configurato per l'autenticazione con negoziazione o Kerberos, se si verifica un errore di autenticazione Kerberos la connessione client al server di report avrà esito negativo. Di seguito vengono riportate le condizioni che indicano la presenza di errori di autenticazione Kerberos:  
  
-   Il servizio del server di report è in esecuzione come account utente di dominio di Windows, ma non è stato registrato un nome SPN per l'account.  
  
-   Il server di report è configurato con l'impostazione **RSWindowsNegotiate** .  
  
-   Nell'intestazione dell'autenticazione della richiesta inviata al server di report dal browser viene scelta l'autenticazione Kerberos anziché l'autenticazione NTLM.  
  
 È possibile rilevare l'errore se è stata abilitata la registrazione Kerberos. Un altro sintomo dell'errore consiste in più richieste delle credenziali e successivamente nella visualizzazione di una finestra del browser vuota.  
  
 Per confermare che si è verificato un errore di autenticazione Kerberos, rimuovere < **RSWindowsNegotiate** /> dal file di configurazione, quindi riprovare a stabilire la connessione.  
  
 Dopo avere confermato la presenza del problema, è possibile risolverlo nei modi seguenti:  
  
-   Registrare un nome SPN per il servizio del server di report con l'account utente di dominio. Per altre informazioni, vedere [Registrare un nome dell'entità servizio &#40;SPN&#41; per un server di report](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).  
  
-   Modificare l'account del servizio in modo che venga eseguito con un account predefinito, ad esempio Servizio di rete. Gli account predefiniti eseguono il mapping del nome SPN HTTP al nome SPN dell'host, definito quando un computer viene collegato a una rete. Per altre informazioni, vedere [Configurare un account del servizio &#40;Gestione configurazione del server di report&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).
  
-   Utilizzare NTLM, che in genere funzionerà nei casi in cui l'autenticazione Kerberos ha esito negativo. Per usare NTLM, rimuovere **RSWindowsNegotiate** dal file RSReportServer.config e verificare che sia specificato solo **RSWindowsNTLM** . Se si sceglie questo approccio, è possibile continuare a utilizzare un account utente di dominio per il servizio del server di report anche se per tale account non si definisce un nome SPN.  
  
#### <a name="logging-information"></a>Informazioni di registrazione  
 Esistono diverse origini di informazioni di registrazione che possono consentire la risoluzione dei problemi relativi a Kerberos.  
  
##### <a name="user-account-control-attribute"></a>Attributo UserAccountControl  
 Stabilire se l'account di servizio di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dispone del set di attributi sufficiente in Active Directory. Esaminare il file di log di traccia del servizio Reporting Services per trovare il valore registrato per l'attributo UserAccountControl. Il valore registrato è in formato decimale. È necessario convertire il valore decimale in formato esadecimale e trovare quindi tale valore nell'argomento MSDN in cui viene descritto l'attributo UserAccountControl.  
  
-   La voce del log di traccia del servizio Reporting Services sarà simile alla seguente:  
  
    ```  
    appdomainmanager!DefaultDomain!8f8!01/14/2010-14:42:28:: i INFO: The UserAccountControl value for the service account is 590336  
    ```  
  
-   Un'opzione per la conversione del valore decimale in formato esadecimale è rappresentata dalla Calcolatrice di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Tale calcolatrice supporta diverse modalità che includono le opzioni "Dec" ed "Hex". Selezionare l'opzione "Dec", incollare o digitare il valore decimale nel file di log, quindi selezionare l'opzione "Hex".  
  
-   Fare quindi riferimento all'argomento [User-Account-Control Attribute](/windows/win32/adschema/a-useraccountcontrol) (Attributo UserAccountControl) per derivare l'attributo per l'account di servizio.  
  
##### <a name="spns-configured-in-active-directory-for-the-reporting-services-service-account"></a>Nomi SPN configurati in Active Directory per l'account di servizio di Reporting Services.  
 Per registrare i nomi SPN nel file di log di traccia del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , è possibile abilitare temporaneamente la caratteristica Protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Modificare il file di configurazione **rsreportserver.config** impostando gli elementi seguenti:  
  
    ```  
    <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>   
    <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>  
    ```  
  
-   Riavviare il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .

 Se non si vuole continuare a usare la Protezione estesa, ripristinare le impostazioni predefinite per i valori di configurazione e riavviare l'account di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
```  
<RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>  
<RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
```  
  
 Per altre informazioni, vedere [Protezione estesa per l'autenticazione con Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
#### <a name="how-the-browser-chooses-negotiated-kerberos-or-negotiated-ntlm"></a>Scelta tra l'autenticazione negoziata Kerberos o NTLM da parte del browser  
 Quando si utilizza Internet Explorer per connettersi al server di report, nell'intestazione di autenticazione viene specificata l'autenticazione negoziata Kerberos o NTLM. Di seguito vengono riportati i casi in cui viene utilizzata l'autenticazione NTLM anziché l'autenticazione Kerberos:  
  
-   Invio della richiesta a un server di report locale.  
  
-   Invio della richiesta a un indirizzo IP del computer del server di report anziché a un'intestazione host oppure a un nome del server.  
  
-   Blocco delle porte utilizzate per l'autenticazione Kerberos da parte del software firewall.  
  
-   Autenticazione Kerberos non abilitata nel sistema operativo di un server specifico.  
  
-   Presenza nel dominio di versioni precedenti di sistemi operativi client e server Windows che non supportano la caratteristica di autenticazione Kerberos disponibile nelle versioni più recenti del sistema operativo.  
  
 Internet Explorer potrebbe inoltre scegliere l'autenticazione negoziata Kerberos o NTLM in base alla configurazione delle impostazioni LAN, dell'URL e del proxy.  
  
###### <a name="report-server-url"></a>URL del server di report  
 Se nell'URL è incluso un nome di dominio completo o se nell'URL è specificato localhost, Internet Explorer seleziona l'autenticazione NTLM. Se nell'URL è specificato il nome di rete del computer, Internet Explorer seleziona l'autenticazione con negoziazione, che avrà esito positivo o negativo in base all'esistenza o meno di un nome SPN per l'account del servizio del server di report.  
  
###### <a name="lan-and-proxy-settings-on-the-client"></a>Impostazioni LAN e del proxy nel client  
 Le impostazioni LAN e del proxy specificate in Internet Explorer possono determinare la scelta dell'autenticazione NTLM rispetto all'autenticazione Kerberos. Poiché le impostazioni LAN e del proxy variano tra le organizzazioni, non è possibile tuttavia determinare con precisione le impostazioni esatte che contribuiscono agli errori di autenticazione Kerberos. Un'organizzazione potrebbe ad esempio applicare impostazioni del proxy che trasformano gli URL Intranet in URL con nome di dominio completo che vengono risolti durante le connessioni Internet. Se per tipi diversi di URL vengono utilizzati provider di autenticazione differenti, alcune connessioni potrebbero avere esito positivo sebbene sia previsto un esito negativo.  
  
 Se si verificano problemi di connessione che si ritiene siano dovuti a errori di autenticazione, è possibile provare a utilizzare combinazioni diverse di impostazioni LAN e del proxy per isolare il problema. In Internet Explorer le impostazioni LAN e del proxy sono presenti nella finestra di dialogo **Impostazioni rete locale (LAN)** . Per aprire tale finestra, fare clic su **Impostazioni LAN** nella scheda **Connessione** in **Opzioni Internet**.  
  
## <a name="external-resources"></a>Risorse esterne  
  
-   Per altre informazioni su server di report e Kerberos, vedere [Deploying a Business Intelligence Solution Using SharePoint, Reporting Services, and PerformancePoint Monitoring Server with Kerberos](https://www.kasperonbi.com/deploying-a-business-intelligence-solution-using-sharepoint-reporting-services-and-performancepoint-monitoring-server-with-kerberos/)(Distribuzione di una soluzione di Business Intelligence utilizzando SharePoint, Reporting Services e server di monitoraggio di PerformancePoint con Kerberos).  
  
## <a name="see-also"></a>Vedere anche  
 [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Configurare l'autenticazione di base nel server di report](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
 [Configurare l'autenticazione personalizzata o basata su form nel server di report](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)   
 [Protezione estesa per l'autenticazione con Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
