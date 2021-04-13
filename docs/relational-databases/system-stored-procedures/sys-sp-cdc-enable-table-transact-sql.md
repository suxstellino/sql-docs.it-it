---
description: sys.sp_cdc_enable_table (Transact-SQL)
title: sys.sp_cdc_enable_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_enable_table_TSQL
- sp_cdc_enable_table_TSQL
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], enabling tables
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
ms.assetid: 26150c09-2dca-46ad-bb01-3cb3165bcc5d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 608b2ec17719ac6c22ab290daff84134b914fc42
ms.sourcegitcommit: 2811226b366781489a34450ab62d58f7a523a2bb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2021
ms.locfileid: "107265375"
---
# <a name="syssp_cdc_enable_table-transact-sql"></a>sys.sp_cdc_enable_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Abilita l'acquisizione dei dati delle modifiche per la tabella di origine specificata nel database corrente. Quando una tabella viene abilitata per Change Data Capture, un record di ogni operazione DML (Data Manipulation Language) applicata alla tabella viene scritto nel log delle transazioni. Il processo Change Data Capture recupera queste informazioni dal log e le scrive nelle tabelle delle modifiche accessibili mediante un set di funzioni.  
  
 Change Data Capture non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_enable_table   
  [ @source_schema = ] 'source_schema',   
  [ @source_name = ] 'source_name' ,  [,[ @capture_instance = ] 'capture_instance' ]  
  [,[ @supports_net_changes = ] supports_net_changes ]  
  , [ @role_name = ] 'role_name'  
  [,[ @index_name = ] 'index_name' ]  
  [,[ @captured_column_list = ] 'captured_column_list' ]  
  [,[ @filegroup_name = ] 'filegroup_name' ]  
  [,[ @allow_partition_switch = ] 'allow_partition_switch' ]  
  [;]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @source_schema = ] 'source_schema'` Nome dello schema a cui appartiene la tabella di origine. *source_schema* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @source_name = ] 'source_name'` Nome della tabella di origine in cui abilitare Change Data Capture. *source_name* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
 *source_name* deve esistere nel database corrente. Le tabelle nello schema **CDC** non possono essere abilitate per Change Data Capture.  
  
`[ @role_name = ] 'role_name'` Nome del ruolo del database utilizzato per controllare l'accesso ai dati delle modifiche. *role_name* è di **tipo sysname** e deve essere specificato. Se viene impostato in modo esplicito su NULL, non viene utilizzato alcun ruolo di controllo per limitare l'accesso ai dati delle modifiche.  
  
 Il ruolo viene utilizzato se esiste. Se il ruolo non esiste, viene eseguito un tentativo per creare un ruolo del database con il nome specificato. Nel nome del ruolo vengono eliminati gli spazi vuoti nella parte destra della stringa prima di provare a creare il ruolo. Se il chiamante non è autorizzato a creare un ruolo all'interno del database, non è possibile eseguire l'operazione della stored procedure.  
  
`[ @capture_instance = ] 'capture_instance'` Nome dell'istanza di acquisizione utilizzata per denominare oggetti Change Data Capture specifici dell'istanza. *capture_instance* è di **tipo sysname** e non può essere null.  
  
 Se non specificato, il nome deriva dal nome dello schema di origine più il nome della tabella di origine nel formato *schemaname_sourcename*. *capture_instance* non può superare i 100 caratteri e deve essere univoco all'interno del database. Se specificato o derivato, *capture_instance* viene tagliato di tutti gli spazi vuoti a destra della stringa.  
  
 Una tabella di origine può avere un massimo di due istanze di acquisizione. Per ulteriori informazioni, vedere [sys.sp_cdc_help_change_data_capture &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md).  
  
`[ @supports_net_changes = ] supports_net_changes` Indica se è necessario abilitare il supporto per l'esecuzione di query per le modifiche delta per questa istanza di acquisizione. *supports_net_changes* è di **bit** e il valore predefinito è 1 se la tabella contiene una chiave primaria o se la tabella contiene un indice univoco identificato mediante il @index_name parametro. In caso contrario, il valore predefinito del parametro è 0.  
  
 Se è 0, vengono generate solo le funzioni di supporto per l'esecuzione di query per tutte le modifiche.  
  
 Se è 1, vengono generate le funzioni necessarie per l'esecuzione di query per le modifiche delta.  
  
 Se *supports_net_changes* è impostato su 1, è necessario specificare *index_name* oppure è necessario che la tabella di origine disponga di una chiave primaria definita.  
  
`[ @index_name = ] 'index_name_'` Nome di un indice univoco da utilizzare per identificare in modo univoco le righe nella tabella di origine. *index_name* è di **tipo sysname** e può essere null. Se specificato, *index_name* deve essere un indice univoco valido nella tabella di origine. Se si specifica *index_name* , le colonne di indice identificate hanno la precedenza su qualsiasi colonna di chiave primaria definita come identificatore di riga univoco per la tabella.  
  
`[ @captured_column_list = ] 'captured_column_list'` Identifica le colonne della tabella di origine da includere nella tabella delle modifiche. *captured_column_list* è di **tipo nvarchar (max)** e può essere null. Se il valore è NULL, nella tabella delle modifiche vengono incluse tutte le colonne.  
  
 I nomi delle colonne devono essere colonne valide nella tabella di origine. È necessario includere le colonne definite in un indice di chiave primaria o le colonne definite in un indice a cui fa riferimento *index_name* .  
  
 *captured_column_list* è un elenco delimitato da virgole di nomi di colonna. I nomi delle singole colonne all'interno dell'elenco possono essere racchiusi tra virgolette ("") o parentesi quadrate ([]). Se un nome di una colonna contiene una virgola incorporata, il nome della colonna deve essere racchiuso tra virgolette.  
  
 *captured_column_list* non può contenere i seguenti nomi di colonna riservati: **_ _ $ start_lsn**, **_ _ $ end_lsn**, **_ _ $ seqval**, **_ _ $ operation** e **_ _ $ update_mask**.  
  
`[ @filegroup_name = ] 'filegroup_name'` Filegroup da utilizzare per la tabella delle modifiche creata per l'istanza di acquisizione. *filegroup_name* è di **tipo sysname** e può essere null. Se specificato, è necessario definire *filegroup_name* per il database corrente. Se il valore è NULL; viene utilizzato il filegroup predefinito.  
  
 Si consiglia di creare un filegroup separato per modificare le tabelle di acquisizione dei dati delle modifiche.  
  
`[ @allow_partition_switch = ] 'allow_partition_switch'` Indica se il comando SWITCH PARTITION di ALTER TABLE può essere eseguito su una tabella abilitata per Change Data Capture. *allow_partition_switch* è di **bit** e il valore predefinito è 1.  
  
 Per le tabelle non partizionate, l'impostazione di cambio è sempre 1 mentre l'impostazione effettiva viene ignorata. Se il cambio viene impostato in modo esplicito su 0 per una tabella non partizionata, viene generato l'avviso 22857 per indicare che è stata ignorata l'impostazione di cambio. Se il cambio viene impostato in modo esplicito su 0 per una tabella partizionata, viene generato l'avviso 22356 per indicare che non saranno consentite operazioni di cambio partizione nella tabella di origine. Infine, se l'impostazione di cambio viene impostata in modo esplicito su 1 o viene utilizzato 1 come valore predefinito e la tabella abilitata viene partizionata, viene generato l'avviso 22855 per indicare che i cambi di partizione non saranno bloccati. Se si verifica un cambio di partizione, Change Data Capture non rileverà le modifiche risultanti dal cambio. In questo modo verranno generate inconsistenze dei dati quando vengono utilizzati i dati delle modifiche.  
  
> [!IMPORTANT]  
>  SWITCH PARTITION è un'operazione eseguita sui metadati, ma apporta modifiche anche ai dati. Le modifiche dei dati associate a tale operazione non vengono acquisite nella tabella delle modifiche di Change Data Capture. Si consideri ad esempio una tabella in cui sono presenti tre partizioni cui vengono apportate modifiche. Il processo di acquisizione registrerà le operazioni di inserimento, aggiornamento ed eliminazione eseguite sulla tabella dall'utente. Tuttavia, se una partizione è stata trasferita in un'altra tabella, ad esempio per l'esecuzione di un'eliminazione bulk, le righe spostate come parte di questa operazione non verranno acquisite come righe eliminate nella tabella di modifica. Analogamente, se alla tabella viene aggiunta una nuova partizione in cui sono presenti righe che contengono dati, tali righe non verranno riflesse nella tabella di modifica. Questa situazione può provocare incoerenza tra i dati quando le modifiche vengono utilizzate da un'applicazione e applicate a una destinazione.  
 
> [!NOTE] 
> Se si abilita il cambio della partizione usando la versione corrente in SQL Server 2008 R2, successivamente potrebbero essere necessarie anche operazioni di divisione e merge. Prima di eseguire un'operazione di divisione o di merge su una tabella abilitata per il CDC o replicata, assicurarsi che la partizione in questione non abbia comandi replicati in sospeso. È anche necessario assicurarsi che nessuna operazione DML venga eseguita sulla partizione durante le operazioni di divisione e unione. Se sono presenti transazioni non elaborate dalla lettura log o dal processo di acquisizione CDC o se le operazioni DML vengono eseguite in una partizione di una tabella abilitata per il CDC o replicata durante l'esecuzione di un'operazione di divisione o di merge che coinvolge tale partizione, è possibile che si verifichi un errore di elaborazione (Error 608 - No catalog entry found for partition ID (Errore 608. Nessuna voce di catalogo trovata per l’ID partizione)) nell'agente di lettura log o nel processo di acquisizione CDC. Per correggere l'errore potrebbe essere necessario reinizializzare la sottoscrizione o disabilitare la funzionalità CDC sulla tabella o sul database.



## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Prima di abilitare una tabella per Change Data Capture, è necessario abilitare il database. Per determinare se il database è abilitato per la Change Data Capture, eseguire una query sulla colonna **is_cdc_enabled** nella vista del catalogo [sys. databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) . Per abilitare il database, utilizzare l'stored procedure [sys.sp_cdc_enable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) .  
  
 Quando Change Data Capture viene abilitato per una tabella, vengono generate una tabella delle modifiche e una o due funzioni di query. La tabella delle modifiche viene utilizzata come repository per le modifiche della tabella di origine estratte dal log delle transazioni dal processo di acquisizione. Le funzioni di query vengono utilizzate per estrarre i dati dalla tabella delle modifiche. I nomi di queste funzioni sono derivati dal parametro *capture_instance* nei modi seguenti:  
  
-   Funzione all changes: **cdc.fn_cdc_get_all_changes_<capture_instance>**  
  
-   Funzione net changes: **cdc.fn_cdc_get_net_changes_<capture_instance>**  
  
 **sys.sp_cdc_enable_table** inoltre creare i processi di acquisizione e pulizia per il database se la tabella di origine è la prima tabella del database da abilitare per Change Data Capture e non sono presenti pubblicazioni transazionali per il database. Imposta la colonna **is_tracked_by_cdc** nella vista del catalogo [sys. Tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) su 1.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non deve essere in esecuzione quando Change Data Capture viene abilitato per una tabella. Tuttavia, il processo di acquisizione elaborerà il log delle transazioni e scriverà voci nella tabella delle modifiche solo se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è in esecuzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **db_owner** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-enabling-change-data-capture-by-specifying-only-required-parameters"></a>R. Abilitazione di Change Data Capture tramite l'indicazione dei parametri obbligatori  
 Nell'esempio seguente viene abilitata l'acquisizione dei dati delle modifiche per la tabella `HumanResources.Employee`. Vengono specificati solo i parametri obbligatori.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Employee'  
  , @role_name = N'cdc_Admin';  
GO  
```  
  
### <a name="b-enabling-change-data-capture-by-specifying-additional-optional-parameters"></a>B. Abilitazione di Change Data Capture tramite l'indicazione di parametri aggiuntivi facoltativi  
 Nell'esempio seguente viene abilitata l'acquisizione dei dati delle modifiche per la tabella `HumanResources.Department`. Vengono specificati tutti i parametri tranne `@allow_partition_switch`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Department'  
  , @role_name = N'cdc_admin'  
  , @capture_instance = N'HR_Department'   
  , @supports_net_changes = 1  
  , @index_name = N'AK_Department_Name'   
  , @captured_column_list = N'DepartmentID, Name, GroupName'   
  , @filegroup_name = N'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_disable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.sp_cdc_help_jobs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)  
  
  
