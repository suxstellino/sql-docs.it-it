---
description: FILE_ID (Transact-SQL)
title: FILE_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILE_ID
- FILE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IDs [SQL Server], files
- file IDs [SQL Server]
- FILE_ID function
- names [SQL Server], files
- identification numbers [SQL Server], files
- file names [SQL Server], FILE_ID
ms.assetid: 6a7382cf-a360-4d62-b9d2-5d747f56f076
author: cawrites
ms.author: chadam
ms.openlocfilehash: 979b1f1689c4538fd78aa33e9473b6e0a2e69c85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198462"
---
# <a name="file_id-transact-sql"></a>FILE_ID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Per il nome logico specificato per un file di componente del database corrente, questa funzione restituisce il numero di identificazione (ID) del file.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Usare [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) in alternativa.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
FILE_ID ( file_name )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*file_name*  
Un'espressione di tipo **sysname**, che rappresenta il nome logico del file per cui `FILE_ID` restituirà il valore dell'ID del file.  
  
## <a name="return-types"></a>Tipi restituiti  
**smallint**  
  
## <a name="remarks"></a>Osservazioni  
*file_name* corrisponde al nome di file logico visualizzato nella colonna name della vista del catalogo sys.master_files o sys.database_files.  

`FILE_ID` restituisce `NULL` se *file_name* non corrisponde al nome logico di un file di componente del database corrente.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il numero di identificazione di file assegnato ai cataloghi full-text è maggiore di 32767. Dato che la funzione `FILE_ID` ha un tipo restituito **smallint**, `FILE_ID` non supporterà file full-text. Usare [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) in alternativa.  
  
## <a name="examples"></a>Esempi  
Questo esempio restituisce il valore di ID file per il file `AdventureWorks_Data`, un file di componente del database `ADVENTUREWORKS2012`.  

```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_ID('AdventureWorks2012_Data')AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
1  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzionalità del motore di database deprecate in SQL Server 2016](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [FILE_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/file-name-transact-sql.md)   
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
