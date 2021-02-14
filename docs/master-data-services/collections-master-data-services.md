---
description: Raccolte (Master Data Services)
title: Raccolte
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services]
- collections [Master Data Services], about collections
ms.assetid: 5aa1d1e0-b4e5-4897-8e74-01dcf418df73
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 574a99b245764a504297829ba3c037186d88672d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272792"
---
# <a name="collections-master-data-services"></a>Raccolte (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Una raccolta è un gruppo di membri foglia e membri consolidati da una singola entità. Utilizzare le raccolte quando non è necessaria una gerarchia completa e si desidera visualizzare raggruppamenti diversi dei membri per la creazione di report o l'analisi o quando è necessario creare una tassonomia.  
  
> [!NOTE]  
>  La funzione relativa alle raccolte è deprecata.  
  
## <a name="what-collections-contain"></a>Contenuto delle raccolte  
 Le raccolte non impongono un limite sul numero o il tipo di membri che è possibile includere, purché questi si trovino all'interno della stessa entità. Una raccolta può contenere membri foglia e consolidati di più gerarchie esplicite obbligatorie e non obbligatorie.  
  
 Quando si crea una raccolta, non si crea una struttura gerarchica, ma un elenco semplice di membri. Quando si seleziona un nodo da una gerarchia e lo si aggiunge alla raccolta, il membro consolidato selezionato è l'unico membro aggiunto alla raccolta.  
  
 Una raccolta può inoltre contenere altre raccolte. È possibile utilizzare raccolte di raccolte per creare tassonomie.  
  
 L'utente che crea una raccolta viene elencato automaticamente come proprietario. Gli amministratori possono creare altri attributi per la raccolta in base alle necessità.  
  
## <a name="subscription-views-for-collections"></a>Viste sottoscrizioni per le raccolte  
 Sono disponibili due tipi di viste sottoscrizioni che mostrano le raccolte. Il formato **Attributi raccolta** mostra un elenco di raccolte e degli attributi correlati alle raccolte (come descrizione o proprietario). Il formato **Raccolte** mostra tutti i membri di tutte le raccolte,, nonché il peso e l'ordinamento dei membri. Per altre informazioni, vedere [Panoramica: Esportazione di dati &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md).  
  
 Se si impostano valori di peso per membri specifici di una raccolta, questi valori sono disponibili nelle viste sottoscrizioni correlate.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare una nuova raccolta.|[Creare una raccolta &#40;Master Data Services&#41;](../master-data-services/create-a-collection-master-data-services.md)|  
|Aggiungere membri a una raccolta esistente.|[Aggiungere membri a una raccolta &#40;Master Data Services&#41;](../master-data-services/add-members-to-a-collection-master-data-services.md)|  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Gerarchie esplicite &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [Panoramica: Esportazione di dati &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)  
  
  
