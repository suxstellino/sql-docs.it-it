---
description: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
title: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_ENCRYPTION_KEY_TSQL
- ENCRYPTION_KEY_TSQL
- sql13.swb.dbencryptionkeyo.f1
- ENCRYPTION KEY
- DATABASE ENCRYPTION KEY
- CREATE_DATABASE_ENCRYPTION_KEY_TSQL
- CREATE DATABASE ENCRYPTION KEY
- sql13.swb.dbencryptionkeyg.f1
- CREATE DATABASE ENCRYPTION
- CREATE_DATABASE_ENCRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key
- CREATE DATABASE ENCRYPTION KEY statement
- database encryption key, create
ms.assetid: 2ee95a32-5140-41bd-9ab3-a947b9990688
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f0bf20d9ac242727ec98a3d617c07ea5588b8fb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192717"
---
# <a name="create-database-encryption-key-transact-sql"></a>CREATE DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

 Crea una chiave di crittografia usata per crittografare in modo trasparente un database. Per altre informazioni sulla crittografia trasparente del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```  
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY  }  
Specifica l'algoritmo di crittografia utilizzato per la chiave di crittografia.

> [!WARNING]
> A partire da SQL Server 2016, tutti gli algoritmi diversi da AES_128, AES_192 e AES_256 sono deprecati. Per usare algoritmi meno recenti (sconsigliato), è necessario impostare il database sul livello di compatibilità del database 120 o su uno inferiore.  
  
ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name  
Specifica il nome del componente di crittografia utilizzato per crittografare la chiave di crittografia del database.  
  
ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
Specifica il nome della chiave asimmetrica utilizzata per crittografare la chiave di crittografia del database. Per crittografare la chiave di crittografia del database con una chiave asimmetrica, è necessario che quest'ultima risieda in un provider EKM (Extensible Key Management).  
  
## <a name="remarks"></a>Osservazioni  
Per poter crittografare un database usando *Transparent Data Encryption (TDE)* è necessaria una chiave di crittografia del database. Crittografandolo in modo trasparente, l'intero database viene crittografato a livello di file, in assenza di qualunque modifica particolare del codice. La chiave asimmetrica o il certificato utilizzato per crittografare la chiave di crittografia del database deve essere archiviata nel database di sistema master.  
  
Le istruzioni sulla crittografia del database sono consentite solo sui database utente.  
  
Non è possibile esportare dal database la relativa chiave di crittografia. È disponibile solo per il sistema, per gli utenti con autorizzazioni di debug per il server e per gli utenti che hanno accesso ai certificati utilizzati per crittografare e decrittografare la chiave di crittografia del database.  
  
Non è necessario rigenerare la chiave di crittografia del database in caso di modifica del proprietario del database (dbo).  
  
Una chiave di crittografia del database viene creata automaticamente per un database [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Non è necessario creare una chiave usando l'istruzione CREATE DATABASE ENCRYPTION KEY.  
  
## <a name="permissions"></a>Autorizzazioni  
Sono necessarie l'autorizzazione CONTROL per il database e l'autorizzazione VIEW DEFINITION per la chiave asimmetrica o il certificato utilizzato per crittografare la chiave di crittografia del database.  
  
## <a name="examples"></a>Esempi  
Per altri esempi in cui viene usato TDE, vedere [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md), [Abilitare TDE in SQL Server con EKM](../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md) e [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
Nell'esempio seguente viene creata una chiave di crittografia del database tramite l'algoritmo `AES_256` e tale chiave viene quindi protetta con un certificato denominato `MyServerCert`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
[Crittografia di SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
[Chiavi di crittografia del database e di SQL Server &#40;Motore di database&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
[Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
[Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
[ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
[DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
[sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
    
