---
description: cursor (Transact-SQL)
title: cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- cursor data type
ms.assetid: fbea16ef-f2cc-4734-9149-ec2598fd3cca
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: cc1f3981733712758230287c770c47dd0bf43562
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124982"
---
# <a name="cursor-transact-sql"></a>cursor (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Tipo di dati per variabili o parametri di OUTPUT di stored procedure che contengono un riferimento a un cursore.
  
## <a name="remarks"></a>Commenti  
Le operazioni in cui è possibile fare riferimento a variabili e parametri con tipo di dati **cursor** sono le seguenti:
-   Istruzioni DECLARE *\@local_variable* e SET *\@local_variable*.  
-   Istruzioni di cursore OPEN, FETCH, CLOSE e DEALLOCATE.  
-   Parametri di output di stored procedure.  
-   Funzione CURSOR_STATUS.  
-   Stored procedure di sistema **sp_cursor_list**, **sp_describe_cursor**, **sp_describe_cursor_tables** e **sp_describe_cursor_columns**.  
  
La colonna di output **cursor_name** di **sp_cursor_list** e **sp_describe_cursor** restituisce il nome della variabile di cursore.
  
Le variabili create con il tipo di dati **cursor** ammettono i valori Null.
  
Il tipo di dati **cursor** non può essere usato per una colonna in un'istruzione CREATE TABLE.
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CURSOR_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-status-transact-sql.md)  
[Conversione del tipo di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  
