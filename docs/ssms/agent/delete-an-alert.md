---
title: Delete an Alert
description: Delete an Alert
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], deleting
- deleting alerts
- canceling alerts
- dropping alerts
- disabling alerts
- removing alerts
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: bf4cefa3d6bcd307b80ae4f4dc0944cdb18bcdba
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978787"
---
# <a name="delete-an-alert"></a>Delete an Alert

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo articolo descrive come eliminare gli avvisi di Microsoft SQL Server Agent usando SQL Server Management Studio o Transact-SQL.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni

Quando si rimuove un avviso, vengono rimosse anche le notifiche associate.

### <a name="security"></a><a name="Security"></a>Sicurezza

#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni

Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** possono eliminare gli avvisi.  

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio

### <a name="to-delete-an-alert"></a>Per eliminare un avviso

1. In **Esplora oggetti** selezionare il segno più per espandere il server che contiene l'avviso di SQL Server Agent che si desidera eliminare.

2. Selezionare il segno più per espandere **SQL Server Agent**.

3. Selezionare il segno più per espandere la cartella **avvisi** .

4. Fare clic con il pulsante destro del mouse sull'avviso da eliminare e scegliere **Elimina**.

5. Nella finestra di dialogo **Elimina oggetto** verificare che sia stato selezionato l'avviso corretto, quindi scegliere **OK**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL

### <a name="to-delete-an-alert"></a>Per eliminare un avviso

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].

2. Sulla barra Standard selezionare **Nuova query**.  

3. Copiare e incollare l'esempio seguente nella finestra query e selezionare **Execute (Esegui**).

    ```
    -- deletes the SQL Server Agent alert called 'Test Alert.'
    USE msdb ;
    GO
  
    EXEC dbo.sp_delete_alert
       @name = N'Test Alert' ;
    GO
    ```

Per altre informazioni, vedere [sp_delete_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md).
