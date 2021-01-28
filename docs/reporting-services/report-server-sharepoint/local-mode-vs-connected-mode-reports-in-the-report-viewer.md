---
title: Report in modalità locale e con connessione in Visualizzatore di report | Microsoft Docs
description: I report di SQL Server Reporting Services possono essere configurati per l'esecuzione in modalità locale o modalità connessa. Informazioni sulle diverse modalità.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: beeba27a4186aa8ea19401284b8e1f3c4c7f6990
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597198"
---
# <a name="local-mode-vs-connected-mode-reports-in-the-report-viewer"></a>Report in modalità locale e con connessione in Visualizzatore di report

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] possono essere configurati per l'esecuzione in *modalità locale* o *modalità connessa*, che usa un server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] repot server. In alternativa, è possibile utilizzare il visualizzatore di report per eseguire il rendering dei report direttamente da SharePoint quando l'estensione per i dati supporta la creazione di report in modalità locale. Questo approccio è definito *modalità locale*. Nelle versioni precedenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]è necessario che la farm di SharePoint sia connessa a un server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configurato in modalità SharePoint per eseguire il rendering dei report tramite il controllo Visualizzatore report. Questo approccio è definito *modalità remota* o *modalità con connessione*.  

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.

 In *modalità locale* non è presente un server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . È necessario installare il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per prodotti SharePoint, ma non è richiesto alcun server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Con la modalità locale, gli utenti possono visualizzare report ma non potranno accedere alle funzionalità lato server quali sottoscrizioni e avvisi dati.  

## <a name="local-mode-vs-connected-mode-and-supported-extensions"></a>Confronto tra modalità locale e modalità connessa ed estensioni supportate

 **Modalità locale:** se è disponibile un'estensione dati che supporta la modalità locale, il Visualizzatore report esegue direttamente il rendering dei report da SharePoint. In *modalità locale* non è presente un server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . È necessario installare il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per prodotti SharePoint, ma non è richiesto alcun server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Con la modalità locale, gli utenti possono visualizzare report ma **non** potranno accedere alle funzionalità lato server quali sottoscrizioni e avvisi dati.  
  
 La **modalità connessa**, definita anche *modalità remota* , richiede un server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità SharePoint, connesso alla farm di SharePoint, in modo che il Visualizzatore report possa eseguire il rendering dei report.  
  
 Di seguito è riportato un elenco delle estensioni per l'elaborazione dati che supportano la creazione di report in modalità locale:  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access 2010. Per altre informazioni su Access Services, vedere l'articolo [Use Access Services with SQL Reporting Services: Installing SQL Server 2008 R2 Reporting Services Add-In (SharePoint Server 2010)](/previous-versions/office/sharepoint-server-2010/ee662542(v=office.14))(Utilizzo di Access Services con SQL Reporting Services: installazione del componente aggiuntivo SQL Server 2008 R2 Reporting Services (SharePoint Server 2010)).  
  
-   Estensione per i dati dell'elenco SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni sull'estensione per i dati dell'elenco SharePoint, vedere [Origini dei dati supportate da Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
  
 È inoltre possibile sviluppare estensioni personalizzate per l'elaborazione dati per il supporto della modalità locale. Per ulteriori informazioni, vedere [Implementing a Data Processing Extension](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
 Nella modalità locale è possibile eseguire il rendering di report che presentano un'origine dati incorporata o un'origine dati condivisa da un file rsds. Non è tuttavia possibile gestire il report o la relativa origine dati associata. Se si tenta di eseguire questa operazione, un errore segnalerà che tale operazione non è supportata in modalità locale. La gestione delle origini dati nel sito di SharePoint è supportata solo in modalità con connessione.  
  
> [!NOTE]  
>  Come nelle versioni precedenti, non è possibile incorporare nomi utente e password nel file con estensione rsds.  
  
## <a name="configure-local-mode-and-access-services-with-sharepoint-2013"></a>Configurare la modalità locale e Access Services con SharePoint 2013

 È possibile configurare la farm di SharePoint 2013 per supportare database Web esistenti di Access 2010 e la modalità locale di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per ulteriori informazioni, vedere l'articolo relativo all' [installazione e alla configurazione dei servizi di Access 2010 per database Web in SharePoint Server 2013](https://technet.microsoft.com/library/ee748653\(office.15\).aspx).  
  
 Non è possibile creare nuovi database Web di Access per SharePoint 2013. In Access 2013 viene utilizzato un nuovo tipo di database, *app Web Access* che si compila in Access e quindi si utilizza e condivide con altri come app SharePoint in un Web browser.  
  
 Per ulteriori informazioni, vedere quanto segue.  
  
-   [Novità di Access 2013](https://office.microsoft.com/access-help/what-s-new-in-access-2013-HA102809500.aspx) (https://office.microsoft.com/access-help/what-s-new-in-access-2013-HA102809500.aspx).  
  
-   [Attività di base per un'app Access](https://office.microsoft.com/access-help/basic-tasks-for-an-access-app-HA102840210.aspx?CTT=5&origin=HA102809500) (https://office.microsoft.com/access-help/basic-tasks-for-an-access-app-HA102840210.aspx?CTT=5&origin=HA102809500).  
  
## <a name="configure-local-mode-reporting-with-sharepoint-2010"></a>Configurare la creazione di report in modalità locale con SharePoint 2010

 La modalità locale richiede lo stato della sessione ASP.NET. L'installazione di Access Services abilita lo stato delle sessioni ASP.NET. È inoltre possibile eseguire l'abilitazione tramite PowerShell.  
  
1.  Aprire la shell di gestione di SharePoint 2010.  
  
2.  Digitare il comando seguente:  
  
    ```  
    - Enable-SPSessionStateService  
    ```  
  
3.  Quando richiesto, digitare il nome del database.  
  
4.  Eseguire una reimpostazione IIS.  
  
 Per altre informazioni, vedere l'articolo [Use Access Services with SQL Reporting Services: Installing SQL Server 2008 R2 Reporting Services Add-In (SharePoint Server 2010)](/previous-versions/office/sharepoint-server-2010/ee662542(v=office.14)) (Utilizzo di Access Services con SQL Reporting Services: installazione del componente aggiuntivo SQL Server 2008 R2 Reporting Services (SharePoint Server 2010)) e [Enable-SPSessionStateService](https://technet.microsoft.com/library/ff607857\(v=office.15\).aspx).  
  
## <a name="connected-mode"></a>modalità connessa

 Per le informazioni più recenti sull'uso delle estensioni ADS con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità connessa, vedere l'articolo relativo al [report di Access Services nel sito di SharePoint con errori nell'estensione dati 'ADS'](https://social.technet.microsoft.com/wiki/contents/articles/25298.access-services-report-in-sharepoint-site-shows-error-in-data-extension-ads.aspx).  
  
## <a name="see-also"></a>Vedere anche

 [Origini dati supportate da Reporting Services](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)