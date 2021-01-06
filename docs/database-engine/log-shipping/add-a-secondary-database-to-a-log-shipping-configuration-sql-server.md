---
title: Aggiungere un database secondario per il log shipping
description: Viene descritto come aggiungere un database secondario a una configurazione per il log shipping esistente usando SQL Server Management Studio o Transact-SQL in SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- adding secondary databases
- secondary databases [SQL Server], in log shipping
- secondary data files [SQL Server], adding
- log shipping [SQL Server], secondary databases
ms.assetid: b02eba13-f8e6-4684-b7e4-75ea038ea473
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3864af84ee12056b29c5e3b4c27a2693b1424120
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642619"
---
# <a name="add-a-secondary-database-to-a-log-shipping-configuration-sql-server"></a>Aggiungere un database secondario a una configurazione per il log shipping (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrata la procedura per l'aggiunta di un database secondario a una configurazione per il log shipping esistente in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le stored procedure per il log shipping richiedono l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>Per aggiungere un database secondario per il log shipping  
  
1.  Fare clic con il pulsante destro del mouse sul database che si vuole usare come database primario nella configurazione per il log shipping e quindi scegliere **Proprietà**.  
  
2.  Nella casella **Selezionare una pagina** fare clic su **Log shipping delle transazioni**.  
  
3.  In **Istanze del server e database secondari** fare clic su **Aggiungi**.  
  
4.  Fare clic su **Connetti** e connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che si intende utilizzare come server secondario.  
  
5.  Nella casella **Database secondario** scegliere un database dall'elenco oppure digitare il nome del database che si desidera creare.  
  
6.  Nella scheda **Inizializza database secondario** scegliere l'opzione che si intende utilizzare per inizializzare il database secondario.  
  
7.  Nella casella **Cartella di destinazione per i file copiati** della **scheda Copia file** digitare il percorso della cartella nella quale copiare i backup dei log delle transazioni. Spesso questa cartella si trova nel server secondario.  
  
8.  Si noti la pianificazione di copia presente nella casella **Pianificazione** in **Processo di copia**. Se si desidera personalizzare la pianificazione dell'installazione, fare clic su **Pianificazione** e quindi modificare la pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in base alle specifiche esigenze. Questa pianificazione dovrebbe essere abbastanza simile alla pianificazione del backup.  
  
9. In **Stato del database durante il ripristino dei backup** nella scheda **Ripristino** scegliere l'opzione **Modalità nessun recupero** oppure **Modalità standby** .  
  
10. Se si sceglie l'opzione **Modalità standby** , scegliere se si desidera disconnettere gli utenti dal database secondario durante l'operazione di ripristino.  
  
11. Se si desidera posticipare il processo di ripristino sul server secondario, scegliere un tempo di ritardo in **Ritardo minimo per il ripristino dei backup**.  
  
12. Scegliere una soglia di avviso in **Invia avviso se il ripristino non viene eseguito entro**.  
  
13. Si noti la pianificazione di ripristino presente nella casella **Pianificazione** in **Processo di ripristino**. Se si desidera personalizzare la pianificazione dell'installazione, fare clic su **Pianificazione** e quindi modificare la pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in base alle specifiche esigenze. Questa pianificazione dovrebbe essere abbastanza simile alla pianificazione del backup.  
  
14. Fare clic su **OK**.  
  
15. Fare clic su **OK** nella finestra di dialogo Proprietà database per avviare il processo di configurazione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>Per aggiungere un database secondario per il log shipping  
  
1.  Nel server secondario eseguire [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) specificando i dettagli del server e del database primario. Questa stored procedure restituisce l'ID secondario e gli ID dei processi di copia e ripristino.  
  
2.  Nel server secondario eseguire [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) per impostare la pianificazione relativa ai processi di copia e ripristino.  
  
3.  Nel server secondario eseguire [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) per aggiungere un database secondario.  
  
4.  Nel server primario eseguire [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) per aggiungere le informazioni necessarie relative al nuovo database secondario.  
  
5.  Nel server secondario abilitare i processi di copia e ripristino. Per altre informazioni, vedere [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Aggiornamento del log shipping a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Rimuovere un database secondario da una configurazione per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Rimuovere il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Visualizzare il report di log shipping &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Monitorare il log shipping &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tabelle e stored procedure relative al log shipping](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
