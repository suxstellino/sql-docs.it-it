---
title: Supporto di Transact-SQL per OLTP in memoria | Microsoft Docs
description: Informazioni sulle istruzioni Transact-SQL che includono opzioni della sintassi per supportare OLTP in memoria. Usare collegamenti a riferimenti aggiuntivi sulle funzionalità supportate.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: b1cc7c30-1747-4c21-88ac-e95a5e58baac
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a531bc3e7b997bf160b28e2b19a44bb39e54983
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492181"
---
# <a name="transact-sql-support-for-in-memory-oltp"></a>Supporto di Transact-SQL per OLTP in memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti includono opzioni della sintassi per supportare OLTP in memoria:  
  
-   [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) (aggiunta di **MEMORY_OPTIMIZED_DATA**)  
  
-   [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) (aggiunta di **MEMORY_OPTIMIZED_DATA**)  
  
-   [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
-   [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
  
-   [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)  
  
-   [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
    In una stored procedure compilata in modo nativo è possibile dichiarare una variabile come **NOT NULL**. Questo non è possibile in una stored procedure normale.  
  
 A partire da SQL Server 2016,**AUTO_UPDATE_STATISTICS** può essere **ON** per le tabelle ottimizzate per la memoria. Per altre informazioni, vedere [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md).  
  
 [SET STATISTICS XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-xml-transact-sql.md) ON non è supportata dalle stored procedure compilate in modo nativo.  
  
 Per informazioni su funzionalità non supportate, vedere [Costrutti Transact-SQL non supportati da OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
 Per informazioni sui costrutti supportati nelle stored procedure compilate in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md) e [Costrutti supportati su stored procedure compilate in modo nativo](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md).  
  
## <a name="see-also"></a>Vedere anche  
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)   
 [Problemi di migrazione relativi alle stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Funzionalità di SQL Server non supportate per OLTP in memoria](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)   
 [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
