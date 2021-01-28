---
title: Prenotazioni e registrazione URL (Configuration Manager) | Microsoft Docs
description: Gli URL per le applicazioni di Reporting Services vengono definiti come prenotazioni URL in HTTP.SYS.
ms.date: 01/16/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- URL reservations
- URL registration
- Report Server service, URL reservations
ms.assetid: c2c460c3-e749-4efd-aa02-0f8a98ddbc76
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1203c1190b0c14a4d5d9c3f7218adc8d6ab5d57a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595288"
---
# <a name="about-url-reservations-and-registration--report-server-configuration-manager"></a>Informazioni su prenotazioni e registrazione URL (Gestione configurazione del server di report)
  Gli URL per le applicazioni di Reporting Services vengono definiti come prenotazioni URL in HTTP.SYS. Una prenotazione URL definisce la sintassi di un endpoint dell'URL in un'applicazione Web. Le prenotazioni URL vengono definite sia per il servizio Web ReportServer sia per il portale Web quando si configurano le applicazioni nel server di report. Le prenotazioni URL vengono create automaticamente quando si configurano gli URL tramite il programma di installazione o lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Le prenotazioni URL vengono create dal programma di installazione utilizzando valori predefiniti. Se il programma di installazione usa la configurazione predefinita, vengono prenotati due URL, uno per il servizio Web ReportServer e l'altro per il portale Web. È possibile utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per aggiungere altri URL o modificare quelli predefiniti creati dal programma di installazione.  
  
-   Lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] crea una prenotazione URL basata sull'URL specificato nella pagina **URL servizio Web** o **URL del portale Web** dello strumento.  
  
 Sia il programma di installazione che lo strumento assegneranno inoltre autorizzazioni per l'URL al servizio del server di report, verificheranno la presenza di istanze duplicate e aggiungeranno la prenotazione URL a HTTP.SYS. Non creare o modificare mai una prenotazione URL di Reporting Services utilizzando direttamente HttpCfg.exe o un altro strumento. Se si ignora un passaggio o si imposta un valore non valido, si verificheranno problemi di difficile identificazione o correzione.  
  
> [!NOTE]  
> HTTP.SYS è un componente del sistema operativo che rimane in attesa delle richieste di rete e ne esegue quindi il routing a una coda di richieste. In questa versione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] HTTP.SYS definisce e gestisce la coda di richieste per il servizio Web ReportServer e per il portale Web. Internet Information Services (IIS) non viene più utilizzato per ospitare le applicazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o accedervi. Per altre informazioni sulla funzionalità HTTP.SYS, vedere [HTTP Server API](/windows/win32/http/http-api-start-page).  
  
##  <a name="urls-in-reporting-services"></a><a name="ReportingServicesURLs"></a> URL in Reporting Services  
 In un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è possibile accedere agli strumenti, alle applicazioni e agli elementi seguenti tramite URL:  
  
-   servizio Web ReportServer  
  
-   Portale Web  
  
-   Report pubblicati in un server di report  
  
 Non accedere tramite URL come elementi autonomi ad altri elementi pubblicati indirizzabili tramite URL, quali origini dati condivise. Se presentati in una finestra del browser, tali elementi non vengono visualizzati in un formato significativo dal server di report.  
  
> [!NOTE]  
> Questo articolo non descrive l'accesso tramite URL a report specifici archiviati nel server di report. Per altre informazioni sull'accesso tramite URL a questi elementi, vedere [Accedere agli elementi del server di report usando l'accesso tramite URL](../../reporting-services/access-report-server-items-using-url-access.md).  
  
##  <a name="url-reservation-and-registration"></a><a name="URLreservation"></a> Prenotazione e registrazione URL  
 Una prenotazione URL definisce gli URL che possono essere utilizzati per accedere a un'applicazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] riserverà uno o più URL per il servizio Web ReportServer e [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] in HTTP.SYS e li registra quindi all'avvio del servizio. Aggiungendo parametri all'URL, è possibile aprire i report tramite il servizio Web. Le prenotazioni e la registrazione vengono forniti da HTTP.SYS. Per altre informazioni, vedere [Prenotazioni, registrazione e routing dello spazio dei nomi](/windows/win32/http/namespace-reservations-registrations-and-routing).  
  
 Una *prenotazione URL* è un processo tramite cui un endpoint dell'URL a un'applicazione Web viene creato e archiviato in HTTP.SYS. HTTP.SYS è il repository comune di tutte le prenotazioni URL definite in un computer e determina un set di regole comuni che garantiscono l'univocità delle prenotazioni URL.  
  
 La *registrazione URL* viene eseguita all'avvio del servizio. Viene creata la coda di richieste e HTTP.SYS inizia a eseguire il routing delle richieste alla coda. Prima che le richieste indirizzate all'endpoint URL vengano aggiunte alla coda, è necessario che l'endpoint sia registrato. All'avvio del servizio del server di report, verranno registrati tutti gli URL prenotati per tutte le applicazioni attivate. Di conseguenza, il servizio Web deve essere attivato affinché venga eseguita la registrazione. Se si imposta la proprietà **WebServiceAndHTTPAccessEnabled** su **False** nel facet Configurazione superficie di attacco per Reporting Services della gestione basata su criteri, l'URL per il servizio Web non verrà registrato all'avvio del servizio.  
  
 La registrazione degli URL viene annullata se si arresta il servizio o si ricicla il dominio applicazione di [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] o del servizio Web. Se si modifica una prenotazione URL mentre il servizio è in esecuzione, il server di report riciclerà immediatamente il dominio applicazione per consentire l'annullamento della registrazione dell'URL precedente e l'utilizzo del nuovo URL.  
  
 Il concetto di prenotazione URL e il modo in cui questa è correlata agli indirizzi URL utilizzati per le applicazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] possono essere illustrati tramite alcuni semplici esempi. Un aspetto essenziale da osservare è che la prenotazione URL ha una sintassi diversa dall'URL usato per accedere all'applicazione:  
  
|Prenotazione URL in HTTP.SYS|URL|Spiegazione|  
|---------------------------------|---------|-----------------|  
|`https://+:80/reportserver`|`https://<computername>/reportserver`<br /><br /> `https://<IPAddress>/reportserver`<br /><br /> `https://localhost/reportserver`|La prenotazione URL specifica un carattere jolly (+) sulla porta 80. In questo modo nella coda del server di report viene inserita qualsiasi richiesta in ingresso che specifica un host per la risoluzione nel computer server di report sulla porta 80. Si noti che con tale prenotazione URL è possibile utilizzare il numero desiderato di URL per accedere al server di report.<br /><br /> Si tratta della prenotazione URL predefinita per un server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per la maggior parte dei sistemi operativi.|  
|`https://123.45.67.0:80/reportserver`|`https://123.45.67.0/reportserver`|Questa prenotazione URL specifica un indirizzo IP ed è molto più restrittiva della prenotazione URL con carattere jolly. Solo gli URL che includono l'indirizzo IP possono essere utilizzati per la connessione al server di report. Tenuto conto di questa prenotazione URL, una richiesta a un server di report a `https://<computername>/reportserver` o `https://localhost/reportserver` avrà esito negativo.|  
  
##  <a name="default-urls"></a><a name="DefaultURLs"></a> URL predefiniti  
 Se [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene installato usando la configurazione predefinita, il programma di installazione prenoterà gli URL per il servizio Web ReportServer e per [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]. È possibile accettare questi valori predefiniti anche quando si definiscono prenotazioni URL nello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Gli URL predefiniti includono il nome di un'istanza se si installa [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] o se [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene installato come istanza denominata.  
  
> [!IMPORTANT]  
> Il carattere dell'istanza è un carattere di sottolineatura ( **_** ).  
  
 Le prenotazioni URL includono un numero di porta. I sistemi operativi seguenti consentono la condivisione di una porta da parte di più applicazioni Web:  
  
-   [!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2  
  
-   [!INCLUDE[win8srv](../../includes/win8srv-md.md)]  
  
-   [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)]  
  
-   [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)]  
  
-   [!INCLUDE[win7](../../includes/win7-md.md)]  
  
-   [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)]  
  
|Tipo di istanza|Applicazione|URL predefinito|Prenotazione di URL effettiva in HTTP.SYS|  
|-------------------|-----------------|-----------------|----------------------------------------|  
|Istanza predefinita|servizio Web ReportServer|`https://<servername>/reportserver`|`https://<servername>:80/reportserver`|  
|Istanza predefinita|Portale Web|`https://<servername>/reports`|`https://<servername>:80/reports`|  
|Istanza denominata|servizio Web ReportServer|`https://<servername>/reportserver_<instancename>`|`https://<servername>:80/reportserver_<instancename>`|  
|Istanza denominata|Portale Web|`https://<servername>/reports_<instancename>`|`https://<servername>:80/reports_<instancename>`|  
|SQL Server Express|servizio Web ReportServer|`https://<servername>/reportserver_SQLExpress`|`https://<servername>:80/reportserver_SQLExpress`|  
|SQL Server Express|Portale Web|`https://<servername>/reports_SQLExpress`|`https://<servername>:80/reports_SQLExpress`|  
  
##  <a name="authentication-and-service-identity-for-reporting-services-urls"></a><a name="URLPermissionsAccounts"></a> Autenticazione e identità del servizio per gli URL di Reporting Services  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Le prenotazioni di URL visualizzano l'account della prenotazione dell'URL. L'account del servizio virtuale viene usato per tutti gli URL creati per le applicazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in esecuzione nella stessa istanza.
  
 
 L'accesso anonimo è disabilitato perché la sicurezza predefinita è **RSWindowsNegotiate**. Per l'accesso Intranet, gli URL del server di report utilizzano nomi di computer di rete. Se si desidera configurare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per le connessioni Internet, è necessario utilizzare impostazioni diverse. Per altre informazioni sull'autenticazione, vedere [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md).  
  
##  <a name="urls-for-local-administration"></a><a name="URLlocalAdmin"></a> URL per l'amministrazione locale  
 È possibile utilizzare `https://localhost/reportserver` o `https://localhost/reports` se è stato specificato un carattere jolly sicuro o vulnerabile per la prenotazione URL.  
  
 L'URL `https://localhost` viene interpretato come `https://127.0.0.1`. Se la prenotazione URL è stata associata a un nome di computer o a un singolo indirizzo IP, non è possibile utilizzare localhost se non si crea una prenotazione aggiuntiva per 127.0.0.1 nel computer locale. Analogamente, se localhost o 127.0.0.1 è disabilitato nel computer, non è possibile utilizzare l'URL.  
  
 [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)], [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] e versioni successive includono nuove funzionalità di sicurezza per ridurre al minimo il rischio di eseguire inavvertitamente programmi con privilegi elevati. Per attivare l'amministrazione locale su tali sistemi operativi, è necessario eseguire operazioni aggiuntive. Per altre informazioni, vedere [Configurare un server di report in modalità nativa per gli amministratori locali &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare un URL &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
 [Sintassi delle prenotazioni URL &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md)  

