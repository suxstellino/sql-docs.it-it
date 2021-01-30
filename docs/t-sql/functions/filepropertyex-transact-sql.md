---
description: FILEPROPERTYEX (Transact-SQL)
title: FILEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEPROPERTYEX_TSQL
- FILEPROPERTYEX
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file properties
- displaying file properties
- file properties [SQL Server]
- FILEPROPERTYEX function
- file names [SQL Server], FILEPROPERTYEX
author: stevestein
ms.author: sstein
ms.openlocfilehash: 7335b9e78a8b0cdce4ca341a8de62254003e2e1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209580"
---
# <a name="filepropertyex-transact-sql"></a>FILEPROPERTYEX (Transact-SQL)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce il valore della proprietà file estesa specificata quando vengono indicati il nome di un file nel database corrente e il nome di una proprietà. Restituisce NULL per i file che non sono nel database corrente o per le proprietà file estese che non esistono. Attualmente le proprietà file estese si applicano solo ai database nell'archivio BLOB di Azure.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
FILEPROPERTYEX ( name , property )  
```  
  
## <a name="arguments"></a>Argomenti  
 *nome*  
 Espressione contenente il nome del file associato al database corrente per cui si desidera restituire le informazioni sulla proprietà. *file_name* è di tipo **nchar(128)**.  
  
 *property*  
 Espressione contenente il nome della proprietà del file da restituire. *property* è di tipo **varchar(128)**. I valori possibili sono riportati di seguito.  


  
|valore|Descrizione|
|-----------|-----------------|  
|**BlobTier**|Livello del BLOB di pagine di Azure di destinazione. Si applica solo ai database Standard e GeneralPurpose che usano l'archivio BLOB di pagine di Azure.|
|**AccountType**|Tipo di account di archiviazione che indica se si tratta di archiviazione BLOB o di archiviazione dei file e se è Premium o Standard.|
|**IsInferredTier**|Indica se il livello è implicito (derivato), che può aumentare con le dimensioni dei dati, o esplicito (fisso).|
|**IsPageBlob**|Indica se il BLOB di destinazione è un BLOB di pagine.|
  
## <a name="return-types"></a>Tipi restituiti  
 **sql_variant**  
  
## <a name="remarks"></a>Osservazioni  
 *file_name* corrisponde alla colonna **name** nella vista del catalogo **sys.master_files** o **sys.database_files**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituita l'impostazione dei file di database:
```sql
SELECT s.file_id,
       s.type_desc,
       s.name,
       FILEPROPERTYEX(s.name, 'BlobTier') AS BlobTier,
       FILEPROPERTYEX(s.name, 'AccountType') AS AccountType,
       FILEPROPERTYEX(s.name, 'IsInferredTier') AS IsInferredTier,
       FILEPROPERTYEX(s.name, 'IsPageBlob') AS IsPageBlob
FROM sys.database_files AS s
WHERE s.type_desc IN ('ROWS', 'LOG');
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
file_id  type_desc  name  BlobTier  AccountType  IsInferredTier  IsPageBlob
--------------------------------------------------------------------------------------
1     ROWS      data_0  P30  PremiumBlobStorage  0   1
2     LOG       log     P30  PremiumBlobStorage  0   1

(2 rows affected)
```  
  
## <a name="see-also"></a>Vedere anche  
 [FILEGROUPPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/filegroupproperty-transact-sql.md)   
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
