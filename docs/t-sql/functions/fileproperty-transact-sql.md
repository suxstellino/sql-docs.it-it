---
description: FILEPROPERTY (Transact-SQL)
title: FILEPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEPROPERTY_TSQL
- FILEPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file properties
- names [SQL Server], files
- displaying file properties
- file properties [SQL Server]
- FILEPROPERTY function
- file names [SQL Server], FILEPROPERTY
ms.assetid: b82244ed-d623-431f-aa06-8017349d847f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0cbbef3b704a8ec174fd9c3e5c1cbf5938a5a4ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183527"
---
# <a name="fileproperty-transact-sql"></a>FILEPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il valore di una proprietà di un file quando vengono indicati il nome di un file nel database corrente e il nome di una proprietà. Restituisce NULL per i file che non sono nel database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
FILEPROPERTY ( file_name , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *file_name*  
 Espressione contenente il nome del file associato al database corrente per cui si desidera restituire le informazioni sulla proprietà. *file_name* è di tipo **nchar(128)**.  
  
 *property*  
 Espressione contenente il nome della proprietà del file da restituire. *property* è di tipo **varchar(128)**. I valori possibili sono riportati di seguito.  
  
|Valore|Descrizione|Valore restituito|  
|-----------|-----------------|--------------------|  
|**IsReadOnly**|Filegroup di sola lettura.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Input non valido.|  
|**IsPrimaryFile**|File primario.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Input non valido.|  
|**IsLogFile**|File di log.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Input non valido.|  
|**SpaceUsed**|Quantità di spazio utilizzata dal file specificato.|Numero di pagine allocate nel file|  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 *file_name* corrisponde alla colonna **name** nella vista del catalogo **sys.master_files** o **sys.database_files**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituita l'impostazione della proprietà `IsPrimaryFile` per il file `AdventureWorks_Data` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT FILEPROPERTY('AdventureWorks2012_Data', 'IsPrimaryFile')AS [Primary File];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Primary File   
-------------  
1  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [FILEGROUPPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/filegroupproperty-transact-sql.md)   
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
