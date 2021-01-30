---
description: DATEFROMPARTS (Transact-SQL)
title: DATEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEFROMPARTS_TSQL
- DATEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATEFROMPARTS function
ms.assetid: 5b885376-87aa-41f1-9e18-04987aead250
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b8d7879bc296864b786f1422fcbd4b8a1d05a23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185332"
---
# <a name="datefromparts-transact-sql"></a>DATEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce un valore **date** che esegue il mapping ai valori year, month e day specificati.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATEFROMPARTS ( year, month, day )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*year*  
Espressione Integer che specifica un anno.
  
*month*  
Espressione Integer che specifica un mese, compreso tra 1 e 12.
  
*day*  
Espressione Integer che specifica un giorno.
  
## <a name="return-types"></a>Tipi restituiti
**date**
  
## <a name="remarks"></a>Osservazioni  
`DATEFROMPARTS` restituisce un valore **date**, con la parte relativa alla data impostata sull'anno, mese e giorno specificati e la parte relativa all'ora impostata sul valore predefinito. Per gli argomenti non validi, `DATEFROMPARTS` genererà un errore. `DATEFROMPARTS` restituisce null se almeno un argomento obbligatorio ha un valore null.
  
Questa funzione consente di gestire la comunicazione remota con i server [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. Non può gestire la comunicazione remota con server precedenti alla versione [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].
  
## <a name="examples"></a>Esempi  
Questo esempio illustrata la funzione `DATEFROMPARTS` in azione.
  
```sql
SELECT DATEFROMPARTS ( 2010, 12, 31 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------  
2010-12-31  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche
[date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)
  
  

