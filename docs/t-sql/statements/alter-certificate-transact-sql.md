---
description: ALTER CERTIFICATE (Transact-SQL)
title: ALTER CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ddfb2f32bb0cc8389fbb8991904925e39cbba946
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191082"
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)

[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  Modifica la password usata per crittografare la chiave privata di un certificato, rimuove la chiave privata o importa la chiave privata se non è presente. Modifica la disponibilità di un certificato per [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> )  
    | WITH ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
  
<private_key_spec> ::=   
      {   
        { FILE = 'path_to_private_key' | BINARY = private_key_bits }  
         [ , DECRYPTION BY PASSWORD = 'current_password' ]  
         [ , ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
    |  
      {  
         [ DECRYPTION BY PASSWORD = 'current_password' ]  
         [ [ , ] ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
``` 
 
 
```syntaxsql  
-- Syntax for Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *certificate_name*  
 Nome univoco con il quale il certificato è noto nel database.  
  
 REMOVE PRIVATE KEY  
 Specifica che la chiave privata non deve più essere mantenuta all'interno del database.  
  
 WITH PRIVATE KEY specifica che la chiave privata del certificato è caricata in SQL Server.

 FILE ='*path_to_private_key*'  
 Specifica il percorso completo, compreso il nome del file, per la chiave privata. Questo parametro può essere un percorso locale o un percorso UNC di rete. L'accesso al file verrà effettuato nel contesto di sicurezza dell'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando si usa questa opzione, verificare che l'account del servizio abbia accesso al file specificato.
 
 Se viene specificato solo un nome file, il file viene salvato nella cartella dei dati utente predefinita per l'istanza. Questa cartella potrebbe essere la cartella DATA di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per Local DB di SQL Server Express, la cartella dei dati utente predefinita per l'istanza corrisponde al percorso specificato dalla variabile di ambiente `%USERPROFILE%` per l'account che ha creato l'istanza.  
  
 BINARY ='*private_key_bits*'  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Bit della chiave privata specificati come costante binaria. Questi bit possono essere in formato crittografato. Se crittografati, l'utente deve fornire una password di decrittografia. I controlli dei criteri della password non vengono eseguiti su questa password. I bit della chiave privata devono essere in un formato di file PVK.  
  
 DECRYPTION BY PASSWORD ='*current_password*'  
 Specifica la password necessaria per decrittografare la chiave privata.  
  
 ENCRYPTION BY PASSWORD ='*new_password*'  
 Viene specificata la password utilizzata per crittografare la chiave privata del certificato nel database. *new_password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [Password Policy](../../relational-databases/security/password-policy.md).  
  
 ACTIVE FOR BEGIN_DIALOG **=** { ON | OFF }  
 Rende il certificato disponibile per un initiator di una conversazione di dialogo di [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="remarks"></a>Commenti  
 La chiave privata deve corrispondere alla chiave pubblica specificata da *certificate_name*.  
  
 La clausola DECRYPTION BY PASSWORD può essere omessa se la password nel file è protetta con un valore di password Null.  
  
 Quando si importa la chiave privata di un certificato già esistente nel database, verrà protetta automaticamente dalla chiave master del database. Per proteggere la chiave privata con una password, usare la clausola ENCRYPTION BY PASSWORD.  
  
 L'opzione REMOVE PRIVATE KEY eliminerà la chiave privata del certificato dal database. È possibile rimuovere la chiave privata nei casi in cui la verifica delle firme verrà eseguita tramite certificato, o negli scenari di [!INCLUDE[ssSB](../../includes/sssb-md.md)] in cui non è richiesta una chiave privata. Non rimuovere la chiave privata di un certificato che protegge una chiave simmetrica. Sarà necessario ripristinare la chiave privata per firmare eventuali altri moduli o altre stringhe che devono essere verificati con il certificato o per decrittografare un valore che è stato crittografato con il certificato.   
  
 Non è necessario specificare una password di decrittografia quando la chiave privata è crittografata tramite la chiave master del database.  
 
 Per cambiare la password usata per crittografare la chiave privata, non specificare le clausole FILE o BINARY.
  
> [!IMPORTANT]  
>  Eseguire sempre una copia di archivio della chiave privata prima di rimuoverla dal database. Per altre informazioni, vedere [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md) e [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md).  
  
 L'opzione WITH PRIVATE KEY non è disponibile in un database indipendente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il certificato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-removing-the-private-key-of-a-certificate"></a>R. Rimozione della chiave privata di un certificato  
  
```sql  
ALTER CERTIFICATE Shipping04   
    REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>B. Modifica della password utilizzata per crittografare la chiave privata  
  
```sql  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%',  
    ENCRYPTION BY PASSWORD = '34958tosdgfkh##38');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>C. Importazione di una chiave privata per un certificato che è già presente nel database  
  
```sql  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>D. Modifica del tipo di protezione della chiave privata, da password a chiave master di database  
  
```sql  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)  
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

