---
description: Autenticazione con il server di report
title: Autenticazione con il server di report | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- connections [Reporting Services], configuring
- connections [Reporting Services], accounts
- Windows authentication [Reporting Services]
- authentication [Reporting Services]
- Forms authentication
ms.assetid: 753c2542-0e97-4d8f-a5dd-4b07a5cd10ab
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8f682e468bffe704df57cb63b811682488909605
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595197"
---
# <a name="authentication-with-the-report-server"></a>Autenticazione con il server di report

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (SSRS) offre molte opzioni configurabili per l'autenticazione di utenti e applicazioni client rispetto al server di report. Per impostazione predefinita, il server di report utilizza l'autenticazione integrata di Windows e presuppone la presenza di relazioni attendibili per cui le risorse client e quelle di rete si trovano nello stesso dominio trusted. A seconda della topologia di rete e delle esigenze specifiche dell'organizzazione, è possibile personalizzare il protocollo di autenticazione usato per l'autenticazione integrata di Windows, usare l'autenticazione di base o l'estensione di autenticazione basata su moduli personalizzata indicata. Ogni tipo di autenticazione può essere singolarmente abilitato o disabilitato. È possibile abilitare più di un tipo di autenticazione se si desidera che il server di report accetti richieste di più tipi.
  
 Tutti gli utenti o le applicazioni che richiedono l'accesso al contenuto oppure a operazioni del server di report devono essere autenticati prima che ne venga consentito l'accesso.  
  
## <a name="authentication-types"></a>Tipi di autenticazione  
 Tutti gli utenti o le applicazioni che richiedono l'accesso al contenuto oppure a operazioni del server di report devono essere autenticati utilizzando il tipo di autenticazione configurato nel server di report prima che ne venga consentito l'accesso. Nella tabella seguente vengono descritti i tipi di autenticazione supportati da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
|Nome del tipo di autenticazione|Valore del livello di autenticazione HTTP|Utilizzato per impostazione predefinita|Descrizione|  
|-----------------------------|-------------------------------------|---------------------|-----------------|  
|RSWindowsNegotiate|Negotiate|Sì|Viene dapprima effettuato il tentativo di utilizzare Kerberos per l'autenticazione integrata di Windows. Se Active Directory non è in grado di concedere un ticket per la richiesta client al server di report, viene impostata nuovamente l'autenticazione NTML. L'autenticazione verrà reimpostata su NTLM solo se il ticket non è disponibile. Se il primo tentativo genera un errore anziché un ticket mancante, il server di report non esegue un secondo tentativo.|  
|RSWindowsNTLM|NTLM|Sì|Utilizza NTLM per l'autenticazione integrata di Windows.<br /><br /> Le credenziali non verranno delegate né rappresentate in altre richieste. Le richieste successive seguiranno una nuova sequenza In attesa/Risposta. A seconda delle impostazioni di sicurezza della rete, è possibile che a un utente vengano richieste le credenziali o che la richiesta di autenticazione venga gestita in modo trasparente.|  
|RSWindowsKerberos|Kerberos|No|Utilizza Kerberos per l'autenticazione integrata di Windows. È necessario configurare Kerberos impostando i nomi SPN (Service Principle Name) per gli account del servizio, per cui è necessario disporre dei privilegi di amministratore di dominio. Se viene configurata una delega dell'identità utilizzando Kerberos, il token dell'utente che richiede un report può essere utilizzato anche in una connessione aggiuntiva alle origini dati esterne che forniscono i dati ai report.<br /><br /> Prima di specificare RSWindowsKerberos, assicurarsi che il tipo di browser in uso lo supporti. Se si usa Microsoft Edge o Internet Explorer, l'autenticazione Kerberos è supportata solo tramite negoziazione. Microsoft Edge o Internet Explorer non formulerà una richiesta di autenticazione che specifica direttamente Kerberos.|  
|RSWindowsBasic|Basic|No|L'autenticazione di base è definita nel protocollo HTTP e può essere utilizzata solo per autenticare richieste HTTP al server di report.<br /><br /> Le credenziali vengono passate nella richiesta HTTP in codifica in base 64. Se si usa l'autenticazione di base, applicare il protocollo TLS (Transport Layer Security), precedentemente noto come SSL (Secure Sockets Layer), per crittografare le informazioni relative all'account utente prima di inviarle sulla rete. SSL fornisce un canale crittografato per l'invio di una richiesta di connessione dal client al server di report mediante una connessione TCP/IP HTTP. Per altre informazioni, vedere [Using SSL to Encrypt Confidential Data (Uso di SSL per crittografare dati riservati)](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) sul sito Web [!INCLUDE[msCoName](../../includes/msconame-md.md)] TechNet.|  
|Personalizzato|(anonimo)|No|L'autenticazione anonima indica al server di report di ignorare l'intestazione di autenticazione nelle richieste HTTP. Il server di report accetta tutte le richieste, ma esegue una chiamata a un'autenticazione basata su form di [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] fornita per autenticare l'utente.<br /><br /> Specificare **Personalizzato** solo se si distribuisce un modulo di autenticazione personalizzato che gestisce tutte le richieste di autenticazione sul server di report. Non è possibile utilizzare il tipo di autenticazione Custom con l'estensione di autenticazione di Windows predefinita.|  
  
## <a name="unsupported-authentication-methods"></a>Metodi di autenticazione non supportati  
 I metodi e le richieste di autenticazione seguenti non sono supportati.  
  
|Metodo di autenticazione|Spiegazione|  
|---------------------------|-----------------|  
|Anonima|Il server di report non accetterà richieste non autenticate da un utente anonimo, tranne nel caso di distribuzioni che includono un'estensione di autenticazione personalizzata.<br /><br /> Generatore report accetterà richieste non autenticate se si abilita l'accesso a Generatore report in un server di report configurato per l'autenticazione di base.<br /><br /> In tutti gli altri casi, le richieste anonime vengono rifiutate con un messaggio di errore di accesso negato con stato HTTP 401 prima che la richiesta raggiunga [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. I client che ricevono l'errore di accesso negato 401 devono riformulare la richiesta con un tipo di autenticazione valido.|  
|Tecnologie Single Sign-On (SSO)|In [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]non è disponibile il supporto nativo per le tecnologie Single Sign-On. Se si desidera utilizzare una tecnologia di questo tipo, è necessario creare un'estensione di autenticazione personalizzata.<br /><br /> L'ambiente host del server di report non supporta i filtri ISAPI. Se la tecnologia SSO utilizzata è implementata come filtro ISAPI, utilizzare il supporto incorporato di ISA Server per RSASecueID o il protocollo RADIUS. In caso contrario, è possibile creare un filtro ISAPI di ISA Server o un modulo HTTPModule per RS. È tuttavia consigliabile utilizzare direttamente ISA Server.|  
|Passport|Non supportato in SQL Server Reporting Services.|  
|Digest|Non supportato in SQL Server Reporting Services.|  
  
## <a name="configuration-of-authentication-settings"></a>Configurazione delle impostazioni di autenticazione  
 Le impostazioni di autenticazione vengono configurate per la sicurezza predefinita quando l'URL del server di report è riservato. Se queste impostazioni vengono modificate in modo errato, il server di report restituisce il messaggio di errore di accesso negato HTTP 401 per le richieste HTTP che non possono essere autenticate. Prima di scegliere un tipo di autenticazione, è necessario conoscere il tipo di supporto per l'autenticazione di Windows disponibile nella rete. È necessario specificare almeno un tipo di autenticazione. Per RSWindows, è possibile specificare più tipi di autenticazione. I tipi di autenticazione RSWindows (ovvero, **RSWindowsBasic**, **RSWindowsNTLM**, **RSWindowsKerberos**, e **RSWindowsNegotiate**) si escludono a vicenda con Personalizzato.  
  
> [!IMPORTANT]  
>  In Reporting Services le impostazioni specificate non vengono convalidate per determinare se sono corrette per l'ambiente informatico in uso. È possibile che la sicurezza predefinita non funzioni in una determinata installazione o che vengano specificate impostazioni di configurazione non valide per l'infrastruttura di sicurezza implementata. Per questo motivo, è importante testare con attenzione la distribuzione del server di report in un ambiente di test controllato prima di renderla disponibile per l'organizzazione in senso lato.  
  
 Il servizio Web ReportServer e [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] usano sempre lo stesso tipo di autenticazione. Non è possibile configurare tipi di autenticazione diversi per le aree di caratteristiche del servizio del server di report. Se di dispone di una distribuzione con scalabilità orizzontale, duplicare tutte le modifiche in tutti i nodi della distribuzione. Non è possibile configurare nodi diversi nella stessa topologia con scalabilità orizzontale per l'utilizzo di tipi di autenticazione diversi.  
  
 L'elaborazione in background non accetta richieste degli utenti finali, ma autentica tutte le richieste ai fini dell'esecuzione automatica. Durante questo tipo di elaborazione viene sempre utilizzata l'autenticazione di Windows e le richieste vengono autenticate utilizzando il servizio del server di report o l'account di esecuzione automatica, se è configurato.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Configurare l'autenticazione di Windows nel server di report](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
  
-   [Configurare l'autenticazione di base nel server di report](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)  
  
-   [Configurare l'autenticazione personalizzata o basata su form nel server di report](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizioni delle attività|Collegamenti|  
|-----------------------|-----------|  
|Configurare il tipo di autenticazione integrata di Windows.|[Configurare l'autenticazione di Windows nel server di report](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)|  
|Configurare il tipo di autenticazione di base.|[Configurare l'autenticazione di base nel server di report](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)|  
|Configurare l'autenticazione basata su form o un tipo di autenticazione personalizzata.|[Configurare l'autenticazione personalizzata o basata su form nel server di report](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)|  
|Consentire a [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] di gestire lo scenario basato su autenticazione personalizzata.|[Configurare il portale Web per il passaggio di cookie di autenticazione personalizzati](configure-the-web-portal-to-pass-custom-authentication-cookies.md)|  

## <a name="next-steps"></a>Passaggi successivi

[Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
[File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Creare e gestire assegnazioni di ruolo](../../reporting-services/security/create-and-manage-role-assignments.md)   
[Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
[Implementazione di un'estensione di sicurezza](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)   
[Configurare connessioni TLS in un server di report in modalità nativa](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)   
[Panoramica sulle estensioni di sicurezza](../../reporting-services/extensions/security-extension/security-extensions-overview.md)   
[Autenticazione in Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)   
[Autorizzazione in Reporting Services](../../reporting-services/extensions/security-extension/authorization-in-reporting-services.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)