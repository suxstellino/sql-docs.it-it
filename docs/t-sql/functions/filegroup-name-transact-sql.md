---
description: FILEGROUP_NAME (Transact-SQL)
title: FILEGROUP_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILEGROUP_NAME_TSQL
- FILEGROUP_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- displaying filegroup names
- identification numbers [SQL Server], filegroups
- filegroups [SQL Server], IDs
- IDs [SQL Server], filegroups
- FILEGROUP_NAME function
- filegroups [SQL Server], names
- names [SQL Server], filegroups
- viewing filegroup names
ms.assetid: 26add1c0-56e5-47a8-b489-ae56784a7ee9
author: cawrites
ms.author: chadam
ms.openlocfilehash: aca2bd20cf01614944ee9637cda6c194e9734805
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101139"
---
# <a name="filegroup_name-transact-sql"></a>FILEGROUP_NAME (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questa funzione restituisce il nome del filegroup corrispondente al numero di identificazione (ID) di filegroup specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
FILEGROUP_NAME ( filegroup_id )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *filegroup_id*  

Numero dell'ID di filegroup per cui `FILEGROUP_NAME` restituirà il nome di filegroup. *filegroup_id* ha un tipo di dati **smallint**.  
  
## <a name="return-types"></a>Tipi restituiti  
**nvarchar(128)**  
  
## <a name="remarks"></a>Commenti  
*filegroup_id* corrisponde alla colonna **data_space_id** nella vista del catalogo **sys.filegroups**.  
  
## <a name="examples"></a>Esempi  
Questo esempio restituisce il nome del filegroup corrispondente all'ID di filegroup `1` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT FILEGROUP_NAME(1) AS [Filegroup Name];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Filegroup Name   
-----------------------  
PRIMARY  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)  
  
  
