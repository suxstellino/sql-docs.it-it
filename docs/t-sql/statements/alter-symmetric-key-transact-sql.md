---
description: ALTER SYMMETRIC KEY (Transact-SQL)
title: ALTER SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SYMMETRIC KEY
- ALTER_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], modifying
- cryptography [SQL Server], symmetric keys
- ALTER SYMMETRIC KEY statement
ms.assetid: d3c776a4-7d71-4e6f-84fc-1db47400c465
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: deb59e00ee0d95f7a4dc36b095cb73496b8a5261
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182157"
---
# <a name="alter-symmetric-key-transact-sql"></a>ALTER SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifica le proprietà di una chiave simmetrica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER SYMMETRIC KEY Key_name <alter_option>  
  
<alter_option> ::=  
   ADD ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
   |   
   DROP ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
<encrypting_mechanism> ::=  
   CERTIFICATE certificate_name  
   |  
   PASSWORD = 'password'  
   |  
   SYMMETRIC KEY Symmetric_Key_Name  
   |  
   ASYMMETRIC KEY Asym_Key_Name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Key_name*  
 Nome con il quale la chiave simmetrica da modificare è nota all'interno del database.  
  
 ADD ENCRYPTION BY  
 Aggiunge la crittografia con il metodo specificato.  
  
 DROP ENCRYPTION BY  
 Rimuove la crittografia con il metodo specificato. Non è possibile rimuovere tutte le forme di crittografia da una chiave simmetrica.  
  
 CERTIFICATE *Certificate_name*  
 Specifica il certificato utilizzato per crittografare la chiave simmetrica. Il certificato deve esistere nel database corrente.  
  
 PASSWORD **='** _password_ **'**  
 Specifica la password utilizzata per crittografare la chiave simmetrica. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 SYMMETRIC KEY *Symmetric_Key_Name*  
 Specifica la chiave simmetrica utilizzata per crittografare la chiave simmetrica da modificare. La chiave simmetrica deve esistere nel database ed essere aperta.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Specifica la chiave asimmetrica utilizzata per crittografare la chiave simmetrica da modificare. Tale chiave asimmetrica deve esistere nel database.  
  
## <a name="remarks"></a>Commenti  
  
> [!CAUTION]  
>  Se si crittografa una chiave simmetrica con una password anziché con la chiave pubblica della chiave master del database, viene utilizzato l'algoritmo di crittografia TRIPLE_DES. Per questo motivo, le chiavi create con un algoritmo di crittografia avanzato, come AES, vengono a loro volta protette con un algoritmo meno avanzato.  
  
 Per modificare la crittografia della chiave simmetrica, utilizzare le istruzioni ADD ENCRYPTION e DROP ENCRYPTION. Una chiave non può mai essere archiviata in forma non crittografata. Per questo motivo, è consigliabile aggiungere la nuova forma di crittografia prima di rimuovere la forma precedente.  
  
 Per modificare il proprietario di una chiave simmetrica, usare [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
> [!NOTE]  
>  L'algoritmo RC4 è supportato solo per motivi di compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la chiave simmetrica. Se la crittografia viene applicata con un certificato o una chiave asimmetrica, è richiesta l'autorizzazione VIEW DEFINITION per il certificato o la chiave asimmetrica. Se la crittografia viene rimossa con un certificato o una chiave asimmetrica, è richiesta l'autorizzazione CONTROL per il certificato o la chiave asimmetrica.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene modificato il metodo di crittografia utilizzato per proteggere una chiave simmetrica. La chiave simmetrica `JanainaKey043` è stata crittografata con il certificato `Shipping04` al momento della creazione. Poiché la chiave non può mai essere archiviata in forma non crittografata, in questo esempio la crittografia viene aggiunta con una password e poi rimossa con il certificato.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey043 WITH ALGORITHM = AES_256   
    ENCRYPTION BY CERTIFICATE Shipping04;  
-- Open the key.   
OPEN SYMMETRIC KEY JanainaKey043 DECRYPTION BY CERTIFICATE Shipping04  
    WITH PASSWORD = '<enterStrongPasswordHere>';   
-- First, encrypt the key with a password.  
ALTER SYMMETRIC KEY JanainaKey043   
    ADD ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';  
-- Now remove encryption by the certificate.  
ALTER SYMMETRIC KEY JanainaKey043   
    DROP ENCRYPTION BY CERTIFICATE Shipping04;  
CLOSE SYMMETRIC KEY JanainaKey043;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
