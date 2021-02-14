---
title: Modello di recupero dei database
description: Informazioni su come abilitare un criterio per controllare il modello di recupero di backup per i database utente per ridurre la perdita di dati.
ms.custom: seo-lt-2019
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 8c68b60243ec033fff3735901cc4e35134222704
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345503"
---
# <a name="database-recovery-model"></a>Modello di recupero del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per SQL Server Enterprise Edition e Standard Edition, questa regola consente di controllare i database utente non di sola lettura per cui è impostato il modello di recupero con registrazione minima. Per i database di produzione, è consigliabile utilizzare il modello di recupero con registrazione completa anziché il modello di recupero con registrazione minima. La modalità di recupero con registrazione completa consente di eseguire il recupero temporizzato, che riduce la perdita di dati in caso di recupero di emergenza.
  
## <a name="best-practices-recommendations"></a>Raccomandazioni per le procedure consigliate  
 Per i database di produzione è consigliabile utilizzare la modalità di recupero con registrazione completa e di eseguire backup frequenti del log delle transazioni per garantire una recuperabilità con perdita di dati minima.
  
## <a name="see-also"></a>Vedere anche 
  
 [Panoramica del ripristino e del recupero](../backup-restore/restore-and-recovery-overview-sql-server.md)   
  
 [Ripristini di database completi (modello di recupero con registrazione completa)](../backup-restore/complete-database-restores-full-recovery-model.md)  

 [Backup di log delle transazioni](../backup-restore/transaction-log-backups-sql-server.md)   
  
