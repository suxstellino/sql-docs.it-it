---
title: Visualizzare le dipendenze di una stored procedure | Microsoft Docs
description: Informazioni su come visualizzare le dipendenze di una stored procedure in SQL Server 2019 (15.x) tramite SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], dependencies
- displaying stored procedure dependencies
- viewing stored procedure dependencies
ms.assetid: 6ae0a369-1bc7-4ae4-be89-2b483697cd1f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 91886e00f10f3aa4a84b054145e8078310c37024
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250156"
---
# <a name="view-the-dependencies-of-a-stored-procedure"></a>Visualizzare le dipendenze di una stored procedure
[!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md](../../includes/appliesto-ss-asdb-xxxx-pdw-md.md)]
  In questo argomento viene descritto come visualizzare le dipendenze di una stored procedure in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="Top"></a>   
-   **Prima di iniziare:**  [Limitazioni e restrizioni](#Restrictions), [Sicurezza](#Security)  
  
-   **Per visualizzare le dipendenze di una procedura usando:**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Funzione di sistema: **sys.dm_sql_referencing_entities**  
 Richiede l'autorizzazione CONTROL per l'entità a cui si fa riferimento e l'autorizzazione SELECT per sys.dm_sql_referencing_entities. Quando l'entità a cui si fa riferimento è una funzione di partizione, è necessaria l'autorizzazione CONTROL per il database. Per impostazione predefinita, l'autorizzazione SELECT è concessa al ruolo public.  
  
 Funzione di sistema: **sys.dm_sql_referenced_entities**  
 È richiesta l'autorizzazione SELECT per sys.dm_sql_referenced_entities e l'autorizzazione VIEW DEFINITION per l'entità di riferimento. Per impostazione predefinita, l'autorizzazione SELECT è concessa al ruolo public. È richiesta l'autorizzazione VIEW DEFINITION per il database o un'autorizzazione ALTER ANY DATABASE DDL TRIGGER per il database corrente quando l'entità di riferimento è un trigger DDL a livello di database. È richiesta l'autorizzazione VIEW ANY DEFINITION per il server quando l'entità di riferimento è un trigger DDL a livello di server.  
  
 Vista del catalogo dell'oggetto: **sys.sql_expression_dependencies**  
 Sono richieste l'autorizzazione VIEW DEFINITION sul database e l'autorizzazione SELECT su sys.sql_expression_dependencies per il database. L'autorizzazione SELECT è concessa per impostazione predefinita solo ai membri del ruolo predefinito del database di db_owner. Quando le autorizzazioni SELECT e VIEW DEFINITION vengono concesse a un altro utente, l'utente autorizzato può visualizzare tutte le dipendenze nel database.  
  
##  <a name="how-to-view-the-dependencies-of-a-stored-procedure"></a><a name="Procedures"></a> Modalità di visualizzazione delle dipendenze di una stored procedure  
 È possibile usare uno dei seguenti elementi:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per visualizzare le dipendenze di una stored procedure in Esplora oggetti**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database**, espandere il database a cui appartiene la stored procedure, quindi espandere **Programmabilità**.  
  
3.  Espandere **Stored procedure**, fare clic con il pulsante destro del mouse sulla stored procedure, quindi scegliere **Visualizza dipendenze**.  
  
4.  Visualizzare l'elenco di oggetti che dipendono dalla stored procedure.  
  
5.  Visualizzare l'elenco di oggetti da cui dipende la stored procedure.  
  
6.  Fare clic su **OK**.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per visualizzare le dipendenze di una stored procedure nell'editor di query**  
  
 Funzione di sistema: **sys.dm_sql_referencing_entities**  
 Questa funzione viene usata per visualizzare gli oggetti che dipendono da una stored procedure.  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database** ed espandere il database a cui appartiene la stored procedure.  
  
3.  Scegliere **Nuova query** dal menu **File** .  
  
4.  Copiare e incollare gli esempi seguenti nell'editor di query. Nel primo esempio viene creata la stored procedure `uspVendorAllInfo` mediante la quale vengono restituiti i nomi di tutti i fornitori nel database [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , i prodotti da essi forniti, nonché le informazioni relative alla posizione creditizia e alla disponibilità.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Dopo aver creato la stored procedure, nel secondo esempio viene usata la funzione sys.dm_sql_referencing_entities per visualizzare gli oggetti che dipendono dalla stored procedure.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
    FROM sys.dm_sql_referencing_entities ('Purchasing.uspVendorAllInfo', 'OBJECT');   
    GO  
  
    ```  
  
 Funzione di sistema: **sys.dm_sql_referenced_entities**  
 Questa funzione viene usata per visualizzare gli oggetti da cui dipende una stored procedure.  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database** ed espandere il database a cui appartiene la stored procedure.  
  
3.  Scegliere **Nuova query** dal menu **File** .  
  
4.  Copiare e incollare gli esempi seguenti nell'editor di query. Nel primo esempio viene creata la stored procedure `uspVendorAllInfo` mediante la quale vengono restituiti i nomi di tutti i fornitori nel database [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , i prodotti da essi forniti, nonché le informazioni relative alla posizione creditizia e alla disponibilità.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Dopo aver creato la stored procedure, nel secondo esempio viene usata la funzione sys.dm_sql_referenced_entities per visualizzare gli oggetti da cui dipende la stored procedure.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT referenced_schema_name, referenced_entity_name,  
    referenced_minor_name,referenced_minor_id, referenced_class_desc,  
    is_caller_dependent, is_ambiguous  
    FROM sys.dm_sql_referenced_entities ('Purchasing.uspVendorAllInfo', 'OBJECT');  
    GO  
    ```  
  
 Vista del catalogo dell'oggetto: **sys.sql_expression_dependencies**  
 Questa vista può essere usata per visualizzare gli oggetti da cui dipende una stored procedure o che dipendono da una stored procedure.  
  
 Visualizzazione degli oggetti che dipendono da una stored procedure.  
 1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database** ed espandere il database a cui appartiene la stored procedure.  
  
3.  Scegliere **Nuova query** dal menu **File** .  
  
4.  Copiare e incollare gli esempi seguenti nell'editor di query. Nel primo esempio viene creata la stored procedure `uspVendorAllInfo` mediante la quale vengono restituiti i nomi di tutti i fornitori nel database [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , i prodotti da essi forniti, nonché le informazioni relative alla posizione creditizia e alla disponibilità.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Al termine della creazione della stored procedure, nel secondo esempio viene usata la vista sys.sql_expression_dependencies per visualizzare gli oggetti che dipendono dalla stored procedure.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
        OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referenced_id = OBJECT_ID(N'Purchasing.uspVendorAllInfo')  
    GO  
    ```  
  
 Visualizzazione degli oggetti da cui dipende una stored procedure.  
 1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database** ed espandere il database a cui appartiene la stored procedure.  
  
3.  Scegliere **Nuova query** dal menu **File** .  
  
4.  Copiare e incollare gli esempi seguenti nell'editor di query. Nel primo esempio viene creata la stored procedure `uspVendorAllInfo` mediante la quale vengono restituiti i nomi di tutti i fornitori nel database [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , i prodotti da essi forniti, nonché le informazioni relative alla posizione creditizia e alla disponibilità.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Al termine della creazione della stored procedure, nel secondo esempio viene usata la vista sys.sql_expression_dependencies per visualizzare gli oggetti da cui dipende la stored procedure.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referencing_id = OBJECT_ID(N'Purchasing.uspVendorAllInfo');  
    GO  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Rinominare una stored procedure](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
