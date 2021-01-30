---
description: DECRYPTBYKEYAUTOCERT (Transact-SQL)
title: DECRYPTBYKEYAUTOCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYKEYAUTOCERT
- DECRYPTBYKEYAUTOCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOCERT function
ms.assetid: 6b45fa2e-ffaa-46f7-86ff-5624596eda4a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 11cc7a88ad012554fac63b08030e31686ef1cc8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207204"
---
# <a name="decryptbykeyautocert-transact-sql"></a>DECRYPTBYKEYAUTOCERT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questa funzione decrittografa i dati con una chiave simmetrica. Tale chiave simmetrica esegue automaticamente la decrittografia con un certificato.  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DecryptByKeyAutoCert ( cert_ID , cert_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *cert_ID*  
ID del certificato usato per proteggere la chiave simmetrica. *cert_ID* ha un tipo di dati **int**.  
  
*cert_password*  
Password usata per crittografare la chiave privata del certificato. Può avere un valore `NULL` se la chiave master del database protegge la chiave privata. *cert_password* ha un tipo di dati **nvarchar**.  

'*ciphertext*'  
Stringa di dati crittografata con la chiave. *ciphertext* ha un tipo dati **varbinary**.  

@ciphertext  
Variabile di tipo **varbinary** contenente dati crittografati con la chiave.  

*add_authenticator*  
Indica se il processo di crittografia originale includeva e crittografava un autenticatore insieme al testo non crittografato. Deve corrispondere al valore passato a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durante il processo di crittografia dei dati. *add_authenticator* ha un valore pari a 1 se il processo di crittografia ha usato un autenticatore. *add_authenticator* ha un tipo di dati **int**.  
  
@add_authenticator  
Variabile che indica se il processo di crittografia originale includeva e crittografava un autenticatore insieme al testo non crittografato. Deve corrispondere al valore passato a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durante il processo di crittografia dei dati. *\@add_authenticator* ha un tipo di dati **int**.  
  
*authenticator*  
Dati usati come base per la generazione dell'autenticatore. Deve corrispondere al valore specificato per [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *authenticator* ha un tipo di dati **sysname**.  
  
@authenticator  
Variabile contenente i dati dai quali derivare un autenticatore. Deve corrispondere al valore specificato per [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* ha un tipo di dati **sysname**.  
  
## <a name="return-types"></a>Tipi restituiti  
**varbinary** con un valore massimo di 8.000 byte.  
  
## <a name="remarks"></a>Osservazioni  
`DECRYPTBYKEYAUTOCERT` consente di combinare le funzionalità di `OPEN SYMMETRIC KEY` e `DECRYPTBYKEY`. In un'unica operazione consente prima di decrittografare una chiave simmetrica e quindi di usarla per la decrittografia del testo crittografato.  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'autorizzazione `VIEW DEFINITION` per la chiave simmetrica e l'autorizzazione `CONTROL` per il certificato.   
  
## <a name="examples"></a>Esempi  
In questo esempio viene illustrato come `DECRYPTBYKEYAUTOCERT` consente di semplificare il codice di decrittografia. Questo codice deve essere eseguito in un database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] per cui non è già presente una chiave master.  
  
```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE CERTIFICATE HumanResources037   
   WITH SUBJECT = 'Sammamish HR',   
   EXPIRY_DATE = '10/31/2009';  
CREATE SYMMETRIC KEY SSN_Key_01 WITH ALGORITHM = DES  
    ENCRYPTION BY CERTIFICATE HumanResources037;  
GO  
----Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037 ;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
--  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key  
--2. Decrypt the data  
--3. Close the symmetric key  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
SELECT NationalIDNumber, EncryptedNationalIDNumber    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--OPTION TWO, using DecryptByKeyAutoCert()  
SELECT NationalIDNumber, EncryptedNationalIDNumber   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoCert ( cert_ID('HumanResources037') , NULL ,EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
