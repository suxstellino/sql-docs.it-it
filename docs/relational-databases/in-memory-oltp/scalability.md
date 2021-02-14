---
title: Scalabilità | Microsoft Docs
description: Informazioni sui miglioramenti della scalabilità dell'archiviazione su disco per le tabelle ottimizzate per la memoria in SQL Server, ad esempio l'uso di più thread per rendere persistenti le tabelle.
ms.custom: ''
ms.date: 08/27/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a4891c57-56bb-49f4-9bb5-f11b745279e5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 45b69a78843a601bc30f1799901659ba6dd1c13e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353687"
---
# <a name="scalability"></a>Scalabilità
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] contiene miglioramenti di scalabilità dell'archiviazione su disco per le tabelle ottimizzate per la memoria. 

## <a name="multiple-threads-to-persist-memory-optimized-tables"></a>Più thread per rendere persistenti le tabelle ottimizzate per la memoria  
  
[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ha un unico thread di gestione dei checkpoint offline che analizza il log delle transazioni per rilevare le modifiche apportate alle tabelle ottimizzate per la memoria e le rende persistenti nei file di checkpoint, ad esempio nei file di dati e differenziali. In computer con un numero elevato di core, un unico thread di gestione dei checkpoint offline può non essere sufficiente.  
  
A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la persistenza delle modifiche apportate alle tabelle ottimizzate per la memoria è affidata a più thread simultanei.  
  
## <a name="multi-threaded-recovery"></a>Recupero multithread
Nella versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]l'applicazione del log come parte dell'operazione di recupero era a thread singolo. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], l'applicazione del log è multithread.  
  
## <a name="merge-operation"></a>Operazione MERGE  
Ora l'operazione MERGE è multithread.  
   
> [!NOTE]
> L'unione manuale è stata disabilitata perché si prevede che il carico possa essere gestito da un'unione multithread. 

## <a name="dynamic-management-views"></a>Viste a gestione dinamica  
Le DMV [sys.dm_db_xtp_checkpoint_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md) e [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) sono state modificate in modo significativo.  

## <a name="storage-management"></a>Gestione dell'archiviazione
Il motore OLTP in memoria continua a usare i filegroup ottimizzati per la memoria in base a FILESTREAM, ma i singoli file nel filegroup vengono disaccoppiati da FILESTREAM. Questi file sono completamente gestiti dal motore di OLTP in memoria, ad esempio per le operazioni di creazione, eliminazione e Garbage Collection. 

> [!NOTE]
> [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) non è supportato.  
  
## <a name="see-also"></a>Vedere anche   
[Creazione e gestione dell'archiviazione per gli oggetti con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)     
[Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)    
[Opzioni per file e filegroup ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)    
