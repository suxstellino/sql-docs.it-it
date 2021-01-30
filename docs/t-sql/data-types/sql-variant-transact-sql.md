---
description: sql_variant (Transact-SQL)
title: sql_variant (Transact-SQL)
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql_variant
- sql_variant_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sql_variant comparisons
- storing data [SQL Server], sql_variant
- sql_variant data type
- storage [SQL Server], sql_variant
ms.assetid: 01229779-8bc1-4c7d-890a-8246d4899250
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 3787a49e6488a2b43ba8ef6e7b67c2a94b6347b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179693"
---
# <a name="sql_variant-transact-sql"></a>sql_variant (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Tipo di dati per l'archiviazione di valori per vari tipi di dati supportati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
sql_variant  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni  
**sql_variant** può essere usato in colonne, parametri, variabili e in valori restituiti di funzioni definite dall'utente. **sql_variant** consente a questi oggetti di database di supportare valori di altri tipi di dati.
  
Una colonna di tipo **sql_variant** può includere righe con tipi di dati diversi. Ad esempio, una colonna definita come **sql_variant** può archiviare valori **int**, **binary**, e **char**.
  
La lunghezza massima supportata da **sql_variant** è pari a 8016 byte. La lunghezza include sia le informazioni sul tipo di base che il valore del tipo di base. La lunghezza massima del valore effettivo del tipo di base è di 8.000 byte.
  
Per poter usare il tipo di dati **sql_variant** in operazioni quali l'addizione e la sottrazione, è prima necessario eseguirne il casting al valore del tipo di dati di base corrispondente.
  
È possibile assegnare un valore predefinito a **sql_variant**. A questo tipo di dati è inoltre possibile assegnare NULL come valore sottostante, ma in questo caso ai valori NULL non verrà associato alcun tipo di base. **sql_variant** non può avere un altro valore **sql_variant** come tipo di base.
  
Una chiave univoca, primaria o esterna può includere colonne di tipo **sql_variant**, ma la lunghezza totale dei valori di dati che compongono la chiave di una riga specifica non deve superare la lunghezza massima di un indice, ovvero 900 byte.
  
Una tabella può contenere un numero qualsiasi di colonne di tipo **sql_variant**.
  
Non è possibile usare il tipo di dati **sql_variant** in CONTAINSTABLE e FREETEXTTABLE.
  
ODBC non offre il supporto completo di **sql_variant**. Quando si usa il provider Microsoft OLE DB per ODBC (MSDASQL), pertanto, le query eseguite su colonne di tipo **sql_variant** restituiscono dati binari. Ad esempio, la stringa di caratteri "PS2091" di una colonna di tipo **sql_variant** viene restituita come 0x505332303931.
  
## <a name="comparing-sql_variant-values"></a>Confronto di valori sql_variant  
Il tipo di dati **sql_variant** è incluso nella parte iniziale dell'elenco della gerarchia dei tipi di dati per la conversione. Per l'esecuzione di confronti di valori **sql_variant**, la gerarchia dei tipi di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene suddivisa in gruppi di tipi di dati.
  
|Gerarchia dei tipi di dati|Gruppo di tipi di dati|  
|---|---|
|**sql_variant**|sql_variant |  
|**datetime2**|Date e Time|  
|**datetimeoffset**|Date e Time|  
|**datetime**|Date e Time|  
|**smalldatetime**|Date e Time|  
|**date**|Date e Time|  
|**time**|Date e Time|  
|**float**|Valori numerici approssimati|  
|**real**|Valori numerici approssimati|  
|**decimal**|Valori numerici esatti|  
|**money**|Valori numerici esatti|  
|**smallmoney**|Valori numerici esatti|  
|**bigint**|Valori numerici esatti|  
|**int**|Valori numerici esatti|  
|**smallint**|Valori numerici esatti|  
|**tinyint**|Valori numerici esatti|  
|**bit**|Valori numerici esatti|  
|**nvarchar**|Unicode|  
|**nchar**|Unicode|  
|**varchar**|Unicode|  
|**char**|Unicode|  
|**varbinary**|Binary|  
|**binary**|Binary|  
|**uniqueidentifier**|Uniqueidentifier |  
  
Per i confronti **sql_variant** vengono applicate le regole seguenti:
-   Nei confronti tra valori **sql_variant** con tipi di dati di base diversi appartenenti a gruppi di tipi di dati diversi, il valore appartenente al gruppo di tipi di dati che occupa un livello più alto nella gerarchia viene considerato il valore maggiore.  
-   Nei confronti di valori **sql_variant** con tipi di dati di base diversi appartenenti allo stesso gruppo, il valore del tipo di dati di base che appartiene a un gruppo di livello inferiore nella gerarchia viene convertito in modo implicito nell'altro tipo di dati, dopodiché viene eseguito il confronto.  
-   Quando vengono confrontati i valori **sql_variant** dei tipi di dati **char**, **varchar**, **nchar** o **nvarchar**, vengono messe a confronto per prime le regole di confronto in base ai criteri seguenti: LCID, versione LCID, flag di confronto e ID di ordinamento. Per ognuno di questi criteri il confronto viene eseguito con valori interi e nell'ordine elencato. Se tutti questi criteri sono uguali, i valori effettivi della stringa vengono confrontati in base alle regole di confronto.  
  
## <a name="converting-sql_variant-data"></a>Conversione di dati sql_variant  
Nel caso del tipo di dati **sql_variant**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta la conversione implicita in **sql_variant** degli oggetti con tipi di dati diversi. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta tuttavia le conversioni implicite dal tipo di dati **sql_variant** in oggetti con un tipo di dati diverso.
  
## <a name="restrictions"></a>Restrizioni

Di seguito sono elencati i tipi di valori che non è possibile archiviare con **sql_variant**:

- **datetimeoffset**<sup>1</sup>
- **geography**
- **geometry**
- **hierarchyid**
- **image**
- **ntext**
- **nvarchar(max)**
- **rowversion** (**timestamp**)
- **text**
- **ntext**
- **varbinary(max)**
- **sql_variant**
- Tipi definiti dall'utente
- **xml**

<sup>1</sup> SQL Server 2012 e versioni successive non limitano **datetimeoffset**.

## <a name="examples"></a>Esempi  

### <a name="a-using-a-sql_variant-in-a-table"></a>R. Uso di un tipo sql_variant in una tabella  
 Nell'esempio seguente viene creata una tabella con tipi di dati sql_variant. In seguito vengono recuperate le informazioni di `SQL_VARIANT_PROPERTY` relative al valore `colA``46279.1`, dove `colB` =`1689`, partendo dal presupposto che `tableA` include `colA` di tipo `sql_variant` e `colB`.  
  
```sql    
CREATE TABLE tableA(colA sql_variant, colB INT)  
INSERT INTO tableA values ( CAST(46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE     colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Si noti che tutti e tre i valori sono di tipo **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. Uso di un tipo sql_variant come variabile   
 Nell'esempio seguente viene creata una variabile con il tipo di dati sql_variant e in seguito vengono recuperate le informazioni di `SQL_VARIANT_PROPERTY` su una variabile denominata @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```    


## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[SQL_VARIANT_PROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sql-variant-property-transact-sql.md)
  
  
