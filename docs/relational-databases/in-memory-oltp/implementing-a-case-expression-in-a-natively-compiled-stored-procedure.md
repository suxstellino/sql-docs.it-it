---
title: Espressione CASE in una stored procedure compilata in modo nativo
description: I moduli T-SQL compilati in modo nativo supportano le espressioni CASE in alcune versioni di SQL Server. In questo esempio viene implementata l'espressione CASE in una query.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2f82db01-da7e-4a7d-8bc0-48b245e6f768
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8011f052e960818bd376c7a1986083674fc74c61
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492309"
---
# <a name="implementing-a-case-expression-in-a-natively-compiled-stored-procedure"></a>Implementazione di un'espressione CASE in una stored procedure compilata in modo nativo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

**Si applica a**: [!INCLUDE[ssSDSFull_md](../../includes/sssdsfull-md.md)] e SQL Server a partire da [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]

Le espressioni CASE sono supportate nei moduli T-SQL compilati in modo nativo. Nell'esempio seguente viene illustrato come usare l'espressione CASE in una query. 

``` 
-- Query using a CASE expression in a natively compiled stored procedure.
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
   SELECT   
      SalesOrderID,   
      CASE (OnlineOrderFlag)   
      WHEN 1 THEN N'Order placed online by customer'  
      ELSE N'Order placed by sales person'  
      END  
   FROM Sales.SalesOrderHeader_inmem
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
``` 

**Si applica a**: [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)] e SQL Server a partire da [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]

  Le espressioni CASE *non* sono supportate nei moduli T-SQL compilati in modo nativo. L'esempio seguente illustra una modalità di implementazione della funzionalità di un'espressione CASE in una stored procedure compilata in modo nativo.  
  
 Gli esempi di codice usano una variabile di tabella per costruire un singolo set di risultati. Questa opzione è applicabile solo quando si elabora un numero limitato di righe perché prevede la creazione di un'altra copia delle righe di dati.  
  
 È necessario testare le prestazioni di questa soluzione alternativa.  
  
```  
-- original query  
SELECT   
   SalesOrderID,   
   CASE (OnlineOrderFlag)   
   WHEN 1 THEN N'Order placed online by customer'  
   ELSE N'Order placed by sales person'  
   END  
FROM Sales.SalesOrderHeader_inmem  
  
--  workaround for CASE in natively compiled stored procedures  
--  use a table for the single resultset  
CREATE TYPE dbo.SOHOnlineOrderResult AS TABLE  
(  
   SalesOrderID uniqueidentifier not null index ix_SalesOrderID,  
     OrderFlag nvarchar(100) not null  
) with (memory_optimized=on)  
go  
  
-- natively compiled stored procedure that includes the query  
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  
      (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
  
   -- table variable for creating the single resultset  
   DECLARE @result dbo.SOHOnlineOrderResult  
  
   -- CASE OnlineOrderFlag=1  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed online by customer'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag=1  
  
   -- ELSE  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed by sales person'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag!=1  
  
   -- return single resultset  
   SELECT SalesOrderID, OrderFlag FROM @result  
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Problemi di migrazione relativi alle stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Costrutti Transact-SQL non supportati da OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
