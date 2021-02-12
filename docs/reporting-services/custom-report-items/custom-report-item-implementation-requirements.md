---
title: Requisiti per l'implementazione di elementi dei report personalizzati| Microsoft Docs
description: Informazioni sui requisiti di sviluppo e distribuzione necessari per le implementazioni di elementi di report personalizzati.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items
ms.assetid: cfacd816-00d6-4a3d-be72-1bba6f7f6886
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 95f33dde355621aecea3a67376b1d28f67b88cdc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062076"
---
# <a name="custom-report-item-implementation-requirements"></a>Requisiti per l'implementazione di elementi dei report personalizzati
  In questo argomento vengono illustrati i prerequisiti per lo sviluppo e la distribuzione di elementi dei report personalizzati.  
  
## <a name="development-and-deployment-requirements"></a>Requisiti relativi a sviluppo e distribuzione  
 Per lo sviluppo di un elemento del report personalizzato per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è necessario quanto segue:  
  
-   Accesso amministrativo a un server che esegue [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
-   [!INCLUDE[vsprvsext](../../includes/vsprvsext-md.md)] o versioni successive in cui sia installato [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  
  
-   Accesso alla documentazione di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  
  
-   Familiarità con le attività di creazione dei componenti e gli spazi dei nomi del modello di componente in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
## <a name="language-and-namespace-requirements"></a>Requisiti relativi a linguaggio e spazio dei nomi  
 Gli elementi dei report personalizzati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offrono supporto completo per [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. È possibile sviluppare elementi dei report personalizzati utilizzando un linguaggio di propria scelta conforme a .NET.  
  
 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] offre allo sviluppatore numerosi strumenti e caratteristiche per semplificare e accelerare i cicli iterativi di codifica, di debug e test, nonché per semplificare la distribuzione. [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK include i compilatori [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] e C# e gli strumenti correlati.  
  
-   Gli elementi del report personalizzati usano gli spazi dei nomi **Microsoft.ReportDesigner** e <xref:Microsoft.ReportingServices.Interfaces>. Questi elementi sono archiviati negli assembly Microsoft.ReportingServices.Designer.DLL e Microsoft.ReportingServices.Interfaces.DLL, installati con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   I componenti della fase di progettazione degli elementi dei report personalizzati devono implementare le interfacce dallo spazio dei nomi <xref:System.ComponentModel> in [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. La documentazione relativa a <xref:System.ComponentModel> è disponibile in [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  

## <a name="see-also"></a>Vedere anche  
 [Creazione di un componente runtime dell'elemento del report personalizzato](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Creazione di un componente dell'elemento del report personalizzato per la fase di progettazione](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)   
 [Procedura: Distribuire un elemento del report personalizzato](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)   
 [Librerie di classi dell'elemento del report personalizzato](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
  
  
