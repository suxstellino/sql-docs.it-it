---
description: sys.fn_get_audit_file (Transact-SQL)
title: sys.fn_get_audit_file (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_audit_file_TSQL
- sys.fn_get_audit_file_TSQL
- fn_get_audit_file
- sys.fn_get_audit_file
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_get_audit_file function
- fn_get_audit_file function
ms.assetid: d6a78d14-bb1f-4987-b7b6-579ddd4167f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 701740849d7b1ec8a946fa8f26bcd25568f2e62e
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812932"
---
# <a name="sysfn_get_audit_file-transact-sql"></a>sys.fn_get_audit_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]    

  Restituisce informazioni da un file di controllo creato da un controllo del server in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SQL Server Audit &#40;Motore di database&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
fn_get_audit_file ( file_pattern,   
    { default | initial_file_name | NULL },   
    { default | audit_record_offset | NULL } )  
```  
  
## <a name="arguments"></a>Argomenti  
 *file_pattern*  
 Specifica la directory o il percorso e il nome di file per il set di file di controllo da leggere. Il tipo è **nvarchar (260)**. 
 
 - **SQL Server**:
    
    Questo argomento deve includere sia un percorso (lettera di unità o condivisione di rete) che un nome di file, che può includere un carattere jolly. È possibile utilizzare un singolo asterisco (*) per raccogliere più file da un set di file di controllo. Ad esempio:  
  
    -   **\<path>\\\** _: Raccoglie tutti i file di controllo nel percorso specificato.  
  
    -   _* \<path> \LoginsAudit_{Guid} * * _-raccoglie tutti i file di controllo con il nome e la coppia GUID specificati.  
  
    -   _* \<path> \LoginsAudit_{GUID} _00_29384. sqlaudit * *-raccoglie un file di controllo specifico.  
  
 - **Database SQL di Azure**:
 
    Questo argomento viene usato per specificare un URL BLOB (incluso l'endpoint di archiviazione e il contenitore). Sebbene non supporti un carattere jolly asterisco, è possibile usare un prefisso di nome di file parziale (BLOB), anziché il nome del BLOB completo, per raccogliere più file (BLOB) che iniziano con questo prefisso. Ad esempio:
 
      - **\<Storage_endpoint\>/\<Container\>/\<ServerName\>/\<DatabaseName\>/** : raccoglie tutti i file di controllo (BLOB) per il database specifico.    
      
      - **\<Storage_endpoint\> / \<Container\> / \<ServerName\> / \<DatabaseName\> / \<AuditName\> / \<CreationDate\> / \<FileName\> . xel** : raccoglie un file di controllo specifico (BLOB).
  
> [!NOTE]  
>  Se si passa un percorso senza un criterio del nome di file, verrà generato un errore.  
  
 *initial_file_name*  
 Specifica il percorso e il nome di un file specifico del set di file di controllo da cui avviare la lettura dei record di controllo. Il tipo è **nvarchar (260)**.  
  
> [!NOTE]  
>  L'argomento *initial_file_name* deve contenere voci valide o deve contenere l'impostazione predefinita | Valore NULL.  
  
 *audit_record_offset*  
 Specifica un percorso noto con il file specificato per l'argomento initial_file_name. Quando viene utilizzato questo argomento, la funzione avvierà la lettura dal primo record del buffer immediatamente successivo all'offset specificato.  
  
> [!NOTE]  
>  L'argomento *audit_record_offset* deve contenere voci valide o deve contenere l'impostazione predefinita | Valore NULL. Il tipo è **bigint**.  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Nella tabella seguente viene descritto il contenuto del file di controllo che può essere restituito da questa funzione.  
  
| Nome colonna | Type | Descrizione |  
|-------------|------|-------------|  
| action_id | **varchar(4)** | ID dell'azione. Non ammette i valori NULL. |  
| additional_information | **nvarchar(4000)** | Le informazioni univoche applicabili solo a un singolo evento vengono restituite in formato XML. Questo tipo di informazioni è contenuto in un numero ridotto di azioni controllabili.<br /><br /> Un livello di stack TSQL sarà visualizzato in formato XML per le azioni associate a tale stack. Il formato XML sarà:<br /><br /> `<tsql_stack><frame nest_level = '%u' database_name = '%.*s' schema_name = '%.*s' object_name = '%.*s' /></tsql_stack>`<br /><br /> Frame nest_level indica il livello di nidificazione corrente del frame. Il nome del modulo viene rappresentato in un formato composto da tre parti (nome_database, nome_schema e nome_oggetto)  Il nome del modulo verrà analizzato in modo da evitare caratteri XML non validi, ad esempio `'\<'` ,, `'>'` `'/'` , `'_x'` . Verranno sottoposte a escape come `_xHHHH\_` . dove HHHH rappresenta il codice UCS-2 esadecimale a quattro cifre per il carattere.<br /><br /> Ammette i valori Null. Restituisce NULL quando non sono presenti informazioni aggiuntive segnalate dall'evento. |
| affected_rows | **bigint** | **Si applica a**: solo database SQL di Azure<br /><br /> Numero di righe interessate dall'istruzione eseguita. |  
| application_name | **nvarchar(128)** | **Si applica a**: database SQL di Azure + SQL Server (a partire da 2017)<br /><br /> Nome dell'applicazione client che ha eseguito l'istruzione che ha causato l'evento di controllo |  
| audit_file_offset | **bigint** | **Si applica a**: solo SQL Server<br /><br /> Offset del buffer nel file che contiene il record di controllo. Non ammette i valori Null. |  
| audit_schema_version | **int** | Sempre 1 |  
| class_type | **varchar(2)** | Tipo di entità controllabile in cui si verifica il controllo. Non ammette i valori Null. |  
| client_ip | **nvarchar(128)** | **Si applica a**: database SQL di Azure + SQL Server (a partire da 2017)<br /><br />  IP di origine dell'applicazione client |  
| connection_id | GUID | **Si applica a**: database SQL di Azure e SQL istanza gestita<br /><br /> ID della connessione nel server |
| data_sensitivity_information | nvarchar(4000) | **Si applica a**: solo database SQL di Azure<br /><br /> Tipi di informazioni e etichette di riservatezza restituite dalla query controllata, basate sulle colonne classificate nel database. Altre informazioni sull' [individuazione e la classificazione dei dati del database SQL di Azure](/azure/sql-database/sql-database-data-discovery-and-classification) |
| database_name | **sysname** | Contesto del database in cui si è verificata l'azione. Ammette i valori Null. Restituisce NULL per i controlli che si verificano a livello di server. |  
| database_principal_id | **int** |ID del contesto dell'utente del database in cui viene eseguita l'azione. Non ammette i valori Null. Se non applicabile, ad esempio nel caso di un'operazione server, restituisce 0.|
| database_principal_name | **sysname** | Utente corrente. Ammette i valori Null. Se non disponibile, restituisce NULL. |  
| duration_milliseconds | **bigint** | **Si applica a**: database SQL di Azure e SQL istanza gestita<br /><br /> Durata esecuzione query in millisecondi |
| event_time | **datetime2** | Data e ora di attivazione dell'azione controllabile. Non ammette i valori Null. |  
| file_name | **varchar(260)** | Percorso e nome del file di log di controllo da cui proviene il record. Non ammette i valori Null. |
| is_column_permission | **bit** | Flag che indica se si tratta di un'autorizzazione a livello di colonna. Non ammette i valori Null. Restituisce 0 quando permission_bitmask = 0.<br /> 1 = True<br /> 0 = False |
| object_id | **int** | ID dell'entità in cui si è verificato il controllo. Il comportamento predefinito include quanto segue:<br /> Oggetti server<br /> Database<br /> Oggetti di database<br /> Oggetti dello schema<br /> Non ammette i valori Null. Restituisce 0 se l'entità è il server stesso o se il controllo non viene eseguito a livello di oggetto, ad esempio nel caso dell'autenticazione. |  
| object_name | **sysname** | Nome dell'entità in cui si è verificato il controllo. Il comportamento predefinito include quanto segue:<br /> Oggetti server<br /> Database<br /> Oggetti di database<br /> Oggetti dello schema<br /> Ammette i valori Null. Restituisce NULL se l'entità è il server stesso o se il controllo non viene eseguito a livello di oggetto, ad esempio nel caso dell'autenticazione. |
| permission_bitmask | **varbinary(16)** | In alcune azioni, si tratta delle autorizzazioni concesse, negate o revocate. |
| response_rows | **bigint** | **Si applica a**: database SQL di Azure e SQL istanza gestita<br /><br /> Numero di righe restituite nel set di risultati. |  
| schema_name | **sysname** | Contesto dello schema in cui si è verificata l'azione. Ammette i valori Null. Restituisce NULL per i controlli che si verificano all'esterno di uno schema. |  
| sequence_group_id | **varbinary** | **Si applica a**: solo SQL Server (a partire da 2016)<br /><br />  Identificatore univoco |  
| sequence_number | **int** | Viene tenuta traccia della sequenza dei record all'interno di un singolo record di controllo con dimensioni troppo elevate per il buffer di scrittura dei controlli. Non ammette i valori Null. |  
| server_instance_name | **sysname** | Nome dell'istanza del server in cui si è verificato il controllo. Viene utilizzato il formato server\istanza standard. |  
| server_principal_id | **int** | ID del contesto dell'account di accesso utilizzato per eseguire l'azione. Non ammette i valori Null. |  
| server_principal_name | **sysname** | Account di accesso corrente. Ammette i valori Null. |  
| server_principal_sid | **varbinary** | SID dell'account di accesso corrente. Ammette i valori Null. |  
| session_id | **smallint** | ID della sessione in cui si è verificato l'evento. Non ammette i valori Null. |  
| session_server_principal_name | **sysname** | Entità server per la sessione. Ammette i valori Null. |  
| istruzione | **nvarchar(4000)** | Istruzione TSQL, se esiste. Ammette i valori Null. Se non applicabile, viene restituito NULL. |  
| riuscito | **bit** | Indica se l'azione che ha generato l'evento è riuscita. Non ammette i valori Null. Per tutti gli eventi diversi dagli eventi di accesso, riporta solo l'esito del controllo dell'autorizzazione, non l'operazione.<br /> 1 = esito positivo<br /> 0 = esito negativo |
| target_database_principal_id | **int** | Entità database su cui viene eseguita un'operazione GRANT/DENY/REVOKE. Non ammette i valori Null. Se non applicabile, restituisce 0. |  
| target_database_principal_name | **sysname** | Utente di destinazione dell'azione. Ammette i valori Null. Se non applicabile, viene restituito NULL. |  
| target_server_principal_id | **int** | Entità server su cui viene eseguita un'operazione GRANT/DENY/REVOKE. Non ammette i valori Null. Se non applicabile, restituisce 0. |  
| target_server_principal_name | **sysname** | Account di accesso di destinazione dell'azione. Ammette i valori Null. Se non applicabile, viene restituito NULL. |  
| target_server_principal_sid | **varbinary** | SID dell'account di accesso di destinazione. Ammette i valori Null. Se non applicabile, viene restituito NULL. |  
| transaction_id | **bigint** | **Si applica a**: solo SQL Server (a partire da 2016)<br /><br /> Identificatore univoco per identificare più eventi di controllo in una transazione |  
| user_defined_event_id | **smallint** | **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, database SQL di Azure e SQL istanza gestita<br /><br /> ID evento definito dall'utente passato come argomento per **sp_audit_write**. **Null** per gli eventi di sistema (impostazione predefinita) e diverso da zero per l'evento definito dall'utente. Per ulteriori informazioni, vedere [sp_audit_write &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md). |  
| user_defined_information | **nvarchar(4000)** | **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, database SQL di Azure e SQL istanza gestita<br /><br /> Utilizzato per registrare eventuali informazioni aggiuntive che l'utente desidera registrare nel log di controllo utilizzando il **sp_audit_write** stored procedure. |  

  
## <a name="remarks"></a>Commenti  
- Se l'argomento *file_pattern* passato a **fn_get_audit_file** fa riferimento a un percorso o a un file che non esiste o se il file non è un file di controllo, viene restituito il messaggio di errore **MSG_INVALID_AUDIT_FILE** .  
- non è possibile usare **fn_get_audit_file** quando il controllo viene creato con le opzioni **APPLICATION_LOG**, **SECURITY_LOG** o **EXTERNAL_MONITOR** .

## <a name="permissions"></a>Autorizzazioni

- **SQL Server**: è richiesta l'autorizzazione **Control Server** .  
- **Database SQL di Azure**: è richiesta l'autorizzazione **Control database** .     
  - Gli amministratori del server possono accedere ai log di controllo di tutti i database nel server.
  - Gli amministratori non server possono accedere ai log di controllo solo dal database corrente.
  - I BLOB che non soddisfano i criteri indicati sopra verranno ignorati (un elenco di BLOB ignorati verrà visualizzato nel messaggio di output della query) e la funzione restituirà i log solo dai BLOB per cui è consentito l'accesso.  
  
## <a name="examples"></a>Esempio

- **SQL Server**

  In questo esempio viene eseguita la lettura da un file denominato `\\serverName\Audit\HIPAA_AUDIT.sqlaudit`.  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('\\serverName\Audit\HIPAA_AUDIT.sqlaudit',default,default);  
  GO  
  ```  

- **Database SQL di Azure**

  In questo esempio viene letto da un file denominato `ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel` :  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default);
  GO  
  ```  

  In questo esempio viene letto dallo stesso file precedente, ma con clausole T-SQL aggiuntive (**Top**, **Order by** e clausola **where** per filtrare i record di controllo restituiti dalla funzione):
  
  ```  
  SELECT TOP 10 * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default)
  WHERE server_principal_name = 'admin1'
  ORDER BY event_time
  GO
  ```  

  Questo esempio legge tutti i log di controllo dai server che iniziano con `Sh` : 
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/Sh',default,default);
  GO  
  ```

Per un esempio completo delle modalità di creazione di un controllo, vedere [SQL Server Audit &#40;motore di database&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).

Per informazioni sulla configurazione del controllo del database SQL di Azure, vedere [Introduzione al controllo del database SQL](/azure/sql-database/sql-database-auditing).
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Creazione di un controllo del server e di una specifica del controllo del server](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
