---
description: Configurare connessioni TLS in un server di report in modalità nativa
title: Configurare connessioni TLS in un server di report in modalità nativa | Microsoft Docs
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Secure Sockets Layer (SSL)
ms.assetid: 212f2042-456a-4c0a-8d76-480b18f02431
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6d76229a17951c4b0bad35ee78e95a903bb590e3
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594957"
---
# <a name="configure-tls-connections-on-a-native-mode-report-server"></a>Configurare connessioni TLS in un server di report in modalità nativa

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] La modalità nativa usa il servizio SSL HTTP (Secure Sockets Layer) per stabilire connessioni crittografate a un server di report. Il protocollo TLS (Transport Layer Security) era noto in precedenza come SSL (Secure Sockets Layer). Se si dispone di un file di certificato (con estensione cer) installato in un archivio certificati locale nel server di report, è possibile associare il certificato a una prenotazione di URL di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per supportare le connessioni al server di report tramite un canale crittografato.  
  
> [!TIP]  
>  Se si utilizza la modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vedere la documentazione su SharePoint per ulteriori informazioni. Ad esempio, [Come abilitare TLS in un'applicazione Web SharePoint 2010](/archive/blogs/sowmyancs/how-to-enable-ssl-on-a-sharepoint-2010-web-application).  
  
 Poiché anche Internet Information Services (IIS) usa il servizio SSL HTTP, si verificano problemi di interoperabilità significativi di cui è necessario tenere conto se si sceglie di eseguire IIS e [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nello stesso computer. Esaminare la sezione Problemi di interoperabilità con IIS per indicazioni su come risolvere tali problemi.  
  
## <a name="server-certificate-requirements"></a>Requisiti dei certificati server  
 È necessario che nel computer sia installato un certificato server. I certificati client non sono supportati. In Reporting Services non sono disponibili funzionalità per la richiesta, la generazione, il download o l'installazione di un certificato. Windows Server 2012 e versioni successive includono uno snap-in Certificati che è possibile usare per richiedere un certificato da un'autorità di certificazione attendibile.  
  
 A scopo di test, è possibile generare un certificato in locale. Se si usano l'utilità **MakeCert** e il comando di esempio come modello, assicurarsi di specificare il nome del server come host e di rimuovere tutte le interruzioni di riga prima di eseguire il comando. Se si esegue il comando in una finestra DOS, può essere necessario aumentare le dimensioni del buffer della finestra per consentire l'inserimento dell'intero comando.  
  
 Se si eseguono IIS e [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nello stesso computer, è possibile usare l'applicazione console IIS Manager per ottenere il certificato installato nel computer. IIS Manager include opzioni per la creazione e l'assemblaggio di un file di richiesta di certificato (con estensione crt) per l'elaborazione successiva da parte di un'autorità di certificazione attendibile. L'autorità di certificazione scelta genererà e restituirà un file di certificato (con estensione cer). È possibile utilizzare la console di gestione IIS per installare il file di certificato nell'archivio locale. Per altre informazioni, vedere [Using SSL to Encrypt Confidential Data](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) (Uso di SSL per crittografare dati riservati) nel sito Microsoft Technet.  
  
## <a name="interoperability-issues-with-iis"></a>Problemi di interoperabilità con IIS  
 La presenza di IIS nello stesso computer di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] influirà in modo significativo sulle connessioni TLS a un server di report:  
  
-   Se IIS è installato, è necessario che il servizio World Wide Web (W3SVC) sia sempre in esecuzione. Il servizio SSL HTTP creerà una dipendenza da IIS se rileva che IIS è in esecuzione. Pertanto, il servizio World Wide Web (W3SVC) deve essere in esecuzione ogni volta che IIS e [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vengono installati nello stesso computer e vengono configurati gli URL del server di report per le connessioni TLS.  
  
-   La disinstallazione di IIS può interrompere temporaneamente il servizio per un URL del server di report associato a TLS. Per questo motivo, si consiglia di riavviare il computer dopo la disinstallazione di IIS.  
  
     Il riavvio del computer è necessario per cancellare tutte le sessioni TLS dalla cache. In alcuni sistemi operativi le sessioni TLS vengono memorizzate nella cache per un massimo di 10 ore. In questo caso, gli URL https:// continuano a funzionare anche dopo la rimozione dell'associazione TLS dalla prenotazione URL in HTTP.SYS. Con il riavvio del computer vengono chiuse tutte le connessioni aperte che utilizzano il canale.  
  
## <a name="bind-tls-to-a-reporting-services-url-reservation"></a>Associare TLS a una prenotazione URL di Reporting Services  
 Nei passaggi seguenti non sono incluse le istruzioni per la richiesta, la generazione, il download o l'installazione di un certificato. È necessario che un certificato sia installato e disponibile per l'uso. Le proprietà del certificato specificate, l'autorità di certificazione da cui lo si ottiene e gli strumenti e le utilità che si utilizzano per richiedere e installare il certificato sono di responsabilità dell'utente.  
  
 Per associare il certificato, è possibile utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Se il certificato è installato correttamente nell'archivio del computer locale, lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] lo rileva e lo visualizza nell'elenco **Certificati SSL** delle pagine **URL servizio Web** e **URL portale Web**.  
  
### <a name="to-configure-a-report-server-url-for-tls"></a>Per configurare un URL del server di report per TLS  
  
1.  Avviare lo strumento di configurazione di Reporting Services e connettersi al server di report.  
  
2.  Selezionare **URL servizio Web**.  
  
3.  Espandere l'elenco di certificati TLS/SSL. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] consente di rilevare i certificati di autenticazione server nell'archivio locale. Se è stato installato un certificato che non viene visualizzato nell'elenco, può essere necessario riavviare il servizio. A tale scopo, è possibile usare i pulsanti **Arresta** e **Avvia** nella pagina **Stato server di report** dello strumento di configurazione di Reporting Services (pagina principale).  
  
4.  Selezionare il certificato.  
  
5.  Fare clic su **Applica**.  
  
6.  Fare clic sull'URL per verificarne il funzionamento.  
  
 La configurazione del database del server di report è un requisito per testare l'URL. Se il database del server di report non è ancora stato creato, completare questo passaggio prima di testare l'URL.  
  
 Le prenotazioni URL per l'URL del portale Web e l'URL del servizio Web ReportServer vengono configurate in modo indipendente. Se si vuole configurare anche l'accesso al portale Web tramite un canale crittografato con TLS, continuare con i passaggi seguenti:  
  
1.  Immettere l'**URL del portale Web**.
  
2.  Fare clic su **Avanzate**.  
  
3.  In **Più identità HTTPS per la funzionalità Reporting Services attualmente selezionata** selezionare **Aggiungi**.  
  
4.  Selezionare il certificato, selezionare **OK** e quindi selezionare **Applica**.  
  
5.  Testare l'URL per verificarne il funzionamento.  
  
## <a name="how-certificate-bindings-are-stored"></a>Modalità di archiviazione delle associazioni certificato  
 Le associazioni certificato verranno archiviate in HTTP.SYS. Una rappresentazione delle associazioni definite verrà anche archiviata nella sezione **URLReservations** del file RSReportServer.config. Le impostazioni nel file di configurazione sono solo una rappresentazione dei valori effettivi specificati altrove. Non modificare i valori direttamente nel file di configurazione. Le impostazioni di configurazione verranno visualizzate nel file solo dopo che è stato utilizzato lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o il provider WMI (Windows Management Instrumentation, Strumentazione gestione Windows) del server di report per associare un certificato.  
  
> [!NOTE]  
>  Se si configura un'associazione con un certificato TLS/SSL in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e successivamente si vuole rimuovere il certificato dal computer, assicurarsi di rimuovere l'associazione da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prima di rimuovere il certificato dal computer. In caso contrario, l'associazione non potrà essere rimossa tramite lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o WMI e verrà visualizzato l'errore "Parametro non valido". Se è già stato rimosso il certificato dal computer, è possibile utilizzare lo strumento Httpcfg.exe per rimuovere l'associazione da HTTP.SYS. Per ulteriori informazioni su Httpcfg.exe, vedere la documentazione di Windows.  
  
 Le associazioni TLS sono una risorsa condivisa in Microsoft Windows. Le modifiche apportate da Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o da altri strumenti come Gestione IIS possono influire su altre applicazioni presenti nello stesso computer. Per modificare le associazioni è quindi consigliabile utilizzare lo stesso strumento adoperato per la creazione.  Ad esempio, se le associazioni TLS sono state create mediante Gestione configurazione, è consigliabile usare questo stesso strumento per gestirne il ciclo di vita. Lo stesso concetto vale per la creazione e la gestione delle associazioni mediante Gestione IIS. Se IIS viene installato nel computer prima di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], è consigliabile esaminare la configurazione TLS in IIS prima di configurare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Se si rimuovono le associazioni TLS per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] mediante Gestione configurazione del server di report, TLS potrebbe non funzionare più per i siti Web di un server in cui è in esecuzione Internet Information Services (IIS) o un altro server HTTP.SYS. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Gestione configurazione rimuove la chiave del Registro di sistema riportata di seguito: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443** Quando questa chiave del Registro di sistema viene rimossa, viene rimossa anche l'associazione TLS per IIS. Senza questa associazione, TLS non viene supportato per il protocollo HTTPS. Per diagnosticare questo problema, usare Gestione IIS o l'utilità della riga di comando HTTPCFG.exe. Per risolvere il problema, ripristinare l'associazione TLS per i siti Web tramite Gestione IIS. Per evitare questo problema in futuro, usare Gestione IIS per rimuovere le associazioni TLS e quindi usare Gestione IIS per ripristinare l'associazione per i siti Web desiderati. Per altre informazioni, vedere l'articolo della Knowledge Base [Mancato funzionamento di SSL dopo la rimozione di un'associazione SSL (https://support.microsoft.com/kb/956209/n)](https://support.microsoft.com/kb/956209/n).  
  
## <a name="see-also"></a>Vedere anche  
 [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Configurare e amministrare un server di report &#40;modalità nativa SSRS&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Configurare gli URL del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
  
