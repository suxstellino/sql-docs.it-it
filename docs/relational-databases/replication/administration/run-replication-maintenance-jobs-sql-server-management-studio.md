---
title: Eseguire processi di manutenzione della replica (SSMS)
description: Informazioni su come avviare e arrestare i processi di manutenzione della replica in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server replication]
ms.assetid: 0dc485a0-5a50-41eb-a29d-f2b2fb920174
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f1ec276bbb288adf069e6f4913a648395a932d1d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467462"
---
# <a name="run-replication-maintenance-jobs-sql-server-management-studio"></a>Esecuzione di processi di manutenzione della replica (SQL Server Management Studio)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Nella replica vengono utilizzati i seguenti processi di manutenzione:  
  
-   **Reinizializzazione delle sottoscrizioni con errori di convalida dei dati**  
  
-   **Eliminazione del contenuto della cronologia dell'agente: distribuzione**  
  
-   **Aggiornamento del monitoraggio della replica per la distribuzione.**  
  
-   **Controllo degli agenti di replica**  
  
-   **Eliminazione del contenuto della distribuzione: distribuzione**  
  
-   **Pulizia dei riferimenti alla sottoscrizione scaduta**  
  
 Avviare e arrestare questi processi dalla cartella **Processi** in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] e dalla scheda **Agenti** in Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md). Visualizzare e modificare le proprietà per ogni processo nella finestra di dialogo **Proprietà processo - \<Job>** , accessibile dalla stessa cartella e dalla stessa scheda.  
  
### <a name="to-start-or-stop-a-replication-maintenance-job-in-management-studio"></a>Per avviare e arrestare un processo di manutenzione della replica in Management Studio  
  
1.  Connettersi al database di distribuzione in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], quindi espandere il nodo del server.  
  
2.  Espandere la cartella **SQL Server Agent** e quindi la cartella **Processi** .  
  
3.  Fare clic con il pulsante destro del mouse su un processo e quindi scegliere **Avvia processo** o **Arresta processo**.  
  
### <a name="to-start-or-stop-a-replication-maintenance-job-in-replication-monitor"></a>Per avviare e arrestare un processo di manutenzione della replica in Monitoraggio replica  
  
1.  Espandere un gruppo di server di pubblicazione in Monitoraggio replica e quindi selezionare un server di pubblicazione.  
  
2.  Fare clic sulla scheda **Agenti** .  
  
3.  Fare clic con il pulsante destro del mouse su un processo e quindi scegliere **Avvia processo** o **Arresta processo**.  
  
### <a name="to-view-and-modify-properties-for-a-replication-maintenance-job-in-management-studio"></a>Per visualizzare e modificare le proprietà per un processo di manutenzione della replica in Management Studio  
  
1.  Connettersi al database di distribuzione in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], quindi espandere il nodo del server.  
  
2.  Espandere la cartella **SQL Server Agent** e quindi la cartella **Processi** .  
  
3.  Fare clic con il pulsante destro del mouse su un processo e scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **Proprietà processo - \<Job>** modificare le proprietà, se necessario, e quindi fare clic su **OK**.  
  
### <a name="to-view-and-modify-properties-for-a-replication-maintenance-job-in-replication-monitor"></a>Per visualizzare e modificare le proprietà per un processo di manutenzione della replica in Monitoraggio replica  
  
1.  Espandere un gruppo di server di pubblicazione in Monitoraggio replica e quindi selezionare un server di pubblicazione.  
  
2.  Fare clic sulla scheda **Agenti** .  
  
3.  Fare clic con il pulsante destro del mouse su un processo nella griglia e quindi scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **Proprietà processo - \<Job>** modificare le proprietà, se necessario, e quindi fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
 [Avviare e arrestare un agente di replica &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Visualizzare le informazioni ed eseguire attività relative a un server di pubblicazione &#40;Monitoraggio replica&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Amministrazione dell'agente di replica](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
