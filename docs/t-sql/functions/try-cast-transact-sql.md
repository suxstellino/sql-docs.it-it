---
description: TRY_CAST (Transact-SQL)
title: TRY_CAST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRY_CAST_TSQL
- TRY_CAST
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CAST function
ms.assetid: ea3a16de-995b-415c-b5f0-9355cf7bb401
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016 ||>= sql-server-linux-2017||>= aps-pdw-2016||= azure-sqldw-latest
ms.openlocfilehash: d431626368ec87622bed482ca19bade8c4700231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211341"
---
# <a name="try_cast-transact-sql"></a>TRY_CAST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un cast del valore nel tipo di dati specificato se il cast ha esito positivo. In caso contrario, restituisce Null.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TRY_CAST ( expression AS data_type [ ( length ) ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Valore di cui eseguire il cast. Qualsiasi espressione valida.  
  
 *data_type*  
 Tipo di dati in cui eseguire il cast di *expression*.  
  
 *length*  
 Numero intero facoltativo con cui si specifica la lunghezza del tipo di dati di destinazione.  
  
 L'intervallo di valori accettabili è determinato dal valore di *data_type*.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce un cast del valore nel tipo di dati specificato se il cast ha esito positivo. In caso contrario, restituisce Null.  
  
## <a name="remarks"></a>Commenti  
 **TRY_CAST** accetta il valore passato e prova a convertirlo nel tipo di dati *data_type* specificato. Se il cast ha esito positivo, **TRY_CAST** restituisce il valore come elemento *data_type* specificato. Se si verifica un errore, viene restituito Null. Se tuttavia si richiede una conversione non consentita in modo esplicito, **TRY_CAST** ha esito negativo e viene restituito un errore.  
  
 **TRY_CAST** non è una nuova parola chiave riservata ed è disponibile in tutti i livelli di compatibilità. La semantica di **TRY_CAST** è uguale a quella di **TRY_CONVERT** quando si esegue la connessione a server remoti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-try_cast-returns-null"></a>R. Restituzione del valore Null da parte di TRY_CAST  
 Nell'esempio seguente viene dimostrato che se il cast ha esito negativo viene restituito il valore Null da parte di TRY_CAST.  
  
```sql  
SELECT   
    CASE WHEN TRY_CAST('test' AS float) IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 Nell'esempio seguente viene dimostrato che l'espressione deve essere nel formato previsto.  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-try_cast-fails-with-an-error"></a>B. Esito negativo di TRY_CAST con visualizzazione di un errore  
 Nell'esempio seguente viene dimostrato che se il cast non è consentito in modo esplicito viene restituito un errore da parte di TRY_CAST.  
  
```sql  
SELECT TRY_CAST(4 AS xml) AS Result;  
GO  
```  
  
 Il risultato di questa istruzione è un errore perché il cast di un tipo di dati Integer non può essere eseguito nel tipo di dati xml.  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-try_cast-succeeds"></a>C. Esito positivo di TRY_CAST  
 In questo esempio viene dimostrato che l'espressione deve essere nel formato previsto.  
  
```sql
SET DATEFORMAT mdy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [TRY_CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
