---
description: Create an Operator
title: Create an Operator
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- jobs [SQL Server Agent], notification options
- operators (users) [Database Engine], creating with Management Studio
- SQL Server Agent jobs, notification options
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
ms.assetid: 1359d790-5905-4927-a208-e7155e7768a2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 0460e2726c58a56f88ba11f5a515718cac82199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342735"
---
# <a name="create-an-operator"></a>Create an Operator
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come configurare un utente per la ricezione di notifiche relative ai processi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
  
-   Le opzioni Cercapersone e **net send** verranno rimosse da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in una versione futura di [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare pertanto di utilizzarle in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui sono state implementate.  
  
-   Si noti che per inviare notifiche tramite posta elettronica e cercapersone agli operatori, è necessario configurare SQL Server Agent per l'utilizzo di Posta elettronica database. Per ulteriori informazioni, vedere [Procedura: Assegnazione di avvisi a un operatore (SQL Server Management Studio)](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è incluso un semplice strumento grafico per la gestione dei processi, che è lo strumento consigliato per la creazione e la gestione dell'infrastruttura dei processi.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Solo i membri del ruolo predefinito del server **sysadmin** possono creare gli operatori.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-an-operator"></a>Per creare un operatore  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera creare un operatore di SQL Server Agent.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Operatori** e quindi scegliere **Nuovo operatore**.  
  
    Nella pagina **Generale** della finestra di dialogo **Nuovo operatore** sono disponibili le opzioni indicate di seguito.  
  
    **Nome**  
    Consente di modificare il nome dell'operatore.  
  
    **Enabled**  
    Consente di abilitare l'operatore. Se non è abilitato, all'operatore non verranno inviate notifiche.  
  
    **Indirizzo posta elettronica**  
    Specifica l'indirizzo di posta elettronica dell'operatore.  
  
    **Indirizzo Net Send**  
    Specifica l'indirizzo da usare per **Net Send**.  
  
    **Indirizzo cercapersone**  
    Specifica l'indirizzo di posta elettronica da utilizzare per il cercapersone dell'operatore.  
  
    **Pianificazione cercapersone per operatore in servizio**  
    Consente di impostare gli orari in cui il cercapersone è attivo.  
  
    **Lunedì - Domenica**  
    Consente di selezionare i giorni in cui il cercapersone è attivo.  
  
    **Inizio giornata lavorativa**  
    Consente di selezionare l'ora a partire da cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent invierà messaggi al cercapersone.  
  
    **Fine giornata lavorativa**  
    Consente di selezionare l'ora oltre cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non invierà più messaggi al cercapersone.  
  
    Nella pagina **Generale** della finestra di dialogo **Notifiche** sono disponibili le opzioni indicate di seguito.  
  
    **Avvisi**  
    Consente di visualizzare gli avvisi nell'istanza.  
  
    **Processi**  
    Consente di visualizzare i processi nell'istanza.  
  
    **Elenco avvisi**  
    Consente di visualizzare l'elenco degli avvisi nell'istanza.  
  
    **Elenco processi**  
    Consente di visualizzare l'elenco dei processi nell'istanza.  
  
    **Posta elettronica**  
    Consente di inviare una notifica all'operatore tramite posta elettronica.  
  
    **Cercapersone**  
    Inviare una notifica all'operatore tramite un messaggio di posta elettronica all'indirizzo del cercapersone.  
  
    **Net Send**  
    Notificare questo operatore tramite **Net Send**.  
  
4.  Al termine della creazione del nuovo operatore, fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-create-an-operator"></a>Per creare un operatore  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- sets up the operator information for user 'danwi.'
    -- The operator is enabled.   
    -- SQL Server Agent sends notifications by pager 
    -- from Monday through Friday from 8 A.M. to 5 P.M.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_operator  
        @name = N'Dan Wilson',  
        @enabled = 1,  
        @email_address = N'danwi',  
        @pager_address = N'5551290AW@pager.Adventure-Works.com',  
        @weekday_pager_start_time = 080000,  
        @weekday_pager_end_time = 170000,  
        @pager_days = 62 ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_add_operator (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md).  
