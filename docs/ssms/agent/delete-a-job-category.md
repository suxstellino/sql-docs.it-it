---
description: Eliminare una categoria di processi
title: Eliminare una categoria di processi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, categories
- deleting job category
- jobs [SQL Server Agent], categories
- categories [SQL Server Agent jobs]
- removing job category
ms.assetid: 47a7640b-20b3-4639-ab37-b6fc73575e6c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 541b44d968d34e2a3c97ee645b5c7bee823c1415
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234157"
---
# <a name="delete-a-job-category"></a>Eliminare una categoria di processi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come eliminare una categoria di processi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
Le categorie consentono di organizzare i processi per semplificare le operazioni di raggruppamento e filtro. È ad esempio possibile organizzare tutti i processi di backup dei database raggruppandoli nella categoria Manutenzione database.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
Quando si elimina una categoria di processi definita dall'utente, tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene richiesto di riassegnare i processi appartenenti alla categoria a un'altra categoria. È possibile eliminare solo categorie di processi definite dall'utente.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-delete-a-job-category"></a>Per eliminare una categoria di processi  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera eliminare una categoria di processi.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Processi** e selezionare **Gestione categorie processi**.  
  
4.  Nella finestra di dialogo **Gestisci categorie di processi -** _nome\_server_ selezionare la categoria di processi da eliminare.  
  
5.  Fare clic su **Elimina**.  
  
6.  Nella finestra di dialogo **Categorie di processi** fare clic su **Sì**.  
  
7.  Chiudere la finestra di dialogo **Gestisci categorie di processi -** _nome\_server_.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-delete-a-job-category"></a>Per eliminare una categoria di processi  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- deletes the job category named AdminJobs.  
    USE msdb ;  
    GO   
    EXEC dbo.sp_delete_category  
        @name = N'AdminJobs',  
        @class = N'JOB' ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_delete_category (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per eliminare una categoria di processi**  
  
Chiamare la classe **JobCategory** con un linguaggio di programmazione a scelta, ad esempio Visual Basic, Visual C# o PowerShell.  
