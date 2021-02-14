---
description: Approvazione necessaria (Master Data Services)
title: Approvazione necessaria
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b475a53d-269d-49f3-bb42-965c555f80be
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 25de1b29a2d19457d6c74a02ec1937bcd654c023
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339580"
---
# <a name="approval-required-master-data-services"></a>Approvazione necessaria (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]l'amministratore può impostare un'entità su Approvazione necessaria. Tutte le modifiche all'entità richiedono l'esame e l'approvazione delle modifiche da parte di uno degli amministratori dell'entità.  
  
> [!NOTE]  
>  Le modifiche apportate ai membri foglia richiedono l'approvazione. Le modifiche apportate alle raccolte e alle gerarchie esplicite deprecate non richiedono l'approvazione.  
>   
>  Le modifiche apportate al processo della tabella di staging non richiedono l'approvazione.  
>   
>  Le modifiche apportate da una regola business non richiedono l'approvazione.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessaria l'autorizzazione per accedere all'area funzionale Amministrazione sistema  
  
-   È necessario essere un amministratore del modello. Per altre informazioni, vedere [Amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)  
  
-   Deve essere presente un'entità. Per altre informazioni, vedere [creare un'entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)  
  
## <a name="to-enable-approval-required-for-an-entity"></a>Per abilitare Approvazione necessaria per un'entità  
  
1.  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], fare clic su **Amministrazione sistema**.  
  
2.  Nella pagina **Gestisci modello** selezionare un modello dalla griglia e quindi fare clic su **entità**.  
  
3.  Dalla griglia della pagina **Gestisci entità** selezionare la riga per l'entità per cui si vuole abilitare  **Approvazione necessaria** .  
  
4.  Fare clic su **Modifica**, selezionare **Approvazione necessaria**, quindi scegliere **Salva**.  
  
## <a name="see-also"></a>Vedere anche  
 [Insiemi di modifiche &#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md)  
  
  
