---
title: Visualizzare le proprietà delle guide di piano | Microsoft Docs
description: Informazioni su come visualizzare le proprietà delle guide di piano in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.planguideprop.general.f1
helpviewer_keywords:
- plan guides [SQL Server], view plan guide properties
- viewing plan guide properties
ms.assetid: 8c0d2f39-59c1-4168-a649-65473f6a771b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 72b34306250bc24018c719f10700a29f4b7d6d0c
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766484"
---
# <a name="view-plan-guide-properties"></a>Visualizzare le proprietà delle guide di piano
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile visualizzare le proprietà delle guide di piano in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per visualizzare le proprietà delle guide di piano utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 La visibilità dei metadati nelle viste del catalogo è limitata alle entità a protezione diretta di cui un utente è proprietario o per le quali dispone di autorizzazioni.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-a-plan-guide"></a>Per visualizzare le proprietà di una guida di piano  
  
1.  Fare clic sul segno più per espandere il database in cui si desidera visualizzare le proprietà di una guida di piano, quindi fare clic sul segno più per espandere la cartella **Programmabilità** .  
  
2.  Fare clic sul segno più per espandere la cartella **Guide di piano** .  
  
3.  Fare clic con il pulsante destro del mouse sulla guida di piano di cui visualizzare le proprietà e selezionare **Proprietà**.  
  
     Le seguenti proprietà vengono visualizzate nella finestra di dialogo **Proprietà guida di piano** .  
  
     **Hint**  
     Vengono visualizzati gli hint per la query o il piano di query da applicare all'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] . Quando un piano di query viene specificato come un hint, viene visualizzato l'output di Showplan XML per il piano.  
  
     **Disabilitato**  
     Visualizza lo stato della guida di piano. I valori possibili sono **True** e **False**.  
  
     **Nome**  
     Visualizza il nome della guida di piano.  
  
     **Parameters**  
     Quando il tipo di ambito è SQL o TEMPLATE, vengono visualizzati il nome e il tipo di dati di tutti i parametri incorporati nell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
     **Batch ambito**  
     Visualizza il testo del batch nel quale viene visualizzata l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
     **Nome oggetto dell'ambito**  
     Quando il tipo di ambito è OBJECT, viene visualizzato il nome della stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] , della funzione scalare definita dall'utente o del trigger DML in cui viene visualizzata l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
     **Nome schema ambito**  
     Quando il tipo di ambito è OBJECT, viene visualizzato il nome dello schema che contiene l'oggetto.  
  
     **Tipo di ambito**  
     Visualizza il tipo di entità nel quale viene visualizzata l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] . Viene specificato il contesto per adeguare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] alla guida di piano. I valori possibili sono **OBJECT**, **SQL** e **TEMPLATE**.  
  
     **Istruzione**  
     Viene visualizzata l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] sulla quale deve essere applicata la guida di piano.  
  
4.  Fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-view-the-properties-of-a-plan-guide"></a>Per visualizzare le proprietà di una guida di piano  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- If a plan guide named "Guide1" already exists in the AdventureWorks2012 database, delete it.  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID(N'Guide1') IS NOT NULL  
       EXEC sp_control_plan_guide N'DROP', N'Guide1';  
    GO  
    -- creates a plan guide named Guide1 based on a SQL statement  
    EXEC sp_create_plan_guide   
        @name = N'Guide1',   
        @stmt = N'SELECT TOP 1 *   
                  FROM Sales.SalesOrderHeader   
                  ORDER BY OrderDate DESC',   
        @type = N'SQL',  
        @module_or_batch = NULL,   
        @params = NULL,   
        @hints = N'OPTION (MAXDOP 1)';  
    GO  
    -- Gets the name, created date, and all other relevant property information on the plan guide created above.   
    SELECT name AS plan_guide_name,  
       create_date,  
       query_text,  
       scope_type_desc,  
       OBJECT_NAME(scope_object_id) AS scope_object_name,  
       scope_batch,  
       parameters,  
       hints,  
       is_disabled  
    FROM sys.plan_guides  
    WHERE name = N'Guide1';  
    GO  
    ```  
  
 Per altre informazioni, vedere [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md).  
  
  
