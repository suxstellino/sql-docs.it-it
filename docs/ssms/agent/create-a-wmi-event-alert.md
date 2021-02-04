---
description: Creare un avviso per evento WMI
title: Creare un avviso per evento WMI
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI event alerts [SQL Server Management Studio]
ms.assetid: b8c46db6-408b-484e-98f0-a8af3e7ec763
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9dcb25ede4558b9a0980cc2443f7b0142f368a03
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236422"
---
# <a name="create-a-wmi-event-alert"></a>Creare un avviso per evento WMI
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene descritto come creare un avviso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che viene generato quando si verifica un evento specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] monitorato dal provider WMI per eventi del server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Per informazioni sull'uso del provider WMI per il monitoraggio degli eventi del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Classi e proprietà del provider WMI per eventi del server](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md). Per informazioni sulle autorizzazioni necessarie per ricevere notifiche di avviso per eventi WMI, vedere [Selezionare un account per il servizio SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md). Per altre informazioni su WQL, vedere [Utilizzo di WQL con il provider WMI per eventi del server](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md).  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include un semplice strumento grafico per la gestione del sistema di avvisi ed è lo strumento consigliato per la configurazione di un'infrastruttura di avvisi.  
  
-   Gli eventi generati con la stored procedure **xp_logevent** si verificano nel database master. Pertanto, **xp_logevent** genera un avviso solo se **\@database_name** per l'avviso è **'master'** o NULL.  
  
-   Sono supportati solo gli spazi dei nomi WMI sul computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_alert**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-wmi-event-alert"></a>Per creare un avviso per evento WMI  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera creare un avviso di evento WMI.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse su **Avvisi** e selezionare **Nuovo avviso**.  
  
4.  Nella casella **Nome** della finestra di dialogo **Nuovo avviso** immettere un nome per l'avviso.  
  
5.  Selezionare la casella di controllo **Abilita** per abilitare l'esecuzione dell'avviso. Per impostazione predefinita, l'opzione **Abilita** è selezionata.  
  
6.  Nell'elenco **Tipo** selezionare **Avviso per evento WMI**.  
  
7.  Nella casella **Spazio dei nomi** in **Definizione di avviso di evento WMI** specificare lo spazio dei nomi WMI da usare per l'istruzione WQL (WMI Query Language) che identifica l'evento WMI che attiva l'avviso.  
  
8.  Nella casella **Query** specificare l'istruzione WQL che identifica l'evento al quale risponde l'avviso.  
  
9. Fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-create-a-wmi-event-alert"></a>Per creare un avviso per evento WMI  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- creates a WMI event alert that retrieves all event properties for any ALTER_TABLE event that occurs on table AdventureWorks2012.Sales.SalesOrderDetail  
    -- This example assumes that the message 54001 already exists.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert  
        @name = N'Test Alert 2',  
        @message_id = 54001  
        @notification_message = N'Error 54001 has occurred on the Sales.SalesOrderDetail table on the AdventureWorks2012 database.',  
        @wmi_namespace = '\\.\root\Microsoft\SqlServer\ServerEvents\,  
        @wmi_query = N'SELECT * FROM ALTER_TABLE   
    WHERE DatabaseName = 'AdventureWorks2012' AND SchemaName = 'Sales'   
        AND ObjectType='Table' AND ObjectName = 'SalesOrderDetail'';  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_add_alert](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md).  
