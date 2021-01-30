---
description: SET @local_variable (Transact-SQL)
title: SET @local_variable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- SET @local_variable
- variables [SQL Server], assigning
- SET statement, @local_variable
- local variables [SQL Server]
ms.assetid: d410e06e-061b-4c25-9973-b2dc9b60bd85
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e3b9f365940dfe2b9e8544bf32391458e567e6d5
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077129"
---
# <a name="set-local_variable-transact-sql"></a>SET @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Imposta la variabile locale specificata, creata in precedenza tramite l'istruzione DECLARE @*local_variable*, sul valore specificato.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
Sintassi per SQL Server e database SQL di Azure:

```syntaxsql
SET   
{ @local_variable  
    [ . { property_name | field_name } ] = { expression | udt_name { . | :: } method_name }  
}  
|  
{ @SQLCLR_local_variable.mutator_method  
}  
|  
{ @local_variable  
    {+= | -= | *= | /= | %= | &= | ^= | |= } expression  
}  
|   
  { @cursor_variable =   
    { @cursor_variable | cursor_name   
    | { CURSOR [ FORWARD_ONLY | SCROLL ]   
        [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
        [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
        [ TYPE_WARNING ]   
    FOR select_statement   
        [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]   
      }   
    }  
}   
```  
Sintassi per Azure Synapse Analytics e Parallel Data Warehouse:  
```syntaxsql
SET @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
**@** _local_variable_  
Nome di una variabile di qualsiasi tipo tranne **cursor**, **text**, **ntext**, **image** o **table**. I nomi delle variabili devono iniziare con un simbolo di chiocciola ( **@** ) e rispettare le regole relative agli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
*property_name*  
Proprietà di un tipo definito dall'utente.  
  
*field_name*  
Campo pubblico di un tipo definito dall'utente.  
  
*udt_name*  
Nome di un tipo CLR (Common Language Runtime) definito dall'utente.  
  
`{ . | :: }`  
Specifica un metodo di un tipo CRL definito dall'utente. Per un metodo di istanza (non statico), usare un punto (**.**). Per un metodo statico, usare una coppia di due punti (**::**). Per richiamare un metodo, una proprietà o un campo di un tipo CLR definito dall'utente, è necessario disporre dell'autorizzazione EXECUTE per il tipo.  
  
_method_name_ **(** _argument_ [ **,** ... *n* ] **)**  
Metodo di un tipo definito dall'utente che accetta uno o più argomenti per modificare lo stato di un'istanza di un tipo. I metodi statici devono essere pubblici.  
  
**@** _SQLCLR_local_variable_  
Variabile il cui tipo si trova in un assembly. Per altre informazioni, vedere [Concetti relativi alla programmazione dell'integrazione con CLR &#40;Common Language Runtime&#41;](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md).  
  
*mutator_method*  
Metodo dell'assembly che può modificare lo stato dell'oggetto. A questo metodo viene applicato SQLMethodAttribute.IsMutator.  
  
`{ += | -= | *= | /= | %= | &= | ^= | |= }`  
Operatore di assegnazione composto:  
  
 +=              Aggiunta e assegnazione  
  
 -=              Sottrazione e assegnazione  
  
 *=              Sottrazione e assegnazione  
  
 /=              Divisione e assegnazione  
  
 %=              Modulo e assegnazione  
  
 &=              AND bit per bit e assegnazione  
  
 ^=              XOR bit per bit e assegnazione  
  
 |=              OR bit per bit e assegnazione  
  
*expression*  
Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida.  
  
*cursor_variable*  
Nome di una variabile di cursore. Se in precedenza la variabile di cursore di destinazione faceva riferimento a un cursore diverso, il riferimento precedente viene rimosso.  
  
*cursor_name*  
Nome di un cursore dichiarato con l'istruzione DECLARE CURSOR.  
  
CURSOR  
Specifica che l'istruzione SET contiene una dichiarazione di cursore.  
  
SCROLL  
Specifica che il cursore supporta tutte le opzioni di recupero, ovvero FIRST, LAST, NEXT, PRIOR, RELATIVE e ABSOLUTE. Non è possibile specificare SCROLL quando si è usato anche FAST_FORWARD.  
  
FORWARD_ONLY  
Specifica che il cursore supporta solo l'opzione di recupero FETCH NEXT. Il cursore viene recuperato in una sola direzione, dalla prima riga all'ultima. Quando si specifica FORWARD_ONLY senza la parola chiave STATIC, KEYSET o DYNAMIC, il cursore viene implementato come DYNAMIC. Se non si specifica FORWARD_ONLY o SCROLL, FORWARD_ONLY è l'impostazione predefinita, a meno che non si usi la parola chiave STATIC, KEYSET o DYNAMIC. Per i cursori di tipo STATIC, KEYSET e DYNAMIC, l'opzione predefinita è SCROLL.  
  
STATIC  
Definisce un cursore che crea una copia temporanea dei dati utilizzati dal cursore. Tutte le richieste indirizzate al cursore ottengono una risposta da questa tabella temporanea in tempdb. Di conseguenza, le modifiche apportate alle tabelle di base dopo l'apertura del cursore non si riflettono nei dati restituiti dalle operazioni di recupero eseguite sul cursore. Questo cursore non supporta le modifiche.  
  
KEYSET  
Specifica che all'apertura del cursore l'appartenenza e l'ordine delle righe nel cursore sono fissi. Il set di chiavi che identifica le righe in modo univoco è incluso nella tabella keyset di tempdb. Le modifiche di valori non chiave delle tabelle di base che sono state apportate dal proprietario del cursore o di cui è stato eseguito il commit da altri utenti sono visibili quando il proprietario scorre il cursore. Gli inserimenti eseguiti da altri utenti non sono visibili e non è possibile eseguire inserimenti tramite un cursore server [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Se una riga viene eliminata, il tentativo di recuperarla restituirà un valore -2 per la funzione @@FETCH_STATUS. Le operazioni di aggiornamento di valori di chiave dall'esterno del cursore sono simili a un'operazione di eliminazione della riga precedente seguita da un'operazione di inserimento della nuova riga. La riga con i nuovi valori non è visibile e i tentativi di recuperare la riga con i valori precedenti restituiscono @@FETCH_STATUS -2. I nuovi valori sono visibili se l'aggiornamento viene effettuato tramite il cursore specificando la clausola WHERE CURRENT OF.  
  
DYNAMIC  
Definisce un cursore che visualizza nel set di risultati tutte le modifiche apportate ai dati delle righe quando il proprietario scorre il cursore. I valori dei dati, l'ordine e l'appartenenza delle righe possono cambiare a ogni operazione di recupero. I cursori dinamici non supportano le opzioni di recupero assoluto e relativo.  
  
FAST_FORWARD  
Specifica un cursore FORWARD_ONLY e READ_ONLY con le ottimizzazioni abilitate. Non è possibile specificare FAST_FORWARD quando viene usato anche SCROLL.  
  
READ_ONLY  
Impedisce l'esecuzione di aggiornamenti tramite il cursore. Non è possibile fare riferimento al cursore in una clausola WHERE CURRENT OF di un'istruzione UPDATE o DELETE. Questa opzione è prioritaria rispetto alla funzionalità di aggiornamento predefinita di un cursore.  
  
SCROLL LOCKS  
Specifica che gli aggiornamenti o le eliminazioni posizionate eseguite tramite il cursore avranno esito positivo. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] blocca le righe mentre vengono lette nel cursore per garantirne la disponibilità per modifiche successive. Non è possibile specificare SCROLL_LOCKS quando viene usato anche FAST_FORWARD.  
  
OPTIMISTIC  
Specifica che le eliminazioni o gli aggiornamenti posizionati eseguiti tramite il cursore non avranno esito positivo se la riga è stata aggiornata dopo la lettura nel cursore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non blocca le righe mentre vengono lette nel cursore. Determina invece se la riga è stata modificata dopo la lettura nel cursore usando confronti tra i valori della colonna timestamp oppure un valore di checksum se la tabella non contiene una colonna timestamp. Se la riga è stata modificata, i tentativi di eseguire un aggiornamento o un'eliminazione posizionata hanno esito negativo. Non è possibile specificare OPTIMISTIC quando viene usato anche FAST_FORWARD.  
  
TYPE_WARNING  
Specifica che deve essere inviato un messaggio di avviso al client quando il cursore viene convertito in modo implicito dal tipo richiesto in un altro tipo.  
  
FOR *select_statement*  
Istruzione SELECT standard che definisce il set di risultati del cursore. Le parole chiave FOR BROWSE e INTO non sono consentite in *select_statement* per una dichiarazione di cursore.  
  
Se si usa DISTINCT, UNION, GROUP BY o HAVING o si include un'espressione di aggregazione in *select_list*, il cursore viene creato come STATIC.  
  
Se nessuna tabella sottostante include un indice univoco e un cursore ISO SCROLL o è richiesto un cursore KEYSET [!INCLUDE[tsql](../../includes/tsql-md.md)], il cursore è automaticamente STATIC.  
  
Se *select_statement* contiene una clausola ORDER BY in cui le colonne non sono identificatori di riga univoci, un cursore DYNAMIC viene convertito in un cursore KEYSET oppure, se non è possibile aprire un cursore KEYSET, in un cursore STATIC. Questo processo viene eseguito anche per i cursori definiti con la sintassi ISO ma senza la parola chiave STATIC.  
  
READ ONLY  
Impedisce l'esecuzione di aggiornamenti tramite il cursore. Non è possibile fare riferimento al cursore in una clausola WHERE CURRENT OF di un'istruzione UPDATE o DELETE. Questa opzione è prioritaria rispetto alla funzionalità di aggiornamento predefinita di un cursore. Questa parola chiave si differenzia dalla parola chiave READ_ONLY descritta in precedenza in quanto tra READ e ONLY esiste uno spazio anziché un carattere di sottolineatura.  
  
`UPDATE [OF column_name[ ,... n ] ]`  
Definisce le colonne aggiornabili nel cursore. Se viene specificato OF *column_name* [**,**...*n*], è possibile apportare modifiche solo nelle colonne elencate. Se non viene specificato alcun elenco, è possibile aggiornare tutte le colonne, a meno che il cursore non sia stato definito come READ_ONLY.  
  
## <a name="remarks"></a>Osservazioni  
Dopo la dichiarazione, le variabili vengono inizializzate sul valore NULL. Per assegnare un valore diverso da NULL a una variabile dichiarata, usare l'istruzione SET. L'istruzione SET che assegna un valore alla variabile restituisce un solo valore. Per inizializzare più variabili, utilizzare un'istruzione SET separata per ogni variabile locale.  
  
È possibile usare variabili solo nelle espressioni, non in sostituzione di parole chiave o nomi di oggetto. Per creare istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] dinamiche, utilizzare EXECUTE.  
  
Le regole di sintassi per SET **@** _cursor_variable_ non includono le parole chiave LOCAL e GLOBAL. Quando si usa la sintassi SET **@** _cursor_variable_ = CURSOR..., viene creato un cursore GLOBAL o LOCAL a seconda dell'impostazione dell'opzione di database "default to local cursor".  
  
Le variabili di cursore sono sempre locali, anche quando fanno riferimento a un cursore globale. Quando una variabile di cursore fa riferimento a un cursore globale, esistono sia un riferimento al cursore locale che un riferimento al cursore globale. Per ulteriori informazioni, vedere l'esempio C.  
  
Per altre informazioni, vedere [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md).  
  
È possibile usare l'operatore di assegnazione composta in tutti i casi in cui è presente un'assegnazione con un'espressione a destra dell'operatore, incluse le variabili e un'istruzione SET in un'istruzione UPDATE, SELECT o RECEIVE.  
  
Non usare una variabile in un'istruzione SELECT per concatenare valori, ovvero per calcolare valori aggregati. Si potrebbero verificare risultati di query imprevisti. Non necessariamente, infatti, tutte le espressioni nell'elenco SELECT, incluse le assegnazioni, vengono eseguite esattamente una volta per ogni riga di output. Per altre informazioni, vedere [questo articolo della Knowledge Base](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/287515).  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo public. Tutti gli utenti possono usare SET **@** _local_variable_.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-printing-the-value-of-a-variable-initialized-by-using-set"></a>R. Visualizzazione del valore di una variabile inizializzata tramite l'istruzione SET  
Nell'esempio seguente viene creata la variabile `@myvar`, viene immesso un valore stringa nella variabile e quindi viene visualizzato il valore della variabile `@myvar`.  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT @myvar;  
GO  
```  
  
### <a name="b-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>B. Utilizzo in un'istruzione SELECT di una variabile locale a cui viene assegnato un valore tramite l'istruzione SET  
L'esempio seguente crea una variabile locale denominata `@state` e usa la variabile locale in un'istruzione `SELECT` per trovare i nomi e i cognomi di tutti i dipendenti che risiedono nello stato dell'`Oregon`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @state CHAR(25);  
SET @state = N'Oregon';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name, City  
FROM HumanResources.vEmployee  
WHERE StateProvinceName = @state;  
```  
  
### <a name="c-using-a-compound-assignment-for-a-local-variable"></a>C. Utilizzo di un'assegnazione composta per una variabile locale  
I due esempi seguenti consentono di ottenere lo stesso risultato. Tramite gli esempi viene creata una variabile locale denominata `@NewBalance`, quindi la variabile viene moltiplicata per 10 e il nuovo valore della variabile locale viene visualizzato in un'istruzione `SELECT`. Nel secondo esempio viene utilizzato un operatore di assegnazione composto.  
  
```sql  
/* Example one */  
DECLARE  @NewBalance  INT ;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT  @NewBalance;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT @NewBalance;  
```  
  
### <a name="d-using-set-with-a-global-cursor"></a>D. Utilizzo dell'istruzione SET con un cursore globale  
Nell'esempio seguente viene creata una variabile locale e quindi viene impostata la variabile di cursore sul nome del cursore globale.  
  
```sql  
DECLARE my_cursor CURSOR GLOBAL   
FOR SELECT * FROM Purchasing.ShipMethod  
DECLARE @my_variable CURSOR ;  
SET @my_variable = my_cursor ;   
--There is a GLOBAL cursor declared(my_cursor) and a LOCAL variable  
--(@my_variable) set to the my_cursor cursor.  
DEALLOCATE my_cursor;   
--There is now only a LOCAL variable reference  
--(@my_variable) to the my_cursor cursor.  
```  
  
### <a name="e-defining-a-cursor-by-using-set"></a>E. Definizione di un cursore tramite l'istruzione SET  
Nell'esempio seguente viene utilizzata l'istruzione `SET` per definire un cursore.  
  
```sql  
DECLARE @CursorVar CURSOR;  
  
SET @CursorVar = CURSOR SCROLL DYNAMIC  
FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN @CursorVar;  
  
FETCH NEXT FROM @CursorVar;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM @CursorVar  
END;  
  
CLOSE @CursorVar;  
DEALLOCATE @CursorVar;  
```  
  
### <a name="f-assigning-a-value-from-a-query"></a>F. Assegnazione di un valore tramite una query  
Nell'esempio seguente viene utilizzata una query per assegnare un valore a una variabile.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM Sales.Customer);  
SELECT @rows;  
```  
  
### <a name="g-assigning-a-value-to-a-user-defined-type-variable-by-modifying-a-property-of-the-type"></a>G. Assegnazione di un valore a una variabile con tipo definito dall'utente tramite la modifica di una proprietà del tipo  
Nell'esempio seguente viene impostato un valore per il tipo definito dall'utente `Point` tramite la modifica del valore della proprietà `X` del tipo.  
  
```sql  
DECLARE @p Point;  
SET @p.X = @p.X + 1.1;  
SELECT @p;  
GO  
```  
  
### <a name="h-assigning-a-value-to-a-user-defined-type-variable-by-invoking-a-method-of-the-type"></a>H. Assegnazione di un valore a una variabile con tipo definito dall'utente tramite la chiamata di un metodo del tipo  
Nell'esempio seguente viene impostato un valore per il tipo definito dall'utente **point** tramite la chiamata del metodo `SetXY` del tipo.  
  
```sql  
DECLARE @p Point;  
SET @p=point.SetXY(23.5, 23.5);  
```  
  
### <a name="i-creating-a-variable-for-a-clr-type-and-calling-a-mutator-method"></a>I. Creazione di una variabile per un tipo CLR e chiamata di un metodo mutatore  
Nell'esempio seguente viene creata una variabile per il tipo `Point`, quindi viene eseguito un metodo mutatore in `Point`.  
  
```sql  
CREATE ASSEMBLY mytest FROM 'c:\test.dll' WITH PERMISSION_SET = SAFE  
CREATE TYPE Point EXTERNAL NAME mytest.Point  
GO  
DECLARE @p Point = CONVERT(Point, '')  
SET @p.SetXY(22, 23);  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-printing-the-value-of-a-variable-initialized-by-using-set"></a>J. Visualizzazione del valore di una variabile inizializzata tramite l'istruzione SET  
Nell'esempio seguente viene creata la variabile `@myvar`, viene immesso un valore stringa nella variabile e quindi viene visualizzato il valore della variabile `@myvar`.  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT TOP 1 @myvar FROM sys.databases;
```  
  
### <a name="k-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>K. Utilizzo in un'istruzione SELECT di una variabile locale a cui viene assegnato un valore tramite l'istruzione SET  
Nell'esempio seguente viene creata una variabile locale denominata `@dept` che viene quindi usata in un'istruzione `SELECT` per individuare i nomi e i cognomi di tutti i dipendenti che lavorano nel reparto `Marketing`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @dept CHAR(25);  
SET @dept = N'Marketing';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name  
FROM DimEmployee   
WHERE DepartmentName = @dept;  
```  
  
### <a name="l-using-a-compound-assignment-for-a-local-variable"></a>L. Utilizzo di un'assegnazione composta per una variabile locale  
I due esempi seguenti consentono di ottenere lo stesso risultato. In questi esempi viene creata una variabile locale denominata `@NewBalance`, quindi la variabile viene moltiplicata per `10` e il nuovo valore della variabile locale viene visualizzato in un'istruzione `SELECT`. Nel secondo esempio viene utilizzato un operatore di assegnazione composto.  
  
```sql  
/* Example one */  
DECLARE  @NewBalance INT;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
```  
  
### <a name="m-assigning-a-value-from-a-query"></a>M. Assegnazione di un valore tramite una query  
Nell'esempio seguente viene utilizzata una query per assegnare un valore a una variabile.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM dbo.DimCustomer);  
SELECT TOP 1 @rows FROM sys.tables;  
```  
  
## <a name="see-also"></a>Vedere anche  
[Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
[EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
[Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

