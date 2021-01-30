---
description: Precedenza delle regole di confronto
title: Precedenza delle regole di confronto | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- coercible-default collation label
- precedence [SQL Server], collations
- collation sensitive
- collations [SQL Server], precedence
- explicit collation label [SQL Server]
- implicit collation label
- no-collation label
- collation insensitive
- operators [Transact-SQL], collations
- collation labels
- collation coercion rules
- rules [SQL Server], collations
ms.assetid: 58c4e64b-5634-4c29-aa22-33193282dd27
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 167a870a59319ea28c19545d241af6cb2dd943c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184508"
---
# <a name="collation-precedence"></a>Precedenza delle regole di confronto
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  La precedenza delle regole di confronto, definita anche regola di coercizione delle regole di confronto, determina:  
  
-   Le regole di confronto del risultato finale di un'espressione che restituisce una stringa di caratteri.  
  
-   Le regole di confronto utilizzate dagli operatori sensibili alle regole di confronto che utilizzano input di stringhe di caratteri ma non restituiscono stringhe di caratteri, ad esempio gli operatori LIKE e IN.  
  
Le regole sulla precedenza delle regole di confronto vengono applicate solo alle stringhe di caratteri di tipo **char**, **varchar**, **text**, **nchar**, **nvarchar** e **ntext**. Gli oggetti con altri tipi di dati non vengono inclusi nelle valutazioni delle regole di confronto.  
  
## <a name="collation-labels"></a>Etichette per le regole di confronto  
Nella tabella seguente sono elencate e descritte le quattro categorie in base alle quali vengono classificate le regole di confronto di tutti gli oggetti. Il nome di ogni categoria corrisponde all'etichetta per le regole di confronto.  
  
|Etichetta per le regole di confronto|Tipi di oggetto|  
|---------------------|----------------------|  
|Coercible-default|Qualsiasi variabile, parametro, valore letterale di stringhe di caratteri [!INCLUDE[tsql](../../includes/tsql-md.md)] oppure l'output di una funzione di catalogo predefinita o una funzione predefinita che non accetta input di stringhe ma restituisce stringhe.<br /><br /> A un oggetto dichiarato in una funzione definita dall'utente, in una stored procedure o in un trigger vengono assegnate le regole di confronto predefinite del database in cui si crea la funzione, la stored procedure o il trigger. A un oggetto dichiarato in un batch vengono assegnate le regole di confronto predefinite del database corrente per la connessione.|  
|Implicit X|Riferimento di colonna. Le regole di confronto dell'espressione (X) derivano dalle regole di confronto definite per la colonna della tabella o vista.<br /><br /> Sebbene le regole di confronto siano state assegnate alla colonna in modo esplicito tramite una clausola COLLATE nell'istruzione CREATE TABLE o CREATE VIEW, il riferimento di colonna viene classificato comunque come implicito.|  
|Explicit X|Espressione convertita in modo esplicito in regole di confronto specifiche (X) tramite una clausola COLLATE nell'espressione.|  
|No-collation|Indica che il valore di un'espressione è il risultato di un'operazione tra due stringhe con regole di confronto di tipo Implicit in conflitto. Tale espressione viene considerata priva di regole di confronto.|  
  
## <a name="collation-rules"></a>Regole relative alle regole di confronto  
L'etichetta per le regole di confronto di un'espressione semplice che fa riferimento a un solo oggetto stringa di caratteri corrisponde all'etichetta dell'oggetto a cui viene fatto riferimento.  
  
L'etichetta per le regole di confronto di un'espressione complessa che fa riferimento a due espressioni operando aventi la stessa etichetta per le regole di confronto corrisponde all'etichetta di tali espressioni.  
  
L'etichetta per le regole di confronto del risultato finale di un'espressione complessa che fa riferimento a due espressioni operando con regole di confronto diverse viene determinata in base alle regole seguenti:  
  
-   L'etichetta Explicit è prioritaria rispetto all'etichetta Implicit, che a sua volta è prioritaria rispetto all'etichetta Coercible-default.  
  
     Explicit > Implicit > Coercible-default  
  
-   Se si combinano due espressioni di tipo Explicit a cui sono assegnate regole di confronto diverse, viene generato un errore.  
  
     Explicit X + Explicit Y = Errore  
  
-   Se si combinano due espressioni di tipo Implicit a cui sono assegnate regole di confronto diverse, si ottiene l'etichetta per le regole di confronto No-collation.  
  
     Implicit X + Implicit Y = No-collation  
  
-   Se si combina un'espressione di tipo No-collation e un'espressione con qualsiasi etichetta, ad eccezione di Explicit X (vedi punto successivo), al risultato viene associata l'etichetta per le regole di confronto No-collation.  
  
     No-collation + qualsiasi etichetta = No-collation  
  
-   Se si combina un'espressione di tipo No-collation e un'espressione con regole di confronto di tipo Explicit, si ottiene un'espressione con l'etichetta Explicit.  
  
     No-collation + Explicit X = Explicit  
  
Nella tabella seguente vengono riepilogate le regole descritte in precedenza.  
  
|Etichetta di coercizione dell'operando|Explicit X|Implicit X|Coercible-default|No-collation|  
|----------------------------|----------------|----------------|------------------------|-------------------|  
|**Explicit Y**|Genera un errore|Il risultato è Explicit Y|Il risultato è Explicit Y|Il risultato è Explicit Y|  
|**Implicit Y**|Il risultato è Explicit X|Il risultato è No-collation|Il risultato è Implicit Y|Il risultato è No-collation|  
|**Coercible-default**|Il risultato è Explicit X|Il risultato è Implicit X|Il risultato è Coercible-default|Il risultato è No-collation|  
|**No-collation**|Il risultato è Explicit X|Il risultato è No-collation|Il risultato è No-collation|Il risultato è No-collation|  
  
Per la precedenza delle regole di confronto vengono applicate inoltre le regole aggiuntive seguenti:  
  
-   Un'espressione che è già esplicita non può includere più clausole COLLATE. La clausola `WHERE` seguente, ad esempio, non è valida perché viene specificata una clausola `COLLATE` per un'espressione che è già esplicita.  
  
     `WHERE ColumnA = ( 'abc' COLLATE French_CI_AS) COLLATE French_CS_AS`  
  
-   Le conversioni tra tabelle codici per il tipo di dati **text** non sono consentite. Non è possibile eseguire il cast di un'espressione di tipo **text** tra regole di confronto con tabelle codici diverse. Tramite l'operatore di assegnazione non è possibile assegnare valori se alle regole di confronto dell'operando di testo a destra è associata una tabella codice diversa da quella dell'operando di testo a sinistra.  
  
La precedenza delle regole di confronto viene determinata dopo la conversione dei tipi di dati. L'operando da cui derivano le regole di confronto risultanti può essere diverso dall'operando il cui tipo di dati viene applicato al risultato finale. Si consideri, ad esempio, il batch seguente.  
  
```sql  
CREATE TABLE TestTab  
   (PrimaryKey int PRIMARY KEY,  
    CharCol char(10) COLLATE French_CI_AS  
   )  
  
SELECT *  
FROM TestTab  
WHERE CharCol LIKE N'abc'  
```  
  
Il tipo di dati Unicode dell'espressione semplice `N'abc'` ha una precedenza più alta a livello di tipo di dati. Nell'espressione risultante il tipo di dati Unicode viene assegnato a `N'abc'`. L'etichetta delle regole di confronto dell'espressione `CharCol` è Implicit e `N'abc'` è associata a un'etichetta di coercizione di livello inferiore, ovvero Coercible-default. Le regole di confronto utilizzate saranno pertanto le regole `French_CI_AS` di `CharCol`.  
  
### <a name="examples-of-collation-rules"></a>Esempi di regole di confronto  
 Nell'esempio seguente viene illustrato il funzionamento delle regole di confronto. Per eseguire gli esempi descritti, è necessario creare la tabella di prova seguente.  
  
```sql  
USE tempdb;  
GO  
  
CREATE TABLE TestTab (  
   id int,   
   GreekCol nvarchar(10) collate greek_ci_as,   
   LatinCol nvarchar(10) collate latin1_general_cs_as  
   )  
INSERT TestTab VALUES (1, N'A', N'a');  
GO  
```  
  
#### <a name="collation-conflict-and-error"></a>Conflitti ed errori a livello di regole di confronto  
 Nel predicato della query seguente esiste un conflitto di regole di confronto che genera un errore.  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 448, Level 16, State 9, Line 2  
Cannot resolve collation conflict between 'Latin1_General_CS_AS' and 'Greek_CI_AS' in equal to operation.  
```  
  
#### <a name="explicit-label-vs-implicit-label"></a>Etichette Explicit e Implicit  
 Il predicato della query seguente viene valutato nelle regole di confronto `greek_ci_as` perché l'etichetta dell'espressione di destra è Explicit. Questa etichetta ha la priorità sull'etichetta Implicit dell'espressione di sinistra.  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol COLLATE greek_ci_as;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
id          GreekCol             LatinCol  
----------- -------------------- --------------------  
          1 A                    a  
  
(1 row affected)  
```  
  
#### <a name="no-collation-labels"></a>Etichette No-Collation  
Le espressioni `CASE` delle query seguenti sono associate all'etichetta No-collation. Di conseguenza non sono visualizzabili nell'elenco di selezione, né utilizzabili con gli operatori sensibili alle regole di confronto. Le espressioni possono essere tuttavia utilizzate con operatori non sensibili alle regole di confronto.  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END)   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 451, Level 16, State 1, Line 1  
Cannot resolve collation conflict for column 1 in SELECT statement.  
```  
  
```sql  
SELECT PATINDEX((CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END), 'a')  
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 446, Level 16, State 9, Server LEIH2, Line 1  
Cannot resolve collation conflict for patindex operation.  
```  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END) COLLATE Latin1_General_CI_AS   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------  
a  
  
(1 row affected)  
```  
  
## <a name="collation-sensitive-and-collation-insensitive"></a>Sensibilità alle regole di confronto  
Gli operatori e le funzioni possono essere sensibili alle regole di confronto o meno.  
  
Sensibilità alle regole di confronto  
In caso di sensibilità alle regole di confronto, se si specifica un operando di tipo No-collation viene generato un errore in fase di compilazione. L'espressione risultante non può essere di tipo No-collation.  
  
Insensibilità alle regole di confronto  
In caso di insensibilità alle regole di confronto, gli operandi e il risultato possono essere di tipo No-collation.  
  
### <a name="operators-and-collation"></a>Operatori e regole di confronto  
Gli operatori di confronto e gli operatori MAX, MIN, BETWEEN, LIKE e IN sono sensibili alle regole di confronto. Alla stringa utilizzata dagli operatori viene assegnata l'etichetta per le regole di confronto dell'operando con priorità maggiore. Anche l'istruzione UNION è sensibile alle regole di confronto. Agli operandi stringa e al risultato finale vengono assegnate le regole di confronto dell'operando con priorità maggiore. La precedenza delle regole di confronto dell'operando UNION e il risultato vengono valutati colonna per colonna.  
  
L'operatore di assegnazione non è sensibile alle regole di confronto. All'espressione di destra vengono applicate le regole di confronto dell'espressione di sinistra.  
  
L'operatore di concatenazione delle stringhe è sensibile alle regole di confronto. Ai due operandi stringa e al risultato viene assegnata l'etichetta per le regole di confronto dell'operando le cui regole di confronto hanno priorità maggiore. Le istruzioni UNION ALL e CASE non sono sensibili alle regole di confronto. A tutti gli operandi stringa e ai risultati finali viene assegnata l'etichetta per le regole di confronto dell'operando con priorità maggiore. La precedenza delle regole di confronto degli operandi UNION ALL e il risultato vengono valutati colonna per colonna.  
  
### <a name="functions-and-collation"></a>Funzioni e regole di confronto  
Le funzioni CAST, CONVERT e COLLATE sono sensibili alle regole di confronto per i tipi di dati **char**, **varchar** e **text**. Se l'input e l'output delle funzioni CAST e CONVERT sono stringhe di caratteri, alla stringa di output è associata l'etichetta per le regole di confronto della stringa di input. Se l'input non è una stringa di caratteri, la stringa di output risulta di tipo Coercible-default e vi vengono assegnate le regole di confronto del database corrente per la connessione oppure quelle del database che include la funzione definita dall'utente, la stored procedure o il trigger in cui viene fatto riferimento alla funzione CAST o CONVERT.  
  
 Per le funzioni predefinite che restituiscono una stringa ma non accettano una stringa come input, la stringa risultante risulta di tipo Coercible-default e vi vengono assegnate le regole di confronto del database corrente oppure quelle del database che include la funzione definita dall'utente, la stored procedure o il trigger in cui viene fatto riferimento alla funzione.  
  
 Le funzioni elencate di seguito sono sensibili alle regole di confronto e alle stringhe di output corrispondenti è associata l'etichetta per le regole di confronto della stringa di input:  

:::row:::
    :::column:::
        CHARINDEX
    :::column-end:::
    :::column:::
        REPLACE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DIFFERENCE
    :::column-end:::
    :::column:::
        REVERSE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ISNUMERIC
    :::column-end:::
    :::column:::
        RIGHT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEFT
    :::column-end:::
    :::column:::
        SOUNDEX
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEN
    :::column-end:::
    :::column:::
        STUFF
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LOWER
    :::column-end:::
    :::column:::
        SUBSTRING
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        PATINDEX
    :::column-end:::
    :::column:::
        UPPER
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Vedere anche  
 [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md)   
 [Conversione di tipi di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Espressioni &#40; Transact-SQL &#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
