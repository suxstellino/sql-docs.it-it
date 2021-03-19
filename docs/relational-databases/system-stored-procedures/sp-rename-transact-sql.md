---
description: sp_rename (Transact-SQL)
title: sp_rename (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_rename_TSQL
- sp_rename
dev_langs:
- TSQL
helpviewer_keywords:
- renaming indexes
- renaming columns
- sp_rename
- renaming tables
ms.assetid: bc3548f0-143f-404e-a2e9-0a15960fc8ed
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 2e24d74b8c2d2d3e661ab5169ae800e99af42f68
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610841"
---
# <a name="sp_rename-transact-sql"></a>sp_rename (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Consente di modificare il nome di un oggetto creato dall'utente nel database corrente. Questo oggetto può essere una tabella, un indice, una colonna, un tipo di dati alias o un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] tipo definito dall'utente Common Language Runtime (CLR).  
  
> [!NOTE]
> In [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] sp_rename è in **Anteprima** e può essere utilizzato solo per rinominare una colonna in un oggetto utente nello schema **dbo** .

> [!CAUTION]  
>  La modifica di una parte del nome di un oggetto potrebbe compromettere il funzionamento di script e stored procedure. È consigliabile evitare di utilizzare questa istruzione per rinominare stored procedure, trigger, funzioni definite dall'utente o viste. In alternativa, eliminare l'oggetto e ricrearlo con il nuovo nome.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
-- Transact-SQL Syntax for sp_rename in SQL Server and Azure SQL Database
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    [ , [ @objtype = ] 'object_type' ]   
```  

```sql  
-- Transact-SQL Syntax for sp_rename (preview) in Azure Synapse Analytics
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    , [ @objtype = ] 'COLUMN'   
``` 
  
## <a name="arguments"></a>Argomenti  
 [ @objname =]'*object_name*'  
 Nome corrente completo o non qualificato dell'oggetto utente o del tipo di dati. Se l'oggetto da rinominare è una colonna in una tabella, *object_name* deve essere nel formato *Table. Column* o *schema. Table. Column*. Se l'oggetto da rinominare è un indice, *object_name* deve essere nel formato *Table. index* o *schema. Table. index*. Se l'oggetto da rinominare è un vincolo, *object_name* deve essere nel formato *schema. vincolo*.  
  
 Le virgolette sono necessarie solo se viene specificato un nome di oggetto completo. Nel caso di un nome completo, ovvero contenente un nome di database, il nome del database deve corrispondere a quello del database corrente. *object_name* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito.  
  
 [ @newname =]'*new_name*'  
 Nuovo nome dell'oggetto specificato. *new_name* deve essere un nome costituito da una parte e deve rispettare le regole per gli identificatori. *newname* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
> [!NOTE]  
>  I nomi di trigger non possono iniziare con # o ##.  
  
 [ @objtype =]'*object_type*'  
 Tipo dell'oggetto da rinominare. *object_type* è di tipo **varchar (13)** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|COLUMN|Colonna da rinominare.|  
|DATABASE|Database definito dall'utente. Quando si rinomina un database è necessario specificare questo tipo di oggetto.|  
|INDEX|Indice definito dall'utente. Se si rinomina un indice con statistiche, vengono automaticamente rinominate anche le statistiche.|  
|OBJECT|Elemento di un tipo rilevato in [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). È ad esempio possibile utilizzare OBJECT per rinominare oggetti che includono vincoli (CHECK, FOREIGN KEY, PRIMARY/UNIQUE KEY), tabelle utente e regole.|  
|STATISTICS|**SI APPLICA A**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Statistiche create in modo esplicito da un utente o in modo implicito con un indice. Se si rinominano le statistiche di un indice, viene automaticamente rinominato anche l'indice.|  
|USERDATATYPE|[Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) aggiunti mediante l'esecuzione di [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md) o [sp_addtype](../../relational-databases/system-stored-procedures/sp-addtype-transact-sql.md).|  
  
[ @objtype =]'*Column*' **si applica a**: Azure sinapsi Analytics  
In sp_rename (Preview) per [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] , *Column* è un parametro obbligatorio che specifica che il tipo di oggetto da rinominare è una colonna. È di tipo **varchar (13)** e non prevede alcun valore predefinito e deve essere sempre incluso nell'istruzione sp_rename (Preview). Una colonna può essere rinominata solo se si tratta di una colonna non di distribuzione.

## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo)  
  
## <a name="remarks"></a>Commenti  
**Si applica a** SQL Server (tutte le versioni supportate) e il database SQL di Azure  
 La stored procedure sp_rename rinomina automaticamente l'indice associato ogni volta che viene rinominato un vincolo PRIMARY KEY o UNIQUE. Se un indice rinominato è associato a un vincolo PRIMARY KEY, quando si esegue sp_rename viene rinominato automaticamente anche il vincolo PRIMARY KEY.  

**Si applica a** SQL Server (tutte le versioni supportate) e il database SQL di Azure  
 La stored procedure sp_rename può essere utilizzata per rinominare indici XML primari e secondari.  
  
**Si applica a** SQL Server (tutte le versioni supportate) e il database SQL di Azure  
 La ridenominazione di un stored procedure, di una funzione, di una vista o di un trigger non comporterà la modifica del nome dell'oggetto corrispondente nella colonna di definizione della vista del catalogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) oppure ottenuta utilizzando la funzione incorporata [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) . È pertanto consigliabile evitare di utilizzare sp_rename per rinominare questi tipi di oggetto. In alternativa, eliminare e ricreare l'oggetto con il nuovo nome.  

**Si applica a** SQL Server (tutte le versioni supportate), il database SQL di Azure e Azure sinapsi Analytics  
 La ridenominazione di un oggetto, ad esempio una tabella o una colonna, non aggiorna automaticamente i riferimenti a tale oggetto ed è necessario modificare manualmente tutti gli oggetti che fanno riferimento all'oggetto rinominato. Se, ad esempio, si rinomina una colonna di una tabella a cui viene fatto riferimento all'interno di un trigger, è necessario modificare il trigger in base al nuovo nome della colonna. Usare [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) per elencare le dipendenze dall'oggetto prima di rinominarlo.  

**Si applica a** SQL Server (tutte le versioni supportate), il database SQL di Azure e Azure sinapsi Analytics  
 È possibile modificare il nome di un oggetto o un tipo di dati solo nel database corrente. I nomi della maggior parte dei tipi di dati e degli oggetti di sistema non sono modificabili.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per rinominare oggetti, colonne e indici, è necessario disporre dell'autorizzazione ALTER per l'oggetto. Per rinominare tipi definiti dall'utente, è necessario disporre dell'autorizzazione CONTROL per il tipo. Per rinominare un database, è richiesta l'appartenenza al ruolo predefinito del server sysadmin o dbcreator.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-renaming-a-table"></a>R. Ridenominazione di una tabella  
 Nell'esempio seguente la tabella `SalesTerritory` viene rinominata in `SalesTerr` nello schema `Sales` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
GO  
```  
  
### <a name="b-renaming-a-column"></a>B. Ridenominazione di una colonna  
 Nell'esempio seguente la colonna della tabella viene rinominata in `TerritoryID` `SalesTerritory` `TerrID` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';  
GO  
```  
  
### <a name="c-renaming-an-index"></a>C. Ridenominazione di un indice  
 Nell'esempio seguente l'indice `IX_ProductVendor_VendorID` viene rinominato in `IX_VendorID`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Purchasing.ProductVendor.IX_ProductVendor_VendorID', N'IX_VendorID', N'INDEX';  
GO  
```  
  
### <a name="d-renaming-an-alias-data-type"></a>D. Ridenominazione di un tipo di dati alias  
 Nell'esempio seguente il tipo di dati alias `Phone` viene rinominato in `Telephone`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Phone', N'Telephone', N'USERDATATYPE';  
GO  
```  
  
### <a name="e-renaming-constraints"></a>E. Ridenominazione dei vincoli  
 Negli esempi seguenti vengono rinominati un vincolo PRIMARY KEY, un vincolo CHECK e un vincolo FOREIGN KEY. Quando si rinomina un vincolo, è necessario specificare lo schema di appartenenza del vincolo.  
  
```sql  
USE AdventureWorks2012;   
GO  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');   
GO  
  
-- Rename the primary key constraint.  
sp_rename 'HumanResources.PK_Employee_BusinessEntityID', 'PK_EmployeeID';  
GO  
  
-- Rename a check constraint.  
sp_rename 'HumanResources.CK_Employee_BirthDate', 'CK_BirthDate';  
GO  
  
-- Rename a foreign key constraint.  
sp_rename 'HumanResources.FK_Employee_Person_BusinessEntityID', 'FK_EmployeeID';  
  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');  
  
```  
  
```  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_Person_BusinessEntityID   HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_BusinessEntityID          HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_Employee_BirthDate                 HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_ID                        HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_ID                        HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_BirthDate                          HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
```  
  
### <a name="f-renaming-statistics"></a>F. Ridenominazione delle statistiche  
 Nell'esempio seguente viene creato un oggetto statistiche denominato contactMail1, quindi viene rinominata la statistica in NewContact tramite sp_rename. Quando si rinominano le statistiche, l'oggetto deve essere specificato nel formato schema.table.statistics_name.  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
  
sp_rename 'Person.Person.ContactMail1', 'NewContact','Statistics';  
  
```  

## <a name="examples-sssdwfull"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]
### <a name="g-renaming-a-column"></a>G. Ridenominazione di una colonna  
 Nell'esempio seguente la colonna della tabella viene rinominata in `c1` `table1` `col1` . 

> [!NOTE]
> Questa [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] funzionalità è ancora in anteprima ed è attualmente disponibile solo per gli oggetti nello schema **dbo** . 
  
```sql  
CREATE TABLE table1 (c1 INT, c2 INT);
EXEC sp_rename 'table1.c1', 'col1', 'COLUMN';
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
