---
description: sp_addmergearticle (Transact-SQL)
title: sp_addmergearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergearticle
- sp_addmergearticle_TSQL
helpviewer_keywords:
- sp_addmergearticle
ms.assetid: 0df654ea-24e2-4c61-a75a-ecaa7a140a6c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 52e59c1c4c7a75c209be99cbb6caacc425bd39f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192353"
---
# <a name="sp_addmergearticle-transact-sql"></a>sp_addmergearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un articolo a una pubblicazione di tipo merge esistente. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergearticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
        , [ @source_object = ] 'source_object'   
    [ , [ @type = ] 'type' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @column_tracking = ] 'column_tracking' ]   
    [ , [ @status = ] 'status' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @subset_filterclause = ] 'subset_filterclause' ]   
    [ , [ @article_resolver = ] 'article_resolver' ]   
    [ , [ @resolver_info = ] 'resolver_info' ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @verify_resolver_signature = ] verify_resolver_signature ]   
    [ , [ @destination_object = ] 'destination_object' ]   
    [ , [ @allow_interactive_resolver = ] 'allow_interactive_resolver' ]   
    [ , [ @fast_multicol_updateproc = ] 'fast_multicol_updateproc' ]   
    [ , [ @check_permissions = ] check_permissions ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @published_in_tran_pub = ] 'published_in_tran_pub' ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @logical_record_level_conflict_detection = ] 'logical_record_level_conflict_detection' ]  
    [ , [ @logical_record_level_conflict_resolution = ] 'logical_record_level_conflict_resolution' ]  
    [ , [ @partition_options = ] partition_options ]  
    [ , [ @processing_order = ] processing_order ]  
    [ , [ @subscriber_upload_options = ] subscriber_upload_options ]  
    [ , [ @identityrangemanagementoption = ] 'identityrangemanagementoption' ]  
    [ , [ @delete_tracking = ] delete_tracking ]  
    [ , [ @compensate_for_errors = ] 'compensate_for_errors' ]   
    [ , [ @stream_blob_columns = ] 'stream_blob_columns' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione contenente l'articolo. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo. Deve essere un nome univoco all'interno della pubblicazione. *article* è di **tipo sysname** e non prevede alcun valore predefinito. l' *articolo* deve trovarsi nel computer locale che esegue [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e deve essere conforme alle regole per gli identificatori.  
  
`[ @source_object = ] 'source_object'` Oggetto di database da pubblicare. *source_object* è di **tipo sysname** e non prevede alcun valore predefinito. Per ulteriori informazioni sui tipi di oggetti che possono essere pubblicati utilizzando la replica di tipo merge, vedere [pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
`[ @type = ] 'type'` Tipo di articolo. *Type* è di tipo **sysname** e il valore predefinito è **Table**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**Table** (impostazione predefinita)|Tabella con schema e dati. La tabella viene monitorata per determinare i dati da replicare.|  
|**func schema only**|Funzione con solo schema.|  
|**solo schema** della **vista indicizzata**|Vista indicizzata con solo schema.|  
|**proc schema only**|Stored procedure con solo schema.|  
|**solo schema sinonimo**|Sinonimo con solo schema.|  
|**view schema only**|Vista con solo schema.|  
  
`[ @description = ] 'description'` Descrizione dell'articolo. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @column_tracking = ] 'column_tracking'` Impostazione per il rilevamento a livello di colonna. *column_tracking* è di **tipo nvarchar (10)** e il valore predefinito è false. **true** attiva il rilevamento della colonna. **false** disattiva il rilevamento a livello di colonna e lascia il rilevamento dei conflitti a livello di riga. Se la tabella è già pubblicata in altre pubblicazioni di tipo merge, è necessario utilizzare lo stesso valore di rilevamento a livello di colonna utilizzato dagli articoli esistenti basati su questa tabella. Questo parametro è disponibile solo per gli articoli di tabelle.  
  
> [!NOTE]  
>  Se si utilizza il rilevamento a livello di riga per il rilevamento dei conflitti (impostazione predefinita), la tabella di base può includere fino a 1.024 colonne, che devono tuttavia essere filtrate dall'articolo in modo da pubblicare un massimo di 246 colonne. Se viene utilizzato il rilevamento a livello di colonna, nella tabella di base possono essere incluse al massimo 246 colonne.  
  
`[ @status = ] 'status'` Stato dell'articolo. *status* è di **tipo nvarchar (10)** e il valore predefinito è **unsynced**. Se **attivo**, viene eseguito lo script di elaborazione iniziale per pubblicare la tabella. Se non **sincronizzato**, lo script di elaborazione iniziale per la pubblicazione della tabella viene eseguito alla successiva esecuzione del agente di snapshot.  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` Specifica il sistema da eseguire se la tabella esiste nel Sottoscrittore durante l'applicazione dello snapshot. *pre_creation_cmd* è di **tipo nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**nessuna**|Se la tabella esiste già nel Sottoscrittore, non viene eseguita alcuna azione.|  
|**delete**|Esegue un'operazione di eliminazione in base alla clausola WHERE del filtro di subset.|  
|**Drop** (impostazione predefinita)|Elimina la tabella prima di ricrearla. Necessario per supportare i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori.|  
|**truncate**|Tronca la tabella di destinazione.|  
  
`[ @creation_script = ] 'creation_script'` Percorso e nome di uno script facoltativo dello schema dell'articolo utilizzato per creare l'articolo nel database di sottoscrizione. *creation_script* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
> [!NOTE]  
>  Gli script di creazione non vengono eseguiti nei Sottoscrittori [!INCLUDE[ssEW](../../includes/ssew-md.md)].  
  
`[ @schema_option = ] schema_option` Mappa di bit dell'opzione di generazione dello schema per l'articolo specificato. *schema_option* è **binario (8)** e può essere [| (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) prodotto di uno o più di questi valori.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0x00**|Disabilita la creazione di script da parte del agente di snapshot e utilizza lo script di creazione dello schema fornito definito in *creation_script*.|  
|**0x01**|Genera le istruzioni per la creazione di oggetti (CREATE TABLE, CREATE PROCEDURE e così via). Questo è il valore predefinito per gli articoli di stored procedure.|  
|**0x10**|Genera un indice cluster corrispondente. Anche se questa opzione non viene impostata, se sono già definiti in una tabella pubblicata, gli indici correlati alle chiavi primarie e i vincoli UNIQUE vengono generati.|  
|**0x20**|Converte i tipi di dati definiti dall'utente (UDT) in tipi di dati di base nel Sottoscrittore. Questa opzione non può essere usata quando è presente un vincolo CHECK o DEFAULT su una colonna UDT, se una colonna UDT è inclusa nella chiave primaria o se una colonna calcolata fa riferimento a una colonna UDT.|  
|**0x40**|Genera indici non cluster corrispondenti. Anche se questa opzione non viene impostata, se sono già definiti in una tabella pubblicata, gli indici correlati alle chiavi primarie e i vincoli UNIQUE vengono generati.|  
|**0x80**|Replica i vincoli PRIMARY KEY. Vengono replicati anche tutti gli indici correlati al vincolo, anche se le opzioni **0x10** e **0x40** non sono abilitate.|  
|**0x100**|Replica gli eventuali trigger dell'utente di un articolo di tabella.|  
|**0x200**|Replica i vincoli FOREIGN KEY. Se la tabella con riferimenti non fa parte di una pubblicazione, tutti i vincoli FOREIGN KEY in una tabella pubblicata non vengono replicati.|  
|**0x400**|Replica i vincoli CHECK.|  
|**0x800**|Replica i valori predefiniti.|  
|**0x1000**|Replica le regole di confronto a livello di colonna.|  
|**0x2000**|Replica le proprietà estese associate all'oggetto di origine dell'articolo pubblicato.|  
|**0x4000**|Replica i vincoli UNIQUE. Vengono replicati anche tutti gli indici correlati al vincolo, anche se le opzioni **0x10** e **0x40** non sono abilitate.|  
|**0x8000**|Questa opzione non è valida per i server di pubblicazione che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versioni successive.|  
|**0x10000**|Replica i vincoli CHECK come NOT FOR REPLICATION in modo che i vincoli non vengono imposti durante la sincronizzazione.|  
|**0x20000**|Replica i vincoli FOREIGN KEY come NOT FOR REPLICATION in modo che i vincoli non vengono imposti durante la sincronizzazione.|  
|**0x40000**|Replica i filegroup associati a una tabella o un indice partizionato.|  
|**0x80000**|Replica lo schema di partizione per una tabella partizionata.|  
|**0x100000**|Replica lo schema di partizione per un indice partizionato.|  
|**0x200000**|Replica le statistiche della tabella.|  
|**0x400000**|Replica le associazioni predefinite.|  
|**0x800000**|Replica le associazioni di regole.|  
|**0x1000000**|Replica l'indice full-text.|  
|**0x2000000**|Le raccolte di XML Schema associate alle colonne **XML** non vengono replicate.|  
|**0x4000000**|Replica gli indici nelle colonne **XML** .|  
|**0x8000000**|Crea gli eventuali schemi non ancora presenti nel Sottoscrittore.|  
|**0x10000000**|Converte le colonne **XML** in **ntext** nel Sottoscrittore.|  
|**0x20000000**|Converte i tipi di dati LOB (**nvarchar (max)**, **varchar (max)** e **varbinary (max)**) introdotti in in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tipi di dati supportati in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
|**0x40000000**|Replica le autorizzazioni.|  
|**0x80000000**|Esegue un tentativo di rimozione delle dipendenze dagli oggetti che non fanno parte della pubblicazione.|  
|**0x100000000**|Utilizzare questa opzione per replicare l'attributo FILESTREAM se è specificato nelle colonne **varbinary (max)** . Non specificare questa opzione se si stanno replicando tabelle nei Sottoscrittori [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La replica di tabelle con colonne FILESTREAM [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] nei Sottoscrittori non è supportata, indipendentemente dalla modalità di impostazione di questa opzione dello schema. Vedere l'opzione correlata **0x800000000**.|  
|**0x200000000**|Converte i tipi di dati di data e ora (**Data**, **ora**, **DateTimeOffset** e **datetime2**) introdotti in in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipi di dati supportati nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**0x400000000**|Replica l'opzione di compressione per dati e indici. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
|**0x800000000**|Impostare questa opzione per archiviare i dati FILESTREAM nel relativo filegroup nel Sottoscrittore. Se questa opzione non è impostata, i dati FILESTREAM vengono archiviati nel filegroup predefinito. Tramite la replica non vengono creati filegroup, pertanto, se si imposta questa opzione, è necessario creare il filegroup prima di applicare lo snapshot nel Sottoscrittore. Per altre informazioni su come creare oggetti prima di applicare lo snapshot, vedere [eseguire gli script prima e dopo l'applicazione dello snapshot](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vedere l'opzione correlata **0x100000000**.|  
|**0x1000000000**|Converte Common Language Runtime tipi CLR definiti dall'utente (UDT) in **varbinary (max)** in modo che le colonne di tipo UDT possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x2000000000**|Converte il tipo di dati **hierarchyid** in **varbinary (max)** in modo che le colonne di tipo **hierarchyid** possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Per ulteriori informazioni sull'utilizzo delle colonne **hierarchyid** nelle tabelle replicate, vedere [hierarchyid &#40;&#41;Transact-SQL](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
|**0x4000000000**|Replica gli eventuali indici filtrati sulla tabella. Per altre informazioni sugli indici filtrati, vedere [creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md).|  
|**0x8000000000**|Converte i tipi di dati **geography** e **Geometry** in **varbinary (max)** in modo che le colonne di questi tipi possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x10000000000**|Replica gli indici nelle colonne di tipo **geography** e **Geometry**.|  
  
 Se è NULL, il sistema genera automaticamente un'opzione di schema valida per l'articolo. La tabella delle **opzioni predefinite dello schema** nella sezione Osservazioni Mostra il valore scelto in base al tipo di articolo. Inoltre, non tutti i valori *schema_option* sono validi per ogni tipo di replica e tipo di articolo. La tabella delle opzioni di **schema valida** indicata nella sezione Osservazioni Mostra le opzioni che è possibile specificare per un determinato tipo di articolo.  
  
> [!NOTE]  
>  Il parametro *schema_option* influiscono solo sulle opzioni di replica per lo snapshot iniziale. Una volta che lo schema iniziale è stato generato dal agente di snapshot e applicato nel Sottoscrittore, la replica delle modifiche dello schema di pubblicazione nel Sottoscrittore viene eseguita in base alle regole di replica delle modifiche dello schema e all'impostazione del parametro *replicate_ddl* specificata in [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Per altre informazioni, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
`[ @subset_filterclause = ] 'subset_filterclause'` Clausola WHERE che specifica il filtro orizzontale di un articolo di tabella senza la parola WHERE. *subset_filterclause* è di **tipo nvarchar (1000)** e il valore predefinito è una stringa vuota.  
  
> [!IMPORTANT]  
>  Per motivi relativi alle prestazioni, è consigliabile evitare di applicare funzioni ai nomi di colonna nelle clausole di filtro di riga con parametri, come `LEFT([MyColumn]) = SUSER_SNAME()`. Se si usa [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) in una clausola di filtro e si sostituisce il valore di HOST_NAME, potrebbe essere necessario convertire i tipi di dati tramite [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Per ulteriori informazioni sulle procedure consigliate in questo caso, vedere la sezione "override del valore HOST_NAME ()" nei [filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
`[ @article_resolver = ] 'article_resolver'` È il sistema di risoluzione basato su COM utilizzato per risolvere i conflitti nell'articolo di tabella o l'assembly .NET Framework richiamato per eseguire la logica di business personalizzata sull'articolo di tabella. *article_resolver* è di tipo **varchar (255)** e il valore predefinito è null. I valori disponibili per questi parametri sono elencati nell'argomento relativo ai sistemi di risoluzione personalizzati [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Se il valore specificato non corrisponde a uno dei sistemi di risoluzione [!INCLUDE[msCoName](../../includes/msconame-md.md)], in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene utilizzato il sistema di risoluzione specificato anziché quello di sistema. Usare **sp_enumcustomresolvers** per enumerare l'elenco di resolver personalizzati disponibili. Per ulteriori informazioni, vedere [esecuzione della logica di business durante la sincronizzazione di tipo merge](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md) e [rilevamento e risoluzione dei conflitti di replica di tipo merge avanzati](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
`[ @resolver_info = ] 'resolver_info'` Viene usato per specificare informazioni aggiuntive richieste da un resolver personalizzato. Alcuni sistemi di risoluzione [!INCLUDE[msCoName](../../includes/msconame-md.md)] richiedono una colonna come input. *resolver_info* è di **tipo nvarchar (255)** e il valore predefinito è null. Per altre informazioni, vedere [Sistemi di risoluzione dei conflitti basati su Microsoft COM](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
`[ @source_owner = ] 'source_owner'` Nome del proprietario del *source_object*. *source_owner* è di **tipo sysname** e il valore predefinito è null. Se il valore è NULL, l'utente corrente verrà considerato il proprietario.  
  
`[ @destination_owner = ] 'destination_owner'` Proprietario dell'oggetto nel database di sottoscrizione, se diverso da "dbo". *destination_owner* è di **tipo sysname** e il valore predefinito è null. Se NULL, 'dbo' verrà considerato il proprietario.  
  
`[ @vertical_partition = ] 'column_filter'` Abilita e Disabilita l'applicazione di filtri alle colonne in un articolo di tabella. *vertical_partition* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
 **false** indica che non è presente alcun filtro verticale e che tutte le colonne vengono pubblicate.  
  
 **true** Cancella tutte le colonne ad eccezione della chiave primaria e delle colonne ROWGUID dichiarate. Le colonne vengono aggiunte utilizzando **sp_mergearticlecolumn**.  
  
`[ @auto_identity_range = ] 'automatic_identity_range'` Abilita e Disabilita la gestione automatica degli intervalli di valori Identity per questo articolo di tabella in una pubblicazione al momento della creazione. *auto_identity_range* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** consente la gestione automatica degli intervalli di valori Identity, mentre **false** lo Disabilita.  
  
> [!NOTE]  
>  *auto_identity_range* è stato deprecato e viene fornito solo per compatibilità con le versioni precedenti. Usare *identityrangemanagementoption* per specificare le opzioni di gestione degli intervalli di valori Identity. Per altre informazioni, vedere [Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @pub_identity_range = ] pub_identity_range` Controlla la dimensione dell'intervallo di valori Identity allocata a un Sottoscrittore con una sottoscrizione server quando viene utilizzata la gestione automatica degli intervalli di valori Identity. L'intervallo di valori Identity è riservato al Sottoscrittore di ripubblicazione per l'assegnazione ai propri Sottoscrittori. *pub_identity_range* è di tipo **bigint** e il valore predefinito è null. È necessario specificare questo parametro se *identityrangemanagementoption* è **automatico** o se *auto_identity_range* è **true**.  
  
`[ @identity_range = ] identity_range` Controlla la dimensione dell'intervallo di valori Identity allocata al server di pubblicazione e al Sottoscrittore quando viene utilizzata la gestione automatica degli intervalli di valori Identity. *identity_range* è di tipo **bigint** e il valore predefinito è null. È necessario specificare questo parametro se *identityrangemanagementoption* è **automatico** o se *auto_identity_range* è **true**.  
  
> [!NOTE]  
>  *identity_range* controlla le dimensioni dell'intervallo di valori Identity nei Sottoscrittori di ripubblicazione utilizzando versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @threshold = ] threshold` Valore percentuale che controlla quando il agente di merge assegna un nuovo intervallo di valori Identity. Quando viene utilizzata la percentuale di valori specificata in *Threshold* , il agente di merge crea un nuovo intervallo di valori Identity. *Threshold* è di **tipo int** e il valore predefinito è null. È necessario specificare questo parametro se *identityrangemanagementoption* è **automatico** o se *auto_identity_range* è **true**.  
  
`[ @verify_resolver_signature = ] verify_resolver_signature` Specifica se una firma digitale viene verificata prima di utilizzare un sistema di risoluzione dei conflitti nella replica di tipo merge. *verify_resolver_signature* è di **tipo int** e il valore predefinito è 1.  
  
 **0** indica che la firma non verrà verificata.  
  
 **1** specifica che la firma verrà verificata per verificare se si tratta di una fonte attendibile.  
  
`[ @destination_object = ] 'destination_object'` Nome dell'oggetto nel database di sottoscrizione. *destination_object* è di **tipo sysname** e il valore predefinito è **\@ source_object**. È possibile specificare questo parametro solo se l'articolo include solo lo schema, come nel caso di stored procedure, viste e funzioni definite dall'utente. Se l'articolo specificato è un articolo di tabella, il valore in *\@ source_object* esegue l'override del valore *destination_object*.  
  
`[ @allow_interactive_resolver = ] 'allow_interactive_resolver'` Abilita o Disabilita l'utilizzo del sistema di risoluzione interattivo in un articolo. *allow_interactive_resolver* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** consente l'utilizzo del sistema di risoluzione interattivo sull'articolo. **false** lo Disabilita.  
  
> [!NOTE]  
>  Il sistema di risoluzione interattivo non è supportato dai Sottoscrittori [!INCLUDE[ssEW](../../includes/ssew-md.md)].  
  
`[ @fast_multicol_updateproc = ] 'fast_multicol_updateproc'` Questo parametro è stato deprecato e viene mantenuto per compatibilità con le versioni precedenti degli script.  
  
`[ @check_permissions = ] check_permissions` Bitmap delle autorizzazioni a livello di tabella verificate quando il agente di merge applica le modifiche al server di pubblicazione. Se l'account di accesso o l'account utente del server di pubblicazione utilizzato per il processo di merge non dispone delle autorizzazioni corrette per le tabelle, le modifiche non valide vengono registrate come conflitti. *check_permissions* è di **tipo int** e può essere [| (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) prodotto di uno o più dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0x00** (impostazione predefinita)|Le autorizzazioni non vengono controllate.|  
|**0x10**|Le autorizzazioni vengono controllate nel server di pubblicazione prima di consentire il caricamento delle operazioni di inserimento eseguite nel Sottoscrittore.|  
|**0x20**|Le autorizzazioni vengono controllate nel server di pubblicazione prima di consentire il caricamento delle operazioni di aggiornamento eseguite nel Sottoscrittore.|  
|**0x40**|Le autorizzazioni vengono controllate nel server di pubblicazione prima di consentire il caricamento delle operazioni di eliminazione eseguite nel Sottoscrittore.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è di **bit** e il valore predefinito è 0.  
  
 **0** indica che l'aggiunta di un articolo non comporta l'invalidità dello snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che l'aggiunta di un articolo potrebbe invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot. *force_invalidate_snapshot* viene impostato su **1** quando si aggiunge un articolo a una pubblicazione con uno snapshot esistente.  
  
`[ @published_in_tran_pub = ] 'published_in_tran_pub'` Indica che un articolo in una pubblicazione di tipo merge viene pubblicato anche in una pubblicazione transazionale. *published_in_tran_pub* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** specifica che l'articolo è pubblicato anche in una pubblicazione transazionale.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Conferma che l'azione eseguita da questo stored procedure potrebbe richiedere la reinizializzazione delle sottoscrizioni esistenti. *force_reinit_subscription* è di **bit** e il valore predefinito è 0.  
  
 **0** indica che l'aggiunta di un articolo non comporta la reinizializzazione della sottoscrizione. Se la stored procedure rileva che la modifica richiede la reinizializzazione delle sottoscrizioni esistenti, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** indica che le modifiche apportate all'articolo di merge causano la reinizializzazione delle sottoscrizioni esistenti e consente la reinizializzazione della sottoscrizione. *force_reinit_subscription* viene impostato su **1** quando *subset_filterclause* specifica un filtro di riga con parametri.  
  
`[ @logical_record_level_conflict_detection = ] 'logical_record_level_conflict_detection'` Specifica il livello di rilevamento dei conflitti per un articolo membro di un record logico. *logical_record_level_conflict_detection* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
 **true** specifica che verrà rilevato un conflitto se le modifiche vengono apportate in qualsiasi punto del record logico.  
  
 **false** specifica che il rilevamento dei conflitti predefinito viene utilizzato come specificato da *column_tracking*. Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  Poiché i record logici non sono supportati dai [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, è necessario specificare il valore **false** per *logical_record_level_conflict_detection* per supportare tali Sottoscrittori.  
  
`[ @logical_record_level_conflict_resolution = ] 'logical_record_level_conflict_resolution'` Specifica il livello di risoluzione dei conflitti per un articolo membro di un record logico. *logical_record_level_conflict_resolution* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
 **true** specifica che l'intero record logico vincente sovrascrive il record logico perdente.  
  
 **false** specifica che le righe vincenti non sono vincolate al record logico. Se *logical_record_level_conflict_detection* è **true**, è necessario impostare anche *logical_record_level_conflict_resolution* su **true**. Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  Poiché i record logici non sono supportati dai [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, è necessario specificare il valore **false** per *logical_record_level_conflict_resolution* per supportare tali Sottoscrittori.  
  
`[ @partition_options = ] partition_options` Definisce la modalità di partizionamento dei dati nell'articolo, che consente di ottimizzare le prestazioni se tutte le righe appartengono a una sola partizione o a una sola sottoscrizione. *partition_options* è di **tinyint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|Il filtro applicato all'articolo è statico oppure non restituisce un subset di dati univoco per ogni partizione, ovvero si creano partizioni sovrapposte.|  
|**1**|Le partizioni sono sovrapposte e gli aggiornamenti DML (Data Manipulation Language) eseguiti nel Sottoscrittore non possono modificare la partizione a cui appartiene una riga.|  
|**2**|Il filtro applicato all'articolo restituisce partizioni non sovrapposte, ma più Sottoscrittori possono ricevere la stessa partizione.|  
|**3**|Il filtro applicato all'articolo restituisce partizioni non sovrapposte univoche per ogni sottoscrizione.|  
  
> [!NOTE]  
>  Se la tabella di origine di un articolo è già pubblicata in un'altra pubblicazione, il valore di *partition_options* deve essere lo stesso per entrambi gli articoli.  
  
`[ @processing_order = ] processing_order` Indica l'ordine di elaborazione degli articoli in una pubblicazione di tipo merge. *processing_order* è di **tipo int** e il valore predefinito è 0. **0** indica che l'articolo non è ordinato e qualsiasi altro valore rappresenta il valore ordinale dell'ordine di elaborazione per l'articolo. Gli articoli vengono elaborati in ordine crescente in base al valore. Se due articoli hanno lo stesso valore, l'ordine di elaborazione è determinato dall'ordine del nome alternativo dell'articolo nella tabella di sistema [sysmergearticles](../../relational-databases/system-tables/sysmergearticles-transact-sql.md) . Per altre informazioni, vedere [Specificare le proprietà della replica di tipo merge](../../relational-databases/replication/merge/specify-merge-replication-properties.md).  
  
`[ @subscriber_upload_options = ] subscriber_upload_options` Definisce le restrizioni sugli aggiornamenti eseguiti in un Sottoscrittore con una sottoscrizione client. Per altre informazioni, vedere [Ottimizzare le prestazioni della replica di tipo merge con gli articoli di solo download](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md). *subscriber_upload_options* è di **tinyint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|Nessuna restrizione. Le modifiche eseguite nel Sottoscrittore vengono caricate nel server di pubblicazione.|  
|**1**|Sono consentite modifiche in un Sottoscrittore, ma tali modifiche non vengono caricate nel server di pubblicazione.|  
|**2**|Non sono consentite modifiche nel Sottoscrittore.|  
  
 Per modificare *subscriber_upload_options* è necessario reinizializzare la sottoscrizione chiamando [sp_reinitmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergepullsubscription-transact-sql.md).  
  
> [!NOTE]  
>  Se la tabella di origine di un articolo è già pubblicata in un'altra pubblicazione, il valore di *subscriber_upload_options* deve essere lo stesso per entrambi gli articoli.  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` Specifica il modo in cui viene gestita la gestione degli intervalli di valori Identity per l'articolo. *identityrangemanagementoption* è di **tipo nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**nessuna**|Disabilita la gestione degli intervalli di valori Identity.|  
|**Manuale**|Contrassegna la colonna Identity con NOT FOR REPLICATION per consentire la gestione manuale degli intervalli di valori Identity.|  
|**Automatico**|Imposta la gestione automatica degli intervalli di valori Identity.|  
|NULL (impostazione predefinita)|Il valore predefinito è **None** quando il valore di *auto_identity_range* non è **true**.|  
  
 Per compatibilità con le versioni precedenti, quando il valore di *identityrangemanagementoption* è null, viene verificato il valore di *auto_identity_range* . Tuttavia, quando il valore di *identityrangemanagementoption* non è null, il valore di *auto_identity_range* viene ignorato. Per altre informazioni, vedere [Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @delete_tracking = ] 'delete_tracking'` Indica se le eliminazioni vengono replicate. *delete_tracking* è di **tipo nvarchar (5)** e il valore predefinito è true. **false** indica che le eliminazioni non vengono replicate e **true** indica che vengono replicate le eliminazioni, che è il comportamento usuale per la replica di tipo merge. Se *delete_tracking* è impostato su **false**, le righe eliminate nel Sottoscrittore devono essere rimosse manualmente nel server di pubblicazione e le righe eliminate nel server di pubblicazione devono essere rimosse manualmente nel Sottoscrittore.  
  
> [!IMPORTANT]  
>  L'impostazione di *delete_tracking* su **false** comporta la non convergenza. Se la tabella di origine di un articolo è già pubblicata in un'altra pubblicazione, il valore di *delete_tracking* deve essere lo stesso per entrambi gli articoli.  
  
> [!NOTE]  
>  Impossibile impostare le opzioni di *delete_tracking* tramite la **creazione guidata nuova pubblicazione** o la finestra di dialogo **Proprietà pubblicazione** .  
  
`[ @compensate_for_errors = ] 'compensate_for_errors'` Indica se vengono eseguite azioni di compensazione quando vengono rilevati errori durante la sincronizzazione. *compensate_for_errors* è di **tipo nvarchar (5)** e il valore predefinito è false. Se impostato su **true**, le modifiche che non possono essere applicate a un Sottoscrittore o a un server di pubblicazione durante la sincronizzazione comportano sempre azioni di compensazione per annullare la modifica. Tuttavia, un sottoscrittore configurato in modo non corretto che genera un errore può causare l'annullamento delle modifiche in altri sottoscrittori e autori. **false** Disabilita queste azioni di compensazione. Tuttavia, gli errori vengono comunque registrati come con la compensazione e le unioni successive continuano a tentare di applicare le modifiche fino a quando l'operazione non riesce.  
  
> [!IMPORTANT]  
>  Anche se i dati nelle righe interessate potrebbero sembrare non convergenti, non appena vengono risolti gli eventuali errori generati le modifiche potranno essere applicate e si otterrà la convergenza dei dati. Se la tabella di origine di un articolo è già pubblicata in un'altra pubblicazione, il valore di *compensate_for_errors* deve essere lo stesso per entrambi gli articoli.  
  
`[ @stream_blob_columns = ] 'stream_blob_columns'` Specifica che viene utilizzata l'ottimizzazione del flusso di dati durante la replica di colonne BLOB (Binary Large Object). *stream_blob_columns* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** indica che verrà tentata l'ottimizzazione. *stream_blob_columns* è impostato su true quando FILESTREAM è abilitato. In questo modo, la replica dei dati FILESTREAM può essere eseguita in maniera ottimale e si riduce l'utilizzo della memoria. Per forzare gli articoli di tabella FILESTREAM in modo che non utilizzino flussi blob, utilizzare **sp_changemergearticle** per impostare *stream_blob_columns* su false.  
  
> [!IMPORTANT]  
>  L'abilitazione di questa ottimizzazione della memoria può ridurre le prestazioni dell'agente di merge durante la sincronizzazione. È consigliabile utilizzare questa opzione solo se vengono replicate colonne contenenti più megabyte di dati.  
  
> [!NOTE]  
>  Alcune funzionalità della replica di tipo merge, come i record logici, possono comunque impedire l'utilizzo dell'ottimizzazione del flusso durante la replica di oggetti binari di grandi dimensioni anche con *stream_blob_columns* impostato su **true**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergearticle** viene utilizzata nella replica di tipo merge.  
  
 Quando si pubblicano gli oggetti, nei Sottoscrittori vengono copiate le relative definizioni. Per la pubblicazione di un oggetto di database che dipende da altri oggetti, è necessario pubblicare tutti gli oggetti a cui fa riferimento. Se ad esempio si pubblica una vista che dipende da una tabella, sarà necessario pubblicare anche la tabella.  
  
 Se si specifica un valore pari a **3** per *partition_options*, può essere presente una sola sottoscrizione per ogni partizione di dati nell'articolo. Se si crea una seconda sottoscrizione nella quale il criterio di filtro porta alla restituzione della stessa partizione della sottoscrizione esistente, quest'ultima viene eliminata.  
  
 Quando si specifica un valore pari a 3 per *partition_options*, i metadati vengono puliti ogni volta che viene eseguita la agente di merge e lo snapshot partizionato scade più rapidamente. Quando si utilizza questa opzione è consigliabile prendere in considerazione l'abilitazione di snapshot partizionati richiesti dal Sottoscrittore. Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 L'aggiunta di un articolo con un filtro orizzontale statico, utilizzando *subset_filterclause*, a una pubblicazione esistente con articoli con filtri con parametri richiede la reinizializzazione delle sottoscrizioni.  
  
 Quando si specifica *processing_order*, è consigliabile lasciare gap tra i valori dell'ordine degli articoli, semplificando l'impostazione di nuovi valori in futuro. Se, ad esempio, si dispone di tre articoli denominati Articolo1, Articolo2 e Articolo3, impostare *processing_order* su 10, 20 e 30, anziché 1, 2 e 3. Per altre informazioni, vedere [Specificare le proprietà della replica di tipo merge](../../relational-databases/replication/merge/specify-merge-replication-properties.md).  
  
## <a name="default-schema-option-table"></a>Tabella delle opzioni predefinite dello schema  
 In questa tabella viene descritto il valore predefinito impostato dal stored procedure se viene specificato un valore NULL per *schema_option*, che dipende dal tipo di articolo.  
  
|Tipo di articolo|Valore delle opzioni di schema|  
|------------------|-------------------------|  
|**func schema only**|**0x01**|  
|**indexed view schema only**|**0x01**|  
|**proc schema only**|**0x01**|  
|**tabella**|  -  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] pubblicazioni compatibili con 0x0C034FD1: e versioni successive con snapshot in modalità nativa.<br /><br />   -  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] pubblicazioni compatibili con 0x08034FF1 e versioni successive con snapshot in modalità carattere.|  
|**view schema only**|**0x01**|  
  
> [!NOTE]  
>  Se la pubblicazione supporta versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'opzione dello schema predefinito per **Table** è **0x30034FF1**.  
  
## <a name="valid-schema-option-table"></a>Tabella delle opzioni di schema valide  
 Nella tabella seguente vengono descritti i valori consentiti *schema_option* a seconda del tipo di articolo.  
  
|Tipo di articolo|Valori delle opzioni di schema|  
|------------------|--------------------------|  
|**func schema only**|**0x01** e **0x2000**|  
|**indexed view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** e **0x200000**|  
|**proc schema only**|**0x01** e **0x2000**|  
|**tabella**|Tutte le opzioni.|  
|**view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** e **0x200000**|  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-addmergearticle-trans_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **db_owner** .  
  
## <a name="see-also"></a>Vedere anche  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replica colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
