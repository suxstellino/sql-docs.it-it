---
description: Registrare lo stato del processo nel registro applicazioni di Windows
title: Registrare lo stato del processo nel registro applicazioni di Windows
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- status information [SQL Server], jobs
- SQL Server Agent jobs, status
- writing job status to log
- jobs [SQL Server Agent], status
- logs [SQL Server], jobs
ms.assetid: 3b813702-8f61-40ec-bf3b-ce9deb7e68be
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: a09db511bc9ececa5b5929f12123fda60162b36f
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251281"
---
# <a name="write-the-job-status-to-the-windows-application-log"></a>Registrare lo stato del processo nel registro applicazioni di Windows

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come configurare [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] per registrare lo stato del processo nel log eventi dell'applicazione Windows tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
Tramite le risposte ai processi gli amministratori del database vengono informati in merito al completamento e alla frequenza di esecuzione dei processi. Le risposte ai processi tipiche includono:  
  
-   Notifica all'operatore tramite posta elettronica, trasmissione di messaggi su cercapersone o messaggi **Net Send** . Usare uno di questi metodi di risposta al processo se l'operatore dovrà eseguire operazioni basate sull'esito. Ad esempio, se un processo di backup viene completato, l'operatore dovrà ricevere una notifica per rimuovere il nastro di backup e riporlo in un luogo sicuro.  
  
-   Scrittura di un messaggio di evento nel registro delle applicazioni di Windows. Questa risposta può essere usata esclusivamente per i processi non riusciti.  
  
-   Eliminazione automatica del processo. Usare la risposta soltanto se si è certi che non sarà necessario rieseguire il processo.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-write-job-status-to-the-windows-application-log"></a>Per registrare lo stato del processo nel registro applicazioni di Windows  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.  
  
2.  Espandere **SQL Server Agent**, espandere **Processi**, fare clic con il pulsante destro del mouse sul processo da modificare e quindi scegliere **Proprietà**.  
  
3.  Scegliere la pagina **Notifiche** .  
  
4.  Selezionare l'opzione **Scrivi nel registro eventi delle applicazioni di Windows** e quindi scegliere una delle opzioni seguenti:  
  
    -   Fare clic su **In caso di esito positivo processo** per registrare lo stato del processo quando questo viene completato correttamente.  
  
    -   Fare clic su **In caso di esito negativo processo** per registrare lo stato del processo quando questo non viene completato correttamente.  
  
    -   Fare clic su **Al termine del processo** per registrare lo stato del processo indipendentemente dal suo completamento.  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per registrare lo stato del processo nel registro applicazioni di Windows**  
  
Chiamare la proprietà **EventLogLevel** della classe **Job** tramite un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell.  
  
Nell'esempio di codice seguente il processo viene impostato per generare una voce nel registro eventi del sistema operativo al termine dell'esecuzione del processo.  
  
**PowerShell**  
  
```  
$srv = new-object Microsoft.SqlServer.Management.Smo.Server("(local)")  
$jb = new-object Microsoft.SqlServer.Management.Smo.Agent.Job($srv.JobServer, "Test Job")  
$jb.EventLogLevel = [Microsoft.SqlServer.Management.Smo.Agent.CompletionAction]::Always  
```  
