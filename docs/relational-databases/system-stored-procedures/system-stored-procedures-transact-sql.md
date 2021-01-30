---
description: Stored procedure di sistema (Transact-SQL)
title: Stored procedure di sistema (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sql13.TSQLSysNoExpandPortal.f1
- sql13.TSQLSysNoExpandPortal.f1_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server]
- APIs [SQL Server]
- stored procedures [SQL Server], categories
- system stored procedures [SQL Server], categories
- system stored procedures [SQL Server]
ms.assetid: a5c4d5b8-5a24-4a2d-99b4-d003b546ee3a
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 603a16d94c185783b3b90566d71b90e195949254
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203652"
---
# <a name="system-stored-procedures-transact-sql"></a>Stored procedure di sistema (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] molte attività amministrative e informative possono essere eseguite mediante le stored procedure di sistema. Le stored procedure di sistema sono raggruppate in categorie, descritte nella tabella seguente.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Category|Descrizione|  
|--------------|-----------------|  
|[Stored procedure Geo-Replication attive]()|Usato per gestire le configurazioni di Geo-Replication attive nel database SQL di Azure|  
|[Stored procedure per il catalogo](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)|Consentono di implementare le funzioni del dizionario dei dati ODBC e di isolare le applicazioni ODBC in caso di modifiche alle tabelle di sistema sottostanti.|  
|[Stored procedure per Change Data Capture](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)|Consentono di abilitare, disabilitare o creare report relativi a oggetti Change Data Capture.|  
|[Stored procedure per cursori](../../relational-databases/system-stored-procedures/cursor-stored-procedures-transact-sql.md)|Consentono di implementare la funzionalità per le variabili di cursore.|  
|[Stored procedure dell'agente di raccolta dati](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)|Consentono di lavorare con l'agente di raccolta dati e i componenti seguenti: set di raccolta, elementi delle raccolte e tipi di raccolta.|  
|[Stored procedure per il motore di database](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)|Consentono di eseguire le operazioni di manutenzione generale del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)|Consentono di eseguire le operazioni di posta elettronica da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Stored procedure per i piani di manutenzione del database](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)|Consentono di impostare le attività di manutenzione principali necessarie per gestire le prestazioni dei database.|  
|[Stored procedure per le query distribuite](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)|Consentono di implementare e gestire le query distribuite.|  
|[Stored procedure FILESTREAM e FileTable &#40;&#41;Transact-SQL ](./filestream-and-filetable-sp-filestream-force-garbage-collection.md)|Utilizzato per configurare e gestire le caratteristiche FILESTREAM e FileTable.|  
|[Stored procedure per le regole del firewall &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/firewall-rules-stored-procedures-azure-sql-database.md)|Usato per configurare il firewall del database SQL di Azure.|  
|[Stored procedure per ricerche full-text](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)|Consentono di implementare ed eseguire query su indici full-text.|  
|[Stored procedure estese generali](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)|Consentono di utilizzare un'interfaccia tra un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e programmi esterni per varie attività di manutenzione.|  
|[Stored procedure per il log shipping](../../relational-databases/system-stored-procedures/log-shipping-stored-procedures-transact-sql.md)|Consentono di configurare, modificare e monitorare le configurazioni per il log shipping.|  
|[Stored procedure del data warehouse di gestione &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/management-data-warehouse-stored-procedures-transact-sql.md)|Utilizzato per configurare il data warehouse di gestione.|  
|[Stored procedure di automazione OLE](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)|Consentono di abilitare gli oggetti di automazione standard per l'utilizzo all'interno di un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] standard.|  
|[Stored procedure per la gestione basata su criteri](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)|Consentono di implementare la gestione basata su criteri.|  
|[Stored procedure di PolyBase](./polybase-stored-procedures-sp-polybase-join-group.md)|Aggiungere o rimuovere un computer da un gruppo con scalabilità orizzontale di base.|  
|[Stored procedure di Query Store &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)|Utilizzato per ottimizzare le prestazioni.|  
|[Stored procedure per la replica](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)|Consentono di gestire le operazioni di replica.|  
|[Stored procedure per la sicurezza](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)|Consentono di gestire la sicurezza.|  
|[Stored procedure di backup di snapshot](./snapshot-backup-sp-delete-backup.md)|Utilizzato per eliminare il backup FILE_SNAPSHOT insieme a tutti i relativi snapshot o per eliminare un singolo snapshot del file di backup.|  
|[Stored procedure relative agli indici spaziali](./spatial-index-stored-procedures-arguments-and-properties.md)|Consentono di analizzare e migliorare le prestazioni di esecuzione dell'indicizzazione degli indici spaziali.|  
|[Stored procedure per SQL Server Agent](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)|Consentono a [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] di monitorare prestazione e attività.|  
|[Stored procedure di SQL Server Profiler](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|Consentono a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di gestire attività pianificate e guidate dagli eventi.|  
|[Stored procedure Stretch Database](../../relational-databases/system-stored-procedures/stretch-database-extended-stored-procedures-transact-sql.md)|Usato per gestire estensione database.|  
|[Stored procedure per tabelle temporali](./spatial-index-stored-procedures-arguments-and-properties.md)|Usare per le tabelle temporali|  
|[Stored procedure per XML](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)|Consentono di gestire il testo XML.|  
  
> [!NOTE]  
>  Se non specificato diversamente, tutte le stored procedure di sistema restituiscono il valore 0 per indicare esito positivo. Per indicare esito negativo, viene restituito un valore diverso da zero.  
  
## <a name="api-system-stored-procedures"></a>Stored procedure di sistema (API)  
 Gli utenti che eseguono [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per applicazioni ADO, OLE DB e ODBC possono notare che queste applicazioni utilizzano stored procedure di sistema che non vengono trattate nella Guida di riferimento a [!INCLUDE[tsql](../../includes/tsql-md.md)]. Queste stored procedure vengono utilizzate dal [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider OLE DB di Native client e dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client per implementare la funzionalità di un'API di database. Rappresentano il meccanismo con cui il provider o il driver comunica le richieste degli utenti a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Devono essere utilizzate solo internamente dal provider o dal driver. La chiamata in modo esplicito da un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applicazione basata su non è supportata.  
  
 Le stored procedure sp_createorphan e sp_droporphans vengono utilizzate per l'elaborazione di tipo **ntext**, **Text** e **Image** di ODBC.  
  
 La stored procedure sp_reset_connection viene utilizzata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il supporto di chiamate di stored procedure remote in una transazione. Questa stored procedure causa inoltre la generazione degli eventi Audit Login e Audit Logout quando una connessione viene riutilizzata da un pool di connessioni.  
  
 Le stored procedure di sistema riportate nelle tabelle seguenti vengono utilizzate solo in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o tramite API client e non devono essere utilizzate dagli utenti finali. Sono soggette a modifiche e la compatibilità non è garantita.  
  
 Le stored procedure seguenti sono documentate nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
:::row:::
    :::column:::
        sp_catalogs
    :::column-end:::
    :::column:::
        sp_column_privileges
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_ex
    :::column-end:::
    :::column:::
        sp_columns
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_ex
    :::column-end:::
    :::column:::
        sp_databases
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursor
    :::column-end:::
    :::column:::
        sp_cursorclose
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorexecute
    :::column-end:::
    :::column:::
        sp_cursorfetch
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursoroption
    :::column-end:::
    :::column:::
        sp_cursoropen
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorprepare
    :::column-end:::
    :::column:::
        sp_cursorprepexec
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorunprepare
    :::column-end:::
    :::column:::
        sp_execute
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_datatype_info
    :::column-end:::
    :::column:::
        sp_fkeys
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreignkeys
    :::column-end:::
    :::column:::
        sp_indexes
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_pkeys
    :::column-end:::
    :::column:::
        sp_primarykeys
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_prepare
    :::column-end:::
    :::column:::
        sp_prepexec
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_prepexecrpc
    :::column-end:::
    :::column:::
        sp_unprepare
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_server_info
    :::column-end:::
    :::column:::
        sp_special_columns
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_sproc_columns
    :::column-end:::
    :::column:::
        sp_statistics
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges
    :::column-end:::
    :::column:::
        sp_table_privileges_ex
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables
    :::column-end:::
    :::column:::
        sp_tables_ex
    :::column-end:::
:::row-end:::

&nbsp;
  
Le stored procedure seguenti non sono documentate:  

:::row:::
    :::column:::
        sp_assemblies_rowset
    :::column-end:::
    :::column:::
        sp_assemblies_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_assemblies_rowset2
    :::column-end:::
    :::column:::
        sp_assembly_dependencies_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_assembly_dependencies_rowset_rmt
    :::column-end:::
    :::column:::
        sp_assembly_dependencies_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_bcp_dbcmptlevel
    :::column-end:::
    :::column:::
        sp_catalogs_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_catalogs_rowset;2
    :::column-end:::
    :::column:::
        sp_catalogs_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_catalogs_rowset_rmt
    :::column-end:::
    :::column:::
        sp_catalogs_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constbytable_rowset
    :::column-end:::
    :::column:::
        sp_check_constbytable_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constbytable_rowset2
    :::column-end:::
    :::column:::
        sp_check_constraints_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constraints_rowset;2
    :::column-end:::
    :::column:::
        sp_check_constraints_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset
    :::column-end:::
    :::column:::
        sp_column_privileges_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset;5
    :::column-end:::
    :::column:::
        sp_column_privileges_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset2
    :::column-end:::
    :::column:::
        sp_columns_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_90_rowset
    :::column-end:::
    :::column:::
        sp_columns_90_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_90_rowset2
    :::column-end:::
    :::column:::
        sp_columns_ex_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset
    :::column-end:::
    :::column:::
        sp_columns_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset;5
    :::column-end:::
    :::column:::
        sp_columns_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset2
    :::column-end:::
    :::column:::
        sp_constr_col_usage_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_datatype_info_90
    :::column-end:::
    :::column:::
        sp_ddopen;1
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;10
    :::column-end:::
    :::column:::
        sp_ddopen;11
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;12
    :::column-end:::
    :::column:::
        sp_ddopen;13
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;2
    :::column-end:::
    :::column:::
        sp_ddopen;3
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;4
    :::column-end:::
    :::column:::
        sp_ddopen;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;6
    :::column-end:::
    :::column:::
        sp_ddopen;7
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;8
    :::column-end:::
    :::column:::
        sp_ddopen;9
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset;3
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset_rmt
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset3
    :::column-end:::
    :::column:::
        sp_indexes_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_90_rowset_rmt
    :::column-end:::
    :::column:::
        sp_indexes_90_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset
    :::column-end:::
    :::column:::
        sp_indexes_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset;5
    :::column-end:::
    :::column:::
        sp_indexes_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset2
    :::column-end:::
    :::column:::
        sp_linkedservers_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_linkedservers_rowset;2
    :::column-end:::
    :::column:::
        sp_linkedservers_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_database
    :::column-end:::
    :::column:::
        sp_oledb_defdb
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_deflang
    :::column-end:::
    :::column:::
        sp_oledb_language
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_ro_usrname
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset;2
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset;3
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset;5
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset2
    :::column-end:::
    :::column:::
        sp_procedure_params_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedure_params_90_rowset2
    :::column-end:::
    :::column:::
        sp_procedure_params_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedure_params_rowset;2
    :::column-end:::
    :::column:::
        sp_procedure_params_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedures_rowset
    :::column-end:::
    :::column:::
        sp_procedures_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedures_rowset2
    :::column-end:::
    :::column:::
        sp_provider_types_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_provider_types_rowset
    :::column-end:::
    :::column:::
        sp_schemata_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_schemata_rowset;3
    :::column-end:::
    :::column:::
        sp_special_columns_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_sproc_columns_90
    :::column-end:::
    :::column:::
        sp_statistics_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_statistics_rowset;2
    :::column-end:::
    :::column:::
        sp_statistics_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_stored_procedures
    :::column-end:::
    :::column:::
        sp_table_constraints_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_constraints_rowset;2
    :::column-end:::
    :::column:::
        sp_table_constraints_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset
    :::column-end:::
    :::column:::
        sp_table_privileges_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset;5
    :::column-end:::
    :::column:::
        sp_table_privileges_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset2
    :::column-end:::
    :::column:::
        sp_table_statistics_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_statistics_rowset;2
    :::column-end:::
    :::column:::
        sp_table_statistics2_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tablecollations
    :::column-end:::
    :::column:::
        sp_tablecollations_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_90_rowset
    :::column-end:::
    :::column:::
        sp_tables_info_90_rowset_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_90_rowset2
    :::column-end:::
    :::column:::
        sp_tables_info_90_rowset2_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset
    :::column-end:::
    :::column:::
        sp_tables_info_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset_64
    :::column-end:::
    :::column:::
        sp_tables_info_rowset_64;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset2
    :::column-end:::
    :::column:::
        sp_tables_info_rowset2_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_rowset;2
    :::column-end:::
    :::column:::
        sp_tables_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_rowset_rmt
    :::column-end:::
    :::column:::
        sp_tables_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_usertypes_rowset
    :::column-end:::
    :::column:::
        sp_usertypes_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_usertypes_rowset2
    :::column-end:::
    :::column:::
        sp_views_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_views_rowset2
    :::column-end:::
    :::column:::
        sp_xml_schema_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_xml_schema_rowset2
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  

## <a name="see-also"></a>Vedere anche  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Stored procedure &#40;Motore di database&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [Esecuzione di stored procedure &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/stored-procedures-running.md)   
 [Esecuzione di stored procedure](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Esecuzione delle stored procedure](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
