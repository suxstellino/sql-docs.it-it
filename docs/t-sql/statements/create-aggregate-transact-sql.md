---
description: CREATE AGGREGATE (Transact-SQL)
title: CREATE AGGREGATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_AGGREGATE_TSQL
- CREATE AGGREGATE
- AGGREGATE_TSQL
- AGGREGATE
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE AGGREGATE statement
- aggregate functions [SQL Server], user-defined
- user-defined functions [CLR integration]
ms.assetid: 62eebc19-9f15-4245-94fa-b3fcd64a9d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 773c2010cdefa2455e0800ed0370cbc118c2dab9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098431"
---
# <a name="create-aggregate-transact-sql"></a>CREATE AGGREGATE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una funzione di aggregazione definita dall'utente la cui implementazione è definita in una classe di un assembly in [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Affinché in [!INCLUDE[ssDE](../../includes/ssde-md.md)] la funzione di aggregazione venga associata alla relativa implementazione, l'assembly [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] che include l'implementazione deve innanzitutto essere caricato in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite un'istruzione CREATE ASSEMBLY.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
CREATE AGGREGATE [ schema_name . ] aggregate_name  
        (@param_name <input_sqltype>   
        [ ,...n ] )  
RETURNS <return_sqltype>  
EXTERNAL NAME assembly_name [ .class_name ]  
  
<input_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
<return_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome dello schema a cui appartiene la funzione di aggregazione definita dall'utente.  
  
 *aggregate_name*  
 Nome della funzione di aggregazione che si desidera creare.  
  
 **@** _param_name_  
 Uno o più parametri della funzione di aggregazione definita dall'utente. Il valore di un parametro deve essere fornito dall'utente quando la funzione di aggregazione viene eseguita. Specificare un nome di parametro usando come primo carattere il simbolo di chiocciola ( **@** ). Il nome di parametro deve essere conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). I parametri sono locali rispetto alla funzione.  
  
 *system_scalar_type*  
 Qualsiasi tipo di dati scalare di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la memorizzazione del valore del parametro di input o del valore restituito. È possibile usare tutti i tipi di dati scalari come parametri di una funzione di aggregazione definita dall'utente, eccetto **text**, **ntext** e **image**. Non è possibile specificare tipi di dati non scalari, come **cursor** e **table**.  
  
 *udt_schema_name*  
 Nome dello schema a cui appartiene il tipo CLR definito dall'utente. Se viene omesso, [!INCLUDE[ssDE](../../includes/ssde-md.md)] fa riferimento a *udt_type_name* nell'ordine seguente:  
  
-   Spazio dei nomi del tipo SQL nativo.  
  
-   Schema predefinito dell'utente corrente nel database corrente.  
  
-   Schema **dbo** nel database corrente.  
  
 *udt_type_name*  
 Nome di un tipo CLR definito dall'utente già creato nel database corrente. Se *udt_schema_name* viene omesso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presume che il tipo appartenga allo schema dell'utente corrente.  
  
 *assembly_name* [ **.** _class_name_ ]  
 Specifica l'assembly da associare alla funzione di aggregazione definita dall'utente e, facoltativamente, il nome dello schema a cui l'assembly appartiene e il nome della classe nell'assembly che implementa la funzione di aggregazione definita dall'utente. È necessario che l'assembly sia già stato creato nel database tramite un'istruzione CREATE ASSEMBLY. *class_name* deve essere un identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido e corrispondere al nome di una classe esistente nell'assembly. *class_name* può essere un nome qualificato con lo spazio dei nomi se il linguaggio di programmazione usato per scrivere la classe usa spazi dei nomi, ad esempio C#. Se *class_name* viene omesso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presume che equivalga ad *aggregate_name*.  
  
## <a name="remarks"></a>Commenti  
 Per impostazione predefinita, la capacità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di eseguire il codice CLR è disattivata. È possibile creare, modificare ed eliminare gli oggetti di database che fanno riferimento a moduli di codice gestito, ma il codice in tali moduli verrà eseguito nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo se l'opzione[clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) viene abilitata tramite la stored procedure [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 La classe dell'assembly a cui viene fatto riferimento in *assembly_name* e i relativi metodi devono soddisfare tutti i requisiti di implementazione di una funzione di aggregazione definita dall'utente in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Aggregazioni CLR definite dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione CREATE AGGREGATE e anche dell'autorizzazione REFERENCES per l'assembly specificato nella clausola EXTERNAL NAME.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente si presuppone che sia stata compilata l'applicazione di esempio StringUtilities.csproj. Per altre informazioni, vedere [Esempio String Utility Functions](/previous-versions/sql/sql-server-2016/ff878119(v=sql.130)).  
  
 Nell'esempio viene creata la funzione di aggregazione `Concatenate`, ma prima che venga effettivamente creata la funzione di aggregazione, l'assembly `StringUtilities.dll` viene registrato nel database locale.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SamplesPath nvarchar(1024)  
-- You may have to modify the value of the this variable if you have  
--installed the sample some location other than the default location.  
  
SELECT @SamplesPath = REPLACE(physical_name, 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf', 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\')   
     FROM master.sys.database_files   
     WHERE name = 'master';  
  
CREATE ASSEMBLY StringUtilities FROM @SamplesPath + 'StringUtilities\CS\StringUtilities\bin\debug\StringUtilities.dll'  
WITH PERMISSION_SET=SAFE;  
GO  
  
CREATE AGGREGATE Concatenate(@input nvarchar(4000))  
RETURNS nvarchar(4000)  
EXTERNAL NAME [StringUtilities].[Microsoft.Samples.SqlServer.Concatenate];  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DROP AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-aggregate-transact-sql.md)  
  
