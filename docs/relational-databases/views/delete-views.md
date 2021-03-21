---
description: Eliminare viste
title: Eliminare viste | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- dropping views
- deleting views
- views [SQL Server], deleting
- removing views
ms.assetid: 6823c7f8-06ca-4bda-8482-7092f03d52a0
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: df88a3910bcedaf6180b65bb3f2663f10cdb8af9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751761"
---
# <a name="delete-views"></a>Eliminare viste
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  È possibile eliminare (rimuovere) le viste in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per eliminare una vista da un database utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Quando si rimuove una vista, dal catalogo di sistema vengono eliminate la definizione e altre informazioni della vista. Vengono inoltre eliminate tutte le autorizzazioni per la vista.  
  
-   Qualsiasi vista di una tabella che viene eliminata tramite `DROP TABLE` deve essere eliminata in modo esplicito utilizzando `DROP VIEW`.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per l'autorizzazione SCHEMA o CONTROL per OBJECT.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-delete-a-view-from-a-database"></a>Per eliminare una vista da un database  
  
1.  In **Esplora oggetti** espandere il database contenente la vista da eliminare, quindi espandere la cartella **Viste** .  
  
2.  Fare clic con il pulsante destro del mouse sulla vista da eliminare e scegliere **Elimina**.  
  
3.  Nella finestra di dialogo **Elimina oggetto** fare clic su **OK**.  
  
    > [!IMPORTANT]  
    >  Fare clic su **Mostra dipendenze** nella finestra di dialogo **Elimina oggetto** per aprire la finestra di dialogo **Dipendenze di**_nome\_vista_. Verranno visualizzati tutti gli oggetti che dipendono dalla vista e tutti gli oggetti da cui dipende la vista.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-delete-a-view-from-a-database"></a>Per eliminare una vista da un database  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio si elimina la vista specificata solo se la vista già esiste.  
  
    ```  
    USE AdventureWorks2012 ;  
    GO  
    IF OBJECT_ID ('HumanResources.EmployeeHireDate', 'V') IS NOT NULL  
    DROP VIEW HumanResources.EmployeeHireDate;  
    GO  
    ```  
  
 Per altre informazioni, vedere [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md).  
  
  
