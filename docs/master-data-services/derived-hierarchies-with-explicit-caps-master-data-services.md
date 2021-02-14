---
description: Gerarchie derivate con estremità esplicite (Master Data Services)
title: Gerarchie derivate con estremità esplicite
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [Master Data Services], derived hierarchies with explicit caps
- explicit hierarchies, derived hierarchies with explicit caps
- derived hierarchies, derived hierarchies with explicit caps
ms.assetid: 6a82ff66-c137-4757-99bb-787d189b4295
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 24996956a4ee2b1fef5904337eaaf2c735770d37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350117"
---
# <a name="derived-hierarchies-with-explicit-caps-master-data-services"></a>Gerarchie derivate con estremità esplicite (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], quando i livelli di una gerarchia esplicita vengono usati come livelli principali di una gerarchia derivata, questa viene chiamata gerarchia derivata con estremità esplicita.  
  
 La gerarchia esplicita deve essere basata sulla stessa entità dell'entità al livello principale della gerarchia derivata.  
  
 Nell'interfaccia utente di [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , si crea questo tipo di gerarchia trascinando una gerarchia esplicita al di sopra di una gerarchia derivata.  
  
 ![mds_conc_explicit_cap_UI_structure](../master-data-services/media/mds-conc-explicit-cap-ui-structure.gif "mds_conc_explicit_cap_UI_structure")  
  
## <a name="derived-hierarchy-with-explicit-cap-example"></a>Esempio di gerarchia derivata con estremità esplicita  
 In questo esempio, i membri nella gerarchia esplicita provengono dall'entità Subcategory. Nella gerarchia derivata, anche i membri del livello principale provengono dall'entità Subcategory.  
  
 ![mds_conc_explicit_cap_UI_example](../master-data-services/media/mds-conc-explicit-cap-ui-example.gif "mds_conc_explicit_cap_UI_example")  
  
 Posizionando la gerarchia esplicita al di sopra di una gerarchia derivata, la gerarchia derivata diventa incompleta.  
  
## <a name="rules"></a>Regole  
  
-   Non è possibile avere più di una gerarchia esplicita in una gerarchia derivata con estremità esplicita.  
  
-   È possibile utilizzare come estremità la stessa gerarchia esplicita per più gerarchie derivate.  
  
-   Non è possibile assegnare autorizzazioni membri gerarchia alle gerarchie derivate con estremità esplicite. Se si assegnano autorizzazioni individualmente alla gerarchia esplicita o alla gerarchia derivata, le autorizzazioni hanno effetto su entrambe le gerarchie.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare una gerarchia derivata.|[Creare una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|Creare una gerarchia esplicita.|[Creare una gerarchia esplicita &#40;Master Data Services&#41;](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|Eliminare una gerarchia derivata esistente.|[Eliminare una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/delete-a-derived-hierarchy-master-data-services.md)|  
|||  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Gerarchie derivate &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
-   [Gerarchie esplicite &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
  
