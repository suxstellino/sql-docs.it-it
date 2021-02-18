---
description: Avviare un processo
title: Avviare un processo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], starting
- SQL Server Agent jobs, starting
- starting jobs
ms.assetid: cec9f7f7-d0a7-4239-9dc5-a69c011ebaa0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 63c2b0cff0d18b1aae8c14708100d519f0a02b86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344234"
---
# <a name="start-a-job"></a>Avviare un processo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come avviare l'esecuzione di un processo di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
Un processo è una serie specificata di azioni eseguite da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent possono essere eseguiti in un server locale o in più server remoti.  
  
-   **Prima di iniziare:**  
  
    [Sicurezza](#Security)  
  
-   **Per avviare un processo utilizzando:**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-start-a-job"></a>Per avviare un processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.  
  
2.  Espandere **SQL Server Agent** e quindi **Processi**. In base alla modalità di avvio del processo desiderata, eseguire una delle seguenti operazioni:  
  
    -   Se si usa un unico server oppure un server di destinazione, oppure se si esegue un processo del server locale in un server master, fare clic con il pulsante destro del mouse sul processo da avviare e scegliere **Avvia processo**.  
  
    -   Se si vuole avviare più processi, fare clic con il pulsante destro del mouse su **Monitoraggio attività processi** e scegliere **Visualizza attività processi**. In Monitoraggio attività processi è possibile selezionare più processi; fare clic con il pulsante destro del mouse sui processi selezionati e scegliere **Avvia processi**.  
  
    -   Se si usa un server master e si vuole eseguire il processo contemporaneamente in tutti i server di destinazione, fare clic con il pulsante destro del mouse sul processo da avviare, scegliere **Avvia processo** e fare clic su **Avvia su tutti i server di destinazione**.  
  
    -   Se si usa un server master e si vogliono specificare i server di destinazione in cui eseguire il processo, fare clic con il pulsante destro del mouse sul processo da avviare, scegliere **Avvia processo** e fare clic su **Avvia sui server di destinazione specificati**. Nella finestra di dialogo **Invia istruzioni di download** selezionare la casella di controllo **Solo i server di destinazione seguenti** e quindi selezionare i server di destinazione in cui si desidera eseguire il processo.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-start-a-job"></a>Per avviare un processo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- starts a job named Weekly Sales Data Backup.    
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_start_job N'Weekly Sales Data Backup' ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_start_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per avviare un processo**  
  
Chiamare il metodo **Start** della classe **Job** tramite un linguaggio di programmazione a scelta, ad esempio Visual Basic, Visual C# o PowerShell. Per altre informazioni, vedere [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
