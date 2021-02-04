---
description: Escludere un server di destinazione da un server master
title: Escludere un server di destinazione da un server master
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, target servers
- target servers [SQL Server], defecting
- SQL Server Agent jobs, master servers
- master servers [SQL Server], defecting target servers
- defecting target servers
ms.assetid: a6da262b-7b38-4ce4-bfd6-6a557c6e8a84
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1c015e64fa99613f385e2d5f426ac469e25f96ae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234732"
---
# <a name="defect-a-target-server-from-a-master-server"></a>Escludere un server di destinazione da un server master
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene descritto come escludere un server di destinazione da un server master in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects (SMO). Eseguire questa procedura dal server di destinazione.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per eseguire questa stored procedure, è necessario che gli utenti siano membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-defect-a-target-server-from-a-master-server"></a>Per escludere un server di destinazione da un server master  
  
1.  In **Esplora oggetti** espandere un server configurato come server di destinazione.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent**, scegliere **Amministrazione multiserver** e fare clic su **Escludi**.  
  
3.  Fare clic su **Sì** per confermare l'esclusione del server di destinazione da un server master.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-defect-a-target-server-from-a-master-server"></a>Per escludere un server di destinazione da un server master  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
```  
sp_msx_defect ;  
```  
  
Per altre informazioni, vedere [sp_msx_defect (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md).  
  
## <a name="using-sql-server-management-objects-smo"></a><a name="PowerShellProcedure"></a>Utilizzo di SQL Server Management Objects (SMO)  
Usare il **metodo MsxDefect**.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione di un ambiente multiserver](../../ssms/agent/create-a-multiserver-environment.md)  
[Amministrazione automatizzata in un'organizzazione](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[Escludere più server di destinazione da un server master](../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
