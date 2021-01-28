---
description: Cancellare il contenuto del log di cronologia processi
title: Cancellare il contenuto del log di cronologia processi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- clearing job history log
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: 34b9398a-c409-4040-8ea1-0deceb18f961
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9d48a3ea6b2660504b613b402f024c8f42ae7557
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766299"
---
# <a name="clear-the-job-history-log"></a>Cancellare il contenuto del log di cronologia processi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come eliminare il contenuto del log della cronologia processo di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-clear-the-job-history-log"></a>Per cancellare il contenuto del log di cronologia processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.  
  
2.  Espandere **SQL Server Agent** e quindi espandere **Processi**.  
  
3.  Fare clic con il pulsante destro del mouse su un processo e scegliere **Visualizza cronologia**.  
  
4.  Nel **Visualizzatore file di log** selezionare il processo di cui si desidera cancellare la cronologia e quindi eseguire una delle operazioni seguenti:  
  
    -   Fare clic su **Elimina** e quindi su **Elimina tutta la cronologia** nella finestra di dialogo **Elimina cronologia** . È possibile eliminare tutta la cronologia processo oppure solo quella precedente a una data specificata. Per rimuovere tutta la cronologia processo, fare clic su **Elimina tutta la cronologia**. Per rimuovere solo i log cronologia processo più vecchi, fare clic su **Elimina la cronologia precedente a** e quindi specificare una data.  
  
    -   Fare clic su **Stato processo** se si desidera cancellare il contenuto del log della cronologia di un processo multiserver. Fare clic su **Processo**, selezionare il nome di un processo e quindi fare clic su **Visualizza cronologia processi remoti**.  
  
5.  Fare clic su **Elimina**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-clear-the-job-history-log"></a>Per cancellare il contenuto del log di cronologia processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- example removes the history for a job named NightlyBackups.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_purge_jobhistory  
        @job_name = N'NightlyBackups' ;  
    GO  
    ```  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per cancellare il contenuto del log di cronologia processo**  
  
Usare il metodo **PurgeJobHistory** della classe **JobServer** tramite un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell. Per altre informazioni, vedere [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
