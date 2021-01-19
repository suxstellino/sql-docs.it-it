---
title: Funzionalità per i moduli T-SQL compilati in modo nativo
description: Informazioni sulla superficie di attacco e sulle funzionalità supportate di T-SQL nel corpo dei moduli T-SQL compilati in modo nativo, ad esempio stored procedure e funzioni scalari definite dall'utente.
ms.custom: seo-dt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 05515013-28b5-4ccf-9a54-ae861448945b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 42d638afd9fc98fb68eb024c0a712bea5f278a26
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170223"
---
# <a name="supported-features-for-natively-compiled-t-sql-modules"></a>Funzionalità supportate per i moduli T-SQL compilati in modo nativo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


  Questo argomento contiene un elenco delle superfici di attacco e delle funzionalità supportate di T-SQL nei moduli di T-SQL compilati in modo nativo, ad esempio stored procedure ([CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)), funzioni scalari definite dall'utente, funzioni inline con valori di tabella e trigger.  

 Per le funzionalità supportate relative alla definizione dei moduli nativi, vedere [Supported DDL for Natively Compiled T-SQL modules](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md)(DDL supportate per moduli T-SQL compilati in modo nativo).  

 Per informazioni complete sui costrutti non supportati e per informazioni su come sopperire ad alcune funzionalità non supportate nei moduli compilati in modo nativo, vedere [Migration Issues for Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md). Per altre informazioni su funzionalità non supportate, vedere [Costrutti Transact-SQL non supportati da OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  

##  <a name="query-surface-area-in-native-modules"></a><a name="qsancsp"></a> Superficie di attacco delle query nei moduli nativi  

Sono supportati i costrutti delle query indicati di seguito:  

Espressione CASE: L'espressione CASE può essere utilizzata in qualsiasi istruzione o clausola che consenta un'espressione valida.
   - **Si applica a**: [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
    A partire da [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], le istruzioni CASE sono supportate per i moduli T-SQL compilati in modo nativo.

Clausola SELECT:  

-   Alias di nomi e colonne (usando la sintassi AS o =).  

-   Sottoquery scalari
    - **Si applica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      A partire da [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], le sottoquery scalari sono supportate per i moduli compilati in modo nativo.

-   TOP*  

-   SELECT DISTINCT  
    - **Si applica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      A partire da [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], l'operatore DISTINCT è supportato in moduli compilati in modo nativo.

        - Le aggregazioni DISTINCT non sono supportate.  

-   UNION e UNION ALL
    - **Si applica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      A partire da [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], gli operatori UNION e UNION ALL sono supportati in moduli compilati in modo nativo.

-   Assegnazioni di variabili  

Clausola FROM:  

-   FROM \<memory optimized table or table variable>  

-   FROM \<natively compiled inline TVF>  

-   LEFT OUTER JOIN, RIGHT OUTER JOIN, CROSS JOIN e INNER JOIN.
    - **Si applica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      A partire da [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], gli operatori JOINS sono supportati in moduli compilati in modo nativo.

-   Sottoquery `[AS] table_alias`. Per altre informazioni, vedere [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md). 
    - **Si applica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      A partire da [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], le sottoquery sono supportate in moduli compilati in modo nativo.

Clausola WHERE:  

-   Predicato del filtro IS [NOT] NULL  

-   AND, BETWEEN  
-   OR, NOT, IN, EXISTS
    - **Si applica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      A partire da [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], gli operatori OR/NOT/IN/EXISTS sono supportati in moduli compilati in modo nativo.


Clausola[GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md) :

- Funzioni di aggregazione AVG, COUNT, COUNT_BIG, MIN, MAX e SUM.  

- MIN e MAX non sono supportate per i tipi nvarchar, char, varchar, varchar, varbinary e binary.  

Clausola[ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md) :


- **DISTINCT** non è supportato nella clausola **ORDER BY** .


- Supportata con [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md) se un'espressione nell'elenco ORDER BY appare in modo identico nell'elenco GROUP BY.
  - Ad esempio, l'espressione GROUP BY a + b ORDER BY a + b è supportata, ma GROUP BY a, b ORDER BY a + b non lo è.  


Clausola HAVING:

- Soggetta alle stesse limitazioni delle espressioni della clausola WHERE.


#### <a name="order-by-and-top-are-supported-in-natively-compiled-modules-with-some-restrictions"></a>ORDER BY e TOP sono supportati nei moduli compilati in modo nativo con alcune limitazioni


- **WITH TIES** o **PERCENT** non è supportata nella clausola **TOP** .


- **DISTINCT** non è supportato nella clausola **ORDER BY** .  


- **TOP** in combinazione con **ORDER BY** non supporta un valore superiore a 8.192 quando si utilizza una costante nella clausola **TOP** .
  - Questo limite può risultare più basso nel caso in cui la query contenga funzioni di aggregazione o dei join. Ad esempio, con un join (due tabelle) il limite scende a 4.096 righe. Con due join (tre tabelle) il limite è 2.730 righe.  
  - È possibile che si ottengano risultati maggiori di 8.192 archiviando il numero di righe in una variabile:  

```sql
DECLARE @v INT = 9000;
SELECT TOP (@v) ... FROM ... ORDER BY ...
```

Tuttavia, una costante nella clausola **TOP** assicura prestazioni migliori rispetto a una variabile.  

Queste limitazioni di [!INCLUDE[tsql](../../includes/tsql-md.md)] non si applicano all'accesso [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato nelle tabelle ottimizzate per la memoria.  


##  <a name="data-modification"></a><a name="dml"></a> Modifica dei dati  

Sono supportate le istruzioni DML seguenti.  

-   INSERT VALUES (una riga per istruzione) e INSERT ... SELECT  

-   UPDATE  

-   DELETE  

-   La clausola WHERE è supportata con le istruzioni UPDATE e DELETE.  

##  <a name="control-of-flow-language"></a><a name="cof"></a> Elementi del linguaggio per il controllo di flusso  
 Sono supportati i seguenti costrutti degli elementi del linguaggio per il controllo di flusso.  

-   [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)  

-   [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  

-   [RETURN &#40;Transact-SQL&#41;](../../t-sql/language-elements/return-transact-sql.md)  

-   [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md) può usare tutti i [tipi di dati supportati per OLTP in memoria](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md), nonché i tipi di tabella ottimizzata per la memoria. Le variabili possono essere dichiarate NULL o NOT NULL.  

-   [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  

-   [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  

    - Per ottimizzare le prestazioni, usare un solo blocco TRY/CATCH per un intero modulo T-SQL compilato in modo nativo.  

-   [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)  

-   BEGIN ATOMIC (al livello esterno della stored procedure). Per altri dettagli, vedere [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  

##  <a name="supported-operators"></a><a name="so"></a> Operatori supportati  
 Di seguito vengono elencati gli operatori supportati.  

-   [Operatori di confronto &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md) (ad esempio >, \<, >= e <=)  

-   Operatori unari (+, -).  

-   Operatori binari (*, /, +, -, % (modulo)).  

    - L'operatore di addizione (+) è supportato sia con i numeri che con le stringhe.  

-   Operatori logici (AND, OR, NOT).  

-   Operatori bit per bit ~, &, | e ^  

-   APPLY - operatore
    - **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].  
      A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], l'operatore APPLY è supportato in moduli compilati in modo nativo.

##  <a name="built-in-functions-in-natively-compiled-modules"></a><a name="bfncsp"></a> Funzioni integrate nei moduli compilati in modo nativo  
 Le seguenti funzioni sono supportate nei vincoli nelle tabelle ottimizzate per la memoria e nei moduli T-SQL compilati in modo nativo.  

-   Tutte le [funzioni matematiche &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  

-   Funzioni di data: CURRENT_TIMESTAMP, DATEADD, DATEDIFF, DATEFROMPARTS, DATEPART, DATETIME2FROMPARTS, DATETIMEFROMPARTS, DAY, EOMONTH, GETDATE, GETUTCDATE, MONTH, SMALLDATETIMEFROMPARTS, SYSDATETIME, SYSUTCDATETIME e YEAR.  

-   Funzioni di stringa: LEN, LTRIM, RTRIM e SUBSTRING.  
    - **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].  
      A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] sono supportate anche le funzioni integrate seguenti: TRIM, TRANSLATE e CONCAT_WS.  

-   Funzioni di identità: SCOPE_IDENTITY  

-   Funzioni NULL: ISNULL  

-   Funzioni di identificazione univoca: NEWID e NEWSEQUENTIALID  

-   Funzioni JSON  
    - **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].  
      A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], le funzioni JSON sono supportate nei moduli compilati in modo nativo.

-   Funzioni di errore: ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY ed ERROR_STATE  

-   Funzioni di sistema: @@rowcount. Le istruzioni all'interno di stored procedure compilate in modo nativo aggiornano @@rowcount ed è possibile usare @@rowcount in una stored procedure compilata in modo nativo per determinare il numero di righe interessate dall'ultima istruzione eseguita all'interno della stored procedure. Tuttavia, @@rowcount viene reimpostato su 0 all'inizio e alla fine dell'esecuzione di una stored procedure compilata in modo nativo.  

-   Funzioni di sicurezza: IS_MEMBER({'group' | 'role'}), IS_ROLEMEMBER ('role' [, 'database_principal']), IS_SRVROLEMEMBER ('role' [, 'login']), ORIGINAL_LOGIN(), SESSION_USER, CURRENT_USER, SUSER_ID(['login']), SUSER_SID(['login'] [, Param2]), SUSER_SNAME([server_user_sid]), SYSTEM_USER, SUSER_NAME, USER, USER_ID(['user']), USER_NAME([id]), CONTEXT_INFO().

-   Le esecuzioni dei moduli nativi possono essere annidate.

##  <a name="auditing"></a><a name="auditing"></a> Controllo  
 Il controllo a livello di routine è supportato nelle stored procedure compilate in modo nativo.  

 Per ulteriori informazioni sul controllo, vedere [Creazione di un controllo del server e di una specifica del controllo del database](../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md).  

##  <a name="table-and-query-hints"></a><a name="tqh"></a> Hint di tabella e per la query  
 Sono supportati gli elementi seguenti:  

-   Gli hint INDEX, FORCESCAN e FORCESEEK, nella sintassi di hint di tabella o nella [clausola OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md) della query. Per altre informazioni, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  

-   FORCE ORDER  

-   Hint LOOP JOIN  

-   OPTIMIZE FOR  

 Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  

##  <a name="limitations-on-sorting"></a><a name="los"></a> Limitazioni relative all'ordinamento  
 È possibile ordinare più di 8.000 righe in una query che usa [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) e una clausola [ORDER BY Clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md). Tuttavia, senza la [ clausola ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md), [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) può ordinare fino a 8.000 righe, meno se sono presenti join.  

 Se la query usa sia l'operatore [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) che una [clausola ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md), è possibile specificare fino a 8192 righe per l'operatore TOP. Se si specificano più di 8192 righe viene visualizzato il messaggio di errore seguente: **Messaggio 41398, livello 16, stato 1, procedura *\<procedureName>* , riga *\<lineNumber>* L'operatore TOP può restituire un massimo di 8192 righe. Il numero richiesto è *\<number>* .**  

 Se non si dispone di una clausola TOP, è possibile ordinare qualsiasi numero di righe con ORDER BY.  

 Se non si utilizza una clausola ORDER BY, è possibile utilizzare qualsiasi valore intero con l'operatore TOP.  

 Esempio con TOP N = 8192: esegue la compilazione  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8192 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 Esempio con TOP N > 8192: non riesce a compilare.  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8193 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 Il limite di 8192 righe si applica solo a `TOP N``N` dove  è una costante, come negli esempi precedenti.  Se è necessario un valore `N` maggiore di 8192 è possibile assegnare il valore a una variabile e utilizzare tale variabile con `TOP`.  

 Esempio d'uso di una variabile: esegue la compilazione  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    DECLARE @v int = 8193   
    SELECT TOP (@v) ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 **Limitazioni per le righe restituite:** In due casi è possibile che il numero di righe restituibili dall'operatore TOP venga ridotto:  

-   Utilizzando JOIN nella query.  L'influenza di JOIN sulla limitazione dipende dal piano di query.  

-   Utilizzando funzioni di aggregazione o riferimenti a funzioni di aggregazione nella clausola ORDER BY.  

 La formula per calcolare un valore N massimo supportato nel caso peggiore in TOP N è: `N = floor ( 65536 / number_of_tables * 8 + total_size+of+aggs )`.  

## <a name="see-also"></a>Vedere anche  
 [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Problemi di migrazione relativi alle stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)