---
description: Assegnare un processo a una categoria di processi
title: Assegnare un processo a una categoria di processi
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], assigning
- SQL Server Agent jobs, categories
- jobs [SQL Server Agent], categories
- categories [SQL Server Agent jobs]
- SQL Server Agent jobs, assigning
- assigning job to category
ms.assetid: a9ea65a2-1d73-4582-a335-63adeb450cb6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 5c2ab0d3b985a619fbb31492d47aca88e932dbc7
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765519"
---
# <a name="assign-a-job-to-a-job-category"></a>Assegnare un processo a una categoria di processi

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come assegnare processi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent a categorie di processi in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
Le categorie consentono di organizzare i processi per semplificare le operazioni di raggruppamento e filtro. È ad esempio possibile organizzare tutti i processi di backup dei database raggruppandoli nella categoria Manutenzione database. È possibile assegnare processi a categorie predefinite, oppure creare una categoria definita dall'utente e usarla per l'assegnazione dei processi.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-assign-a-job-to-a-job-category"></a>Per assegnare un processo a una categoria di processi  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera assegnare un processo a una categoria di processi.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic sul segno più per espandere la cartella **Processi** .  
  
4.  Fare clic con il pulsante destro del mouse sul processo da modificare e selezionare **Proprietà**.  
  
5.  Nella finestra di dialogo **Proprietà processo-**_nome\_processo_ selezionare nell'elenco **Categoria** la categoria di processi alla quale assegnare il processo.  
  
6.  Fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-assign-a-job-to-a-job-category"></a>Per assegnare un processo a una categoria di processi  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```sql
    -- adding a new job category to the "NightlyBackups" job  
    USE msdb ;  
    GO  
    EXEC dbo.sp_update_job  
        @job_name = N'NightlyBackups',  
        @category_name = N'[Uncategorized (Local)]';  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per assegnare un processo a una categoria di processi**  
  
Usare la classe **JobCategory** con un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell.  
