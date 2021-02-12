---
description: Configurare un URL (Gestione configurazione del server di report)
title: Configurare un URL (Gestione configurazione) | Microsoft Docs
ms.date: 05/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services], syntax
ms.assetid: 851e163a-ad2a-491e-bc1e-4df92327092f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bd4d8deaa9be114ffb2ce806f67be3b96babb8b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015281"
---
# <a name="configure-a-url--report-server-configuration-manager"></a>Configurare un URL (Gestione configurazione del server di report)
  Per usare [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] o il servizio Web ReportServer, è necessario configurare almeno un URL per ogni applicazione. La configurazione degli URL è obbligatoria se [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è stato installato in modalità "solo file", ovvero se è stata selezionata l'opzione **Installa senza configurare il server** nella pagina Opzioni di installazione Server report dell'Installazione guidata. Se [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è stato installato con la configurazione predefinita, gli URL sono già configurati per ogni applicazione.  
  
 Per configurare gli URL, utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , che consente di definire tutte le parti dell'URL. A differenza delle versioni precedenti, i siti Web di Internet Information Services (IIS) non forniscono più accesso alle applicazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fornisce valori predefiniti adatti per la maggior parte degli scenari di distribuzione, incluse le distribuzioni affiancate con altri servizi e applicazioni Web. Gli URL predefiniti includono i nomi di istanza, per ridurre il rischio di conflitti tra URL se si eseguono più istanze del server di report nello stesso computer.  
  
 In questo argomento sono incluse istruzioni per le attività seguenti:  
  
-   Creare un URL per il servizio Web ReportServer.  
  
-   Creare un URL per [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)].  
  
-   Impostare proprietà avanzate per gli URL per definire altri URL.  
  
 Per altre informazioni sul modo in cui gli URL vengono archiviati e gestiti o sui problemi di interoperabilità, vedere [Informazioni su prenotazioni e registrazione URL &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md) e [Installare Reporting Services e Internet Information Services side-by-side &#40;SSRS in modalità nativa&#41;](../../reporting-services/install-windows/install-reporting-and-internet-information-services-side-by-side.md). Per esaminare alcuni esempi di URL utilizzati di frequente in un'installazione di Reporting Services, vedere [Esempi di URL](#URLExamples) in questo argomento.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Prima di creare o modificare un URL, tenere presenti gli aspetti seguenti:  
  
-   È necessario essere un membro del gruppo Administrators locale nel computer server di report.  
  
-   Se IIS è installato nello stesso computer, controllare i nomi delle directory virtuali in tutti i siti Web che usano la porta 80. Se una o più directory virtuali utilizzano i nomi delle directory virtuali predefiniti di Reporting Services, ovvero "Report" e "ReportServer", scegliere nomi delle directory virtuali diversi per gli URL di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da configurare.  
  
-   Per configurare gli URL, è necessario utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Non utilizzare un'utilità di sistema. Non modificare mai prenotazioni URL direttamente nella sezione **URLReservations** del file RSReportServer.config. L'utilizzo dello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è necessario per aggiornare la prenotazione URL sottostante archiviata internamente e per sincronizzare le impostazioni URL archiviate nel file RSReportServer.config.  
  
-   Scegliere un'ora con un'attività di report ridotta. Ogni volta che la prenotazione URL cambia, potrebbe essere eseguito il riciclo dei domini applicazione per il servizio Web ReportServer e per [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] .  
  
-   Per una panoramica della creazione di URL e l'utilizzo in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Configurare gli URL del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md).  
  
### <a name="to-configure-a-url-for-the-report-server-web-service"></a>Per configurare un URL per il servizio Web ReportServer  
  
1.  Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi a un'istanza del server di report locale.  
  
2.  Fare clic su **URL servizio Web**.  
  
3.  Specificare la directory virtuale. Il nome della directory virtuale identifica l'applicazione che riceve la richiesta. Poiché un indirizzo e una porta IP possono essere condivisi da più applicazioni, il nome della directory virtuale specifica l'applicazione che riceve la richiesta.  
  
     Tale valore deve essere univoco per garantire che la richiesta raggiunga la destinazione prevista. Questo valore è obbligatorio. e supporta la distinzione tra maiuscole e minuscole. Tra un nome di directory virtuale e un'istanza di un'applicazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] esiste una corrispondenza uno-a-uno. Se si creano più URL per la stessa istanza dell'applicazione, è necessario utilizzare lo stesso nome di directory virtuale in tutti gli URL definiti per l'istanza dell'applicazione.  
  
     Per il servizio Web ReportServer il nome di directory virtuale predefinito è **ReportServer**.  
  
4.  Specificare l'indirizzo IP che identifica in modo univoco il computer server di report in rete. Se si desidera specificare un'intestazione host o definire URL aggiuntivi per la stessa istanza dell'applicazione, è necessario fare clic su **Avanzate**. Per indicazioni su come impostare le proprietà avanzate nell'URL, vedere le istruzioni fornite più avanti in questo argomento. Altrimenti, utilizzare la pagina **URL servizio Web** per impostare uno dei valori seguenti:  
  
    -   **Tutti assegnati** : specifica che qualunque indirizzo IP assegnato al computer può essere usato in un URL che punta a un'applicazione del server di report. Questo valore include anche i nomi host descrittivi, ad esempio i nomi computer, che possono essere risolti da un DNS in un indirizzo IP assegnato al computer. Si tratta del valore predefinito per un URL di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
    -   **Tutti non assegnati** : specifica che il server di report riceverà tutte le richieste che non sono state gestite da un'altra applicazione. È consigliabile evitare di specificare questa opzione. In caso contrario, è possibile che un'altra applicazione che dispone di una prenotazione URL più specifica intercetti le richieste destinate al server di report.  
  
    -   **127.0.0.1** : indirizzo IPv4 utilizzato per accedere a localhost. Tale indirizzo supporta l'amministrazione locale nel computer server di report. Se si seleziona solo questo valore, potranno accedere all'applicazione solo gli utenti connessi localmente al computer server di report.  
  
    -   **::1** : indirizzo di loopback in formato IPv6.  
  
    -   Nell'elenco vengono visualizzati anche indirizzi IP specifici. Gli indirizzi IP possono essere in formato IPv4 o IPv6. *Nnn.nnn.nnn.nnn* : indirizzo IPv4 a 32 bit di una scheda di rete nel computer. Gli indirizzi IPv6 sono a 128 bit, con otto campi a 4 byte separati dai due punti: \<prefix>:*nnnn:nnnn:nnnn:nnnn:nnnn:nnnn*  
  
         Se sono presenti più schede o la rete supporta indirizzi IPv4 e IPv6, verranno visualizzati più indirizzi IP. Se si seleziona solo un indirizzo IP, l'accesso all'applicazione sarà limitato all'indirizzo IP specificato e a qualsiasi nome host di cui il DNS esegue il mapping all'indirizzo. Non è possibile utilizzare localhost per accedere a un server di report, né utilizzare gli indirizzi IP di altre schede di rete installate nel computer server di report. Si seleziona in genere questo valore per configurare più prenotazioni di URL che specificano anche indirizzi IP o nomi host espliciti, ad esempio uno per una scheda di rete utilizzata per le connessioni Intranet e un altro per le connessioni Extranet.  
  
5.  Specificare la porta. La porta 80 è la porta predefinita perché può essere condivisa con altre applicazioni. Se si desidera utilizzare un numero di porta personalizzato, sarà necessario specificarlo sempre nell'URL utilizzato per accedere al server di report. Per trovare una porta disponibile, è possibile utilizzare i metodi seguenti:  
  
    -   Al prompt dei comandi digitare il comando seguente per ottenere l'elenco delle porte TCP in uso:  
  
         `netstat -anp tcp`  
  
    -   Per informazioni sulle assegnazioni di porta TCP e le differenze tra porte note (da 0 a 1023), porte registrate (da 1024 a 49151) e porte dinamiche o private (da 49152 a 65535), vedere l'articolo [Informazioni sull'assegnazione di porte TCP/IP](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/174904)nel sito Web del supporto tecnico Microsoft.  
  
    -   Se si utilizza Windows Firewall, è necessario aprire la porta. Per istruzioni, vedere [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
6.  Se necessario, verificare che in IIS (se installato) non sia presente una directory virtuale con lo stesso nome che si intende utilizzare.  
  
7.  Se è stato installato un certificato TLS/SSL, è possibile selezionarlo a questo punto per associare l'URL al certificato TLS/SSL installato nel computer.  
  
8.  Facoltativamente, se si seleziona un certificato TLS/SSL, è possibile specificare una porta personalizzata. L'impostazione predefinita è 443, ma è possibile utilizzare qualsiasi porta disponibile.  
  
9. Fare clic su **Applica** per creare l'URL.  
  
10. Eseguire il test dell'URL facendo clic sul collegamento nella sezione **URL** della pagina. Per poter eseguire il test dell'URL, è necessario creare e configurare il database del server di report. Per istruzioni, vedere [Creare un database del server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

> [!NOTE]
>  Se si hanno associazioni TLS esistenti e prenotazioni di URL e si vuole modificare l'associazione TLS, ad esempio per usare un'intestazione host o un certificato diversi, è consigliabile completare in ordine i passaggi seguenti:  
> 
>  1.  Innanzitutto rimuovere tutte le prenotazioni di URL.  
> 2.  Successivamente rimuovere tutte le associazioni TLS.  
> 3.  Infine ricreare gli URL e le associazioni TLS.  
> 
>  I passaggi precedenti possono essere completati utilizzando Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
> 
>  Microsoft Windows supporta un'associazione per ogni combinazione di indirizzo IP e porta. Se un server di report viene configurato in modo che venga utilizzato un valore di intestazione host specifico e il certificato relativo alla combinazione tra porta e indirizzo IP viene emesso con un valore di intestazione host diverso, nel browser verrà visualizzato un avviso in cui viene indicato che il certificato non corrisponde all'URL in uso.  
> 
>  Per risolvere questo problema, eliminare tutte le associazioni, quindi crearne di nuove con impostazioni univoche o configurare le registrazioni di URL di Reporting Services con caratteri jolly.
  
### <a name="to-create-a-url-reservation-for-the-ssrswebportal"></a>Per creare una prenotazione URL per [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]  
  
1.  Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi all'istanza del server di report.  
  
2.  Fare clic su **URL del portale Web**.  
  
3.  Specificare la directory virtuale. [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] è in attesa sulla stessa porta e sullo stesso indirizzo IP del servizio Web ReportServer. Se [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] è stato configurato per puntare a un servizio Web ReportServer diverso, è necessario modificare le impostazioni URL di [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] nel file RSReportServer.config.  
  
4.  Se è stato installato un certificato TLS/SSL, è possibile selezionarlo in modo che tutte le richieste a [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] vengano indirizzate tramite HTTPS.  
  
     Facoltativamente, se si seleziona un certificato TLS/SSL, è possibile specificare una porta personalizzata. L'impostazione predefinita è 443, ma è possibile utilizzare qualsiasi porta disponibile.  
  
5.  Fare clic su **Applica** per creare l'URL.  
  
6.  Eseguire il test dell'URL facendo clic sul collegamento nella sezione **URL** della pagina.  
  
## <a name="setting-advanced-properties-to-specify-additional-urls"></a>Impostazione di proprietà avanzate per specificare altri URL  
 È possibile riservare più URL per il servizio Web ReportServer o [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] specificando porte o nomi host diversi, ovvero un indirizzo IP o un nome di intestazione host che può essere risolto da un DNS in un indirizzo IP assegnato al computer. Creando più URL, è possibile configurare percorsi di accesso diversi per la stessa istanza del server di report. Per attivare l'accesso Intranet ed Extranet a un server di report, ad esempio, è possibile utilizzare l'URL predefinito per l'accesso tramite Intranet e un altro nome host completo per l'accesso Extranet:  
  
-   `https://myserver01/reportserver`  
  
-   `https://www.adventure-works.com/reportserver`  
  
 Non è possibile impostare più nomi delle directory virtuali per la stessa istanza dell'applicazione. Di ogni istanza dell'applicazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene eseguito il mapping a un singolo nome di directory virtuale. Se si dispone di più istanze di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nello stesso computer, il nome della directory virtuale per un'applicazione deve includere il nome di istanza per garantire che ogni richiesta raggiunga la destinazione prevista.  
 
  **Intestazione host**  
 Se è già stata definita un'intestazione host in un DNS risolto nel computer, è possibile specificare tale intestazione host in un URL configurato per l'accesso al server di report.  
  
 Un'intestazione host è un nome univoco che consente a più siti Web di condividere un singolo indirizzo IP e una singola porta. I nomi di intestazione host sono più semplici da ricordare e da digitare rispetto all'indirizzo IP e ai numeri di porta. Un esempio di un nome di intestazione host potrebbe essere www.adventure-works.com.  
  
 **Porta SSL**  
 Consente di specificare la porta per le connessioni TLS/SSL. Il numero di porta predefinito per TLS è 443.  
  
 **Certificato SSL**  
 Consente di specificare il nome di un certificato TLS/SSL installato nel computer. Se il certificato esegue il mapping a un carattere jolly, è possibile utilizzarlo per una connessione del server di report.  
  
 Consente di specificare il nome completo del computer per cui viene registrato il certificato. Il nome specificato deve essere identico al nome per cui viene registrato il certificato.  
  
 Per utilizzare questa opzione, è necessario disporre di un certificato installato. È inoltre necessario modificare l'impostazione di configurazione UrlRoot nel file RSReportServer.config in modo che specifichi il nome completo del computer per il quale viene registrato il certificato. Per altre informazioni, vedere [Configurare connessioni TLS in un server di report in modalità nativa](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
### <a name="to-set-advanced-properties-on-a-url"></a>Per impostare proprietà avanzate in un URL  
  
1.  Nella pagina **URL servizio Web** o **URL del portale Web** fare clic su **Avanzate**.  
  
2.  Scegliere **Aggiungi**.  
  
3.  Fare clic su Indirizzo IP o Nome intestazione host. Nel caso di un'intestazione host, assicurarsi di specificare un nome che il servizio DNS sia in grado di risolvere. Se si specifica un nome di dominio pubblico, specificare l'intero URL, incluso `https://www`.  
  
4.  Specificare la porta. Se si specifica una porta personalizzata, l'URL dell'applicazione deve sempre includere il numero di porta.  
  
5.  Fare clic su **OK**.  
  
6.  Eseguire il test dell'URL aprendo una finestra del browser e immettendo l'URL.  
  
## <a name="urls-for-multiple-report-server-instances-on-the-same-computer"></a>URL per più istanze del server di report nello stesso computer  
 Se si riservano URL per più istanze di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], è necessario seguire le convenzioni di denominazione per evitare conflitti di denominazione. Per altre informazioni, vedere [Prenotazioni URL per le distribuzioni di più istanze del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/url-reservations-for-multi-instance-report-server-deployments.md).  
  
##  <a name="examples-of-url-configurations"></a><a name="URLExamples"></a> Esempi di configurazioni di URL  
 Nell'elenco seguente sono inclusi alcuni esempi di URL di server di report:  
  
-   `https://localhost/reportserver`  
  
-   `https://localhost/reportserver_SQLEXPRESS`  
  
-   `https://sales01/reportserver`  
  
-   `https://sales01:8080/reportserver`  
  
-   `https://sales.adventure-works.com/reportserver`  
  
-   `https://www.adventure-works.com:8080/reportserver01`  
  
 Gli URL usati per accedere a [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] condividono un formato simile e vengono in genere creati nello stesso sito Web che ospita il server di report. L'unica differenza è costituita dal nome di directory virtuale. In questo caso viene usato il nome **reports** , ma è possibile configurare qualsiasi nome:  
  
-   `https://localhost/reports`  
  
-   `https://localhost/reports_SQLEXPRESS`  
  
-   `https://sales01/reports`  
  
-   `https://sales01:8080/reports`  
  
-   `https://sales.adventure-works.com/reports`  
  
-   `https://www.adventure-works.com:8080/reports`  
  
## <a name="see-also"></a>Vedere anche

 [Configurare gli URL del server di report &#40Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
