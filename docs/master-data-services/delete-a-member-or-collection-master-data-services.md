---
description: Eliminare un membro o una raccolta (Master Data Services)
title: Eliminare un membro o una raccolta
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services], deleting
- leaf members [Master Data Services], deleting
- deleting members [Master Data Services]
- members [Master Data Services], deleting
- consolidated members [Master Data Services], deleting
ms.assetid: 519130a7-4226-4d71-9124-d2ee0ce7e5bd
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b8810bb155896b4935163bb1143d535faddb1dad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339004"
---
# <a name="delete-a-member-or-collection-master-data-services"></a>Eliminare un membro o una raccolta (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]eliminare un membro o una raccolta quando non è più necessaria. Per eliminare i membri in blocco, usare invece le tabelle di staging. Per altre informazioni, vedere [importare dati da tabelle &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md)  
  
> [!NOTE]  
>  Non è possibile eliminare un membro se viene utilizzato come valore dell'attributo basato su dominio per un altro membro.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre dell'autorizzazione per accedere all'area funzionale **Esplora** .  
  
-   Per i membri, è necessario avere almeno l'autorizzazione **Eliminazione** per l'oggetto modello foglia dal quale eliminare un membro.  
  
-   Per le raccolte, è necessario disporre almeno dell'autorizzazione **Aggiornamento** per l'oggetto raccolta foglia in fase di eliminazione.  
  
### <a name="to-delete-a-member-or-collection"></a>Per eliminare un membro o una raccolta  
  
1.  Nella home page di [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] selezionare un modello nell'elenco **Modello** .  
  
2.  Selezionare una versione dall'elenco **Versione** .  
  
3.  Fare clic su **Esplora**.  
  
4.  Per eliminare:  
  
    -   Un membro foglia, scegliere **Entità** dalla barra dei menu, quindi fare clic sul nome dell'entità che contiene il membro.  
  
    -   Un membro consolidato, scegliere **Gerarchie** dalla barra dei menu, quindi fare clic sul nome della gerarchia che contiene il membro. Quindi fare clic sul nodo nella gerarchia che contiene il membro.  
  
    -   Una raccolta, scegliere **Raccolte** dalla barra dei menu, quindi fare clic sul nome dell'entità che contiene la raccolta.  
  
5.  Nella griglia fare clic sulla riga del membro o della raccolta che si desidera eliminare.  
  
6.  Fare clic su **Elimina membro**, **Elimina** o **Elimina raccolta**.  
  
7.  Gli amministratori di entità vedranno anche un'opzione di menu per ripulire, ovvero eliminare definitivamente, tutti i membri eliminati temporaneamente nell'entità.  
  
8.  Nella finestra di dialogo di conferma fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
 [Riattivare un membro o una raccolta &#40;Master Data Services&#41;](../master-data-services/reactivate-a-member-or-collection-master-data-services.md)   
 [Membri &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Raccolte &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
  
