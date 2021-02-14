---
title: Gerarchie ricorsive
description: In Master Data Services, una gerarchia ricorsiva include una relazione ricorsiva, che esiste quando un'entità dispone di un attributo basato su dominio basato sull'entità.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- recursive hierarchies [Master Data Services]
- hierarchies [Master Data Services], recursive hierarchies
ms.assetid: 9408c6ea-d9c4-4a0b-8a1b-1457fb6944af
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e5aaae9f766832d64c3ca9e59940b933fbcda1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353708"
---
# <a name="recursive-hierarchies-master-data-services"></a>Gerarchie ricorsive (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]una gerarchia ricorsiva è una gerarchia derivata in cui è inclusa una relazione ricorsiva. Una relazione ricorsiva si verifica quando un'entità dispone di un attributo basato su dominio che si fonda sull'entità stessa.  
  
## <a name="recursive-hierarchy-example"></a>Esempio di gerarchia ricorsiva  
 Un esempio tipico di gerarchia ricorsiva è una struttura organizzativa. In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]questa operazione viene effettuata creando un'entità Employee con un attributo basato su dominio chiamato Manager. L'attributo Manager viene popolato dall'elenco dei dipendenti. Nell'organizzazione di esempio, tutti i dipendenti possono essere responsabili (Manager).  
  
 ![mds_conc_recursive_table_w_data](../master-data-services/media/mds-conc-recursive-table-w-data.gif "mds_conc_recursive_table_w_data")  
  
 È possibile creare una gerarchia derivata che evidenzia la relazione tra l'entità Employee e l'attributo basato su dominio Manager.  
  
 ![mds_conc_recursive_UI_structure](../master-data-services/media/mds-conc-recursive-ui-structure.gif "mds_conc_recursive_UI_structure")  
  
 Per includere ciascun membro solo una volta nella gerarchia, è possibile ancorare relazioni Null. Quando si fa ciò, i membri con valori di attributo basati su dominio vuoti vengono visualizzati al livello gerarchico principale.  
  
 ![mds_conc_recursive_UI_example_anchored](../master-data-services/media/mds-conc-recursive-ui-example-anchored.gif "mds_conc_recursive_UI_example_anchored")  
  
 Se non si ancorano relazioni Null, i membri vengono inclusi più volte. Tutti i membri vengono visualizzati al livello principale. Vengono anche visualizzati al di sotto dei membri dei quali sono attributi.  
  
 ![mds_conc_recursive_UI_example_nonanchored](../master-data-services/media/mds-conc-recursive-ui-example-nonanchored.gif "mds_conc_recursive_UI_example_nonanchored")  
  
 In questo esempio, Marcia è al livello principale. Non è il responsabile (Manager) di alcun dipendente poiché non viene utilizzata come un valore di attributo basato su dominio per nessun altro membro definito come Employee. Robert, invece, presenta un livello al di sotto di lui poiché Marcia ha Robert come suo valore dell'attributo Manager.  
  
## <a name="rules"></a>Regole  
  
-   Una gerarchia derivata non può contenere più di una relazione ricorsiva. Tuttavia, può disporre di altre relazioni derivate, ad esempio una gerarchia derivata che contiene una relazione ricorsiva da amministratore a dipendente può disporre anche delle relazioni da paese ad amministratore e da dipendente a archivia).  
  
-   Non è possibile assegnare autorizzazioni (nella scheda **Membri gerarchia** ) ai membri appartenenti a una gerarchia ricorsiva.  
  
-   Le gerarchie ricorsive non possono includere relazioni circolari. Ad esempio, Katherine non può essere il responsabile (Manager) di Sandeep se Sandeep è il suo responsabile. Inoltre, Katherine non può essere il responsabile (Manager) di se stessa.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare una gerarchia derivata.|[Creare una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|Modificare il nome di una gerarchia derivata esistente.|[Modificare il nome di una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/change-a-derived-hierarchy-name-master-data-services.md)|  
|Eliminare una gerarchia derivata esistente.|[Eliminare una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/delete-a-derived-hierarchy-master-data-services.md)|  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Attributi basati su dominio &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md)  
  
-   [Gerarchie derivate &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
