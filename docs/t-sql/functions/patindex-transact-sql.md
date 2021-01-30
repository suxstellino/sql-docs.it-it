---
description: PATINDEX (Transact-SQL)
title: PATINDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PATINDEX
- PATINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- first occurrence of pattern [SQL Server]
- searches [SQL Server], pattern starting position
- starting position of patten search
- pattern searching [SQL Server]
- PATINDEX function
ms.assetid: c0dfb17f-2230-4e36-98da-a9b630bab656
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2433b4bc3e83392bd77c58aed5c4fb48d71971df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99120958"
---
# <a name="patindex-transact-sql"></a>PATINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce la posizione di inizio della prima occorrenza di un modello in un'espressione specificata, oppure zero se il modello non viene trovato, in tutti i dati di tipo carattere e testo validi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
PATINDEX ( '%pattern%' , expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *pattern*  
 Espressione di caratteri che contiene la sequenza da cercare. È possibile usare i caratteri jolly. Il carattere %, tuttavia, deve precedere e seguire *pattern*, tranne nelle ricerche del primo o dell'ultimo carattere. *pattern* è un'espressione appartenente alla categoria di tipi di dati per stringhe di caratteri. La lunghezza massima di *pattern* è 8000 caratteri.

 > [!NOTE]
 > Anche se le espressioni regolari tradizionali non sono supportate in modo nativo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile ottenere una corrispondenza di modelli complessi simili usando varie espressioni con caratteri jolly. Per informazioni più dettagliate sulla sintassi con caratteri jolly, vedere la documentazione relativa agli [operatori stringa](../../t-sql/language-elements/string-operators-transact-sql.md).
  
 *expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) che in genere indica una colonna in cui viene cercato il modello specificato. *expression* appartiene alla categoria di tipi di dati per stringhe di caratteri.  
  
## <a name="return-types"></a>Tipi restituiti  
**bigint** se *expression* è del tipo di dati **varchar(max)** o **nvarchar(max)**, in caso contrario **int**.  
  
## <a name="remarks"></a>Commenti  
Se *pattern* o *expression* è NULL, PATINDEX restituisce NULL.  
 
La posizione iniziale per PATINDEX è 1.
 
L'istruzione PATINDEX consente di eseguire i confronti in base alle regole di confronto dell'input. Per eseguire un confronto in base a regole di confronto specifiche, è possibile utilizzare COLLATE per applicare regole di confronto esplicite all'input.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
Quando si usano le regole di confronto SC, qualsiasi coppia di surrogati UTF-16 nel parametro *expression* viene considerata come un singolo carattere dal valore restituito. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
0x0000 (**char(0)**) è un carattere non definito nelle regole di confronto di Windows e non può essere incluso in PATINDEX.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-patindex-example"></a>R. Esempio semplice di PATINDEX  
 L'esempio seguente verifica in una stringa di caratteri breve (`interesting data`) la posizione iniziale dei caratteri `ter`.  
  
```sql  
SELECT position = PATINDEX('%ter%', 'interesting data');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
3
```
  
### <a name="b-using-a-pattern-with-patindex"></a>B. Utilizzo di un modello con PATINDEX  
Nell'esempio seguente viene individuata la posizione in cui il modello `ensure` ha inizio in una riga specifica della colonna `DocumentSummary` nella tabella `Document` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT position = PATINDEX('%ensure%',DocumentSummary)  
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
GO   
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
64  
```  
  
Se non si imposta una limitazione per le righe in cui eseguire la ricerca tramite la clausola `WHERE`, la query restituisce tutte le righe della tabella, indicando valori diversi da zero per le righe in cui il modello è stato trovato e zero per tutte le righe in cui la ricerca ha avuto esito negativo.  
  
### <a name="c-using-wildcard-characters-with-patindex"></a>C. Utilizzo di caratteri jolly con PATINDEX  
 Nell'esempio seguente vengono utilizzati i caratteri jolly % e _ per individuare la posizione iniziale del modello `'en'`, seguito da un carattere qualsiasi e `'ure'` nella stringa specificata (l'indice comincia col valore 1):  
  
```sql  
SELECT position = PATINDEX('%en_ure%', 'Please ensure the door is locked!');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
8  
```  
  
Il funzionamento di `PATINDEX` è uguale a quello di `LIKE`, pertanto è possibile utilizzare qualsiasi carattere jolly. Non è necessario racchiudere il modello tra percentuali. `PATINDEX('a%', 'abc')` restituisce 1 e `PATINDEX('%a', 'cba')` restituisce 3.  
  
 A differenza di `LIKE`, `PATINDEX` restituisce una posizione, analogamente a `CHARINDEX`.  

### <a name="d-using-complex-wildcard-expressions-with-patindex"></a>D. Uso di espressioni con caratteri jolly complesse con PATINDEX 
Nell'esempio seguente viene usato l' [operatore stringa](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)`[^]` per trovare la posizione di un carattere diverso da un numero, una lettera o uno spazio.

```sql
SELECT position = PATINDEX('%[^ 0-9A-z]%', 'Please ensure the door is locked!'); 
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
33
```

### <a name="e-using-collate-with-patindex"></a>E. Utilizzo di COLLATE con PATINDEX  
 Nell'esempio seguente viene utilizzata la funzione `COLLATE` per specificare in modo esplicito le regole di confronto dell'espressione indicante il contesto della ricerca.  
  
```sql  
USE tempdb;  
GO  
SELECT PATINDEX ( '%ein%', 'Das ist ein Test'  COLLATE Latin1_General_BIN) ;  
GO  
```  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
9
```

### <a name="f-using-a-variable-to-specify-the-pattern"></a>F. Utilizzo di una variabile per specificare il modello  
L'esempio usa una variabile per passare un valore al parametro *pattern*. In questo esempio viene usato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DECLARE @MyValue VARCHAR(10) = 'safety';   
SELECT position = PATINDEX('%' + @MyValue + '%', DocumentSummary)   
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
22
```  
  
## <a name="see-also"></a>Vedere anche  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [CHARINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/charindex-transact-sql.md)  
 [LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funzioni stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
 [&#40;Wildcard - Character&#40;s&#41; to Match&#41; (Carattere jolly - Corrispondente/i) &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [&#40;Wildcard - Character&#40;s&#41; to Match&#41; (Carattere jolly - Non corrispondente/i) &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)   
 [_ &#40;Carattere jolly per corrispondenze di singoli caratteri&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)   
 [Carattere di percentuale &#40;Caratteri jolly per la corrispondenza&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)  
  
  


