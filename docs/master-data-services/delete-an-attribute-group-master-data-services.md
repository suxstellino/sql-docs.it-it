---
description: Eliminare un gruppo di attributi (Master Data Services)
title: Eliminare un gruppo di attributi
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting attribute groups [Master Data Services]
- attribute groups [Master Data Services], deleting
ms.assetid: f915e89b-629d-4725-aea6-a7f051978244
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c1331d00c0c39cbe3a82f4372aaed38a315dc3c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338987"
---
# <a name="delete-an-attribute-group-master-data-services"></a>Eliminare un gruppo di attributi (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]è possibile eliminare un gruppo di attributi quando non è più necessario visualizzare la scheda nell'area funzionale **Visualizzatore** di [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   **Nota** Se sono presenti gruppi di attributi, gli attributi che non appartengono a un gruppo non saranno visualizzati nel **Visualizzatore**. Quando non sono presenti gruppi di attributi, vengono visualizzati tutti gli attributi.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-an-attribute-group"></a>Per eliminare un gruppo di attributi  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Gestisci modello** selezionare un modello dalla griglia e quindi fare clic su **entità**.  
  
3.  Dalla griglia nella pagina **Gestisci entità** selezionare la riga per l'entità per cui si vuole modificare un gruppo di attributi.  
  
4.  Fare clic su **Gruppi di attributi**.  
  
5.  Nella pagina **Gestisci gruppi di attributi** selezionare il tipo di membro nell'elenco a discesa **Tipi di membri** per espandere **Foglia**, **Consolidato** o **Raccolta**, a seconda del tipo di gruppo che si vuole eliminare.  
  
6.  Fare clic sul gruppo di attributi che si desidera eliminare.  
  
7.  Fare clic su **Elimina**.  
  
8.  Nella finestra di dialogo di conferma fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
 [Gruppi di attributi &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [Creare un gruppo di attributi &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)  
  
  
