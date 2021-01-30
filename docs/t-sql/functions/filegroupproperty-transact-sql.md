---
description: FILEGROUPPROPERTY (Transact-SQL)
title: FILEGROUPPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEGROUPPROPERTY_TSQL
- FILEGROUPPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], property values
- FILEGROUPPROPERTY function
- viewing filegroup properties
- displaying filegroup properties
ms.assetid: b3a930e6-df05-4034-929c-f681f5f6fc6e
author: cawrites
ms.author: chadam
ms.openlocfilehash: cdc8e66d73e7296d269edd420fde4709c11d41f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161864"
---
# <a name="filegroupproperty-transact-sql"></a>FILEGROUPPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questa funzione restituisce il valore della proprietà filegroup per un valore specificato di nome e filegroup.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
FILEGROUPPROPERTY ( filegroup_name, property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *filegroup_name*  
Espressione di tipo **sysname** che rappresenta il nome del filegroup per il quale `FILEGROUPPROPERTY` restituisce le informazioni sulla proprietà specificata.  
  
 *property*  
Espressione di tipo **varchar(128)** che restituisce il nome della proprietà del filegroup. *Property* può restituire uno di questi valori:  
  
|valore|Descrizione|Valore restituito|  
|-----------|-----------------|--------------------|  
|**IsReadOnly**|Filegroup di sola lettura.|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = input non valido.|  
|**IsUserDefinedFG**|Filegroup definito dall'utente.|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = input non valido.|  
|**IsDefault**|Filegroup predefinito.|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = input non valido.|  
  
## <a name="return-types"></a>Tipi restituiti  
**int**  
  
## <a name="remarks"></a>Osservazioni  
*filegroup_name* corrisponde alla colonna **name** dalla vista del catalogo **sys.filegroups**.  
  
## <a name="examples"></a>Esempi  
Questo esempio restituisce l'impostazione della proprietà `IsDefault` per il filegroup primario nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT FILEGROUPPROPERTY('PRIMARY', 'IsDefault') AS 'Default Filegroup';  
GO  
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
Default Filegroup   
---------------------   
1  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [FILEGROUP_ID &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-id-transact-sql.md)   
 [FILEGROUP_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-name-transact-sql.md)   
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)  
  
  
