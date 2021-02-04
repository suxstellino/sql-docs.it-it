---
description: Creazione di un avviso utilizzando un numero di errore
title: Creazione di un avviso utilizzando un numero di errore
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
- SQL Server Agent, alerts
- alerts [SQL Server], error numbers
ms.assetid: 03dd7fac-5073-4f86-babd-37e45a86023c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 19475d35ff7b0c3e44cf0222135129ac9838ed4c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236729"
---
# <a name="create-an-alert-using-an-error-number"></a>Creazione di un avviso utilizzando un numero di errore
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Istanza gestita di SQL di Azure da SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come creare un avviso di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] che viene generato quando si verifica un errore a cui è associato un numero specifico tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include un semplice strumento grafico per la gestione del sistema di avvisi ed è lo strumento consigliato per la configurazione di un'infrastruttura di avvisi.  
  
-   Gli eventi generati con la stored procedure **xp_logevent** si verificano nel database master. Pertanto, **xp_logevent** genera un avviso solo se **\@database_name** per l'avviso è **'master'** o NULL.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_alert**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-an-alert-using-an-error-number"></a>Per creare un avviso utilizzando un numero di errore  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera creare un avviso tramite un numero di errore.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse su **Avvisi** e selezionare **Nuovo avviso**.  
  
4.  Nella casella **Nome** della finestra di dialogo **Nuovo avviso** immettere un nome per l'avviso.  
  
5.  Selezionare la casella di controllo **Abilita** per abilitare l'esecuzione dell'avviso. Per impostazione predefinita, l'opzione **Abilita** è selezionata.  
  
6.  Nell'elenco **Tipo** selezionare **Avviso per evento di SQL Server**.  
  
7.  Nell'elenco **Nome database** sotto **Definizione di avviso di evento** selezionare un database per limitare l'avviso a un database specifico.  
  
8.  In **Genera avvisi in base a** fare clic su **Numero errore**, quindi digitare un numero errore valido per l'avviso. Alternativamente, fare clic su **Gravità** , quindi selezionare la gravità specifica che genererà l'avviso.  
  
9. Per limitare l'avviso a una particolare sequenza di caratteri, selezionare la casella di controllo corrispondente a **Genera avviso quando il messaggio contiene** e immettere una parola chiave o una stringa di caratteri nella casella **Testo del messaggio**. Il numero massimo di caratteri consentito è 100.  
  
10. Fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-create-an-alert-using-an-error-number"></a>Per creare un avviso utilizzando un numero di errore  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- adds an alert (Test Alert) that runs the Back up
    -- the AdventureWorks2012 Database job when fired   
    -- assumes that the message 55001 and the Back up
    -- the AdventureWorks2012 Database job already exist.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert  
        @name = N'Test Alert',  
        @message_id = 55001,   
       @severity = 0,   
       @notification_message = N'Error 55001 has occurred. The DB will be backed up...',   
       @job_name = N'Back up the AdventureWorks2012 Database' ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_add_alert](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md).  
