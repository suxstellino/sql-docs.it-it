---
title: Abilitare e disabilitare la stampa sul lato client per Reporting Services | Microsoft Docs
description: Informazioni su come abilitare o disabilitare la stampa sul lato client per i report di Reporting Services visualizzati in un browser. La stampa sul lato client usa il formato PDF ed è abilitata per impostazione predefinita.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- pdf
- viewer
- reportviewer
- toolbar
ms.assetid: 0e709c96-7517-4547-8ef6-5632f8118524
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8cf4544579f4a3258d4bc785c98ada5935653dff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075777"
---
# <a name="enable-and-disable-client-side-printing-for-reporting-services"></a>Abilitare e disabilitare la stampa sul lato client per Reporting Services

  Il pulsante di stampa nella barra degli strumenti del visualizzatore di report usa il formato PDF (Portable Document Format) per la stampa sul lato client dei report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] visualizzati in un browser. La nuova esperienza di stampa remota usa l'estensione per il rendering PDF inclusa in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]per il rendering dei report in formato PDF. È possibile scaricare un modulo PDF del report oppure, se è installata un'applicazione per la visualizzazione dei file PDF, il pulsante di stampa visualizza una finestra di dialogo con gli elementi di configurazione della pagina comuni, ad esempio le dimensioni e l'orientamento della pagina e l'anteprima del file PDF. Sebbene la funzionalità di stampa sul alto client sia abilitata per impostazione predefinita, è possibile disabilitarla per evitare che venga utilizzata.  
  
 Le versioni precedenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usano un controllo ActiveX per il quale è necessario il download nel computer client dal server di report. Se si aggiorna il server di report a SQL Server 2016 o versioni successive, il controllo di stampa non viene rimosso dal server di report né dai computer client.  

##  <a name="the-print-experience"></a><a name="bkmk_clientside_printexpereince"></a> L'esperienza di stampa  
 Quando si fa clic sul pulsante di stampa ![htmlviewer_print](../../reporting-services/report-server/media/htmlviewer-print.png "htmlviewer_print") nella barra degli strumenti del visualizzatore di report, l'esperienza varia in base alle applicazioni di visualizzazione dei file PDF installate nel computer client e al browser in uso.   È possibile scaricare il file PDF o configurare le opzioni di stampa da una finestra di dialogo, o eseguire entrambe le operazioni, a seconda del computer client.  
  
 ![Barra degli strumenti dei report](../../reporting-services/media/ssrs-htmlviewer-toolbar.png "Barra degli strumenti dei report")  
  
|Interazione|Interfaccia utente|  
|-|-|  
|La prima finestra di dialogo è la stessa per tutti i browser e consente di modificare le proprietà di base del layout, ad esempio l'orientamento. Quando si fa clic su **Stampa**, l'esperienza diventa leggermente diversa in base al browser in uso.|![ssrs_pdfprint_chrome1](../../reporting-services/report-server/media/ssrs-pdfprint-chrome1.png "ssrs_pdfprint_chrome1")|  
|In Chrome, viene aperta una finestra di dialogo di stampa dettagliata del browser.   È possibile modificare la configurazione di stampa, stampare e aprire la finestra di dialogo di stampa dei sistemi operativi.|![ssrs_pdfprint_chrome2](../../reporting-services/report-server/media/ssrs-pdfprint-chrome2.png "ssrs_pdfprint_chrome2") ![ssrs_pdfprint_chrome3.png](../../reporting-services/report-server/media/ssrs-pdfprint-chrome3-png.png "ssrs_pdfprint_chrome3.png")|  
|Se è installata un'applicazione per la lettura dei file PDF, il pulsante di stampa apre una finestra di anteprima del file PDF e consente di salvare o stampare.||  
|Se non è installata un'applicazione di lettura dei PDF, le esperienze utente possibili sono due:<br /><br /> Il rendering del report viene eseguito automaticamente e viene usato il processo di download del browser per scaricare il file PDF.   **Nota:** più complicato è il report, maggiore sarà il ritardo tra il momento in cui si fa clic su **Stampa** e il momento in cui viene visualizzata la notifica del download nel browser. È anche possibile forzare di nuovo il download facendo clic su **Fare clic qui per visualizzare il report in formato PDF**.<br /><br /> Per forzare il download, fare clic su **Fare clic qui per visualizzare il report in formato PDF**.|![ssrs_pdfprint_firefox2](../../reporting-services/report-server/media/ssrs-pdfprint-firefox2.png "ssrs_pdfprint_firefox2")|  
  
##  <a name="troubleshoot-client-side-printing"></a><a name="bkmk_troubleshoot_clientsideprinting"></a> Risoluzione dei problemi della stampa sul lato client  
 Se il pulsante di stampa della barra degli strumenti del visualizzatore di report è disabilitato, verificare quanto segue:  
  
-   La stampa sul lato client è disabilitata per il server di report in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Vedere la sezione  [Abilitare e disabilitare la stampa sul lato client](#bkmk_enable) in questo argomento.  
  
-   L'estensione per il rendering PDF di [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] è disabilitata. Esaminare la sezione `<Extension Name="PDF"` del file **rsreportserver.config** .  
  
-   Si sta visualizzando l'attività di generazione dei report in modalità di confronto, che utilizza il vecchio motore di rendering HTML 4 di [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . L'esperienza di stampa PDF richiede il motore di rendering HTML 5.  Fare clic sul pulsante di **anteprima** nella barra degli strumenti.  
  
     ![ssrs_html5_switch2html5](../../reporting-services/report-server/media/ssrs-html5-switch2html5.png "ssrs_html5_switch2html5")  
  
##  <a name="enable-and-disable-client-side-printing"></a><a name="bkmk_enable"></a> Abilitare e disabilitare la stampa sul lato client  
 Gli amministratori dei server di report possono disabilitare la funzionalità di stampa remota impostando la proprietà di sistema **EnableClientPrinting** del server di report su **false**. Questa impostazione disabilita la stampa sul lato client per tutti i report gestiti dal server. Per impostazione predefinita, la proprietà **EnableClientPrinting** è impostata su **true**. È possibile disabilitare la stampa sul lato client nei modi seguenti:  
  
-   Per un **server di report in modalità nativa**:  
  
    1.  Avviare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] con privilegi amministrativi.  
  
    2.  Connettersi a un'istanza del server di report in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
    3.  Fare clic con il pulsante destro del mouse sul nodo del server di report, quindi scegliere **Proprietà**. Se l'opzione **Proprietà** è disabilitata, verificare che [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] sia stato avviato con i privilegi amministrativi.  
  
    4.  Fare clic su **Avanzate**.  
  
    5.  Selezionare **EnableClientPrinting**.  
  
    6.  Impostare su True o False e fare clic su **OK**.  
  
         ![ssrs_ssmsproperties_clientprinting](../../reporting-services/report-server/media/ssrs-ssmsproperties-clientprinting.png "ssrs_ssmsproperties_clientprinting")  
  
-   Per un **server di report in modalità SharePoint**:  
  
    1.  In Amministrazione centrale SharePoint fare clic su **Gestione applicazioni**.  
  
    2.  Fare clic su **Gestisci applicazioni di servizio**.  
  
    3.  Fare clic sul nome dell'applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , quindi su **Gestisci** nella barra multifunzione di SharePoint.  
  
    4.  Fare clic su **Impostazioni sistema**.  
  
    5.  Selezionare **Abilita stampa client**. L'opzione **Abilita stampa client** si trova nella parte inferiore della pagina.  
  
    6.  Fare clic su **OK**.  
  
-   Scrivere script o codice per impostare la proprietà di sistema del server di report **EnableClientPrinting** su **false.**  
  
 Nello script di esempio riportato di seguito viene illustrato un approccio per la disabilitazione della stampa sul alto client. Compilare e quindi eseguire il codice di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] seguente per impostare la proprietà **EnableClientPrinting** su **False**. Al termine dell'esecuzione del codice, riavviare IIS.  
  
### <a name="sample-script"></a>Script di esempio  
  
```  
Imports System  
Imports System.Web.Services.Protocols  
Class Sample  
   Public Shared Sub Main()  
Dim rs As New ReportingService()  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
        Dim props(0) As [Property]  
        Dim setProp As New [Property]  
        setProp.Name = "EnableClientPrinting"  
        setProp.Value = "False"   
        props(0) = setProp  
        Try  
            rs.SetSystemProperties(props)  
        Catch ex As System.Web.Services.Protocols.SoapException  
            Console.Write(ex.Detail.InnerXml)  
        Catch e as Exception  
            Console.Write(e.Message)  
        End Try  
    End Sub 'Main  
End Class 'Sample  
```

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
