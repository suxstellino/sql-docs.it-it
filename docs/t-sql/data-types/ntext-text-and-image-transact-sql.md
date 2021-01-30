---
description: ntext, text e image (Transact-SQL)
title: ntext, text e image (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ntext_TSQL
- ntext
dev_langs:
- TSQL
helpviewer_keywords:
- text data type, about text data type
- text [SQL Server], data types
- ntext data type
- ntext data type, about ntext data type
- image data type, about image data type
ms.assetid: b0d8769c-7598-4f97-8162-ace5f182b5bc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a7f89f1cb8585befbcd04aa03cdf8309cbe07d10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187901"
---
# <a name="ntext-text-and-image-transact-sql"></a>ntext, text e image (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Tipi di dati a lunghezza fissa e variabile per l'archiviazione di dati di tipo binario e carattere sia Unicode che non Unicode di dimensioni elevate. Nei dati Unicode viene utilizzato il set di caratteri UNICODE UCS-2.
  
>**IMPORTANTE**  I tipi di dati **ntext**, **text** e **image** verranno rimossi in una versione futura di SQL Server. Evitare di utilizzare questi tipi di dati in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente li utilizzano. Usare in alternativa [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)e [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) .  
  
## <a name="arguments"></a>Argomenti
**ntext**  
Dati Unicode a lunghezza variabile con lunghezza massima della stringa di 2^30 - 1 (1.073.741.823) byte. Le dimensioni dello spazio di archiviazione, espresse in byte, sono pari al doppio della lunghezza della stringa immessa. Il sinonimo ISO per **ntext** è **national text**.
  
**text**  
Dati non Unicode a lunghezza variabile nella tabella codici del server con lunghezza massima della stringa di 2^31-1 (2.147.483.647). Quando nella tabella codici del server vengono utilizzati caratteri DBCS, lo spazio di archiviazione è sempre pari a 2.147.483.647 byte. In base alla stringa di caratteri, le dimensioni dello spazio di archiviazione possono essere minori di 2.147.483.647 byte.
  
**image**  
Dati binari a lunghezza variabile da 0 a 2^31-1 (2.147.483.647) byte.
  
## <a name="remarks"></a>Osservazioni  
Con dati di tipo **ntext**, **text** o **image** è possibile usare le funzioni e le istruzioni seguenti.
  
|Funzioni|Istruzioni|  
|---|---|
|[DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)|[READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)|  
|[PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)|[SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)|  
|[SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)|[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)|  
|[TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)|[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)|  
|[TEXTVALID &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)||  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversione del tipo di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)

