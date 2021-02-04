---
description: Modificare un processo
title: Modificare un processo
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], modifying
- modifying jobs
- SQL Server Agent jobs, modifying
ms.assetid: dd5e5f20-20c4-4ab9-a19a-db87577dcd43
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 3f4a1a656831836b4ef5276197c896e664d1f7bd
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251365"
---
# <a name="modify-a-job"></a>Modificare un processo

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come modificare le proprietà dei processi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
Un processo master di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non può essere destinato sia a server locali sia a server remoti.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
È possibile modificare solo i processi di cui si è proprietari, a meno che non si appartenga al ruolo predefinito del server **sysadmin** . Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-modify-a-job"></a>Per modificare un processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.  
  
2.  Espandere il nodo **SQL Server Agent** e il nodo **Processi**; fare clic con il pulsante destro del mouse sul processo che si vuole modificare e scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà processo** aggiornare le proprietà, i passaggi, la pianificazione e le notifiche del processo nelle schede corrispondenti.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-modify-a-job"></a>Per modificare un processo  
  
1.  In Esplora oggetti connettersi a un'istanza del motore di database ed espanderla.  
  
2.  Sulla barra degli strumenti fare clic su **Nuova query**.  
  
3.  Nella finestra Query utilizzare le stored procedure di sistema seguenti per modificare un processo.  
  
    -   Eseguire [sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md) per modificare gli attributi di un processo.  
  
    -   Eseguire [sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md) per modificare i dettagli della pianificazione per la definizione di un processo.  
  
    -   Eseguire [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) per aggiungere nuovi passaggi al processo.  
  
    -   Eseguire [sp_update_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md) per modificare i passaggi esistenti del processo.  
  
    -   Eseguire [sp_delete_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md) per rimuovere un passaggio di processo da un processo.  
  
    -   Stored procedure aggiuntive per modificare qualsiasi processo master di SQL Server Agent:  
  
        -   Eseguire [sp_delete_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md) per eliminare un server attualmente associato a un processo.  
  
        -   Eseguire [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) per associare un server al processo corrente.  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per modificare un processo**  
  
Usare la classe **Job** tramite un linguaggio di programmazione a scelta, ad esempio Visual Basic, Visual C# o PowerShell. Per altre informazioni, vedere [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
