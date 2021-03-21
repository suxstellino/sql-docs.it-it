---
description: SQL_VARIANT_PROPERTY (Transact-SQL)
title: SQL_VARIANT_PROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_VARIANT_PROPERTY_TSQL
- SQL_VARIANT_PROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- SQL_VARIANT_PROPERTY function
- sql_variant data type
ms.assetid: 50e5c1d9-4e95-4ed0-9c92-435c872a399e
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24134065588787a367b7a975c36b3be5631483cd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750511"
---
# <a name="sql_variant_property-transact-sql"></a>SQL_VARIANT_PROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il tipo di dati di base e altre informazioni su un valore di tipo **sql_variant**.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
SQL_VARIANT_PROPERTY ( expression , property )  
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Espressione di tipo **sql_variant**.  
  
 *property*  
 Contiene il nome della proprietà **sql_variant** per la quale è necessario specificare informazioni. *property* è **varchar(** 128 **)** . I valori possibili sono i seguenti:  
  
|valore|Descrizione|Tipo di base di sql_variant restituito|  
|-----------|-----------------|----------------------------------------|  
|**BaseType**|Tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio:<br /><br /> **bigint**<br /><br /> **binary**<br /><br /> **bit**<br /><br /> **char**<br /><br /> **date**<br /><br /> **datetime**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **int**<br /><br /> **money**<br /><br /> **nchar**<br /><br /> **numeric**<br /><br /> **nvarchar**<br /><br /> **real**<br /><br /> **smalldatetime**<br /><br /> **smallint**<br /><br /> **smallmoney**<br /><br /> **time**<br /><br /> **tinyint**<br /><br /> **uniqueidentifier**<br /><br /> **varbinary**<br /><br /> **varchar**|**sysname**<br /><br /> NULL = Input non valido.|  
|**Precisione**|Numero di cifre del tipo di dati numerici di base:<br /><br /> **date** = 10<br /><br /> **datetime** = 23<br /><br /> **datetime2** = 27<br /><br /> **datetime2** (s) = 19 quando s = 0, altrimenti s + 20<br /><br /> **datetimeoffset** = 34<br /><br /> **datetimeoffset** (s) = 26 quando s = 0, altrimenti s + 27<br /><br /> **smalldatetime** = 16<br /><br /> **time** = 16<br /><br /> **time** (s) = 8 quando s = 0, altrimenti s + 9<br /><br /> **float** = 53<br /><br /> **real** = 24<br /><br /> **decimal** e **numeric** = 18<br /><br /> **decimal** (p,s) e **numeric** (p,s) = p<br /><br /> **money** = 19<br /><br /> **smallmoney** = 10<br /><br /> **bigint** = 19<br /><br /> **int** = 10<br /><br /> **smallint** = 5<br /><br /> **tinyint** = 3<br /><br /> **bit** = 1<br /><br /> Tutti gli altri tipi = 0|**int**<br /><br /> NULL = Input non valido.|  
|**Ridimensionare**|Numero di cifre a destra del separatore decimale con tipo di base numerico:<br /><br /> **decimal** e **numeric** = 0<br /><br /> **decimal** (p,s) e **numeric** (p,s) = s<br /><br /> **money** e **smallmoney** = 4<br /><br /> **datetime** = 3<br /><br /> **datetime2** = 7<br /><br /> **datetime2** (s) = s (0 - 7)<br /><br /> **datetimeoffset** = 7<br /><br /> **datetimeoffset** (s) = s (0 - 7)<br /><br /> **time** = 7<br /><br /> **time** (s) = s (0 - 7)<br /><br /> Tutti gli altri tipi = 0|**int**<br /><br /> NULL = Input non valido.|  
|**TotalBytes**|Numero di byte necessari per l'archiviazione sia dei metadati che dei dati del valore. Questo valore risulta utile per verificare le dimensioni massime dei dati in una colonna **sql_variant**. Se il valore è maggiore di 900, la creazione dell'indice genera un errore.|**int**<br /><br /> NULL = Input non valido.|  
|**Regole di confronto**|Regole di confronto del valore di tipo **sql_variant** specifico.|**sysname**<br /><br /> NULL = Input non valido.|  
|**MaxLength**|Lunghezza massima del tipo di dati espressa in byte. Ad esempio, **MaxLength** di **nvarchar(** 50 **)** è 100, **MaxLength** di **int** è 4.|**int**<br /><br /> NULL = Input non valido.|  
  
## <a name="return-types"></a>Tipi restituiti  
 **sql_variant**  
  
## <a name="examples"></a>Esempi  
### <a name="a-using-a-sql_variant-in-a-table"></a>R. Uso di un tipo sql_variant in una tabella  
 Nell'esempio seguente vengono recuperate le informazioni di `SQL_VARIANT_PROPERTY` relative al valore `colA``46279.1`, dove `colB` =`1689`, dato che `tableA` include `colA` di tipo `sql_variant` e `colB`.  
  
```sql    
CREATE   TABLE tableA(colA sql_variant, colB int)  
INSERT INTO tableA values ( cast (46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE      colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Si noti che tutti e tre i valori sono di tipo **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. Uso di un tipo sql_variant come variabile   
 Nell'esempio seguente vengono recuperate informazioni `SQL_VARIANT_PROPERTY` su una variabile con nome @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
  

