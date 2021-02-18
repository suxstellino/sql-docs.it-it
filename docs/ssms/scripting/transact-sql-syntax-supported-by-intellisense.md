---
title: Sintassi Transact-SQL supportata da IntelliSense
description: Informazioni sulle istruzioni Transact-SQL e gli elementi di sintassi supportati da SQL Server Management Studio IntelliSense in SQL Server 2019 (15.x).
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL IntelliSense
- IntelliSense [SQL Server], Transact-SQL syntax
ms.assetid: 194e8f4f-fd7e-4f32-a169-f23531128004
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38c0ce9079dddb67e274fccb071aabbb778fb050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352917"
---
# <a name="transact-sql-syntax-supported-by-intellisense"></a>Sintassi Transact-SQL supportata da IntelliSense
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In questo argomento vengono descritti le istruzioni e gli elementi di sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] supportati da IntelliSense in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="statements-supported-by-intellisense"></a>Istruzioni supportate da IntelliSense  
 In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], IntelliSense supporta solo le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] di uso più frequente. Alcune condizioni generali dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] potrebbero impedire il funzionamento di IntelliSense. Per altre informazioni, vedere [Risoluzione dei problemi di IntelliSense &#40;SQL Server Management Studio&#41;](./troubleshooting-intellisense.md).  
  
> [!NOTE]  
>  IntelliSense non è disponibile per gli oggetti di database crittografati, ad esempio stored procedure o funzioni definite dall'utente crittografate. Le funzionalità Guida relativa ai parametri e Informazioni rapide non sono disponibili per i parametri di stored procedure estese e per i tipi definiti dall'utente di Integrazione con CLR.  
  
### <a name="select-statement"></a>Istruzione SELECT  
 L'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] offre supporto IntelliSense per gli elementi della sintassi seguenti nell'istruzione SELECT:  
  
:::row:::
    :::column:::
        SELECT
    :::column-end:::
    :::column:::
        WHERE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FROM
    :::column-end:::
    :::column:::
        ORDER BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        HAVING
    :::column-end:::
    :::column:::
        UNION
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FOR
    :::column-end:::
    :::column:::
        GROUP BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        TOP
    :::column-end:::
    :::column:::
        OPTION (hint)
    :::column-end:::
:::row-end:::

### <a name="additional-transact-sql-statements-that-are-supported"></a>Istruzioni Transact-SQL aggiuntive supportate  
 L'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] offre inoltre supporto IntelliSense per le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] elencate nella tabella seguente.  
  
|Istruzione Transact-SQL|Sintassi supportata|Eccezioni|  
|-----------------------------|----------------------|----------------|  
|[INSERT](../../t-sql/statements/insert-transact-sql.md)|Tutta la sintassi, eccetto la clausola *execute_statement* .|nessuno|  
|[UPDATE](../../t-sql/queries/update-transact-sql.md)|Tutta la sintassi.|nessuno|  
|[DELETE](../../t-sql/statements/delete-transact-sql.md)|Tutta la sintassi.|nessuno|  
|[DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)|Tutta la sintassi.|nessuno|  
|[SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)|Tutta la sintassi.|nessuno|  
|[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)|Esecuzione di stored procedure e di funzioni definite dall'utente e di sistema.|nessuno|  
|[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)|Tutta la sintassi.|nessuno|  
|[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)|Tutta la sintassi.|nessuno|  
|[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)|Tutta la sintassi.|Non è disponibile supporto IntelliSense per la clausola EXTERNAL NAME.<br /><br /> Nella clausola AS IntelliSense supporta solo le istruzioni e la sintassi elencate in questo argomento.|  
|[ALTER PROCEDURE](../../t-sql/statements/alter-procedure-transact-sql.md)|Tutta la sintassi|Non è disponibile supporto IntelliSense per la clausola EXTERNAL NAME.<br /><br /> Nella clausola AS IntelliSense supporta solo le istruzioni e la sintassi elencate in questo argomento.|  
|[USE](../../t-sql/language-elements/use-transact-sql.md)|Tutta la sintassi.|nessuno|  
  
## <a name="intellisense-in-supported-statements"></a>IntelliSense in istruzioni supportate  
 Nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] IntelliSense supporta i seguenti elementi della sintassi quando vengono utilizzati in una delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] supportate:  
  
-   Tutti i tipi di join, ad esempio APPLY  
  
-   PIVOT e UNPIVOT  
  
-   Riferimenti agli oggetti di database seguenti:  
  
    -   Database e schemi  
  
    -   Tabelle, viste, funzioni con valori di tabella ed espressioni di tabella  
  
    -   Colonne  
  
    -   Procedure e parametri di procedura  
  
    -   Funzioni ed espressioni scalari  
  
    -   Variabili locali  
  
    -   Espressioni di tabella comuni (CTE)  
  
-   Oggetti di database cui viene fatto riferimento solo in istruzioni CREATE o ALTER nello script o nel batch, ma che non esistono nel database perché lo script o il batch non è ancora stato eseguito. Questi oggetti sono i seguenti:  
  
    -   Tabelle e procedure specificate in un'istruzione CREATE TABLE o CREATE PROCEDURE nello script o nel batch.  
  
    -   Modifiche a tabelle e procedure specificate in un'istruzione ALTER TABLE o ALTER PROCEDURE nello script o nel batch.  
  
    > [!NOTE]  
    >  IntelliSense non è disponibile per le colonne di un'istruzione CREATE VIEW fino a che non è stata eseguita l'istruzione CREATE VIEW.  
  
 IntelliSense non è disponibile per gli elementi elencati sopra quando vengono usati in altre istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] . Il supporto IntelliSense è ad esempio disponibile per i nomi di colonna utilizzati in un'istruzione SELECT, ma non per le colonne utilizzate nell'istruzione CREATE FUNCTION.  
  
## <a name="examples"></a>Esempi  
 All'interno di uno script o di un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] , nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] IntelliSense supporta solo le istruzioni e la sintassi elencate in questo argomento. Negli esempi di codice [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti sono mostrati le istruzioni e gli elementi della sintassi che IntelliSense supporta. Ad esempio, nel batch seguente, IntelliSense è disponibile per l'istruzione `SELECT` quando è codificata da sola, ma non quando `SELECT` è contenuta in un'istruzione `CREATE FUNCTION` .  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE Name LIKE N'Road-250%' and Color = N'Red';  
GO  
CREATE FUNCTION Production.ufn_Red250 ()  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT Name  
    FROM AdventureWorks2012.Production.Product  
    WHERE Name LIKE N'Road-250%'  
      AND Color = N'Red'  
);GO  
```  
  
 Questa funzionalità si applica anche ai set di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] nella clausola AS di un'istruzione CREATE PROCEDURE o ALTER PROCEDURE.  
  
 All'interno di uno script o un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] , IntelliSense supporta gli oggetti che sono stati specificati in un'istruzione CREATE o ALTER, ma questi oggetti non esistono nel database perché le istruzioni non sono state eseguite. È possibile ad esempio immettere nell'editor di query il codice seguente:  
  
```  
USE MyTestDB;  
GO  
CREATE TABLE MyTable  
    (PrimaryKeyCol   INT PRIMARY KEY,  
    FirstNameCol      NVARCHAR(50),  
   LastNameCol       NVARCHAR(50));  
GO  
SELECT   
```  
  
 Quando si digita `SELECT`, IntelliSense elenca **PrimaryKeyCol**, **FirstNameCol** e **LastNameCol** come possibili elementi dell'elenco di selezione, anche se lo script non è stato eseguito e `MyTable` non esiste ancora in `MyTestDB`.  
  
