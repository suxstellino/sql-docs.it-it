---
description: HASHBYTES (Transact-SQL)
title: HASHBYTES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fae7a123c69f485dc1f398a877f98249367eb85c
ms.sourcegitcommit: 14f2051d329b69a7b5ff7bce1d136cf7f25bb219
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2021
ms.locfileid: "106232128"
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

  Restituisce l'hash MD2, MD4, MD5, SHA1 o SHA2 del relativo input in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)   
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

`<algorithm>`  
Identifica l'algoritmo di hash da utilizzare per eseguire l'hashing dell'input. Si tratta di un argomento obbligatorio in assenza di impostazioni predefinite. Le virgolette singole sono obbligatorie. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], tutti gli algoritmi diversi da SHA2_256 e SHA2_512 sono deprecati.  
  
`@input`  
Specifica una variabile contenente i dati di cui eseguire l'hashing. `@input` è di tipo **varchar**, **nvarchar** o **varbinary**.  
  
'*input*'  
Specifica un'espressione che restituisce un carattere o una stringa binaria di cui eseguire l'hashing.  
  
 L'output è conforme allo standard dell'algoritmo, ovvero 128 bit (16 byte) per MD2, MD4 e MD5 e 160 bit (20 byte) per SHA e SHA1; 256 bit (32 byte) per SHA2_256 e 512 bit (64 byte) per SHA2_512.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive
  
 Per [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni precedenti, la dimensione dei valori di input consentiti è limitata a 8.000 byte.  
  
## <a name="return-value"></a>Valore restituito  
 **varbinary** (non più di 8000 byte)  

## <a name="remarks"></a>Commenti  
È consigliabile usare `CHECKSUM` o `BINARY_CHECKSUM` come alternative per calcolare un valore hash.

Gli algoritmi MD2, MD4, MD5, SHA e SHA1 sono deprecati a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Usare invece SHA2_256 o SHA2_512. Gli algoritmi precedenti continueranno a funzionare, ma genereranno un evento Deprecation.

## <a name="examples"></a>Esempi  
### <a name="return-the-hash-of-a-variable"></a>Restituire l'hash di una variabile  
 Nell'esempio seguente viene restituito l'hash `SHA2_256` dei dati **nvarchar** archiviati nella variabile `@HashThis`.  
  
```sql  
DECLARE @HashThis NVARCHAR(32);  
SET @HashThis = CONVERT(NVARCHAR(32),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA2_256', @HashThis);  
```  
  
### <a name="return-the-hash-of-a-table-column"></a>Restituire l'hash di una colonna di tabella  
 Nell'esempio seguente viene restituito l'hash SHA2_256 dei valori della colonna `c1` nella tabella `Test1`.  
  
```sql  
CREATE TABLE dbo.Test1 (c1 NVARCHAR(32));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA2_256', c1) FROM dbo.Test1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------------  
0x741238C01D9DB821CF171BF61D72260B998F7C7881D90091099945E0B9E0C2E3 
0x91DDCC41B761ACA928C62F7B0DA61DC763255E8247E0BD8DCE6B22205197154D  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
[Scegliere un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
