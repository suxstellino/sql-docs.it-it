---
title: Guida al processo di pulizia fantasma | Microsoft Docs
description: Informazioni sul processo di pulizia fantasma, un processo in background che elimina record da pagine contrassegnate per l'eliminazione in SQL Server.
ms.custom: ''
ms.date: 05/02/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- ghost cleanup
- ghost records
- ghost clean up process
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05113205c693de928321880f1586996f81b76a66
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750361"
---
# <a name="ghost-cleanup-process-guide"></a>Guida al processo di pulizia fantasma

Il processo di pulizia fantasma è un processo in background a thread singolo che elimina record da pagine contrassegnate per l'eliminazione. L'articolo seguente include una panoramica di questo processo.

## <a name="ghost-records"></a>Record fantasma

I record eliminati da un livello foglia di una pagina di indice non sono rimossi fisicamente dalla pagina, ma vengono contrassegnati come "record da eliminare" o record *fantasma*. La riga rimane nella pagina, ma l'intestazione è leggermente modificata, a indicare che si tratta di una riga fantasma. Questa impostazione ha lo scopo di ottimizzare le prestazioni durante un'operazione di eliminazione. Gli elementi fantasma sono necessari per il blocco a livello di riga, ma anche per l'isolamento di snapshot in cui è necessario gestire le versioni precedenti delle righe.

## <a name="ghost-record-cleanup-task"></a>Attività di pulizia di record fantasma

I record contrassegnati per l'eliminazione o *record fantasma* vengono rimossi tramite il processo di pulizia fantasma in background. Questo processo in background viene eseguito dopo il commit dell'operazione di eliminazione e rimuove fisicamente i record fantasma dalle pagine. Il processo di pulizia fantasma viene eseguito automaticamente a un intervallo determinato (ogni 5 secondi per SQL Server 2012 e versioni successive, ogni 10 secondi per SQL Server 2008/2008R2) e verifica se sono presenti pagine contrassegnate con record fantasma. Se trova tali pagine, elimina i record contrassegnati per l'eliminazione o *record fantasma*, elaborando un massimo di 10 pagine per esecuzione.

Quando un record è un record fantasma, il database è contrassegnato come provvisto di voci fantasma. Il processo di pulizia fantasma analizza solo tali database. Dopo che tutti i record fantasma sono stati eliminati il processo di pulizia fantasma contrassegna il database come "privo di record fantasma" e lo ignora all'esecuzione successiva. Il processo ignora anche tutti i database ai quali non è in grado di applicare un blocco condiviso e ne ritenta l'analisi all'esecuzione successiva.

La query seguente identifica il numero di record fantasma esistenti in un singolo database. 

 ```sql
 SELECT sum(ghost_record_count) total_ghost_records, db_name(database_id) 
 FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'SAMPLED')
 group by database_id
 order by total_ghost_records desc
```

## <a name="disable-the-ghost-cleanup"></a>Disabilitare la pulizia fantasma

Nei sistemi con carichi di lavoro elevati e molte operazioni di eliminazione il processo di pulizia fantasma può causare un problema di prestazioni, originato dal mantenimento delle pagine nel pool di buffer e dalle operazioni I/O generate. È possibile disabilitare il processo usando il [flag di traccia 661](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Tuttavia la disabilitazione del processo presenta implicazioni a livello di prestazioni.

La disabilitazione del processo di pulizia fantasma può causare una crescita eccessiva e non necessaria del database e di conseguenza originare problemi di prestazioni. Dato che il processo di pulizia fantasma rimuove i record contrassegnati come elementi fantasma, quando si disabilita il processo questi record restano nella pagina e impediscono a SQL Server di riusare questo spazio. SQL Server deve aggiungere dati a nuove pagine, dando origine a file di database troppo grandi ed eventualmente a [divisioni di pagina](indexes/specify-fill-factor-for-an-index.md). Le divisioni di pagina causano problemi di prestazioni durante la creazione dei piani di esecuzione e quando si eseguono operazioni di analisi. 

Dopo aver disabilitato il processo di pulizia fantasma, è necessario adottare misure alternative per rimuovere i record fantasma. Un'opzione possibile è la ricompilazione dell'indice, che ridistribuisce i dati nelle pagine. Un'altra opzione è l'esecuzione manuale di [sp_clean_db_free_space](system-stored-procedures/sp-clean-db-free-space-transact-sql.md) (per pulire tutti i file di dati del database) o [sp_clean_db_file_free_space](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) (per pulire un singolo file di dati del database): queste operazioni eliminano i record fantasma.

 >[!warning]
 > La disabilitazione del processo di pulizia fantasma è in genere sconsigliata. Questa operazione va testata accuratamente in un ambiente controllato prima di implementarla in modo permanente in un ambiente di produzione.


## <a name="next-steps"></a>Passaggi successivi  
[Disabilitazione del processo di pulizia fantasma](https://support.microsoft.com/help/920093/tuning-options-for-sql-server-when-running-in-high-performance-workloa)
<br>[Rimuovere i record fantasma da un singolo file di database](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
<br>[Rimuovere i record fantasma da tutti i file di dati del database](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)


