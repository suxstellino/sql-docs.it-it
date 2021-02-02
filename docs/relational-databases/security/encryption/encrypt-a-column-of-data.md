---
title: Crittografare una colonna di dati | Microsoft Docs
description: Informazioni su come crittografare una colonna di dati usando la crittografia simmetrica in SQL Server tramite Transact-SQL, talvolta chiamata crittografia a livello di colonna o di cella.
ms.custom: ''
titleSuffix: SQL Server & Azure Synapse Analytics & Azure SQL Database & SQL Managed Instance
ms.date: 12/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], columns
- cryptography [SQL Server], columns
- column level encryption
- cell level encryption
ms.assetid: 38e9bf58-10c6-46ed-83cb-e2d76cda0adc
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: ae34b381a6f1d46329855662d0a796ea6ab3d265
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250194"
---
# <a name="encrypt-a-column-of-data"></a>Crittografia di una colonna di dati

[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]  

Questo articolo descrive come crittografare una colonna di dati tramite la crittografia simmetrica in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] con [!INCLUDE[tsql](../../../includes/tsql-md.md)]. È chiamata a volte crittografia a livello di colonna o crittografia a livello di cella. Questa funzionalità è in anteprima per Azure Synapse Analytics

Gli esempi in questo articolo sono stati convalidati con AdventureWorks2017. Per ottenere i database di esempio, vedere [Database di esempio AdventureWorks](../../../samples/adventureworks-install-configure.md).

## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  

Le autorizzazioni seguenti sono necessarie per eseguire i passaggi riportati di seguito:  
  
- Autorizzazione `CONTROL` per il database.  
- Autorizzazione `CREATE CERTIFICATE` per il database. Solo gli account di accesso di Windows e di SQL Server e i ruoli applicazione possono disporre di certificati. I gruppi e i ruoli non possono disporre di certificati.  
- Autorizzazione `ALTER` per la tabella.  
- È necessario disporre di un'autorizzazione per la chiave e che non venga negata l'autorizzazione `VIEW DEFINITION`.  
  
## <a name="create-database-master-key"></a>Creare la chiave master del database  

Per usare gli esempi seguenti è necessaria una chiave master di database. Se il database non dispone già di una chiave master del database, crearne una. A tal fine, connettersi al database ed eseguire lo script seguente. Assicurarsi di usare una password complessa.

Copiare e incollare l'esempio riportato di seguito nella finestra della query che è connessa al database di esempio AdventureWorks. Fare clic su **Execute**.  

```sql  
CREATE MASTER KEY ENCRYPTION BY   
PASSWORD = '<complex password>';  
```  

Creare sempre una copia di backup della chiave master del database. Per altre informazioni sulla creazione di chiavi master di database, vedere [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).

## <a name="example-encrypt-with-symmetric-encryption-and-authenticator"></a>Esempio: Crittografare con la crittografia simmetrica e l'autenticatore
  
1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. Sulla barra Standard fare clic su **Nuova query**.  
  
3. Copiare e incollare l'esempio riportato di seguito nella finestra della query che è connessa al database di esempio AdventureWorks. Fare clic su **Execute**.

    ```sql
    CREATE CERTIFICATE Sales09  
       WITH SUBJECT = 'Customer Credit Card Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY CreditCards_Key11  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE Sales.CreditCard   
        ADD CardNumber_Encrypted varbinary(160);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
  
    -- Encrypt the value in column CardNumber using the  
    -- symmetric key CreditCards_Key11.  
    -- Save the result in column CardNumber_Encrypted.    
    UPDATE Sales.CreditCard  
    SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11')  
        , CardNumber, 1, HASHBYTES('SHA2_256', CONVERT( varbinary  
        , CreditCardID)));  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Now list the original card number, the encrypted card number,  
    -- and the decrypted ciphertext. If the decryption worked,  
    -- the original number will match the decrypted number.  
  
    SELECT CardNumber, CardNumber_Encrypted   
        AS 'Encrypted card number', CONVERT(nvarchar,  
        DecryptByKey(CardNumber_Encrypted, 1 ,   
        HASHBYTES('SHA2_256', CONVERT(varbinary, CreditCardID))))  
        AS 'Decrypted card number' FROM Sales.CreditCard;  
    GO  
    ```  
  
## <a name="encrypt-with-simple-symmetric-encryption"></a>Crittografare con la crittografica simmetrica semplice  

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. Sulla barra Standard fare clic su **Nuova query**.  
  
3. Copiare e incollare l'esempio riportato di seguito nella finestra della query che è connessa al database di esempio AdventureWorks. Fare clic su **Execute**.  
  
    ```sql
     CREATE CERTIFICATE HumanResources037  
       WITH SUBJECT = 'Employee Social Security Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY SSN_Key_01  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    USE [AdventureWorks2012];  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE HumanResources.Employee  
        ADD EncryptedNationalIDNumber varbinary(128);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
  
    -- Encrypt the value in column NationalIDNumber with symmetric   
    -- key SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
    UPDATE HumanResources.Employee  
    SET EncryptedNationalIDNumber = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    -- Now list the original ID, the encrypted ID, and the   
    -- decrypted ciphertext. If the decryption worked, the original  
    -- and the decrypted ID will match.  
    SELECT NationalIDNumber, EncryptedNationalIDNumber   
        AS 'Encrypted ID Number',  
        CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
        AS 'Decrypted ID Number'  
        FROM HumanResources.Employee;  
    GO  
    ```  
  
 Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
