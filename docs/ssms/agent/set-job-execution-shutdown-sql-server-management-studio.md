---
description: Impostare l'arresto dell'esecuzione del processo
title: Impostare l'arresto dell'esecuzione del processo
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], stopping
- SQL Server Agent jobs, stopping
- stopping jobs
- SQL Server Agent jobs, execution shutdowns
ms.assetid: ac23e88f-53fc-41de-bb16-0c27c002d5a5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 6b94cc2703613e53703b9531f574bc5e925a1c55
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474392"
---
# <a name="set-job-execution-shutdown"></a>Impostare l'arresto dell'esecuzione del processo

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come impostare il periodo di attesa di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent prima di terminare l'esecuzione dei processi, trascorso il quale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent stesso viene interrotto in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per impostazione predefinita, i membri del ruolo predefinito del server **sysadmin** possono impostare il periodo di attesa di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent prima di terminare l'esecuzione dei processi, trascorso il quale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent stesso viene interrotto. Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-set-job-execution-shutdown"></a>Per impostare l'arresto dell'esecuzione del processo  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera impostare un intervallo per l'arresto dell'esecuzione del processo.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent** e scegliere **Proprietà**.  
  
3.  In **Selezione pagina** selezionare **Sistema processi**.  
  
4.  Impostare l'opzione **Intervallo di time-out chiusura** (in secondi) per aumentare o ridurre l'intervallo di time-out di chiusura. Questo valore determina l'intervallo di attesa del completamento dell'esecuzione dei processi da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, trascorso il quale viene interrotto anche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
