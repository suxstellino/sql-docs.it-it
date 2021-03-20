---
title: CHARINDEX (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione CHARINDEX.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHARINDEX
- CHARINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], pattern searching
- CHARINDEX function
- pattern searching [SQL Server]
- starting point of expression in character string
ms.assetid: 78c10341-8373-4b30-b404-3db20e1a3ac4
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1ec1919c5b9b60e4109b7f76463dab21fd1e50b3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750001"
---
# <a name="charindex-transact-sql"></a>CHARINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione esegue la ricerca dell'espressione di un carattere all'interno di una seconda espressione di caratteri, e restituisce la posizione iniziale della prima espressione, se trovata.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*expressionToFind*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di caratteri contenente la sequenza da trovare. *expressionToFind* ha un limite di 8000 caratteri.
  
*expressionToSearch*  
Espressione di caratteri da cercare.
  
*start_location*  
Espressione **integer** o **bigint** in corrispondenza della quale inizia la ricerca. Se *start_location* viene omesso, è un numero negativo oppure è uguale a zero, la ricerca viene avviata all'inizio di *expressionToSearch*.
  
## <a name="return-types"></a>Tipi restituiti
**bigint** se *expressionToSearch* è del tipo di dati **varchar(max)** , **varbinary(max)** o **varchar(max)** . In caso contrario, **int**.
  
## <a name="remarks"></a>Osservazioni  
Se l'espressione *expressionToFind* o *expressionToSearch* ha un tipo di dati Unicode (**nchar** oppure **nvarchar**) e l'altra espressione non ha tale tipo di dati, la funzione CHARINDEX converte l'altra espressione in un tipo di dati Unicode. Non è possibile usare CHARINDEX con i tipi di dati **image**, **ntext**, o **text**.
  
Se l'espressione *expressionToFind* o *expressionToSearch* è NULL, CHARINDEX restituisce NULL.
  
Se CHARINDEX non trova *expressionToFind* in *expressionToSearch*, restituisce 0.
  
CHARINDEX esegue confronti in base alle regole di confronto dell'input. Per eseguire un confronto in base a regole di confronto specifiche, è possibile usare COLLATE per applicare regole di confronto esplicite all'input.
  
La posizione di inizio restituita è in base 1 e non in base 0.
  
0x0000 (**char(0)** ) è un carattere non definito nelle regole di confronto di Windows e non può essere incluso in CHARINDEX.
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
Quando si usano le regole di confronto SC, *start_location* e il valore restituito conteggiano le coppie di surrogati come un carattere, non due. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-starting-position-of-an-expression"></a>R. Restituzione della posizione iniziale di un'espressione  
In questo esempio viene cercato l'elemento `bicycle` nella variabile di valore stringa ricercata `@document`.
  
```sql
DECLARE @document VARCHAR(64);  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bicycle', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
48            
```  
  
### <a name="b-searching-from-a-specific-position"></a>B. Ricerca da una posizione specifica  
Nell'esempio seguente viene usato il parametro facoltativo *start_location* per avviare la ricerca di `vital` in corrispondenza del quinto carattere della variabile di valore stringa ricercata `@document`.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('vital', @document, 5);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
16            
  
(1 row(s) affected)  
```  
  
### <a name="c-searching-for-a-nonexistent-expression"></a>C. Ricerca di un'espressione inesistente  
Nell'esempio seguente viene illustrato il set di risultati ottenuto quando CHARINDEX non trova *expressionToFind* in *expressionToSearch*.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bike', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
  
(1 row(s) affected)
```
  
### <a name="d-performing-a-case-sensitive-search"></a>D. Esecuzione di una ricerca con distinzione tra maiuscole e minuscole  
Nell'esempio seguente viene eseguita una ricerca con distinzione tra maiuscole e minuscole della stringa `'TEST'` nella stringa `'This is a Test``'`.
  
```sql
USE tempdb;  
GO  
--perform a case sensitive search  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
```  
  
Nell'esempio seguente viene eseguita una ricerca con distinzione tra maiuscole e minuscole della stringa `'Test'` in `'This is a Test'`.
  
```sql
  
USE tempdb;  
GO  
SELECT CHARINDEX ( 'Test',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
### <a name="e-performing-a-case-insensitive-search"></a>E. Esecuzione di una ricerca senza distinzione tra maiuscole e minuscole  
Nell'esempio seguente viene eseguita una ricerca senza distinzione tra maiuscole e minuscole della stringa `'TEST'` in `'This is a Test'`.
  
```sql
USE tempdb;  
GO  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CI_AS);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-searching-from-the-start-of-a-string-expression"></a>F. Ricerca dall'inizio di un'espressione stringa  
Nell'esempio seguente viene restituita la prima posizione della stringa `is` nella stringa `This is a string`, a partire dalla posizione 1 (il primo carattere) di `This is a string`.
  
```sql
SELECT CHARINDEX('is', 'This is a string');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
3
```  
  
### <a name="g-searching-from-a-position-other-than-the-first-position"></a>G. Ricerca da una posizione diversa dalla prima posizione  
Nell'esempio seguente viene restituita la prima posizione della stringa `is` nella stringa `This is a string`, a partire dalla posizione 4 (il quarto carattere).
  
```sql
SELECT CHARINDEX('is', 'This is a string', 4);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
 6
 ```  
  
### <a name="h-results-when-the-string-is-not-found"></a>H. Risultati quando la stringa non viene trovata  
L'esempio seguente illustra il valore restituito quando CHARINDEX non trova la stringa *string_pattern* nella stringa cercata.
  
```sql
SELECT TOP(1) CHARINDEX('at', 'This is a string') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
0
```  
  
## <a name="see-also"></a>Vedere anche
 [LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
 [+ &#40;String Concatenation&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  


