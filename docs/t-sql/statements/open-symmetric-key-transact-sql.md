---
description: OPEN SYMMETRIC KEY (Transact-SQL)
title: OPEN SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN SYMMETRIC KEY
- OPEN_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], opening
- OPEN SYMMETRIC KEY statement
ms.assetid: ff019a7c-c373-46c7-ac43-ffb7e2ee60b3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 939ad2351cfa4bf6340d0451af3ba71656fe5b85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204641"
---
# <a name="open-symmetric-key-transact-sql"></a>OPEN SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Decrittografa una chiave simmetrica e la rende disponibile per l'uso.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
OPEN SYMMETRIC KEY Key_name DECRYPTION BY <decryption_mechanism>  
  
<decryption_mechanism> ::=  
    CERTIFICATE certificate_name [ WITH PASSWORD = 'password' ]  
    |  
    ASYMMETRIC KEY asym_key_name [ WITH PASSWORD = 'password' ]  
    |  
    SYMMETRIC KEY decrypting_Key_name  
    |  
    PASSWORD = 'decryption_password'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Key_name*  
 Nome della chiave simmetrica da aprire.  
  
 CERTIFICATE *certificate_name*  
 Nome del certificato la cui chiave privata verrà utilizzata per decrittografare la chiave simmetrica.  
  
 ASYMMETRIC KEY *asym_key_name*  
 Nome della chiave asimmetrica la cui chiave privata verrà utilizzata per decrittografare la chiave simmetrica.  
  
 WITH PASSWORD ='*password*'  
 Password utilizzata per crittografare la chiave privata del certificato o la chiave asimmetrica.  
  
 SYMMETRIC KEY *decrypting_key_name*  
 Nome della chiave simmetrica che verrà utilizzata per decrittografare la chiave simmetrica aperta.  
  
 PASSWORD ='*password*'  
 Password utilizzata per proteggere la chiave simmetrica.  
  
## <a name="remarks"></a>Commenti  
 Le chiavi simmetriche aperte sono associate alla sessione, non al contesto di sicurezza. Una chiave aperta resterà disponibile finché non viene chiusa in modo esplicito o la sessione non viene terminata. Se si apre una chiave simmetrica e quindi si cambia contesto, la chiave resterà aperta e disponibile nel contesto rappresentato. Le informazioni sulle chiavi simmetriche aperte sono visibili nella vista del catalogo [sys.openkeys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md).  
  
 Se la chiave simmetrica è stata crittografata con un'altra chiave, è necessario aprire innanzitutto la chiave.  
  
 Se la chiave simmetrica è già aperta, la query è di tipo **NO_OP**.  
  
 Se la password, la chiave o il certificato fornito per decrittografare la chiave simmetrica non è corretto, la query non verrà eseguita correttamente.  
  
 Non è possibile aprire chiavi simmetriche create da provider di crittografia. Le operazioni di crittografia e decrittografia che usano questo tipo di chiave simmetrica vengono eseguite correttamente senza l'istruzione **OPEN** dato che è il provider di crittografia ad aprire e chiudere la chiave.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario che il chiamante disponga di un'autorizzazione per la chiave e che non gli sia stata negata l'autorizzazione VIEW DEFINITION per la chiave. I requisisti aggiuntivi possono variare, in base al meccanismo di decrittografia.  
  
-   DECRYPTION BY CERTIFICATE: autorizzazione CONTROL per il certificato e password che crittografa la relativa chiave privata.  
  
-   DECRYPTION BY ASYMMETRIC KEY: autorizzazione CONTROL per la chiave asimmetrica e password che crittografa la relativa chiave privata.  
  
-   DECRYPTION BY PASSWORD: una delle password utilizzate per crittografare la chiave simmetrica.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-opening-a-symmetric-key-by-using-a-certificate"></a>R. Apertura di una chiave simmetrica tramite un certificato  
 Nell'esempio seguente la chiave simmetrica `SymKeyMarketing3` viene aperta e quindi decrittografata tramite la chiave privata del certificato `MarketingCert9`.  
  
```sql  
USE AdventureWorks2012;  
OPEN SYMMETRIC KEY SymKeyMarketing3   
    DECRYPTION BY CERTIFICATE MarketingCert9;  
GO  
```  
  
### <a name="b-opening-a-symmetric-key-by-using-another-symmetric-key"></a>B. Apertura di una chiave simmetrica tramite un'altra chiave simmetrica  
 Nell'esempio seguente la chiave simmetrica `MarketingKey11` viene aperta e quindi decrittografata tramite la chiave simmetrica `HarnpadoungsatayaSE3`.  
  
```sql  
USE AdventureWorks2012;  
-- First open the symmetric key that you want for decryption.  
OPEN SYMMETRIC KEY HarnpadoungsatayaSE3   
    DECRYPTION BY CERTIFICATE sariyaCert01;  
-- Use the key that is already open to decrypt MarketingKey11.  
OPEN SYMMETRIC KEY MarketingKey11   
    DECRYPTION BY SYMMETRIC KEY HarnpadoungsatayaSE3;  
GO   
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
