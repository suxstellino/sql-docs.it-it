---
description: Attività Esegui processo di SQL Server Agent
title: Attività Esegui processo di SQL Server Agent
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executesqlserveragentjobtask.f1
helpviewer_keywords:
- Execute SQL Server Agent Job task [Integration Services]
- jobs [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 3aa3bc0e-1a1c-452e-81b8-b4e3422ea053
author: chugugrace
ms.author: chugu
ms.custom: seo-lt-2019
ms.openlocfilehash: f11a70a1118fcfc2b6393d8b204ac7b1b8250d0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352075"
---
# <a name="execute-sql-server-agent-job-task"></a>Attività Esegui processo di SQL Server Agent

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Esegui processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent consente di eseguire processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è un servizio di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows per l'esecuzione di processi definiti in un'istanza di SQL Server. È possibile creare processi che eseguono istruzioni Transact-SQL e script ActiveX, attività di manutenzione della replica e di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] oppure pacchetti. È anche possibile configurare un processo per il monitoraggio di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e la generazione di avvisi. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] I processi di Agent vengono generalmente usati per rendere automatiche le attività più ripetitive. Per altre informazioni, vedere [Implementazione di processi](../../ssms/agent/implement-jobs.md).  
  
 Tramite l'attività Esegui processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, un pacchetto può eseguire attività amministrative correlate ai componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent può ad esempio eseguire una stored procedure di sistema quale **sp_enum_dtspackages** per ottenere l'elenco dei pacchetti di una cartella.  
  
> [!NOTE]  
>  È possibile eseguire automaticamente processi amministrativi locali o multiserver solo se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è in esecuzione.  
  
 Questa attività incapsula la procedura di sistema **sp_start_job** e passa il nome del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent alla procedura come argomento. Per altre informazioni, vedere [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).  
  
## <a name="configuring-the-execute-sql-server-agent-job-task"></a>Configurazione dell'attività Esegui processo di SQL Server Agent  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Questa attività è disponibile nella sezione **Attività di manutenzione** della **casella degli strumenti** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)].  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Attività Esegui processo di SQL Server Agent &#40;Piano di manutenzione&#41;](../../relational-databases/maintenance-plans/execute-sql-server-agent-job-task-maintenance-plan.md)  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
