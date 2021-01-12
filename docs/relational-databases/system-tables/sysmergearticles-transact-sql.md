---
description: sysmergearticles (Transact-SQL)
title: sysmergearticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergearticles
- sysmergearticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergearticles system table
ms.assetid: e9b1648e-4660-4688-9f56-18b2baf7228c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 50d2129741447444b1da2adc82521966a7045530
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096119"
---
# <a name="sysmergearticles-transact-sql"></a>sysmergearticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni articolo di merge definito nel database locale. Questa tabella è archiviata nel database di pubblicazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome dell'articolo.|  
|**type**|**tinyint**|Specifica il tipo di articolo. I possibili valori sono i seguenti:<br /><br /> **10** = tabella.<br /><br /> **32** = stored procedure (solo schema).<br /><br /> **64** = vista o vista indicizzata (solo schema).<br /><br /> **128** = funzione definita dall'utente (solo schema).<br /><br /> **160** = sinonimo (solo schema).|  
|**objid**|**int**|Identificatore dell'oggetto.|  
|**sync_objid**|**int**|ID di oggetto della vista che rappresenta il set di dati sincronizzato.|  
|**view_type**|**tinyint**|Tipo di vista:<br /><br /> **0** = non è una vista; utilizzare tutti gli oggetti di base.<br /><br /> **1** = visualizzazione permanente.<br /><br /> **2** = visualizzazione temporanea.|  
|**artid**|**uniqueidentifier**|Identificatore univoco per l'articolo specificato.|  
|**description**|**nvarchar(255)**|Breve descrizione dell'articolo.|  
|**pre_creation_command**|**tinyint**|Azione predefinita da eseguire quando viene creato l'articolo nel database di sottoscrizione:<br /><br /> **0 =** None: se la tabella esiste già nel Sottoscrittore, non viene eseguita alcuna azione.<br /><br /> **1** = Drop: Elimina la tabella prima di ricrearla.<br /><br /> **2** = Delete: rilascia un'eliminazione in base alla clausola WHERE nel filtro di subset.<br /><br /> **3** = troncamento: uguale a **2**, ma elimina pagine anziché righe. La clausola WHERE in questo caso non viene utilizzata.|  
|**pubid**|**uniqueidentifier**|ID della pubblicazione a cui appartiene l'articolo corrente.|  
|**Nickname**|**int**|Mapping di un nome alternativo per l'identificazione dell'articolo.|  
|**column_tracking**|**int**|Indica se il rilevamento a livello di colonna viene implementato per l'articolo.|  
|**Stato**|**tinyint**|Specifica lo stato dell'articolo. I possibili valori sono i seguenti:<br /><br /> **1** = non sincronizzato: lo script di elaborazione iniziale per la pubblicazione della tabella verrà eseguito alla successiva esecuzione del agente di snapshot.<br /><br /> **2** = attivo: lo script di elaborazione iniziale per la pubblicazione della tabella è stato eseguito.<br /><br /> **5** = New_inactive-da aggiungere.<br /><br /> **6** = New_active-da aggiungere.|  
|**conflict_table**|**sysname**|Nome della tabella locale che include i record in conflitto per l'articolo corrente. Lo scopo di questa tabella è esclusivamente informativo. Il contenuto può essere modificato o eliminato da routine di risoluzione dei conflitti personalizzate oppure direttamente dall'amministratore.|  
|**creation_script**|**nvarchar(255)**|Script per la creazione dell'articolo.|  
|**conflict_script**|**nvarchar(255)**|Script dei conflitti dell'articolo.|  
|**article_resolver**|**nvarchar(255)**|Sistema di risoluzione dei conflitti a livello di riga personalizzato per l'articolo.|  
|**ins_conflict_proc**|**sysname**|Procedura utilizzata per scrivere un conflitto **conflict_table**.|  
|**insert_proc**|**sysname**|Procedura utilizzata dal sistema di risoluzione dei conflitti predefinito per l'inserimento di righe durante la sincronizzazione.|  
|**update_proc**|**sysname**|Procedura utilizzata dal sistema di risoluzione dei conflitti predefinito per l'aggiornamento di righe durante la sincronizzazione.|  
|**select_proc**|**sysname**|Nome di una stored procedure generata automaticamente utilizzata dall'agente di merge per l'implementazione di blocchi e l'individuazione di righe e colonne per un articolo.|  
|**metadata_select_proc**|**sysname**|Nome della stored procedure generata automaticamente utilizzata per accedere a metadati nelle tabelle del sistema di replica di tipo merge.|  
|**delete_proc**|**sysname**|Procedura utilizzata dal sistema di risoluzione dei conflitti predefinito per l'eliminazione di righe durante la sincronizzazione.|  
|**schema_option**|**binario (8)**|Per i valori supportati di *schema_option*, vedere [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nome della tabella creata nel Sottoscrittore.|  
|**destination_owner**|**sysname**|Nome del proprietario dell'oggetto di destinazione.|  
|**resolver_clsid**|**nvarchar(50)**|ID del sistema di risoluzione dei conflitti personalizzato.|  
|**subset_filterclause**|**nvarchar (1000)**|Clausola di filtro per l'articolo.|  
|**missing_col_count**|**int**|Numero di colonne mancanti.|  
|**missing_cols**|**varbinary(128)**|Mappa di bit delle colonne mancanti.|  
|**excluded_cols**|**varbinary(128)**|Mappa di bit delle colonne escluse dall'articolo quando viene inviato al Sottoscrittore.|  
|**excluded_col_count**|**int**|Numero di colonne escluse.|  
|**colonne**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deleted_cols**|**varbinary(128)**|Mappe di bit delle colonne eliminate dalla tabella di origine.|  
|**resolver_info**|**nvarchar(255)**|Archivio per informazioni aggiuntive necessarie per il sistema di risoluzione dei conflitti personalizzato.|  
|**view_sel_proc**|**nvarchar (290)**|Nome di una stored procedure utilizzata dall'agente di merge per il popolamento iniziale di un articolo in una pubblicazione filtrata in modo dinamico e per l'enumerazione delle righe modificate in qualsiasi pubblicazione filtrata.|  
|**gen_cur**|**int**|Numero di generazione per modifiche locali della tabella di base di un articolo.|  
|**vertical_partition**|**int**|Specifica se in un articolo di tabella il filtraggio delle colonne è abilitato. **0** indica che non è presente alcun filtro verticale e che tutte le colonne vengono pubblicate.|  
|**identity_support**|**int**|Specifica se è abilitata la gestione automatica degli intervalli di valori Identity. **1** indica che la gestione degli intervalli di valori Identity è abilitata e **0** indica che non è disponibile alcun supporto per l'intervallo di valori Identity.|  
|**before_image_objid**|**int**|ID dell'oggetto tabella di rilevamento. La tabella di rilevamento contiene determinati valori di colonna chiave quando viene creata una pubblicazione con *\@ keep_partition_changes*  =  **true**.|  
|**before_view_objid**|**int**|ID di oggetto di una tabella di vista. La vista è relativa a una tabella in cui viene tenuto traccia se una riga appartiene a un Sottoscrittore specifico prima di essere eliminata o aggiornata. Si applica solo quando viene creata una pubblicazione con *\@ keep_partition_changes*  =  **true.**|  
|**verify_resolver_signature**|**int**|Specifica se una firma digitale viene verificata o meno prima dell'utilizzo di un sistema di risoluzione dei conflitti in una replica di tipo merge:<br /><br /> **0** = la firma non viene verificata.<br /><br /> **1** = la firma viene verificata per verificare se si tratta di una fonte attendibile.|  
|**allow_interactive_resolver**|**bit**|Specifica se per un articolo l'utilizzo del sistema di risoluzione dei conflitti interattivo è attivato. **1** specifica che l'articolo usa il sistema di risoluzione interattivo.|  
|**fast_multicol_updateproc**|**bit**|Specifica se l'agente di merge è stato attivato per l'applicazione di modifiche a più colonne della stessa riga tramite una sola istruzione UPDATE:<br /><br /> **0** = genera un aggiornamento separato per ogni colonna modificata.<br /><br /> **1** = rilascia un'istruzione Update che consente di eseguire gli aggiornamenti in più colonne in un'unica istruzione.|  
|**check_permissions**|**int**|Mappa di bit delle autorizzazioni a livello di tabella verificate quando l'agente di merge applica le modifiche nel server di pubblicazione. *check_permissions* possono avere uno dei valori seguenti:<br /><br /> **0x00 =** Le autorizzazioni non vengono controllate.<br /><br /> **0x10 =** Verifica le autorizzazioni nel server di pubblicazione prima che sia possibile caricare gli inserimenti eseguiti in un Sottoscrittore.<br /><br /> **0x20 =** Verifica le autorizzazioni nel server di pubblicazione prima che sia possibile caricare gli aggiornamenti eseguiti in un Sottoscrittore.<br /><br /> **0x40 =** Verifica le autorizzazioni nel server di pubblicazione prima che sia possibile caricare le eliminazioni eseguite in un Sottoscrittore.|  
|**maxversion_at_cleanup**|**int**|La generazione con il valore più alto per cui i metadati vengono rimossi.|  
|**processing_order**|**int**|Indica l'ordine di elaborazione degli articoli in una pubblicazione di tipo merge. dove il valore **0** indica che l'articolo non è ordinato e che gli articoli vengono elaborati in ordine dal valore più basso a quello più alto. Se due articoli hanno lo stesso valore, essi vengono elaborati simultaneamente. Per altre informazioni, vedere [Specificare le proprietà della replica di tipo merge](../../relational-databases/replication/merge/specify-merge-replication-properties.md).|  
|**upload_options**|**tinyint**|Imposta le restrizioni per gli aggiornamenti eseguiti in un Sottoscrittore con una sottoscrizione client. I possibili valori sono i seguenti.<br /><br /> **0** = non sono previste restrizioni per gli aggiornamenti eseguiti in un Sottoscrittore con una sottoscrizione client. tutte le modifiche vengono caricate nel server di pubblicazione.<br /><br /> **1** = le modifiche sono consentite in un Sottoscrittore con una sottoscrizione client, ma non vengono caricate nel server di pubblicazione.<br /><br /> **2** = le modifiche non sono consentite in un Sottoscrittore con una sottoscrizione client.<br /><br /> Per altre informazioni, vedere [Ottimizzare le prestazioni della replica di tipo merge con gli articoli di solo download](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).|  
|**published_in_tran_pub**|**bit**|Indica che un articolo in una pubblicazione di tipo merge viene pubblicato anche in una pubblicazione transazionale.<br /><br /> **0** = l'articolo non è pubblicato in un articolo transazionale.<br /><br /> **1** = l'articolo è pubblicato anche in un articolo transazionale.|  
|**leggero**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**procname_postfix**|**nchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**well_partitioned_lightweight**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**before_upd_view_objid**|**int**|Da aggiungere.|  
|**delete_tracking**|**bit**|Indica se viene eseguita la replica delle eliminazioni.<br /><br /> **0** = le eliminazioni non vengono replicate<br /><br /> **1** = le eliminazioni vengono replicate, ovvero il comportamento predefinito per la replica di tipo merge.<br /><br /> Quando il valore di *delete_tracking* è **0**, le righe eliminate nel Sottoscrittore devono essere rimosse manualmente nel server di pubblicazione e le righe eliminate nel server di pubblicazione devono essere rimosse manualmente nel Sottoscrittore.<br /><br /> Nota: un valore pari a **0** comporta la non convergenza.|  
|**compensate_for_errors**|**bit**|Indica se vengono eseguite azioni di compensazione quando si verificano errori durante la sincronizzazione.<br /><br /> **0** = le azioni di compensazione sono disabilitate.<br /><br /> **1** = le modifiche che non possono essere applicate a un Sottoscrittore o a un server di pubblicazione comportano sempre azioni di compensazione per annullare queste modifiche, che è il comportamento predefinito per la replica di tipo merge.<br /><br /> Nota: un valore pari a **0** comporta la non convergenza.|  
|**pub_range**|**bigint**|Dimensioni dell'intervallo di valori Identity del server di pubblicazione.|  
|**range**|**bigint**|Dimensioni dei valori Identity consecutivi che verrebbero assegnati nei Sottoscrittori durante un intervento di regolazione.|  
|**threshold**|**int**|Percentuale di soglia dell'intervallo di valori Identity.|  
|**stream_blob_columns**|**bit**|Specifica se viene utilizzata l'ottimizzazione del flusso di dati per la replica di colonne BLOB (Binary Large Object). **1** indica che viene eseguito un tentativo di ottimizzazione.|  
|**preserve_rowguidcol**|**bit**|Indica se la replica utilizza una colonna rowguid esistente. Il valore **1** indica che viene utilizzata una colonna ROWGUIDCOL esistente. **0** indica che la replica ha aggiunto la colonna ROWGUIDCOL.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergearticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)  
  
  
