---
description: sysmergepartitioninfoview (Transact-SQL)
title: sysmergepartitioninfoview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergepartitioninfoview
- sysmergepartitioninfoview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfoview view
ms.assetid: 714e2935-1bc7-4901-aea2-64b1bbda03d6
author: stevestein
ms.author: sstein
ms.openlocfilehash: f9a5063691cb0d8ed77fdc19ce3bba1adebf8858
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181579"
---
# <a name="sysmergepartitioninfoview-transact-sql"></a>sysmergepartitioninfoview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **sysmergepartitioninfoview** espone le informazioni di partizionamento per gli articoli di tabella. Questa vista è archiviata nel database di pubblicazione del server di pubblicazione e nel database di sottoscrizione del Sottoscrittore.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome dell'articolo.|  
|**type**|**tinyint**|Specifica il tipo di articolo. I possibili valori sono i seguenti:<br /><br /> **0x0A** = tabella.<br /><br /> **0x20** = solo schema di procedura.<br /><br /> **0x40** = solo schema della vista o solo schema della vista indicizzata.<br /><br /> **0x80** = solo schema di funzione.|  
|**objid**|**int**|Identificatore dell'oggetto pubblicato.|  
|**sync_objid**|**int**|ID di oggetto della vista che rappresenta il set di dati sincronizzato.|  
|**view_type**|**tinyint**|Tipo di vista:<br /><br /> **0** = non è una vista; utilizzare tutti gli oggetti di base.<br /><br /> **1** = visualizzazione permanente.<br /><br /> **2** = visualizzazione temporanea.|  
|**artid**|**uniqueidentifier**|Identificatore univoco per l'articolo specificato.|  
|**description**|**nvarchar(255)**|Breve descrizione dell'articolo.|  
|**pre_creation_command**|**tinyint**|Azione predefinita da eseguire quando viene creato l'articolo nel database di sottoscrizione:<br /><br /> **0** = nessuna: se la tabella esiste già nel Sottoscrittore, non viene eseguita alcuna azione.<br /><br /> **1** = Drop: Elimina la tabella prima di ricrearla.<br /><br /> **2** = Delete: rilascia un'eliminazione in base alla clausola WHERE nel filtro di subset.<br /><br /> **3** = troncamento: uguale a 2, ma elimina pagine anziché righe. La clausola WHERE in questo caso non viene utilizzata.|  
|**pubid**|**uniqueidentifier**|ID della pubblicazione a cui appartiene l'articolo corrente.|  
|**Nickname**|**int**|Mapping di un nome alternativo per l'identificazione dell'articolo.|  
|**column_tracking**|**int**|Indica se viene implementato il rilevamento a livello di colonna per l'articolo.|  
|**Stato**|**tinyint**|Specifica lo stato dell'articolo. I possibili valori sono i seguenti:<br /><br /> **1** = non sincronizzato: lo script di elaborazione iniziale per la pubblicazione della tabella verrà eseguito alla successiva esecuzione del agente di snapshot.<br /><br /> **2** = attivo: lo script di elaborazione iniziale per la pubblicazione della tabella è stato eseguito.|  
|**conflict_table**|**sysname**|Nome della tabella locale che include i record in conflitto per l'articolo corrente. Lo scopo di questa tabella è esclusivamente informativo. Il contenuto può essere modificato o eliminato da routine di risoluzione dei conflitti personalizzate oppure direttamente dall'amministratore.|  
|**creation_script**|**nvarchar(255)**|Script per la creazione dell'articolo.|  
|**conflict_script**|**nvarchar(255)**|Script dei conflitti dell'articolo.|  
|**article_resolver**|**nvarchar(255)**|Sistema di risoluzione dei conflitti dell'articolo.|  
|**ins_conflict_proc**|**sysname**|Procedura utilizzata per scrivere le informazioni sui conflitti nella tabella con conflitti.|  
|**insert_proc**|**sysname**|Procedura utilizzata per inserire le righe durante la sincronizzazione.|  
|**update_proc**|**sysname**|Procedura utilizzata per aggiornare le righe durante la sincronizzazione.|  
|**select_proc**|**sysname**|Nome di una stored procedure generata automaticamente utilizzata dall'agente di merge per l'implementazione del blocco e l'individuazione delle righe e colonne per un articolo.|  
|**metadata_select_proc**|**sysname**|Nome della stored procedure generata automaticamente utilizzata per accedere ai metadati nelle tabelle di sistema della replica di tipo merge.|  
|**delete_proc**|**sysname**|Procedura utilizzata per eliminare le righe durante la sincronizzazione.|  
|**schema_option**|**binario (8)**|Mappa di bit dell'opzione di creazione dello schema per l'articolo specificato. Per informazioni sui valori di *schema_option* supportati, vedere [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nome della tabella creata nel Sottoscrittore.|  
|**destination_owner**|**sysname**|Nome del proprietario dell'oggetto di destinazione.|  
|**resolver_clsid**|**nvarchar(50)**|ID del sistema di risoluzione dei conflitti personalizzato. Per un gestore della logica di business questo valore è NULL.|  
|**subset_filterclause**|**nvarchar (1000)**|Clausola di filtro per l'articolo.|  
|**missing_col_count**|**int**|Numero di colonne pubblicate mancanti nell'articolo.|  
|**missing_cols**|**varbinary(128)**|Mappa di bit che descrive le colonne mancanti nell'articolo.|  
|**excluded_cols**|**varbinary(128)**|Mappa di bit delle colonne escluse dall'articolo.|  
|**excluded_col_count**|**int**|Numero di colonne escluse dall'articolo.|  
|**colonne**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deleted_cols**|**varbinary(128)**|Mappa di bit che descrive le colonne eliminate dall'articolo.|  
|**resolver_info**|**nvarchar(255)**|Archivio per informazioni aggiuntive necessarie per il sistema di risoluzione dei conflitti personalizzato.|  
|**view_sel_proc**|**nvarchar (290)**|Nome di una stored procedure utilizzata dall'agente di merge per il popolamento iniziale di un articolo in una pubblicazione filtrata in modo dinamico e per l'enumerazione delle righe modificate in qualsiasi pubblicazione filtrata.|  
|**gen_cur**|**bigint**|Genera il numero di modifica locale per la tabella di base di un articolo.|  
|**vertical_partition**|**int**|Specifica se in un articolo di tabella il filtraggio delle colonne è abilitato. **0** indica che non è presente alcun filtro verticale e che tutte le colonne vengono pubblicate.|  
|**identity_support**|**int**|Specifica se è abilitata la gestione automatica degli intervalli di valori Identity. **1** indica che la gestione degli intervalli di valori Identity è abilitata e **0** indica che non è disponibile alcun supporto per l'intervallo di valori Identity.|  
|**before_image_objid**|**int**|ID dell'oggetto tabella di rilevamento. La tabella di rilevamento include valori di colonna chiave specifici se per la pubblicazione sono state abilitate le ottimizzazioni delle modifiche delle partizioni.|  
|**before_view_objid**|**int**|ID di oggetto di una tabella di vista. La vista è relativa a una tabella in cui viene tenuto traccia se una riga appartiene a un Sottoscrittore specifico prima di essere eliminata o aggiornata. Valido solo se per la pubblicazione sono state abilitate le ottimizzazioni delle modifiche delle partizioni.|  
|**verify_resolver_signature**|**int**|Specifica se una firma digitale viene verificata o meno prima dell'utilizzo di un sistema di risoluzione dei conflitti in una replica di tipo merge:<br /><br /> **0** = la firma non viene verificata.<br /><br /> **1** = la firma viene verificata per verificare se si tratta di una fonte attendibile.|  
|**allow_interactive_resolver**|**bit**|Specifica se per un articolo l'utilizzo del sistema di risoluzione dei conflitti interattivo è attivato. **1** indica che è possibile utilizzare il sistema di risoluzione interattivo per l'articolo.|  
|**fast_multicol_updateproc**|**bit**|Specifica se l'agente di merge è stato attivato per l'applicazione di modifiche a più colonne della stessa riga tramite una sola istruzione UPDATE:<br /><br /> **0** = genera un aggiornamento separato per ogni colonna modificata.<br /><br /> **1** = emesso sull'istruzione Update che fa sì che gli aggiornamenti vengano eseguiti in più colonne in un'unica istruzione.|  
|**check_permissions**|**int**|Mappa di bit delle autorizzazioni a livello di tabella che verranno verificate quando l'agente di merge applicherà le modifiche nel server di pubblicazione. *check_permissions* possono avere uno dei valori seguenti:<br /><br /> **0x00** = le autorizzazioni non vengono controllate.<br /><br /> **0x10** = verifica le autorizzazioni nel server di pubblicazione prima che sia possibile caricare gli inserimenti in un Sottoscrittore.<br /><br /> **0x20** = verifica le autorizzazioni nel server di pubblicazione prima che sia possibile caricare gli aggiornamenti eseguiti in un Sottoscrittore.<br /><br /> **0x40** = verifica le autorizzazioni nel server di pubblicazione prima che sia possibile caricare le eliminazioni eseguite in un Sottoscrittore.|  
|**maxversion_at_cleanup**|**int**|Numero massimo di generazioni rimosse alla successiva esecuzione dell'agente di merge.|  
|**processing_order**|**int**|Indica l'ordine di elaborazione degli articoli in una pubblicazione di tipo merge. dove il valore **0** indica che l'articolo non è ordinato e che gli articoli vengono elaborati in ordine dal valore più basso a quello più alto. Se due articoli hanno lo stesso valore, essi vengono elaborati simultaneamente. Per altre informazioni, vedere [Specificare le proprietà della replica di tipo merge](../../relational-databases/replication/merge/specify-merge-replication-properties.md).|  
|**upload_options**|**tinyint**|Specifica se è possibile apportare modifiche nel Sottoscrittore o caricare modifiche dal Sottoscrittore. I possibili valori sono i seguenti.<br /><br /> **0** = non sono previste restrizioni per gli aggiornamenti eseguiti nel Sottoscrittore. tutte le modifiche vengono caricate nel server di pubblicazione.<br /><br /> **1** = le modifiche sono consentite nel Sottoscrittore, ma non vengono caricate nel server di pubblicazione.<br /><br /> **2** = le modifiche non sono consentite nel Sottoscrittore.|  
|**published_in_tran_pub**|**bit**|Indica che un articolo in una pubblicazione di tipo merge viene pubblicato anche in una pubblicazione transazionale.<br /><br /> **0** = l'articolo non è pubblicato in un articolo transazionale.<br /><br /> **1** = l'articolo è pubblicato anche in un articolo transazionale.|  
|**leggero**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**procname_postfix**|**nchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**well_partitioned_lightweight**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**before_upd_view_objid**|**int**|ID della vista della tabella prima degli aggiornamenti.|  
|**delete_tracking**|**bit**|Specifica se le eliminazioni vengono replicate.<br /><br /> **0** = le eliminazioni non vengono replicate.<br /><br /> **1** = le eliminazioni vengono replicate, ovvero il comportamento predefinito per la replica di tipo merge.<br /><br /> Quando il valore di *delete_tracking* è **0**, le righe eliminate nel Sottoscrittore devono essere rimosse manualmente nel server di pubblicazione e le righe eliminate nel server di pubblicazione devono essere rimosse manualmente nel Sottoscrittore.<br /><br /> Nota: un valore pari a **0** comporta la non convergenza.|  
|**compensate_for_errors**|**bit**|Specifica se devono essere eseguite azioni di compensazione quando vengono rilevati errori durante la sincronizzazione.<br /><br /> **0** = le azioni di compensazione sono disabilitate.<br /><br /> **1** = le modifiche che non possono essere applicate a un Sottoscrittore o a un server di pubblicazione comportano sempre azioni di compensazione per annullare queste modifiche, che è il comportamento predefinito per la replica di tipo merge.<br /><br /> Nota: un valore pari a **0** comporta la non convergenza.|  
|**pub_range**|**bigint**|Dimensioni dell'intervallo di valori Identity del server di pubblicazione.|  
|**range**|**bigint**|Dimensioni dei valori Identity consecutivi che verrebbero assegnati nei Sottoscrittori durante un intervento di regolazione.|  
|**threshold**|**int**|Percentuale di soglia dell'intervallo di valori Identity.|  
|**stream_blob_columns**|**bit**|Indica se per le colonne BLOB (Binary Large Object) viene utilizzata l'ottimizzazione del flusso. **1** indica che viene eseguito un tentativo di ottimizzazione.|  
|**preserve_rowguidcol**|**bit**|Specifica se per la replica viene utilizzata una colonna rowguid esistente. Il valore **1** indica che viene utilizzata una colonna ROWGUIDCOL esistente. **0** indica che la replica ha aggiunto la colonna ROWGUIDCOL.|  
|**partition_view_id**|**int**|Identifica la vista che definisce una partizione del Sottoscrittore.|  
|**repl_view_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**partition_deleted_view_rule**|**sysname**|Istruzione utilizzata all'interno di un trigger di replica di tipo merge per recuperare l'ID di partizione per ogni riga eliminata o aggiornata in base ai relativi valori di colonna precedenti.|  
|**partition_inserted_view_rule**|**Sysname**|Istruzione utilizzata all'interno di un trigger di replica di tipo merge per recuperare l'ID di partizione per ogni riga inserita o aggiornata in base ai relativi nuovi valori di colonna.|  
|**membership_eval_proc_name**|**sysname**|Nome della procedura che valuta gli ID di partizione correnti delle righe in [MSmerge_contents &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/msmerge-contents-transact-sql.md).|  
|**column_list**|**sysname**|Elenco separato da virgole delle colonne pubblicate in un articolo.|  
|**column_list_blob**|**sysname**|Elenco separato da virgole delle colonne pubblicate in un articolo, comprese le colonne BLOB.|  
|**expand_proc**|**sysname**|Nome della procedura che restituisce nuovamente gli ID di partizione per tutte le righe figlio di una riga padre appena inserita e per le righe padre sottoposte a modifica a livello di partizione oppure che sono state eliminate.|  
|**logical_record_parent_nickname**|**int**|Nome alternativo del padre di livello principale di un articolo specifico in un record logico.|  
|**logical_record_view**|**int**|Vista che ha come output la colonna rowguid dell'articolo padre di livello principale corrispondente a ogni colonna rowguid figlio.|  
|**logical_record_deleted_view_rule**|**sysname**|Simile a **logical_record_view**, ad eccezione del fatto che visualizza le righe figlio nella tabella "Deleted" nei trigger UPDATE e DELETE.|  
|**logical_record_level_conflict_detection**|**bit**|Indica se è necessario rilevare i conflitti a livello di record logico oppure a livello di riga o colonna.<br /><br /> **0** = viene utilizzato il rilevamento dei conflitti a livello di riga o di colonna.<br /><br /> **1** = viene utilizzato il rilevamento dei conflitti tra record logici, in cui una modifica in una riga del server di pubblicazione e la modifica in una riga distinta lo stesso record logico nel Sottoscrittore viene gestita come un conflitto.<br /><br /> Se questo valore è 1, è possibile utilizzare solo la risoluzione dei conflitti a livello di record logico.|  
|**logical_record_level_conflict_resolution**|**bit**|Indica se è necessario risolvere i conflitti a livello di record logico oppure a livello di riga o colonna.<br /><br /> **0** = viene utilizzata la risoluzione a livello di riga o di colonna.<br /><br /> **1** = in caso di conflitto, l'intero record logico del vincitore sovrascrive l'intero record logico sul lato perduto.<br /><br /> È possibile utilizzare il valore 1 sia con il rilevamento a livello di record logico che con il rilevamento a livello di riga o colonna.|  
|**partition_options**|**tinyint**|Definisce il modo in cui vengono partizionati i dati nell'articolo. Ciò consente di ottimizzare le prestazioni se tutte le righe appartengono a un'unica partizione o a un'unica sottoscrizione. Il *partition_options* può essere uno dei valori seguenti.<br /><br /> **0** = il filtro per l'articolo è statico oppure non restituisce un subset univoco di dati per ogni partizione, ovvero una partizione "sovrapposta".<br /><br /> **1** = le partizioni sono sovrapposte e gli aggiornamenti DML eseguiti nel Sottoscrittore non possono modificare la partizione a cui appartiene una riga.<br /><br /> **2** = il filtro per l'articolo restituisce partizioni non sovrapposte, ma più sottoscrittori possono ricevere la stessa partizione.<br /><br /> **3** = il filtro per l'articolo restituisce partizioni non sovrapposte univoche per ogni sottoscrizione.|  
|**nome**|**sysname**|Nome di una partizione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire le partizioni per una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepartition &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_helpmergepartition &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md)  
  
  
