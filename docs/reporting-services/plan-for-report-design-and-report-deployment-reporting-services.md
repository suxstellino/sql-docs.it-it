---
title: Pianificare la progettazione e la distribuzione di report | Reporting Services | Microsoft Docs
description: Informazioni su come pianificare un ambiente di creazione di report e un server di report che interagiscono tramite Reporting Services.
ms.date: 09/12/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 1c1e265e-52a2-4de3-96fd-ca4abae01c02
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ec946e008990e9cc4f75fc6a6eb0882756d25b8b
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596557"
---
# <a name="plan-for-report-design-and-report-deployment--reporting-services"></a>Pianificare la progettazione e la distribuzione di report | Reporting Services
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] sono disponibili diversi approcci per la creazione e la distribuzione di report impaginati. Viene illustrato come pianificare un ambiente di creazione di report e un server di report che interagiscono.

In questo argomento viene fornita una panoramica del supporto delle definizioni di report mediante i componenti [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Una definizione di report è un file XML scritto in linguaggio RDL (Report Definition Language) o RDLC (Report Definition Language for Clients). Ogni definizione di report è conforme a una versione di schema specifica elencata all'inizio del file.  
  
 I file RDL vengono creati in Progettazione report nei progetti di [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] e in Generatore report. I file RDLC vengono creati tramite i controlli ReportViewer inclusi in [!INCLUDE[vsprvs](../includes/vsprvs-md.md)].
  
##  <a name="rdl-schema-versions"></a><a name="bkmk_rdl_schema_versions"></a> Versioni dello schema RDL  
 Nella tabella seguente viene fornito un elenco delle versioni dello schema disponibili e delle relative abbreviazioni utilizzate nella parte restante dell'argomento:  
  
|Abbreviazione|Versione dello schema|  
|------------------|--------------------|  
|2016 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition`|
|2010 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`|  
|2008 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2008/01/reportdefinition`|  
|2005 RDL<br /><br /> 2005 RDLC|`https://schemas.microsoft.com/sqlserver/reporting/2005/01/reportdefinition`|  
|2000 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2003/10/reportdefinition`|  
  
 Per ulteriori informazioni su RDL e sugli schemi RDL, vedere quanto riportato di seguito:  
  
-   [XML Schema di Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=31850)  
  
-   [Specifiche del linguaggio RDL (Report Definition Language)](/openspecs/sql_server_protocols/ms-rdl/53287204-7cd0-4bc9-a5cd-d42a5925dca1)  
  
-   [Report Definition Language &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
 Per altre informazioni sui controlli ReportViewer, vedere [Controlli ReportViewer (Visual Studio)](/previous-versions/ms251671(v=vs.140)).  
  
##  <a name="report-server-and-rdl-schema-support"></a><a name="bkmk_report_server_rdl_schema_support"></a> Server di report e supporto dello schema RDL  
 In un server di report di [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] è possibile distribuire un file di definizione del report nei modi seguenti:  
  
-   **Progettazione report:** distribuire un report da Progettazione report in [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)].  
  
-   **Generatore report:** salvare un report nel server di report da Generatore report.  
  
-   **Portale Web:** caricare un report in un server di report in modalità nativa da [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)].  

::: moniker range="=sql-server-2016"
  
-   **SharePoint:** caricare un report in un sito di SharePoint configurato con un server di report in modalità SharePoint.  

::: moniker-end
  
-   **A livello di codice:** pubblicare un report a livello di programmazione tramite le interfacce API SOAP in un server di report. Per ulteriori informazioni, vedere [Report Server Web Service](../reporting-services/report-server-web-service/report-server-web-service.md).  
  
 Nella tabella seguente viene elencata la versione supportata dello schema rdl in base alla versione del server di report.  
  
|Versione del server di report|Versione dello schema RDL|  
|---------------------------|------------------------|  
|SQL Server 2016|2016 RDL<br /><br />2010 RDL<br /><br /> 2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> Oppure<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> Oppure<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|2010 RDL<br /><br /> 2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL|  
  
 Quando si carica una definizione di report nel server di report o si aggiorna un server di report che contiene report esistenti, il server di report mantiene il formato originale della definizione del report. **Quando viene utilizzato per la prima volta**, il server di report aggiorna il report nel database del server di report a un formato binario mantenuto per le viste successive. La definizione del report (con estensione RDL) non viene aggiornata.  
  
 È possibile estrarre dal server di report una copia di sola lettura del file di definizione del report (con estensione RDL). In un server di report in modalità nativa, passare a [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], selezionare il report e fare clic su **Download**. 

::: moniker range="=sql-server-2016"

In una distribuzione in modalità SharePoint, passare alla raccolta documenti, selezionare il report e fare clic su **Scarica una copia**.  

::: moniker-end
  
 Per aggiornare la definizione di report, è necessario aprire il report in un ambiente di creazione di report, ad esempio SQL Server Data Tools o Generatore report, e quindi salvarlo.  
  
 Per altre informazioni sugli aggiornamenti del report e sulle versioni dello schema supportate, vedere [Aggiornare i report](../reporting-services/install-windows/upgrade-reports.md).  
  
##  <a name="report-authoring-and-deployment-support"></a><a name="bkmk_report_authoring_and_deployment"></a> Supporto della creazione e della distribuzione di report  
 Gli ambienti di creazione di report sono Progettazione report nei progetti di [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] e Generatore report. Gli ambienti di creazione di report forniscono supporto per l'aggiornamento e la progettazione di report, la visualizzazione in anteprima dei report in locale o sul server di report e la distribuzione dei report.  
  
 Nella tabella seguente viene riepilogato il supporto per la creazione e la distribuzione di definizioni di report per le diverse versioni dello schema:  
  
|Ambiente di creazione|Versione di RDL creata|Distribuzione versione RDL|Distribuzione alle versioni del server di report|  
|---------------------------|--------------------------|------------------------|--------------------------------------|  
|Generatore report per SQL Server 2016|Crea 2016 RDL<br /><br /> Comporta l'aggiornamento di versioni precedenti di RDL a RDL 2016|2016 RDL|SQL Server 2016|
|Progettazione report in SQL Server 2016 Data Tools - Business Intelligence per Microsoft Visual Studio 2015|Crea 2016 RDL<br /><br /> Comporta l'aggiornamento di versioni precedenti di RDL a RDL 2016|2016 RDL|SQL Server 2016|
|Progettazione report in SQL Server 2014 Data Tools - Business Intelligence per Microsoft Visual Studio 2012<br /><br /> Oppure<br /><br /> Progettazione report in SQL Server 2012 Data Tools - Business Intelligence per Microsoft Visual Studio 2012<br /><br /> Oppure<br /><br /> Progettazione report in [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Data Tools, incluso in [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].|Crea 2010 RDL<br /><br /> Comporta l'aggiornamento di versioni precedenti di RDL a 2010 RDL|2010 RDL|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|Progettazione report in [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] Business Intelligence Development Studio|Crea 2010 RDL<br /><br /> Comporta l'aggiornamento di versioni precedenti di RDL a 2010 RDL|2010 RDL|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|Progettazione report in [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] Business Intelligence Development Studio|Crea 2008 RDL<br /><br /> Comporta l'aggiornamento di versioni precedenti di RDL a 2008 RDL|2008 RDL|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|
  
 Per altre informazioni su SQL Server Data Tools (SSDT), vedere quanto riportato di seguito:  
  
-   [Distribuzione e supporto della versione in SQL Server Data Tools &#40;SSRS&#41;](../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  
  
-   [Scaricare SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).  
  
##  <a name="reportviewer-controls"></a><a name="bkmk_reportviewer"></a> Controlli ReportViewer  
 Un controllo ReportViewer di [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] può visualizzare un report con estensione RDLC in modalità di anteprima locale o in modalità remota, il controllo può visualizzare un file con estensione RDL ospitato in un server di report di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Nella tabella seguente è riportato l'elenco delle versioni di RDL supportate dai controlli ReportViewer per l'elaborazione locale (con estensione RDLC). Il supporto RDL lato server è riepilogato nella sezione [Server di report e supporto dello schema RDL](#bkmk_report_server_rdl_schema_support).  
  
|Controllo ReportViewer nel prodotto|Versione di RDL per l'anteprima locale|  
|-------------------------------------|--------------------------------------|  
|[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2015 <br/><br/>Oppure<br/><br/>[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2013<br /><br /> Oppure<br /><br /> [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2012<br /><br /> Oppure<br /><br /> [!INCLUDE[vs_dev10_long](../includes/vs-dev10-long-md.md)]|2008 RDL|  
|[!INCLUDE[vsprvslong](../includes/vsprvslong-md.md)]<br /><br /> Oppure<br /><br /> [!INCLUDE[vsOrcas](../includes/vsorcas-md.md)]|2005 RDL|  
  
 Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Conversione di file RDLC in file RDL](/previous-versions/ms252109(v=vs.140))  
  
-   [Controlli ReportViewer (Visual Studio)](/previous-versions/ms251671(v=vs.140))  
  
-   [Aggiunta e configurazione dei controlli ReportViewer](/previous-versions/ms252104(v=vs.140))  
  
## <a name="see-also"></a>Vedere anche  
 [Report, parti del report e definizioni dei report &#40;Generatore report e SSRS&#41;](../reporting-services/report-design/reports-report-parts-and-report-definitions-report-builder-and-ssrs.md)   
 [Strumenti di Reporting Services](../reporting-services/tools/reporting-services-tools.md)   
 [Report Definition Language &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
