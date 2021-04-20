---
description: Impostare o modificare le regole di confronto delle colonne
title: Impostare o modificare le regole di confronto delle colonne | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- tempdb database [SQL Server], collations
- collations [SQL Server], column
ms.assetid: d7a9638b-717c-4680-9b98-8849081e08be
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a96458ffbe5330c4d2ee2e2f7a13f0e8062ac6c7
ms.sourcegitcommit: 708b3131db2e542b1d461d17dec30d673fd5f0fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107729208"
---
# <a name="set-or-change-the-column-collation"></a>Impostare o modificare le regole di confronto delle colonne
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile ignorare le regole di confronto del database per i dati **char**, **varchar**, **text**, **nchar**, **nvarchar** e **ntext** specificando regole di confronto diverse per una colonna specifica di una tabella e utilizzando uno degli elementi seguenti:  
  
-   Clausola COLLATE di [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) e [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md), come illustrato negli esempi seguenti. 

    -   **Conversione sul posto.** Considerare una delle tabelle esistenti definite di seguito:

        ```sql
        -- NVARCHAR column is encoded in UTF-16 because a supplementary character enabled collation is used
        CREATE TABLE dbo.MyTable (CharCol NVARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC);

        -- VARCHAR column is encoded the Latin code page and therefore is not Unicode capable
        CREATE TABLE dbo.MyTable (CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI);
        ```

        Per convertire la colonna sul posto per l'uso di UTF-8, eseguire un'istruzione `ALTER COLUMN` che imposta il tipo di dati richiesto e le regole di confronto con supporto di UTF-8:

        ```sql 
        ALTER TABLE dbo.MyTable 
        ALTER COLUMN CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8
        ```

        Questo metodo è facile da implementare, ma è un'operazione potenzialmente pesante, che può diventare un problema per tabelle di grandi dimensioni e applicazioni usate in modo intensivo.

    -   **Copiare e sostituire.** Considerare una delle tabelle esistenti definite di seguito:

        ```sql
        -- NVARCHAR column is encoded in UTF-16 because a supplementary character enabled collation is used
        CREATE TABLE dbo.MyTable (CharCol NVARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC);
        GO

        -- VARCHAR column is encoded using the Latin code page and therefore is not Unicode capable
        CREATE TABLE dbo.MyTable (CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI);
        GO
        ```

        Per convertire la colonna in modo che usi la codifica UTF-8, copiare i dati in una nuova tabella in cui la colonna di destinazione ha già il tipo di dati richiesto e regole di confronto abilitate per UTF-8, quindi sostituire la tabella precedente:

        ```sql
        CREATE TABLE dbo.MyTableNew (CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8);
        GO
        INSERT INTO dbo.MyTableNew 
        SELECT * FROM dbo.MyTable;
        GO
        DROP TABLE dbo.MyTable;
        GO
        EXEC sp_rename 'dbo.MyTableNew', 'dbo.MyTable';
        GO
        ```

        Questo metodo è molto più veloce rispetto alla conversione sul posto, ma la gestione di schemi complessi con molte dipendenze (FK, SDK, trigger, DFs) e la sincronizzazione della parte finale della tabella (se il database è in uso) richiede una pianificazione maggiore.
        
    Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere [Modificare colonne (motore di database)](../../relational-databases/tables/modify-columns-database-engine.md#SSMSProcedure).  
  
-   Uso della proprietà **Column.Collation** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects (SMO).  
  
 Non è possibile modificare le regole di confronto di una colonna a cui fa riferimento uno qualsiasi degli elementi seguenti:  
  
-   Una colonna calcolata  
-   Un indice  
-   Statistiche di distribuzione, generate automaticamente o tramite l'istruzione `CREATE STATISTICS`  
-   Un vincolo CHECK  
-   Un vincolo FOREIGN KEY  
  
 Quando si usa il database **tempdb**, la clausola [COLLATE](~/t-sql/statements/collations.md) include un'opzione *database_default* per specificare che una colonna di una tabella temporanea utilizza le regole di confronto predefinite del database utente corrente per la connessione anziché quelle del database **tempdb**.  
  
## <a name="collations-and-text-columns"></a>Regole di confronto e colonne di tipo text  
 È possibile inserire o aggiornare valori in una colonna **text** le cui regole di confronto sono diverse dalla tabella codici delle regole di confronto predefinite del database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] converte in modo implicito i valori nelle regole di confronto della colonna.  
  
## <a name="collations-and-tempdb"></a>Regole di confronto e database tempdb  
 Il database **tempdb** viene compilato a ogni avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e presenta le stesse regole di confronto predefinite del database **model** . Queste sono generalmente le stesse regole di confronto predefinite dell'istanza. Se si crea un database utente e si specificano regole di confronto predefinite diverse da quelle del database **model**, il database utente utilizzerà regole di confronto predefinite diverse da quelle di **tempdb**. Tutte le stored procedure temporanee o le tabelle temporanee vengono create e archiviate in **tempdb**. Di conseguenza, tutte le colonne implicite delle tabelle temporanee e tutte le costanti, le variabili e i parametri a cui possono essere assegnati valori predefiniti e che si trovano in stored procedure temporanee, utilizzano regole di confronto diverse da quelle degli oggetti analoghi creati in tabelle e stored procedure permanenti.  
  
 Ciò può causare problemi derivanti da una non corrispondenza nelle regole di confronto tra i database definiti dall'utente e gli oggetti di database del sistema. Si supponga ad esempio che un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzi le regole di confronto Latin1_General_CS_AS e di eseguire le seguenti istruzioni:  
  
```sql  
CREATE DATABASE TestDB COLLATE Estonian_CS_AS;  
USE TestDB;  
CREATE TABLE TestPermTab (PrimaryKey int PRIMARY KEY, Col1 nchar );  
```  
  
 In questo sistema nel database **tempdb** vengono utilizzate le regole di confronto Latin1_General_CS_AS con la tabella codici 1252, mentre in `TestDB` e `TestPermTab.Col1` vengono utilizzate le regole di confronto `Estonian_CS_AS` con la tabella codici 1257. Ad esempio:  
  
```sql  
USE TestDB;  
GO  
-- Create a temporary table with the same column declarations  
-- as TestPermTab  
CREATE TABLE #TestTempTab (PrimaryKey int PRIMARY KEY, Col1 nchar );  
INSERT INTO #TestTempTab  
         SELECT * FROM TestPermTab;  
GO  
```  
  
 Con l'esempio precedente, il database **tempdb** utilizza le regole di confronto Latin1_General_CS_AS, mentre `TestDB` e `TestTab.Col1` utilizzano le regole di confronto `Estonian_CS_AS` . Ad esempio:  
  
```sql  
SELECT * FROM TestPermTab AS a INNER JOIN #TestTempTab on a.Col1 = #TestTempTab.Col1;  
```  
  
 Poiché **tempdb** utilizza le regole di confronto predefinite del server e `TestPermTab.Col1` utilizza regole di confronto diverse, in SQL Server viene restituito un errore in cui viene indicato che è impossibile risolvere il conflitto delle regole di confronto tra Latin1_General_CI_AS_KS_WS ed Estonian_CS_AS nell'operazione.  
  
 Per evitare l'errore è possibile utilizzare una delle alternative seguenti:  
  
-   Specificare che per la colonna della tabella temporanea siano utilizzate le regole di confronto predefinite del database utente, anziché quelle del database **tempdb**. Ciò consente alla tabella temporanea di utilizzare tabelle con formattazione simile in più database, se ciò è richiesto dal sistema.  
  
    ```sql  
    CREATE TABLE #TestTempTab  
       (PrimaryKey int PRIMARY KEY,  
        Col1 nchar COLLATE database_default  
       );  
    ```  
  
-   Specificare le regole di confronto corrette per la colonna `#TestTempTab` :  
  
    ```sql  
    CREATE TABLE #TestTempTab  
       (PrimaryKey int PRIMARY KEY,  
        Col1 nchar COLLATE Estonian_CS_AS  
       );  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Impostazione o modifica di regole di confronto del server](../../relational-databases/collations/set-or-change-the-server-collation.md)   
 [Impostare o modificare le regole di confronto del database](../../relational-databases/collations/set-or-change-the-database-collation.md)   
 [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  
