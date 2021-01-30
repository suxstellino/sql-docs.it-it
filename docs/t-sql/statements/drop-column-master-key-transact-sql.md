---
description: DROP COLUMN MASTER KEY (Transact-SQL)
title: DROP COLUMN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP COLUMN MASTER KEY
- DROP_COLUMN_MASTER_KEY_TSQL
- DROP COLUMN MASTER
- DROP_COLUMN_MASTER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_master_keys catalog view
ms.assetid: fd5e77c8-a3ae-4795-bb46-b322c0500041
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: fb03c400ade4beecd77592e49d65980cdf599d38
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198226"
---
# <a name="drop-column-master-key-transact-sql"></a>DROP COLUMN MASTER KEY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Elimina la chiave master di una colonna da un database. Si tratta di un'operazione sui metadati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DROP COLUMN MASTER KEY key_name;  
```  

## <a name="arguments"></a>Argomenti
 *key_name*  
 Nome della chiave master della colonna.  
  
## <a name="remarks"></a>Commenti
 La chiave master della colonna può essere eliminata solo se non sono presenti valori della chiave di crittografia della colonna crittografati con tale chiave. Per eliminare i valori della chiave di crittografia della colonna, usare l'istruzione [DROP COLUMN ENCRYPTION KEY](../../t-sql/statements/drop-column-encryption-key-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione **ALTER ANY COLUMN MASTER KEY** per il database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-a-column-master-key"></a>R. Eliminazione di una chiave master della colonna  
 Nell'esempio seguente viene eliminata una chiave master della colonna denominata `MyCMK`.  
  
```sql  
DROP COLUMN MASTER KEY MyCMK;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
 [DROP COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-encryption-key-transact-sql.md)   
 [sys.column_master_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
 [Panoramica della gestione delle chiavi per Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [Gestire le chiavi per Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
  
  
