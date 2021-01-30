---
description: ALTER ASYMMETRIC KEY (Transact-SQL)
title: ALTER ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.date: 04/12/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ASYMMETRIC_KEY_TSQL
- ALTER ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- passwords [SQL Server], asymmetric keys
- encryption [SQL Server], asymmetric keys
- DECRYPTION BY PASSWORD option
- ALTER ASYMMETRIC KEY statement
- asymmetric keys [SQL Server], modifying
ms.assetid: 958e95d6-fbe6-43e8-abbd-ccedbac2dbac
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5d9604d366417688c8c04697e77852c3a27e189a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182991"
---
# <a name="alter-asymmetric-key-transact-sql"></a>ALTER ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifica le proprietà di una chiave asimmetrica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER ASYMMETRIC KEY Asym_Key_Name <alter_option>  
  
<alter_option> ::=  
      <password_change_option>   
    | REMOVE PRIVATE KEY   

<password_change_option> ::=  
    WITH PRIVATE KEY ( <password_option> [ , <password_option> ] )  

<password_option> ::=  
      ENCRYPTION BY PASSWORD = 'strongPassword'  
    | DECRYPTION BY PASSWORD = 'oldPassword'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Asym_Key_Name*  
 Nome con il quale è nota la chiave asimmetrica all'interno del database.  
  
 REMOVE PRIVATE KEY  
 Rimuove la chiave privata dalla chiave asimmetrica. La chiave pubblica non viene rimossa.  
  
 WITH PRIVATE KEY  
 Modifica la protezione della chiave privata.  
  
 ENCRYPTION BY PASSWORD **='** _strongPassword_*_'_*  
 Specifica una nuova password per proteggere la chiave privata. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se questa opzione viene omessa, la chiave privata verrà crittografata con la chiave master del database.  
  
 DECRYPTION BY PASSWORD **='** _oldPassword_*_'_*  
 Specifica la vecchia password, attualmente utilizzata per proteggere la chiave privata. Questo parametro non è richiesto se la chiave privata è crittografata con la chiave master del database.  
  
## <a name="remarks"></a>Osservazioni  
 Se non è presente una chiave master del database, l'opzione ENCRYPTION BY PASSWORD è obbligatoria e l'operazione avrà esito negativo se non si specifica una password. Per altre informazioni su come creare una chiave master del database, vedere [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md).  
  
 È possibile utilizzare ALTER ASYMMETRIC KEY per modificare la protezione della chiave privata specificando le opzioni PRIVATE KEY come illustrato nella tabella seguente.  
  
|Modifica della protezione|ENCRYPTION BY PASSWORD|DECRYPTION BY PASSWORD|  
|----------------------------|----------------------------|----------------------------|  
|Sostituzione della vecchia password con una nuova password|Obbligatoria|Obbligatoria|  
|Sostituzione della password con la chiave master|Omettere|Obbligatorio|  
|Sostituzione della chiave master con una password|Obbligatorio|Omettere|  
  
 È necessario aprire la chiave master del database prima di poterla utilizzare per proteggere una chiave privata. Per altre informazioni, vedere [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md).  
  
 Per modificare il proprietario di una chiave asimmetrica, usare [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per la chiave asimmetrica se si rimuove la chiave privata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-password-of-the-private-key"></a>R. Modifica della password della chiave privata  
 Nell'esempio seguente viene modificata la password utilizzata per proteggere la chiave privata della chiave asimmetrica `PacificSales09`. Verrà impostata la nuova password `<enterStrongPasswordHere>`.  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales09   
    WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<oldPassword>',  
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>');  
GO  
```  
  
### <a name="b-removing-the-private-key-from-an-asymmetric-key"></a>B. Rimozione della chiave privata da una chiave asimmetrica  
 Nell'esempio seguente viene rimossa la chiave privata da `PacificSales19`, lasciando solo la chiave pubblica.  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales19 REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="c-removing-password-protection-from-a-private-key"></a>C. Rimozione della protezione con password da una chiave privata  
 Nell'esempio seguente viene rimossa la protezione con password da una chiave privata e la chiave viene protetta con la chiave master del database.  
  
```sql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '<database master key password>';  
ALTER ASYMMETRIC KEY PacificSales09 WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<enterStrongPasswordHere>' );  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Chiavi di crittografia del database e di SQL Server &#40;Motore di database&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)   
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
