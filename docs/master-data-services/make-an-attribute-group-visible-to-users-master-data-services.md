---
description: Rendere visibile un gruppo di attributi per gli utenti (Master Data Services)
title: Rendere visibile un gruppo di attributi per gli utenti
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b2f6cc27-dbc9-4f3f-961e-e81e76375248
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c0e7c6f05ce075571a78e6b16abd8c3f40ccfd75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338200"
---
# <a name="make-an-attribute-group-visible-to-users-master-data-services"></a>Rendere visibile un gruppo di attributi per gli utenti (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]rendere visibile un gruppo di attributi a utenti o gruppi quando si vuole che gli utenti dispongano di schede sopra alla griglia nell'area funzionale **Visualizzatore** .  
  
 Quando si crea un gruppo di attributi, questo viene automaticamente nascosto a tutti gli utenti ad eccezione di quello che lo creato.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre di autorizzazione per accedere all'area funzionale **Amministrazione sistema** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   È necessario che sia presente almeno un gruppo di attributi. Per altre informazioni, vedere [Creare un gruppo di attributi &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md).  
  
### <a name="to-make-an-attribute-group-visible-to-users"></a>Per rendere visibile un gruppo di attributi agli utenti  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Gestisci modello** selezionare un modello dalla griglia e quindi fare clic su **entità**.  
  
3.  Dalla griglia nella pagina **Gestisci entità** selezionare la riga per l'entità di cui si vuole modificare il gruppo di attributi.  
  
4.  Fare clic su **Gruppi di attributi**.  
  
5.  Nella pagina **Gestisci gruppi di attributi** selezionare il tipo di membro nell'elenco a discesa **Tipi di membri** per espandere **Foglia**, **Consolidato** o **Raccolta**, a seconda del tipo di gruppo che si vuole rendere visibile.  
  
6.  Selezionare il gruppo di attributi che si vuole modificare nella griglia e quindi fare clic su **Modifica**.  
  
7.  Fare clic su un utente o gruppo nella casella **Disponibile** e quindi sulla freccia **Aggiungi** . Per aggiungere tutto, fare clic sulla freccia **Aggiungi tutto** .  
  
8.  Fare clic su **Salva**.  
  
## <a name="see-also"></a>Vedere anche  
 [Gruppi di attributi &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [Creare un gruppo di attributi &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)  
  
  
