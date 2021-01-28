---
title: Distribuzione e supporto della versione in SQL Server Data Tools (SSRS) | Microsoft Docs
description: Informazioni sulle versioni supportate per creare e pubblicare report in SQL Server Data Tools e su come impostare le proprietà di configurazione e distribuzione dei progetti.
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
ms.assetid: 36f5686d-7e40-4f31-be81-bd197ca33a02
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c820c7a9e0cefaee2a22a401213754d0a3274152
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596174"
---
# <a name="deployment-and-version-support-in-sql-server-data-tools-ssdt"></a>Distribuzione e supporto della versione in SQL Server Data Tools (SSRS)
  [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] supporta gli scenari seguenti:  
  
-   Apertura delle definizioni dei report (*.rdl) e dei progetti server di report (\*.rptproj).  
  
-   Compilazione delle definizioni di report.  
  
-   Visualizzazione in anteprima dei report in Progettazione report.  
  
-   Distribuzione dei report nei server di report.  
  
##  <a name="configuration-and-deployment-properties"></a><a name="bkmk_ConfigurationandDeploymentProperties"></a> Proprietà di configurazione e distribuzione  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] supporta le configurazioni di progetto. Una configurazione di progetto è costituita da un set di proprietà che consentono di specificare percorsi e comportamenti quando un progetto viene compilato come passaggio nella visualizzazione in anteprima o nella distribuzione dei report. Per ulteriori informazioni sulle configurazioni di progetto, vedere la documentazione di Visual Studio.  
  
 Utilizzare le configurazioni di progetto per controllare l'aggiornamento delle definizioni di report alle versioni dello schema compatibili con i server di report di destinazione. Le proprietà controllate dalle configurazioni di progetto includono il server di report di destinazione, la cartella in cui il processo di compilazione consente di archiviare temporaneamente le definizioni di report per l'anteprima e la distribuzione, nonché i livelli di errore.  
  
 I report vengono compilati prima di essere sottoposti a rendering come anteprime in Progettazione report o di essere distribuiti nel server di report.  
  
 È possibile impostare le proprietà di configurazione nella finestra di dialogo **Proprietà progetto** di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Nelle proprietà di compilazione e distribuzione è incluso quanto segue:  
  
-   OutputPath è una proprietà di compilazione che identifica il percorso delle cartelle per archiviare la definizione del report usata nella verifica della compilazione, nella distribuzione e nell'anteprima dei report.  
  
-   ErrorLevel è una proprietà di compilazione che identifica la gravità dei problemi di compilazione segnalati come errori. I problemi con livelli di gravità minori o uguali al valore di ErrorLevel vengono segnalati come errori; in caso contrario, vengono segnalati come avvisi. Per altre informazioni, vedere la sezione "Convalida del report e livelli di errore" di [Progettare report con Progettazione report &#40;SSRS&#41;](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md).  
  
-   TargetServerVersion è una proprietà di distribuzione che consente di identificare la versione prevista di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installata nel server di report di destinazione specificato nella proprietà TargetServerURL.  
  
 Quando si specifica la versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nella finestra di dialogo **Proprietà progetto**, non vengono ripristinati automaticamente i report di questa versione. Di conseguenza, un progetto server di report può contenere report delle due diverse versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando il progetto server di report viene distribuito, tutti i report presenti nel progetto vengono convertiti nella versione specificata in TargetServerVersion.  
  
 È possibile aggiungere più di una configurazione di progetto a un progetto; ognuna viene utilizzata per uno scenario differente, ad esempio la distribuzione in versioni diverse di server di report. Per altre informazioni, vedere [Impostare le proprietà di distribuzione &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md) e [Finestra di dialogo Pagine delle proprietà del progetto](../../reporting-services/tools/project-property-pages-dialog-box.md).  
  
##  <a name="supported-versions"></a><a name="bkmk_SupportedVersions"></a> Versioni supportate  
  
> [!NOTE]  
>  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], l'ambiente di sviluppo a 32 bit per progetti server di report, non può essere eseguito in computer basati su [!INCLUDE[vcpritanium](../../includes/vcpritanium-md.md)] e non può essere installato nei server basati su [!INCLUDE[vcpritanium](../../includes/vcpritanium-md.md)]. Tuttavia, è possibile usare [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] in computer con processore x64.  
  
 Nella tabella seguente sono descritte le versioni supportate per la creazione e la pubblicazione di report in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
> [!NOTE]  
>  Lo schema non è stato cambiato fin da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
|Tipo di progetto o file|Versione|Creazione di report|Pubblicazione di report|Note|  
|--------------------------|-------------|--------------------|---------------------|-----------|  
|Progetto server di report<br /><br /> o<br /><br /> Progetto Creazione guidata report|[!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]|Schema RDL 2016|[!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]||  
|Progetto server di report<br /><br /> o<br /><br /> Progetto Creazione guidata report|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|Schema RDL 2014|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]||  
|Progetto server di report<br /><br /> o<br /><br /> Progetto Creazione guidata report|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|Schema RDL 2012|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]||  
|Progetto server di report<br /><br /> o<br /><br /> Progetto Creazione guidata report|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|Schema RDL 2008 R2|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]||  
|Progetto server di report<br /><br /> o<br /><br /> Progetto Creazione guidata report|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|Schema RDL 2008|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] solo server di report|Aggiornamenti dello schema da RDL 2003 e RDL 2005 a RDL 2008 in locale.|  
  
 Per altre informazioni sull'apertura di report in una versione precedente dello schema di definizione del report, vedere [Aggiornare i report](../../reporting-services/install-windows/upgrade-reports.md). Per ulteriori informazioni su schemi di definizione dei report specifici, vedere la pagina relativa alla [specifica del linguaggio RDL](/openspecs/sql_server_protocols/ms-rdl/53287204-7cd0-4bc9-a5cd-d42a5925dca1).  
  
## <a name="see-also"></a>Vedere anche  
 [Pubblicazione di origini dati e report](../../reporting-services/reports/publishing-data-sources-and-reports.md)  
  
