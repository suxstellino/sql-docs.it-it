---
description: CHECKSUM_AGG (Transact-SQL)
title: CHECKSUM_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKSUM_AGG
- CHECKSUM_AGG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checksum values
- CHECKSUM_AGG function
- groups [SQL Server], checksum values
ms.assetid: cdede70c-4eb5-4c92-98ab-b07787ab7222
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bea0aad7839bc0b487b77b6ac1434396b84b765
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102634"
---
# <a name="checksum_agg-transact-sql"></a>CHECKSUM_AGG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce il valore di checksum dei valori di un gruppo. `CHECKSUM_AGG` ignora i valori Null. La [clausola OVER](../../t-sql/queries/select-over-clause-transact-sql.md) può seguire `CHECKSUM_AGG`.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CHECKSUM_AGG ( [ ALL | DISTINCT ] expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
**ALL**  
Applica la funzione di aggregazione a tutti i valori. L'argomento predefinito è ALL.
  
DISTINCT  
Specifica che `CHECKSUM_AGG` restituisce il valore di checksum di valori univoci.
  
*expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) integer. `CHECKSUM_AGG` non consente l'uso di funzioni o sottoquery di aggregazione.
  
## <a name="return-types"></a>Tipi restituiti
Restituisce il valore di checksum di tutti i valori di *expression* come valore **int**.
  
## <a name="remarks"></a>Commenti  
`CHECKSUM_AGG` consente di rilevare le modifiche in una tabella.
  
Il risultato `CHECKSUM_AGG` non dipende dall'ordine delle righe nella tabella. Le funzioni `CHECKSUM_AGG` consentono anche l'uso della parola chiave `DISTINCT` e della clausola `GROUP BY`.
  
Se un valore di elenco di espressioni cambia, è probabile che cambi anche l'elenco di valori di checksum dell'elenco. Esiste tuttavia una minima possibilità che il valore di checksum calcolato non verrà modificato.
  
`CHECKSUM_AGG` offre una funzionalità simile a quella di altre funzioni di aggregazione. Per altre informazioni, vedere [Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md).
  
## <a name="examples"></a>Esempi  
Negli esempi seguenti viene usata la funzione `CHECKSUM_AGG` per rilevare le modifiche apportate nella colonna `Quantity` della tabella `ProductInventory` inclusa nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
--Get the checksum value before the column value is changed.  

SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
262  
```  
  
```sql
UPDATE Production.ProductInventory   
SET Quantity=125  
WHERE Quantity=100;  
GO  

--Get the checksum of the modified column.  
SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
287  
```  
  
## <a name="see-also"></a>Vedi anche
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
[Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
