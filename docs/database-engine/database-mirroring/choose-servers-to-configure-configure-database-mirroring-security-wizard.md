---
title: 'Configurazione guidata sicurezza: Selezionare i server'
description: Descrive le proprietà disponibili nella pagina 'Selezione dei server' della 'Configurazione guidata sicurezza mirroring del database'.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.choosesrvrs.f1
ms.assetid: 59e23ff3-d7ee-4e32-9629-0b54d3a258f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c2561f81c55ca387f8038ca8ab10f82ea6fce704
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350717"
---
# <a name="configure-database-mirroring-wizard-choose-servers-to-configure"></a>Configurazione guidata mirroring del database: Selezione dei server da configurare 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa pagina per specificare le istanze del server che si desidera configurare. Prima di proseguire con la procedura guidata è necessario selezionare almeno un'istanza del server.  
  
 Se si deseleziona la casella di controllo corrispondente a un'istanza del server, la procedura guidata non apporterà alcuna modifica a tale istanza. Verrà chiesto tuttavia di immettere informazioni sull'istanza, che verranno salvate come parte della configurazione delle altre istanze del server. Se ad esempio si deseleziona la casella di controllo corrispondente all'istanza del server di controllo del mirroring, la procedura guidata chiederà di immettere l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del server di controllo del mirroring, perché è necessario creare tale account di accesso come parte della configurazione di sicurezza salvata nelle istanze del server principale e del server mirror.  
  
 **Per configurare il mirroring del database tramite SQL Server Management Studio**  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Opzioni  
 **Istanza server principale**  
 Consente di configurare la sicurezza per il server principale.  
  
 **Istanza server mirror**  
 Consente di configurare la sicurezza per il server mirror.  
  
 **Istanza server di controllo del mirroring**  
 Consente di configurare la sicurezza per il server di controllo del mirroring, se presente.  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà del database &#40;Pagina Mirroring&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
