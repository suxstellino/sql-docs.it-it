---
description: Visualizzazione di informazioni sulle regole di confronto
title: Visualizzazione di informazioni sulle regole di confronto | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- collations [SQL Server], view
ms.assetid: 1338b4ea-7142-44bc-a3b9-44e54431405f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a8806f26cd27ebe671946a0efa723ddaa030970
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251166"
---
# <a name="view-collation-information"></a>Visualizzazione di informazioni sulle regole di confronto
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
<a name="Top"></a> È possibile visualizzare le regole di confronto di un server, di un database o di una colonna in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usando le opzioni di menu di Esplora oggetti oppure tramite [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="how-to-view-a-collation-setting"></a><a name="Procedures"></a> Visualizzazione di un'impostazione relativa alle regole di confronto  
 È possibile usare uno dei seguenti elementi:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per visualizzare un'impostazione delle regole di confronto per un server (istanza di SQL Server) in Esplora oggetti**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Fare clic con il pulsante destro del mouse sull'istanza e selezionare **Proprietà**.  
  
 **Per visualizzare un'impostazione delle regole di confronto per un database in Esplora oggetti**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database**, fare clic con il pulsante destro del mouse sul database e selezionare **Proprietà**.  
  
 **Per visualizzare un'impostazione delle regole di confronto per una colonna in Esplora oggetti**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database**, il database desiderato, quindi **Tabelle**.  
  
3.  Espandere la tabella che contiene la colonna, quindi espandere **Colonne**.  
  
4.  Fare clic con il pulsante destro del mouse sulla colonna e selezionare **Proprietà**. Se la proprietà di regola di confronto è vuota, la colonna non è un tipo di dati character.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per visualizzare l'impostazione delle regole di confronto di un server**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e nella barra degli strumenti fare clic su **Nuova query**.  
  
2.  Nella finestra Query, immettere l'istruzione seguente che usano la funzione di sistema SERVERPROPERTY.  
  
    ```sql  
    SELECT CONVERT (varchar(256), SERVERPROPERTY('collation'));  
    ```  
  
3.  In alternativa, è possibile usare la stored procedure di sistema sp_helpsort.  
  
    ```sql  
    EXECUTE sp_helpsort;  
    ```  
  
 **Per visualizzare tutte le regole di confronto supportate da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e nella barra degli strumenti fare clic su **Nuova query**.  
  
2.  Nella finestra Query, immettere l'istruzione seguente che usano la funzione di sistema SERVERPROPERTY.  
  
    ```sql  
    SELECT name, description FROM sys.fn_helpcollations();  
    ```  
  
 **Per visualizzare l'impostazione delle regole di confronto di un database**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e nella barra degli strumenti fare clic su **Nuova query**.  
  
2.  Nella finestra Query, immettere l'istruzione seguente che usano la vista del catalogo sys.databases.  
  
    ```sql  
    SELECT name, collation_name FROM sys.databases;  
    ```  
  
3.  In alternativa, è possibile usare la funzione di sistema DATABASEPROPERTYEX.  
  
    ```sql  
    SELECT CONVERT (varchar(256), DATABASEPROPERTYEX('database_name','collation'));  
    ```  
  
 **Per visualizzare l'impostazione delle regole di confronto di una colonna**  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e nella barra degli strumenti fare clic su **Nuova query**.  
  
2.  Nella finestra Query, immettere l'istruzione seguente che usano la vista del catalogo sys.columns.  
  
    ```sql  
    SELECT name, collation_name FROM sys.columns WHERE name = N'<insert character data type column name>';  
    ```  
  
 **Per visualizzare le impostazioni delle regole di confronto per tabelle e colonne**  

1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e nella barra degli strumenti fare clic su **Nuova query**.  
  
2.  Nella finestra Query, immettere l'istruzione seguente che usano la vista del catalogo sys.columns.  
  
    ```sql  
    SELECT t.name TableName, c.name ColumnName, collation_name  
    FROM sys.columns c  
    inner join sys.tables t on c.object_id = t.object_id;  
    ```  



## <a name="see-also"></a>Vedere anche  
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [Precedenza delle regole di confronto &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md)   
 [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)      
 [sp_helpsort &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsort-transact-sql.md)  
  
  
