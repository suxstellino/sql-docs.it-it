---
description: Compilazione di un modello (componente aggiuntivo MDS per Excel)
title: Compilazione di un modello
ms.custom: microsoft-excel-add-in
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8ae26ec3-c5d5-4c4f-a810-2951a7454439
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ffdf789438f2699bfa9d18c74fa4f748bfe3996d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351516"
---
# <a name="building-a-model-mds-add-in-for-excel"></a>Compilazione di un modello (componente aggiuntivo MDS per Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]gli amministratori possono eseguire un subset delle funzioni amministrative disponibili nell'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
 Le attività di compilazione dei modelli che gli amministratori possono eseguire in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] sono le seguenti:  
  
-   Creare entità. Per altre informazioni sulle entità, vedere [Entità &#40;Master Data Services&#41;](../../master-data-services/entities-master-data-services.md).  
  
-   Creare attributi di tutti i tipi, inclusi attributi basati su dominio. Per altre informazioni sugli attributi, vedere [Attributi &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md) e [Attributi basati su dominio &#40;Master Data Services&#41;](../../master-data-services/domain-based-attributes-master-data-services.md).  
  
 Un amministratore deve creare il modello utilizzando il servizio Web o l'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . È quindi possibile utilizzare [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] per creare entità e attributi all'interno del modello. Per altre informazioni sugli oggetti modello, vedere [Modelli &#40;Master Data Services&#41;](../../master-data-services/models-master-data-services.md).  
  
## <a name="related-tasks"></a>Attività correlate  
 La maggior parte delle attività amministrative deve ancora essere eseguita nell'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] oppure tramite il servizio Web. Nella tabella seguente sono illustrati gli strumenti che gli amministratori possono utilizzare per completare le attività in MDS.  
  
|Descrizione dell'attività|Strumento|Argomento|  
|----------------------|----------|-----------|  
|Creare modelli.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Creare un modello &#40;Master Data Services&#41;](../../master-data-services/create-a-model-master-data-services.md)|  
|Creare un'entità.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] (applicazione Web), servizio Web o [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]|[Creare un'entità &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/create-an-entity-mds-add-in-for-excel.md)|  
|Creare un attributo basato su dominio.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] (applicazione Web), servizio Web o [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]|[Creare un attributo basato su dominio &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/create-a-domain-based-attribute-mds-add-in-for-excel.md)|  
|Creare gruppi di attributi.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Creare un gruppo di attributi &#40;Master Data Services&#41;](../../master-data-services/create-an-attribute-group-master-data-services.md)|  
|Creare regole business.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Creare e pubblicare una regola business &#40;Master Data Services&#41;](../../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|Creare viste sottoscrizioni.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Creare una vista sottoscrizioni per esportare i dati &#40;Master Data Services&#41;](../../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)|  
|Creare gerarchie.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Creare una gerarchia derivata &#40;Master Data Services&#41;](../../master-data-services/create-a-derived-hierarchy-master-data-services.md)<br /><br /> [Creare una gerarchia esplicita &#40;Master Data Services&#41;](../../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|Creare raccolte.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Creare una raccolta &#40;Master Data Services&#41;](../../master-data-services/create-a-collection-master-data-services.md)|  
|Creare versioni dei dati.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web o servizio Web|[Bloccare una versione &#40;Master Data Services&#41;](../../master-data-services/lock-a-version-master-data-services.md)|  
|Distribuire modelli.|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web, servizio Web o strumento MDSModelDeploy.|[Creare un pacchetto di distribuzione di modelli tramite MDSModelDeploy](../../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Modelli &#40;Master Data Services&#41;](../../master-data-services/models-master-data-services.md)  
  
-   [Entità &#40;Master Data Services&#41;](../../master-data-services/entities-master-data-services.md)  
  
-   [Attributi &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md)  
  
-   [Attributi basati su dominio &#40;Master Data Services&#41;](../../master-data-services/domain-based-attributes-master-data-services.md)  
  
-   [Gruppi di attributi &#40;Master Data Services&#41;](../../master-data-services/attribute-groups-master-data-services.md)  
  
-   [Regole di business &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md)  
  
-   [Panoramica: Esportazione di dati &#40;Master Data Services&#41;](../../master-data-services/overview-exporting-data-master-data-services.md)  
  
-   [Gerarchie &#40;Master Data Services&#41;](../../master-data-services/hierarchies-master-data-services.md)  
  
-   [Raccolte &#40;Master Data Services&#41;](../../master-data-services/collections-master-data-services.md)  
  
-   [Versioni &#40;Master Data Services&#41;](../../master-data-services/versions-master-data-services.md)  
  
-   [Sicurezza &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md)  
  
-   [Distribuzione di modelli &#40;Master Data Services&#41;](../../master-data-services/deploying-models-master-data-services.md)  
  
  
