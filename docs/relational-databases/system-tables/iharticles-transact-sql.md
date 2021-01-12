---
description: IHarticles (Transact-SQL)
title: IHarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHarticles
- IHarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHarticles system table
ms.assetid: 773ef9b7-c993-4629-9516-70c47b9dcf65
author: cawrites
ms.author: chadam
ms.openlocfilehash: 00e32678fd1df185607f3e02c5bf1f21780c5d79
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091521"
---
# <a name="iharticles-transact-sql"></a>IHarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella di sistema **IHarticles** contiene una riga per ogni articolo replicato da un server di pubblicazione non SQL Server utilizzando il server di distribuzione corrente. Questa tabella è archiviata nel database di distribuzione.  
  
## <a name="definition"></a>Definizione  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Colonna Identity che offre un numero di ID univoco per l'articolo.|  
|**nome**|**sysname**|Nome associato all'articolo, univoco all'interno della pubblicazione.|  
|**publication_id**|**smallint**|ID della pubblicazione a cui appartiene l'articolo.|  
|**table_id**|**int**|ID della tabella da pubblicare da [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md).|  
|**publisher_id**|**smallint**|ID del server di pubblicazione non SQL Server.|  
|**creation_script**|**nvarchar(255)**|Script dello schema per l'articolo.|  
|**del_cmd**|**nvarchar(255)**|Tipo di comando di replica utilizzato per la replica delle eliminazioni con articoli di tabella. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**filter**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**filter_clause**|**ntext**|Clausola WHERE dell'articolo utilizzata per il filtro orizzontale e scritta in un linguaggio Transact-SQL standard che può essere interpretato dal server di pubblicazione non SQL.|  
|**ins_cmd**|**nvarchar(255)**|Tipo di comando di replica utilizzato per la replica degli inserimenti con articoli di tabella. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**pre_creation_cmd**|**tinyint**|Comando da eseguire prima dell'applicazione dello snapshot iniziale quando nel Sottoscrittore esiste un oggetto con lo stesso nome.<br /><br /> **0** = None-non viene eseguito alcun comando.<br /><br /> **1** = eliminazione della tabella di destinazione.<br /><br /> **2** = Delete-Elimina i dati dalla tabella di destinazione.<br /><br /> **3** = troncamento: troncare la tabella di destinazione.|  
|**Stato**|**tinyint**|Maschera di bit delle opzioni e dello stato dell'articolo, che può corrispondere al risultato dell'applicazione dell'operatore OR logico bit per bit a uno o più dei valori seguenti:<br /><br /> **0** = nessuna proprietà aggiuntiva.<br /><br /> **1** = attivo.<br /><br /> **8** = include il nome della colonna nelle istruzioni INSERT.<br /><br /> **16** = usa istruzioni con parametri.<br /><br /> Ad esempio, un articolo attivo che utilizza istruzioni con parametri includerà il valore 17 in questa colonna. Il valore 0 indica che l'articolo è inattivo e che non sono state definite proprietà aggiuntive.|  
|**type**|**tinyint**|Tipo di articolo:<br /><br /> **1** = articolo basato su log.|  
|**upd_cmd**|**nvarchar(255)**|Tipo di comando di replica utilizzato per la replica degli aggiornamenti con articoli di tabella. Per altre informazioni, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**binario (8)**|Mappa di bit dell'opzione di generazione dello schema per l'articolo specificato, che può corrispondere al risultato dell'applicazione dell'operatore OR logico bit per bit a uno o più dei valori seguenti:<br /><br /> **0x00** = Disabilita lo scripting da parte del agente di snapshot e usa il CreationScript fornito.<br /><br /> **0x01** = genera la creazione dell'oggetto (Create Table, crea procedura e così via).<br /><br /> **0x10** = genera un indice cluster corrispondente.<br /><br /> **0x40** = genera gli indici non cluster corrispondenti.<br /><br /> **0x80** = include l'integrità referenziale dichiarata nelle chiavi primarie.<br /><br /> **0x1000** = replica le regole di confronto a livello di colonna. Nota: questa opzione è impostata per impostazione predefinita per i Publisher Oracle per abilitare i confronti con distinzione tra maiuscole e minuscole.<br /><br /> **0x4000** = replica le chiavi univoche se definito in un articolo di tabella.<br /><br /> **0x8000** = replica una chiave primaria e le chiavi univoche di un articolo di tabella come vincoli mediante istruzioni ALTER TABLE.|  
|**dest_owner**|**sysname**|Proprietario della tabella nel database di destinazione.|  
|**dest_table**|**sysname**|Nome della tabella di destinazione.|  
|**tablespace_name**|**nvarchar(255)**|Identifica lo spazio tabella utilizzato dalla tabella di registrazione per l'articolo.|  
|**objid**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**sync_objid**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**description**|**nvarchar(255)**|Voce descrittiva per l'articolo.|  
|**publisher_status**|**int**|Viene utilizzato per indicare se la vista che definisce l'articolo pubblicato è stata definita chiamando [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).<br /><br /> **0**  =  [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) è stato chiamato.<br /><br /> **1**  =  [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) non è stato chiamato.|  
|**article_view_owner**|**nvarchar(255)**|Proprietario dell'oggetto di sincronizzazione nel server di pubblicazione utilizzato dall'agente di lettura log.|  
|**article_view**|**nvarchar(255)**|Oggetto di sincronizzazione nel server di pubblicazione utilizzato dall'agente di lettura log.|  
|**ins_scripting_proc**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**del_scripting_proc**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**upd_scripting_proc**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**custom_script**|**int**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**fire_triggers_on_snapshot**|**bit**|Questa colonna non viene utilizzata ed è inclusa solo per rendere la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) della tabella **IHarticles** compatibile con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizzata per gli articoli di SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**instance_id**|**int**|Identifica l'istanza corrente del log degli articoli per la tabella pubblicata.|  
|**use_default_datatypes**|**bit**|Indica se l'articolo utilizza i mapping dei tipi di dati predefiniti. il valore **1** indica che vengono utilizzati i mapping dei tipi di dati predefiniti.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)  
  
  
