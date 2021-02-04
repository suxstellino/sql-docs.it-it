---
description: Visualizza attività processi
title: Visualizza attività processi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- viewing job activity
- jobs [SQL Server Agent], viewing
- SQL Server Agent jobs, viewing
- displaying job activity
ms.assetid: 5c284e5e-7775-435d-ac49-f3f12a27ddc7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 35f6abadef986a164ad7ae3da03e4d8f15d1a4d1
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251289"
---
# <a name="view-job-activity"></a>Visualizza attività processi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene illustrato come visualizzare lo stato di runtime dei processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
All'avvio di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene creata una nuova sessione e la tabella **sysjobactivity** del database **msdb** viene popolata con tutti i processi definiti esistenti. In questa tabella sono registrati l'attività e lo stato dei processi correnti. Per visualizzare lo stato corrente dei processi è possibile utilizzare Monitoraggio attività processo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Se il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene interrotto in modo imprevisto, per verificare quali processi erano in esecuzione al momento dell'interruzione è possibile fare riferimento alla tabella **sysjobactivity** .  
  
## <a name="before-you-begin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-job-activity"></a>Per visualizzare l'attività del processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)], quindi espandere questa istanza.  
  
2.  Espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse su **Monitoraggio attività processi** e scegliere **Visualizza attività processi**.  
  
4.  In **Monitoraggio attività processo** è possibile visualizzare i dettagli relativi a ogni processo definito nel server.  
  
5.  Fare clic con il pulsante destro del mouse su un processo per avviarlo, arrestarlo, attivarlo o disabilitarlo, aggiornarne lo stato visualizzato in Monitoraggio attività processo, eliminarlo o visualizzarne la cronologia o le proprietà.  Per avviare, arrestare, attivare o disabilitare o aggiornare più processi, selezionare più righe in Monitoraggio attività processo e fare clic con il pulsante destro del mouse.  
  
6.  Per aggiornare Monitoraggio attività processo fare clic su **Aggiorna**. Per visualizzare un numero inferiore di righe, fare clic su **Filtro** e specificare i parametri del filtro.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-view-job-activity"></a>Per visualizzare l'attività del processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- lists activity for all jobs that the current user has permission to view.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_help_jobactivity ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_help_jobactivity (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-jobactivity-transact-sql.md).  
