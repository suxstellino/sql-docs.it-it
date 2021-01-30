---
description: KEY_ID (Transact-SQL)
title: KEY_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Key_ID
- Key_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identification numbers [SQL Server], symmetric keys
- KEY_ID function
- symmetric keys [SQL Server], IDs
- IDs [SQL Server], symmetric keys
ms.assetid: d7309542-dbbe-41dc-b42e-5d9a1c8b4838
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c1481c85e999ebccdc47e18da315ddd302c77679
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183950"
---
# <a name="key_id-transact-sql"></a>KEY_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce l'ID di una chiave simmetrica nel database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
Key_ID ( 'Key_Name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *Key_Name* **'**  
 Nome di una chiave simmetrica nel database.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 Il nome di una chiave temporanea deve iniziare con un simbolo di cancelletto (#).  
  
## <a name="permissions"></a>Autorizzazioni  
 Dal momento che le chiavi temporanee sono disponibili solo nella sessione in cui vengono create, per accedervi non sono necessarie autorizzazioni. Per accedere a una chiave non temporanea, è necessario che il chiamante disponga di un'autorizzazione per la chiave e che non gli sia stata negata l'autorizzazione VIEW per la chiave.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-id-of-a-symmetric-key"></a>R. Restituzione dell'ID di una chiave simmetrica  
 Nell'esempio seguente viene restituito l'ID di una chiave denominata `ABerglundKey1`.  
  
```sql  
SELECT KEY_ID('ABerglundKey1');  
```  
  
### <a name="b-returning-the-id-of-a-temporary-symmetric-key"></a>B. Restituzione dell'ID di una chiave simmetrica temporanea  
 Nell'esempio seguente viene restituito l'ID di una chiave simmetrica temporanea. Si noti che il nome della chiave è preceduto dal simbolo di cancelletto `#`.  
  
```sql  
SELECT KEY_ID('#ABerglundKey2');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [KEY_GUID &#40;Transact-SQL&#41;](../../t-sql/functions/key-guid-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [sys.key_encryptions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
