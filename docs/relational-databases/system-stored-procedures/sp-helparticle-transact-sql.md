---
description: sp_helparticle (Transact-SQL)
title: sp_helparticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helparticle_TSQL
- sp_helparticle
helpviewer_keywords:
- sp_helparticle
ms.assetid: 9c4a1a88-56f1-45a0-890c-941b8e0f0799
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 14baf63cc2944396a13bbc911fb6be7670244216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176483"
---
# <a name="sp_helparticle-transact-sql"></a>sp_helparticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Visualizza informazioni su un articolo. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione. Per i server di pubblicazione Oracle questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helparticle [ @publication = ] 'publication'   
    [ , [ @article = ] 'article' ]  
    [ , [ @returnfilter = ] returnfilter ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @found = ] found OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome di un articolo della pubblicazione. *article* è di **tipo sysname** e il valore predefinito è **%** . Se l' *articolo* non viene specificato, vengono restituite informazioni su tutti gli articoli relativi alla pubblicazione specificata.  
  
`[ @returnfilter = ] returnfilter` Specifica se deve essere restituita la clausola di filtro. *returnfilter* è di **bit** e il valore predefinito è **1**, che restituisce la clausola di filtro.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Impossibile specificare *Publisher* quando si richiedono informazioni su un articolo pubblicato da un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
`[ @found = ] found OUTPUT` Solo per uso interno.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**ID articolo**|**int**|ID dell'articolo.|  
|**article name**|**sysname**|Nome dell'articolo.|  
|**base object**|**nvarchar (257)**|Nome della tabella sottostante rappresentata dall'articolo o dalla stored procedure.|  
|**oggetto destination**|**sysname**|Nome della tabella di destinazione (sottoscrizione).|  
|**synchronization object**|**nvarchar (257)**|Nome della vista che definisce l'articolo pubblicato.|  
|**type**|**smallint**|Tipo di articolo:<br /><br /> **1** = basato su log.<br /><br /> **3** = basato su log con filtro manuale.<br /><br /> **5** = basato su log con vista manuale.<br /><br /> **7** = basato su log con filtro manuale e vista manuale.<br /><br /> **8** = esecuzione di stored procedure.<br /><br /> **24** = esecuzione stored procedure serializzabile.<br /><br /> **32** = stored procedure (solo schema).<br /><br /> **64** = View (solo schema).<br /><br /> **96** = funzione di aggregazione (solo schema).<br /><br /> **128** = funzione (solo schema).<br /><br /> **257** = vista indicizzata basata su log.<br /><br /> **259** = vista indicizzata basata su log con filtro manuale.<br /><br /> **261** = vista indicizzata basata su log con vista manuale.<br /><br /> **263** = vista indicizzata basata su log con filtro manuale e vista manuale.<br /><br /> **320** = vista indicizzata (solo schema).<br /><br />|  
|**Stato**|**tinyint**|Può essere il risultato [& (and bit per bit)](../../t-sql/language-elements/bitwise-and-transact-sql.md) di una o più proprietà di articolo:<br /><br /> **0x00** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **0x01** = l'articolo è attivo.<br /><br /> **0x08** = include il nome della colonna nelle istruzioni INSERT.<br /><br /> **0x16** = usa istruzioni con parametri.<br /><br /> **0x32** = usa istruzioni con parametri e include il nome della colonna nelle istruzioni INSERT.|  
|**filter**|**nvarchar (257)**|Stored procedure utilizzata per filtrare la tabella in senso orizzontale. Questa stored procedure deve essere stata creata con la clausola FOR REPLICATION.|  
|**description**|**nvarchar(255)**|Voce descrittiva per l'articolo.|  
|**insert_command**|**nvarchar(255)**|Tipo di comando di replica utilizzato per la replica degli inserimenti con articoli di tabella. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**update_command**|**nvarchar(255)**|Tipo di comando di replica utilizzato per la replica degli aggiornamenti con articoli di tabella. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**delete_command**|**nvarchar(255)**|Tipo di comando di replica utilizzato per la replica delle eliminazioni con articoli di tabella. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**creation script path**|**nvarchar(255)**|Percorso e nome di uno script di schema dell'articolo utilizzato per la creazione delle tabelle di destinazione.|  
|**vertical partition**|**bit**|Indica se il partizionamento verticale è abilitato per l'articolo. dove il valore **1** indica che il partizionamento verticale è abilitato.|  
|**pre_creation_cmd**|**tinyint**|Comando preliminare per l'istruzione DROP TABLE, DELETE TABLE o TRUNCATE TABLE.|  
|**filter_clause**|**ntext**|Clausola WHERE che specifica il filtro orizzontale.|  
|**schema_option**|**binario (8)**|Maschera di bit dell'opzione di creazione dello schema per l'articolo specificato. Per un elenco completo dei valori di **schema_option** , vedere [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Nome del proprietario dell'oggetto di destinazione.|  
|**source_owner**|**sysname**|Proprietario dell'oggetto di origine.|  
|**unqua_source_object**|**sysname**|Nome dell'oggetto di origine, senza il nome del proprietario.|  
|**sync_object_owner**|**sysname**|Proprietario della vista che definisce l'articolo pubblicato. .|  
|**unqualified_sync_object**|**sysname**|Nome della vista che definisce l'articolo pubblicato, senza il nome del proprietario.|  
|**filter_owner**|**sysname**|Proprietario del filtro.|  
|**unqua_filter**|**sysname**|Nome del filtro, senza il nome del proprietario.|  
|**auto_identity_range**|**int**|Flag che indica se la gestione automatica degli intervalli di valori Identity era attivata nella pubblicazione quando la pubblicazione è stata creata. **1** indica che l'intervallo di valori Identity automatico è abilitato. **0** indica che è disabilitato.|  
|**publisher_identity_range**|**int**|Dimensioni dell'intervallo di valori Identity nel server di pubblicazione se l'articolo ha *identityrangemanagementoption* impostato su **auto** o **auto_identity_range** impostato su **true**.|  
|**identity_range**|**bigint**|Dimensioni dell'intervallo di valori Identity nel Sottoscrittore se l'articolo ha *identityrangemanagementoption* impostato su **auto** o **auto_identity_range** impostato su **true**.|  
|**threshold**|**bigint**|Valore percentuale che indica quando l'agente di distribuzione assegna un nuovo intervallo di valori Identity.|  
|**identityrangemanagementoption**|**int**|Indica la gestione degli intervalli di valori Identity per l'articolo.|  
|**fire_triggers_on_snapshot**|**bit**|Indica se i trigger utente replicati vengono eseguiti quando viene applicato lo snapshot iniziale.<br /><br /> **1** = i trigger utente vengono eseguiti.<br /><br /> **0** = i trigger utente non vengono eseguiti.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helparticle** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_helparticle** solo i membri del ruolo predefinito del server **sysadmin** , del ruolo predefinito del database **db_owner** o dell'elenco di accesso alla pubblicazione corrente.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_helptranarticle](../../relational-databases/replication/codesnippet/tsql/sp-helparticle-transact-_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà degli articoli](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
