---
description: sys.user_token (Transact-SQL)
title: sys.user_token (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.user_token
- user_token
- sys.user_token_TSQL
- user_token_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], security tokens
- sys.user_token catalog view
- user tokens [SQL Server]
- tokens [SQL Server]
- user_token catalog view
ms.assetid: be018103-5e57-43a4-9160-9bf420892aa7
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017|| = azure-sqldw-latest
ms.openlocfilehash: 05dcc6f926834170ce5541ae146f2f24e66a6333
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206806"
---
# <a name="sysuser_token-transact-sql"></a>sys.user_token (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)]

  Restituisce una riga per ogni entità di database che fa parte del token utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**principal_id**|**int**|ID dell'entità. Questo valore deve essere univoco all'interno del database.|  
|**SID**|**varbinary(85)**|Identificatore di sicurezza dell'entità se l'entità è esterna al database. Questo valore può essere ad esempio un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], di Windows, di un gruppo di Windows oppure sul quale viene eseguito il mapping a un certificato. In caso contrario, questo valore è NULL.|  
|**nome**|**nvarchar (128)**|Nome dell'entità. Questo valore deve essere univoco all'interno del database.|  
|**type**|**nvarchar (128)**|Descrizione del tipo dell'entità. Viene eseguito il mapping di tutti i tipi a **SID**. I possibili valori sono i seguenti:<br /><br /> SQL USER<br /><br /> WINDOWS LOGIN<br /><br /> WINDOWS GROUP<br /><br /> ROLE<br /><br /> APPLICATION ROLE<br /><br /> RUOLO DEL DATABASE<br /><br /> USER MAPPED TO CERTIFICATE<br /><br /> USER MAPPED TO ASYMMETRIC KEY<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC KEY|  
|**utilizzo**|**nvarchar (128)**|Specifica che l'entità partecipa alla valutazione di autorizzazioni GRANT or DENY o funge da autenticatore.<br /><br /> I valori validi sono i seguenti:<br /><br /> GRANT OR DENY<br /><br /> DENY ONLY<br /><br /> AUTHENTICATOR|  
  
## <a name="see-also"></a>Vedere anche  
 [sys.login_token &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-login-token-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
