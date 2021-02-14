---
description: Riattivare un membro o una raccolta (Master Data Services)
title: Riattivare un membro o una raccolta
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services], reactivating
- consolidated members [Master Data Services], reactivating
- reactivating members [Master Data Services]
- members [Master Data Services], reactivating
- reactivating collections [Master Data Services]
- leaf members [Master Data Services], reactivating
ms.assetid: bb4884c0-3658-4763-92d1-636804278b1c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8c17875c71e9ab7a69cdc9a5a535708e3792ae8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345604"
---
# <a name="reactivate-a-member-or-collection-master-data-services"></a>Riattivare un membro o una raccolta (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]è possibile riattivare un membro che è stato:  
  
-   Disattivato dal processo di gestione temporanea.  
  
-   Eliminato in [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]MDS.  
  
-   Eliminato nell'applicazione Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
 Quando si riattiva un membro, i relativi attributi e l'appartenenza a gerarchie e raccolte vengono ripristinati.  
  
 È inoltre possibile riattivare le raccolte. Quando si esegue questa operazione, vengono ripristinati gli attributi della raccolta e i membri che appartengono ad essa.  
  
 Quando si riattiva una raccolta o un membro, vengono ripristinate tutte le transazioni precedenti.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]è necessario disporre dell'autorizzazione per l'area funzionale **Gestione versioni** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-reactivate-a-member-or-collection"></a>Per riattivare un membro o una raccolta  
  
1.  Scegliere [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Gestione versioni **dalla home page di**.  
  
2.  Sulla barra dei menu fare clic su **Transazioni**.  
  
3.  Nella pagina **Transazioni** selezionare un modello dall'elenco **Modello** .  
  
4.  Selezionare una versione dall'elenco **Versione** .  
  
5.  Nel riquadro **Transazioni** fare clic sulla riga del membro o della raccolta che si vuole riattivare. Per questa riga deve essere visualizzata l'indicazione **Attivo** nella colonna **Valore precedente** e l'indicazione **Disattivato** nella colonna **Nuovo valore** .  
  
6.  Fare clic su **Inverti transazione selezionata**.  
  
7.  Nella finestra di dialogo di conferma fare clic su **OK**. Viene aggiunta una nuova transazione con l'indicazione **Attivo** nella colonna **Nuovo valore** .  
  
## <a name="see-also"></a>Vedere anche  
 [Eliminare un membro o una raccolta &#40;Master Data Services&#41;](../master-data-services/delete-a-member-or-collection-master-data-services.md)   
 [Membri &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Raccolte &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
  
