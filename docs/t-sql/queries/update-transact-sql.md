---
description: UPDATE (Transact-SQL)
title: UPDATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATE_TSQL
- UPDATE
dev_langs:
- TSQL
helpviewer_keywords:
- DML [SQL Server], UPDATE statement
- data updates [SQL Server], UPDATE statement
- updating data [SQL Server]
- TOP clause, UPDATE
- OUTPUT clause
- large value data types
- data manipulation language [SQL Server], UPDATE statement
- user-defined types [SQL Server], modifying
- INSTEAD OF triggers
- modifying data [SQL Server], UPDATE statement
- data modifications [SQL Server], UPDATE statement
- large data, modifying
- .WRITE clause
- table modifications [SQL Server], UPDATE statement
- UDTs [SQL Server], modifying
- UPDATE statement [SQL Server], about UPDATE statement
- LOB data [SQL Server], modifying
- modifying LOB data
- UPDATE statement [SQL Server]
- FROM clause, UPDATE statement
- WHERE clause, UPDATE statement
ms.assetid: 40e63302-0c68-4593-af3e-6d190181fee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b876b8e1a54f500e800486259f20d805f2d6a987
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159185"
---
# <a name="update-transact-sql"></a>UPDATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica i dati esistenti in una tabella o vista in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Per alcuni esempi, vedere [Esempi](#UpdateExamples).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [...n] ]  
UPDATE   
    [ TOP ( expression ) [ PERCENT ] ]   
    { { table_alias | <object> | rowset_function_limited   
         [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
      }  
      | @table_variable      
    }  
    SET  
        { column_name = { expression | DEFAULT | NULL }  
          | { udt_column_name.{ { property_name = expression  
                                | field_name = expression }  
                                | method_name ( argument [ ,...n ] )  
                              }  
          }  
          | column_name { .WRITE ( expression , @Offset , @Length ) }  
          | @variable = expression  
          | @variable = column = expression  
          | column_name { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable = column { += | -= | *= | /= | %= | &= | ^= | |= } expression  
        } [ ,...n ]   
  
    [ <OUTPUT Clause> ]  
    [ FROM{ <table_source> } [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                  { { [ GLOBAL ] cursor_name }   
                      | cursor_variable_name   
                  }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
    | database_name .[ schema_name ] .   
    | schema_name .  
    ]  
    table_or_view_name}  
```  
  
```syntaxsql 
-- Syntax for Azure Synapse Analysis

[ WITH <common_table_expression> [ ,...n ] ]
UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name
SET { column_name = { expression | NULL } } [ ,...n ]  
FROM [ database_name . [ schema_name ] . | schema_name . ] table_name   
JOIN {<join_table_source>}[ ,...n ] 
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name   
SET { column_name = { expression | NULL } } [ ,...n ]  
[ FROM from_clause ]  
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 WITH \<common_table_expression>  
 Specifica una vista o un set di risultati denominato temporaneo, anche noto come espressione di tabella comune (CTE), definito nell'ambito di un'istruzione SELECT, INSERT, UPDATE o DELETE. Il set di risultati di espressione di tabella comune è derivato da una query semplice e vi viene fatto riferimento dall'istruzione UPDATE.  
  
 Le espressioni di tabella comune possono essere utilizzate anche con le istruzioni SELECT, INSERT, DELETE e CREATE VIEW. Per altre informazioni, vedere [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 Specifica il numero o la percentuale di righe che vengono aggiornate. Il valore di *expression* può essere specificato come numero o come percentuale di righe.  
  
 Le righe a cui viene fatto riferimento nell'espressione TOP utilizzata con INSERT, UPDATE o DELETE non sono disposte in alcun ordine.  
  
 Le parentesi che delimitano *expression* nell'espressione TOP sono obbligatorie nelle istruzioni INSERT, UPDATE e DELETE. Per altre informazioni, vedere [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 *table_alias*  
 Alias specificato nella clausola FROM che rappresenta la tabella o la vista da cui vengono aggiornate le righe.  
  
 *server_name*  
 Nome del server (che usa come nome un nome di server collegato o la funzione [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md)) in cui si trova la tabella o la vista. Se *server_name* è specificato, è obbligatorio specificare *database_name* e *schema_name*.  
  
 *database_name*  
 Nome del database.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la tabella o la vista.  
  
 *table_or_view_name*  
 Nome della tabella o della vista da cui devono essere aggiornate le righe. È necessario che la vista a cui viene fatto riferimento in *table_or_view_name* sia aggiornabile e includa un riferimento esatto a una tabella di base nella clausola FROM della definizione della vista. Per altre informazioni sulle viste aggiornabili, vedere [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 Funzione [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) o [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md), in base alle funzionalità del provider.  
  
 WITH **(** \<Table_Hint_Limited> **)**  
 Specifica uno o più hint di tabella consentiti per una tabella di destinazione. La parola chiave WITH e le parentesi sono obbligatorie. Le opzioni NOLOCK e READUNCOMMITTED non sono consentite. Per informazioni sugli hint di tabella, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 @*table_variable*  
 Indica una variabile[ table](../../t-sql/data-types/table-transact-sql.md) come origine di tabella.  
  
 SET  
 Indica l'elenco dei nomi di colonna o di variabile da aggiornare.  
  
 *column_name*  
 È una colonna contenente i dati da modificare. *column_name* deve essere presente in *table_or view_name*. Non è possibile aggiornare le colonne Identity.  
  
 *expression*  
 Variabile, valore letterale, espressione o istruzione sub-SELECT racchiusa tra parentesi che restituisce un valore singolo. Il valore restituito da *expression* sostituisce il valore esistente in *column_name* o @*variable*.  
  
> [!NOTE]  
>  Quando si fa riferimento ai tipi di dati dei caratteri Unicode **nchar**, **nvarchar** e **ntext**, è necessario far precedere 'expression' dalla lettera maiuscola 'N'. Se la lettera "N" non è specificata, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la stringa viene convertita in base alla tabella codici corrispondente alle regole di confronto predefinite del database o della colonna. Tutti i caratteri non trovati nella tabella codici vengono persi.  
  
 DEFAULT  
 Specifica che il valore predefinito impostato per la colonna deve sostituire il valore esistente all'interno della colonna. Questo argomento consente inoltre di modificare il valore della colonna in NULL se la colonna non dispone di un valore predefinito e ammette valori Null.  
  
 { **+=** \| **-=** \| **\*=** \| **/=** \| **%=** \| **&=** \| **^=** \| **|=** }  
 Operatore di assegnazione composto:  
 +=                       Somma e assegnazione  
 -=                        Sottrazione e assegnazione  
 *=                        Moltiplicazione e assegnazione  
 /=                         Divisione e assegnazione  
 %=                       Modulo e assegnazione  
 &=                        AND bit per bit e assegnazione  
 ^=                        XOR bit per bit e assegnazione  
 |=                         OR bit per bit e assegnazione  
  
 *udt_column_name*  
 Colonna definita dall'utente.  
  
 *property_name* | *field_name*  
 Proprietà pubblica o membro pubblico di dati di un tipo definito dall'utente.  
  
 *method_name* **(** *argument* [ **,** ... *n*] **)**  
 Metodo mutatore pubblico non statico di *udt_column_name* che accetta uno o più argomenti.  
  
 **.** WRITE **(** _expression_ **,** @_Offset_ **,** @_Length_ **)**  
 Specifica che è necessario modificare una sezione del valore di *column_name*. *expression* sostituisce unità @*Length* a partire da @*Offset* di *column_name*. È possibile specificare con questa clausola solo colonne di tipo **varchar(max)** , **nvarchar(max)** o **varbinary(max)** . *column_name* non può essere NULL e non può essere qualificato con un nome di tabella o un alias di tabella.  
  
 *expression* è il valore copiato in *column_name*. *expression* deve restituire il tipo di *column_name* oppure deve essere possibile eseguirne il cast implicito a tale tipo. Se il valore di *expression* è impostato su NULL, @*Length* viene ignorato e il valore in *column_name* viene troncato in corrispondenza del valore di @*Offset* specificato.  
  
 @*Offset* è il punto iniziale nel valore memorizzato in *column_name* in corrispondenza del quale viene scritto il valore *expression*. @*Offset* è una posizione di byte ordinale in base zero, è di tipo **bigint** e non può essere un numero negativo. Se @*Offset* è NULL, l'operazione di aggiornamento accoda *expression* al termine del valore *column_name* esistente e @*Length* viene ignorato. Se @*Offset* è maggiore della lunghezza in byte del valore *column_name*, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] restituisce un errore. Se la somma di @*Offset* e @*Length* supera la fine del valore sottostante nella colonna, l'eliminazione viene eseguita fino all'ultimo carattere del valore.  
  
 @*Length* è la lunghezza della sezione nella colonna, a partire da @*Offset*, sostituita da *expression*. @*Length* è di tipo **bigint** e non può essere un numero negativo. Se @*Length* è NULL, l'operazione di aggiornamento rimuove tutti i dati da @*Offset* fino al termine del valore *column_name*.  
  
 Per altre informazioni, vedere la sezione [Aggiornamento dei tipi di dati per valori di grandi dimensioni](#updating-lobs).  
  
 **@** *variable*  
 Variabile dichiarata impostata sul valore restituito da *expression*.  
  
 SET **@** _variable_ = *column* = *expression* imposta la variabile sullo stesso valore della colonna, a differenza di SET **@** _variable_ = _column_, _column_ = _expression_, che imposta la variabile sul valore precedente all'aggiornamento della colonna.  
  
 \<OUTPUT_Clause>  
 Restituisce dati aggiornati o espressioni basate su di essi come parte dell'operazione UPDATE. La clausola OUTPUT non è supportata in alcuna istruzione DML applicata a tabelle o viste remote. Per altre informazioni sugli argomenti e il comportamento di questa clausola, vedere [Clausola OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 FROM \<table_source>  
 Specifica che una tabella, vista o origine di tabella derivata viene utilizzata per fornire i criteri per l'operazione di aggiornamento. Per altre informazioni, vedere [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
 Se l'oggetto da aggiornare coincide con l'oggetto specificato nella clausola FROM e la clausola FROM include un solo riferimento all'oggetto, non è necessario specificare un alias di oggetto. Se l'oggetto da aggiornare è specificato più di una volta nella clausola FROM, un solo riferimento all'oggetto non deve specificare un alias della tabella. Tutti gli altri riferimenti all'oggetto nella clausola FROM devono includere un alias dell'oggetto.  
  
 Una vista in cui è incluso un trigger INSTEAD OF UPDATE non può essere la destinazione di un'istruzione UPDATE in cui è specificata la clausola FROM.  
  
> [!NOTE]  
>  Qualsiasi chiamata a OPENDATASOURCE, OPENQUERY o OPENROWSET nella clausola FROM viene valutata separatamente e indipendentemente da qualsiasi altra chiamata a queste funzioni utilizzate come destinazione dell'aggiornamento, anche se alle due chiamate vengono forniti argomenti identici. In particolare, le condizioni di filtro o join applicate al risultato di una di tali chiamate non hanno effetto sui risultati dell'altra.  
  
 WHERE  
 Vengono specificate le condizioni che consentono di limitare le righe da aggiornare. Sono disponibili due tipi di aggiornamento basati sul tipo di clausola WHERE:  
  
-   Gli aggiornamenti con ricerca specificano una condizione di ricerca che qualifica le righe da eliminare.  
  
-   Gli aggiornamenti posizionati utilizzano la clausola CURRENT OF per specificare un cursore. L'operazione di aggiornamento viene in questo caso eseguita nella posizione corrente del cursore.  
  
\<search_condition>  
 Specifica la condizione che le righe da aggiornare devono soddisfare. La condizione di ricerca può inoltre essere rappresentata dalla condizione per un join. Non sono previsti limiti per il numero di predicati che è possibile includere in una condizione di ricerca. Per altre informazioni sui predicati e sulle condizioni di ricerca, vedere [Condizioni di ricerca &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
CURRENT OF  
 Specifica che l'aggiornamento viene eseguito nella posizione corrente del cursore specificato.  
  
 Se si esegue un aggiornamento posizionato tramite la clausola WHERE CURRENT OF, viene aggiornata la riga singola che corrisponde alla posizione corrente del cursore. Questa operazione risulta più accurata rispetto a un aggiornamento con ricerca che usa una clausola WHERE \<search_condition> per qualificare le righe da aggiornare. Un aggiornamento con ricerca modifica più righe se le condizioni di ricerca non identificano una singola riga in modo univoco.  
  
GLOBAL  
 Specifica che *cursor_name* fa riferimento a un cursore globale.  
  
*cursor_name*  
 Nome del cursore aperto dal quale deve essere eseguita l'operazione di recupero. Se sono presenti un cursore globale e un cursore locale denominati *cursor_name* ed è stato specificato l'argomento GLOBAL, l'argomento fa riferimento al cursore globale. Se non è stato specificato l'argomento GLOBAL, fa riferimento al cursore locale. Il cursore deve consentire operazioni di aggiornamento.  
  
*cursor_variable_name*  
 Nome di una variabile di cursore. *cursor_variable_name* deve fare riferimento a un cursore che consente operazioni di aggiornamento.  
  
OPTION **(** \<query_hint> [ **,** ... *n* ] **)**  
 Specifica che vengono utilizzati hint di ottimizzazione per personalizzare la modalità di elaborazione dell'istruzione nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="best-practices"></a>Procedure consigliate  
 Usare la funzione `@@ROWCOUNT` per restituire il numero di righe inserite nell'applicazione client. Per altre informazioni, vedere [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
 Nelle istruzioni UPDATE è possibile utilizzare nomi di variabili per indicare il valore da aggiornare e il valore in base a cui eseguire l'aggiornamento. I nomi di variabili, tuttavia, devono essere utilizzati solo quando l'istruzione UPDATE è relativa a un unico record. In caso contrario, usare la [clausola OUTPUT](../../t-sql/queries/output-clause-transact-sql.md).  
  
 Prestare attenzione nello specificare la clausola FROM per fornire i criteri per l'operazione di aggiornamento. I risultati di un'istruzione UPDATE sono indefiniti se l'istruzione include una clausola FROM non specificata in modo che sia disponibile un unico valore per ogni occorrenza di colonna che viene aggiornata, ovvero se l'istruzione UPDATE non è deterministica. Ad esempio, nell'istruzione UPDATE dello script riportato di seguito entrambe le righe in `Table1` soddisfano le condizioni della clausola FROM nell'istruzione UPDATE, ma non viene specificato quale riga di `Table1` viene utilizzata per aggiornare la riga in `Table2.`  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT PRIMARY KEY NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;  
```  
  
 Lo stesso problema può verificarsi quando si combinano le clausole `FROM` e `WHERE CURRENT OF`. Nell'esempio seguente, entrambe le righe nella tabella `Table2` soddisfano le condizioni della clausola `FROM` nell'istruzione `UPDATE`. La riga di `Table2` da utilizzare per l'aggiornamento della riga in `Table1` non viene specificata.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1  
    (c1 INT PRIMARY KEY NOT NULL, c2 INT NOT NULL);  
GO  
CREATE TABLE dbo.Table2  
    (d1 INT PRIMARY KEY NOT NULL, d2 INT NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES (1, 10);  
INSERT INTO dbo.Table2 VALUES (1, 20), (2, 30);  
GO  
DECLARE abc CURSOR LOCAL FOR  
    SELECT c1, c2   
    FROM dbo.Table1;  
OPEN abc;  
FETCH abc;  
UPDATE dbo.Table1   
SET c2 = c2 + d2   
FROM dbo.Table2   
WHERE CURRENT OF abc;  
GO  
SELECT c1, c2 FROM dbo.Table1;  
GO  
```  
  
## <a name="compatibility-support"></a>Informazioni sulla compatibilità  
 In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà rimosso il supporto per l'utilizzo degli hint READUNCOMMITTED e NOLOCK nella clausola FROM per la tabella di destinazione di un'istruzione UPDATE o DELETE. Evitare di usare questi hint in questo contesto nei nuovi progetti di sviluppo e pianificare la modifica delle applicazioni in cui sono attualmente usati.  
  
## <a name="data-types"></a>Tipi di dati  
 Tutte le colonne di tipo **char** e **nchar** vengono riempite con caratteri nulli a destra fino a raggiungere la lunghezza definita.  
  
 Se l'opzione ANSI_PADDING è impostata su OFF, tutti gli spazi finali vengono rimossi dai dati inseriti nelle colonne **varchar** e **nvarchar**, tranne nel caso di stringhe che includono solo spazi, le quali vengono troncate come stringhe vuote. Se l'opzione ANSI_PADDING è impostata su ON, vengono inseriti spazi finali. Il driver ODBC di Microsoft SQL Server e il provider OLE DB per SQL Server impostano automaticamente l'opzione ANSI_PADDING su ON per ogni connessione. Questa opzione può essere configurata in origini dati ODBC oppure impostando gli attributi o le proprietà della connessione. Per altre informazioni, vedere [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
### <a name="updating-text-ntext-and-image-columns"></a>Aggiornamento di colonne text, ntext e image  
 Se si modifica una colonna **text**, **ntext** o **image** tramite l'istruzione UPDATE, la colonna viene inizializzata, viene associata a un puntatore di testo valido e viene allocata almeno una pagina di dati, a meno che la colonna non venga aggiornata con NULL.  
  
 Per sostituire o modificare grandi blocchi di dati di tipo **text**, **ntext** o **image**, usare le istruzioni [WRITETEXT](../../t-sql/queries/writetext-transact-sql.md) o [UPDATETEXT](../../t-sql/queries/updatetext-transact-sql.md) invece dell'istruzione UPDATE.  
  
 Se l'istruzione UPDATE modifica più righe durante l'aggiornamento della chiave di clustering e di una o più colonne di tipo **text**, **ntext** o **image**, l'aggiornamento parziale di queste colonne viene eseguito come sostituzione completa dei valori.  
  
> [!IMPORTANT]
>  I tipi di dati **ntext**, **text** e **image** verranno rimossi in una versione futura di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di utilizzare questi tipi di dati in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente li utilizzano. Usare in alternativa [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)e [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) .  
  
### <a name="updating-large-value-data-types"></a><a name="updating-lobs"></a> Aggiornamento dei tipi di dati per valori di grandi dimensioni  
 Usare la clausola **.** WRITE **(** _expression_ **,** @_Offset_ **,** @_Length_ **)** per eseguire un aggiornamento parziale o completo dei tipi di dati **varchar(max)** , **nvarchar(max)** e **varbinary(max)** . 
 
 Un aggiornamento parziale di una colonna di tipo **varchar(max)** , ad esempio, può eliminare o modificare solo i primi 200 byte della colonna (200 caratteri se si usano caratteri ASCII), mentre un aggiornamento completo elimina o modifica tutti i dati nella colonna. Gli aggiornamenti **.WRITE** che inseriscono o accodano nuovi dati vengono registrati in maniera minima se il modello di recupero del database è impostato su registrazione minima delle operazioni bulk oppure su registrazione minima. La registrazione minima non viene utilizzata in caso di aggiornamento di valori esistenti. Per altre informazioni, vedere [Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 In [!INCLUDE[ssDE](../../includes/ssde-md.md)] un aggiornamento parziale viene convertito in aggiornamento completo quando l'istruzione UPDATE provoca una di queste azioni:  
-   modifica una colonna chiave della vista o tabella partizionata  
-   modifica più di una riga e allo stesso tempo aggiorna la chiave di un indice cluster non univoco ad un valore non costante.  
  
Non è possibile usare la clausola **.WRITE** per aggiornare una colonna NULL o impostare il valore di *column_name* su NULL.  
  
I valori di @*Offset* e @*Length* vengono specificati in byte per i tipi di dati **varbinary** e **varchar** e in coppie di byte per il tipo di dati **nvarchar**. Per altre informazioni sulle lunghezze dei tipi di dati stringa, vedere [char e varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md) e [nchar e nvarchar (Transact-SQL)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md).
  
Per prestazioni ottimali, è consigliabile inserire o aggiornare i dati in dimensioni di blocco multiple di 8040 byte.  
  
Se in una clausola OUTPUT si fa riferimento alla colonna modificata dalla clausola **\.WRITE**, il valore completo della colonna, ovvero l'immagine precedente all'aggiornamento in **deleted.** _column\_name_ oppure l'immagine successiva all'aggiornamento in **inserted.** _column\_name_ viene restituito nella colonna specificata nella variabile di tabella. Vedere l'esempio R seguente.  
  
Per ottenere la stessa funzionalità di **\.WRITE** con altri tipi di dati character o binary, usare [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md).  
  
### <a name="updating-user-defined-type-columns"></a>Aggiornamento delle colonne di tipo definito dall'utente  
 L'aggiornamento dei valori nelle colonne di tipo definito dall'utente può essere eseguito in uno dei modi seguenti:  
  
-   Specificando un valore in un tipo di dati di sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a condizione che i tipi definiti dall'utente supportino la conversione implicita o esplicita da quel tipo. Nell'esempio seguente viene illustrato come aggiornare un valore in una colonna del tipo `Point` definito dall'utente, eseguendo la conversione esplicita da una stringa.  
  
    ```sql  
    UPDATE Cities  
    SET Location = CONVERT(Point, '12.3:46.2')  
    WHERE Name = 'Anchorage';  
    ```  
  
-   Richiamando un metodo, contrassegnato come mutatore, del tipo definito dall'utente, per eseguire l'aggiornamento. Nell'esempio seguente viene richiamato un metodo mutatore di tipo `Point` denominato `SetXY`. Viene aggiornato lo stato dell'istanza del tipo.  
  
    ```sql  
    UPDATE Cities  
    SET Location.SetXY(23.5, 23.5)  
    WHERE Name = 'Anchorage';  
    ```  
  
    > [!NOTE]  
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore se viene richiamato un metodo mutatore in un valore Null [!INCLUDE[tsql](../../includes/tsql-md.md)] oppure se un nuovo valore prodotto da un metodo mutatore è Null.  
  
-   Modificando il valore di una proprietà registrata o di un membro pubblico di dati del tipo definito dall'utente. È necessario che l'espressione che fornisce il valore possa essere convertita in modo implicito nel tipo della proprietà. Nell'esempio seguente viene modificato il valore di proprietà `X` del tipo definito dall'utente `Point`.  
  
    ```sql  
    UPDATE Cities  
    SET Location.X = 23.5  
    WHERE Name = 'Anchorage';  
    ```  
  
     Per modificare proprietà diverse della stessa colonna di tipo definito dall'utente, eseguire più istruzioni UPDATE o richiamare un metodo mutatore del tipo.  
  
### <a name="updating-filestream-data"></a>Aggiornamento di dati FILESTREAM  
 È possibile utilizzare l'istruzione UPDATE per aggiornare un campo FILESTREAM a un valore Null, a un valore vuoto o a una quantità di dati inline relativamente piccola. Tuttavia, una grande quantità di dati viene trasmessa in modo più efficace in un file mediante l'utilizzo di interfacce Win32. Quando si aggiorna un campo FILESTREAM, si modificano i dati BLOB sottostanti nel file system. Quando un campo FILESTREAM viene impostato su NULL, i dati BLOB associati al campo vengono eliminati. Non è possibile usare .WRITE() per eseguire aggiornamenti parziali a dati FILESTREAM. Per altre informazioni, vedere [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
## <a name="error-handling"></a>Gestione degli errori  
 Se un'operazione di aggiornamento in una riga non rispetta un vincolo o una regola, viola l'impostazione NULL per la colonna oppure il nuovo valore è un tipo di dati incompatibile, l'istruzione viene annullata, viene restituito un errore e non viene aggiornato alcun record.  
  
 Quando un'istruzione UPDATE rileva un errore aritmetico (un errore di overflow, una divisione per zero o un errore di dominio) durante la valutazione di un'espressione, l'aggiornamento non viene eseguito. La parte rimanente del batch non viene eseguita e viene visualizzato un messaggio di errore.  
  
 Se dopo un aggiornamento a una o più colonne che fanno parte di un indice cluster le dimensioni dell'indice cluster e della riga superano gli 8.060 byte, l'aggiornamento non viene eseguito correttamente e viene restituito un messaggio di errore.  
  
## <a name="interoperability"></a>Interoperabilità  
 Le istruzioni UPDATE sono consentite all'interno delle funzioni definite dall'utente solo se la tabella da modificare è una variabile di tabella.  
  
 Quando viene definito un trigger `INSTEAD OF` in azioni UPDATE eseguite su una tabella, viene eseguito il trigger anziché l'istruzione UPDATE. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportati solo i trigger AFTER definiti in UPDATE e altre istruzioni di modifica dei dati. La clausola FROM non può essere specificata in un'istruzione UPDATE in cui si fa riferimento diretto o indiretto a una vista in cui è definito un trigger `INSTEAD OF`. Per altre informazioni sui trigger INSTEAD OF, vedere [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 La clausola FROM non può essere specificata in un'istruzione UPDATE in cui si fa riferimento diretto o indiretto a una vista in cui è definito un trigger `INSTEAD OF`. Per altre informazioni sui trigger `INSTEAD OF`, vedere [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 Quando un'espressione di tabella comune (CTE) è la destinazione di un'istruzione UPDATE, tutti i riferimenti a tale espressione nell'istruzione devono corrispondere. Se, ad esempio, alla CTE è assegnato un alias nella clausola FROM, l'alias deve essere utilizzato per tutti gli altri riferimenti alla CTE. Sono necessari riferimenti CTE non ambigui perché una CTE non dispone di un ID oggetto utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per riconoscere la relazione implicita tra l'oggetto e il relativo alias. Senza questa relazione è possibile che il piano di query produca un comportamento del join e risultati della query imprevisti. Negli esempi seguenti vengono illustrati i metodi corretti ed errati della definizione di una CTE quando questa è l'oggetto di destinazione dell'operazione di aggiornamento.  
  
```sql  
USE tempdb;  
GO  
-- UPDATE statement with CTE references that are correctly matched.  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE x -- cte is referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;  
SELECT * FROM @x;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ID     Value  
 ------ -----  
 1      100  
 2      200  
 (2 row(s) affected)  
```  

Istruzione UPDATE con riferimenti CTE associati in modo non corretto.  

```sql  
USE tempdb;  
GO  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE cte   -- cte is not referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;   
SELECT * FROM @x;   
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ID     Value  
------ -----  
1      100  
2      100  
(2 row(s) affected)  
```  

## <a name="locking-behavior"></a>Comportamento di blocco  
 Un'istruzione UPDATE acquisisce un blocco esclusivo (X) su qualsiasi riga modificata e mantiene questi blocchi fino al completamento della transazione. A seconda del piano di query per l'istruzione UPDATE, del numero di righe modificate e del livello di isolamento della transazione, è possibile che i blocchi vengano acquisiti a livello PAGE o TABLE anziché a livello ROW. Per evitare questi blocchi di livello superiore, provare a dividere le istruzioni di aggiornamento che interessano migliaia di righe o più in batch e assicurarsi che le condizioni di join e filtro siano supportate dagli indici. Per informazioni dettagliate sui meccanismi di blocco in SQL Server, vedere l'articolo [Utilizzo dei blocchi in Motore di database](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine).  
  
## <a name="logging-behavior"></a>Comportamento di registrazione  
 L'istruzione UPDATE viene registrata. Viene tuttavia eseguita una registrazione minima per gli aggiornamenti parziali a tipi di dati per valori di grandi dimensioni tramite la clausola **\.WRITE**. Per altre informazioni, vedere "Aggiornamento dei tipi di dati per valori di grandi dimensioni" nella sezione precedente "Tipi di dati".  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni `UPDATE` sono necessarie nella tabella di destinazione. Sono anche necessarie le autorizzazioni `SELECT` per la tabella da aggiornare se l'istruzione UPDATE include una clausola WHERE o se l'argomento *expression* nella clausola SET usa una colonna della tabella.  
  
 Le autorizzazioni UPDATE vengono concesse per impostazione predefinita ai membri del ruolo predefinito del server `sysadmin`, ai membri dei ruoli predefiniti del database `db_owner` e `db_datawriter` e al proprietario della tabella. I membri dei ruoli `sysadmin`, `db_owner` e `db_securityadmin` e il proprietario della tabella possono trasferire le autorizzazioni ad altri utenti.  
  
##  <a name="examples"></a><a name="UpdateExamples"></a> Esempi  
  
|Category|Elementi di sintassi inclusi|  
|--------------|------------------------------|  
|[Sintassi di base](#BasicSyntax)|UPDATE|  
|[Limitazione delle righe aggiornate](#LimitingValues)|WHERE • TOP • espressione di tabella comune WITH • WHERE CURRENT OF|  
|[Impostazione dei valori di colonna](#ColumnValues)|valori calcolati • operatori composti • valori predefiniti • sottoquery|  
|[Indicazione di oggetti di destinazione diversi dalle tabelle standard](#TargetObjects)|viste • variabili di tabella • alias di tabella|  
|[Aggiornamento di dati in base ai dati di altre tabelle](#OtherTables)|FROM|  
|[Aggiornamento di righe in una tabella remota](#RemoteTables)|server collegato • OPENQUERY • OPENDATASOURCE|  
|[Aggiornamento di tipi di dati Large Object](#LOBValues)|.WRITE • OPENROWSET|  
|[Aggiornamento di tipi definiti dall'utente](#UDTs)|tipi definiti dall'utente|  
|[Override del comportamento predefinito di Query Optimizer tramite hint](#TableHints)|hint di tabella • hint per la query|  
|[Acquisizione dei risultati dell'istruzione UPDATE](#CaptureResults)|Clausola OUTPUT|  
|[Uso di UPDATE in altre istruzioni](#Other)|Stored procedure • TRY...CATCH|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintassi di base  
 Negli esempi contenuti in questa sezione vengono illustrate le funzionalità di base dell'istruzione UPDATE tramite la sintassi minima richiesta.  
  
#### <a name="a-using-a-simple-update-statement"></a>R. Esecuzione di un'istruzione UPDATE semplice  
 Nell'esempio seguente viene aggiornata una singola colonna per tutte le righe della tabella `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.Address  
SET ModifiedDate = GETDATE();  
```  
  
#### <a name="b-updating-multiple-columns"></a>B. Aggiornamento di più colonne  
 Nell'esempio seguente vengono aggiornati i valori nelle colonne `Bonus`, `CommissionPct` e `SalesQuota` per tutte le righe nella tabella `SalesPerson`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET Bonus = 6000, CommissionPct = .10, SalesQuota = NULL;  
GO  
```  
  
###  <a name="limiting-the-rows-that-are-updated"></a><a name="LimitingValues"></a> Limitazione delle righe aggiornate  
 Negli esempi contenuti in questa sezione vengono illustrati i modi che è possibile utilizzare per limitare il numero di righe interessate dall'istruzione UPDATE.  
  
#### <a name="c-using-the-where-clause"></a>C. Utilizzo della clausola WHERE  
 Nell'esempio seguente viene utilizzata la clausola WHERE per specificare le righe da aggiornare. L'istruzione aggiorna il valore nella colonna `Color` della tabella `Production.Product` per tutte le righe che contengono un valore "Red" esistente nella colonna `Color` e un valore nella colonna `Name` che inizia con "Road-250".  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
SET Color = N'Metallic Red'  
WHERE Name LIKE N'Road-250%' AND Color = N'Red';  
GO  
```  
  
#### <a name="d-using-the-top-clause"></a>D. Utilizzo della clausola TOP  
 Negli esempi seguenti viene utilizzata la clausola TOP per limitare il numero di righe modificate in un'istruzione UPDATE. Quando si usa una clausola TOP (*n*) con UPDATE, l'operazione di aggiornamento viene eseguita su una selezione casuale di un numero '*n*' di righe. Nell'esempio seguente viene aggiornata la colonna `VacationHours` del 25% per 10 righe casuali nella tabella `Employee`.  
  
```sql  
USE AdventureWorks2012;
GO
UPDATE TOP (10) HumanResources.Employee
SET VacationHours = VacationHours * 1.25 ;
GO  
```  
  
 Se è necessario utilizzare TOP per applicare gli aggiornamenti in un ordine cronologico significativo, è necessario utilizzare TOP insieme a ORDER BY in un'istruzione sub-SELECT. Nell'esempio seguente le ore di ferie dei 10 dipendenti vengono aggiornate con le prime date di assunzione.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
#### <a name="e-using-the-with-common_table_expression-clause"></a>E. Utilizzo della clausola WITH common_table_expression  
 Nell'esempio seguente viene aggiornato il valore `PerAssemblyQty` per tutte le parti e tutti i componenti utilizzati direttamente o indirettamente per creare `ProductAssemblyID 800`. L'espressione di tabella comune restituisce un elenco gerarchico di parti utilizzate direttamente per compilare `ProductAssemblyID 800` e di parti utilizzate per compilare tali componenti e così via. Vengono modificate solo le righe restituite dall'espressione di tabella comune.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
#### <a name="f-using-the-where-current-of-clause"></a>F. Utilizzo della clausola WHERE CURRENT OF  
 Nell'esempio seguente viene utilizzata la clausola WHERE CURRENT OF per aggiornare solo la riga in cui è posizionato il cursore. Se un cursore è basato su un join, viene modificato solo il valore `table_name` specificato nell'istruzione UPDATE. Le altre tabelle interessate dal cursore rimangono invariate.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
UPDATE HumanResources.EmployeePayHistory  
SET PayFrequency = 2   
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
###  <a name="setting-column-values"></a><a name="ColumnValues"></a> Impostazione dei valori di colonna  
 Negli esempi contenuti in questa sezione viene illustrato l'aggiornamento di colonne tramite valori calcolati, sottoquery e valori DEFAULT.  
  
#### <a name="g-specifying-a-computed-value"></a>G. Specifica di un valore calcolato  
 Negli esempi seguenti vengono utilizzati valori calcolati in un'istruzione UPDATE. Nell'esempio viene raddoppiato il valore della colonna `ListPrice` per tutte le righe della tabella `Product`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
UPDATE Production.Product  
SET ListPrice = ListPrice * 2;  
GO  
```  
  
#### <a name="h-specifying-a-compound-operator"></a>H. Specifica di un operatore composto  
 Nell'esempio seguente viene utilizzata la variabile `@NewPrice` per incrementare il prezzo di tutte le biciclette rosse aggiungendo 10 al prezzo corrente.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NewPrice INT = 10;  
UPDATE Production.Product  
SET ListPrice += @NewPrice  
WHERE Color = N'Red';  
GO  
```  
  
 Nell'esempio seguente viene utilizzato l'operatore composto + = per aggiungere i dati `' - tool malfunction'` al valore esistente nella colonna `Name` per le righe con `ScrapReasonID` tra 10 e 12.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ScrapReason   
SET Name += ' - tool malfunction'  
WHERE ScrapReasonID BETWEEN 10 and 12;  
```  
  
#### <a name="i-specifying-a-subquery-in-the-set-clause"></a>I. Specifica di una sottoquery nella clausola SET  
 Nell'esempio seguente viene utilizzata una sottoquery nella clausola SET per determinare il valore utilizzato per aggiornare la colonna. La sottoquery deve restituire solo un valore scalare, ovvero un solo valore per riga. Nell'esempio la colonna `SalesYTD` della tabella `SalesPerson` viene modificata in modo che includa le vendite più recenti registrate nella tabella `SalesOrderHeader`. Con la sottoquery vengono aggregate le vendite per ogni venditore nell'istruzione `UPDATE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
#### <a name="j-updating-rows-using-default-values"></a>J. Aggiornamento di righe tramite valori DEFAULT  
 Nell'esempio seguente viene impostata la colonna `CostRate` sul valore predefinito (0.00) per tutte le righe con un valore `CostRate` maggiore di `20.00`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Location  
SET CostRate = DEFAULT  
WHERE CostRate > 20.00;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> Indicazione di oggetti di destinazione diversi dalle tabelle standard  
 Negli esempi contenuti in questa sezione viene illustrato come aggiornare le righe specificando una vista, un alias di tabella o una variabile di tabella.  
  
#### <a name="k-specifying-a-view-as-the-target-object"></a>K. Specifica di una vista come oggetto di destinazione  
 Nell'esempio seguente vengono aggiornate le righe di una tabella specificando una vista come oggetto di destinazione. La definizione di vista fa riferimento a più tabelle, tuttavia l'istruzione UPDATE ha esito positivo perché fa riferimento alle colonne di una sola delle tabelle sottostanti. L'istruzione UPDATE avrebbe esito negativo se venissero specificate colonne di entrambe le tabelle. Per altre informazioni, vedere [Modificare i dati tramite una vista](../../relational-databases/views/modify-data-through-a-view.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.vStateProvinceCountryRegion  
SET CountryRegionName = 'United States of America'  
WHERE CountryRegionName = 'United States';  
```  
  
#### <a name="l-specifying-a-table-alias-as-the-target-object"></a>L. Specifica di un alias di tabella come oggetto di destinazione  
 Nell'esempio seguente vengono aggiornate le righe della tabella `Production.ScrapReason`. L'alias di tabella assegnato a `ScrapReason` nella clausola FROM viene specificato come oggetto di destinazione nella clausola UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE sr  
SET sr.Name += ' - tool malfunction'  
FROM Production.ScrapReason AS sr  
JOIN Production.WorkOrder AS wo   
     ON sr.ScrapReasonID = wo.ScrapReasonID  
     AND wo.ScrappedQty > 300;  
```  
  
#### <a name="m-specifying-a-table-variable-as-the-target-object"></a>M. Specifica di una variabile di tabella come oggetto di destinazione  
 Nell'esempio seguente vengono aggiornate le righe in una variabile di tabella.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create the table variable.  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
-- Populate the table variable with employee ID values from HumanResources.Employee.  
INSERT INTO @MyTableVar (EmpID)  
    SELECT BusinessEntityID FROM HumanResources.Employee;  
  
-- Update columns in the table variable.  
UPDATE @MyTableVar  
SET NewVacationHours = e.VacationHours + 20,  
    ModifiedDate = GETDATE()  
FROM HumanResources.Employee AS e   
WHERE e.BusinessEntityID = EmpID;  
  
-- Display the results of the UPDATE statement.  
SELECT EmpID, NewVacationHours, ModifiedDate FROM @MyTableVar  
ORDER BY EmpID;  
GO  
```  
  
###  <a name="updating-data-based-on-data-from-other-tables"></a><a name="OtherTables"></a> Aggiornamento di dati in base ai dati di altre tabelle  
 Negli esempi contenuti in questa sezione vengono illustrati i metodi per l'aggiornamento delle righe di una tabella in base alle informazioni contenute in un'altra.  
  
#### <a name="n-using-the-update-statement-with-information-from-another-table"></a>N. Utilizzo dell'istruzione UPDATE con informazioni di un'altra tabella  
 Nell'esempio seguente la colonna `SalesYTD` della tabella `SalesPerson` viene modificata in modo che includa le vendite più recenti registrate nella tabella `SalesOrderHeader`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD + SubTotal  
FROM Sales.SalesPerson AS sp  
JOIN Sales.SalesOrderHeader AS so  
    ON sp.BusinessEntityID = so.SalesPersonID  
    AND so.OrderDate = (SELECT MAX(OrderDate)  
                        FROM Sales.SalesOrderHeader  
                        WHERE SalesPersonID = sp.BusinessEntityID);  
GO  
```  
  
 Nell'esempio precedente si presume che venga registrata una sola vendita per un determinato venditore in una data specifica e che i dati siano aggiornati. Se è possibile registrare più vendite per un determinato venditore nello stesso giorno, l'esempio non funziona correttamente. Viene eseguito senza errori, ma ogni valore `SalesYTD` viene aggiornato con una sola vendita, indipendentemente dal numero effettivo di vendite relative al giorno specificato. Un'istruzione UPDATE infatti non aggiorna mai la stessa riga due volte.  
  
 Nel caso in cui sia necessario registrare più vendite per un determinato venditore nello stesso giorno, tutte le vendite relative allo stesso venditore devono essere aggregate all'interno dell'istruzione `UPDATE`, come illustrato nell'esempio seguente:  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
###  <a name="updating-rows-in-a-remote-table"></a><a name="RemoteTables"></a> Aggiornamento di righe in una tabella remota  
 Gli esempi di questa sezione illustrano come aggiornare le righe in una tabella di destinazione remota tramite un [server collegato](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o una [funzione per i set di righe](../functions/opendatasource-transact-sql.md) per fare riferimento alla tabella remota.  
  
#### <a name="o-updating-data-in-a-remote-table-by-using-a-linked-server"></a>O. Aggiornamento di dati in una tabella remota tramite un server collegato  
 Nell'esempio seguente viene aggiornata una tabella in un server remoto. L'esempio inizia con la creazione di un collegamento all'origine dati remota tramite [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Il nome del server collegato, `MyLinkedServer`, viene quindi specificato come parte del nome dell'oggetto in quattro parti nel formato server.catalogo.schema.oggetto. Si noti che è necessario specificare un nome server valido per `@datasrc`.  
  
```sql  
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkedServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI10',   
    @datasrc = N'<server name>',  
    @catalog = N'AdventureWorks2012';  
GO  
USE AdventureWorks2012;  
GO  
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
UPDATE MyLinkedServer.AdventureWorks2012.HumanResources.Department  
SET GroupName = N'Public Relations'  
WHERE DepartmentID = 4;  
```  
  
#### <a name="p-updating-data-in-a-remote-table-by-using-the-openquery-function"></a>P. Aggiornamento di dati in una tabella remota tramite una funzione OPENQUERY  
 L'esempio seguente aggiorna una riga in una tabella remota specificando la funzione per i set di righe [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). Viene utilizzato il nome del server collegato creato nell'esempio precedente.  
  
```sql  
UPDATE OPENQUERY (MyLinkedServer, 'SELECT GroupName FROM HumanResources.Department WHERE DepartmentID = 4')   
SET GroupName = 'Sales and Marketing';  
```  
  
#### <a name="q-updating-data-in-a-remote-table-by-using-the-opendatasource-function"></a>Q. Aggiornamento di dati in una tabella remota tramite una funzione OPENDATASOURCE  
 L'esempio seguente aggiorna una riga in una tabella remota specificando la funzione per i set di righe [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Specificare un nome server valido per l'origine dati usando il formato *nome_server* oppure *nome_server\nome_istanza*. Potrebbe essere necessario configurare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per Ad Hoc Distributed Queries. Per altre informazioni, vedere [Opzione di configurazione del server ad hoc distributed queries](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md).  

```sql
UPDATE OPENDATASOURCE('SQLNCLI', 'Data Source=<server name>;Integrated Security=SSPI').AdventureWorks2012.HumanResources.Department
SET GroupName = 'Sales and Marketing' WHERE DepartmentID = 4;  
```

###  <a name="updating-large-object-data-types"></a><a name="LOBValues"></a> Aggiornamento di tipi di dati Large Object  
 Negli esempi contenuti in questa sezione vengono illustrati i metodi per l'aggiornamento di valori in colonne definite con tipi di dati LOB (Large Object).  
  
#### <a name="r-using-update-with-write-to-modify-data-in-an-nvarcharmax-column"></a>R. Utilizzo di UPDATE con la clausola .WRITE per modificare dati in una colonna nvarchar(max)  
 L'esempio seguente usata la clausola .WRITE per aggiornare un valore parziale nella colonna `DocumentSummary` di tipo **nvarchar(max)** della tabella `Production.Document`. La parola `components` viene sostituita con la parola `features` specificando la parola sostitutiva, il percorso iniziale (offset) della parola da sostituire nei dati esistenti e il numero di caratteri da sostituire (lunghezza). L'esempio usa anche la clausola OUTPUT per restituire le immagini precedente e successiva della colonna `DocumentSummary`nella variabile di tabella `@MyTableVar`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO  
```  
  
#### <a name="s-using-update-with-write-to-add-and-remove-data-in-an-nvarcharmax-column"></a>S. Utilizzo di UPDATE con la clausola .WRITE per aggiungere e rimuovere dati in una colonna di tipo nvarchar(max)  
 Gli esempi seguenti aggiungono e rimuovono dati da una colonna di tipo **nvarchar(max)** con valore impostato su NULL. Poiché la clausola .WRITE non può essere usata per modificare una colonna NULL, la colonna viene prima popolata con dati temporanei. Questi dati vengono quindi sostituiti con i dati corretti tramite la clausola .WRITE. Negli esempi aggiuntivi vengono accodati dati al termine del valore della colonna, rimossi (troncati) dati dalla colonna e infine rimossi dati parziali dalla colonna. Le istruzioni SELECT consentono di visualizzare la modifica dei dati generata da ogni istruzione UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Replacing NULL value with temporary data.  
UPDATE Production.Document  
SET DocumentSummary = N'Replacing NULL value'  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Replacing temporary data with the correct data. Setting @Length to NULL   
-- truncates all existing data from the @Offset position.  
UPDATE Production.Document  
SET DocumentSummary .WRITE(N'Carefully inspect and maintain the tires and crank arms.',0,NULL)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Appending additional data to the end of the column by setting   
-- @Offset to NULL.  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N' Appending data to the end of the column.', NULL, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing all data from @Offset to the end of the existing value by   
-- setting expression to NULL.   
UPDATE Production.Document  
SET DocumentSummary .WRITE (NULL, 56, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing partial data beginning at position 9 and ending at   
-- position 21.  
UPDATE Production.Document  
SET DocumentSummary .WRITE ('',9, 12)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
```  
  
#### <a name="t-using-update-with-openrowset-to-modify-a-varbinarymax-column"></a>T. Utilizzo di UPDATE con OPENROWSET per modificare una colonna di tipo varbinary(max)  
 L'esempio seguente sostituisce un'immagine esistente archiviata in una colonna di tipo **varbinary(max)** con una nuova immagine. La funzione [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) viene usata con l'opzione BULK per caricare l'immagine nella colonna. In questo esempio si presuppone che un file `Tires.jpg` esista nel percorso di file specificato.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ProductPhoto  
SET ThumbNailPhoto = (  
    SELECT *  
    FROM OPENROWSET(BULK 'c:Tires.jpg', SINGLE_BLOB) AS x )  
WHERE ProductPhotoID = 1;  
GO  
```  
  
#### <a name="u-using-update-to-modify-filestream-data"></a>U. Utilizzo di UPDATE per modificare dati FILESTREAM  
 Nell'esempio seguente viene utilizzata l'istruzione UPDATE per modificare i dati nel file del file system. Questo metodo non è consigliabile per trasmettere tramite flusso grandi quantità di dati in un file. Utilizzare le interfacce Win32 appropriate. Nel seguente esempio il testo nel record del file viene sostituito con il testo `Xray 1`. Per altre informazioni, vedere [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
```sql  
UPDATE Archive.dbo.Records  
SET [Chart] = CAST('Xray 1' as VARBINARY(max))  
WHERE [SerialNumber] = 2;  
```  
  
###  <a name="updating-user-defined-types"></a><a name="UDTs"></a> Aggiornamento di tipi definiti dall'utente  
 Negli esempi seguenti vengono modificati i valori nelle colonne di tipo CLR definito dall'utente. Vengono illustrati tre metodi. Per altre informazioni sulle colonne di tipo definito dall'utente, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
#### <a name="v-using-a-system-data-type"></a>V. Utilizzo di un tipo di dati di sistema  
 È possibile aggiornare un tipo definito dall'utente specificando un valore in un tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a condizione che il tipo definito dall'utente supporti la conversione implicita o esplicita da tale tipo. Nell'esempio seguente viene illustrato come aggiornare un valore in una colonna del tipo `Point` definito dall'utente, eseguendo la conversione esplicita da una stringa.  
  
```sql  
UPDATE dbo.Cities  
SET Location = CONVERT(Point, '12.3:46.2')  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="w-invoking-a-method"></a>W. Chiamata di un metodo  
 È possibile aggiornare un tipo definito dall'utente richiamando un metodo, contrassegnato come mutatore, del tipo definito dall'utente per eseguire l'aggiornamento. Nell'esempio seguente viene richiamato un metodo mutatore di tipo `Point` denominato `SetXY`. Viene aggiornato lo stato dell'istanza del tipo.  
  
```sql  
UPDATE dbo.Cities  
SET Location.SetXY(23.5, 23.5)  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="x-modifying-the-value-of-a-property-or-data-member"></a>X. Modifica del valore di una proprietà o di un membro dati  
 È possibile aggiornare un tipo definito dall'utente modificando il valore di una proprietà registrata o di un membro dati pubblico del tipo definito dall'utente. È necessario che l'espressione che fornisce il valore possa essere convertita in modo implicito nel tipo della proprietà. Nell'esempio seguente viene modificato il valore di proprietà `X` del tipo definito dall'utente `Point`.  
  
```sql  
UPDATE dbo.Cities  
SET Location.X = 23.5  
WHERE Name = 'Anchorage';  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> Override del comportamento predefinito di Query Optimizer tramite hint  
 Negli esempi contenuti in questa sezione viene illustrato come utilizzare gli hint di tabella e gli hint per le query per eseguire temporaneamente l'override del comportamento predefinito di Query Optimizer durante l'elaborazione dell'istruzione UPDATE.  
  
> [!CAUTION]  
>  Poiché Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente in genere di selezionare il piano di esecuzione migliore per una query, gli hint devono essere usati solo se strettamente necessari ed esclusivamente da sviluppatori e amministratori di database esperti.  
  
#### <a name="y-specifying-a-table-hint"></a>Y. Specifica di un hint di tabella  
 L'esempio seguente specifica l'[hint di tabella](../../t-sql/queries/hints-transact-sql-table.md) TABLOCK. L'hint specifica l'acquisizione di un blocco condiviso sulla tabella `Production.Product`. Tale blocco viene mantenuto attivo fino al termine dell'istruzione UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
#### <a name="z-specifying-a-query-hint"></a>Z. Specifica di un hint per la query  
 L'esempio seguente specificato l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md)`OPTIMIZE FOR (@variable)` nell'istruzione UPDATE. Tramite questo hint si indica a Query Optimizer di utilizzare un valore specifico per una variabile locale quando la query viene compilata e ottimizzata. Il valore viene utilizzato solo durante l'ottimizzazione della query e non durante l'esecuzione.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE Production.uspProductUpdate  
@Product NVARCHAR(25)  
AS  
SET NOCOUNT ON;  
UPDATE Production.Product  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE @Product  
OPTION (OPTIMIZE FOR (@Product = 'BK-%') );  
GO  
-- Execute the stored procedure   
EXEC Production.uspProductUpdate 'BK-%';  
```  
  
###  <a name="capturing-the-results-of-the-update-statement"></a><a name="CaptureResults"></a> Acquisizione dei risultati dell'istruzione UPDATE  
 Gli esempi contenuti in questa sezione illustrano come usare la [clausola OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) per restituire informazioni da (o espressioni basate su) ogni riga interessata da un'istruzione UPDATE. Questi risultati possono essere restituiti all'applicazione di elaborazione per l'utilizzo nei messaggi di errore, l'archiviazione e altri scopi simili dell'applicazione.  
  
#### <a name="aa-using-update-with-the-output-clause"></a>AA. Utilizzo di UPDATE con la clausola OUTPUT  
 Nell'esempio seguente viene aggiornata la colonna `VacationHours` nella tabella `Employee` del 25% per le prime 10 righe e viene inoltre impostato il valore nella colonna `ModifiedDate` sulla data corrente. La clausola `OUTPUT` restituisce il valore di `VacationHours` esistente prima di applicare l'istruzione `UPDATE` nella colonna `deleted.VacationHours` e il valore aggiornato nella colonna `inserted.VacationHours` alla variabile di tabella `@MyTableVar`.  
  
 Questa variabile è seguita da due istruzioni `SELECT` che restituiscono i valori in `@MyTableVar` e i risultati dell'operazione di aggiornamento nella tabella `Employee`. Per altri esempi sull'uso della clausola OUTPUT, vedere [Clausola OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate datetime);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
###  <a name="using-update-in-other-statements"></a><a name="Other"></a> Uso di UPDATE in altre istruzioni  
 Negli esempi inclusi in questa sezione viene illustrato l'utilizzo di UPDATE in altre istruzioni.  
  
#### <a name="ab-using-update-in-a-stored-procedure"></a>AB. Utilizzo di UPDATE in una stored procedure  
 Nell'esempio seguente viene usata un'istruzione UPDATE in una stored procedure. Per la stored procedure sono previsti un unico parametro di input `@NewHours` e un unico parametro di output `@RowCount`. Il valore del parametro `@NewHours` viene usato nell'istruzione UPDATE per aggiornare la colonna `VacationHours` della tabella `HumanResources.Employee`. Il parametro di output `@RowCount` viene usato per restituire il numero di righe interessate a una variabile locale. L'espressione CASE viene utilizzata nella clausola SET per determinare in modo condizionale il valore impostato per `VacationHours`. Quando un dipendente percepisce una paga oraria (`SalariedFlag` = 0), `VacationHours` viene impostato sul numero corrente di ore più il valore specificato in `@NewHours`. In caso contrario, `VacationHours` viene impostato sul valore specificato in `@NewHours`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE HumanResources.Update_VacationHours  
@NewHours SMALLINT  
AS   
SET NOCOUNT ON;  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN SalariedFlag = 0 THEN VacationHours + @NewHours  
         ELSE @NewHours  
       END  
    )  
WHERE CurrentFlag = 1;  
GO  
  
EXEC HumanResources.Update_VacationHours 40;  
```  
  
#### <a name="ac-using-update-in-a-trycatch-block"></a>AC. Uso di UPDATE in un blocco TRY...CATCH  
 L'esempio seguente usa un'istruzione UPDATE in un blocco TRY...CATCH per gestire gli errori di esecuzione che potrebbero verificarsi durante l'operazione di aggiornamento.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Intentionally generate a constraint violation error.  
    UPDATE HumanResources.Department  
    SET Name = N'MyNewName'  
    WHERE DepartmentID BETWEEN 1 AND 2;  
END TRY  
BEGIN CATCH  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="ad-using-a-simple-update-statement"></a>AD. Esecuzione di un'istruzione UPDATE semplice  
 Gli esempi seguenti illustrano gli effetti possibili su tutte le righe del mancato uso di una clausola WHERE per specificare la riga o le righe da aggiornare.  
  
 Questo esempio aggiorna i valori nelle colonne `EndDate` e `CurrentFlag` per tutte le righe nella tabella `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET EndDate = '2010-12-31', CurrentFlag='False';  
```  
  
 In un'istruzione UPDATE è inoltre possibile utilizzare valori calcolati. Nell'esempio seguente viene raddoppiato il valore della colonna `ListPrice` per tutte le righe della tabella `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET BaseRate = BaseRate * 2;  
```  
  
### <a name="ae-using-the-update-statement-with-a-where-clause"></a>AE. Uso dell'istruzione UPDATE con una clausola WHERE  
 Nell'esempio seguente viene utilizzata la clausola WHERE per specificare le righe da aggiornare.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET FirstName = 'Gail'  
WHERE EmployeeKey = 500;  
```  
  
### <a name="af-using-the-update-statement-with-label"></a>AF. Uso dell'istruzione UPDATE con un'etichetta  
 L'esempio seguente illustra l'uso dell'opzione LABEL in un'istruzione UPDATE.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimProduct  
SET ProductSubcategoryKey = 2   
WHERE ProductKey = 313  
OPTION (LABEL = N'label1');  
```  
  
### <a name="ag-using-the-update-statement-with-information-from-another-table"></a>AG. Utilizzo dell'istruzione UPDATE con informazioni di un'altra tabella  
 Questo esempio creata una tabella per archiviare le vendite totali per anno. L'esempio aggiorna le vendite totali per l'anno 2004 eseguendo un'istruzione SELECT sulla tabella FactInternetSales.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE TABLE YearlyTotalSales (  
    YearlySalesAmount MONEY NOT NULL,  
    Year SMALLINT NOT NULL )  
WITH ( DISTRIBUTION = REPLICATE );  
  
INSERT INTO YearlyTotalSales VALUES (0, 2004);  
INSERT INTO YearlyTotalSales VALUES (0, 2005);  
INSERT INTO YearlyTotalSales VALUES (0, 2006);  
  
UPDATE YearlyTotalSales  
SET YearlySalesAmount=  
(SELECT SUM(SalesAmount) FROM FactInternetSales WHERE OrderDateKey >=20040000 AND OrderDateKey < 20050000)  
WHERE Year=2004;  
  
SELECT * FROM YearlyTotalSales;   
```  

### <a name="ah-ansi-join-for-update-statements"></a>AH. Join ANSI per istruzioni UPDATE
Questo esempio illustra come aggiornare i dati in base al risultato del join con un'altra tabella.

```sql
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;
GO
```

## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Cursori &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Funzioni per i valori text e image &#40;Transact-SQL&#41;](../functions/text-and-image-functions-textptr-transact-sql.md)   
 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)  
 [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)    
 [Set di caratteri a byte singolo e multibyte](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
