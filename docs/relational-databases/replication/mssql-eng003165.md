---
description: MSSQL_ENG003165
title: MSSQL_ENG003165 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG003165 error
ms.assetid: 707d33dd-644e-4cc9-ac51-dddd49031530
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 5c5e213a50b08b685bc6984ebfad5b4629dd1134
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198971"
---
# <a name="mssql_eng003165"></a>MSSQL_ENG003165
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|3165|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Il database '%ls' è stato ripristinato ma è stato rilevato un errore durante il ripristino o la rimozione della replica. Il database è stato lasciato offline. Vedere l'argomento MSSQL_ENG003165 della documentazione online di SQL Server.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore viene generato se si verifica un problema durante il ripristino di un backup di un database replicato:  
  
-   Se il ripristino del backup avviene nello stesso database e nello stesso server sui quali è stato eseguito, l'errore indica che non è stato possibile ripristinare correttamente le impostazioni della replica.  
  
-   Se invece il backup viene ripristinato in un database o in un server diverso, l'errore indica che non è stato possibile rimuovere correttamente le impostazioni della replica (per impostazione predefinita, queste ultime vengono rimosse se il database o il server è diverso).  
  
 È probabile che l'errore sia il risultato di una mancata corrispondenza tra lo stato del database ripristinato e uno o più database di sistema contenenti metadati di replica, come il database **msdb**, **master** o il database di distribuzione.  
  
## <a name="user-action"></a>Azione dell'utente  
 Per risolvere il problema:  
  
1.  Eseguire l'istruzione ALTER DATABASE per portare il database online. Ad esempio: `ALTER DATABASE AdventureWorks SET ONLINE`. Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md). Per mantenere le impostazioni di replica, andare al passaggio 2. In caso contrario, andare al passaggio 3.  
  
2.  Eseguire [sp_restoredbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-restoredbreplication-transact-sql.md). Se l'esecuzione di questa stored procedure riesce, il ripristino sarà completo. In caso contrario, andare al passaggio 3.  
  
3.  Eseguire [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) per rimuovere tutte le impostazioni della replica.  
  
     Se necessario, riconfigurare la replica. Se sono stati creati gli script della topologia di replica, come consigliato, utilizzare tali script per riconfigurare la topologia.  
  
## <a name="see-also"></a>Vedere anche  
 [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Backup e ripristino di database replicati](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
