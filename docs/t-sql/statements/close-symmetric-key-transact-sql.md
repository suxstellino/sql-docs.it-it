---
description: CLOSE SYMMETRIC KEY (Transact-SQL)
title: CLOSE SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CLOSE SYMMETRIC KEY
- CLOSE_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- closing symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], closing
- CLOSE SYMMETRIC KEY statement
- cryptography [SQL Server], symmetric keys
ms.assetid: 3b083cbb-3c6a-4f59-8d34-601db1efcc83
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 88b36782a114ca5a078d438719147892e46749c0
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421915"
---
# <a name="close-symmetric-key-transact-sql"></a>CLOSE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Chiude una chiave simmetrica oppure tutte le chiavi simmetriche aperte nella sessione corrente.  
  
  ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)] 
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CLOSE { SYMMETRIC KEY key_name | ALL SYMMETRIC KEYS }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Key_name*  
 Nome della chiave simmetrica da chiudere.  
  
## <a name="remarks"></a>Commenti  
 Le chiavi simmetriche aperte sono associate alla sessione, non al contesto di sicurezza. Una chiave aperta resterà disponibile finché non viene chiusa in modo esplicito o la sessione non viene terminata. CLOSE ALL SYMMETRIC KEYS chiuderà qualsiasi chiave master del database aperta nella sessione corrente tramite l'istruzione [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md).  Le informazioni sulle chiavi aperte sono visibili nella vista del catalogo [sys.openkeys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Non sono richieste autorizzazioni esplicite per chiudere una chiave simmetrica.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-closing-a-symmetric-key"></a>R. Chiusura di una chiave simmetrica  
 Nell'esempio seguente viene chiusa la chiave simmetrica `ShippingSymKey04`.  
  
```sql  
CLOSE SYMMETRIC KEY ShippingSymKey04;  
GO  
```  
  
### <a name="b-closing-all-symmetric-keys"></a>B. Chiusura di tutte le chiavi simmetriche  
 Nell'esempio seguente vengono chiuse tutte le chiavi simmetriche aperte nella sessione corrente insieme alla chiave master del database aperta in modo esplicito.  
  
```sql  
CLOSE ALL SYMMETRIC KEYS;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)  
  
  
