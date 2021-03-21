---
description: BACKUP CERTIFICATE (Transact-SQL)
title: BACKUP CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DUMP_CERTIFICATE_TSQL
- BACKUP CERTIFICATE
- sql13.swb.exportcertificate.f1
- DUMP CERTIFICATE
- BACKUP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- decryption [SQL Server], certificates
- exporting certificates
- certificates [SQL Server], exporting
- BACKUP CERTIFICATE statement
- backing up certificates [SQL Server]
- decryption [SQL Server]
- cryptography [SQL Server], certificates
ms.assetid: 509b9462-819b-4c45-baae-3d2d90d14a1c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 942b16b089c4f5324073c679345bd88ad77e4d60
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740011"
---
# <a name="backup-certificate-transact-sql"></a>BACKUP CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-pdw](../../includes/applies-to-version/sql-pdw.md)]

  Esporta un certificato in un file.  
  
 ![Icona di collegamento](../../database-engine/configure-windows/media/topic-link.gif "icona di collegamento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server  
  
BACKUP CERTIFICATE certname TO FILE = 'path_to_file'  
    [ WITH PRIVATE KEY   
      (   
        FILE = 'path_to_private_key_file' ,  
        ENCRYPTION BY PASSWORD = 'encryption_password'   
        [ , DECRYPTION BY PASSWORD = 'decryption_password' ]   
      )   
    ]  
```  
  
   
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
BACKUP CERTIFICATE certname TO FILE ='path_to_file'  
      WITH PRIVATE KEY   
      (   
        FILE ='path_to_private_key_file',  
        ENCRYPTION BY PASSWORD ='encryption_password'   
      )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *certname*  
 Il nome del certificato di cui eseguire il backup.

 TO FILE = '*path_to_file*'  
 Specifica il percorso completo, nome di file incluso, del file in cui verrà salvato il certificato. Questo percorso può essere un percorso locale o un percorso UNC di rete. Se si specifica solo un nome, il file verrà salvato nella cartella di dati utente predefinita dell'istanza, che può essere o meno la cartella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DATA. Per LocalDB di SQL Server Express, la cartella di dati utente predefinita dell'istanza corrisponde al percorso specificato dalla variabile di ambiente `%USERPROFILE%` per l'account che ha creato l'istanza.  

 WITH PRIVATE KEY specifica che la chiave privata del certificato deve essere salvata in un file. Questa clausola è facoltativa.

 FILE = '*path_to_private_key_file*'  
 Specifica il percorso completo, nome di file incluso, del file in cui verrà salvata la chiave privata. Questo percorso può essere un percorso locale o un percorso UNC di rete. Se si specifica solo un nome, il file verrà salvato nella cartella di dati utente predefinita dell'istanza, che può essere o meno la cartella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DATA. Per LocalDB di SQL Server Express, la cartella di dati utente predefinita dell'istanza corrisponde al percorso specificato dalla variabile di ambiente `%USERPROFILE%` per l'account che ha creato l'istanza.  

 ENCRYPTION BY PASSWORD = '*encryption_password*'  
 Password utilizzata per crittografare la chiave privata prima di scriverla nel file di backup. Questa password è soggetta ai controlli di complessità delle password.  
  
 DECRYPTION BY PASSWORD = '*decryption_password*'  
 Password utilizzata per decrittografare la chiave privata prima di eseguire il backup della chiave. Questo argomento non è necessario se il certificato viene crittografato tramite la chiave master. 
  
## <a name="remarks"></a>Commenti  
 Se la chiave privata è crittografata con una password nel database, è necessario specificare la password di decrittografia.  
  
 Quando si esegue il backup della chiave privata in un file, la crittografia è obbligatoria. La password usata per proteggere la chiave privata nel file non corrisponde a quella usata per crittografare la chiave privata del certificato nel database.  

 Le chiavi private vengono salvate nel formato di file PVK.

 Per ripristinare il backup di un certificato, con o senza la chiave privata, usare l'istruzione [CREATE CERTIFICATE](../../t-sql/statements/create-certificate-transact-sql.md).
 
 Per ripristinare una chiave privata in un certificato esistente nel database, usare l'istruzione [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md).
 
 Quando si esegue un backup, i file verranno inclusi nell'elenco di controllo di accesso per l'account del servizio dell'istanza di SQL Server. Se è necessario ripristinare il certificato in un server in esecuzione con un account diverso, sarà necessario modificare le autorizzazioni per i file in modo che possano essere letti dal nuovo account. 
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il certificato ed è necessario conoscere la password utilizzata per crittografare la chiave privata. Se si esegue il backup della sola parte pubblica del certificato, questo comando richiede alcune autorizzazioni per il certificato ed è necessario che al chiamante non sia stata negata l'autorizzazione VIEW per il certificato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-exporting-a-certificate-to-a-file"></a>R. Esportazione di un certificato in un file  
 Nell'esempio seguente un certificato viene esportato in un file.  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert';  
GO  
```  
  
### <a name="b-exporting-a-certificate-and-a-private-key"></a>B. Esportazione di un certificato e di una chiave privata  
 Nell'esempio seguente la chiave privata del certificato di cui si esegue il backup verrà crittografata con la password `997jkhUbhk$w4ez0876hKHJH5gh`.  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert'  
    WITH PRIVATE KEY ( FILE = 'c:\storedkeys\sales05key' ,   
    ENCRYPTION BY PASSWORD = '997jkhUbhk$w4ez0876hKHJH5gh' );  
GO  
```  
  
### <a name="c-exporting-a-certificate-that-has-an-encrypted-private-key"></a>C. Esportazione di un certificato con una chiave privata crittografata  
 Nell'esempio seguente la chiave privata del certificato è crittografata nel database ed è necessario decrittografarla con la password `9875t6#6rfid7vble7r`. Quando il certificato viene archiviato nel file di backup, la chiave privata verrà crittografata con la password `9n34khUbhk$w4ecJH5gh`.  
  
```sql
BACKUP CERTIFICATE sales09 TO FILE = 'c:\storedcerts\sales09cert'   
    WITH PRIVATE KEY ( DECRYPTION BY PASSWORD = '9875t6#6rfid7vble7r' ,  
    FILE = 'c:\storedkeys\sales09key' ,   
    ENCRYPTION BY PASSWORD = '9n34khUbhk$w4ecJH5gh' );  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

