---
description: Eliminare funzioni definite dall'utente
title: Eliminare funzioni definite dall'utente | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: db1d668a-23b7-4757-a9c5-1bd848ba7f6d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9b532481a55f23116d0fd90760d1a940f4cd82f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184290"
---
# <a name="delete-user-defined-functions"></a>Eliminare funzioni definite dall'utente
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile eliminare funzioni definite dall'utente in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Eliminare una funzione definita dall'utente tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile eliminare la funzione se nel database sono presenti funzioni o viste Transact-SQL che fanno riferimento a questa funzione e che sono state create tramite SCHEMABINDING oppure se sono presenti colonne calcolate, vincoli CHECK o vincoli DEFAULT che fanno riferimento alla funzione.  
  
-   Non è possibile eliminare la funzione se sono presenti colonne calcolate che fanno riferimento alla funzione e che sono state indicizzate.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER per lo schema a cui appartiene la funzione o l'autorizzazione CONTROL per la funzione.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-delete-a-user-defined-function"></a>Per eliminare una funzione definita dall'utente  
  
1.  Fare clic sul segno più accanto al database che contiene la funzione che si desidera modificare.  
  
2.  Fare clic sul segno più accanto alla cartella **Programmabilità** .  
  
3.  Fare clic sul segno più accanto alla cartella che contiene la funzione che si desidera modificare:  
  
    -   Table-valued Function  
  
    -   Funzione a valori scalari  
  
    -   Funzione di aggregazione  
  
4.  Fare clic con il pulsante destro del mouse sulla funzione che si desidera eliminare e scegliere **Elimina**.  
  
5.  Nella finestra di dialogo **Elimina oggetto** fare clic su **OK**.  

    > [!IMPORTANT]  
    >  Fare clic su **Mostra dipendenze** nella finestra di dialogo **Elimina oggetto** per aprire la finestra di dialogo **Dipendenze di**_nome\_funzione_. Verranno visualizzati tutti gli oggetti che dipendono dalla funzione e tutti gli oggetti da cui dipende la funzione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-delete-a-user-defined-function"></a>Per eliminare una funzione definita dall'utente  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- creates function called "Sales.ufn_SalesByStore"  
    USE AdventureWorks2012;  
    GO  
    CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
    RETURNS TABLE  
    AS  
    RETURN   
    (  
        SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
        FROM Production.Product AS P   
        JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
        JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
        JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
        WHERE C.StoreID = @storeid  
        GROUP BY P.ProductID, P.Name  
    );  
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- determines if function exists in database  
    IF OBJECT_ID (N'Sales.fn_SalesByStore', N'IF') IS NOT NULL  
    -- deletes function  
        DROP FUNCTION Sales.fn_SalesByStore;  
    GO  
    ```  
  
 Per altre informazioni, vedere [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md).  
  
  
