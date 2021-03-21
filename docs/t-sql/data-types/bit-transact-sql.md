---
description: bit (Transact-SQL)
title: bit (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bit_TSQL
- bit
dev_langs:
- TSQL
helpviewer_keywords:
- bit data type
ms.assetid: 40adfd08-a31c-49cb-a172-386bcaa6edee
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5787d6d100eaa860313de2b911675d3061b9355b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735811"
---
# <a name="bit-transact-sql"></a>bit (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Tipo di dati integer che può accettare un valore di 1, 0 o NULL.  
  
## <a name="remarks"></a>Commenti  
[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ottimizza l'archiviazione delle colonne di tipo **bit**. Se una tabella contiene al massimo 8 colonne di tipo **bit**, le colonne vengono archiviate come singolo byte. Se la tabella contiene da 9 a 16 colonne di tipo **bit**, le colonne vengono archiviate in 2 byte e così via.
  
I valori di stringa TRUE e FALSE possono essere convertiti in valori di tipo **bit**: TRUE viene convertito in 1 e FALSE viene convertito in 0.
  
Nelle conversioni nel tipo di dati bit i valori diversi da zero vengono promossi a 1.
  
## <a name="see-also"></a>Vedere anche
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Conversione del tipo di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
