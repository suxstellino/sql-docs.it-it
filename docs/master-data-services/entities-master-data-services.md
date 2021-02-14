---
title: Entità
description: Le entità sono oggetti contenuti nei modelli Master Data Services. Ogni entità contiene membri che sono le righe dei dati master gestiti.
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], about entities
- entities [Master Data Services]
ms.assetid: 0af057d5-6b73-472b-99eb-9f5eb61a9b5b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 2862edbc70fdd2fd4d26ba580a04b18ebe1c9ce3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341776"
---
# <a name="entities-master-data-services"></a>Entità (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Le entità sono oggetti contenuti nei modelli [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Ogni entità contiene membri che sono le righe dei dati master gestiti.  
  
## <a name="how-many-entities-are-appropriate"></a>Numero di entità appropriate  
 I modelli possono contenere tutte le entità che si desidera gestire. Ogni entità deve raggruppare un tipo di dati simile. Ad esempio, potrebbe essere necessaria un'entità per tutti gli account aziendali o un'entità per l'elenco principale di dipendenti.  
  
 Sono in genere disponibili una o più entità centrali importanti per l'azienda, alle quali sono correlati altri oggetti nel modello. Ad esempio, in un modello Product è possibile disporre di un'entità centrale denominata Product e di entità denominate Subcategory e Category alle quali è correlata l'entità Product. Tuttavia, non è necessario disporre di un'entità centrale. A seconda delle necessità, è possibile disporre di diverse entità che sono ritenute della stessa importanza.  
  
## <a name="how-entities-relate-to-other-model-objects"></a>Correlazione tra entità e gli altri oggetti modello  
 Un'entità può essere considerata come una tabella che contiene dati master, dove le righe rappresentano i membri e le colonne rappresentano gli attributi.  
  
 ![Entità Master Data Services rappresentata come tabella](../master-data-services/media/mds-conc-entity-table.gif "Entità Master Data Services rappresentata come tabella")  
  
 Popolare l'entità con un elenco di dati master che si desidera gestire.  
  
 Le entità possono essere utilizzate per compilare gerarchie derivate, che sono gerarchie basate su livelli in base a più entità. Per altre informazioni, vedere [Gerarchie derivate &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md).  
  
 Le entità inoltre possono essere abilitate a contenere gerarchie esplicite (strutture incomplete basate su una sola entità) e raccolte (uniche combinazioni di subset di membri). Per altre informazioni, vedere [Gerarchie esplicite &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md) e [Raccolte &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md).  
  
## <a name="using-entities-as-constrained-lists"></a>Utilizzo di entità come elenchi vincolati  
 Quando gli utenti stanno assegnando gli attributi ai membri in un'entità, è possibile sceglierli da un elenco di valori vincolato. A tale scopo, utilizzare un'entità per popolare l'elenco di valori per l'attributo. Questa caratteristica è nota come attributo basato su dominio. Per altre informazioni, vedere [Attributi basati su dominio &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md).  
  
## <a name="base-entities"></a>Entità di base  
 Un'entità di base rappresenta un punto di partenza per gli utenti, durante lo spostamento tra oggetti nel modello. Un'entità di base determina il layout dello schermo quando un utente apre l'area funzionale **Visualizzatore** e fa clic su **Visualizzatore** sulla barra dei menu. Per specificare un'entità come entità di base, passare all'area funzionale **Amministrazione sistema** . Nella pagina **Vista modello** trascinare l'entità dal controllo albero sulla destra sul nome del modello nel controllo albero sulla sinistra.  
  
## <a name="entity-security"></a>Sicurezza dell'entità  
 È possibile concedere agli utenti l'autorizzazione per un'entità che include oggetti modello correlati. Per altre informazioni, vedere [Autorizzazioni per le entità &#40;Master Data Services&#41;](../master-data-services/entity-permissions-master-data-services.md).  
  
## <a name="entity-examples"></a>Esempi di entità  
 Nell'esempio seguente viene illustrata un'entità che dispone degli attributi Name, Code, Subcategory, StandardCost, ListPrice e FilePhoto. Tali attributi descrivono i membri. Ogni membro viene rappresentato da una singola riga di valori di attributo.  
  
 ![Tabella dell'entità relativa al prodotto biciclette](../master-data-services/media/mds-conc-entity-table-w-data.gif "Tabella dell'entità relativa al prodotto biciclette")  
  
 Nel seguente esempio, l'entità Product è l'entità centrale. L'entità Subcategory è un attributo basato su dominio dell'entità Product. L'entità Category è un attributo basato su dominio dell'entità Subcategory. StandardCost e ListPrice sono attributi in formato libero dell'entità Product mentre FilePhoto è un attributo di file dell'entità Product.  
  
 ![Struttura ad albero dell'entità relativa al prodotto](../master-data-services/media/mds-conc-entity-ui.gif "Struttura ad albero dell'entità relativa al prodotto")  
  
> [!NOTE]  
>  Si tratta di un esempio basato sull'interfaccia utente di [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . La struttura ad albero gerarchica mostra le relazioni tra le entità e gli attributi basati su dominio. Lo scopo è illustrare le relazioni anziché rappresentare i livelli di importanza.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare una nuova entità.|[Creare un'entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)|  
|Modificare il nome di un'entità esistente.|[Modificare un'entità &#40;Master Data Services&#41;](../master-data-services/edit-an-entity-master-data-services.md)|  
|Eliminare un'entità esistente.|[Eliminare un'entità &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)|  
|Assegnare autorizzazioni alle entità.|[Assegnare autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)|  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Modelli &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
-   [Membri &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)  
  
-   [Attributi &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
