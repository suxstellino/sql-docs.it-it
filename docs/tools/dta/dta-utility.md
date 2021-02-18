---
title: dta - utilità
description: L'utilità dta è una versione del prompt dei comandi di Ottimizzazione guidata motore di database che consente di usare le funzionalità in applicazioni e script.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/09/2017
ms.openlocfilehash: b05279df08153efab4257a74a8389ea33b38b966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338740"
---
# <a name="dta-utility"></a>dta - utilità

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'utilità **dta** è la versione per il prompt dei comandi dello strumento Ottimizzazione guidata motore di database. L'utilità **dta** è stata sviluppata per consentire l'utilizzo della funzionalità Ottimizzazione guidata motore di database in applicazioni e script.  

In modo analogo a Ottimizzazione guidata motore di database, l'utilità **dta** analizza il carico di lavoro e propone strutture di progettazione fisica per ottimizzare le prestazioni a livello di server per il carico di lavoro specifico. Il carico di lavoro può essere una cache dei piani, un file o una tabella di traccia di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] o uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Le strutture di progettazione fisica includono indici, viste indicizzate e schemi di partizionamento. Dopo aver analizzato un carico di lavoro, l'utilità **dta** visualizza un'indicazione di progettazione fisica dei database e quindi genera lo script necessario per implementare tale indicazione. I carichi di lavoro possono essere specificati dal prompt dei comandi con gli argomenti **-if** o **-it** . È anche possibile specificare un file di input XML dal prompt dei comandi con l'argomento **-ix** . In quest'ultimo caso, il carico di lavoro viene specificato nel file di input XML.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | -E  }  
      { -D database_name [ ,...n ] }  
      [ -d database_name ]   
      [ -Tl table_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -iq }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -or output_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -n number_of_events ]
      [ -I time_window_in_hours ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi filtered_indexes]  
      [ -fc columnstore_indexes]  
      [ -fp partitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fx drop_only_mode ]  
      [ -B storage_size ]  
      [ -c max_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>Argomenti  
 **-?**  
 Visualizza le informazioni sull'utilizzo.  
  
 **-A** _time_for_tuning_in_minutes_  
 Specifica il limite di tempo di ottimizzazione espresso in minuti. **dta** utilizza l'intervallo di tempo specificato per ottimizzare il carico di lavoro e generare uno script in base alle indicazioni relative alla modifica della progettazione fisica. Per impostazione predefinita, **dta** utilizza un tempo di ottimizzazione pari a 8 ore. Se si specifica 0, viene impostato un tempo di ottimizzazione illimitato. È possibile che l'utilità **dta** termini l'ottimizzazione dell'intero carico di lavoro prima che sia trascorso il limite di tempo specificato. Per garantire l'ottimizzazione dell'intero carico di lavoro, è tuttavia consigliabile specificare un tempo di ottimizzazione illimitato (-A 0).  
  
 **-a**  
 Ottimizza il carico di lavoro e applica l'indicazione senza richiedere conferma all'utente.  
  
 **-B** _storage_size_  
 Specifica lo spazio massimo, espresso in megabyte, che può essere utilizzato dall'indice e dal partizionamento consigliati. Se si ottimizzano più database, per il calcolo dello spazio vengono considerate le indicazioni per tutti i database. Per impostazione predefinita, **dta** utilizza il valore più basso delle dimensioni dello spazio di archiviazione riportate di seguito:  
  
-   Valore triplo della dimensione corrente dei dati non elaborati, incluse le dimensioni totali degli heap e degli indici cluster nelle tabelle del database.  
  
-   Lo spazio disponibile su tutte le unità disco collegate sommato alle dimensioni dei dati non elaborati.  
  
 Le dimensioni dello spazio di archiviazione predefinite non includono gli indici non cluster e le viste indicizzate.  
  
 **-C** _max_columns_in_index_  
 Specifica il numero massimo di colonne negli indici proposto da **dta** . Il valore massimo è 1024. Per impostazione predefinita, questo argomento è impostato su 16.  
  
 **-c** _max_key_columns_in_index_  
 Specifica il numero massimo di colonne chiave negli indici proposto da **dta** . Il valore predefinito è 16, ovvero il valore massimo consentito. In **dta** viene inoltre presa in considerazione la creazione di indici con colonne incluse. Gli indici con colonne incluse indicati dall'utilità potrebbero superare il numero di colonne specificato in questo argomento.  
  
 **-D** _database_name_  
 Specifica il nome di ogni database da ottimizzare. Il primo database è il database predefinito. Per specificare più database, separare i relativi nomi con una virgola, ad esempio:  
  
```  
dta -D database_name1, database_name2...  
```  
  
 In alternativa, è possibile specificare più database usando l'argomento **-D** per ogni nome di database, ad esempio:  
  
```  
dta -D database_name1 -D database_name2... n  
```  
  
 L'argomento **-D** è obbligatorio. Se si omette l'argomento **-d** , **dta** si connette inizialmente al database specificato nella prima clausola `USE database_name` del carico di lavoro. Se non è presente alcuna clausola `USE database_name` esplicita nel carico di lavoro, è necessario usare l'argomento **-d** .  
  
 Se, ad esempio, un carico di lavoro non include una clausola `USE database_name` esplicita e si utilizza il comando **dta** seguente, non verrà generata alcuna indicazione.  
  
```  
dta -D db_name1, db_name2...  
```  
  
 Se invece si usa lo stesso carico di lavoro e si specifica il comando **dta** seguente con l'argomento **-d** , viene generata un'indicazione:  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** _database_name_  
 Specifica il primo database al quale si connette **dta** per ottimizzare un carico di lavoro. Per questo argomento è possibile specificare solo un database. Ad esempio:  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 Se vengono specificati più nomi di database, **dta** restituisce un errore. L'argomento **-d** è facoltativo.  
  
 Se si utilizza un file di input XML, è possibile specificare il primo database al quale l'utilità **dta** si connette usando l'elemento **DatabaseToConnect** che si trova sotto l'elemento **TuningOptions** . Per altre informazioni, vedere [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).  
  
 In caso di ottimizzazione di un solo database, l'argomento **-d** è caratterizzato da una funzionalità simile all'argomento **-d** dell'utilità **sqlcmd** , ma non esegue l'istruzione USE *database_name* . Per altre informazioni, vedere [sqlcmd Utility](../../tools/sqlcmd-utility.md).  
  
 **-E**  
 Utilizza una connessione trusted anziché richiedere una password. È necessario usare l'argomento **-E** o **-U** , che specifica un ID di accesso.  
  
 **-e** _tuning_log_name_  
 Specifica il nome della tabella o del file in cui **dta** registra gli eventi che non possono essere ottimizzati. La tabella viene creata nel server in cui viene eseguita l'ottimizzazione.  
  
 Se si usa una tabella, specificarne il nome in formato *[database_name].[owner_name].table_name*. Nella tabella seguente sono riportati i valori predefiniti per ogni parametro.  
  
|Parametro|Valore predefinito|Dettagli|  
|---------------|-------------------|-------------|  
|*database_name*|*database_name* specificato con l'opzione **-D**||  
|*owner_name*|**dbo**|*owner_name* deve essere **dbo**. Se si specifica un qualsiasi altro valore, l'esecuzione di **dta** ha esito negativo e viene restituito un errore.|  
|*table_name*|nessuno||  
  
 Se si utilizza un file, specificare l'estensione xml, ad esempio TuningLog.xml.  
  
> [!NOTE]  
>  L'utilità **dta** non elimina il contenuto delle tabelle del log di ottimizzazione specificate dall'utente se la sessione viene eliminata. Quando si esegue l'ottimizzazione di carichi di lavoro molto estesi, è consigliabile specificare una tabella per il log di ottimizzazione. Poiché l'ottimizzazione di carichi di lavoro estesi può generare log di ottimizzazione estesi, è possibile eliminare le sessioni molto più velocemente in caso di utilizzo di una tabella.  
  
 **-F**  
 Consente a **dta** di sovrascrivere un file di output esistente. Se esiste già un file di output con lo stesso nome e si omette **-F** , **dta** restituisce un errore. È possibile usare **-F** con **-of**, **-or** oppure **-ox**.  
  
 **-fa** _physical_design_structures_to_add_  
 Specifica i tipi di strutture di progettazione fisica che **dta** deve includere nell'indicazione. Nella tabella seguente sono riportati e descritti i valori che è possibile specificare per questo argomento. Se non si specifica alcun valore, **dta** usa l'argomento predefinito **-fa IDX**.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|IDX_IV|Indici e viste indicizzate.|  
|IDX|Solo indici.|  
|IV|Solo viste indicizzate.|  
|NCL_IDX|Solo indici non cluster.|  
  
 **-fi**  
 Specifica gli indici filtrati da considerare per le nuove indicazioni. Per altre informazioni, vedere [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
**-fc**  
 Specifica che gli indici columnstore devono essere considerati per nuove indicazioni. DTA considererà gli indici columnstore sia cluster sia non cluster. Per ulteriori informazioni, vedere    
[Indicazioni relative agli indici columnstore in Ottimizzazione guidata motore di database](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md).
 ||  
|-|  
|**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive.|  

  
 **-fk** _keep_existing_option_  
 Specifica le strutture di progettazione fisica esistenti che **dta** deve conservare durante la generazione dell'indicazione. Nella tabella seguente sono riportati e descritti i valori che è possibile specificare per questo argomento.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|NONE|Nessuna struttura esistente.|  
|ALL|Tutte le strutture esistenti.|  
|ALIGNED|Tutte le strutture con partizionamento allineato.|  
|CL_IDX|Tutti gli indici cluster nelle tabelle.|  
|IDX|Tutti gli indici cluster e non cluster nelle tabelle.|  
  
 **-fp** _partitioning_strategy_  
 Specifica se le nuove strutture di progettazione fisica, ovvero indici e viste indicizzate, proposte da **dta** devono essere partizionate e definisce la modalità di partizionamento. Nella tabella seguente sono riportati e descritti i valori che è possibile specificare per questo argomento.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|NONE|Nessun partizionamento.|  
|FULL|Partizionamento completo (scegliere questo valore per ottimizzare le prestazioni).|  
|ALIGNED|Solo partizionamento allineato (scegliere questo valore per ottimizzare la gestione).|  
  
 Se si specifica ALIGNED, nell'indicazione generata da **dta** ogni indice proposto viene partizionato esattamente nello stesso modo della tabella sottostante per la quale è stato definito l'indice. Gli indici non cluster in una vista indicizzata sono allineati in base alla vista indicizzata. Per questo argomento è possibile specificare solo un valore. Il valore predefinito è **-fp NONE**.  
  
 **-fx** _drop_only_mode_  
 Specifica che **dta** prende in considerazione esclusivamente l'eliminazione delle strutture di progettazione fisica esistenti. Non verranno considerate le nuove strutture di progettazione fisica. Se si specifica questa opzione, **dta** valuta l'utilità delle strutture di progettazione fisica esistenti e propone di eliminare le strutture utilizzate meno di frequente. Questo argomento non utilizza alcun valore e non può essere usato in combinazione con gli argomenti **-fa**, **-fp** o **-fk ALL**  
  
 **-ID** _session_ID_  
 Specifica l'identificatore numerico per la sessione di ottimizzazione. Se omesso, **dta** genera un numero di identificazione. È possibile utilizzare questo identificatore per visualizzare le informazioni relative alle sessioni di ottimizzazione correnti. Se per **-ID** non si specifica alcun valore, è necessario specificare un nome di sessione usando **-s**.  
  
 **-ip**  
 Specifica che la cache dei piani deve essere utilizzata come carico di lavoro. Vengono analizzati i primi 1.000 eventi della cache dei piani per i database selezionati in modo esplicito. Questo valore può essere modificato tramite l'opzione **-n**.  

**-iq**  
 Specifica che Query Store deve essere usato come carico di lavoro. Vengono analizzati i primi 1.000 eventi di Query Store per i database selezionati in modo esplicito. Questo valore può essere modificato tramite l'opzione **-n**.  Per altre informazioni, vedere [Archivio query](../../relational-databases/performance/how-query-store-collects-data.md) e [Ottimizzazione del database tramite un carico di lavoro dell'archivio query](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
 ||  
|-|  
|**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive.|  
     
 **-if** _workload_file_  
 Specifica il percorso e il nome del file del carico di lavoro da utilizzare come input per l'ottimizzazione. Il file deve essere in formato trc (file di traccia di SQL Server Profiler), sql (file SQL) oppure log (file di traccia di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). È inoltre necessario specificare un file o una tabella del carico di lavoro.  
  
 **-it** _workload_trace_table_name_  
 Specifica il nome della tabella contenente la traccia del carico di lavoro per l'ottimizzazione. Il nome viene specificato in formato [*database_name*] **.** [*owner_name*] **.** _table_name_.  
  
 Nella tabella seguente sono riportati i valori predefiniti per ogni parametro.  
  
|Parametro|Valore predefinito|  
|---------------|-------------------|  
|*database_name*|*database_name* specificato con l'opzione **-D**.|  
|*owner_name*|**dbo**|  
|*table_name*|No.|  
  
> [!NOTE]  
>  *owner_name* deve essere **dbo**. Se viene specificato un altro valore, l'esecuzione di **dta** ha esito negativo e viene restituito un errore. È inoltre necessario specificare una tabella o un file del carico di lavoro.  
  
 **-ix** _input_XML_file_name_  
 Specifica il nome del file XML contenente le informazioni di input di **dta** . Tale file deve essere un documento XML valido conforme a DTASchema.xsd. Gli argomenti in conflitto specificati nel prompt dei comandi per le opzioni di ottimizzazione hanno la priorità sul valore corrispondente incluso in questo file XML. L'unica eccezione è rappresentata dal caso in cui una configurazione definita dall'utente venga specificata in modalità di valutazione nel file di input XML. Se, ad esempio, si specifica una configurazione nell'elemento **Configuration** del file di input XML e si specifica anche l'elemento **EvaluateConfiguration** come una delle opzioni di ottimizzazione, le opzioni di ottimizzazione specificate nel file di input XML avranno la priorità su qualsiasi opzione di ottimizzazione specificata nel prompt dei comandi.  

 **-k** _maxtotalindexes_  
 Numero massimo di indici nella raccomandazione.  

 **-K** _maxtotalindexes_  
 Numero massimo di indici per tabella.

 **-m** _minimum_improvement_  
 Specifica la percentuale minima di miglioramento che la configurazione consigliata deve soddisfare.  
  
 **-N** _online_option_  
 Specifica se le strutture di progettazione fisica vengono create online. Nella tabella seguente sono riportati e descritti i valori che è possibile specificare per questo argomento.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|OFF|Le strutture di progettazione fisica indicate non possono essere create online.|  
|ON|Tutte le strutture di progettazione fisica indicate possono essere create online.|  
|MIXED|Ottimizzazione guidata motore di database indica le strutture di progettazione fisica che possono essere create online quando possibile.|  
  
 Se gli indici vengono creati online, ONLINE = ON viene aggiunto alla relativa definizione di oggetto.  
  
 **-n** _number_of_events_  
 Specifica il numero di eventi nel carico di lavoro che **dta** deve ottimizzare. Se si specifica questo argomento e il carico di lavoro è un file di traccia contenente informazioni sulla durata, **dta** ottimizza gli eventi in base all'ordine decrescente di durata. Questo argomento risulta utile per confrontare due configurazioni di strutture di progettazione fisica. Per confrontare due configurazioni, per entrambe le configurazioni specificare lo stesso numero di eventi da ottimizzare e quindi un tempo di ottimizzazione illimitato nel modo illustrato di seguito:  
  
```  
dta -n number_of_events -A 0  
```  
  
 In questo caso, è importante specificare un tempo di ottimizzazione illimitato (`-A 0`). In caso contrario, Ottimizzazione guidata motore di database utilizza il tempo di ottimizzazione predefinito pari a 8 ore.
 
 **-I** _time_window_in_hours_   
   Specifica l'intervallo di tempo (in ore) in cui una query deve essere eseguita per essere considerata da DTA per l'ottimizzazione quando si usa l'opzione **-iq** (carico di lavoro da Query Store). 
```  
dta -iq -I 48  
```  
In questo caso DTA userà Query Store come origine del carico di lavoro e considererà solo le query eseguite nelle ultime 48 ore.  
  ||  
|-|  
|**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive.|  


  
 **-of** _output_script_file_name_  
 Specifica che **dta** scrive l'indicazione sotto forma di script [!INCLUDE[tsql](../../includes/tsql-md.md)] nel nome file e nella destinazione specificati.  
  
 È possibile usare **-F** con questa opzione. Assicurarsi che il nome file sia univoco, soprattutto se vengono specificate anche le opzioni **-or** e **-ox**.  
  
 **-or** _output_xml_report_file_name_  
 Specifica che **dta** scrive l'indicazione in un report di output in formato XML. Se si specifica un nome di file, le indicazioni vengono scritte in tale destinazione. In caso contrario, **dta** utilizza il nome di sessione per generare il nome file e lo scrive nella directory corrente.  
  
 È possibile usare **-F** con questa opzione. Assicurarsi che il nome file sia univoco, soprattutto se vengono specificate anche le opzioni **-of** e **-ox**.  
  
 **-ox** _output_XML_file_name_  
 Specifica che **dta** scrive l'indicazione sotto forma di file XML nel nome file e nella destinazione specificati. Assicurarsi che Ottimizzazione guidata motore di database disponga delle autorizzazioni di scrittura adeguate per la directory di destinazione.  
  
 È possibile usare **-F** con questa opzione. Assicurarsi che il nome file sia univoco, soprattutto se vengono specificate anche le opzioni **-of** e **-or**.  
  
 **-P** _password_  
 Specifica la password per l'ID di accesso. Se si omette questa opzione, **dta** richiede una password.  
  
 **-q**  
 Imposta la modalità non interattiva. Le informazioni, incluse quelle relative alle intestazioni e allo stato, non vengono scritte nella console.  
  
 **-rl** _analysis_report_list_  
 Specifica l'elenco dei report di analisi da generare. Nella tabella seguente sono riportati i valori che è possibile specificare per questo argomento.  
  
|valore|Report|  
|-----------|------------|  
|ALL|Tutti i report di analisi|  
|STMT_COST|Report costo istruzioni|  
|EVT_FREQ|Report frequenza eventi|  
|STMT_DET|Report dettagli istruzioni|  
|CUR_STMT_IDX|Report relazioni istruzioni-indici (configurazione corrente)|  
|REC_STMT_IDX|Report relazioni istruzioni-indici (configurazione consigliata)|  
|STMT_COSTRANGE|Report intervallo di costi istruzione|  
|CUR_IDX_USAGE|Report utilizzo indici (configurazione corrente)|  
|REC_IDX_USAGE|Report utilizzo indici (configurazione consigliata)|  
|CUR_IDX_DET|Report dettagli indici (configurazione corrente)|  
|REC_IDX_DET|Report dettagli indici (configurazione consigliata)|  
|VIW_TAB|Report relazioni viste-tabelle|  
|WKLD_ANL|Report analisi carico di lavoro|  
|DB_ACCESS|Report accessi a database|  
|TAB_ACCESS|Report accessi a tabelle|  
|COL_ACCESS|Report accessi a colonne|  
  
 Per specificare più report, separare i valori utilizzando la virgola, ad esempio:  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** _server_name_[ *\instance*]  
 Specifica il nome del computer e dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui connettersi. Se si omette *server_name* , **dta** si connette all'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer locale. Questa opzione è obbligatoria in caso di connessione a un'istanza denominata oppure quando si esegue **dta** da un computer remoto in rete.  
  
 **-s** _session_name_  
 Specifica il nome della sessione di ottimizzazione. Questa opzione è obbligatoria se non si specifica **-ID** .  
  
 **-Tf** _table_list_file_  
 Specifica il nome del file contenente l'elenco di tabelle da ottimizzare. Ogni tabella inclusa nel file deve iniziare su una nuova riga. Le tabelle devono essere qualificate con nomi di tabella composti da tre parti, ad esempio **AdventureWorks2012.HumanResources.Department**. In alternativa, per richiamare la funzionalità di ridimensionamento delle tabelle, il nome di una tabella esistente può essere seguito da un numero che indica il numero previsto di righe nella tabella. Ottimizzazione guidata motore di database utilizza il numero previsto di righe durante l'ottimizzazione o la valutazione delle istruzioni nel carico di lavoro che fanno riferimento a queste tabelle. Possono essere presenti uno o più spazi tra il numero di *number_of_rows* e *table_name*.  
  
 Il formato di file per *table_list_file* è:  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 Questo argomento rappresenta un'alternativa all'immissione di un elenco di tabelle al prompt dei comandi ( **-Tl**). Non usare un file contenente un elenco di tabelle ( **-Tf**) se si specifica **-Tl**. Se vengono utilizzati entrambi gli argomenti, l'esecuzione di **dta** ha esito negativo e viene restituito un errore.  
  
 Se si omettono gli argomenti **-Tf** e **-Tl** , tutte le tabelle utente nei database specificati verranno considerate per l'ottimizzazione.  
  
 **-Tl** _table_list_  
 Specifica al prompt dei comandi un elenco di tabelle da ottimizzare. Per separare i nomi di tabella, utilizzare la virgola. Se con l'argomento **-D** viene specificato solo un database, non è necessario che i nomi delle tabelle vengano qualificati con un nome di database. In caso contrario, per ogni tabella sarà necessario specificare il nome completo nel formato: *database_name.schema_name.table_name* .  
  
 Questo argomento rappresenta un'alternativa all'utilizzo di un file contenente un elenco di tabelle ( **-Tf**). Se vengono usati entrambi gli argomenti **-Tl** e **-Tf** , l'esecuzione di **dta** non riesce e viene restituito un errore.  
  
 **-U** _login_id_  
 Specifica l'ID di accesso utilizzato per connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **-u**  
 Avvia l'interfaccia utente di Ottimizzazione guidata motore di database. Tutti i parametri vengono considerati come impostazioni iniziali dell'interfaccia utente.  
  
 **-x**  
 Avvia la sessione di ottimizzazione e chiude l'utilità.  
  
## <a name="remarks"></a>Osservazioni  
 Premere CTRL+C una volta per arrestare la sessione di ottimizzazione e generare le indicazioni in base all'analisi completata da **dta** fino a quel momento. Verrà richiesto di decidere se generare le indicazioni o meno. Premere nuovamente CTRL+C per arrestare la sessione di ottimizzazione senza generare le indicazioni.  
  
## <a name="examples"></a>Esempi  
 **A. Ottimizzazione di un carico di lavoro che include indici e viste indicizzate nell'indicazione**  
  
 Questo esempio usa una connessione sicura (`-E`) per connettersi al database **tpcd1G** in MyServer per analizzare un carico di lavoro e creare indicazioni. Scrive l'output in un file di script denominato script.sql. Se script.sql esiste già, l'utilità **dta** sovrascriverà il file in quanto è stato specificato l'argomento `-F` . La sessione di ottimizzazione viene eseguita per un periodo illimitato di tempo per garantire l'analisi completa del carico di lavoro (`-A 0`). L'indicazione deve fornire un miglioramento minimo del 5% (`-m 5`). **dta** deve includere indici e viste indicizzate nell'indicazione finale (`-fa IDX_IV`).  
  
```  
dta -S MyServer -E -D tpcd1G -if tpcd_22.sql -F -of script.sql -A 0 -m 5 -fa IDX_IV  
```  
  
 **B. Limitazione dell'utilizzo del disco**  
  
 Nell'esempio seguente viene limitata la dimensione totale del database, che include i dati non elaborati e gli indici aggiuntivi, a 3 GB (`-B 3000`) e l'output viene reindirizzato su d:\result_dir\script1.sql. Il tempo di esecuzione non è maggiore di 1 ora (`-A 60`).  
  
```  
dta -D tpcd1G -if tpcd_22.sql -B 3000 -of "d:\result_dir\script1.sql" -A 60  
```  
  
 **C. Limitazione del numero di query ottimizzate**  
  
 Nell'esempio seguente il numero di query lette dal file orders_wkld.sql viene limitato a un massimo di 10 (`-n 10`) oppure il tempo di esecuzione viene limitato a 15 minuti (`-A 15`), a seconda di quale dei due eventi si verifica per primo. Per assicurarsi che tutte e 10 le query vengano ottimizzate, specificare un tempo di ottimizzazione illimitato tramite `-A 0`. Se il fattore tempo è rilevante, specificare un limite di tempo adeguato impostando il numero di minuti disponibili per l'ottimizzazione con l'argomento `-A` come illustrato nell'esempio seguente.  
  
```  
dta -D orders -if orders_wkld.sql -of script.sql -A 15 -n 10  
```  
  
 **D. Ottimizzazione di tabelle specifiche elencate in un file**  

 Questo esempio mostra l'utilizzo di *table_list_file* , ovvero l'argomento **-Tf** . Il contenuto del file table_list.txt è riportato di seguito.  

```
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```

Il contenuto di table_list.txt determina quanto segue:  

- Verranno ottimizzate solo le tabelle **Customer**, **Store** e **Product** .  
  
- Si presuppone che il numero di righe delle tabelle **Customer** e **Product** sia rispettivamente 100.000 e 2.000.000.  
  
- Si presuppone inoltre che il numero di righe della tabella **Store** sia il numero corrente di righe di questa tabella.  

    Possono essere presenti uno o più spazi tra il numero totale delle righe e il nome precedente della tabella in *table_list_file*.  
    
    La durata dell'ottimizzazione è pari a 2 ore (`-A 120`) e l'output viene scritto in un file XML (`-ox XMLTune.xml`).  

``` 
dta -D pubs -if pubs_wkld.sql -ox XMLTune.xml -A 120 -Tf table_list.txt  
``` 

## <a name="see-also"></a>Vedere anche

- [Guida di riferimento alle utilità del prompt dei comandi &#40;motore di database&#41;](../../tools/command-prompt-utility-reference-database-engine.md)
- [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)