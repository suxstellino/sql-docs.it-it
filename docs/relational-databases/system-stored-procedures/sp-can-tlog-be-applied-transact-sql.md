---
description: sp_can_tlog_be_applied (Transact-SQL)
title: sp_can_tlog_be_applied (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_can_tlog_be_applied_TSQL
- sp_can_tlog_be_applied
dev_langs:
- TSQL
helpviewer_keywords:
- sp_can_tlog_be_applied
ms.assetid: 9c143b6c-27ac-4ab7-98d1-3b7b265f3963
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9521c435d17e94a034aa1723f5cd023035cd1f87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197689"
---
# <a name="sp_can_tlog_be_applied-transact-sql"></a>sp_can_tlog_be_applied (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verifica se è possibile applicare un backup del log delle transazioni a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **sp_can_tlog_be_applied** richiede che il database sia in stato di ripristino.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_can_tlog_be_applied [ @backup_file_name = ] 'backup_file_name'   
        , [ @database_name = ] 'database_name'   
        , [ @result = ] result OUTPUT  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @backup_file_name = ] 'backup_file_name'` Nome di un file di backup. *backup_file_name* è di **tipo nvarchar (128)**.  
  
`[ @database_name = ] 'database_name'` Nome del database. *database_name* è di tipo **sysname**.  
  
`[ @result = ] _result_ OUTPUT` Indica se il log delle transazioni può essere applicato al database. il *risultato* è **bit**.  
  
 1 = Il log può essere applicato  
  
 0= Il log non può essere applicato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_can_tlog_be_applied**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene dichiarata una variabile locale, `@MyBitVar`, per archiviare il risultato.  
  
```  
USE master;  
GO  
DECLARE @MyBitVar BIT;  
EXEC sp_can_tlog_be_applied  
     @backup_file_name =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\AdventureWorks2012.bak',  
     @database_name = N'AdventureWorks2012',  
     @result = @MyBitVar OUTPUT;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
