---
description: NEXT VALUE FOR (Transact-SQL)
title: NEXT VALUE FOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NEXT_VALUE_TSQL
- NEXT
- NEXT VALUE
- NEXT VALUE FOR
- NEXT_TSQL
- NEXT_VALUE_FOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEXT VALUE FOR function
- sequence number object, NEXT VALUE FOR function
ms.assetid: 92632ed5-9f32-48eb-be28-a5e477ef9076
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 4af385b2d2e8874fe7554fe213c355a97382d520
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189936"
---
# <a name="next-value-for-transact-sql"></a>NEXT VALUE FOR (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Genera un numero di sequenza dall'oggetto sequenza specificato.  
  
 Per una descrizione completa della creazione e dell'uso delle sequenze, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md). Usare [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) per generare un intervallo di numeri di sequenza.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
NEXT VALUE FOR [ database_name . ] [ schema_name . ]  sequence_name  
   [ OVER (<over_order_by_clause>) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *database_name*  
 Nome del database contenente l'oggetto sequenza.  
  
 *schema_name*  
 Nome dello schema contenente l'oggetto sequenza.  
  
 *sequence_name*  
 Nome dell'oggetto sequenza che genera il numero.  
  
 *over_order_by_clause*  
 Determina l'ordine in cui il valore della sequenza viene assegnato alle righe in una partizione. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce un numero utilizzando il tipo della sequenza.  
  
## <a name="remarks"></a>Osservazioni  
 La funzione **NEXT VALUE FOR** può essere usata nelle stored procedure e nei trigger.  
  
 Quando la funzione **NEXT VALUE FOR** viene usata in una query o in un vincolo predefinito, se lo stesso oggetto sequenza viene usato più di una volta o se lo stesso oggetto sequenza viene usato sia nell'istruzione che specifica i valori sia in un vincolo predefinito di cui è in corso l'esecuzione, viene restituito lo stesso valore per tutte le colonne che fanno riferimento alla stessa sequenza all'interno di una riga nel set di risultati.  
  
 La funzione **NEXT VALUE FOR** è non deterministica ed è consentita solo nei contesti in cui il numero di valori di sequenza generati è ben definito. Di seguito è riportata la definizione del numero di valori che verranno utilizzati per ogni oggetto sequenza a cui viene fatto riferimento in una determinata istruzione:  
  
-   **SELECT**: per ogni oggetto sequenza a cui viene fatto riferimento, viene generato un nuovo valore una volta per riga nel risultato dell'istruzione.  
  
-   **INSERT** ... **VALUES**: per ogni oggetto sequenza a cui viene fatto riferimento, viene generato un nuovo valore una volta per ogni riga inserita nell'istruzione.  
  
-   **UPDATE**: per ogni oggetto sequenza a cui viene fatto riferimento, viene generato un nuovo valore per ogni riga aggiornata dall'istruzione.  
  
-   Istruzioni procedurali, ad esempio **DECLARE**, **SET** e così via: per ogni oggetto sequenza a cui viene fatto riferimento, viene generato un nuovo valore per ogni istruzione.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile usare la funzione **NEXT VALUE FOR** nelle situazioni seguenti:  
  
-   Il database è in modalità sola lettura.  
  
-   Come argomento per una funzione con valori di tabella.  
  
-   Come argomento per una funzione di aggregazione.  
  
-   Nelle sottoquery che includono espressioni di tabella comuni e tabelle derivate.  
  
-   Nelle viste, nelle funzioni definite dall'utente o nelle colonne calcolate.  
  
-   In un'istruzione in cui viene usato l'operatore **DISTINCT**, **UNION**, **UNION ALL**, **EXCEPT** o **INTERSECT**.  
  
-   In un'istruzione in cui viene usata la clausola **ORDER BY**, a meno che non venga usato **NEXT VALUE FOR** ... **OVER** (**ORDER BY** ...).  
  
-   Nelle clausole seguenti: **FETCH**, **OVER**, **OUTPUT**, **ON**, **PIVOT**, **UNPIVOT**, **GROUP BY**, **HAVING**, **COMPUTE**, **COMPUTE BY** o **FOR XML**.  
  
-   In espressioni condizionali che usano **CASE**, **CHOOSE**, **COALESCE**, **IIF**, **ISNULL** o **NULLIF**.  
  
-   In una clausola **VALUES** che non fa parte di un'istruzione **INSERT**.  
  
-   Nella definizione di un vincolo CHECK.  
  
-   Nella definizione di un oggetto predefinito o di una regola. Può essere utilizzata in un vincolo predefinito.  
  
-   Come valore predefinito in un tipo di tabella definito dall'utente.  
  
-   In un'istruzione che usa **TOP**, **OFFSET**, o quando è impostata l'opzione **ROWCOUNT**.  
  
-   Nella clausola **WHERE** di un'istruzione.  
  
-   In un'istruzione **MERGE**, eccetto quando la funzione **NEXT VALUE FOR** viene usata in un vincolo predefinito nella tabella di destinazione e il valore predefinito viene usato nell'istruzione **CREATE** dell'istruzione **MERGE**.  
  
## <a name="using-a-sequence-object-in-a-default-constraint"></a>Utilizzo di un oggetto sequenza in un vincolo predefinito  
 Quando si usa la funzione **NEXT VALUE FOR** in un vincolo predefinito, si applicano le regole seguenti:  
  
-   È possibile fare riferimento a un singolo oggetto sequenza dai vincoli predefiniti in più tabelle.  
  
-   La tabella e l'oggetto sequenza devono risiedere nello stesso database.  
  
-   L'utente che aggiunge il vincolo predefinito deve disporre dell'autorizzazione REFERENCES per l'oggetto sequenza.  
  
-   Non è possibile eliminare un oggetto sequenza a cui viene fatto riferimento da un vincolo predefinito prima che venga eliminato il vincolo predefinito.  
  
-   Lo stesso numero di sequenza viene restituito per tutte le colonne di una riga se più vincoli predefiniti utilizzano lo stesso oggetto sequenza o se lo stesso oggetto sequenza viene utilizzato sia nell'istruzione che fornisce i valori, sia in un vincolo predefinito di cui è in corso l'esecuzione.  
  
-   I riferimenti alla funzione **NEXT VALUE FOR** in un vincolo predefinito non possono specificare la clausola **OVER**.  
  
-   È possibile modificare un oggetto sequenza a cui viene fatto riferimento in un vincolo predefinito.  
  
-   Nel caso di un'istruzione `INSERT ... SELECT` o `INSERT ... EXEC` in cui i dati inseriti provengono da una query in cui viene usata una clausola **ORDER BY**, i valori restituiti dalla funzione **NEXT VALUE FOR** verranno generati nell'ordine specificato dalla clausola **ORDER BY**.  
  
## <a name="using-a-sequence-object-with-an-over-order-by-clause"></a>Utilizzo di un oggetto sequenza con una clausola OVER ORDER BY  
 La funzione **NEXT VALUE FOR** supporta la generazione di valori di sequenza ordinati applicando la clausola **OVER** alla chiamata **NEXT VALUE FOR**. Tramite la clausola **OVER** si garantisce che i valori restituiti vengano generati nell'ordine specificato dalla sottoclausola **ORDER BY** della clausola **OVER**. Le regole aggiuntive seguenti sono applicabili in caso di uso della funzione **NEXT VALUE FOR** con la clausola **OVER**:  
  
-   Più chiamate alla funzione **NEXT VALUE FOR** per lo stesso generatore di sequenza in una singola istruzione devono usare tutte la stessa definizione della clausola **OVER**.  
  
-   Più chiamate alla funzione **NEXT VALUE FOR** che fanno riferimento a generatori di sequenza diversi in una singola istruzione possono presentare definizioni della clausola **OVER** diverse.  
  
-   Una clausola **OVER** applicata alla funzione **NEXT VALUE FOR** non supporta la sottoclausola **PARTITION BY**.  
  
-   Se tutte le chiamate alla funzione **NEXT VALUE FOR** in un'istruzione **SELECT** specificano la clausola **OVER**, è possibile usare la clausola **ORDER BY** nell'istruzione **SELECT**.  
  
-   La clausola **OVER** è consentita con la funzione **NEXT VALUE FOR** se usata in un'istruzione **SELECT** o in un'istruzione `INSERT ... SELECT ...`. Non è consentito usare la clausola **OVER** con la funzione **NEXT VALUE FOR** nelle istruzioni **UPDATE** o **MERGE**.  
  
-   Se un altro processo accede contemporaneamente all'oggetto sequenza, i numeri restituiti potrebbero presentare vuoti.  
  
## <a name="metadata"></a>Metadati  
 Per informazioni sulle sequenze, eseguire una query nella vista del catalogo [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **UPDATE** per l'oggetto sequenza o per lo schema della sequenza. Per un esempio relativo alla concessione di autorizzazioni, vedere l'esempio F più avanti in questo argomento.  
  
### <a name="ownership-chaining"></a>Concatenamento della proprietà  
 Gli oggetti sequenza supportano il concatenamento della proprietà. Se l'oggetto sequenza appartiene allo stesso proprietario della tabella, del trigger o della stored procedure chiamante (con oggetto sequenza come vincolo predefinito), non è necessario effettuare alcuna verifica delle autorizzazioni per l'oggetto sequenza. Se l'oggetto sequenza non appartiene allo stesso utente della tabella, del trigger o della stored procedure chiamante, è necessario effettuare la verifica delle autorizzazioni per l'oggetto sequenza.  
  
 Quando la funzione **NEXT VALUE FOR** viene usata come valore predefinito in una tabella, gli utenti devono avere sia l'autorizzazione **INSERT** per la tabella sia l'autorizzazione **UPDATE** per l'oggetto sequenza per inserire dati usando l'impostazione predefinita.  
  
-   Se il vincolo predefinito appartiene allo stesso proprietario dell'oggetto sequenza, non sono necessarie autorizzazioni per l'oggetto sequenza quando viene chiamato il vincolo predefinito.  
  
-   Se il vincolo predefinito e l'oggetto sequenza non appartengono allo stesso utente, sono necessarie autorizzazioni per l'oggetto sequenza anche se viene chiamato attraverso il vincolo predefinito.  
  
### <a name="audit"></a>Audit  
 Per controllare la funzione **NEXT VALUE FOR**, monitorare SCHEMA_OBJECT_ACCESS_GROUP.  
  
## <a name="examples"></a>Esempi  
 Per esempi relativi alla creazione di sequenze e all'uso della funzione **NEXT VALUE FOR** per generare numeri di sequenza, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 Negli esempi seguenti viene utilizzata una sequenza denominata `CountBy1` in uno schema denominato `Test`. Eseguire l'istruzione seguente per creare la sequenza `Test.CountBy1`. Gli esempi C e E utilizzano il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], pertanto la sequenza `CountBy1` viene creata in questo database.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
CREATE SCHEMA Test;  
GO  
  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="a-using-a-sequence-in-a-select-statement"></a>R. Utilizzo di una sequenza in un'istruzione SELECT  
 Nell'esempio seguente viene creata una sequenza denominata `CountBy1` che aumenta di uno ogni volta che viene utilizzata.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1 AS FirstUse;  
SELECT NEXT VALUE FOR Test.CountBy1 AS SecondUse;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstUse  
1  
  
SecondUse  
2
```  
  
### <a name="b-setting-a-variable-to-the-next-sequence-value"></a>B. Impostazione di una variabile sul valore di sequenza successivo  
 Nell'esempio seguente vengono illustrati tre modi per impostare una variabile sul valore successivo di un numero di sequenza.  
  
```sql  
DECLARE @myvar1 BIGINT = NEXT VALUE FOR Test.CountBy1  
DECLARE @myvar2 BIGINT ;  
DECLARE @myvar3 BIGINT ;  
SET @myvar2 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar3 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar1 AS myvar1, @myvar2 AS myvar2, @myvar3 AS myvar3 ;  
GO  
```  
  
### <a name="c-using-a-sequence-with-a-ranking-window-function"></a>C. Utilizzo di una sequenza con una funzione finestra di rango  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 OVER (ORDER BY LastName) AS ListNumber,  
    FirstName, LastName  
FROM Person.Contact ;  
GO  
```  
  
### <a name="d-using-the-next-value-for-function-in-the-definition-of-a-default-constraint"></a>D. Utilizzo della funzione NEXT VALUE FOR nella definizione di un vincolo predefinito  
 L'uso della funzione **NEXT VALUE FOR** nella definizione di un vincolo predefinito è supportato. Per un esempio dell'uso di **NEXT VALUE FOR** in un'istruzione **CREATE TABLE**, vedere l'esempio C in [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md). Nell'esempio seguente viene utilizzato `ALTER TABLE` per aggiungere una sequenza come impostazione predefinita a una tabella corrente.  
  
```sql
CREATE TABLE Test.MyTable  
(  
    IDColumn NVARCHAR(25) PRIMARY KEY,  
    name VARCHAR(25) NOT NULL  
) ;  
GO  
  
CREATE SEQUENCE Test.CounterSeq  
    AS INT  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
ALTER TABLE Test.MyTable  
    ADD   
        DEFAULT N'AdvWorks_' +   
        CAST(NEXT VALUE FOR Test.CounterSeq AS NVARCHAR(20))   
        FOR IDColumn;  
GO  
  
INSERT Test.MyTable (name)  
VALUES ('Larry') ;  
GO  
  
SELECT * FROM Test.MyTable;  
GO  
```  
  
### <a name="e-using-the-next-value-for-function-in-an-insert-statement"></a>E. Utilizzo della funzione NEXT VALUE FOR in un'istruzione INSERT  
 Nell'esempio seguente viene creata una tabella denominata `TestTable` e viene utilizzata la funzione `NEXT VALUE FOR` per inserire una riga.  
  
```sql  
CREATE TABLE Test.TestTable  
     (CounterColumn INT PRIMARY KEY,  
    Name NVARCHAR(25) NOT NULL) ;   
GO  
  
INSERT Test.TestTable (CounterColumn,Name)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Syed') ;  
GO  
  
SELECT * FROM Test.TestTable;   
GO  
```  
  
### <a name="e-using-the-next-value-for-function-with-select--into"></a>E. Uso della funzione NEXT VALUE FOR con SELECT ... INTO  
 Nell'esempio seguente viene usata l'istruzione `SELECT ... INTO` per creare una tabella denominata `Production.NewLocation` e la funzione `NEXT VALUE FOR` per numerare ogni riga.  
  
```sql  
USE AdventureWorks2012 ;   
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 AS LocNumber, Name   
    INTO Production.NewLocation  
    FROM Production.Location ;  
GO  
  
SELECT * FROM Production.NewLocation ;  
GO  
```  
  
### <a name="f-granting-permission-to-execute-next-value-for"></a>F. Concessione dell'autorizzazione per eseguire NEXT VALUE FOR  
 Nell'esempio seguente viene concessa l'autorizzazione **UPDATE** a un utente denominato `AdventureWorks\Larry` per eseguire `NEXT VALUE FOR` usando la sequenza `Test.CounterSeq`.  
  
```sql  
GRANT UPDATE ON OBJECT::Test.CounterSeq TO [AdventureWorks\Larry] ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
