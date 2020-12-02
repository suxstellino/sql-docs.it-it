---
description: Piano di manutenzione (Server)
title: Piano di manutenzione (Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.servers.f1
- sql13.swb.maint.maintplanproperties.server.f1
ms.assetid: ac24d1a8-dd2f-4162-b804-c0df1fc1e61d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1a991404f0acaba448958773f54a4836032ecd07
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88420855"
---
# <a name="maintenance-plan-servers"></a>Piano di manutenzione (Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Usare la finestra di dialogo **Server** per selezionare i server in cui si vuole eseguire il piano di manutenzione.  
  
 Per creare un piano di manutenzione multiserver, è necessario configurare un ambiente multiserver composto da un server master e uno o più server di destinazione. Per i piani di manutenzione multiserver, il server locale deve essere configurato come server master. Negli ambienti multiserver, in questa finestra di dialogo vengono visualizzati il server master **(local)** e tutti i server di destinazione corrispondenti. Viene creato un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per il server locale. Tale processo sarà abilitato o disabilitato a seconda che si selezioni o meno il server **(local)** . Se vengono selezionati server di destinazione, viene creato un processo multiserver e tale processo viene poi scaricato in ognuno dei server di destinazione selezionati. Se non viene selezionato alcun server di destinazione, il processo multiserver viene eliminato.  
  
## <a name="see-also"></a>Vedere anche  
 [Piani di manutenzione](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Creazione di un ambiente multiserver](../../ssms/agent/create-a-multiserver-environment.md)   
 [Configurare un server master](../../ssms/agent/make-a-master-server.md)   
 [Configurare un server di destinazione](../../ssms/agent/make-a-target-server.md)  
  
  
