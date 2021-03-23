---
title: Distribuzione di modelli
description: Distribuire i pacchetti di modelli per spostare le copie di modelli da un ambiente Master Data Services a un altro o per creare nuovi modelli nell'ambiente in uso.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deployment packages [Master Data Services], about deployment packages
- deployment packages [Master Data Services]
ms.assetid: 30085c08-034f-4efe-80fe-408f9091ff5c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec1216cdc50f13279be057558dfd6e10800e9b47
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833796"
---
# <a name="deploying-models-master-data-services"></a>Distribuzione di modelli (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]un pacchetto è un file XML contenente una struttura di modello distribuibile e, facoltativamente, i dati del modello. Utilizzare i pacchetti del modello per spostare le copie di modelli da un ambiente MDS a un altro o per creare nuovi modelli nell'ambiente MDS esistente.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] e versioni successive **lo strumento MDSModelDeploy** è compatibile con le versioni precedenti dei pacchetti creati in [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] o versioni successive.  
  
## <a name="tools-for-deploying-models"></a>Strumenti per la distribuzione di modelli  
 Per utilizzare i pacchetti di modello sono disponibili tre strumenti, a seconda delle proprie esigenze.  
  
-   **Strumento MDSModelDeploy**: per creare e distribuire oggetti modello e dati, usare lo strumento MDSModelDeploy.exe. Se durante l'installazione di MDS è stato selezionato il percorso predefinito, questo strumento si trova in *unità*:\Programmi\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
-   **Distribuzione guidata modello**: per creare e distribuire solo pacchetti della struttura del modello, usare la procedura guidata nell'applicazione Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Non è possibile utilizzare questa procedura guidata per distribuire dati.  
  
-   **Editor pacchetti di modelli**: per modificare un pacchetto di modelli, usare ModelPackageEditor.exe che avvia la procedura guidata Editor pacchetti di modelli. Questa procedura guidata viene utilizzata per modificare un pacchetto creato dallo strumento MDSModelDeploy o dalla Distribuzione guidata modello. Se durante l'installazione di MDS è stato selezionato il percorso predefinito, questo strumento si trova in *unità*:\Programmi\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
> [!IMPORTANT]  
>  È possibile usare lo strumento MDSModelDeploy per creare un nuovo modello, creare un clone di un modello oppure aggiornare un modello esistente e i relativi dati. Se si utilizza lo strumento MDSModelDeploy per aggiornare un modello esistente e i relativi dati e nel pacchetto non è contenuto alcun attributo, entità o membro disponibile nel modello di destinazione, questi elementi non saranno eliminati dal modello tramite MDSModelDeploy.  
  
## <a name="what-packages-contain"></a>Contenuto dei pacchetti  
 Un pacchetto del modello è un file XML salvato con estensione pkg. Quando si crea un pacchetto di distribuzione, è possibile decidere se includere o meno dati. Se si decide di includere dati, è necessario selezionare una versione dei dati da includere.  
  
 Tutti gli oggetti modello vengono inclusi in un pacchetto. Questi oggetti sono:  
  
-   Entità  
  
-   Attributi  
  
-   Gruppi di attributi  
  
-   Gerarchie  
  
-   Raccolte  
  
-   Regole business  
  
-   Flag versione  
  
-   Viste sottoscrizioni  
  
 Non sono inclusi gli attributi di file e le autorizzazioni di utenti e gruppi. Dopo avere distribuito un modello, è necessario aggiornare questi elementi manualmente.  
  
## <a name="sample-packages"></a>Pacchetti di esempio  
 I file di pacchetto di esempio sono inclusi nell'installazione di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Questi file di pacchetto si trovano nella directory Master Data Services\Samples\Packages in cui è stato installato [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Quando si distribuiscono questi pacchetti di esempio tramite lo strumento MDSModelDeploy, i modelli di esempio vengono creati e popolati con dati.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare un nuovo pacchetto di distribuzione di oggetti modello e/o dati tramite lo strumento MDSModelDeploy.|[Creare un pacchetto di distribuzione di modelli tramite MDSModelDeploy](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
|Creare un nuovo pacchetto di distribuzione solo di oggetti modello utilizzando la procedura guidata.|[Creare un pacchetto di distribuzione di modelli tramite la procedura guidata](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md)|  
|Distribuite un pacchetto di oggetti modello e/o dati tramite lo strumento MDSModelDeploy.|[Distribuire un pacchetto di distribuzione di modelli tramite MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
|Distribuire un pacchetto solo di oggetti modello utilizzando la procedura guidata.|[Distribuire un pacchetto di distribuzione di modelli tramite la procedura guidata](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)|  
|Modificare un pacchetto di distribuzione del modello per distribuire parti selezionate di un modello, anziché l'intero modello.|[Modificare un pacchetto di distribuzione di modelli](../master-data-services/edit-a-model-deployment-package.md)|  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Opzioni di distribuzione dei modelli &#40;Master Data Services&#41;](../master-data-services/model-deployment-options-master-data-services.md)  
  
  
