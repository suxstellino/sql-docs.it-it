---
title: sp_addarticle (Transact-SQL) | Microsoft Docs
description: Viene creato un articolo che viene aggiunto a una pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addarticle
- sp_addarticle_TSQL
helpviewer_keywords:
- sp_addarticle
ms.assetid: 0483a157-e403-4fdb-b943-23c1b487bef0
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 5ee845eac866c3954ba03080c037179fa76ad519
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182838"
---
# <a name="sp_addarticle-transact-sql"></a>sp_addarticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Viene creato un articolo che viene aggiunto a una pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addarticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
    [ , [ @source_table = ] 'source_table' ]  
    [ , [ @destination_table = ] 'destination_table' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @type = ] 'type' ]   
    [ , [ @filter = ] 'filter' ]   
    [ , [ @sync_object= ] 'sync_object' ]   
        [ , [ @ins_cmd = ] 'ins_cmd' ]   
    [ , [ @del_cmd = ] 'del_cmd' ]   
        [ , [ @upd_cmd = ] 'upd_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @filter_clause = ] 'filter_clause' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @status = ] status ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @sync_object_owner = ] 'sync_object_owner' ]   
    [ , [ @filter_owner = ] 'filter_owner' ]   
    [ , [ @source_object = ] 'source_object' ]   
    [ , [ @artid = ] article_ID  OUTPUT ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @use_default_datatypes = ] use_default_datatypes  
    [ , [ @identityrangemanagementoption = ] identityrangemanagementoption ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione contenente l'articolo. Deve essere un nome univoco all'interno del database. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo. Deve essere un nome univoco all'interno della pubblicazione. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @source_table = ] 'source_table'` Questo parametro è stato deprecato. in alternativa, usare *source_object* .  
  
 *Questo parametro non è supportato per i Publisher Oracle.*  
  
`[ @destination_table = ] 'destination_table'` Nome della tabella di destinazione (sottoscrizione), se diversa da *source_table* o stored procedure. *destination_table* è di **tipo sysname** e il valore predefinito è null, il che significa che *source_table* è uguale a *destination_table * *.*  
  
`[ @vertical_partition = ] 'vertical_partition'` Abilita e Disabilita l'applicazione di filtri alle colonne in un articolo di tabella. *vertical_partition* è di **nchar (5)** e il valore predefinito è false.  
  
 **false** indica che non è presente alcun filtro verticale e che tutte le colonne vengono pubblicate.  
  
 **true** Cancella tutte le colonne ad eccezione della chiave primaria dichiarata, le colonne che ammettono valori null senza valori predefiniti e colonne chiave univoche. Le colonne vengono aggiunte utilizzando [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md).  
  
`[ @type = ] 'type'` Tipo di articolo. *Type* è di tipo **sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**aggregate schema only**|Funzione di aggregazione con solo schema.|  
|**func schema only**|Funzione con solo schema.|  
|**indexed view logbased**|Articolo di vista indicizzata basato su log. Questa proprietà non è supportata per server di pubblicazione Oracle. Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base.|  
|**indexed view logbased manualboth**|Articolo di vista indicizzata basato su log con filtro manuale e vista manuale. Per questa opzione è necessario specificare sia *sync_object* che parametri di *filtro* . Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**indexed view logbased manualfilter**|Articolo di vista indicizzata basato su log con filtro manuale. Per questa opzione è necessario specificare sia *sync_object* che parametri di *filtro* . Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**indexed view logbased manualview**|Articolo di vista indicizzata basato su log con vista manuale. Per questa opzione è necessario specificare il parametro *sync_object* . Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**indexed view schema only**|Vista indicizzata con solo schema. Per questo tipo di articolo è necessario pubblicare anche la tabella di base.|  
|**logbased** (impostazione predefinita)|Articolo basato su un log.|  
|**logbased manualboth**|Articolo basato su log con filtro manuale e vista manuale. Per questa opzione è necessario specificare sia *sync_object* che parametri di *filtro* . Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**logbased manualfilter**|Articolo basato su log con filtro manuale. Per questa opzione è necessario specificare sia *sync_object* che parametri di *filtro* . Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**logbased manualview**|Articolo basato su log con vista manuale. Per questa opzione è necessario specificare il parametro *sync_object* . Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**proc exec**|Replica l'esecuzione della stored procedure in tutti i Sottoscrittori dell'articolo. Questa proprietà non è supportata per server di pubblicazione Oracle. Si consiglia di utilizzare l'opzione **Serializable proc exec** anziché **proc exec**. Per ulteriori informazioni, vedere la sezione relativa ai tipi di articoli di esecuzione delle stored procedure in [pubblicazione dell'esecuzione di stored procedure nella replica transazionale](../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md). Non disponibile se l'acquisizione dati delle modifiche è abilitata.|  
|**proc schema only**|Procedura con solo schema. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**serializable proc exec**|Replica l'esecuzione della stored procedure solo se viene eseguita nel contesto di una transazione serializzabile. Questa proprietà non è supportata per server di pubblicazione Oracle.<br /><br /> Per permettere la replica dell'esecuzione della procedura, questa procedura deve essere eseguita anche all'interno di una transazione esplicita.|  
|**view schema only**|Vista con solo schema. Questa proprietà non è supportata per server di pubblicazione Oracle. Se si usa questa opzione, è necessario pubblicare anche la tabella di base.|  
  
`[ @filter = ] 'filter'` Stored procedure (creato con per la replica) utilizzato per filtrare orizzontalmente la tabella. *Filter* è di **tipo nvarchar (386)** e il valore predefinito è null. per creare la visualizzazione e filtrare stored procedure, è necessario eseguire manualmente [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) e [sp_articlefilter](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) . Se diverso da NULL, la procedura di filtro non viene creata, in quanto si presume che venga creata manualmente.  
  
`[ @sync_object = ] 'sync_object'` Nome della tabella o della vista utilizzata per la generazione del file di dati utilizzato per rappresentare lo snapshot di questo articolo. *sync_object* è di **tipo nvarchar (386)** e il valore predefinito è null. Se è NULL, viene chiamato [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) per creare automaticamente la vista utilizzata per generare il file di output. Questo errore si verifica dopo l'aggiunta di colonne con [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Se il valore è diverso da NULL, non viene creata alcuna vista, in quanto si presume che venga creata manualmente.  
  
`[ @ins_cmd = ] 'ins_cmd'` Tipo di comando di replica utilizzato per la replica di inserimenti per questo articolo. *ins_cmd* è di **tipo nvarchar (255)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**NONE**|Nessuna operazione.|  
|**CALL sp_MSins_**<br /> **_Table_**  (impostazione predefinita)<br /><br /> -oppure-<br /><br /> **CALL custom_stored_procedure_name**|Chiama una stored procedure per l'esecuzione nel Sottoscrittore. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo. *custom_stored_procedure* è il nome di un stored procedure creato dall'utente. <strong>sp_MSins_ *tabella*</strong> contiene il nome della tabella di destinazione al posto della *_table* parte del parametro. Quando si specifica *destination_owner* , viene anteposto al nome della tabella di destinazione. Per la tabella **ProductCategory** di proprietà dello schema **Production** nel Sottoscrittore, ad esempio, il parametro è `CALL sp_MSins_ProductionProductCategory` . Per un articolo di una topologia di replica peer-to-peer, *_table* viene accodato con un valore GUID. La specifica di *custom_stored_procedure* non è supportata per l'aggiornamento dei sottoscrittori.|  
|**SQL** o null|Replica un'istruzione INSERT. All'istruzione INSERT vengono passati i valori per tutte le colonne pubblicate nell'articolo. Questo comando viene replicato quando si eseguono operazioni di inserimento:<br /><br /> `INSERT INTO <table name> VALUES (c1value, c2value, c3value, ..., cnvalue)`|  
  
 Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @del_cmd = ] 'del_cmd'` Tipo di comando di replica utilizzato per la replica delle eliminazioni per questo articolo. *del_cmd* è di **tipo nvarchar (255)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**NONE**|Nessuna operazione.|  
|**CALLsp_MSdel_**<br /> **_Table_**  (impostazione predefinita)<br /><br /> -oppure-<br /><br /> **CALL custom_stored_procedure_name**|Chiama una stored procedure per l'esecuzione nel Sottoscrittore. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo. *custom_stored_procedure* è il nome di un stored procedure creato dall'utente. <strong>sp_MSdel_ *tabella*</strong> contiene il nome della tabella di destinazione al posto della *_table* parte del parametro. Quando si specifica *destination_owner* , viene anteposto al nome della tabella di destinazione. Per la tabella **ProductCategory** di proprietà dello schema **Production** nel Sottoscrittore, ad esempio, il parametro è `CALL sp_MSdel_ProductionProductCategory` . Per un articolo di una topologia di replica peer-to-peer, *_table* viene accodato con un valore GUID. La specifica di *custom_stored_procedure* non è supportata per l'aggiornamento dei sottoscrittori.|  
|**XCALL sp_MSdel_**<br /> **_tavolo_**<br /><br /> -oppure-<br /><br /> **XCALL custom_stored_procedure_name**|Chiama una stored procedure che accetta i parametri di stile XCALL. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo. L'impostazione di una stored procedure creata dall'utente non è consentita per l'aggiornamento dei Sottoscrittori.|  
|**SQL** o null|Replica un'istruzione DELETE. All'istruzione DELETE vengono passati tutti i valori relativi alle colonne chiave primaria. Questo comando viene replicato quando si eseguono operazioni di eliminazione:<br /><br /> `DELETE FROM <table name> WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
 Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @upd_cmd = ] 'upd_cmd'` Tipo di comando di replica utilizzato per la replica degli aggiornamenti per questo articolo. *upd_cmd* è di **tipo nvarchar (255)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**NONE**|Nessuna operazione.|  
|**CALL sp_MSupd_**<br /> **_tavolo_**<br /><br /> -oppure-<br /><br /> **CALL custom_stored_procedure_name**|Chiama una stored procedure per l'esecuzione nel Sottoscrittore. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo.|  
|**MCALL sp_MSupd_**<br /> **_tavolo_**<br /><br /> -oppure-<br /><br /> **MCALL custom_stored_procedure_name**|Chiama una stored procedure che accetta i parametri di stile MCALL. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo. *custom_stored_procedure* è il nome di un stored procedure creato dall'utente. <strong>sp_MSupd_ *tabella*</strong> contiene il nome della tabella di destinazione al posto della *_table* parte del parametro. Quando si specifica *destination_owner* , viene anteposto al nome della tabella di destinazione. Per la tabella **ProductCategory** di proprietà dello schema **Production** nel Sottoscrittore, ad esempio, il parametro è `MCALL sp_MSupd_ProductionProductCategory` . Per un articolo di una topologia di replica peer-to-peer, *_table* viene accodato con un valore GUID. L'impostazione di una stored procedure creata dall'utente non è consentita per l'aggiornamento dei Sottoscrittori.|  
|**SCALL sp_MSupd_**<br /> **_Table_**  (impostazione predefinita)<br /><br /> -oppure-<br /><br /> **SCALL custom_stored_procedure_name**|Chiama una stored procedure che accetta i parametri di stile SCALL. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo. *custom_stored_procedure* è il nome di un stored procedure creato dall'utente. <strong>sp_MSupd_ *tabella*</strong> contiene il nome della tabella di destinazione al posto della *_table* parte del parametro. Quando si specifica *destination_owner* , viene anteposto al nome della tabella di destinazione. Per la tabella **ProductCategory** di proprietà dello schema **Production** nel Sottoscrittore, ad esempio, il parametro è `SCALL sp_MSupd_ProductionProductCategory` . Per un articolo di una topologia di replica peer-to-peer, *_table* viene accodato con un valore GUID. L'impostazione di una stored procedure creata dall'utente non è consentita per l'aggiornamento dei Sottoscrittori.|  
|**XCALL sp_MSupd_**<br /> **_tavolo_**<br /><br /> -oppure-<br /><br /> **XCALL custom_stored_procedure_name**|Chiama una stored procedure che accetta i parametri di stile XCALL. Per utilizzare questo metodo di replica, utilizzare *schema_option* per specificare la creazione automatica del stored procedure oppure creare il stored procedure specificato nel database di destinazione di ogni sottoscrittore dell'articolo. L'impostazione di una stored procedure creata dall'utente non è consentita per l'aggiornamento dei Sottoscrittori.|  
|**SQL** o null|Replica un'istruzione UPDATE. All'istruzione UPDATE vengono passati tutti i valori delle colonne e i valori delle colonne chiave primaria. Questo comando viene replicato quando si eseguono operazioni di aggiornamento:<br /><br /> `UPDATE <table name> SET c1 = c1value, SET c2 = c2value, SET cn = cnvalue WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
> [!NOTE]  
>  Nel Sottoscrittore vengono propagate quantità di dati diverse a seconda che si utilizzi la sintassi CALL, MCALL o XCALL. Tramite la sintassi CALL vengono passati tutti i valori di tutte le colonne inserite ed eliminate, tramite la sintassi di SCALL vengono passati solo i valori delle colonne modificate, mentre tramite la sintassi XCALL vengono passati i valori di tutte le colonne, modificate o meno, insieme al valore precedente della colonna. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @creation_script = ] 'creation_script'` Percorso e nome di uno script facoltativo dello schema dell'articolo utilizzato per creare l'articolo nel database di sottoscrizione. *creation_script* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @description = ] 'description'` È una voce descrittiva per l'articolo. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` Specifica le operazioni che il sistema deve eseguire se rileva un oggetto esistente con lo stesso nome nel Sottoscrittore durante l'applicazione dello snapshot per questo articolo. *pre_creation_cmd* è di **tipo nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**nessuna**|Non utilizza alcun comando.|  
|**delete**|Elimina i dati dalla tabella di destinazione prima di applicare lo snapshot. Se l'articolo è filtrato in modo orizzontale, vengono eliminati solo i dati nelle colonne specificate dalla clausola di filtro. Valore non supportato per i server di pubblicazione Oracle quando è definito un filtro orizzontale.|  
|**Drop** (impostazione predefinita)|Rimuove la tabella di destinazione.|  
|**truncate**|Tronca la tabella di destinazione. Questo valore non è valido per i Sottoscrittori ODBC o OLE DB.|  
  
`[ @filter_clause = ] 'filter_clause'` Clausola di restrizione (WHERE) che definisce un filtro orizzontale. Quando si specifica la clausola di restrizione, omettere la parola chiave WHERE. *filter_clause* è di tipo **ntext** e il valore predefinito è null. Per altre informazioni, vedere [Filtrare i dati pubblicati](../../relational-databases/replication/publish/filter-published-data.md).  
  
`[ @schema_option = ] schema_option` Maschera di maschera dell'opzione di generazione dello schema per l'articolo specificato. *schema_option* è **binario (8)** e può essere [| (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) prodotto di uno o più dei valori seguenti:  
  
> [!NOTE]  
>  Se è NULL, il sistema genera automaticamente un'opzione di schema valida per l'articolo in base alle proprietà dell'articolo. La tabella delle **opzioni predefinite dello schema** riportata nella sezione Osservazioni Mostra il valore che verrà scelto in base alla combinazione del tipo di articolo e del tipo di replica.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0x00**|Disabilita la creazione di script da parte del agente di snapshot e USA *creation_script*.|  
|**0x01**|Genera lo script per la creazione di oggetti (CREATE TABLE, CREATE PROCEDURE e così via). Corrisponde al valore predefinito per gli articoli di stored procedure.|  
|**0x02**|Genera le stored procedure che propagano le eventuali modifiche per l'articolo.|  
|**0x04**|Gli script per le colonne Identity vengono creati tramite la proprietà IDENTITY.|  
|**0x08**|Replicare le colonne di tipo **timestamp** . Se non è impostato, le colonne **timestamp** vengono replicate come **binarie**.|  
|**0x10**|Genera un indice cluster corrispondente. Anche se questa opzione non viene impostata, se sono già definiti in una tabella pubblicata, gli indici correlati alle chiavi primarie e i vincoli UNIQUE vengono generati.|  
|**0x20**|Converte i tipi di dati definiti dall'utente (UDT) in tipi di dati di base nel Sottoscrittore. Questa opzione non può essere usata quando è presente un vincolo CHECK o DEFAULT su una colonna UDT, se una colonna UDT è inclusa nella chiave primaria o se una colonna calcolata fa riferimento a una colonna UDT. *Non supportato per i Publisher Oracle*.|  
|**0x40**|Genera indici non cluster corrispondenti. Anche se questa opzione non viene impostata, se sono già definiti in una tabella pubblicata, gli indici correlati alle chiavi primarie e i vincoli UNIQUE vengono generati.|  
|**0x80**|Replica i vincoli PRIMARY KEY. Vengono replicati anche tutti gli indici correlati al vincolo, anche se le opzioni **0x10** e **0x40** non sono abilitate.|  
|**0x100**|Replica gli eventuali trigger dell'utente di un articolo di tabella. *Non supportato per i Publisher Oracle*.|  
|**0x200**|Replica i vincoli FOREIGN KEY. Se la tabella con riferimenti non fa parte di una pubblicazione, tutti i vincoli di chiave esterna su una tabella pubblicata non vengono replicati. *Non supportato per i Publisher Oracle*.|  
|**0x400**|Replica i vincoli CHECK. *Non supportato per i Publisher Oracle*.|  
|**0x800**|Replica i valori predefiniti. *Non supportato per i Publisher Oracle*.|  
|**0x1000**|Replica le regole di confronto a livello di colonna.<br /><br /> **Nota:** Questa opzione deve essere impostata per i Publisher Oracle per abilitare i confronti con distinzione tra maiuscole e minuscole.|  
|**0x2000**|Replica le proprietà estese associate all'oggetto di origine dell'articolo pubblicato. *Non supportato per i Publisher Oracle*.|  
|**0x4000**|Replica i vincoli UNIQUE. Vengono replicati anche tutti gli indici correlati al vincolo, anche se le opzioni **0x10** e **0x40** non sono abilitate.|  
|**0x8000**|Questa opzione non è valida per i server di pubblicazione [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
|**0x10000**|Replica i vincoli CHECK come NOT FOR REPLICATION in modo che i vincoli non vengono imposti durante la sincronizzazione.|  
|**0x20000**|Replica i vincoli FOREIGN KEY come NOT FOR REPLICATION in modo che i vincoli non vengono imposti durante la sincronizzazione.|  
|**0x40000**|Replica i filegroup associati a una tabella o un indice partizionato.|  
|**0x80000**|Replica lo schema di partizione per una tabella partizionata.|  
|**0x100000**|Replica lo schema di partizione per un indice partizionato.|  
|**0x200000**|Replica le statistiche della tabella.|  
|**0x400000**|Associazioni predefinite|  
|**0x800000**|Associazioni regola|  
|**0x1000000**|Indice full-text|  
|**0x2000000**|Le raccolte di XML Schema associate alle colonne **XML** non vengono replicate.|  
|**0x4000000**|Replica gli indici nelle colonne **XML** .|  
|**0x8000000**|Crea gli schemi non ancora presenti nel Sottoscrittore.|  
|**0x10000000**|Converte le colonne **XML** in **ntext** nel Sottoscrittore.|  
|**0x20000000**|Converte i tipi di dati LOB (**nvarchar (max)**, **varchar (max)** e **varbinary (max)**) introdotti in in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tipi di dati supportati in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
|**0x40000000**|Replica le autorizzazioni.|  
|**0x80000000**|Tenta di eliminare le dipendenze da tutti gli oggetti che non fanno parte della pubblicazione.|  
|**0x100000000**|Utilizzare questa opzione per replicare l'attributo FILESTREAM se è specificato nelle colonne **varbinary (max)** . Non specificare questa opzione se si stanno replicando tabelle nei Sottoscrittori [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La replica di tabelle con colonne FILESTREAM [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] nei Sottoscrittori non è supportata, indipendentemente dalla modalità di impostazione di questa opzione dello schema.<br /><br /> Vedere l'opzione correlata **0x800000000**.|  
|**0x200000000**|Converte i tipi di dati di data e ora (**Data**, **ora**, **DateTimeOffset** e **datetime2**) introdotti in in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipi di dati supportati nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**0x400000000**|Replica l'opzione di compressione per dati e indici. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
|**0x800000000**|Impostare questa opzione per archiviare i dati FILESTREAM nel relativo filegroup nel Sottoscrittore. Se questa opzione non è impostata, i dati FILESTREAM vengono archiviati nel filegroup predefinito. Tramite la replica non vengono creati filegroup, pertanto, se si imposta questa opzione, è necessario creare il filegroup prima di applicare lo snapshot nel Sottoscrittore. Per altre informazioni su come creare oggetti prima di applicare lo snapshot, vedere [eseguire gli script prima e dopo l'applicazione dello snapshot](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vedere l'opzione correlata **0x100000000**.|  
|**0x1000000000**|Converte i tipi definiti dall'utente (UDT) Common Language Runtime (CLR) più grandi di 8000 byte in **varbinary (max)** in modo che le colonne di tipo UDT possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x2000000000**|Converte il tipo di dati **hierarchyid** in **varbinary (max)** in modo che le colonne di tipo **hierarchyid** possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Per ulteriori informazioni sull'utilizzo delle colonne **hierarchyid** nelle tabelle replicate, vedere [hierarchyid &#40;&#41;Transact-SQL](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
|**0x4000000000**|Replica gli eventuali indici filtrati sulla tabella. Per altre informazioni sugli indici filtrati, vedere [creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md).|  
|**0x8000000000**|Converte i tipi di dati **geography** e **Geometry** in **varbinary (max)** in modo che le colonne di questi tipi possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x10000000000**|Replica gli indici nelle colonne di tipo **geography** e **Geometry**.|  
|**0x20000000000**|Replica l'attributo SPARSE per le colonne. Per altre informazioni su questo attributo, vedere [usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).|  
|**0x40000000000**|Consente di creare script da parte dell'agente snapshot per creare una tabella ottimizzata per la memoria nel Sottoscrittore.|  
|**0x80000000000**|Converte l'indice cluster in un indice non cluster per gli articoli con ottimizzazione per la memoria.|  
|**0x400000000000**|Replica gli indici columnstore non cluster nelle tabelle|  
|**0x800000000000**|Replica tutti gli indici columnstore non cluster flitered nella tabella o nelle tabelle.|  
|NULL|La replica imposta automaticamente *schema_option* su un valore predefinito, il cui valore dipende da altre proprietà degli articoli. Nella tabella "Opzioni predefinite dello schema" riportata nella sezione Osservazioni vengono descritte le opzioni predefinite dello schema in base a tipo di articolo e tipo di replica.<br /><br /> Il valore predefinito per le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazioni non è **0x050D3**.|  
  
 Non tutti i valori *schema_option* sono validi per ogni tipo di replica e tipo di articolo. La tabella **Opzioni schema valide** nella sezione Osservazioni Mostra le opzioni dello schema valide che è possibile scegliere in base alla combinazione del tipo di articolo e del tipo di replica.  
  
`[ @destination_owner = ] 'destination_owner'` Nome del proprietario dell'oggetto di destinazione. *destination_owner* è di **tipo sysname** e il valore predefinito è null. Quando non viene specificato *destination_owner* , il proprietario viene specificato automaticamente in base alle regole seguenti:  
  
|Condizione|Proprietario oggetto di destinazione|  
|---------------|------------------------------|  
|La pubblicazione utilizza la copia bulk in modalità nativa per generare lo snapshot iniziale, che supporta solo Sottoscrittori [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Il valore predefinito è *source_owner*.|  
|Pubblicazione da un server di pubblicazione non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Corrispondente per impostazione predefinita al proprietario del database di destinazione.|  
|La pubblicazione utilizza la copia bulk in modalità carattere per generare lo snapshot iniziale, che supporta Sottoscrittori non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Non assegnato.|  
  
 Per supportare i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non, *destination_owner* deve essere null.  
  
`[ @status = ] status` Specifica se l'articolo è attivo e altre opzioni relative alla modalità di propagazione delle modifiche. *lo stato* è **tinyint** e può essere [| (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) prodotto di uno o più di questi valori.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|L'articolo è attivo.|  
|**8**|Include il nome della colonna nelle istruzioni INSERT.|  
|**16** (impostazione predefinita)|Utilizza istruzioni con parametri.|  
|**24**|Include il nome della colonna nelle istruzioni INSERT e utilizza istruzioni con parametri.|  
|**64**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 Ad esempio, un articolo attivo che utilizza istruzioni con parametri includerà il valore 17 in questa colonna. Il valore **0** indica che l'articolo è inattivo e non è stata definita alcuna proprietà aggiuntiva.  
  
`[ @source_owner = ] 'source_owner'` Proprietario dell'oggetto di origine. *source_owner* è di **tipo sysname** e il valore predefinito è null. è necessario specificare *source_owner* per i Publisher Oracle.  
  
`[ @sync_object_owner = ] 'sync_object_owner'` Proprietario della vista che definisce l'articolo pubblicato. *sync_object_owner* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @filter_owner = ] 'filter_owner'` Proprietario del filtro. *filter_owner* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @source_object = ] 'source_object'` Oggetto di database da pubblicare. *source_object* è di **tipo sysname** e il valore predefinito è null. Se *source_table* è null, *source_object* non può essere null. utilizzare *source_object* anziché *source_table*. Per ulteriori informazioni sui tipi di oggetti che possono essere pubblicati tramite la replica snapshot o transazionale, vedere [pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
`[ @artid = ] _article_ID_ OUTPUT` ID dell'articolo del nuovo articolo. *article_id* è di **tipo int** e il valore predefinito è null. si tratta di un parametro di output.  
  
`[ @auto_identity_range = ] 'auto_identity_range'` Abilita e Disabilita la gestione automatica degli intervalli di valori Identity in una pubblicazione al momento della creazione. *auto_identity_range* è di **tipo nvarchar (5)**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**true**|Abilita la gestione automatica degli intervalli di valori Identity.|  
|**false**|Viene disabilitata la gestione automatica degli intervalli di valori Identity.|  
|NULL (impostazione predefinita)|La gestione degli intervalli di valori Identity è impostata da *identityrangemanagementoption*.|  
  
> [!NOTE]  
>  *auto_identity_range* è stato deprecato e viene fornito solo per compatibilità con le versioni precedenti. Usare *identityrangemanagementoption* per specificare le opzioni di gestione degli intervalli di valori Identity. Per altre informazioni, vedere [Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @pub_identity_range = ] pub_identity_range` Controlla le dimensioni dell'intervallo nel server di pubblicazione se l'articolo ha *identityrangemanagementoption* impostato su **auto** o *auto_identity_range* impostato su **true**. *pub_identity_range* è di tipo **bigint** e il valore predefinito è null. *Non supportato per i Publisher Oracle*.  
  
`[ @identity_range = ] identity_range` Controlla le dimensioni dell'intervallo nel Sottoscrittore se l'articolo ha *identityrangemanagementoption* impostato su **auto** o *auto_identity_range* impostato su **true**. *identity_range* è di tipo **bigint** e il valore predefinito è null. Utilizzato quando *auto_identity_range* è impostato su **true**. *Non supportato per i Publisher Oracle*.  
  
`[ @threshold = ] threshold` Valore percentuale che controlla quando il agente di distribuzione assegna un nuovo intervallo di valori Identity. Quando viene utilizzata la percentuale di valori specificata in *Threshold* , il agente di distribuzione crea un nuovo intervallo di valori Identity. *Threshold* è di tipo **bigint** e il valore predefinito è null. Utilizzato quando *identityrangemanagementoption* è impostato su **auto** o *auto_identity_range* è impostato su **true**. *Non supportato per i Publisher Oracle*.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è di **bit** e il valore predefinito è 0.  
  
 **0** indica che l'aggiunta di un articolo non comporta l'invalidità dello snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che l'aggiunta di un articolo potrebbe invalidare lo snapshot e, se esistono sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
`[ @use_default_datatypes = ] use_default_datatypes` Indica se vengono utilizzati i mapping predefiniti dei tipi di dati delle colonne quando si pubblica un articolo da un server di pubblicazione Oracle. *use_default_datatypes* è di bit e il valore predefinito è 1.  
  
 **1** = vengono utilizzati i mapping predefiniti delle colonne degli articoli. È possibile visualizzare i mapping dei tipi di dati predefiniti eseguendo [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md).  
  
 **0** = i mapping delle colonne degli articoli personalizzati sono definiti e pertanto [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) non viene chiamato da **sp_addarticle**.  
  
 Quando *use_default_datatypes* è impostato su **0**, è necessario eseguire [sp_changearticlecolumndatatype](../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md) una volta per ogni mapping di colonna da modificare rispetto a quello predefinito. Dopo aver definito tutti i mapping delle colonne personalizzate, è necessario eseguire [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).  
  
> [!NOTE]  
>  Questo parametro deve essere usato solo per i server di pubblicazione Oracle. Se si imposta *use_default_datatypes* su **0** per un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione viene generato un errore.  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` Specifica il modo in cui viene gestita la gestione degli intervalli di valori Identity per l'articolo. *identityrangemanagementoption* è di **tipo nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**nessuna**|La replica non esegue la gestione degli intervalli dei valori Identity espliciti. Questa opzione è consigliabile solo per compatibilità con versioni precedenti di SQL Server. Non consentito per la replica peer-to-peer.|  
|**Manuale**|Contrassegna la colonna Identity con NOT FOR REPLICATION per consentire la gestione manuale degli intervalli di valori Identity.|  
|**Automatico**|Imposta la gestione automatica degli intervalli di valori Identity.|  
|NULL (impostazione predefinita)|Il valore predefinito è **None** quando il valore di *auto_identity_range* non è **true**. L'impostazione predefinita è **manuale** in una topologia peer-to-peer predefinita (*auto_identity_range* viene ignorata).|  
  
 Per compatibilità con le versioni precedenti, quando il valore di *identityrangemanagementoption* è null, viene verificato il valore di *auto_identity_range* . Tuttavia, quando il valore di *identityrangemanagementoption* non è null, il valore di *auto_identity_range* viene ignorato.  
  
 Per altre informazioni, vedere [Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  non utilizzare *Publisher* quando si aggiunge un articolo a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
`[ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot'` Indica se i trigger utente replicati vengono eseguiti quando viene applicato lo snapshot iniziale. *fire_triggers_on_snapshot* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** indica che i trigger utente su una tabella replicata vengono eseguiti quando viene applicato lo snapshot. Per consentire la replica dei trigger, il valore della maschera di *schema_option* deve includere il valore **0x100**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addarticle** viene utilizzata per la replica snapshot o transazionale.  
  
 Per impostazione predefinita, vengono escluse dalla pubblicazione le colonne della tabella di origine con tipo di dati non supportato dalla replica. Se è necessario pubblicare una colonna di questo tipo, è necessario eseguire [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) per aggiungere la colonna.  
  
 Per l'aggiunta di un articolo a una pubblicazione che supporta la replica transazionale peer-to-peer sono valide le restrizioni seguenti:  
  
-   È necessario specificare istruzioni con parametri per tutti gli articoli basati su log. È necessario includere **16** nel valore di *stato* .  
  
-   Il nome e il proprietario della tabella di destinazione devono corrispondere a quelli della tabella di origine.  
  
-   Non è possibile filtrare l'articolo in modo orizzontale o verticale.  
  
-   La gestione automatica degli intervalli di valori Identity non è supportata. È necessario specificare il valore Manual per *identityrangemanagementoption*.  
  
-   Se nella tabella è presente una colonna **timestamp** , è necessario includere 0x08 in *schema_option* per replicare la colonna come **timestamp**.  
  
-   Non è possibile specificare un valore **SQL** per *ins_cmd*, *upd_cmd* e *del_cmd*.  
  
 Per altre informazioni, vedere [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 Quando si pubblicano gli oggetti, nei Sottoscrittori vengono copiate le relative definizioni. Per la pubblicazione di un oggetto di database che dipende da altri oggetti, è necessario pubblicare tutti gli oggetti a cui fa riferimento. Se ad esempio si pubblica una vista che dipende da una tabella, sarà necessario pubblicare anche la tabella.  
  
 Se *vertical_partition* è impostato su **true**, **sp_addarticle** rinvia la creazione della vista fino a quando non viene chiamato [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) (dopo l'aggiunta dell'ultimo [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) ).  
  
 Se la pubblicazione consente sottoscrizioni aggiornabili e la tabella pubblicata non contiene una colonna **uniqueidentifier** , **sp_addarticle** aggiunge automaticamente una colonna **uniqueidentifier** alla tabella.  
  
 Quando si esegue la replica in un Sottoscrittore che non è un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (replica eterogenea), solo [!INCLUDE[tsql](../../includes/tsql-md.md)] le istruzioni sono supportate per i comandi **Insert**, **Update** e **Delete** .  
  
 Quando l'agente di lettura log è in esecuzione, l'aggiunta di un articolo a una pubblicazione peer-to-peer può causare un deadlock tra l'agente di lettura log e il processo tramite cui viene aggiunto l'articolo. Per evitare questo problema, prima di aggiungere un articolo a una pubblicazione peer-to-peer usare Monitoraggio replica per arrestare l'agente di lettura log sul nodo in cui si aggiunge l'articolo. Riavviare l'agente di lettura log dopo aver aggiunto l'articolo.  
  
 Quando `@del_cmd = 'NONE'` si imposta o `@ins_cmd = 'NONE'` , la propagazione dei comandi di **aggiornamento** potrebbe essere interessata anche dal mancato invio di tali comandi quando si verifica un aggiornamento associato. Per aggiornamento associato si intende un tipo di istruzione UPDATE dal server di pubblicazione che replica come coppia DELETE/INSERT nel sottoscrittore.  
  
## <a name="default-schema-options"></a>Opzioni predefinite dello schema  
 In questa tabella viene descritto il valore predefinito impostato dalla replica se *schema_options* non è specificato dall'utente, in cui questo valore dipende dal tipo di replica (visualizzato nella parte superiore) e dal tipo di articolo (indicato nella prima colonna).  
  
|Tipo di articolo|Replica transazionale|Replica snapshot|  
|------------------|----------------------|------|  
|**aggregate schema only**|**0x01**|**0x01**|  
|**func schema only**|**0x01**|**0x01**|  
|**indexed view schema only**|**0x01**|**0x01**|  
|**indexed view logbased**|**0x30F3**|**0x3071**|  
|**indexed view logbase manualboth**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualfilter**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualview**|**0x30F3**|**0x3071**|  
|**logbased**|**0x30F3**|**0x3071**|  
|**logbased manualfilter**|**0x30F3**|**0x3071**|  
|**logbased manualview**|**0x30F3**|**0x3071**|  
|**proc exec**|**0x01**|**0x01**|  
|**proc schema only**|**0x01**|**0x01**|  
|**serializable proc exec**|**0x01**|**0x01**|  
|**view schema only**|**0x01**|**0x01**|  
  
> [!NOTE]
>  Se una pubblicazione è abilitata per l'aggiornamento in coda, viene aggiunto un valore *schema_option* **0x80** al valore predefinito indicato nella tabella. Il *schema_option* predefinito per una pubblicazione non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è **0x050D3**.  
  
## <a name="valid-schema-options"></a>Opzioni di schema valide  
 In questa tabella vengono descritti i valori consentiti di *schema_option* in base al tipo di replica (visualizzato nella parte superiore) e al tipo di articolo (indicato nella prima colonna).  
  
|Tipo di articolo|Replica transazionale|Replica snapshot|  
|------------------|----------------------|------|  
|**logbased**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**logbased manualfilter**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**logbased manualview**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**indexed view logbased**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**indexed view logbased manualfilter**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**indexed view logbased manualview**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**indexed view logbase manualboth**|Tutte le opzioni|Tutte le opzioni, ma **0x02**|  
|**proc exec**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|  
|**serializable proc exec**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|  
|**proc schema only**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|  
|**view schema only**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** e **0x80000000**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** e **0x80000000**|  
|**func schema only**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** e **0x80000000**|  
|**indexed view schema only**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** e **0x80000000**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** e **0x80000000**|  
  
> [!NOTE]
>  Per le pubblicazioni ad aggiornamento in coda, è necessario abilitare i valori *schema_option* di **0x8000** e **0x80** . I valori *schema_option* supportati per le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazioni non sono: **0x01**, **0x02**, **0x10**, **0x40**, **0x80**, **0x1000**, **0x4000** e **0x8000**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-addarticle-transact-sql_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addarticle**.  
  
## <a name="see-also"></a>Vedere anche  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_articleview &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
