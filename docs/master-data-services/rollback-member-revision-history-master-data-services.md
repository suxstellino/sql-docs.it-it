---
description: Rollback della cronologia delle revisioni del membro (Master Data Services)
title: Eseguire il rollback della cronologia delle revisioni del membro
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d39d3474-20e7-429f-9c8d-fcc4eb0854fc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec99fc4d464037b4051da46e293efb4b2fb6969a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355279"
---
# <a name="rollback-member-revision-history-master-data-services"></a>Rollback della cronologia delle revisioni del membro (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Una cronologia delle revisioni del membro viene registrata ogni volta che un membro viene modificato. È possibile eseguire il rollback di una cronologia delle revisioni del membro a una versione precedente.  
  
## <a name="prerequisites"></a>Prerequisiti  
  
-   È necessario avere l'autorizzazione per aggiornare almeno uno degli attributi del membro selezionato. Quando si esegue il rollback di una cronologia delle revisioni, verrà eseguito il rollback di tutti i valori di attributo che possono essere aggiornati ai valori delle versioni precedenti.  
  
-   La cronologia delle revisioni è disponibile solo quando il tipo di log delle transazioni dell'entità è membro.  
  
 **Per eseguire il rollback della cronologia delle revisioni del membro**  
  
1.  In Gestione dati master, fare clic su Esplora.  
  
2.  Scegliere l'entità e il membro per eseguire il rollback.  
  
3.  Fare clic su **Visualizza cronologia**.  
  
4.  Scegliere la revisione di cui eseguire il rollback e quindi fare clic su **Rollback**.  
  
## <a name="see-also"></a>Vedere anche  
 [Cronologia revisioni membri &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md)   
 [Modificare il tipo di log delle transazioni dell'entità &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
  
