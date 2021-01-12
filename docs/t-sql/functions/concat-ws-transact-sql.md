---
description: CONCAT_WS (Transact-SQL)
title: CONCAT_WS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- CONCAT_WS
- CONCAT_WS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT_WS function
ms.assetid: f1375fd7-a2fd-48bf-922a-4f778f0deb1f
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: dd7dec83f385c17055548626b2232e65af02ab09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101304"
---
# <a name="concat_ws-transact-sql"></a>CONCAT_WS (Transact-SQL)
[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Questa funzione restituisce una stringa risultante dalla concatenazione o unione in join end-to-end di due o più valori di stringa. Separa tali valori concatenati della stringa con il delimitatore specificato nel primo argomento della funzione. (`CONCAT_WS` indica *concatenare con separatore*.)

##  <a name="syntax"></a>Sintassi   
```syntaxsql
CONCAT_WS ( separator, argument1, argument2 [, argumentN]... )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*separator* Espressione di qualsiasi tipo di carattere (`char`, `nchar`, `nvarchar` o `varchar`).

*argument1, argument2, argumentN* Espressione di qualsiasi tipo. La funzione `CONCAT_WS` richiede almeno due argomenti e non più di 254 argomenti.

## <a name="return-types"></a>Tipi restituiti
Valore stringa la cui lunghezza e tipo dipendono dall'input.

## <a name="remarks"></a>Osservazioni   
`CONCAT_WS` accetta un numero variabile di argomenti stringa e li concatena in una singola stringa. Separa tali valori concatenati della stringa con il delimitatore specificato nel primo argomento della funzione. `CONCAT_WS` richiede un argomento separatore e almeno altri due argomenti dei valori della stringa. In caso contrario, `CONCAT_WS` genererà un errore. `CONCAT_WS` converte in modo implicito tutti gli argomenti nei tipi di stringa prima della concatenazione. 

Per la conversione implicita in stringhe vengono seguite le regole esistenti per le conversioni dei tipi di dati. Per altre informazioni sul comportamento e le conversioni dei tipi di dati, vedere [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md).

### <a name="treatment-of-null-values"></a>Trattamento dei valori NULL

`CONCAT_WS` ignora l'impostazione `SET CONCAT_NULL_YIELDS_NULL {ON|OFF}`.

Se `CONCAT_WS` riceve argomenti con tutti i valori NULL, restituisce una stringa vuota di tipo varchar(1).

`CONCAT_WS` ignora i valori Null durante la concatenazione e non aggiunge il separatore tra i valori Null. `CONCAT_WS` può quindi gestire correttamente la concatenazione di stringhe che potrebbero avere valori "vuoti", ad esempio il campo di un secondo indirizzo. Per altre informazioni, vedere l'esempio B.

Se uno scenario include valori Null separati da un delimitatore, prendere in considerazione la funzione `ISNULL`. Per altre informazioni, vedere l'esempio C.

## <a name="examples"></a>Esempi   

### <a name="a--concatenating-values-with-separator"></a>R.  Concatenazione di valori con separatore
Questo esempio illustra come concatenare tre colonne della tabella sys.databases, separando i valori con un `-`.   

```sql
SELECT CONCAT_WS( ' - ', database_id, recovery_model_desc, containment_desc) AS DatabaseInfo
FROM sys.databases;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

|DatabaseInfo |  
|---------|
|1 - SIMPLE - NONE |
|2 - SIMPLE - NONE |
|3 - FULL - NONE |
|4 - SIMPLE - NONE |


### <a name="b--skipping-null-values"></a>B.  Ignorare i valori Null
Questo esempio ignora i valori `NULL` nell'elenco degli argomenti.

```sql
SELECT CONCAT_WS(',','1 Microsoft Way', NULL, NULL, 'Redmond', 'WA', 98052) AS Address;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

```
Address
------------   
1 Microsoft Way,Redmond,WA,98052
```

### <a name="c--generating-csv-file-from-table"></a>C.  Generazione di file CSV da tabella
Questo esempio usa una virgola `,` come valore separatore e aggiunge il carattere di ritorno a capo `char(13)` nel formato dei valori separati da virgola del set di risultati.

```sql
SELECT 
STRING_AGG(CONCAT_WS( ',', database_id, recovery_model_desc, containment_desc), char(13)) AS DatabaseInfo
FROM sys.databases
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

```
DatabaseInfo
------------   
1,SIMPLE,NONE
2,SIMPLE,NONE
3,FULL,NONE 
4,SIMPLE,NONE 
```

CONCAT_WS ignora i valori NULL nelle colonne. Eseguire il wrapping di una colonna che ammette i valori Null con la funzione `ISNULL` e specificare un valore predefinito. Per altre informazioni, vedere questo esempio:

```sql
SELECT 
STRING_AGG(CONCAT_WS( ',', database_id, ISNULL(recovery_model_desc,''), ISNULL(containment_desc,'N/A')), char(13)) AS DatabaseInfo
FROM sys.databases;
```

## <a name="see-also"></a>Vedere anche
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  

