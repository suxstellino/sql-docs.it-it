---
description: Cursori (Transact-SQL)
title: Cursori (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], cursors
- functions [SQL Server], cursors
- cursors [SQL Server], statements
ms.assetid: 63000023-54fc-4efc-a30f-fb4d4db73aae
author: rothja
ms.author: jroth
ms.openlocfilehash: edfa870dd6f2f21f6d2bfdd122365c9913dc342a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128268"
---
# <a name="cursors-transact-sql"></a>Cursori (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Le istruzioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generano un set di risultati completo, ma in alcuni casi è preferibile elaborare i risultati una riga alla volta. Se si apre un cursore su un set di risultati, il set potrà essere elaborato una riga alla volta. È possibile assegnare un cursore a una variabile o un parametro con il tipo di dati **cursor**.  
  
 Le operazioni di cursori sono supportate nelle seguenti istruzioni:  
  
 [CLOSE](../../t-sql/language-elements/close-transact-sql.md)  
  
 [CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)  
  
 [DEALLOCATE](../../t-sql/language-elements/deallocate-transact-sql.md)  
  
 [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md)  
  
 [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [FETCH](../../t-sql/language-elements/fetch-transact-sql.md)  
  
 [OPEN](../../t-sql/language-elements/open-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [SET](../../t-sql/statements/set-statements-transact-sql.md)  
  
 I cursori sono supportati inoltre nelle seguenti funzioni e stored procedure di sistema:  
  
 [@@CURSOR_ROWS](../../t-sql/functions/cursor-rows-transact-sql.md)  
  
 [CURSOR_STATUS](../../t-sql/functions/cursor-status-transact-sql.md)  
  
 [@@FETCH_STATUS](../../t-sql/functions/fetch-status-transact-sql.md)  
  
 [sp_cursor_list](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)  
  
 [sp_describe_cursor](../../relational-databases/system-stored-procedures/sp-describe-cursor-transact-sql.md)  
  
 [sp_describe_cursor_columns](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)  
  
 [sp_describe_cursor_tables](../../relational-databases/system-stored-procedures/sp-describe-cursor-tables-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Cursori](../../relational-databases/cursors.md)  
  
  
