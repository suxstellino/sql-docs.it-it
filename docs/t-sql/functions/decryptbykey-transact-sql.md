---
description: DECRYPTBYKEY (Transact-SQL)
title: DECRYPTBYKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DecryptByKey_TSQL
- DECRYPTBYKEY
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], DecryptByKey function
- decryption [SQL Server], keys
- decryption [SQL Server], symmetric keys
- DECRYPTBYKEY function
ms.assetid: 6edf121f-ac62-4dae-90e6-6938f32603c9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 92678836af07b625c79fae38ba7630c1ae4d5b21
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211064"
---
# <a name="decryptbykey-transact-sql"></a>DECRYPTBYKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione usa una chiave simmetrica per decrittografare i dati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
DecryptByKey ( { 'ciphertext' | @ciphertext }   
    [ , add_authenticator, { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*ciphertext*  
Variabile di tipo **varbinary** contenente dati crittografati con la chiave.  
  
**\@ciphertext**  
Variabile di tipo **varbinary** contenente dati crittografati con la chiave.  
  
 *add_authenticator*  
Indica se il processo di crittografia originale includeva e crittografava un autenticatore insieme al testo non crittografato. Deve corrispondere al valore passato a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durante il processo di crittografia dei dati. *add_authenticator* ha un tipo di dati **int**.  
  
 *authenticator*  
Dati usati come base per la generazione dell'autenticatore. Deve corrispondere al valore specificato per [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *authenticator* ha un tipo di dati **sysname**.  

**\@authenticator**  
Variabile contenente i dati dai quali derivare un autenticatore. Deve corrispondere al valore specificato per [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* ha un tipo di dati **sysname**.  

## <a name="return-types"></a>Tipi restituiti  
**varbinary** con un valore massimo di 8.000 byte. `DECRYPTBYKEY` restituisce NULL se la chiave simmetrica usata per crittografare i dati non è aperta o se *ciphertext* è NULL.  
  
## <a name="remarks"></a>Commenti  
`DECRYPTBYKEY` usa una chiave simmetrica. Il database deve avere la chiave simmetrica già aperta. `DECRYPTBYKEY` consente di avere più chiavi aperte contemporaneamente. Non è necessario aprire la chiave subito prima di decrittografare il testo crittografato.  
  
La crittografia e la decrittografia simmetriche in genere operano in modo relativamente rapido e funzionano anche per le operazioni che includono volumi di dati di grandi dimensioni.  

La chiamata a `DECRYPTBYKEY` deve essere eseguita nel contesto del database contenente la chiave di crittografia. Assicurarsi che ciò avvenga chiamando `DECRYPTBYKEY` da un oggetto (ad esempio una vista, una stored procedure o una funzione) che si trovi nel database. 
  
## <a name="permissions"></a>Autorizzazioni  
La chiave simmetrica deve essere già aperta nella sessione corrente. Per altre informazioni, vedere [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-decrypting-by-using-a-symmetric-key"></a>R. Decrittografia di dati tramite una chiave simmetrica  
In questo esempio viene decrittografato il testo crittografato con una chiave simmetrica.  
  
```sql  
-- First, open the symmetric key with which to decrypt the data.  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
GO  
  
-- Now list the original ID, the encrypted ID, and the   
-- decrypted ciphertext. If the decryption worked, the original  
-- and the decrypted ID will match.  
SELECT NationalIDNumber, EncryptedNationalID   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalID))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
GO  
```  
  
### <a name="b-decrypting-by-using-a-symmetric-key-and-an-authenticating-hash"></a>B. Decrittografia di dati tramite una chiave simmetrica e un hash di autenticazione  
In questo esempio vengono decrittografati i dati originariamente crittografati insieme a un autenticatore.  
  
```sql  
-- First, open the symmetric key with which to decrypt the data  
OPEN SYMMETRIC KEY CreditCards_Key11  
   DECRYPTION BY CERTIFICATE Sales09;  
GO  
  
-- Now list the original card number, the encrypted card number,  
-- and the decrypted ciphertext. If the decryption worked,   
-- the original number will match the decrypted number.  
SELECT CardNumber, CardNumber_Encrypted   
    AS 'Encrypted card number', CONVERT(nvarchar,  
    DecryptByKey(CardNumber_Encrypted, 1 ,   
    HashBytes('SHA1', CONVERT(varbinary, CreditCardID))))   
    AS 'Decrypted card number' FROM Sales.CreditCard;  
GO  
  
```  

### <a name="c-fail-to-decrypt-when-not-in-the-context-of-database-with-key"></a>C. Non è possibile eseguire la decrittografia al di fuori del contesto del database con la chiave
L'esempio seguente illustra che è necessario eseguire `DECRYPTBYKEY` nel contesto del database che contiene la chiave. La riga non verrà decrittografata quando si esegue `DECRYPTBYKEY` nel database master; il risultato sarà NULL. 

```sql
-- Create the database
CREATE DATABASE TestingDecryptByKey
GO

USE [TestingDecryptByKey]

-- Create the table and view
CREATE TABLE TestingDecryptByKey.dbo.Test(val VARBINARY(8000) NOT NULL);
GO
CREATE VIEW dbo.TestView AS SELECT CAST(DecryptByKey(val) AS VARCHAR(30)) AS DecryptedVal FROM TestingDecryptByKey.dbo.Test;
GO

-- Create the key, and certificate
USE TestingDecryptByKey;
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'ItIsreallyLong1AndSecured!Passsword#';
CREATE CERTIFICATE TestEncryptionCertificate WITH SUBJECT = 'TestEncryption';
CREATE SYMMETRIC KEY TestEncryptSymmmetricKey WITH ALGORITHM = AES_256, IDENTITY_VALUE = 'It is place for test',
KEY_SOURCE = 'It is source for test' ENCRYPTION BY CERTIFICATE TestEncryptionCertificate;

-- Insert rows into the table
DECLARE @var VARBINARY(8000), @Val VARCHAR(30);
SELECT @Val = '000-123-4567';
OPEN SYMMETRIC KEY TestEncryptSymmmetricKey DECRYPTION BY CERTIFICATE TestEncryptionCertificate;
SELECT @var = EncryptByKey(Key_GUID('TestEncryptSymmmetricKey'), @Val);
SELECT CAST(DecryptByKey(@var) AS VARCHAR(30)), @Val;
INSERT INTO dbo.Test VALUES(@var);
GO

-- Switch to master
USE [Master];
GO

-- Results show the date inserted
SELECT DecryptedVal FROM TestingDecryptByKey.dbo.TestView;

-- Results are NULL because we are not in the context of the TestingDecryptByKey Database
SELECT CAST(DecryptByKey(val) AS VARCHAR(30)) AS DecryptedVal FROM TestingDecryptByKey.dbo.Test;
GO

-- Clean up resources
USE TestingDecryptByKey;

DROP SYMMETRIC KEY TestEncryptSymmmetricKey REMOVE PROVIDER KEY;
DROP CERTIFICATE TestEncryptionCertificate;

Use [Master]
DROP DATABASE TestingDecryptByKey;
GO
```

  
## <a name="see-also"></a>Vedere anche  
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Scelta di un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
  
  
