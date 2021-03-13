---
description: DROP SYMMETRIC KEY (Transact-SQL)
title: DROP SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SYMMETRIC KEY
- DROP_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], removing
- deleting symmetric keys
- encryption [SQL Server], symmetric keys
- removing symmetric keys
- dropping symmetric keys
- cryptography [SQL Server], symmetric keys
- DROP SYMMETRIC KEY statement
ms.assetid: 6150bc67-08cb-402e-9c24-b04c9654b434
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 7357a378d91e7ca0d27846cbf7557b1c09e73c61
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421911"
---
# <a name="drop-symmetric-key-transact-sql"></a>DROP SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Rimuove una chiave simmetrica dal database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
DROP SYMMETRIC KEY symmetric_key_name [REMOVE PROVIDER KEY]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *symmetric_key_name*  
 Nome della chiave simmetrica da rimuovere.  
  
 REMOVE PROVIDER KEY  
 Rimuove una chiave EKM (Extensible Key Management ) da un dispositivo EKM. Per altre informazioni su Extensible Key Management, vedere [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
## <a name="remarks"></a>Commenti  
  
Se viene eseguito il mapping della chiave asimmetrica a una chiave EKM (Extensible Key Management) in un dispositivo EKM e l'opzione **REMOVE PROVIDER KEY** non è specificata, la chiave verrà rimossa dal database, ma non dal dispositivo e verrà generato un avviso.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per la chiave simmetrica.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene rimossa la chiave simmetrica denominata `GailSammamishKey6` dal database corrente.  
  
```sql  
CLOSE SYMMETRIC KEY GailSammamishKey6;  
DROP SYMMETRIC KEY GailSammamishKey6;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
