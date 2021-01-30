---
description: DECRYPTBYCERT (Transact-SQL)
title: DECRYPTBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DecryptByCert_TSQL
- DECRYPTBYCERT
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], decryption
- decryption [SQL Server], certificates
- DECRYPTBYCERT function
ms.assetid: 4950d787-40fa-4e26-bce8-2cb2ceca12fb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 792f4ec052b038e53565648f63fcbfb94144f49d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193720"
---
# <a name="decryptbycert-transact-sql"></a>DECRYPTBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione usa la chiave privata di un certificato per decrittografare i dati crittografati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DecryptByCert ( certificate_ID , { 'ciphertext' | @ciphertext }   
    [ , { 'cert_password' | @cert_password } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *certificate_ID*  
ID di un certificato nel database. *certificate_ID* ha un tipo di dati **int**.  
  
 *ciphertext*  
Stringa di dati crittografata tramite la chiave pubblica del certificato.  
  
 @ciphertext  
Variabile di tipo **varbinary** contenente dati crittografati con il certificato.  
  
 *cert_password*  
Password usata per crittografare la chiave privata del certificato. *cert_password* deve avere un formato di dati Unicode.  
  
 @cert_password  
Variabile di tipo **nchar** o **nvarchar** contenente la password usata per crittografare la chiave privata del certificato. *\@cert_password* deve avere un formato di dati Unicode.  

## <a name="return-types"></a>Tipi restituiti  
**varbinary** con un valore massimo di 8.000 byte.  
  
## <a name="remarks"></a>Osservazioni  
Questa funzione decrittografa dati tramite la chiave privata di un certificato. Le trasformazioni di crittografia tramite chiavi asimmetriche richiedono una quantità elevata di risorse. È consigliabile pertanto che gli sviluppatori evitino l'uso di [ENCRYPTBYCERT](./encryptbycert-transact-sql.md) e DECRYPTBYCERT per la crittografia / decrittografia di routine dei dati degli utenti.  

## <a name="permissions"></a>Autorizzazioni  
`DECRYPTBYCERT` richiede l'autorizzazione CONTROL per il certificato.  
  
## <a name="examples"></a>Esempi  
In questo esempio vengono selezionate le righe da `[AdventureWorks2012].[ProtectedData04]` contrassegnate come dati originariamente crittografati dal certificato `JanainaCert02`. Nell'esempio viene prima decrittografata la chiave privata del certificato `JanainaCert02` con la password del certificato `pGFD4bb925DGvbd2439587y`. Viene quindi decrittografato il testo con questa chiave privata. Nell'esempio i dati decrittografati vengono convertiti da **varbinary** a **nvarchar**.  

```sql  
SELECT CONVERT(NVARCHAR(max), DecryptByCert(Cert_Id('JanainaCert02'),  
    ProtectedData, N'pGFD4bb925DGvbd2439587y'))  
FROM [AdventureWorks2012].[ProtectedData04]   
WHERE Description   
    = N'data encrypted by certificate '' JanainaCert02''';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ENCRYPTBYCERT &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbycert-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
