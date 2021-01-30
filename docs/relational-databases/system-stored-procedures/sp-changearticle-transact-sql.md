---
description: sp_changearticle (Transact-SQL)
title: sp_changearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticle
- sp_changearticle_TSQL
helpviewer_keywords:
- sp_changearticle
ms.assetid: 24c33ca5-f03a-4417-a267-131ca5ba6bb5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 22d64091d206161546856e7186958ab60217bcc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203741"
---
# <a name="sp_changearticle-transact-sql"></a>sp_changearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà di un articolo in una pubblicazione transazionale o snapshot. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changearticle [ [@publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione contenente l'articolo. *Publication* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @article = ] 'article'` Nome dell'articolo di cui modificare la proprietà. *article* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @property = ] 'property'` Proprietà dell'articolo da modificare. *Property* è di **tipo nvarchar (100)**.  
  
`[ @value = ] 'value'` Nuovo valore della proprietà article. *value* è di **tipo nvarchar (255)**.  
  
 Nella tabella seguente vengono descritte le proprietà degli articoli e i valori corrispondenti.  
  
|Proprietà|Valori|Descrizione|  
|--------------|------------|-----------------|  
|**creation_script**||Percorso e nome di uno script di schema dell'articolo utilizzato per la creazione delle tabelle di destinazione. Il valore predefinito è NULL.|  
|**del_cmd**||Istruzione DELETE da eseguire. In alternativa, viene creata dal log.|  
|**description**||Nuova voce descrittiva per l'articolo.|  
|**dest_object**||Disponibile per compatibilità con le versioni precedenti. Usare **dest_table**.|  
|**dest_table**||Nuova tabella di destinazione.|  
|**destination_owner**||Nome del proprietario dell'oggetto di destinazione.|  
|**filter**||Nuova stored procedure da utilizzare per filtrare la tabella in modo orizzontale. Il valore predefinito è NULL. Non può essere modificato per le pubblicazioni nella replica peer-to-peer.|  
|**fire_triggers_on_snapshot**|**true**|I trigger utente replicati vengono eseguiti quando si applica lo snapshot iniziale.<br /><br /> Nota: per i trigger da replicare, il valore della maschera di *schema_option* deve includere il valore **0x100**.|  
||**false**|I trigger utente replicati non vengono eseguiti quando si applica lo snapshot iniziale.|  
|**identity_range**||Controlla le dimensioni degli intervalli di valori Identity assegnati nel Sottoscrittore. Non supportato per la replica peer-to-peer.|  
|**ins_cmd**||Istruzione INSERT da eseguire. In alternativa, viene creata dal log.|  
|**pre_creation_cmd**||Comando preliminare per eliminare, rimuovere o troncare la tabella di destinazione prima della sincronizzazione.|  
||**nessuna**|Non utilizza alcun comando.|  
||**goccia**|Rimuove la tabella di destinazione.|  
||**delete**|Elimina la tabella di destinazione.|  
||**truncate**|Tronca la tabella di destinazione.|  
|**pub_identity_range**||Controlla le dimensioni degli intervalli di valori Identity assegnati nel Sottoscrittore. Non supportato per la replica peer-to-peer.|  
|**schema_option**||Specifica la mappa di bit dell'opzione di generazione dello schema per l'articolo specificato. *schema_option* è **binario (8)**. Per ulteriori informazioni, vedere la sezione Osservazioni di seguito in questo argomento.|  
||**0x00**|Disabilita la creazione di script eseguita dall'agente snapshot.|  
||**0x01**|Genera le istruzioni per la creazione di oggetti (CREATE TABLE, CREATE PROCEDURE e così via).|  
||**0x02**|Genera le stored procedure che propagano le eventuali modifiche per l'articolo.|  
||**0x04**|Gli script per le colonne Identity vengono creati tramite la proprietà IDENTITY.|  
||**0x08**|Replicare le colonne di tipo **timestamp** . Se non è impostato, le colonne **timestamp** vengono replicate come **binarie**.|  
||**0x10**|Genera un indice cluster corrispondente.|  
||**0x20**|Converte i tipi di dati definiti dall'utente (UDT) in tipi di dati di base nel Sottoscrittore. Questa opzione non può essere usata quando è presente un vincolo CHECK o DEFAULT su una colonna UDT, se una colonna UDT è inclusa nella chiave primaria o se una colonna calcolata fa riferimento a una colonna UDT. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**0x40**|Genera indici non cluster corrispondenti.|  
||**0x80**|include i vincoli di integrità referenziale dichiarati nelle chiavi primarie.|  
||**0x100**|Replica gli eventuali trigger dell'utente di un articolo di tabella.|  
||**0x200**|Replica i vincoli FOREIGN KEY. Se la tabella con riferimenti non fa parte di una pubblicazione, tutti i vincoli FOREIGN KEY in una tabella pubblicata non vengono replicati.|  
||**0x400**|Replica i vincoli CHECK.|  
||**0x800**|Replica i valori predefiniti.|  
||**0x1000**|Replica le regole di confronto a livello di colonna.|  
||**0x2000**|Replica le proprietà estese associate all'oggetto di origine dell'articolo pubblicato.|  
||**0x4000**|Replica le eventuali chiavi univoche definite in un articolo di tabella.|  
||**0x8000**|Replica come vincoli la chiave primaria e le chiavi univoche di un articolo di tabella tramite istruzioni ALTER TABLE.<br /><br /> Nota: questa opzione è stata deprecata. In alternativa, usare **0x80** e **0x4000** .|  
||**0x10000**|Replica i vincoli CHECK come NOT FOR REPLICATION in modo che i vincoli non vengono imposti durante la sincronizzazione.|  
||**0x20000**|Replica i vincoli FOREIGN KEY come NOT FOR REPLICATION in modo che i vincoli non vengono imposti durante la sincronizzazione.|  
||**0x40000**|Replica i filegroup associati a una tabella o un indice partizionato.|  
||**0x80000**|Replica lo schema di partizione per una tabella partizionata.|  
||**0x100000**|Replica lo schema di partizione per un indice partizionato.|  
||**0x200000**|Replica le statistiche della tabella.|  
||**0x400000**|Associazioni predefinite|  
||**0x800000**|Associazioni regola|  
||**0x1000000**|Indice full-text|  
||**0x2000000**|Le raccolte di XML Schema associate alle colonne **XML** non vengono replicate.|  
||**0x4000000**|Replica gli indici nelle colonne **XML** .|  
||**0x8000000**|Crea gli schemi non ancora presenti nel Sottoscrittore.|  
||**0x10000000**|Converte le colonne **XML** in **ntext** nel Sottoscrittore.|  
||**0x20000000**|Converte i tipi di dati LOB (**nvarchar (max)**, **varchar (max)** e **varbinary (max)**) introdotti in in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tipi di dati supportati in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
||**0x40000000**|Replica le autorizzazioni.|  
||**0x80000000**|Tenta di eliminare le dipendenze da tutti gli oggetti che non fanno parte della pubblicazione.|  
||**0x100000000**|Utilizzare questa opzione per replicare l'attributo FILESTREAM se è specificato nelle colonne **varbinary (max)** . Non specificare questa opzione se si stanno replicando tabelle nei Sottoscrittori [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La replica di tabelle con colonne FILESTREAM [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] nei Sottoscrittori non è supportata, indipendentemente dalla modalità di impostazione di questa opzione dello schema.<br /><br /> Vedere l'opzione correlata **0x800000000**.|  
||**0x200000000**|Converte i tipi di dati di data e ora (**Data**, **ora**, **DateTimeOffset** e **datetime2**) introdotti in in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipi di dati supportati nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
||**0x400000000**|Replica l'opzione di compressione per dati e indici. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
||**0x800000000**|Impostare questa opzione per archiviare i dati FILESTREAM nel relativo filegroup nel Sottoscrittore. Se questa opzione non è impostata, i dati FILESTREAM vengono archiviati nel filegroup predefinito. Tramite la replica non vengono creati filegroup, pertanto, se si imposta questa opzione, è necessario creare il filegroup prima di applicare lo snapshot nel Sottoscrittore. Per altre informazioni su come creare oggetti prima di applicare lo snapshot, vedere [eseguire gli script prima e dopo l'applicazione dello snapshot](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vedere l'opzione correlata **0x100000000**.|  
||**0x1000000000**|Converte Common Language Runtime tipi CLR definiti dall'utente (UDT) di dimensioni maggiori di 8000 byte in **varbinary (max)** in modo che le colonne di tipo UDT possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x2000000000**|Converte il tipo di dati **hierarchyid** in **varbinary (max)** in modo che le colonne di tipo **hierarchyid** possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Per ulteriori informazioni sull'utilizzo delle colonne **hierarchyid** nelle tabelle replicate, vedere [hierarchyid &#40;&#41;Transact-SQL](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Replica gli eventuali indici filtrati sulla tabella. Per altre informazioni sugli indici filtrati, vedere [creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Converte i tipi di dati **geography** e **Geometry** in **varbinary (max)** in modo che le colonne di questi tipi possano essere replicate nei Sottoscrittori che eseguono [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x10000000000**|Replica gli indici nelle colonne di tipo **geography** e **Geometry**.|  
||**0x20000000000**|Replica l'attributo SPARSE per le colonne. Per altre informazioni su questo attributo, vedere [usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).|  
||**0x40000000000**|Consente di creare script da parte dell'agente snapshot per creare una tabella con ottimizzazione per la memoria nel Sottoscrittore.|  
||**0x80000000000**|Converte l'indice cluster in un indice non cluster per gli articoli con ottimizzazione per la memoria.|  
|**Stato**||Nuovo stato della proprietà.|  
||**dts horizontal partitions**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
||**include column names**|I nomi delle colonne sono inclusi nell'istruzione INSERT replicata.|  
||**no column names**|I nomi delle colonne non sono inclusi nell'istruzione INSERT replicata.|  
||**no dts horizontal partitions**|La partizione orizzontale per l'articolo non è definita da una sottoscrizione trasformabile.|  
||**nessuna**|Cancella tutte le opzioni di stato nella tabella [sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md) e contrassegna l'articolo come inattivo.|  
||**parameters**|Le modifiche vengono propagate al Sottoscrittore tramite i comandi con parametri. Questa è l'impostazione predefinita per un nuovo articolo.|  
||**valori letterali stringa**|Le modifiche vengono propagate al Sottoscrittore tramite i valori letterali stringa.|  
|**sync_object**||Nome della tabella o vista utilizzata per generare un file di output di sincronizzazione. Il valore predefinito è NULL. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**spazio tabella**||Identifica lo spazio tabella utilizzato dalla tabella di registrazione per un articolo pubblicato da un database Oracle. Per altre informazioni, vedere [Gestire spazi di tabella Oracle](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md).|  
|**threshold**||Valore percentuale che controlla quando l'agente di distribuzione assegna un nuovo intervallo di valori Identity. Non supportato per la replica peer-to-peer.|  
|**type**||Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**logbased**|Articolo basato su un log.|  
||**logbased manualboth**|Articolo basato su log con filtro manuale e vista manuale. Questa opzione richiede che vengano impostate anche le proprietà *sync_object* e *filtro* . Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**logbased manualfilter**|Articolo basato su log con filtro manuale. Questa opzione richiede che vengano impostate anche le proprietà *sync_object* e *filtro* . Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**logbased manualview**|Articolo basato su log con vista manuale. Per questa opzione è necessario impostare anche la proprietà *sync_object* . Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**viewlogbased indicizzati**|Articolo di vista indicizzata basato su log. Questa proprietà non è supportata per server di pubblicazione Oracle. Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base.|  
||**manualboth viewlogbased indicizzati**|Articolo di vista indicizzata basato su log con filtro manuale e vista manuale. Questa opzione richiede che vengano impostate anche le proprietà *sync_object* e *filtro* . Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**manualfilter viewlogbased indicizzati**|Articolo di vista indicizzata basato su log con filtro manuale. Questa opzione richiede che vengano impostate anche le proprietà *sync_object* e *filtro* . Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
||**manualview viewlogbased indicizzati**|Articolo di vista indicizzata basato su log con vista manuale. Per questa opzione è necessario impostare anche la proprietà *sync_object* . Per questo tipo di articolo non è necessario pubblicare separatamente la tabella di base. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|**upd_cmd**||Istruzione UPDATE da eseguire. In alternativa, viene creata dal log.|  
|NULL|NULL|Restituisce un elenco di proprietà dell'articolo che è possibile modificare.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è di **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non invalidano lo snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo possono invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
 Per informazioni sulle proprietà che richiedono la generazione di un nuovo snapshot quando vengono modificate, vedere la sezione Osservazioni.  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` Conferma che l'azione eseguita da questo stored procedure potrebbe richiedere la reinizializzazione delle sottoscrizioni esistenti. *force_reinit_subscription* è un **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non provocano la reinizializzazione della sottoscrizione. Se la stored procedure rileva che la modifica richiede la reinizializzazione delle sottoscrizioni esistenti, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo provocano la reinizializzazione delle sottoscrizioni esistenti e consente la reinizializzazione della sottoscrizione.  
  
 Per ulteriori informazioni sulle proprietà che richiedono la reinizializzazione di tutte le sottoscrizioni esistenti in caso di modifica, vedere la sezione Osservazioni.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Impossibile utilizzare *Publisher* quando si modificano le proprietà degli articoli in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changearticle** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
 Quando un articolo appartiene a una pubblicazione che supporta la replica transazionale peer-to-peer, è possibile modificare solo le proprietà **Description**, **ins_cmd**, **upd_cmd** e **del_cmd** .  
  
 La modifica di una delle proprietà seguenti richiede la generazione di un nuovo snapshot ed è necessario specificare il valore **1** per il parametro *force_invalidate_snapshot* :  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **ins_cmd**  
  
-   **pre_creation_cmd**  
  
-   **schema_options**  
  
-   **upd_cmd**  
  
 La modifica di una delle proprietà seguenti richiede la reinizializzazione delle sottoscrizioni esistenti ed è necessario specificare il valore **1** per il parametro *force_reinit_subscription* .  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **filter**  
  
-   **ins_cmd**  
  
-   **Stato**  
  
-   **upd_cmd**  
  
 All'interno di una pubblicazione esistente, è possibile utilizzare **sp_changearticle** per modificare un articolo senza dover eliminare e ricreare l'intera pubblicazione.  
  
> [!NOTE]  
>  Quando si modifica il valore di *schema_option*, il sistema non esegue un aggiornamento bit per bit. Ciò significa che quando si imposta *schema_option* utilizzando **sp_changearticle**, le impostazioni di bit esistenti possono essere disattivate. Per mantenere le impostazioni esistenti, è necessario eseguire [| (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) tra il valore da impostare e il valore corrente di *schema_option*, che può essere determinato eseguendo [sp_helparticle](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md).  
  
## <a name="valid-schema-options"></a>Opzioni di schema valide  
 Nella tabella seguente vengono descritti i valori consentiti di *schema_option* in base al tipo di replica (visualizzato nella parte superiore) e al tipo di articolo (mostrata nella prima colonna).  
  
| Tipo di articolo | Tipo di replica-transazionale | Tipo di replica-snapshot |
| ------------ | -------------------------------- | --------------------------- |
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
>  Per le pubblicazioni ad aggiornamento in coda, il valore *schema_option* di **0x80** deve essere abilitato. I valori *schema_option* supportati per le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazioni non sono: **0x01**, **0x02**, **0x10**, **0x40**, **0x80**, **0x1000** e **0x4000**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_changetranarticle](../../relational-databases/replication/codesnippet/tsql/sp-changearticle-transac_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_changearticle**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà degli articoli](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Modificare le proprietà di pubblicazioni e articoli](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)  
  
  
