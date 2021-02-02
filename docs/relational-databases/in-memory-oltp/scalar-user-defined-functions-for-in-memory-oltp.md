---
title: Funzioni scalari definite dall'utente per OLTP in memoria | Microsoft Docs
description: Informazioni su come creare ed eliminare funzioni scalari definite dall'utente e compilate in modo nativo per OLTP in memoria in SQL Server. La compilazione nativa migliora le prestazioni.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: d2546e40-fdfc-414b-8196-76ed1f124bf5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1a120e2e36a1ebbef11f5dec21b8119381c300e5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236123"
---
# <a name="scalar-user-defined-functions-for-in-memory-oltp"></a>Funzioni scalari definite dall'utente per OLTP in memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]consente di creare ed eliminare funzioni scalari definite dall'utente e compilate in modo nativo. Tali funzioni possono anche essere modificate. La compilazione nativa migliora le prestazioni di valutazione delle funzioni definite dall'utente in Transact-SQL.  
  
 Quando si modifica una funzione scalare definita dall'utente e compilata in modo nativo, l'applicazione resta disponibile durante l'esecuzione dell'operazione e la compilazione della nuova versione della funzione.  
  
 Per i costrutti T-SQL supportati, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
## <a name="creating-dropping-and-altering-user-defined-functions"></a>Creazione, eliminazione e modifica delle funzioni definite dall'utente  
 Usare CREATE FUNCTION per creare la funzione scalare definita dall'utente e compilata in modo nativo, DROP FUNCTION per eliminarla e ALTER FUNCTION per modificarla. BEGIN ATOMIC WITH è obbligatorio per le funzioni definite dall'utente.  
  
 Per informazioni sulla sintassi supportata ed eventuali restrizioni, vedere gli argomenti seguenti.  
  
-   [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
  
-   [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)  
  
-   [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)  
  
     La sintassi DROP FUNCTION delle funzioni scalari definite dall'utente e compilate in modo nativo è uguale a quella delle funzioni interpretate definite dall'utente.  
  
-   [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
  
 È possibile usare la stored procedure [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) con la funzione scalare definita dall'utente e compilata in modo nativo. Ne risulterà la funzione ricompilata usando la definizione presente nei metadati.  
  
 Nell'esempio seguente viene illustrata una funzione scalare definita dall'utente contenuta nel database di esempio [AdventureWorks2016CTP3](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) .  
  
```sql  
CREATE FUNCTION [dbo].[ufnLeadingZeros_native](@Value int)   
RETURNS varchar(8)   
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS   
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')  
  
    DECLARE @ReturnValue varchar(8);  
    SET @ReturnValue = CONVERT(varchar(8), @Value);  
       DECLARE @i int = 0, @count int = 8 - LEN(@ReturnValue)  
  
    WHILE @i < @count  
       BEGIN  
            SET @ReturnValue = '0' + @ReturnValue;  
            SET @i += 1  
       END  
  
    RETURN (@ReturnValue);  
  
END  
```  
  
## <a name="calling-user-defined-functions"></a>Chiamata di funzioni definite dall'utente  
 È possibile usare le funzioni scalari definite dall'utente e compilate in modo nativo nelle espressioni, collocandole nella stessa posizione delle funzioni scalari predefinite e delle funzioni scalari definite dall'utente interpretate. Possono essere usate con l'istruzione EXECUTE, in un'istruzione Transact-SQL e in una stored procedure compilata in modo nativo,  
  
 nelle stored procedure compilate in modo nativo e nelle funzioni definite dall'utente e compilate in modo nativo, e ovunque sia consentito l'uso di funzioni predefinite. È possibile usare le funzioni scalare definite dall'utente e compilate in modo nativo nei moduli tradizionali di Transact-SQL.  
  
 Le funzioni scalari definite dall'utente funzionano in modalità di interoperabilità, ovunque possa essere usata una funzione scalare definita dall'utente interpretata. Questo utilizzo è soggetto alle limitazioni delle transazioni tra contenitori, come descritto nella sezione **Supported Isolation Levels for Cross-Container Transactions** (Livelli di isolamento supportati per le transazioni tra contenitori) di [Transactions with Memory-Optimized Tables](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)(Transazioni con tabelle con ottimizzazione per la memoria). Per altre informazioni sulla modalità di interoperabilità, vedere [Accesso alle tabelle con ottimizzazione per la memoria utilizzando codice Transact-SQL interpretato](../../relational-databases/in-memory-oltp/accessing-memory-optimized-tables-using-interpreted-transact-sql.md).  
  
 Le funzioni scalari definite dall'utente e compilate in modo nativo necessitano di un contesto di esecuzione esplicito. Per altre informazioni, vedere [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md). EXECUTE AS CALLER non è supportata. Per altre informazioni, vedere [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Per la sintassi supportata dalle istruzioni Transact-SQL Execute relativa alle funzioni scalari definite dall'utente e compilate in modo nativo, vedere [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md). Per la sintassi supportata per l'esecuzione delle funzioni definite dall'utente in una stored procedure compilata in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
## <a name="hints-and-parameters"></a>Hint e parametri  
 Il supporto per tabella, join e hint per la query all'interno delle funzioni scalari definite dall'utente e compilate in modo nativo è uguale al supporto per gli nelle stored procedure compilate in modo nativo. Come con le funzioni scalari definite dall'utente interpretate, gli hint per le query inclusi in una query Transact-SQL che fanno riferimento a una funzione scalari definite dall'utente e compilata in modo nativo non influiscono sul piano di query per tale funzione definita dall'utente.  
  
 Queste funzioni supportano tutti i parametri supportati per le stored procedure compilate in modo nativo, a condizione che i parametri siano consentiti dalle funzioni scalari definite dall'utente. Il parametro con valori di tabella è un esempio di parametro supportato.  
  
## <a name="schema-bound"></a>Associazione a uno schema  
 Quanto segue si applica alle funzioni scalari definite dall'utente e compilate in modo nativo.  
  
-   È necessaria l'associazione a uno schema tramite l'argomento WITH SCHEMABINDING nell'istruzione CREATE FUNCTION e ALTER FUNCTION.  
  
-   La funzione che fa riferimento a una stored procedure con associazione a uno schema o definita dall'utente non può essere eliminata o modificata.  
  
## <a name="showplan_xml"></a>SHOWPLAN_XML  
 Le funzioni scalari definite dall'utente e compilate in modo nativo supportano SHOWPLAN_XML. È conforme allo schema SHOWPLAN_XML generale, come nelle stored procedure compilate in modo nativo. L'elemento di base per le funzioni definite dall'utente è `<UDF>`.  
  
 Le funzioni scalari definite dall'utente e compilate in modo nativo non supportano STATISTICS XML. Quando si esegue una query che fa riferimento alla funzione definita dall'utente, con l'opzione STATISTICS XML abilitata, viene restituito il contenuto XML senza la parte per la stored procedure definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Come per le stored procedure compilate in modo nativo, al momento della creazione della funzione vengono controllate le autorizzazioni per gli oggetti a cui la funzione fa riferimento. L'istruzione CREATE FUNCTION ha esito negativo se l'utente rappresentato non dispone delle autorizzazioni corrette. Se le modifiche alle autorizzazioni dell'utente rappresentato fanno sì che l'utente perda le autorizzazioni corrette, le successive esecuzioni della funzione definita dall'utente avranno esito negativo.  
  
 Quando si usa una funzione scalare definita dall'utente e compilata in modo nativo all'interno di una stored procedure compilata in modo nativo, le autorizzazioni per l'esecuzione della funzione definita dall'utente vengono controllate al momento della creazione della routine esterna. Se l'utente rappresentato dalla ruotine esterna non dispone di autorizzazioni EXEC per la funzione definita dall'utente, la creazione della stored procedure ha esito negativo. Se le modifiche alle autorizzazioni fanno sì che l'utente perda le autorizzazioni EXEC, l'esecuzione della procedura esterna ha esito negativo.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Salvare un piano di esecuzione in formato XML](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
