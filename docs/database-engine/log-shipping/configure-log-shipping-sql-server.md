---
title: Configurare il log shipping (SQL Server) | Microsoft Docs
description: Informazioni su come configurare il log shipping usando SQL Server Management Studio o Transact-SQL in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], enabling
- log shipping [SQL Server], configuring
ms.assetid: c42aa04a-4945-4417-b4c7-50589d727e9c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71fd673e2453f49540d30567c8007f0fc5ad4c61
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813191"
---
# <a name="configure-log-shipping-sql-server"></a>Configurare il log shipping (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come configurare il log shipping in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] e le versioni successive supportano la compressione dei backup. Quando si crea una configurazione per il log shipping, è possibile determinare il comportamento della compressione dei backup per i backup del log. Per altre informazioni, vedere [Compressione backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Prerequisiti](#Prerequisites)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare il log shipping utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Il database primario deve utilizzare il modello di recupero con registrazione completa o registrazione minima delle operazioni bulk. Il passaggio al modello di recupero con registrazione minima comporterà l'arresto del log shipping.  
  
-   Prima di configurare il log shipping, è necessario creare una condivisione per rendere disponibili i backup dei log delle transazioni al server secondario. Si tratta di una condivisione della directory in cui verranno generati i backup dei log delle transazioni. Se, ad esempio, si esegue il backup dei log delle transazioni nella directory c:\data\tlogs\\, è possibile creare la condivisione \\\\*serverprimario*\tlogs per tale directory.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le stored procedure per il log shipping richiedono l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-log-shipping"></a>Per configurare il log shipping  
  
1.  Fare clic con il pulsante destro del mouse sul database da utilizzare come primario nella configurazione per il log shipping e quindi scegliere **Proprietà**.  
  
2.  Nella casella **Selezionare una pagina** fare clic su **Log shipping delle transazioni**.  
  
3.  Selezionare la casella di controllo **Abilita come database primario in una configurazione per il log shipping** .  
  
4.  In **Backup log delle transazioni** fare clic su **Impostazioni backup**.  
  
5.  Nella casella **Percorso di rete della cartella di backup** digitare il percorso di rete della condivisione creata per la cartella di backup dei log delle transazioni.  
  
6.  Casella **Se la cartella di backup si trova nel server primario, digitare il percorso locale della cartella (ad esempio: c:\\backup)** . Se la cartella di backup non si trova nel server primario, è possibile lasciare vuota la casella.  
  
    > [!IMPORTANT]  
    >  Se l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del server principale viene eseguito utilizzando l'account di sistema locale, è necessario creare la cartella di backup nel server primario e specificare il percorso locale della cartella.  
  
7.  Configurare i parametri **Elimina i file più vecchi di** e **Invia avviso se il backup non viene eseguito entro** .  
  
8.  Si noti la pianificazione di backup presente nella casella **Pianificazione** in **Processo di backup**. Se si desidera personalizzare la pianificazione dell'installazione, fare clic su **Pianificazione** e quindi modificare la pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in base alle specifiche esigenze.  
  
9. [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supporta la [compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md). Quando si crea una configurazione per il log shipping, è possibile determinare il comportamento della compressione dei backup per i backup del log scegliendo una delle opzioni seguenti: **Usa l'impostazione predefinita del server**, **Comprimi backup** o **Non comprimere il backup**. Per altre informazioni, vedere [Log Shipping Transaction Log Backup Settings](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md).  
  
10. Fare clic su **OK**.  
  
11. In **Istanze del server e database secondari** fare clic su **Aggiungi**.  
  
12. Fare clic su **Connetti** e connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che si intende utilizzare come server secondario.  
  
13. Nella casella **Database secondario** scegliere un database dall'elenco oppure digitare il nome del database che si desidera creare.  
  
14. Nella scheda **Inizializza database secondario** scegliere l'opzione che si intende utilizzare per inizializzare il database secondario.  
  
    > [!NOTE]  
    >  Se si sceglie di configurare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] in modo da inizializzare il database secondario da un backup di database, i file di dati e di log del database secondario vengono inseriti nello stesso percorso dei file di dati e di log del database **master** . Questo percorso sarà probabilmente diverso da quello dei file di dati e di log del database primario.  
  
15. Nella casella **Cartella di destinazione per i file copiati** della scheda **Copia file** digitare il percorso della cartella in cui copiare i backup dei log delle transazioni. Spesso questa cartella si trova nel server secondario.  
  
16. Si noti la pianificazione di copia presente nella casella **Pianificazione** in **Processo di copia**. Se si desidera personalizzare la pianificazione dell'installazione, fare clic su **Pianificazione** e quindi modificare la pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in base alle specifiche esigenze. Questa pianificazione dovrebbe essere abbastanza simile alla pianificazione del backup.  
  
17. In **Stato del database durante il ripristino dei backup** nella scheda **Ripristino** scegliere l'opzione **Modalità nessun recupero** oppure **Modalità standby** .  
    > [!IMPORTANT]  
    > L'opzione **Modalità standby** è disponibile solo quando la versione del server primario e quella del server secondario sono uguali. Quando la versione principale del server secondario è maggiore di quella del server primario è consentita solo l'opzione **Modalità nessun recupero**
  
18. Se si sceglie l'opzione **Modalità standby** , scegliere se si desidera disconnettere gli utenti dal database secondario durante l'operazione di ripristino.  
  
19. Se si desidera posticipare il processo di ripristino sul server secondario, scegliere un tempo di ritardo in **Ritardo minimo per il ripristino dei backup**.  
  
20. Scegliere una soglia di avviso in **Invia avviso se il ripristino non viene eseguito entro**.  
  
21. Si noti la pianificazione di ripristino presente nella casella **Pianificazione** in **Processo di ripristino**. Se si desidera personalizzare la pianificazione dell'installazione, fare clic su **Pianificazione** e quindi modificare la pianificazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in base alle specifiche esigenze. Questa pianificazione dovrebbe essere abbastanza simile alla pianificazione del backup.  
  
22. Fare clic su **OK**.  
  
23. In **Istanza server di monitoraggio** selezionare la casella di controllo **Usa un'istanza del server di monitoraggio** e quindi fare clic su **Impostazioni**.  
  
    > [!IMPORTANT]  
    >  Per eseguire il monitoraggio della configurazione per il log shipping, è necessario aggiungere subito il server di monitoraggio. Per aggiungere il server di monitoraggio in un momento successivo, sarà necessario rimuovere la configurazione per il log shipping e sostituirla con una configurazione nuova che includa un server di monitoraggio.  
  
24. Fare clic su **Connetti** e connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che si desidera utilizzare come server di monitoraggio.  
  
25. In **Connessioni server di monitoraggio** scegliere il metodo che i processi di backup, copia e ripristino devono utilizzare per la connessione al server di monitoraggio.  
  
26. In **Periodo memorizzazione cronologia** scegliere il periodo di memorizzazione dei record della cronologia di log shipping.  
  
27. Fare clic su **OK**.  
  
28. Nella finestra di dialogo **Proprietà database** fare clic su **OK** per iniziare il processo di configurazione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-log-shipping"></a>Per configurare il log shipping  
  
1.  Per inizializzare il database secondario, ripristinare un backup completo del database primario sul server secondario.  
  
2.  Nel server primario eseguire [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md) per aggiungere un database primario. La stored procedure restituisce l'ID del processo di backup e l'ID primario.  
  
3.  Nel server primario eseguire [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) per aggiungere una pianificazione per il processo di backup.  
  
4.  Nel server di monitoraggio eseguire [sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md) per aggiungere il processo di gestione degli avvisi.  
  
5.  Sul server primario abilitare il processo di backup.  
  
6.  Nel server secondario eseguire [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) specificando i dettagli del server e del database primario. Questa stored procedure restituisce l'ID secondario e gli ID dei processi di copia e ripristino.  
  
7.  Nel server secondario eseguire [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) per impostare la pianificazione relativa ai processi di copia e ripristino.  
  
8.  Nel server secondario eseguire [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) per aggiungere un database secondario.  
  
9. Nel server primario eseguire [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) per aggiungere le informazioni necessarie relative al nuovo database secondario.  
  
10. Nel server secondario abilitare i processi di copia e ripristino. Per altre informazioni, vedere [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Aggiornamento del log shipping a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Aggiungere un database secondario a una configurazione per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Rimuovere un database secondario da una configurazione per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Rimuovere il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Visualizzare il report di log shipping &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Monitorare il log shipping &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tabelle e stored procedure relative al log shipping](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
