---
description: Visualizzazione delle informazioni relative a un operatore
title: Visualizzazione delle informazioni relative a un operatore
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- viewing operators
- operators (users) [Database Engine], viewing with Management Studio
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
- displaying operators
ms.assetid: 92c82cdf-f704-444e-9539-82aea7fe6fb7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: d270c1fc1a7cfa8e09af346448d4771527b2a514
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251306"
---
# <a name="view-information-about-an-operator"></a>Visualizzazione delle informazioni relative a un operatore
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come visualizzare informazioni relative a un operatore di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-information-about-an-operator"></a>Per visualizzare le informazioni relative a un operatore  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server che contiene l'operatore da visualizzare.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic sul segno più per espandere la cartella **Operatori** .  
  
4.  Fare clic con il pulsante destro del mouse sull'operatore da visualizzare e selezionare **Proprietà**.  
  
    Per altre informazioni sulle opzioni disponibili contenute nella finestra di dialogo _Proprietà_**nome_operatore** , vedere:  
  
    -   [Proprietà operatore - nuovo operatore &#40;pagina generale&#41;](../../ssms/agent/operator-properties-new-operator-general-page.md)  
  
    -   [Proprietà operatore -nuovo operatore &#40;pagina notifiche&#41;](../../ssms/agent/operator-properties-new-operator-notifications-page.md)  
  
    -   [Proprietà operatore &#40;pagina Cronologia&#41;](../../ssms/agent/operator-properties-history-page.md)  
  
5.  Al termine, fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-view-information-about-an-operator"></a>Per visualizzare le informazioni relative a un operatore  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- reports information about operator François Ajenstat   
    -- This example assumes that the operator exists.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_help_operator  
        @operator_name = N'François Ajenstat' ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_help_operator (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md).  
