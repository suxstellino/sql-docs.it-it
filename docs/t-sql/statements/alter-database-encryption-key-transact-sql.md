---
description: ALTER DATABASE ENCRYPTION KEY (Transact-SQL)
title: ALTER DATABASE ENCRYPTION KEY (Transact-SQL) | Microsoft Docs
ms.date: 04/16/2018
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_DATABASE_ENCRYPTION_KEY_TSQL
- ALTER DATABASE ENCRYPTION
- ALTER_DATABASE_ENCRYPTION_TSQL
- ALTER DATABASE ENCRYPTION KEY
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key, alter
- ALTER DATABASE ENCRYPTION KEY
ms.assetid: f88dac4b-efe0-47ed-9808-972a4381377e
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 5d8cbeb34708b32d79ecff368ffb56cc7f99c625
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348537"
---
# <a name="alter-database-encryption-key-transact-sql"></a>ALTER DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-pdw](../../includes/applies-to-version/sql-pdw.md)]

  Modifica una chiave di crittografia e ne certifica l'utilizzo per crittografare in modo trasparente un database. Per altre informazioni sulla crittografia trasparente del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER DATABASE ENCRYPTION KEY  
      REGENERATE WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   |  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER DATABASE ENCRYPTION KEY  
    {  
      {  
        REGENERATE WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
        [ ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name ]  
      }  
      |  
      ENCRYPTION BY SERVER   CERTIFICATE Encryptor_Name    
    }  
[ ; ]  
```  
 
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 REGENERATE WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY }  
 Specifica l'algoritmo di crittografia utilizzato per la chiave di crittografia.  
  
 ENCRYPTION BY SERVER CERTIFICATE *Encryptor_Name*  
 Specifica il nome del certificato utilizzato per crittografare la chiave di crittografia del database.  
  
 ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
 Specifica il nome della chiave asimmetrica utilizzata per crittografare la chiave di crittografia del database.  
  
## <a name="remarks"></a>Osservazioni  
 La chiave asimmetrica o il certificato utilizzato per crittografare la chiave di crittografia del database deve essere archiviata nel database di sistema master.  
  
 Quando il proprietario del database (dbo) cambia non è necessario rigenerare la chiave di crittografia del database.
  
 Dopo che una chiave di crittografia del database è stata modificata due volte, è necessario eseguire un backup del log prima che sia possibile modificare nuovamente la chiave di crittografia del database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono necessarie l'autorizzazione CONTROL per il database e l'autorizzazione VIEW DEFINITION per la chiave asimmetrica o il certificato utilizzato per crittografare la chiave di crittografia del database.  
  
## <a name="examples"></a>Esempi  
 L'esempio seguente illustra come modificare la chiave di crittografia del database per utilizzare l'algoritmo `AES_256`.  
  
```sql  
-- Uses AdventureWorks  
  
ALTER DATABASE ENCRYPTION KEY  
REGENERATE WITH ALGORITHM = AES_256;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [Crittografia di SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Chiavi di crittografia del database e di SQL Server &#40;Motore di database&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
 [sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
  
  

