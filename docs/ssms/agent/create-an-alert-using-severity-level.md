---
description: Creazione di un avviso utilizzando i livelli di gravità
title: Creazione di un avviso utilizzando i livelli di gravità
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
- SQL Server Agent, alerts
- severity levels [SQL Server]
- alerts [SQL Server], severity levels
ms.assetid: a1fd71bf-5bf9-4ce2-9a1d-032576a4a6e9
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 5f6a6eb6dc467674a0bd4f00a6faaa240b8245fd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271682"
---
# <a name="create-an-alert-using-severity-level"></a>Creazione di un avviso utilizzando i livelli di gravità
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come creare un avviso di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, che viene generato quando si verifica un evento di un determinato livello di gravità in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include un semplice strumento grafico per la gestione del sistema di avvisi ed è lo strumento consigliato per la configurazione di un'infrastruttura di avvisi.  
  
-   Gli eventi generati con la stored procedure **xp_logevent** si verificano nel database master. Pertanto, **xp_logevent** genera un avviso solo se **\@database_name** per l'avviso è **'master'** o NULL.  
  
-   I livelli di gravità da 19 a 25 determinano l'invio di un messaggio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al registro applicazioni di [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows e l'attivazione di un avviso. Gli eventi con livello di gravità inferiore a 19 determinano l'attivazione di avvisi solo se è stato usato **sp_altermessage**, RAISERROR WITH LOG oppure **xp_logevent** per forzarne la scrittura nel registro applicazioni di Windows.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_alert**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-an-alert-using-severity-level"></a>Per creare un avviso utilizzando il livello di gravità  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera creare un avviso tramite un livello di gravità.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse su **Avvisi** e selezionare **Nuovo avviso**.  
  
4.  Nella casella **Nome** della finestra di dialogo **Nuovo avviso** immettere un nome per l'avviso.  
  
5.  Nell'elenco **Tipo** selezionare **Avviso per evento di SQL Server**.  
  
6.  Nell'elenco **Nome database** sotto **Definizione di avviso di evento** selezionare un database per limitare l'avviso a un database specifico.  
  
7.  In **Genera avvisi in base a** fare clic su **Gravità** , quindi selezionare una gravità specifica che genera l'avviso.  
  
8.  Per limitare l'avviso a una particolare sequenza di caratteri, selezionare la casella di controllo corrispondente a **Genera avviso quando il messaggio contiene** e immettere una parola chiave o una stringa di caratteri nella casella **Testo del messaggio**. Il numero massimo di caratteri consentito è 100.  
  
9. Fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-create-an-alert-using-severity-level"></a>Per creare un avviso utilizzando il livello di gravità  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Adds an alert (Test Alert) that notifies the
    -- Alert Operator via email when an error with a 
    -- severity of 23 is detected.
    
    -- Assumes that the Alert Operator already exists 
    -- and that database mail is configured.
    
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert @name=N'Test Alert', 
      @message_id = 0, 
      @severity = 23, 
      @enabled = 1, 
      @include_event_description_in = 1
    ;
    GO
    
    EXEC dbo.sp_add_notification @alert_name=N'Test Alert',
      @operator_name=N'Alert Operator',
      @notification_method=1
    ;
    GO

    ```  
  
Per altre informazioni, vedere [sp_add_alert](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md).  
