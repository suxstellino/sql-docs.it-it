---
description: CREATE SYNONYM (Transact-SQL)
title: CREATE SYNONYM (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SYNONYM_TSQL
- SYNONYM_TSQL
- SYNONYM
- CREATE SYNONYM
dev_langs:
- TSQL
helpviewer_keywords:
- alternate names [SQL Server]
- names [SQL Server], synonyms
- CREATE SYNONYM statement
- synonyms [SQL Server], creating
ms.assetid: 41313809-e970-449c-bc35-85da2ef96e48
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 181476c45c78374d359e833bea1a0da7aa924a4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192660"
---
# <a name="create-synonym-transact-sql"></a>CREATE SYNONYM (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un nuovo sinonimo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- SQL Server Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR <object>  
  
<object> :: =  
{  
    [ server_name.[ database_name ] . [ schema_name_2 ]. object_name   
  | database_name . [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
```  
-- Azure SQL Database Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR < object >  
  
< object > :: =  
{  
    [database_name. [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name_1*  
 Specifica lo schema in cui viene creato il sinonimo. Se lo *schema* viene omesso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa lo schema predefinito dell'utente corrente.  
  
 *synonym_name*  
 Nome del nuovo sinonimo.  
  
 *server_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Indica il nome del server in cui è archiviato l'oggetto di base.  
  
 *database_name*  
 Indica il nome del database in cui è archiviato l'oggetto di base. Se *database_name* viene omesso, viene usato il nome del database corrente.  
  
 *schema_name_2*  
 Nome dello schema dell'oggetto di base. Se *schema_name* viene omesso, viene usato lo schema predefinito dell'utente corrente.  
  
 *object_name*  
 Nome dell'oggetto di base a cui fa riferimento il sinonimo.  
  
 Il database SQL di Azure supporta il formato del nome in tre parti, nome_database.[nome_schema].nome_oggetto, quando nome_database è il database corrente oppure nome_database è tempdb e nome_oggetto inizia con #.  
  
## <a name="remarks"></a>Osservazioni  
 Non è necessario che l'oggetto di base sia esistente in fase di creazione del sinonimo. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verifica l'esistenza dell'oggetto di base in fase di esecuzione.  
  
 È possibile creare sinonimi per i tipi di oggetti seguenti:  
  
- Stored procedure di assembly (CLR)
- Funzione con valori di tabella di assembly (CLR)
- Funzione scalare di assembly (CLR)
- Funzioni aggregate di aggregazione assembly (CLR)
- Procedura di filtro della replica
- Stored procedure estesa
- Funzione scalare SQL
- Funzione con valori di tabella di SQL
- Funzione SQL inline con valori di tabella
- Stored procedure di SQL
- Tabella<sup>1</sup> (definita dall'utente)
- Visualizzazione

 <sup>1 Include tabelle temporanee globali e locali</sup>  
  
 I nomi composti da quattro parti non sono supportati per gli oggetti funzione di base.  
  
 È possibile creare, eliminare e fare riferimento ai sinonimi in SQL dinamico.
 
 > [!NOTE]
 > I sinonimi sono specifici del database e non è possibile accedervi da altri database.
  
## <a name="permissions"></a>Autorizzazioni  
 Per poter creare un sinonimo in un determinato schema, un utente deve disporre dell'autorizzazione CREATE SYNONYM, oltre a disporre della proprietà dello schema o dell'autorizzazione ALTER SCHEMA.  
  
 L'autorizzazione CREATE SYNONYM è un'autorizzazione che può essere concessa.  
  
> [!NOTE]  
>  Per compilare l'istruzione CREATE SYNONYM non è necessaria l'autorizzazione sull'oggetto di base, poiché tutti i controlli delle autorizzazioni sull'oggetto di base sono rimandati fino alla fase di esecuzione.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-synonym-for-a-local-object"></a>R. Creazione di un sinonimo per un oggetto locale  
 Nell'esempio seguente viene prima creato un sinonimo per l'oggetto di base `Product` nel database `AdventureWorks2012`, quindi viene eseguita una query sul sinonimo.  
  
```sql 
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
  
-- Query the Product table by using the synonym.  
SELECT ProductID, Name   
FROM MyProduct  
WHERE ProductID < 5;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ----------------------- 
 ProductID   Name 
 ----------- -------------------------- 
 1           Adjustable Race 
 2           Bearing Ball 
 3           BB Ball Bearing 
 4           Headset Ball Bearings 

 (4 row(s) affected)
``` 
  
### <a name="b-creating-a-synonym-to-remote-object"></a>B. Creazione di un sinonimo per l'oggetto remoto  
 Nell'esempio seguente, l'oggetto di base, `Contact`, è contenuto in un server remoto denominato `Server_Remote`.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql 
EXEC sp_addlinkedserver Server_Remote;  
GO  
USE tempdb;  
GO  
CREATE SYNONYM MyEmployee FOR Server_Remote.AdventureWorks2012.HumanResources.Employee;  
GO  
```  
  
### <a name="c-creating-a-synonym-for-a-user-defined-function"></a>C. Creazione di un sinonimo per una funzione definita dall'utente  
 Nell'esempio seguente viene creata una funzione denominata `dbo.OrderDozen` che aumenta le quantità degli ordini a una dozzina di unità uniformi. Viene quindi creato il sinonimo `dbo.CorrectOrder` per la funzione `dbo.OrderDozen`.  
  
```sql  
-- Creating the dbo.OrderDozen function  
CREATE FUNCTION dbo.OrderDozen (@OrderAmt INT)  
RETURNS INT  
WITH EXECUTE AS CALLER  
AS  
BEGIN  
IF @OrderAmt % 12 <> 0  
BEGIN  
    SET @OrderAmt +=  12 - (@OrderAmt % 12)  
END  
RETURN(@OrderAmt);  
END;  
GO  
  
-- Using the dbo.OrderDozen function  
DECLARE @Amt INT;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.OrderDozen(@Amt) AS ModifiedOrder;  
  
-- Create a synonym dbo.CorrectOrder for the dbo.OrderDozen function.  
CREATE SYNONYM dbo.CorrectOrder  
FOR dbo.OrderDozen;  
GO  
  
-- Using the dbo.CorrectOrder synonym.  
DECLARE @Amt INT;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.CorrectOrder(@Amt) AS ModifiedOrder;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DROP SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/drop-synonym-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
