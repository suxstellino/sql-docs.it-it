---
title: Creazione di stored procedure compilate in modo nativo | Microsoft Docs
description: Informazioni sulle funzionalità di Transact-SQL supportate solo per le stored procedure compilate in modo nativo. Vedere come creare stored procedure compilate in modo nativo in SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e6b34010-cf62-4f65-bbdf-117f291cde7b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 398e9dee801465cffd85d3ce65f0e344318b4cf1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481242"
---
# <a name="creating-natively-compiled-stored-procedures"></a>Creazione di stored procedure compilate in modo nativo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Le stored procedure compilate in modo nativo non implementano la programmabilità completa di [!INCLUDE[tsql](../../includes/tsql-md.md)] e la superficie di attacco delle query. Alcuni costrutti [!INCLUDE[tsql](../../includes/tsql-md.md)] non possono essere utilizzati all'interno delle stored procedure compilate in modo nativo. Per altre informazioni, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
Le funzionalità seguenti di [!INCLUDE[tsql](../../includes/tsql-md.md)] sono supportate unicamente per le stored procedure compilate in modo nativo:  
  
-   Blocchi atomici. Per altre informazioni, vedere [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  
  
-   Vincoli `NOT NULL` su parametri e variabili. Non è possibile assegnare valori **NULL** ai parametri o alle variabili dichiarati come **NOT NULL**. Per altre informazioni, vedere [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).  
  
    -   `CREATE PROCEDURE dbo.myproc (@myVarchar VARCHAR(32) NOT NULL) AS (...)`  
  
    -   `DECLARE @myVarchar VARCHAR(32) NOT NULL = "Hello"; -- Must initialize to a value.`  
  
    -   `SET @myVarchar = NULL; -- Compiles, but fails during run time.`  
  
-   Associazione allo schema delle stored procedure compilate in modo nativo.  
  
Le stored procedure compilate in modo nativo vengono create usando [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md). Nell'esempio seguente vengono illustrate una tabella ottimizzata per la memoria e una stored procedure compilata in modo nativo per inserire righe nella tabella.  
  
```sql  
CREATE TABLE [dbo].[T2] (  
  [c1] [int] NOT NULL, 
  [c2] [datetime] NOT NULL,
  [c3] nvarchar(5) NOT NULL, 
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_2] (@c1 int, @c3 nvarchar(5)) 
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
  DECLARE @c2 datetime = GETDATE();  
  INSERT INTO [dbo].[T2] (c1, c2, c3) values (@c1, @c2, @c3);  
END  
GO  
```  
 
Nell'esempio di codice **NATIVE_COMPILATION** indica che questa stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] è una stored procedure compilata in modo nativo. Sono necessarie le opzioni seguenti:  
  
|Opzione|Descrizione|  
|------------|-----------------|  
|**SCHEMABINDING**|Una stored procedure compilata in modo nativo deve essere associata allo schema degli oggetti a cui fa riferimento. Ciò significa che le tabelle a cui fa riferimento la procedura non possono essere eliminate. Le tabelle a cui viene fatto riferimento nella procedura devono includere il nome dello schema e i caratteri jolly (\*) non sono consentiti nelle query (nessun `SELECT * from...`). **SCHEMABINDING** è supportato unicamente per le stored procedure compilate in modo nativo in questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**BEGIN ATOMIC**|Il corpo di una stored procedure compilata in modo nativo deve essere costituito esattamente da un blocco atomico. I blocchi atomici garantiscono l'esecuzione atomica della stored procedure. Se la stored procedure viene richiamata all'esterno del contesto di una transazione attiva, verrà avviata una nuova transazione il cui commit avverrà alla fine del blocco atomico. I blocchi atomici nelle stored procedure compilate in modo nativo presentano due opzioni obbligatorie:<br /><br /> **TRANSACTION ISOLATION LEVEL**. Vedere [Transaction Isolation Levels for Memory-Optimized Tables](/previous-versions/sql/sql-server-2016/dn133175(v=sql.130)) (Livelli di isolamento delle transazioni per tabelle con ottimizzazione per la memoria) per i livelli di isolamento supportati.<br /><br /> **LANGUAGE**. Il linguaggio della stored procedure deve essere impostato su uno dei linguaggi o alias disponibili.|  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
