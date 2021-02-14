---
title: Rinominare una stored procedure | Microsoft Docs
description: Informazioni su come rinominare un stored procedure in SQL Server 2019 (15.x) usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], renaming
- renaming stored procedures
ms.assetid: 5d2e4c68-7e0b-4405-8919-f5b203e46770
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 433fa034c85418919aa48886c3a7c06757de890f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347668"
---
# <a name="rename-a-stored-procedure"></a>Rinominare una stored procedure

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In questo argomento viene descritto come rinominare una stored procedure in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per rinominare una stored procedure tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   I nomi delle procedure devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
-   La ridenominazione di una stored procedure consente di mantenere il valore `object_id` e tutte le autorizzazioni assegnate in modo specifico alla stored procedure. Quando si elimina e ricrea l'oggetto, viene creato un nuovo `object_id` e vengono rimosse tutte le autorizzazioni assegnate in modo specifico alla stored procedure.

-   La ridenominazione di una stored procedure non comporta la modifica del nome dell'oggetto corrispondente nella colonna di definizione della vista del catalogo **sys.sql_modules** . A questo scopo, è necessario eliminare e ricreare la stored procedure con il nuovo nome.  

-   La modifica del nome o della definizione di una stored procedure può causare un errore degli oggetti dipendenti se questi non vengono aggiornati in base alle modifiche apportate alla stored procedure. Per altre informazioni, vedere [Visualizzare le dipendenze di una stored procedure](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 CREATE PROCEDURE  
 Sono richieste l'autorizzazione CREATE PROCEDURE per il database e ALTER per lo schema in cui viene creata la procedura. In alternativa, è richiesta l'appartenenza al ruolo predefinito del database **db_ddladmin**.  
  
 ALTER PROCEDURE  
 È richiesta l'autorizzazione ALTER per la procedura o l'appartenenza al ruolo predefinito **db_ddladmin** del database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-rename-a-stored-procedure"></a>Per rinominare una stored procedure  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
2.  Espandere **Database**, espandere il database a cui appartiene la stored procedure, quindi espandere **Programmabilità**.  
3.  [Determinare le dipendenze della stored procedure](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
4.  Espandere **Stored Procedures**, fare clic con il pulsante destro del mouse sulla procedura da rinominare e quindi scegliere **Rinomina**.  
5.  Modificare il nome della stored procedure.  
6.  Modificare il nome della stored procedure in qualsiasi oggetto dipendente o script che vi fa riferimento.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-rename-a-stored-procedure"></a>Per rinominare una stored procedure  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
2.  Dalla barra Standard fare clic su **Nuova query**.  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene illustrato come rinominare una stored procedure eliminandola e ricreandola con un nuovo nome. Nel primo esempio si crea la stored procedure `'HumanResources.uspGetAllEmployeesTest`, nel secondo esempio la stored procedure viene rinominata in `HumanResources.uspEveryEmployeeTest`.  
  
```sql  
--Create the stored procedure.  
USE AdventureWorks2012;  
GO  

CREATE PROCEDURE HumanResources.uspGetAllEmployeesTest  
AS  
    SET NOCOUNT ON;  
    SELECT LastName, FirstName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory;  
GO  
  
--Rename the stored procedure.  
EXEC sp_rename 'HumanResources.uspGetAllEmployeesTest', 'uspEveryEmployeeTest'; 
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Creazione di una stored procedure](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modificare una stored procedure](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Eliminare una stored procedure](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Visualizzare la definizione di una stored procedure](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Visualizzare le dipendenze di una stored procedure](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)  
  
  
