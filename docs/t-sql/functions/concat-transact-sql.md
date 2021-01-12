---
description: CONCAT (Transact-SQL)
title: CONCAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONCAT
- CONCAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT function
ms.assetid: fce5a8d4-283b-4c47-95e5-4946402550d5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e48b358b33276a42d4c4f9c1cc0915238434e02d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98088474"
---
# <a name="concat-transact-sql"></a>CONCAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce una stringa risultante dalla concatenazione o unione in join end-to-end di due o più valori di stringa. Per l'aggiunta di un valore di separazione durante la concatenazione, vedere [CONCAT_WS](../../t-sql/functions/concat-ws-transact-sql.md).
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CONCAT ( string_value1, string_value2 [, string_valueN ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*string_value*  
Valore stringa da concatenare agli altri valori. La funzione `CONCAT` richiede almeno due, ma non più di 254, argomenti string_value.
  
## <a name="return-types"></a>Tipi restituiti  
*string_value*  
Valore stringa la cui lunghezza e tipo dipendono dall'input.
  
## <a name="remarks"></a>Osservazioni  
`CONCAT` accetta un numero variabile di argomenti stringa e li concatena in una singola stringa. Richiede un minimo di due valori di input. In caso contrario, `CONCAT` genera un errore. `CONCAT` converte in modo implicito tutti gli argomenti nei tipi di stringa prima della concatenazione. `CONCAT` converte in modo implicito i valori Null in stringhe vuote. Se `CONCAT` riceve argomenti con tutti i valori **NULL**, restituisce una stringa vuota di tipo **varchar**(1). Per la conversione implicita in stringhe vengono seguite le regole esistenti per le conversioni dei tipi di dati. Per altre informazioni sulle conversioni dei tipi di dati, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
Il tipo restituito dipende dal tipo degli argomenti. Nella tabella seguente viene illustrato il mapping:
  
|Tipo di input|Tipo di output e lunghezza|  
|---|---|
|1. Qualsiasi argomento di<br><br />un tipo di sistema CLR SQL<br><br />un UDT CLR SQL<br><br />o<br><br />`nvarchar(max)`|**nvarchar(max)**|  
|2. In alternativa, qualsiasi argomento di tipo<br><br />**varbinary(max)**<br><br />o<br><br />**ntext**|**varchar(max)** , a meno che uno dei parametri non sia un tipo **nvarchar** di qualsiasi lunghezza. In questo caso `CONCAT` restituisce un risultato di tipo **nvarchar(max)**.|  
|3. In alternativa, qualsiasi argomento di tipo **nvarchar** di 4000 caratteri al massimo<br><br />( **nvarchar**(<= 4000) )|**nvarchar**(<= 4000)|  
|4. In tutti gli altri casi|**varchar**(<= 8000) (un elemento **varchar** di 8000 caratteri al massimo), a meno che uno dei parametri non sia un tipo nvarchar di qualsiasi lunghezza. In questo caso, `CONCAT` restituisce un risultato di tipo **nvarchar(max)**.|  
  
Quando `CONCAT` riceve argomenti di input **nvarchar** di lunghezza < = 4000 caratteri, o argomenti di input **varchar** di lunghezza < = 8000 caratteri, le conversioni implicite possono influire sulla lunghezza del risultato. Gli altri tipi di dati hanno lunghezze diverse quando vengono convertiti in modo implicito in stringhe. Ad esempio un tipo **int** (14) ha una lunghezza stringa 12, mentre un tipo **float** ha una lunghezza stringa 32. Pertanto, una concatenazione di due interi restituisce un risultato con una lunghezza non inferiore a 24.
  
Se nessuno degli argomenti di input è di un tipo LOB supportato, la lunghezza del tipo restituito viene troncata a 8000 caratteri, indipendentemente dal tipo restituito. Questo troncamento consente di risparmiare spazio e di generare il piano in modo efficiente.
  
La funzione CONCAT può essere eseguita in modalità remota su un server collegato versione [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e successive. Per i server collegati precedenti, CONCAT viene eseguita in locale dopo che il server collegato restituisce i valori non concatenati.
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-concat"></a>R. Utilizzo di CONCAT  
  
```sql
SELECT CONCAT ( 'Happy ', 'Birthday ', 11, '/', '25' ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
-------------------------  
Happy Birthday 11/25  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-concat-with-null-values"></a>B. Utilizzo di CONCAT con valori NULL  
  
```sql
CREATE TABLE #temp (  
    emp_name NVARCHAR(200) NOT NULL,  
    emp_middlename NVARCHAR(200) NULL,  
    emp_lastname NVARCHAR(200) NOT NULL  
);  
INSERT INTO #temp VALUES( 'Name', NULL, 'Lastname' );  
SELECT CONCAT( emp_name, emp_middlename, emp_lastname ) AS Result  
FROM #temp;  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
------------------  
NameLastname  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  


