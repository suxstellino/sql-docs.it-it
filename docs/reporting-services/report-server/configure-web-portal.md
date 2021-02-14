---
title: Configurare il portale Web | Microsoft Docs
description: Informazioni su come configurare l'applicazione del portale Web usata per visualizzare report, gestire il contenuto del server di report e concedere l'accesso utente a un server di report in modalità nativa.
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- the web portal [Reporting Services], configuring
ms.assetid: e918986c-af15-48f6-8178-256aed829c6a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ebbce2708647e15b68ec497cfadd0af2c7c5b154
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074692"
---
# <a name="configure-the-web-portal"></a>Configurare il portale Web

Il Portale Web è un'applicazione front-end Web usata per visualizzare report, gestire il contenuto del server di report e concedere l'accesso utente a un server di report in modalità nativa. Il portale Web viene installato con il servizio Web ReportServer all'interno della stessa istanza del server di report e facoltativamente configurato se si seleziona l'opzione **Installa la configurazione predefinita della modalità nativa** del programma di installazione. Può essere configurato anche come attività di post-installazione. Questo argomento offre informazioni sui seguenti scenari di configurazione del portale Web:

## <a name="prerequisites"></a>Prerequisiti

Per usare il portale Web, è necessario che vengano soddisfatti i prerequisiti seguenti:

- È necessario disporre di una configurazione minima del server di report. Per altre informazioni sulla configurazione minima di un server di report, vedere [Configurare un server di report](../../reporting-services/report-server/configure-a-report-server-reporting-services-native-mode.md).

- Il server di report deve essere eseguito in modalità nativa. Non è possibile usare il portale Web con un server di report configurato per la modalità integrata SharePoint. In SQL Server 2013 non è possibile passare un server di report da una modalità all'altra. Se si desidera modificare il tipo di server di report utilizzato dall'ambiente, è necessario installare la modalità desiderata del server di report desiderato e copiare o spostare gli elementi del report dal server di report precedente in quello nuovo. Questo processo viene in genere chiamato 'migrazione'. I passaggi necessari per eseguire la migrazione dipendono dalla modalità con cui si esegue questa operazione e dalla versione dalla quale si esegue la migrazione. Per ulteriori informazioni, vedere [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).

- È necessario anche aver installato Internet Explorer 11 o versione successiva con gli script abilitati. Per altre informazioni, vedere [Supporto browser per Reporting Services e Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

## <a name="configure-the-web-portal-to-use-the-default-url"></a>Configurare il portale Web per usare l'URL predefinito

Il portale Web è un'applicazione Web a cui si accede tramite un Web browser. È necessario almeno definire l'URL utilizzato per aprire l'applicazione in una finestra del browser. L'URL è costituito da un nome host, una porta e una directory virtuale. I valori predefiniti per questo URL includono i valori del nome host e della porta definiti per l'URL del servizio Web ReportServer, oltre al nome della directory virtuale **reports** . Se si dispone di un'istanza denominata, la directory virtuale è **istanza_report**, dove **istanza** è il nome dell'istanza di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .

Per impostazione predefinita, l'URL del portale Web include un nome di directory virtuale univoco e la porta e il nome host definito per il servizio Web ReportServer eseguito nella stessa istanza. Nella maggior parte dei casi, il nome host corrisponde al nome di rete del computer del server di report, ma può anche essere un indirizzo IP o l'intestazione host che si risolve nel computer. Per configurare il portale Web per l'uso dell'URL predefinito, usare la pagina **URL del portale Web** dello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

> [!TIP]
> Se si tenta di accedere al portale Web in un computer remoto e si ricevono messaggi di errore di connessione nel browser, una causa comune è da individuare nelle impostazioni del firewall. Per altre informazioni, vedere [Configurare un firewall per l'accesso al server di report](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).

#### <a name="to-configure-the-default-the-web-portal-url-and-virtual-directory"></a>Per configurare l'URL e la directory virtuale del portale Web predefiniti

1. Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi all'istanza del server di report.

2. Nello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] selezionare **URL del portale Web** per aprire la pagina per la configurazione dell'URL.

3. Immettere un nome di directory virtuale univoco per il portale Web.

4. Fare clic su **Applica**.

5. Se si usa [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)] o Windows Server 2008, potrebbe essere necessario completare altri passaggi per usare il portale Web. Per altre informazioni, vedere [Configurare un server di report in modalità nativa per gli amministratori locali &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="configure-the-web-portal-to-use-a-specific-report-server-url"></a>Configurare il portale Web per l'uso di un URL del server di report specifico

Per impostazione predefinita, il portale Web si connette al servizio Web ReportServer eseguito nello stesso servizio ReportServer e usa l'URL di questo servizio per eseguire la connessione. Se si definiscono più URL per il servizio Web ReportServer, viene usato quello definito per ultimo. Per alcune distribuzioni, può tuttavia essere necessario connettere sempre il portale Web al servizio Web tramite un URL statico, ad esempio nei casi in cui il filtro dei pacchetti è stato configurato su una porta o un indirizzo IP specifico e si desidera che tutte le connessioni al server di report vengano sottoposte alle regole di filtro definite.

Quando si configurano gli URL nello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], il portale Web rileva e usa automaticamente gli URL nuovi e aggiornati per il server di report eseguito nella stessa istanza del server. Se la distribuzione richiede l'utilizzo di un unico URL statico per tutte le richieste del server di report, è possibile specificare tale URL nel file RSReportServer.config.

#### <a name="to-configure-a-static-report-server-url"></a>Per configurare un URL di un server di report statico

1. Aprire il file **RSReportServer.config** in un editor di testo. Per impostazione predefinita, il percorso di questo file è \Program Files\Microsoft SQL Server\MSRS12.\<*instancename*>\Reporting Services\ReportServer.  

2. Individuare **ReportServerURL**.

3. Sostituirlo con l'URL dell'istanza del server di report.

4. Salvare le modifiche e chiudere il file.

Per altre informazioni sul file di configurazione, vedere [Modificare un file di configurazione di Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md) e [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

## <a name="customize-styles-or-application-title"></a>Personalizzare gli stili o il titolo dell'applicazione

È possibile creare un pacchetto personalizzato per modificare i colori usati per il portale Web. Per altre informazioni, vedere [Personalizzazione del portale Web](../branding-the-web-portal.md)

#### <a name="to-modify-application-title"></a>Per modificare il titolo dell'applicazione

1. Accedere usando un account con autorizzazioni di **Amministratore sistema** sul server di report.

2. Aprire Internet Explorer.

3. Immettere l'URL del portale Web. Per impostazione predefinita, l'URL è https://\<**your-server-name**>/reports, ma se Reporting Services è stato installato come istanza denominata, l'URL predefinito sarà https://\<**your-server-name**>/reports\<**_instancename**>.

4. Selezionare **Impostazioni sito**.

5. Nella scheda **Generale** sostituire **SQL Server Reporting Services** con un nome diverso in **Nome** .

6. Selezionare **Applica**.

## <a name="next-steps"></a>Passaggi successivi

[Portale Web](../../reporting-services/web-portal-ssrs-native-mode.md)  
[Supporto browser per Reporting Services](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
[Configurare un URL](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
[Verificare un'installazione di Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)   
[Abilitare o disabilitare le funzionalità di Reporting Services](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md)   
[Gestione di un server di report in modalità nativa](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configurare un server di report in modalità nativa per gli amministratori locali](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)

 Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
