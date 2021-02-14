---
description: Creare un gruppo di attributi (Master Data Services)
title: Creare un gruppo di attributi
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attribute groups [Master Data Services], creating
- creating attribute groups [Master Data Services]
ms.assetid: 798c325e-e8d8-412a-b02e-118f2741d1c7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3d2e129bdd1a995fb5d0092bc15136ca9e1726f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348088"
---
# <a name="create-an-attribute-group-master-data-services"></a>Creare un gruppo di attributi (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]creare gruppi di attributi quando si desidera che gli attributi vengano visualizzati in schede singole nella griglia **Esplora** .  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   È necessario che esista almeno un attributo. Per altre informazioni, vedere [Creare un attributo di testo &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md).  
  
### <a name="to-create-an-attribute-group"></a>Per creare un gruppo di attributi  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Gestisci modello** selezionare un modello dalla griglia e quindi fare clic su **entità**.  
  
3.  Dalla griglia della pagina **Gestisci entità** selezionare la riga per l'entità per cui si vuole creare un gruppo di attributi.  
  
4.  Fare clic su **Gruppi di attributi**.  
  
5.  Nella pagina Gestisci gruppi di attributi eseguire una di queste operazioni, quindi fare clic su **Aggiungi**.  
  
     Se il gruppo di attributi è per i membri foglia, selezionare **Foglia** dall'elenco a discesa **Tipo di membro** nella parte superiore della pagina.  
  
     Se il gruppo di attributi è per i membri consolidati, selezionare **Consolidato** dall'elenco a discesa **Tipo di membro** .  
  
     Se il gruppo di attributi è per le raccolte, selezionare **Raccolta** dall'elenco a discesa **Tipo di membro** .  
  
6.  Fare clic su **Gruppi di foglie**, **Gruppi consolidati** o **Gruppi di raccolte** per creare rispettivamente un gruppo di attributi per membri foglia, membri consolidati o raccolte.  
  
7.  Nella casella **Nome** digitare un nome per il gruppo di attributi. Si tratta del nome visualizzato nella scheda in **Visualizzatore**.  
  
8.  Per aggiungere un attributo, fare clic su di esso nella casella **Attributi disponibili** , quindi fare clic sulla freccia **Aggiungi** . Per aggiungere tutti gli attributi, fare clic sulla freccia **Aggiungi tutto** .  
  
9. Fare clic sulle frecce **Su** e **Giù** per modificare l'ordine degli attributi da sinistra a destra.  
  
10. Fare clic sugli utenti nella casella **Utenti disponibili** , quindi sulla freccia **Aggiungi** . Per aggiungere tutti gli utenti, fare clic sulla freccia **Aggiungi tutto** .  
  
11. Fare clic sui gruppi nella casella **Gruppi disponibili** , quindi sulla freccia **Aggiungi** . Per aggiungere tutti i gruppi, fare clic sulla freccia **Aggiungi tutto** .  
  
12. Fare clic su **Salva**.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   [Rendere visibile un gruppo di attributi per gli utenti &#40;Master Data Services&#41;](../master-data-services/make-an-attribute-group-visible-to-users-master-data-services.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Gruppi di attributi &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [Attributi &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [Modificare il nome di un gruppo di attributi &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-group-name-master-data-services.md)   
 [Eliminare un gruppo di attributi &#40;Master Data Services&#41;](../master-data-services/delete-an-attribute-group-master-data-services.md)   
 [Autorizzazioni per elementi foglia &#40;Master Data Services&#41;](../master-data-services/leaf-permissions-master-data-services.md)   
   
  
  
