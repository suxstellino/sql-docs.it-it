---
description: Eventi estesi per Stretch Database
title: Eventi estesi per Stretch Database
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 70485e74-2e25-4e7e-be6c-9dd1780a42e3
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f965b68279398720ff7b1b9bcb76b5ec73410466
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080072"
---
# <a name="extended-events-for-stretch-database"></a>Eventi estesi per Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Stretch Database offre un set di eventi estesi per la risoluzione dei problemi.  
  
Per altre informazioni, vedere [Eventi estesi](../../relational-databases/extended-events/extended-events.md). Per informazioni su come avviare una sessione di eventi estesi per la risoluzione dei problemi, vedere [Creare una sessione Eventi estesi](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))  
  
## <a name="list-of-extended-events-for-stretch-database"></a>Elenco di eventi estesi per Estensione database  
  
Nome evento|Descrizione evento   
---------|---------  
remote_data_archive_db_ddl|Si verifica quando viene elaborata l'istruzione DDL T-SQL del database per l'estensione dei dati.  
remote_data_archive_provision_operation|Si verifica all'inizio o alla fine di un'operazione di provisioning.  
remote_data_archive_query_rewrite|Si verifica quando l'oggetto RelOp_Get viene sostituito durante la riscrittura della query per l'estensione.  
remote_data_archive_table_ddl|Si verifica quando viene elaborata l'istruzione DDL T-SQL della tabella per l'estensione dei dati.  
remote_data_archive_telemetry|Si verifica quando un sistema locale trasmette un evento di telemetria al database di Azure.  
remote_data_archive_telemetry_rejected|Si verifica ogni volta che viene rifiutato un evento di telemetria esteso nel database di Azure  
repopulate_stretch_schema_task_queue_complete|Segnala il completamento del ripopolamento della coda di attività di estensione schema.  
repopulate_stretch_schema_task_queue_start|Segnala l'avvio del ripopolamento della coda di attività di estensione schema.  
stretch_codegen_errorlog|Segnala l'output del generatore di codice  
stretch_codegen_start|Segnala l'avvio della generazione del codice di estensione  
stretch_create_remote_table_start|Segnala l'avvio della creazione di una tabella remota  
stretch_database_disable_completed|Segnala il completamento di un comando ALTER DATABASE SET REMOTE_DATA_ARCHIVE OFF  
stretch_database_enable_completed|Segnala il completamento di un comando ALTER DATABASE SET REMOTE_DATA_ARCHIVE ON  
stretch_database_reauthorize_completed|Segnala il completamento di un processo di specifica sp_rda_reauthorize_db  
stretch_index_reconciliation_codegen_completed|Segnala il completamento della generazione del codice per un'operazione di estensione indice remoto  
stretch_index_update_step_completed|Segnala la durata dell'operazione di aggiornamento di un indice esteso  
stretch_migration_debug_trace|Traccia di debug delle azioni di migrazione dell'estensione.  
stretch_migration_dequeue_migration|Evento generato quando un'attività di migrazione dell'estensione viene rimossa dalla coda per un database.  
stretch_migration_queue_migration|Accoda un pacchetto per avviare la migrazione del database e dell'oggetto.  
stretch_migration_requeue_migration|Evento generato quando un pacchetto di attività di migrazione dell'estensione viene reinserito nella coda.  
stretch_migration_start_migration|Avvia la migrazione del database e dell'oggetto.  
stretch_migration_start_unmigration|Avvia l'annullamento della migrazione del database e dell'oggetto.  
stretch_remote_column_execution_completed|Segnala il completamento dell'esecuzione remota per il codice generato per una colonna estesa  
stretch_remote_column_reconciliation_codegen_completed|Segnala il completamento della generazione del codice per la riconciliazione delle colonne remote con estensione  
stretch_remote_index_execution_completed|Segnala il completamento dell'esecuzione remota per il codice generato per un indice esteso  
stretch_schema_queue_task|Segnala quando un pacchetto sta per essere accodato per l'elaborazione di un'attività di schema per il database e l'oggetto.  
stretch_schema_script_execution_completed|Segnala il completamento dell'esecuzione di uno script di estensione durante l'elaborazione dell'attività di estensione schema.  
stretch_schema_script_execution_skipped|Segnala quando viene ignorata l'esecuzione di uno script di estensione durante l'elaborazione dell'attività di estensione schema.  
stretch_schema_script_execution_start|Segnala l'avvio dell'esecuzione di uno script di estensione durante l'elaborazione dell'attività di estensione schema.  
stretch_schema_task_failed|Segnala l'errore di una funzione di estensione schema durante l'attività di estensione schema.  
stretch_schema_task_skipped|Segnala l'attività di estensione schema ignorata durante la funzione di estensione schema.  
stretch_schema_task_start|Segnala l'avvio di una funzione di estensione schema durante l'attività di estensione schema.  
stretch_schema_task_succeeded|Segnala il completamento di una funzione di estensione schema durante l'attività di estensione schema.  
stretch_sp_migration_get_batch_id|Chiama sp_stretch_get_batch_id  
stretch_sync_metadata_start|Segnala l'avvio dei controlli dei metadati durante l'attività di migrazione.  
stretch_table_codegen_completed|Segnala il completamento della generazione del codice per una tabella estesa  
stretch_table_complete_data_reconciliation|Completa la riconciliazione dei dati del database e dell'oggetto.  
stretch_table_data_reconciliation_event|Segnala il completamento della riconciliazione dei dati di un batch di righe  
stretch_table_data_reconciliation_results_event|Segnala un errore o il completamento della riconciliazione dei dati riuscita di un numero di batch di righe  
stretch_table_hinted_admin_delete_event|Segnala l'esecuzione di un'operazione DML di eliminazione dell'estensione che usa un hint admin  
stretch_table_hinted_admin_update_event|Segnala l'esecuzione di un'operazione DML di aggiornamento dell'estensione che usa un hint admin  
stretch_table_provisioning_step_completed|Segnala la durata dell'operazione di provisioning di una tabella estesa  
stretch_table_query_error|Segnala un errore generato durante la riscrittura della query per l'estensione  
stretch_table_remote_creation_completed|Segnala il completamento dell'esecuzione remota per il codice generato per una tabella estesa  
stretch_table_row_migration_event|Segnala il completamento della migrazione di un batch di righe  
stretch_table_row_migration_results_event|Segnala un errore o il completamento della migrazione riuscita di un numero di batch di righe  
stretch_table_row_unmigration_event|Segnala il completamento dell'annullamento della migrazione di un batch di righe  
stretch_table_row_unmigration_results_event|Segnala un errore o il completamento dell'annullamento della migrazione riuscito di un numero di batch di righe  
stretch_table_start_data_reconciliation|Avvia la riconciliazione dei dati del database e dell'oggetto.  
stretch_table_unprovision_completed|Segnala il completamento della rimozione delle risorse locali per una tabella di cui è stata annullata l'estensione  
stretch_table_validation_error|Segnala il completamento della convalida per una tabella quando l'utente abilita l'estensione  
stretch_unprovision_table_start|Segnala l'avvio dell'annullamento del provisioning di una tabella estesa  
  
## <a name="see-also"></a>Vedere anche  
[Gestire e risolvere i problemi di Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)