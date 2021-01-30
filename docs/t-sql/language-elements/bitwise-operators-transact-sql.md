---
description: Operatori bit per bit (Transact-SQL)
title: Operatori bit per bit (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], bitwise
- bitwise operators
- bit manipulations
ms.assetid: 2b994cf5-2daa-438a-b8c7-4bd8d451ac8d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 80e8e56ad03f1a3423359adc7803ce44f3e0925f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193393"
---
# <a name="bitwise-operators-transact-sql"></a>Operatori bit per bit (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gli operatori bit per bit eseguono manipolazioni di bit tra due espressioni di uno dei tipi di dati della categoria integer.  
  Gli operatori bit per bit convertono due valori Integer in bit binari ed eseguono l'operazione AND, OR o NOT su ogni bit, producendo un risultato. Quindi il risultato viene convertito in un valore Integer.  
  
  Ad esempio il valore Integer 170 viene convertito nel valore binario 1010 1010.
il valore Integer 75 viene convertito nel valore binario 0100 1011.

|operator|elaborazione bit per bit|
|---- |---- |
|AND <br> Se i bit in qualsiasi posizione sono entrambi 1, il risultato è 1. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 0000 1010 = 10 |
|OPPURE <br> Se uno dei due bit in qualsiasi posizione è 1, il risultato è 1. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 1110 1011 = 235|
|NOT  <br> Inverte il valore bit in ogni posizione di bit. |1010 1010 = 170 <br>----------------- <br>  0101 0101 = 85 |
  
Vedere gli argomenti seguenti:   
* [& &#40;AND bit per bit&#41;](../../t-sql/language-elements/bitwise-and-transact-sql.md)  
* [&= &#40;Assegnazione AND bit per bit&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
* [&#124; &#40;OR bit per bit&#41;](../../t-sql/language-elements/bitwise-or-transact-sql.md)  
* [&#124;= &#40;Assegnazione OR bit per bit&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
* [^ &#40;OR esclusivo bit per bit&#41;](../../t-sql/language-elements/bitwise-exclusive-or-transact-sql.md)  
* [^= &#40;Assegnazione OR esclusivo bit per bit&#41;](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)  
* [~ &#40;NOT bit per bit&#41;](../../t-sql/language-elements/bitwise-not-transact-sql.md)  
  
 Il tipo di dati degli operandi per gli operatori bit per bit può essere uno dei tipi di dati delle categorie Integer o stringa binaria (tranne il tipo **image**). Non è tuttavia consentito che entrambi gli operandi siano di un tipo della categoria stringa binaria. Nella tabella seguente vengono descritti i tipi di dati supportati per gli operandi.  
  
|Operando sinistro|Operando destro|  
|------------------|-------------------|  
|[binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** o **tinyint**|  
|[bit](../../t-sql/data-types/bit-transact-sql.md)|**int**, **smallint**, **tinyint** o **bit**|  
|[bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**bigint**, **int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** o **tinyint**|  
  
## <a name="see-also"></a>Vedere anche  
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)
