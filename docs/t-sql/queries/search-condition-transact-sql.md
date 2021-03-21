---
description: Condizione di ricerca (Transact-SQL)
title: Condizione di ricerca (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- search
- Search Condition
- condition
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator [Transact-SQL]
- CONTAINS predicate (Transact-SQL)
- ESCAPE keyword
- operators [Transact-SQL], search condition
- search conditions [SQL Server]
- WHERE clause, search conditions
- ALL keyword
- FREETEXT predicate (Transact-SQL)
- EXISTS keyword
- search conditions [SQL Server], about search conditions
- NOT operator [Transact-SQL]
- BETWEEN operator
- SOME | ANY keyword
- predicates [full-text search]
- AND, about search conditions
- logical operators [SQL Server], about search conditions
- precedence [SQL Server], logical operators
- logical operators [SQL Server], precedence
- LIKE comparisons
ms.assetid: 09974469-c5d2-4be8-bc5a-78e404660b2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b19596b68a6ec8a4dac1eef04b14d186d60318ba
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754441"
---
# <a name="search-condition-transact-sql"></a>Condizione di ricerca (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Combinazione di uno o più predicati tramite gli operatori logici AND, OR e NOT.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<search_condition> ::=  
    MATCH(<graph_search_pattern>) | <search_condition_without_match> | <search_condition> AND <search_condition>

<search_condition_without_match> ::= 
    { [ NOT ] <predicate> | ( <search_condition_without_match> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition_without_match> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | ! > | < | < = | ! < } expression   
    | string_expression [ NOT ] LIKE string_expression   
  [ ESCAPE 'escape_character' ]   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | CONTAINS   
  ( { column | * } , '<contains_search_condition>' )   
    | FREETEXT ( { column | * } , 'freetext_string' )   
    | expression [ NOT ] IN ( subquery | expression [ ,...n ] )   
    | expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
  { ALL | SOME | ANY} ( subquery )   
    | EXISTS ( subquery )     }   
    
<graph_search_pattern> ::=
    { <node_alias> { 
                      { <-( <edge_alias> )- } 
                    | { -( <edge_alias> )-> }
                    <node_alias> 
                   } 
    }
  
<node_alias> ::=
    node_table_name | node_table_alias 

<edge_alias> ::=
    edge_table_name | edge_table_alias
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
< search_condition > ::=   
    { [ NOT ] <predicate> | ( <search_condition> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | < | < = } expression   
    | string_expression [ NOT ] LIKE string_expression   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | expression [ NOT ] IN (subquery | expression [ ,...n ] )   
    | expression [ NOT ] EXISTS (subquery)     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 \<search_condition>  
 Specifica le condizioni per le righe restituite nel set dei risultati di un'istruzione SELECT, di un'espressione di query o di una sottoquery. Per un'istruzione UPDATE specifica le righe da aggiornare. Per un'istruzione DELETE specifica le righe da eliminare. Non sussiste alcun limite per il numero di predicati che è possibile includere in una condizione di ricerca di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 \<graph_search_pattern>  
 Specifica il modello di corrispondenza del grafico. Per altre informazioni sugli argomenti di questa clausola, vedere [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md)
 
 NOT  
 Nega l'espressione booleana specificata dal predicato. Per altre informazioni, vedere [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md).  
  
 AND  
 Combina due condizioni e restituisce TRUE quando entrambe le condizioni sono TRUE. Per altre informazioni, vedere [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md).  
  
 OR  
 Combina due condizioni e restituisce TRUE quando una delle due condizioni è TRUE. Per altre informazioni, vedere [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md).  
  
 \< predicate >  
 Espressione che restituisce TRUE, FALSE o UNKNOWN.  
  
 *expression*  
 Nome di colonna, costante, funzione, variabile, sottoquery scalare o qualsiasi combinazione di costanti, funzioni e nomi di colonna collegati da uno o più operatori oppure da una sottoquery. L'espressione può inoltre includere l'espressione CASE.  
  
> [!NOTE]  
>  Le costanti e le variabili delle stringhe diverse da Unicode usano la tabella codici corrispondente alle regole di confronto predefinite del database. Le conversioni della tabella codici possono verificarsi quando si usano unicamente dati di tipo carattere non Unicode e si fa riferimento ai tipi di dati carattere non Unicode **char**, **varchar** e **text**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] converte le costanti e le variabili delle stringhe non Unicode nella tabella codici che corrisponde alle regole di confronto della colonna con riferimenti o specificati mediante COLLATE, se tale tabella codici è diversa da quella che corrisponde a regole di confronto predefinite del database. Tutti i caratteri non trovati nella nuova tabella codici verranno convertiti in un carattere simile, se è reperibile un [mapping di migliore approssimazione](https://www.unicode.org/Public/MAPPINGS/VENDORS/MICSFT/WindowsBestFit/), altrimenti verrà convertito nel carattere di sostituzione predefinito di "?".  
>  
> Quando si usano più tabelle codici, le costanti carattere possono essere precedute dalla lettera maiuscola 'N ' e possono essere usate variabili Unicode, per evitare conversioni delle tabelle codici.  
  
 =  
 Operatore utilizzato per verificare l'uguaglianza di due espressioni.  
  
 <>  
 Operatore utilizzato per verificare la disuguaglianza di due espressioni.  
  
 !=  
 Operatore utilizzato per verificare la disuguaglianza di due espressioni.  
  
 \>  
 Operatore utilizzato per verificare che un'espressione sia maggiore di un'altra.  
  
 \>=  
 Operatore utilizzato per verificare che un'espressione sia maggiore o uguale a un'altra.  
  
 !>  
 Operatore utilizzato per verificare che un'espressione non sia maggiore di un'altra.  
  
 <  
 Operatore utilizzato per verificare che un'espressione sia minore di un'altra.  
  
 <=  
 Operatore utilizzato per verificare che un'espressione sia minore o uguale a un'altra.  
  
 !<  
 Operatore utilizzato per verificare che un'espressione non sia minore di un'altra.  
  
 *string_expression*  
 Stringa di caratteri e caratteri jolly.  
  
 [ NOT ] LIKE  
 Indica che la successiva stringa di caratteri deve essere utilizzata come criterio di ricerca. Per altre informazioni, vedere [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md).  
  
 ESCAPE **'** _escape_ character_ *_'_*  
 Consente la ricerca di un carattere jolly come carattere effettivo in una stringa di caratteri, ignorando la funzionalità di carattere jolly. *escape_character* è il carattere inserito davanti al carattere jolly per specificarne l'utilizzo speciale.  
  
 [ NOT ] BETWEEN  
 Specifica un intervallo di valori inclusivo. Utilizzare l'operatore AND per separare il valore iniziale da quello finale. Per altre informazioni, vedere [BETWEEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/between-transact-sql.md).  
  
 IS [ NOT ] NULL  
 Specifica una ricerca di valori Null o non Null, a seconda delle parole chiave utilizzate. Un'espressione con un operatore bit per bit o aritmetico restituisce NULL se uno degli operandi è NULL.  
  
 CONTAINS  
 Esegue ricerche in colonne che includono dati di tipo carattere per individuare corrispondenze esatte o *fuzzy* (meno esatte) di singole parole e frasi, la prossimità delle parole separate da una distanza massima specifica e le corrispondenze ponderate. È possibile utilizzare questa opzione solo con istruzioni SELECT. Per altre informazioni, vedere [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md).  
  
 FREETEXT  
 Implementa un tipo semplice di query in linguaggio naturale per l'esecuzione di ricerche in colonne che includono dati di tipo carattere, per individuare valori che corrispondano al significato piuttosto che alle parole esatte del predicato. È possibile utilizzare questa opzione solo con istruzioni SELECT. Per altre informazioni, vedere [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md).  
  
 [ NOT ] IN  
 Specifica la ricerca di un'espressione in base alla presenza o meno di tale espressione in un elenco. L'espressione di ricerca può essere una costante o un nome di colonna. L'elenco può essere un set di costanti o, più comunemente, una sottoquery. Racchiudere l'elenco di valori tra parentesi. Per altre informazioni, vedere [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md).  
  
 *subquery*  
 Questo argomento può essere considerato un'istruzione SELECT con restrizioni ed è simile a \<query_expression> nell'istruzione SELECT. La clausola ORDER BY e la parola chiave INTO non sono consentite. Per altre informazioni, vedere [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
 ALL  
 Utilizzato con un operatore di confronto e una sottoquery. Restituisce TRUE per \<predicate> se tutti i valori recuperati per la sottoquery soddisfano l'operazione di confronto, restituisce FALSE se non tutti i valori soddisfano il criterio di confronto oppure la sottoquery non restituisce righe all'istruzione esterna. Per altre informazioni, vedere [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md).  
  
 { SOME | ANY }  
 Utilizzato con un operatore di confronto e una sottoquery. Restituisce TRUE per \<predicate> se almeno un valore recuperato per la sottoquery soddisfa l'operazione di confronto, restituisce FALSE se nessun valore della sottoquery soddisfa il criterio di confronto oppure la sottoquery non restituisce righe all'istruzione esterna. Negli altri casi, l'espressione è UNKNOWN. Per altre informazioni, vedere [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 EXISTS  
 Utilizzato con una sottoquery per verificare l'esistenza delle righe restituite dalla sottoquery. Per altre informazioni, vedere [EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 L'ordine di precedenza degli operatori logici prevede NOT come operatore con precedenza massima, seguito da AND e quindi da OR. È possibile utilizzare le parentesi per ignorare tale ordine di precedenza in una condizione di ricerca. L'ordine di valutazione degli operatori logici può variare a seconda delle scelte effettuate da Query Optimizer. Per altre informazioni sul funzionamento degli operatori logici applicati a valori logici, vedere [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md), [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md) e [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-where-with-like-and-escape-syntax"></a>R. Utilizzo della clausola WHERE con la sintassi LIKE ed ESCAPE  
 Nell'esempio seguente viene eseguita una ricerca delle righe in cui la colonna `LargePhotoFileName` contiene i caratteri `green_` e si usa l'opzione `ESCAPE` perché _ è un carattere jolly. Se non si specifica l'opzione `ESCAPE`, la query esegue una ricerca di qualsiasi valore di descrizione contenente la parola `green` seguita da qualsiasi singolo carattere diverso dal carattere _ .  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT *   
FROM Production.ProductPhoto  
WHERE LargePhotoFileName LIKE '%greena_%' ESCAPE 'a' ;  
```  
  
### <a name="b-using-where-and-like-syntax-with-unicode-data"></a>B. Utilizzo della clausola WHERE e della sintassi LIKE con dati Unicode  
 Nell'esempio seguente viene utilizzata la clausola `WHERE` per recuperare l'indirizzo postale delle società con sede al di fuori degli Stati Uniti (`US`) e in città il cui nome inizia con `Pa`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT AddressLine1, AddressLine2, City, PostalCode, CountryRegionCode    
FROM Person.Address AS a  
JOIN Person.StateProvince AS s ON a.StateProvinceID = s.StateProvinceID  
WHERE CountryRegionCode NOT IN ('US')  
AND City LIKE N'Pa%' ;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-where-with-like"></a>C. Utilizzo della clausola WHERE con LIKE  
 Nell'esempio seguente viene eseguita una ricerca delle righe in cui la colonna `LastName` contiene i caratteri `and`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE '%and%';  
```  
  
### <a name="d-using-where-and-like-syntax-with-unicode-data"></a>D. Utilizzo della clausola WHERE e della sintassi LIKE con dati Unicode  
 L'esempio seguente usa la clausola `WHERE` per eseguire una ricerca Unicode nella colonna `LastName`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE N'%and%';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Cursori &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
  

