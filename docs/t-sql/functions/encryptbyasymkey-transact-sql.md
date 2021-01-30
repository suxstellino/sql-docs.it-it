---
description: ENCRYPTBYASYMKEY (Transact-SQL)
title: ENCRYPTBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYASYMKEY
- ENCRYPTBYASYMKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYASYMKEY function
- encryption [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], ENCRYPTBYASYMKEY function
ms.assetid: 86bb2588-ab13-4db2-8f3c-42c9f572a67b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3c947213e205a823e16fd2f83256c452c6da0f19
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158333"
---
# <a name="encryptbyasymkey-transact-sql"></a>ENCRYPTBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione crittografa i dati con una chiave asimmetrica.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
EncryptByAsymKey ( Asym_Key_ID , { 'plaintext' | @plaintext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*asym_key_ID*  
ID di una chiave asimmetrica nel database. *asym_key_ID* ha un tipo di dati **int**.  
  
*cleartext*  
Stringa di dati che verrà crittografata da `ENCRYPTBYASYMKEY` con la chiave asimmetrica. *cleartext* può avere
 
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
o
  
+ **varchar**
 
come tipo di dati.  
  
**\@plaintext**  
Variabile contenente un valore che verrà crittografato da `ENCRYPTBYASYMKEY` con la chiave asimmetrica. **\@plaintext** può avere
  
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
o
  
+ **varchar**
 
come tipo di dati.  
  
## <a name="return-types"></a>Tipi restituiti  
**varbinary** con un valore massimo di 8.000 byte.  
  
## <a name="remarks"></a>Osservazioni  
Le operazioni di crittografia e decrittografia che usano chiavi asimmetriche usano una quantità elevata di risorse e diventano quindi molto costose rispetto alla crittografia e alla decrittografia a chiavi simmetriche. È consigliabile che gli sviluppatori evitino le operazioni di crittografia e decrittografia con chiavi asimmetriche su set di dati di grandi dimensioni, ad esempio nel caso di set di dati utente archiviati in tabelle di database. Si consiglia invece agli sviluppatori di crittografare prima i dati con una chiave simmetrica avanzata e quindi di crittografare tale chiave simmetrica con una chiave asimmetrica.  
  
A seconda dell'algoritmo, `ENCRYPTBYASYMKEY` restituisce **NULL** se l'input supera un determinato numero di byte. Limiti specifici:

+ una chiave RSA a 512 bit può crittografare fino a 53 byte
+ una chiave a 1024 bit può crittografare fino a 117 byte
+ una chiave a 2048 bit può crittografare fino a 245 byte

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia i certificati che le chiavi asimmetriche fungono da wrapper sulle chiavi RSA.  
  
## <a name="examples"></a>Esempi  
Questo esempio crittografa il testo archiviato in `@cleartext` con la chiave asimmetrica `JanainaAsymKey02`. L'istruzione inserisce i dati crittografati nella tabella `ProtectedData04`.  
  
```sql  
INSERT INTO AdventureWorks2012.Sales.ProtectedData04   
    VALUES( N'Data encrypted by asymmetric key ''JanainaAsymKey02''',  
    EncryptByAsymKey(AsymKey_ID('JanainaAsymKey02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DECRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
