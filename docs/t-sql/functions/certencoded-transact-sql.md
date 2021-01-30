---
description: CERTENCODED (Transact-SQL)
title: CERTENCODED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTENCODED
- CERTENCODED_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CERTENCODED
ms.assetid: 677a0719-7b9a-4f0b-bc61-41634563f924
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 974485667667296edd9a8d4d6c4187fd93900c1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193424"
---
# <a name="certencoded-transact-sql"></a>CERTENCODED (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce la parte pubblica di un certificato in formato binario. Questa funzione accetta un ID certificato come argomento e restituisce il certificato codificato. Per creare un nuovo certificato passare il risultato binario a **CREATE CERTIFICATE ... WITH BINARY**.
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CERTENCODED ( cert_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*cert_id*  
**certificate_id** del certificato. Questo valore è disponibile in sys.certificates; viene restituito anche dalla funzione [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md). *cert_id* è un tipo di dati **int**.
  
## <a name="return-types"></a>Tipi restituiti
**varbinary**
  
## <a name="remarks"></a>Osservazioni  
Usare **CERTENCODED** e **CERTPRIVATEKEY** insieme per restituire parti diverse di un certificato in formato binario.
  
## <a name="permissions"></a>Autorizzazioni  
**CERTENCODED** è disponibile pubblicamente.
  
## <a name="examples"></a>Esempi  
  
### <a name="simple-example"></a>Esempio semplice  
In questo esempio viene creato un certificato denominato `Shipping04` e viene usata la funzione **CERTENCODED** per restituire la codifica binaria del certificato. La data di scadenza del certificato viene impostata al 31 ottobre 2040.
  
```sql
CREATE DATABASE TEST1;
GO
USE TEST1
CREATE CERTIFICATE Shipping04
ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'
WITH SUBJECT = 'Sammamish Shipping Records',
EXPIRY_DATE = '20401031';
GO
SELECT CERTENCODED(CERT_ID('Shipping04'));
  
```  
  
### <a name="b-copying-a-certificate-to-another-database"></a>B. Copia di un certificato in un altro database  
Nell'esempio più complesso vengono creati due database: `SOURCE_DB` e `TARGET_DB`. Viene quindi creato un certificato in `SOURCE_DB` che viene poi copiato in `TARGET_DB`. Si dimostrerà infine come i dati crittografati in `SOURCE_DB` possano essere decrittografati in `TARGET_DB` usando la copia del certificato.
  
Per creare l'ambiente di esempio, creare i database `SOURCE_DB` e `TARGET_DB` e una chiave master in ognuno. Creare quindi un certificato in `SOURCE_DB`.
  
```sql
USE master;  
GO  
CREATE DATABASE SOURCE_DB;  
GO  
USE SOURCE_DB;  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0URCE_DB KEY Pa$$W0rd';  
GO  
CREATE DATABASE TARGET_DB;  
GO  
USE TARGET_DB  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Pa$$W0rd in TARGET_DB';  
GO  
  
-- Create a certificate in SOURCE_DB  
USE SOURCE_DB;  
GO  
CREATE CERTIFICATE SOURCE_CERT WITH SUBJECT = 'SOURCE_CERTIFICATE';  
GO  
```  
  
Estrarre la descrizione binaria del certificato.
  
```sql
DECLARE @CERTENC VARBINARY(MAX);  
DECLARE @CERTPVK VARBINARY(MAX);  
SELECT @CERTENC = CERTENCODED(CERT_ID('SOURCE_CERT'));  
SELECT @CERTPVK = CERTPRIVATEKEY(CERT_ID('SOURCE_CERT'),  
       'CertEncryptionPa$$word');  
SELECT @CERTENC AS BinaryCertificate;  
SELECT @CERTPVK AS EncryptedBinaryCertificate;  
GO  
```  
  
Creare quindi il certificato duplicato nel database `TARGET_DB`. Per il corretto funzionamento, modificare il codice seguente inserendo i due valori binari @CERTENC e @CERTPVK restituiti nel passaggio precedente. Non racchiudere questi valori tra virgolette.
  
```sql
-- Create the duplicate certificate in the TARGET_DB database  
USE TARGET_DB  
GO  
CREATE CERTIFICATE TARGET_CERT  
FROM BINARY = <insert the binary value of the @CERTENC variable>  
WITH PRIVATE KEY (  
BINARY = <insert the binary value of the @CERTPVK variable>  
, DECRYPTION BY PASSWORD = 'CertEncryptionPa$$word');  
-- Compare the certificates in the two databases  
-- The two certificates should be the same   
-- except for name and (possibly) the certificate_id  
SELECT * FROM SOURCE_DB.sys.certificates  
UNION  
SELECT * FROM TARGET_DB.sys.certificates;  
```  
  
Questo codice, eseguito come singolo batch, dimostra che `TARGET_DB` è in grado di decrittografare i dati originariamente crittografati in `SOURCE_DB`.
  
```sql
USE SOURCE_DB;  
  
DECLARE @CLEARTEXT nvarchar(100);  
DECLARE @CIPHERTEXT varbinary(8000);  
DECLARE @UNCIPHEREDTEXT_Source nvarchar(100);  
SET @CLEARTEXT = N'Hello World';  
SET @CIPHERTEXT = ENCRYPTBYCERT(CERT_ID('SOURCE_CERT'), @CLEARTEXT);  
SET @UNCIPHEREDTEXT_Source =   
    DECRYPTBYCERT(CERT_ID('SOURCE_CERT'), @CIPHERTEXT)  
-- Encryption and decryption result in SOURCE_DB  
SELECT @CLEARTEXT AS SourceClearText, @CIPHERTEXT AS SourceCipherText,   
       @UNCIPHEREDTEXT_Source AS SourceDecryptedText;  
  
-- SWITCH DATABASE  
USE TARGET_DB;  
  
DECLARE @UNCIPHEREDTEXT_Target nvarchar(100);  
SET @UNCIPHEREDTEXT_Target = DECRYPTBYCERT(CERT_ID('TARGET_CERT'), @CIPHERTEXT);  
-- Encryption and decryption result in TARGET_DB  
SELECT @CLEARTEXT AS ClearTextInTarget, @CIPHERTEXT AS CipherTextInTarget, @UNCIPHEREDTEXT_Target AS DecriptedTextInTarget;   
GO  
```  
  
## <a name="see-also"></a>Vedi anche
[Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
[CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
[sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
  
  
