---
title: Configurare Reporting Services per usare un nome alternativo del soggetto | Microsoft Docs
description: Informazioni su come configurare SQL Server Reporting Services e Server di report di Power BI per usare un nome alternativo del soggetto modificando il file rsreportserver.config e usando lo strumento Netsh.exe.
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 25e5fce47d86ea4d544b43f297fcb793d8167a66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065166"
---
# <a name="configure-reporting-services-to-use-a-subject-alternative-name-san"></a>Configurare Reporting Services per usare un nome alternativo del soggetto

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

Questo argomento descrive come configurare Reporting Services (SSRS) e Server di report di Power BI per usare un nome alternativo del soggetto modificando il file rsreportserver.config e usando lo strumento Netsh.exe.

Le istruzioni sono valide per l'URL del servizio Web e per l'URL del portale Web nello strumento Gestione configurazione server di report.

Per usare un nome alternativo del soggetto, è necessario che il certificato TLS/SSL sia registrato nel server, sia firmato e contenga la chiave privata. Non è possibile usare un certificato autofirmato.

È possibile configurare gli URL in Reporting Services e in Server di report di Power BI per l'uso di un certificato TLS/SSL. In genere, un certificato contiene solo un nome del soggetto che consente un solo URL per una sessione di TLS (Transport Layer Security), noto in precedenza come SSL (Secure Sockets Layer). Il nome alternativo del soggetto è un campo aggiuntivo nel certificato che consente a un servizio TLS di essere in ascolto per molti URL nonché di condividere la porta TLS con altre applicazioni. Ad esempio, un nome alternativo del soggetto potrebbe essere simile a `www.myreports.com`.

Per altre informazioni sulle impostazioni di TLS per Reporting Services, vedere [Configurare connessioni TLS in un server di report in modalità nativa](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
## <a name="configure-to-use-a-subject-alternative-name-for-web-service-url"></a>Configurazione per l'uso di un nome alternativo del soggetto per l'URL del servizio Web
  
1.  Avviare Gestione configurazione server di report.  
  
     Per altre informazioni, vedere [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Nella pagina **URL servizio Web** selezionare una porta TLS/SSL e un certificato TLS/SSL.  
  
     ![Gestione configurazione server di report](../../reporting-services/report-server-sharepoint/media/reportingservices-configurationmanager.png "Gestione configurazione server di report")  
  
     Gestione configurazione registra il certificato TLS/SSL per la porta.  
  
3.  Aprire il file rsreportserver.config.  
  
     Per impostazione predefinita, nella modalità nativa di SSRS 2016 il file si trova nella cartella seguente:  
  
    ```  
    \Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
    ```  
  
     Per impostazione predefinita, nella modalità nativa di SSRS 2017 e versioni successive il file si trova nella cartella seguente:  
  
    ```  
    \Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer  
    ```  
    
     Per Server di report di Power BI, il file si trova per impostazione predefinita nella cartella seguente:  
  
    ```  
    \Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer  
    ```  
  
4.  Copiare la sezione URL per l'applicazione **ReportServerWebService**.
  
     Ad esempio, la sezione URL originale è:  
  
    ```  
        <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
     La sezione URL modificata è:
  
    ```  
    <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
        <URL>  
         <UrlString>https://www.myreports.com:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051/AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
  > [!TIP]  
>  * Per SSRS 2017 e versioni successive, il valore di `AccountSid` è `S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301` e il valore di `AccountName` è `NT SERVICE\SQLServerReportingServices`.
>  * Per Server di report di Power BI, il valore di `AccountSid` è `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663` e il valore di `AccountName` è `NT SERVICE\PowerBIReportServer`.
  
5.  Salvare il file rsreportserver.config.  
  
6.  Avviare un prompt dei comandi con **Esegui come amministratore** ed eseguire lo strumento Netsh.exe.  
  
    ```  
    C:\windows\system32\netsh  
    ```  
  
7.  Passare al contesto http digitando il testo seguente.  
  
    ```  
    Netsh>http  
    ```  
  
8.  Mostrare gli urlacl esistenti digitando il testo seguente:
  
    ```  
    Netsh http>show urlacl  
    ```  
  
     Verrà visualizzata una voce simile alla seguente.  
  
    ```  
    Reserved URL            : https://+:443/ReportServer/  
        User: NT SERVICE\ReportServer  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
    ```  
  
     Un urlacl è un elenco di controllo di accesso discrezionale (DACL, Discretionary Access Control List) per un URL riservato.  
  
9. Creare una nuova voce per il nome alternativo del soggetto con lo stesso utente e SDDL della voce esistente digitando il testo seguente:  
  
    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/ReportServer    
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
  
10. Per l'**URL del portale Web** creare una nuova voce per il nome alternativo del soggetto digitando quanto segue:

    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/Reports  
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
> [!TIP]  
>  * Per SSRS 2017 e versioni successive, il valore di `user` è `NT SERVICE\SQLServerReportingServices` e il valore di `sddl` è `D:(A;;GX;;;S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301)`.
>  * Per Server di report di Power BI, il valore di `user` è `NT SERVICE\PowerBIReportServer` e il valore di `sddl` è `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663`.

> [!NOTE]  
> Per Server di report di Power BI, è necessario creare due voci aggiuntive per il nome alternativo del soggetto digitando quanto segue:
>  * `add urlacl url=https://www.myreports.com:443/PowerBI user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`
>  * `add urlacl url=https://www.myreports.com:443/wopi user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`

11. Nella pagina **Stato server di report** di Gestione configurazione server di report fare clic su **Arresta** e quindi su **Avvia** per riavviare il server di report.  
  
## <a name="see-also"></a>Vedere anche

 [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Gestione configurazione del server di report](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Modificare un file di configurazione di Reporting Services](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurare gli URL del server di report](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
