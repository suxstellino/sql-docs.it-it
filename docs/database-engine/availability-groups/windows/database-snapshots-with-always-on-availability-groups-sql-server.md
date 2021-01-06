---
title: Creare uno snapshot del database per un gruppo di disponibilità
description: Procedura per la creazione di uno snapshot per un database incluso in un gruppo di disponibilità Always On, che può essere un database primario o secondario.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database snapshots [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: 7432da1c-ce2f-4cd9-af41-54c97744166b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1550f73cf69a4e5011e244def5451539db388b06
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643221"
---
# <a name="database-snapshots-with-always-on-availability-groups-sql-server"></a>Snapshot del database con gruppi di disponibilità Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  È possibile creare uno snapshot del database in un qualsiasi database primario o secondario in un gruppo di disponibilità. Il ruolo di replica deve essere PRIMARY o SECONDARY, non nello stato di RESOLVING.  
  
 È consigliabile uno stato di sincronizzazione del database corrispondente a SYNCHRONIZING o SYNCHRONIZED quando si crea uno snapshot del database. È tuttavia possibile creare gli snapshot del database anche se lo stato della sincronizzazione del database è NOT SYNCHRONIZING.  
  
 Uno snapshot del database in una replica secondaria deve continuare a funzionare anche se la replica è disconnessa (DISCONNECTED) dalla replica primaria.  
  
 Alcune condizioni di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] causano il riavvio del database di origine e degli snapshot del database, disconnettendo temporaneamente gli utenti. Tali condizioni sono descritte di seguito:  
  
-   La replica primaria cambia i ruoli, perché la replica primaria corrente viene portata offline e poi online sulla stessa istanza del server o perché si verifica il failover del gruppo di disponibilità.  
  
-   Il database passa al ruolo secondario.  
  
 Se si verifica il failover della replica di disponibilità che ospita gli snapshot del database, gli snapshot del database rimangono sull'istanza del server dove sono stati creati. Gli utenti possono continuare a utilizzare gli snapshot dopo il failover. Se le prestazioni sono una preoccupazione, è consigliabile creare snapshot del database solo sui database secondari ospitati da una replica secondaria configurata per la modalità failover manuale.  Se si esegue il failover manuale del gruppo di disponibilità in questa replica secondaria, è possibile creare un nuovo set di snapshot del database su un'altra replica secondaria, reindirizzare i client ai nuovi snapshot del database ed eliminare tutti gli snapshot del database dai nuovi database primari.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Snapshot del database &#40;SQL Server&#41;](../../../relational-databases/databases/database-snapshots-sql-server.md)  
  
  
