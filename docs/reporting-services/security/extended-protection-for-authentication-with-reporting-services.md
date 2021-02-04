---
description: Protezione estesa per l'autenticazione con Reporting Services
title: Protezione estesa per l'autenticazione con Reporting Services | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
ms.assetid: eb5c6f4a-3ed5-430b-a712-d5ed4b6b9b2b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b13ca53aad69647aee5158d86de2cfb16ac04710
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076103"
---
# <a name="extended-protection-for-authentication-with-reporting-services"></a>Protezione estesa per l'autenticazione con Reporting Services

  La protezione estesa consiste in un set di miglioramenti apportati alle versioni recenti del sistema operativo [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. La protezione estesa migliora la protezione di credenziali e autenticazione da parte delle applicazioni. La funzionalità non fornisce direttamente la protezione contro attacchi specifici quale l'inoltro delle credenziali, ma rende disponibile un'infrastruttura per le applicazioni, ad esempio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , per l'imposizione della protezione estesa per l'autenticazione.  
  
 I principali miglioramenti introdotti riguardanti l'autenticazione che fanno parte della protezione estesa sono l'associazione di canale e l'associazione al servizio. Nell'associazione di canale è utilizzato un token CBT (Channel Binding Token) per verificare che il canale stabilito tra due endpoint non sia compromesso. Nell'associazione al servizio sono utilizzati nomi SPN (Service Principal Name) per convalidare la destinazione desiderata dei token di autenticazione. Per informazioni complementari sulla protezione estesa, vedere [Integrated Windows Authentication with Extended Protection (autenticazione integrata di Windows con protezione estesa)](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90)).  
  
SQL Server Reporting Services (SSRS) supporta e applica la protezione estesa abilitata nel sistema operativo e configurata in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Per impostazione predefinita, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] accetta le richieste che specificano l'autenticazione Negotiate o NTLM ed è pertanto in grado di usufruire del supporto della protezione estesa offerto dal sistema operativo e dalle funzionalità di protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Per impostazione predefinita, Windows non abilita la protezione estesa. Per informazioni su come abilitare la protezione estesa in Windows, vedere [Protezione estesa per l'autenticazione](https://support.microsoft.com/topic/microsoft-security-advisory-extended-protection-for-authentication-7dd2ee6d-c2e9-3484-2d8e-466261d3f0c7). Per garantire che l'autenticazione abbia esito positivo, è necessario che il sistema operativo e lo stack di autenticazione del client supportino entrambi la protezione estesa. Per i sistemi operativi meno recenti potrebbe essere necessario installare più aggiornamenti per ottenere un computer in grado di utilizzare appieno la protezione estesa. Per informazioni sugli ultimi sviluppi della protezione estesa, vedere la pagina relativa alle [informazioni aggiornate sulla protezione estesa](/previous-versions/sql/sql-server-2008/dd146365(v=sql.100)).  

## <a name="reporting-services-extended-protection-overview"></a>Cenni preliminari sulla protezione estesa di Reporting Services

SSRS supporta e impone la protezione estesa abilitata nel sistema operativo. Se il sistema operativo non supporta la protezione estesa o la funzionalità non è stata abilitata nel sistema operativo, l'autenticazione eseguita dalla funzionalità di protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] avrà esito negativo. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] La protezione estesa richiede anche un certificato TLS/SSL. Per altre informazioni, vedere [Configurare connessioni TLS in un server di report in modalità nativa](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)  
  
> [!IMPORTANT]  
>  Per impostazione predefinita [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non abilita la protezione estesa. La funzionalità può essere attivata modificando il file di configurazione **rsreportserver.config** o usando le API di WMI che consentono di aggiornare il file di configurazione. SSRS non offre un'interfaccia utente per la modifica o la visualizzazione delle impostazioni relative alla protezione estesa. Per altre informazioni, vedere la sezione relativa alle [impostazioni di configurazione](#ConfigurationSettings) in questo argomento.  
  
 I problemi comuni che si verificano a causa della modifica delle impostazioni relative alla protezione estesa o alla configurazione non corretta delle impostazioni non vengono esposti con messaggi o finestre di dialogo di errore. I problemi correlati alla configurazione e alla compatibilità della protezione estesa causano problemi di autenticazione ed errori nei log di traccia di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!IMPORTANT]
>  È possibile che alcune tecnologie di accesso ai dati non supportino la protezione estesa. Per connettersi alle origini dati di SQL Server e al database del catalogo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene utilizzata una tecnologia di accesso ai dati. Un errore della tecnologia di accesso ai dati per supportare la protezione estesa influisce su [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nei seguenti modi:  
> 
>  -   Per l'istanza di SQL Server che esegue il database del catalogo di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non è possibile abilitare la protezione estesa. In caso contrario, il server di report non sarà in grado di connettersi al database del catalogo e restituirà errori di autenticazione.  
> -   Per le istanze di SQL Server usate come origini dati di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non è possibile abilitare la protezione estesa. In caso contrario, i tentativi di connessione effettuati dal server di report alle origini dati dei report non riusciranno e verranno restituiti errori di autenticazione.  
> 
>  La documentazione per una tecnologia di accesso ai dati deve disporre di informazioni sul supporto per la protezione estesa.  
  
### <a name="upgrade"></a>Aggiornamento  
  
-   L'aggiornamento di un server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a SQL Server 2016 determina l'aggiunta delle impostazioni di configurazione con i valori predefiniti al file **rsreportserver.config**. Le impostazioni già presenti verranno mantenute nel file **rsreportserver.config** durante l'installazione di SQL Server 2016.  
  
-   Quando le impostazioni di configurazione vengono aggiunte al file di configurazione **rsreportserver.config** , la funzionalità di protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è disattivata per impostazione predefinita ed è necessario abilitarla come descritto in questo argomento. Per altre informazioni, vedere la sezione relativa alle [impostazioni di configurazione](#ConfigurationSettings) in questo argomento.  
  
-   Il valore predefinito per l'impostazione **RSWindowsExtendedProtectionLevel** è **Off**.  
  
-   Il valore predefinito per l'impostazione **RSWindowsExtendedProtectionScenario** è **Proxy**.  
  
-   Upgrade Advisor non verifica se il sistema operativo o l'installazione corrente di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dispone del supporto di protezione estesa abilitato.  
  
### <a name="what-reporting-services-extended-protection-does-not-cover"></a>Elementi non coperti dalla protezione estesa di Reporting Services  
 Le aree e gli scenari funzionali seguenti non sono supportati dalla funzionalità di protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Gli autori di estensioni di sicurezza personalizzate di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] devono aggiungere il supporto per la protezione estesa all'estensione di sicurezza personalizzata.  
  
-   Per supportare la protezione estesa, i componenti di terze parti aggiunti o usati da un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] devono essere aggiornati dal fornitore di terze parti. Per ulteriori informazioni, contattare il fornitore di terze parti.  
  
## <a name="deployment-scenarios-and-recommendations"></a>Scenari di distribuzione e indicazioni  
 Negli scenari seguenti vengono illustrate distribuzioni e topologie diverse, nonché la configurazione consigliata per proteggerle con la protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
### <a name="direct"></a>Connessione diretta  
 In questo scenario viene descritta la connessione diretta a un server di report, ad esempio un ambiente Intranet.  
  
|Scenario|Diagramma dello scenario|Modalità di protezione|  
|--------------|----------------------|-------------------|  
|Comunicazione TLS diretta.<br /><br /> Il server di report imporrà l'associazione di canale dal client al server di report.|![RS_ExtendedProtection_DirectSSL](../../reporting-services/security/media/rs-extendedprotection-directssl.gif "RS_ExtendedProtection_DirectSSL")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Direct**.<br /><br /> <br /><br /> \- L'associazione al servizio non è necessaria perché il canale TLS verrà usato per l'associazione di canale.|  
|Comunicazione HTTP diretta. Il server di report imporrà l'associazione al servizio dal client al server di report.|![RS_ExtendedProtection_Direct](../../reporting-services/security/media/rs-extendedprotection-direct.gif "RS_ExtendedProtection_Direct")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Any**.<br /><br /> <br /><br /> \- Non esiste alcun canale TLS, pertanto non è possibile alcuna imposizione dell'associazione di canale.<br /><br /> \- L'associazione al servizio può essere convalidata, ma non rappresenta tuttavia una difesa completa senza l'associazione di canale. Se si usa solo l'associazione al servizio si avrà unicamente una protezione contro le minacce di base.|  
  
### <a name="proxy-and-network-load-balancing"></a>Proxy e bilanciamento del carico di rete  
 Le applicazioni client si connettono a un dispositivo o a un software che esegue TLS e passa le credenziali al server per l'autenticazione, ad esempio in un ambiente Extranet, Internet o in una rete Intranet sicura. Il client si connette a un proxy o a tutti i client che utilizzano un proxy.  
  
 La situazione è analoga quando si utilizza un dispositivo per il bilanciamento del carico di rete (NLB).  
  
|Scenario|Diagramma dello scenario|Modalità di protezione|  
|--------------|----------------------|-------------------|  
|Comunicazione HTTP. Il server di report imporrà l'associazione al servizio dal client al server di report.|![RS_ExtendedProtection_Indirect](../../reporting-services/security/media/rs-extendedprotection-indirect.gif "RS_ExtendedProtection_Indirect")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Proxy|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Any**.<br /><br /> <br /><br /> \- Non esiste alcun canale TLS, pertanto non è possibile alcuna imposizione dell'associazione di canale.<br /><br /> \- Per garantire che l'associazione al servizio venga imposta correttamente, è necessario configurare il server di report in modo che sia a conoscenza del nome del server proxy.|  
|Comunicazione HTTP.<br /><br /> Il server di report imporrà l'associazione di canale dal client al proxy e l'associazione al servizio dal client al server di report.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Proxy|Set <br />                    **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Proxy**.<br /><br /> <br /><br /> \- Il canale TLS al proxy è disponibile, pertanto l'associazione di canale al proxy può essere imposta.<br /><br /> \- Anche l'associazione al servizio può essere imposta.<br /><br /> \- Il nome del proxy deve essere noto al server di report e a tale scopo l'amministratore del server di report deve creare una prenotazione di URL per il proxy con un'intestazione host oppure configurare il nome del proxy nella voce **BackConnectionHostNames** del Registro di sistema di Windows.|  
|Comunicazione HTTPS indiretta con un proxy sicuro. Il server di report imporrà l'associazione di canale dal client al proxy e l'associazione al servizio dal client al server di report.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Proxy|Set <br />                    **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Proxy**.<br /><br /> <br /><br /> \- Il canale TLS al proxy è disponibile, pertanto l'associazione di canale al proxy può essere imposta.<br /><br /> \- Anche l'associazione al servizio può essere imposta.<br /><br /> \- Il nome del proxy deve essere noto al server di report e a tale scopo l'amministratore del server di report deve creare una prenotazione di URL per il proxy con un'intestazione host oppure configurare il nome del proxy nella voce **BackConnectionHostNames** del Registro di sistema di Windows.|  
  
### <a name="gateway"></a>Gateway  
 In questo scenario vengono descritte applicazioni client che si connettono a un dispositivo o a un software che esegue TLS e autentica l'utente. Il dispositivo o il software rappresenta quindi il contesto utente o un contesto utente diverso prima di inviare una richiesta al server di report.  
  
|Scenario|Diagramma dello scenario|Modalità di protezione|  
|--------------|----------------------|-------------------|  
|Comunicazione HTTP indiretta.<br /><br /> Il gateway imporrà l'associazione di canale dal client al gateway. È disponibile un'associazione al servizio dal gateway al server di report.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Dispositivo del gateway|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Any**.<br /><br /> <br /><br /> \- L'associazione di canale da client a server di report non è possibile perché il gateway rappresenta un contesto e crea quindi un nuovo token NTLM.<br /><br /> \- Non è disponibile alcun TLS dal gateway al server di report, pertanto l'associazione di canale non può essere imposta.<br /><br /> \- L'associazione al servizio può essere imposta.<br /><br /> \- Per imporre l'associazione di canale, il dispositivo del gateway deve essere configurato dall'amministratore.|  
|Comunicazione HTTPS indiretta con un gateway sicuro. Il gateway imporrà il binding di canale dal client al gateway e il server di report imporrà il binding di canale dal gateway al server di report.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Dispositivo del gateway|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Direct**.<br /><br /> <br /><br /> \- L'associazione di canale da client a server di report non è possibile perché il gateway rappresenta un contesto e crea quindi un nuovo token NTLM.<br /><br /> \- TLS dal gateway al server di report indica che l'associazione di canale può essere imposta.<br /><br /> \- L'associazione al servizio non è necessaria.<br /><br /> \- Per imporre l'associazione di canale, il dispositivo del gateway deve essere configurato dall'amministratore.|  
  
### <a name="combination"></a>Combinazione  
 In questo scenario viene descritto un ambiente Extranet o Internet in cui il client si connette a un proxy. Questo in combinazione con un ambiente Intranet in cui un client si connette al server di report.  
  
|Scenario|Diagramma dello scenario|Modalità di protezione|  
|--------------|----------------------|-------------------|  
|Accesso indiretto e diretto da client a servizio del server di report senza TLS su una delle connessioni da client a proxy o da client a server di report.|1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Proxy<br /><br /> 4) Applicazione client|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Any**.<br /><br /> <br /><br /> \- L'associazione al servizio da client a server di report può essere imposta.<br /><br /> \- Il nome del proxy deve essere noto al server di report e a tale scopo l'amministratore del server di report deve creare una prenotazione di URL per il proxy con un'intestazione host oppure configurare il nome del proxy nella voce **BackConnectionHostNames** del Registro di sistema di Windows.|  
|Accesso indiretto e diretto da client a server di report in cui il client stabilisce una connessione TLS al proxy o al server di report.|![RS_ExtendedProtection_CombinationSSL](../../reporting-services/security/media/rs-extendedprotection-combinationssl.gif "RS_ExtendedProtection_CombinationSSL")<br /><br /> 1) Applicazione client<br /><br /> 2) Server di report<br /><br /> 3) Proxy<br /><br /> 4) Applicazione client|Impostare **RSWindowsExtendedProtectionLevel** su **Allow** o **Require**.<br /><br /> Impostare **RSWindowsExtendedProtectionScenario** su **Proxy**.<br /><br /> <br /><br /> \- L'associazione di canale può essere usata<br /><br /> \- Il nome del proxy deve essere noto al server di report e a tale scopo l'amministratore del server di report deve creare una prenotazione di URL per il proxy con un'intestazione host oppure configurare il nome del proxy nella voce **BackConnectionHostNames** del Registro di sistema di Windows.|  
  
## <a name="configuring-reporting-services-extended-protection"></a>Configurazione della protezione estesa di Reporting Services  
 Il file **rsreportserver.config** contiene i valori di configurazione che controllano il comportamento della protezione estesa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Per altre informazioni sull'utilizzo e la modifica del file **rsreportserver.config** , vedere [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md). Le impostazioni relative alla protezione estesa possono inoltre essere modificate e controllate utilizzando le API di WMI. Per altre informazioni, vedere [Metodo SetExtendedProtectionSettings &#40;MSReportServer_ConfigurationSetting WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md).  
  
 Quando la convalida delle impostazioni di configurazione ha esito negativo, i tipi di autenticazione **RSWindowsNTLM**, **RSWindowsKerberos** e **RSWindowsNegotiate** vengono disabilitati nel server di report.  
  
###  <a name="configuration-settings-for-reporting-services-extended-protection"></a><a name="ConfigurationSettings"></a> Impostazioni di configurazione per la protezione estesa di Reporting Services  
 Nella tabella seguente vengono fornite le informazioni sulle impostazioni di configurazione incluse nel file **rsreportserver.config** per la protezione estesa.  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**RSWindowsExtendedProtectionLevel**|Specifica il grado di imposizione della protezione estesa. I valori validi sono:<br /><br /> **Off**: Valore predefinito. Specifica che non viene eseguita alcuna verifica dell'associazione di canale o dell'associazione al servizio.<br /><br /> **Allow** supporta la protezione estesa ma non la richiede.  Specifica:<br /><br /> \- La protezione estesa verrà imposta per le applicazioni client in esecuzione nei sistemi operativi che supportano la protezione estesa. La modalità di imposizione della protezione è determinata dall'impostazione **RsWindowsExtendedProtectionScenario**<br /><br /> \- L'autenticazione sarà consentita per le applicazioni client in esecuzione nei sistemi operativi che non supportano la protezione estesa.<br /><br /> **Require** specifica:<br /><br /> \- La protezione estesa verrà imposta per le applicazioni client in esecuzione nei sistemi operativi che supportano la protezione estesa.<br /><br /> \- L'autenticazione **non** sarà consentita per le applicazioni in esecuzione nei sistemi operativi che non supportano la protezione estesa.|  
|**RsWindowsExtendedProtectionScenario**|Specifica i formati di protezione estesa che vengono convalidati: associazione di canale, associazione a servizio o entrambe. I valori validi sono:<br /><br /> **Proxy**: Valore predefinito. Specifica:<br /><br /> \- Autenticazione NTLM, Kerberos e Negotiate di Windows quando è presente un token di associazione di canale.<br /><br /> \- L'associazione al servizio viene imposta.<br /><br /> **Any** specifica:<br /><br /> \- L'autenticazione NTLM, Kerberos e Negotiate di Windows e l'associazione di canale non sono necessari.<br /><br /> \- L'associazione al servizio viene imposta.<br /><br /> **Direct** specifica:<br /><br /> \- Autenticazione NTLM, Kerberos e Negotiate quando è presente un token CBT. È anche presente una connessione TLS al servizio corrente e il token CBT per la connessione TLS corrisponde al token CBT del token NTLM, Kerberos o Negotiate.<br /><br /> \- L'associazione al servizio non viene imposta.<br /><br /> <br /><br /> Nota: l'impostazione **RsWindowsExtendedProtectionScenario** viene ignorata se **RsWindowsExtendedProtectionLevel** è impostato su **OFF**.|  
  
 Voci di esempio nel file di configurazione **rsreportserver.config** :  
  
```  
<Authentication>  
         <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
         <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionLevel>  
</Authentication>  
```  
  
## <a name="service-binding-and-included-spns"></a>Associazione al servizio e nomi SPN inclusi  
 L'associazione al servizio utilizza nomi SPN (Service Principal Name) per convalidare la destinazione desiderata dei token di autenticazione. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa le informazioni sulla prenotazione di URL esistenti per compilare un elenco di nomi SPN considerati validi. L'utilizzo delle informazioni sulla prenotazione di URL per la convalida dei nomi SPN e delle prenotazioni di URL consente agli amministratori di sistema di gestire entrambi gli elementi da un'unica posizione.  
  
 L'elenco di nomi SPN validi viene aggiornato all'avvio del server di report, al momento della modifica delle impostazioni di configurazione per la protezione estesa o quando viene riciclato il dominio applicazione.  
  
 L'elenco di nomi SPN validi è specifico di ogni applicazione. Ad esempio, Gestione report e il server di report necessitano ognuno di un elenco diverso di nomi SPN validi elaborati.  
  
 L'elenco di nomi SPN validi elaborati per un'applicazione è determinato dai fattori seguenti:  
  
-   Ogni prenotazione di URL.  
  
-   Ogni nome SPN recuperato dal controller di dominio per l'account del servizio Reporting Services.  
  
-   Se una prenotazione di URL include caratteri jolly ("*" o "+"), il server di report aggiungerà ogni voce dalla raccolta Host.  
  
### <a name="hosts-collection-sources"></a>Origini della raccolta Host.  
 Nella tabella seguente vengono elencate le origini potenziali per la raccolta Host.  
  
|Tipo di origine|Descrizione|  
|--------------------|-----------------|  
|ComputerNameDnsDomain|Nome di dominio DNS assegnato al computer locale. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome di dominio DNS del server virtuale del cluster.|  
|ComputerNameDnsFullyQualified|Nome DNS completo che identifica in modo univoco il computer locale. Questo nome è una combinazione del nome host DNS e del nome di dominio DNS nel formato *NomeHost*.*NomeDominio*. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome DNS completo del server virtuale del cluster.|  
|ComputerNameDnsHostname|Nome host DNS del computer locale. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome host DNS del server virtuale del cluster.|  
|ComputerNameNetBIOS|Nome del NetBIOS del computer locale. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome del NetBIOS del server virtuale del cluster.|  
|ComputerNamePhysicalDnsDomain|Nome di dominio DNS assegnato al computer locale. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome di dominio DNS del computer locale, anziché il nome del server virtuale del cluster.|  
|ComputerNamePhysicalDnsFullyQualified|Nome DNS completo che identifica in modo univoco il computer. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome DNS completo del computer locale, anziché il nome del server virtuale del cluster.<br /><br /> Il nome DNS completo è una combinazione del nome host DNS e del nome di dominio DNS nel formato *NomeHost*.*NomeDominio*.|  
|ComputerNamePhysicalDnsHostname|Nome host DNS del computer locale. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome host DNS del computer locale, anziché il nome del server virtuale del cluster.|  
|ComputerNamePhysicalNetBIOS|Nome del NetBIOS del computer locale. Se il computer locale è un nodo di un cluster, verrà utilizzato il nome del NetBIOS del computer locale, anziché il nome del server virtuale del cluster.|  
  
Per altre informazioni, vedere [Registrare un nome dell'entità servizio &#40;SPN&#41; per un server di report](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md) e [Informazioni su prenotazioni e registrazione URL &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).  
  
## <a name="next-steps"></a>Passaggi successivi

[Connessione al motore di database mediante la protezione estesa](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)   
[Extended Protection for Authentication Overview](/previous-versions/dotnet/netframework-3.5/dd767318(v=vs.90))   
[Integrated Windows Authentication with Extended Protection (autenticazione integrata di Windows con protezione estesa)](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))   
[Avviso sicurezza Microsoft: Protezione estesa per l'autenticazione](https://go.microsoft.com/fwlink/?LinkId=179923)   
[Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)   
[File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Metodo SetExtendedProtectionSettings &#40;MSReportServer_ConfigurationSetting WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
