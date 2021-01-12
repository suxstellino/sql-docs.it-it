---
title: Opzione di configurazione del server Agent XPs | Microsoft Docs
description: Scoprire come usare l'opzione Agent XPs per abilitare le stored procedure estese di SQL Server Agent. Visualizzare un esempio in cui viene usata questa opzione.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Agent XPs option
- extended stored procedures [SQL Server], SQL Server Agent
ms.assetid: 2e1c6c64-5ce7-4357-98c7-ac7763a9f9de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83182354415b7dd442acee73e91ca7e11c20ce74
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091750"
---
# <a name="agent-xps-server-configuration-option"></a>Opzione di configurazione del server Agent XPs
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  L'opzione **Agent XPs** consente di abilitare le stored procedure estese del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent nel server. Se questa opzione non è attivata, il nodo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non sarà disponibile in Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
 Quando si utilizza lo strumento [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per avviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, le stored procedure estese vengono abilitate automaticamente. Per ulteriori informazioni, vedere [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] In Esplora oggetti il nodo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Agent non viene visualizzato a meno che le stored procedure estese non vengano abilitate indipendentemente dallo stato del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 I valori possibili sono:  
  
-   **0**, che indica che le stored procedure estese di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non sono disponibili e corrisponde al valore predefinito.  
  
-   **1**, che indica che le stored procedure estese di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sono disponibili.  
  
 L'impostazione diventa effettiva immediatamente e non richiede l'arresto e il riavvio del server.  
  
## <a name="example"></a>Esempio
 Nell'esempio seguente viene illustrata l'attivazione delle stored procedure estese di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  

1. Connettersi al motore di database da Microsoft SQL Server Management Studio.

2.  Dalla barra Standard fare clic su **Nuova query**.

3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. 
  
```sql 
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'Agent XPs', 1;  
GO  
RECONFIGURE  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Automatizzazione delle attività amministrative &#40;SQL Server Agent&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)   
 [Avvio, arresto o sospensione del servizio SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
