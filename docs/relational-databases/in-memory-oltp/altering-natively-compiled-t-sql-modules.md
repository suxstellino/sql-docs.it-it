---
title: Altering Natively Compiled T-SQL Modules (Modifica dei moduli T-SQL compilati in modo nativo) | Microsoft Docs
description: Informazioni su come eseguire operazioni ALTER su stored procedure compilate in modo nativo e moduli Transact-SQL compilati in modo nativo in SQL Server e nel database SQL di Azure.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 010318a0-6807-47c3-8ecc-bb7cb60513f0
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a873b9613ad85d6cad9a5932453d916a340f5844
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489711"
---
# <a name="altering-natively-compiled-t-sql-modules"></a>Altering Natively Compiled T-SQL Modules
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è possibile eseguire operazioni `ALTER` in stored procedure compilate in modo nativo e in altri moduli [!INCLUDE[tsql](../../includes/tsql-md.md)] compilati in modo nativo, ad esempio UDF scalari e trigger, con l'istruzione `ALTER`.  
  
Quando si esegue `ALTER` in un modulo [!INCLUDE[tsql](../../includes/tsql-md.md)] compilato in modo nativo, il modulo viene ricompilato con una nuova definizione. Durante la ricompilazione la versione precedente del modulo continua a essere disponibile per l'esecuzione. Una volta completata la compilazione, le esecuzioni dei moduli vengono svuotate e viene installata la nuova versione del modulo. Quando si modifica un modulo [!INCLUDE[tsql](../../includes/tsql-md.md)] compilato in modo nativo, è possibile modificare le opzioni seguenti.  
  
-   Parametri  
-   EXECUTE AS  
-   TRANSACTION ISOLATION LEVEL  
-   LANGUAGE  
-   DATEFIRST  
-   DATEFORMAT  
-   DELAYED_DURABILITY  
  
> [!NOTE]  
> I moduli [!INCLUDE[tsql](../../includes/tsql-md.md)] compilati in modo nativo non possono essere convertiti in moduli compilati in modo non nativo. I moduli T-SQL compilati in modo non nativo non possono essere convertiti in moduli compilati in modo nativo.  
  
Per altre informazioni sul funzionamento e la sintassi di `ALTER PROCEDURE`, vedere [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md).  
  
È possibile eseguire [sp_recompile](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) in moduli [!INCLUDE[tsql](../../includes/tsql-md.md)] compilati in modo nativo. Ciò causa la ricompilazione del modulo all'esecuzione successiva.  
  
## <a name="example"></a>Esempio  
L'esempio seguente crea una tabella ottimizzata per la memoria (T1) e una stored procedure compilata in modo nativo (usp_1) che seleziona tutte le colonne T1. usp_1 viene quindi modificata per rimuovere la clausola `EXECUTE AS`, modificare `LANGUAGE` e selezionare una sola colonna (C1) da T1.  
  
```sql  
CREATE TABLE [dbo].[T1] (  
  [c1] [int] NOT NULL,  
  [c2] [float] NOT NULL,  
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
   SELECT c1, c2 FROM dbo.T1  
END  
GO  
  
ALTER PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'Dutch'  
)  
   SELECT c1 FROM dbo.T1  
END  
GO    
```   
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)