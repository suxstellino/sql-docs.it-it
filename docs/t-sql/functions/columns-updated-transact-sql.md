---
description: COLUMNS_UPDATED (Transact-SQL)
title: COLUMNS_UPDATED (Transact-SQL)
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLUMNS_UPDATED_TSQL
- COLUMNS_UPDATED
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS_UPDATED function
- testing columns
- column testing [SQL Server]
- updated columns
ms.assetid: 765fde44-1f95-4015-80a4-45388f18a42c
author: cawrites
ms.author: chadam
ms.openlocfilehash: c927fbad08c222aad4e284a66d3b3e61061e82e9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101285"
---
# <a name="columns_updated-transact-sql"></a>COLUMNS_UPDATED (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce uno schema di bit **varbinary** che indica le colonne inserite o aggiornate in una tabella o vista. Usare `COLUMNS_UPDATED` in qualsiasi punto all'interno del corpo di un trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT o UPDATE per controllare se il trigger deve eseguire operazioni specifiche.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
COLUMNS_UPDATED ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
**varbinary**
  
## <a name="remarks"></a>Osservazioni  
`COLUMNS_UPDATED` controlla l'esecuzione delle operazioni UPDATE o INSERT in più colonne. Per controllare i tentativi di esecuzione delle operazioni UPDATE o INSERT in una colonna, usare [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md).
  
`COLUMNS_UPDATED` restituisce uno o più byte ordinati da sinistra a destra. Il bit più a destra di ogni byte è il bit meno significativo. Il bit più a destra del byte più a sinistra rappresenta la prima colonna della tabella, il bit successivo a sinistra rappresenta la seconda colonna e così via. `COLUMNS_UPDATED` restituisce più byte se la tabella in cui viene creato il trigger include più di otto colonne. Il byte più a sinistra è il meno significativo. `COLUMNS_UPDATED` restituisce TRUE per tutte le colonne nelle azioni INSERT in quanto nelle colonne vengono inseriti valori espliciti o impliciti (NULL).
  
Per controllare l'esecuzione di operazioni di aggiornamento o inserimento in colonne specifiche, aggiungere alla sintassi un operatore bit per bit e una maschera di bit di valori interi delle colonne sottoposte a controllo. Nella tabella **t1**, ad esempio, sono incluse le colonne **C1**, **C2**, **C3**, **C4** e **C5**. Per controllare se le colonne **C2**, **C3** e **C4** sono state aggiornate (la tabella **t1** include un trigger UPDATE), aggiungere alla sintassi **& 14**. Per controllare se solo la colonna **C2** è stata aggiornata, specificare **& 2**. Vedere l'[esempio A](#a-using-columns_updated-to-test-the-first-eight-columns-of-a-table) e l'[esempio B](#b-using-columns_updated-to-test-more-than-eight-columns) per esempi effettivi.
  
Usare `COLUMNS_UPDATED` in qualsiasi punto all'interno di un trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT o UPDATE.
  
La colonna ORDINAL_POSITION della vista INFORMATION_SCHEMA.COLUMNS non è compatibile con lo schema di bit delle colonne restituite da `COLUMNS_UPDATED`. Per ottenere uno schema di bit compatibile con `COLUMNS_UPDATED`, fare riferimento alla proprietà `ColumnID` della funzione di sistema `COLUMNPROPERTY` quando si esegue una query sulla vista `INFORMATION_SCHEMA.COLUMNS`, come illustrato nell'esempio seguente.
  
```sql
SELECT TABLE_NAME, COLUMN_NAME,  
    COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME),  
    COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
```  

Se si applica un trigger a una colonna, il valore `COLUMNS_UPDATED` verrà restituito come `true` o `1`, anche se il valore della colonna rimane invariato. Si tratta di un comportamento predefinito e il trigger deve implementare la logica di business che determina se l'operazione di inserimento/aggiornamento/eliminazione è consentita o meno. 
  
## <a name="column-sets"></a>Set di colonne
Quando un set di colonne è definito in una tabella, il comportamento della funzione `COLUMNS_UPDATED` è il seguente:
-   Quando una colonna appartenente al set di colonne viene aggiornata in modo esplicito, il bit corrispondente per tale colonna e il bit del set di colonne vengono impostati su 1.  
-   Quando un set di colonne viene aggiornato in modo esplicito, il bit del set di colonne e i bit per tutte le colonne di tipo sparse della tabella vengono impostati su 1.  
-   Per le operazioni di inserimento, tutti i bit vengono impostati su 1.  
  
     Poiché le modifiche a un set di colonne determinano l'impostazione dei bit di tutte le colonne del set su 1, anche le colonne di un set che non erano state cambiate sembreranno modificate. Per altre informazioni sui set di colonne, vedere [Usare set di colonne](../../relational-databases/tables/use-column-sets.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-columns_updated-to-test-the-first-eight-columns-of-a-table"></a>R. Utilizzo di COLUMNS_UPDATED per controllare le prime 8 colonne di una tabella  
Nell'esempio seguente vengono create due tabelle: `employeeData` e `auditEmployeeData`. La tabella `employeeData` include informazioni riservate sulle retribuzioni dei dipendenti e può essere modificata dai membri dell'ufficio del personale. Se il numero di codice fiscale, lo stipendio annuale o il numero di conto corrente bancario relativi a un dipendente cambiano, nella tabella di controllo `auditEmployeeData` viene generato e inserito un record di controllo.
  
Con la funzione `COLUMNS_UPDATED()`, è possibile verificare rapidamente eventuali modifiche apportate a colonne contenenti informazioni riservate sui dipendenti. Se si usa `COLUMNS_UPDATED()` ciò funziona solo quando si tenta di rilevare le modifiche alle prime otto colonne della tabella.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'employeeData')  
   DROP TABLE employeeData;  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'auditEmployeeData')  
   DROP TABLE auditEmployeeData;  
GO  
CREATE TABLE dbo.employeeData (  
   emp_id INT NOT NULL PRIMARY KEY,  
   emp_bankAccountNumber CHAR (10) NOT NULL,  
   emp_salary INT NOT NULL,  
   emp_SSN CHAR (11) NOT NULL,  
   emp_lname NCHAR (32) NOT NULL,  
   emp_fname NCHAR (32) NOT NULL,  
   emp_manager INT NOT NULL  
   );  
GO  
CREATE TABLE dbo.auditEmployeeData (  
   audit_log_id uniqueidentifier DEFAULT NEWID() PRIMARY KEY,  
   audit_log_type CHAR (3) NOT NULL,  
   audit_emp_id INT NOT NULL,  
   audit_emp_bankAccountNumber CHAR (10) NULL,  
   audit_emp_salary INT NULL,  
   audit_emp_SSN CHAR (11) NULL,  
   audit_user sysname DEFAULT SUSER_SNAME(),  
   audit_changed DATETIME DEFAULT GETDATE()  
   );  
GO  
CREATE TRIGGER dbo.updEmployeeData   
ON dbo.employeeData   
AFTER UPDATE AS  
/* Check whether columns 2, 3 or 4 have been updated. If any or all  
columns 2, 3 or 4 have been changed, create an audit record.
The bitmask is: power(2, (2-1)) + power(2, (3-1)) + power(2, (4-1)) = 14.
This bitmask translates into base_10 as: 2 + 4 + 8 = 14.
To test whether all columns 2, 3, and 4 are updated, use = 14 instead of > 0  
(below). */
  
   IF (COLUMNS_UPDATED() & 14) > 0  
/* Use IF (COLUMNS_UPDATED() & 14) = 14 to see whether all columns 2, 3,   
and 4 are updated. */  
      BEGIN  
-- Audit OLD record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'OLD',   
            del.emp_id,  
            del.emp_bankAccountNumber,  
            del.emp_salary,  
            del.emp_SSN  
         FROM deleted del;  
  
-- Audit NEW record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'NEW',  
            ins.emp_id,  
            ins.emp_bankAccountNumber,  
            ins.emp_salary,  
            ins.emp_SSN  
         FROM inserted ins;  
   END;  
GO  
  
/* Inserting a new employee does not cause the UPDATE trigger to fire. */  
INSERT INTO employeeData  
   VALUES ( 101, 'USA-987-01', 23000, 'R-M53550M', N'Mendel', N'Roland', 32);  
GO  
  
/* Updating the employee record for employee number 101 to change the   
salary to 51000 causes the UPDATE trigger to fire and an audit trail to   
be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_salary = 51000  
   WHERE emp_id = 101;  
GO  
SELECT * FROM auditEmployeeData;  
GO  
  
/* Updating the employee record for employee number 101 to change both   
the bank account number and social security number (SSN) causes the   
UPDATE trigger to fire and an audit trail to be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_bankAccountNumber = '133146A0', emp_SSN = 'R-M53550M'  
   WHERE emp_id = 101;  
GO  
SELECT * FROM dbo.auditEmployeeData;  
  
GO  
```  
  
### <a name="b-using-columns_updated-to-test-more-than-eight-columns"></a>B. Utilizzo di COLUMNS_UPDATED per controllare più di 8 colonne  
Per controllare gli aggiornamenti eseguiti su colonne diverse dalle prime otto colonne di una tabella, usare la funzione `SUBSTRING` per controllare il bit corretto restituito da `COLUMNS_UPDATED`. Nell'esempio seguente vengono controllati gli aggiornamenti relativi alle colonne `3`, `5` e `9` della tabella `AdventureWorks2012.Person.Person`.
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.uContact2', N'TR') IS NOT NULL  
    DROP TRIGGER Person.uContact2;  
GO  
CREATE TRIGGER Person.uContact2 ON Person.Person  
AFTER UPDATE AS  
    IF ( (SUBSTRING(COLUMNS_UPDATED(), 1, 1) & 20 = 20)   
        AND (SUBSTRING(COLUMNS_UPDATED(), 2, 1) & 1 = 1) )   
    PRINT 'Columns 3, 5 and 9 updated';  
GO  
  
UPDATE Person.Person   
   SET NameStyle = NameStyle,  
      FirstName=FirstName,  
      EmailPromotion=EmailPromotion;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche
[Operatori bit per bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
[CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
[UPDATE&#40;&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/update-trigger-functions-transact-sql.md)
  
  
