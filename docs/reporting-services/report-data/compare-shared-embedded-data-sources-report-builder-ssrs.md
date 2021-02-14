---
description: Confrontare origini dati condivise e incorporate - Generatore report e Reporting Services (SSRS)
title: Confrontare origini dati condivise e incorporate - Generatore report e Reporting Services | Microsoft Docs
ms.date: 11/18/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5e2c72ad95392be7571c05e1dddcee1ed35c3d6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042221"
---
# <a name="compare-shared-and-embedded-data-sources---report-builder--reporting-services-ssrs"></a>Confrontare origini dati condivise e incorporate - Generatore report e Reporting Services (SSRS)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]
 
È possibile connettersi ai dati con un'origine dati condivisa o incorporata. Un'*origine dati condivisa* viene definita indipendentemente da qualsiasi report. È possibile usarla in più report in un server di report o in un sito di SharePoint. Un'*origine dati incorporata* viene definita in un report. Può essere usata solo in quel report. 

 Le origini dati condivise sono utili quando si usano spesso le stesse origini dati. È consigliabile creare e usare per lo più origini dati condivise. Rendono i report e l'accesso ai report più facile da gestire, offrono una maggiore protezione dei report e delle origini dati usate. Se necessario, è possibile chiedere all'amministratore di sistema di creare un'origine dati condivisa.  
  
 Un'origine dati incorporata, detta anche *origine dati in base al report*, è una connessione dati salvata nella definizione del report. Le informazioni di connessione a un'origine dati incorporata possono essere usate solo dal report che la contiene. Per definire e gestire le origini dati incorporate, usare la finestra di dialogo **Proprietà origine dati** .  
  
 La differenza tra le origini dati incorporate e quelle condivise dipende dalle diverse modalità di creazione, archiviazione e gestione.  
  
-   In Progettazione report creare origini dati incorporate o condivise come parte di un progetto di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . È possibile controllare se utilizzarle in locale per l'anteprima o distribuirle come parte del progetto in un server di report o in un sito di SharePoint. È possibile utilizzare estensioni per i dati personalizzate installate nel computer e nel server di report o nel sito di SharePoint dove vengono distribuiti i report.  
  
     Gli amministratori di sistema possono installare e configurare estensioni per l'elaborazione dati e provider di dati .NET Framework aggiuntivi. Per altre informazioni, vedere [Estensioni per l'elaborazione dati e provider di dati .NET Framework &#40;SSRS&#41;](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md).  
  
     Gli sviluppatori possono usare l'API <xref:Microsoft.ReportingServices.DataProcessing> per creare estensioni per l'elaborazione dati che supportino ulteriori tipi di origine dati.  
  
-   In [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]accedere a un server di report o a un sito di SharePoint e selezionare le origini dati condivise oppure creare origini dati incorporate nel report. Non è possibile creare un'origine dati condivisa in [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], né usare estensioni per i dati personalizzate in [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  

## <a name="summary-of-differences"></a>Riepilogo delle differenze
  
 La tabella seguente riepiloga le differenze fra le origini dati incorporate e quelle condivise.  
  
|Descrizione|Origine dati<br /><br /> origine dati|Condiviso<br /><br /> origine dati|  
|-----------------|------------------------------|----------------------------|  
|La connessione dati è incorporata nella definizione del report.|![Disponibile](../../reporting-services/report-data/media/greencheck.gif "Disponibile")||  
|Il puntatore alla connessione dati nel server di report è incorporato nella definizione del report.||![Disponibile](../../reporting-services/report-data/media/greencheck.gif "Disponibile")|  
|Gestita nel server di report|![Disponibile](../../reporting-services/report-data/media/greencheck.gif "Disponibile")|![Disponibile](../../reporting-services/report-data/media/greencheck.gif "Disponibile")|  
|Richiesta per i set di dati condivisi||![Disponibile](../../reporting-services/report-data/media/greencheck.gif "Disponibile")|  
|Richiesta per i componenti||![Disponibile](../../reporting-services/report-data/media/greencheck.gif "Disponibile")|  

## <a name="next-steps"></a>Passaggi successivi

[Creare e gestire origini dati condivise](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)   
[Creare e modificare origini dati incorporate](../../reporting-services/report-data/create-and-modify-embedded-data-sources.md)   
[Impostare le proprietà di distribuzione](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
[Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
