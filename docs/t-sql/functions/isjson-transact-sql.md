---
description: ISJSON (Transact-SQL)
title: ISJSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISJSON
- ISJSON_TSQL
helpviewer_keywords:
- ISJSON function
- JSON, validating
ms.assetid: c836f3d3-3e17-44ae-92bf-f341918896c3
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 5fcad8c84a89361be869d535db04303cc91de16f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097161"
---
# <a name="isjson-transact-sql"></a>ISJSON (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Verifica se una stringa include contenuto JSON valido.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
ISJSON ( expression )  
```  
  
## <a name="arguments"></a>Argomenti
 *expression*  
 Stringa da testare.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce 1 se la stringa include contenuto JSON valido. In caso contrario, restituisce 0. Restituisce Null se *expression* è Null.  
  
 Non restituisce errori.  
  
## <a name="remarks"></a>Osservazioni  
 **ISJSON** non controlla l'univocità delle chiavi allo stesso livello.  
  
## <a name="examples"></a>Esempi  
  
### <a name="example-1"></a>Esempio 1  
L'esempio seguente esegue un blocco di istruzioni in modo condizionale se il valore del parametro `@param` include contenuto JSON valido.  
  
```sql  
DECLARE @param <data type>
SET @param = <value>

IF (ISJSON(@param) > 0)  
BEGIN  
     -- Do something with the valid JSON value of @param.  
END
```  
  
### <a name="example-2"></a>Esempio 2  
L'esempio seguente restituisce le righe in cui la colonna `json_col` include contenuto JSON valido.  
  
```sql  
SELECT id, json_col
FROM tab1
WHERE ISJSON(json_col) > 0 
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
  
