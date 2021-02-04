---
description: Elencare le informazioni sulle categorie di processi
title: Elencare le informazioni sulle categorie di processi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 0fc668d4-6244-4fef-b90e-62d2c776cd7c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9df31336b1f1e24600246b0e2b493df0ff7a59f9
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251103"
---
# <a name="list-job-category-information"></a>Elencare le informazioni sulle categorie di processi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene descritto come elencare le informazioni sulla categoria del processo in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
-   **Prima di iniziare:**  
  
    [Sicurezza](#Security)  
  
-   **Per elencare le informazioni sulle categorie di processi usando:**  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-list-job-category-information"></a>Per elencare le informazioni sulle categorie di processi  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- returns information about jobs that are administered locally  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_help_category  
        @type = N'LOCAL' ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_help_category (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-category-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per elencare le informazioni sulle categorie di processi**  
  
Usare la classe **JobCategory** con un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell.  
