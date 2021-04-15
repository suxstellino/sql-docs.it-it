---
title: Costrutti DDL supportati per i moduli T-SQL compilati in modo nativo | Microsoft Docs
description: Informazioni sui costrutti DDL supportati per i moduli T-SQL compilati in modo nativo, ad esempio stored procedure, funzioni definite dall'utente scalari, funzioni con valori di tabella inline e trigger.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6b21f47e-bceb-4054-8b3c-9d39bb9583c0
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a26582c58c47266a54b08fb7c6fa1e7f4699bf88
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492545"
---
# <a name="supported-ddl-for-natively-compiled-t-sql-modules"></a>Costrutti DDL supportati per i moduli T-SQL compilati in modo nativo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Questo argomento elenca i costrutti DDL supportati per i moduli T-SQL compilati in modo nativo, ad esempio stored procedure, funzioni definite dall'utente scalari, funzioni con valori di tabella inline e trigger.  
  
 Per informazioni sulle funzionalità e sulla superficie di attacco di T-SQL che possono essere usate nei moduli T-SQL compilati in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
 Per altre informazioni su costrutti non supportati, vedere [Costrutti Transact-SQL non supportati da OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
 Sono supportati gli elementi seguenti:  
  
-   [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
-   [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)  
  
-   [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md) e istruzioni INSERT SELECT  
  
-   SCHEMABINDING e BEGIN ATOMIC (obbligatori per le stored procedure compilate in modo nativo)  
  
     Per altre informazioni, vedere [Creazione di stored procedure compilate in modo nativo](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md).  
  
-   NATIVE_COMPILATION  
  
     Per altre informazioni, vedere [Compilazione nativa di tabelle e stored procedure](../../relational-databases/in-memory-oltp/native-compilation-of-tables-and-stored-procedures.md).  
  
-   Parametri e variabili possono essere dichiarati come NOT NULL (disponibile solo per i moduli compilati in modo nativo: stored procedure compilate in modo nativo e funzioni scalari definite dall'utente compilate in modo nativo).  
  
-   Parametri con valori di tabella.  
  
     Per altre informazioni, vedere [Usare parametri con valori di tabella &#40;Motore di database&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
-   EXECUTE AS OWNER, SELF, CALLER e utente.  
  
-   Autorizzazioni GRANT e DENY per tabelle e procedure.  
  
     Per altre informazioni, vedere [GRANT - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md) e [DENY - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
