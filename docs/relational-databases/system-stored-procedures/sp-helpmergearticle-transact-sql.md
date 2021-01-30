---
description: sp_helpmergearticle (Transact-SQL)
title: sp_helpmergearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergearticle
- sp_helpmergearticle_TSQL
helpviewer_keywords:
- sp_helpmergearticle
ms.assetid: 0fb9986a-3c33-46ef-87bb-297396ea5a6a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 43365f58dbfe76c89a5e7e41a0c65c65188edfdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179284"
---
# <a name="sp_helpmergearticle-transact-sql"></a>sp_helpmergearticle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni su un articolo. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di sottoscrizione di un Sottoscrittore di ripubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergearticle [ [ @publication = ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione per cui si desidera recuperare informazioni. *Publication* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti gli articoli di tipo merge contenuti in tutte le pubblicazioni del database corrente.  
  
`[ @article = ] 'article'` Nome dell'articolo per cui si desidera ottenere informazioni. *article* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti gli articoli di tipo merge nella pubblicazione specificata.  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificatore dell'articolo.|  
|**nome**|**sysname**|Nome dell'articolo.|  
|**source_owner**|**sysname**|Nome del proprietario dell'oggetto di origine.|  
|**source_object**|**sysname**|Nome dell'oggetto di origine da cui aggiungere l'articolo.|  
|**sync_object_owner**|**sysname**|Nome del proprietario della vista che definisce l'articolo pubblicato.|  
|**sync_object**|**sysname**|Nome dell'oggetto personalizzato utilizzato per stabilire i dati iniziali per la partizione.|  
|**description**|**nvarchar(255)**|Descrizione dell'articolo.|  
|**Stato**|**tinyint**|Stato dell'articolo. I possibili valori sono i seguenti:<br /><br /> **1** = inattivo<br /><br /> **2** = attivo<br /><br /> **5** = operazione di Data Definition Language (DDL) in sospeso<br /><br /> **6** = operazione DDL con uno snapshot appena generato<br /><br /> Nota: quando un articolo viene reinizializzato, i valori **5** e **6** vengono modificati in **2**.|  
|**creation_script**|**nvarchar(255)**|Percorso e nome di uno script di schema dell'articolo facoltativo utilizzato per la creazione dell'articolo nel database di sottoscrizione.|  
|**conflict_table**|**nvarchar (270)**|Nome della tabella in cui sono archiviati i conflitti di inserimento o aggiornamento.|  
|**article_resolver**|**nvarchar(255)**|Sistema di risoluzione personalizzato per l'articolo.|  
|**subset_filterclause**|**nvarchar (1000)**|Clausola WHERE che specifica il filtro orizzontale.|  
|**pre_creation_command**|**tinyint**|Metodo di creazione preliminare. I possibili valori sono i seguenti:<br /><br /> **0** = nessuna<br /><br /> **1** = Elimina<br /><br /> **2** = Elimina<br /><br /> **3** = troncamento|  
|**schema_option**|**binario (8)**|Mappa di bit dell'opzione di generazione dello schema per l'articolo. Per informazioni su questa opzione bitmap, vedere [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) o [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md).|  
|**type**|**smallint**|Tipo di articolo. I possibili valori sono i seguenti:<br /><br /> **10** = tabella<br /><br /> **32** = stored procedure<br /><br /> **64** = vista o vista indicizzata<br /><br /> **128** = funzione definita dall'utente<br /><br /> **160** = solo schema sinonimo|  
|**column_tracking**|**int**|Impostazione per il rilevamento a livello di colonna; dove **1** indica che il rilevamento a livello di colonna è attivo e **0** indica che il rilevamento a livello di colonna è disattivato.|  
|**resolver_info**|**nvarchar(255)**|Nome del sistema di risoluzione dell'articolo.|  
|**vertical_partition**|**bit**|Se l'articolo è partizionato verticalmente; dove **1** indica che l'articolo è partizionato verticalmente e **0** indica che non lo è.|  
|**destination_owner**|**sysname**|Proprietario dell'oggetto di destinazione. È applicabile solo per gli articoli di schema di tipo merge per stored procedure, viste e funzioni definite dall'utente.|  
|**identity_support**|**int**|Se è abilitata la gestione automatica degli intervalli di valori Identity. dove **1** è abilitato e **0** è disabilitato.|  
|**pub_identity_range**|**bigint**|Dimensioni di intervallo da utilizzare per l'assegnazione di nuovi valori Identity. Per ulteriori informazioni, vedere la sezione relativa alla replica di tipo merge in [replicare le colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**identity_range**|**bigint**|Dimensioni di intervallo da utilizzare per l'assegnazione di nuovi valori Identity. Per ulteriori informazioni, vedere la sezione relativa alla replica di tipo merge in [replicare le colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**threshold**|**int**|Valore percentuale utilizzato per i Sottoscrittori che eseguono [!INCLUDE[ssEW](../../includes/ssew-md.md)] o versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **soglia** controlla quando il agente di merge assegna un nuovo intervallo di valori Identity. Quando viene utilizzata la percentuale di valori specificata in threshold, l'agente di merge crea un nuovo intervallo di valori Identity. Per ulteriori informazioni, vedere la sezione relativa alla replica di tipo merge in [replicare le colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**verify_resolver_signature**|**int**|Se una firma digitale viene verificata prima dell'utilizzo di un sistema di risoluzione nella replica di tipo merge; dove **0** indica che la firma non viene verificata e **1** indica che la firma viene verificata per verificare se si tratta di una fonte attendibile.|  
|**destination_object**|**sysname**|Nome dell'oggetto di destinazione. È applicabile solo per gli articoli di schema di tipo merge per stored procedure, viste e funzioni definite dall'utente.|  
|**allow_interactive_resolver**|**int**|Se il sistema di risoluzione interattivo viene utilizzato in un articolo; dove **1** indica che questo resolver viene utilizzato e **0** indica che non viene utilizzato.|  
|**fast_multicol_updateproc**|**int**|Abilita o Disabilita la agente di merge per applicare le modifiche a più colonne nella stessa riga in un'unica istruzione UPDATE. dove **1** indica che più colonne vengono aggiornate in un'unica istruzione, mentre **0** indica che le istruzioni Update separate sono problemi per ogni colonna aggiornata.|  
|**check_permissions**|**int**|Valore integer che rappresenta la mappa di bit delle autorizzazioni a livello di tabella da verificare. Per un elenco di valori possibili, vedere [sp_addmergearticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**processing_order**|**int**|Ordine di applicazione delle modifiche dei dati agli articoli di una pubblicazione.|  
|**upload_options**|**tinyint**|Imposta le restrizioni per gli aggiornamenti eseguiti in un Sottoscrittore con una sottoscrizione client. I possibili valori sono i seguenti.<br /><br /> **0** = non sono previste restrizioni per gli aggiornamenti eseguiti in un Sottoscrittore con una sottoscrizione client. tutte le modifiche vengono caricate nel server di pubblicazione.<br /><br /> **1** = le modifiche sono consentite in un Sottoscrittore con una sottoscrizione client, ma non vengono caricate nel server di pubblicazione.<br /><br /> **2** = le modifiche non sono consentite in un Sottoscrittore con una sottoscrizione client.<br /><br /> Per altre informazioni, vedere [Ottimizzare le prestazioni della replica di tipo merge con gli articoli di solo download](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).|  
|**identityrangemanagementoption**|**int**|Se è abilitata la gestione automatica degli intervalli di valori Identity. dove **1** è abilitato e **0** è disabilitato.|  
|**delete_tracking**|**bit**|Se le eliminazioni vengono replicate; dove **1** indica che vengono replicate le eliminazioni e **0** indica che non lo sono.|  
|**compensate_for_errors**|**bit**|Indica se vengono eseguite azioni di compensazione quando vengono rilevati errori durante la sincronizzazione. dove **1** indica che vengono eseguite le azioni di compensazione e **0** indica che le azioni di compensazione non vengono eseguite.|  
|**partition_options**|**tinyint**|Definisce il modo in cui vengono partizionati i dati nell'articolo. Ciò consente di ottimizzare le prestazioni se tutte le righe appartengono a un'unica partizione o a un'unica sottoscrizione. *partition_options* può essere uno dei valori seguenti.<br /><br /> **0** = il filtro per l'articolo è statico oppure non restituisce un subset univoco di dati per ogni partizione. ovvero si tratta di una partizione "sovrapposta".<br /><br /> **1** = le partizioni sono sovrapposte e gli aggiornamenti Data MANIPULATION Language (DML) eseguiti nel Sottoscrittore non possono modificare la partizione a cui appartiene una riga.<br /><br /> **2** = il filtro per l'articolo restituisce partizioni non sovrapposte, ma più sottoscrittori possono ricevere la stessa partizione.<br /><br /> **3** = il filtro per l'articolo restituisce partizioni non sovrapposte univoche per ogni sottoscrizione.|  
|**artid**|**uniqueidentifier**|Identificatore univoco dell'articolo.|  
|**pubid**|**uniqueidentifier**|Identificatore univoco della pubblicazione in cui viene pubblicato l'articolo.|  
|**stream_blob_columns**|**bit**|Indica se viene utilizzata l'ottimizzazione del flusso di dati per la replica di colonne BLOB. **1** indica che viene utilizzata l'ottimizzazione e **0** indica che l'ottimizzazione non viene utilizzata.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergearticle** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del database **db_owner** nel database di pubblicazione, il ruolo **replmonitor** nel database di distribuzione o l'elenco di accesso alla pubblicazione per una pubblicazione possono eseguire **sp_helpmergearticle**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_helpmergearticle](../../relational-databases/replication/codesnippet/tsql/sp-helpmergearticle-tran_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà degli articoli](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addmergearticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
