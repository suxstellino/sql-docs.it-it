---
description: CRYPT_GEN_RANDOM (Transact-SQL)
title: CRYPT_GEN_RANDOM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CRYPT_GEN_RANDOM_TSQL
- CRYPT_GEN_RANDOM
dev_langs:
- TSQL
helpviewer_keywords:
- CRYPT_GEN_RANDOM function
ms.assetid: b74bd9d4-758e-4b94-89a0-76dcda6d8c42
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7c0381c2348174e5d3663d0b38a44714a96df621
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184152"
---
# <a name="crypt_gen_random-transact-sql"></a>CRYPT_GEN_RANDOM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce un numero generato casualmente di crittografia, generato da CryptoAPI (CAPI). `CRYPT_GEN_RANDOM` restituisce un numero esadecimale di lunghezza pari a un numero specificato di byte.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CRYPT_GEN_RANDOM ( length [ , seed ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*length*  
Lunghezza, in byte, del numero che verrà creato da `CRYPT_GEN_RANDOM`. L'argomento *length* ha un tipo di dati **int** e un intervallo di valori compreso tra 1 e 8000. `CRYPT_GEN_RANDOM` restituisce NULL per un valore **int** non compreso in questo intervallo. 
  
*seed*  
Numero esadecimale facoltativo, da usare come valore di inizializzazione casuale. La lunghezza di *seed* deve corrispondere al valore dell'argomento *length*. L'argomento *seed* ha un tipo di dati **varbinary(8000)**.
  
## <a name="returned-types"></a>Tipi restituiti  
**varbinary(8000)**
  
## <a name="permissions"></a>Autorizzazioni  
Questa funzione è pubblica e non richiede autorizzazioni speciali.
  
## <a name="examples"></a>Esempi  
  
### <a name="a-generating-a-random-number"></a>R. Generazione di un numero casuale  
Questo esempio genera un numero casuale di lunghezza pari a 50 byte:
  
```sql
SELECT CRYPT_GEN_RANDOM(50) ;  
```  
  
Questo esempio genera un numero casuale di lunghezza pari a 4 byte, usando un valore di inizializzazione a 4 byte:
  
```sql
SELECT CRYPT_GEN_RANDOM(4, 0x25F18060) ;  
```  
  
## <a name="see-also"></a>Vedere anche
[RAND &#40;Transact-SQL&#41;](../../t-sql/functions/rand-transact-sql.md)
  
  
