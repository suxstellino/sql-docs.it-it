---
title: Creare chiavi simmetriche identiche su due server | Microsoft Docs
description: Informazioni su come creare chiavi simmetriche identiche su due server in SQL Server usando Transact-SQL. Questa operazione supporta la crittografia in database o server distinti.
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- symmetric keys [SQL Server], creating
ms.assetid: a13d0b21-a43b-43c0-9c22-7ba8f3d15e80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 780944c299548aaa9a5664fef4688c11cdf69646
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250340"
---
# <a name="create-identical-symmetric-keys-on-two-servers"></a>Creare chiavi simmetriche identiche su due server
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come creare chiavi simmetriche identiche in due server diversi in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Al fine di decrittografare l'argomento ciphertext, è necessario disporre della chiave usata per crittografarlo. Quando la crittografia e la decrittografia vengono eseguite in un unico database, la chiave viene archiviata nel database ed è disponibile, a seconda delle autorizzazioni, sia per la crittografia che per la decrittografia. Viceversa, quando la crittografia e la decrittografia vengono eseguite in database separati, la chiave archiviata in un database non è disponibile per l'utilizzo nell'altro database.
  
## <a name="before-you-begin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
  
- Quando si crea una chiave simmetrica è necessario crittografarla con almeno uno degli elementi seguenti: certificato, password, chiave simmetrica, chiave asimmetrica o PROVIDER. Una chiave può essere crittografata con più elementi di ogni tipo, ovvero una singola chiave simmetrica può essere crittografata contemporaneamente con più certificati, password, chiavi simmetriche e chiavi asimmetriche.  
  
- Se si crittografa una chiave simmetrica con una password anziché con la chiave pubblica della chiave master del database, viene usato l'algoritmo di crittografia TRIPLE DES. Per questo motivo, le chiavi create con un algoritmo di crittografia avanzato, come AES, vengono a loro volta protette con un algoritmo meno avanzato.  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY SYMMETRIC KEY per il database. Se si specifica AUTHORIZATION, è richiesta l'autorizzazione IMPERSONATE per l'utente di database o l'autorizzazione ALTER per il ruolo applicazione. Se la crittografia viene applicata con un certificato o una chiave asimmetrica, è richiesta l'autorizzazione VIEW DEFINITION per il certificato o la chiave asimmetrica. Solo gli account di accesso di Windows e di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e i ruoli applicazione possono disporre di chiavi simmetriche. I gruppi e i ruoli non possono disporre di chiavi simmetriche.  
  
## <a name="using-transact-sql"></a>Uso di Transact-SQL  
  
### <a name="to-create-identical-symmetric-keys-on-two-different-servers"></a>Per creare chiavi simmetriche identiche su due server diversi  
  
1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. Sulla barra Standard fare clic su **Nuova query**.  
  
3. Creare una chiave eseguendo le seguenti istruzioni CREATE MASTER KEY, CREATE CERTIFICATE e CREATE SYMMETRIC KEY.  
  
    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'My p@55w0Rd';  
    GO  
    CREATE CERTIFICATE [cert_keyProtection] WITH SUBJECT = 'Key Protection';  
    GO  
    CREATE SYMMETRIC KEY [key_DataShare] WITH  
        KEY_SOURCE = 'My key generation bits. This is a shared secret!',  
        ALGORITHM = AES_256,   
        IDENTITY_VALUE = 'Key Identity generation bits. Also a shared secret'  
        ENCRYPTION BY CERTIFICATE [cert_keyProtection];  
    GO  
    ```  
  
4. Connettersi a un'istanza del server separata, aprire una finestra Query diversa ed eseguire le istruzioni SQL precedenti per creare la stessa chiave nel secondo server.  
  
5. Verificare le chiavi eseguendo prima l'istruzione OPEN SYMMETRIC KEY e quindi l'istruzione SELECT seguente nel primo server.  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    SELECT encryptbykey(key_guid('key_DataShare'), 'MyData' )  
    GO  
    -- For example, the output might look like this: 0x2152F8DA8A500A9EDC2FAE26D15C302DA70D25563DAE7D5D1102E3056CE9EF95CA3E7289F7F4D0523ED0376B155FE9C3  
    ```  
  
6. Nell'altro server incollare il risultato dell'istruzione SELECT precedente nel codice indicato di seguito come valore di `@blob` ed eseguire il codice per verificare che la chiave duplicata sia in grado di decrittografare il testo crittografato.  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    DECLARE @blob varbinary(8000);  
    SET @blob = SELECT CONVERT(varchar(8000), decryptbykey(@blob));  
    GO  
    ```  
  
7. Chiudere la chiave simmetrica in entrambi i server.  
  
    ```sql
    CLOSE SYMMETRIC KEY [key_DataShare];  
    GO  
    ```  

### <a name="encryption-changes-in-sql-server-2017-cu2"></a>Modifiche della crittografia in SQL Server 2017 CU2

SQL Server 2016 usa l'algoritmo hash SHA1 per le operazioni di crittografia. A partire da SQL Server 2017 viene invece usato SHA-2. Ciò significa che possono essere necessari alcuni passaggi aggiuntivi affinché l'installazione di SQL Server 2017 sia in grado di decrittografare gli elementi crittografati da SQL Server 2016. Ecco i passaggi aggiuntivi:

- Verificare che SQL Server 2017 sia aggiornato ad almeno l'aggiornamento cumulativo 2 (CU2).
  - Per importanti informazioni dettagliate, vedere [Aggiornamento cumulativo 2 (CU2) per SQL Server 2017](https://support.microsoft.com/help/4052574).
- Dopo aver installato CU2, attivare il flag di traccia 4631 in SQL Server 2017: `DBCC TRACEON(4631, -1);`
  - Il flag di traccia 4631 è nuovo in SQL Server 2017. Il flag di traccia 4631 richiede un `ON` globale per creare la chiave master, il certificato o la chiave simmetrica in SQL Server 2017. In questo modo gli elementi creati possono interagire con SQL Server 2016 e versioni precedenti.

Per altre informazioni, vedere:

- [CORREZIONE: SQL Server 2017 non è in grado di decrittografare i dati crittografati da versioni precedenti di SQL Server usando la stessa chiave simmetrica](https://support.microsoft.com/help/4053407/sql-server-2017-cannot-decrypt-data-encrypted-by-earlier-versions)
- [Le chiavi simmetriche identiche non funzionano tra SQL Server 2017 e altre versioni di SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/33116269-identical-symmetric-keys-do-not-work-between-sql-s) <!-- Issue 2225. Thank you Stephen W and Sam Rueby. -->

## <a name="for-more-information"></a>Per ulteriori informazioni

-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
  
-   [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbykey-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
