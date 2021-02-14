---
description: Opzioni di distribuzione dei modelli (Master Data Services)
title: Opzioni di distribuzione dei modelli
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cf1b17b4-47d5-4eba-83f9-fb0555806867
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: eb67b14b6f0023a0b3b112718b09bb94ddbebff1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340351"
---
# <a name="model-deployment-options-master-data-services"></a>Opzioni di distribuzione dei modelli (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]quando si distribuisce un file di pacchetto di modello, è necessario decidere se distribuire un modello nuovo o clonato oppure se aggiornare un modello clonato in precedenza.  
  
## <a name="workflows"></a>Flussi di lavoro  
 Quando si utilizzano pacchetti di modello, vi sono due flussi di lavoro principali.  
  
-   Creare un pacchetto di un modello in un ambiente di testing e distribuire un clone del modello in un ambiente di produzione. Con il tempo, distribuire gli aggiornamenti dall'ambiente di testing all'ambiente di produzione.  
  
-   Creare un pacchetto di un modello e distribuirlo come nuovo modello nello stesso ambiente. In questo caso, è necessario assegnare al modello un nuovo nome.  
  
## <a name="options"></a>Opzioni  
 Nel database MDS ogni oggetto modello dispone di un identificatore univoco (ID). Questi ID sono inclusi nei pacchetti di distribuzione dei modelli. Quando si distribuisce il pacchetto, è necessario scegliere come utilizzare questi ID.  
  
 Le informazioni nella tabella seguente consentono di determinare la scelta da effettuare quando si distribuisce un modello utilizzando la Distribuzione guidata modello di Amministrazione sistema o lo strumento MDSModelDeploy.  
  
|Opzione|Descrizione|Note|  
|------------|-----------------|-----------|  
|Nuova|Consente di creare un nuovo modello con un nome univoco. Per tutti gli oggetti modello vengono creati nuovi identificatori.|Se si crea un nuovo modello con nuovi identificatori, non è possibile utilizzare gli strumenti di distribuzione del modello per applicare aggiornamenti al modello in un secondo momento. Quando si utilizza la procedura guidata nell'applicazione Web per distribuire un pacchetto di modello, è possibile creare un nuovo modello solo se è già presente un modello con lo stesso nome o ID.|  
|Clone|Consente di creare un nuovo modello che è un clone esatto del modello nel pacchetto. Questa opzione funziona solo se il modello non è presente (con lo stesso nome o identificatore) nell'ambiente di destinazione. Usare la clonazione quando si vuole disporre dello stesso modello in più ambienti e aggiornare il modello clonato nel tempo.|Si tratta del comportamento predefinito della procedura guidata nell'applicazione Web. Se è già presente un modello con lo stesso nome o ID, viene richiesto di creare invece un nuovo modello.|  
|Aggiorna|Consente di aggiornare un modello esistente con il modello nel pacchetto. Gli identificatori devono essere gli stessi in entrambi i modelli. Questa opzione consente di aggiornare un modello clonato in precedenza.|È possibile aggiornare solo i modelli precedentemente clonati. (I nomi e gli ID devono corrispondere).|  
  
## <a name="see-also"></a>Vedere anche  
 [Distribuire un pacchetto di distribuzione di modelli tramite MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)   
 [Distribuire un pacchetto di distribuzione di modelli tramite la procedura guidata](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)   
 [Distribuzione di modelli &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
  
