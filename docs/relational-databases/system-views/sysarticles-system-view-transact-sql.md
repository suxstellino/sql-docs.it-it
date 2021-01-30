---
description: sysarticles (vista di sistema) (Transact-SQL)
title: sysarticles (vista di sistema) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticles
- sysarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticles view
ms.assetid: 18f8c9b3-cab7-4e8f-8754-11ac38c3f789
author: stevestein
ms.author: sstein
ms.openlocfilehash: 21e1ca1548eee3fd9b17674cdf8ee7d32845b43f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160339"
---
# <a name="sysarticles-system-view-transact-sql"></a>sysarticles (vista di sistema) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **sysarticles** espone le proprietà degli articoli. Questa vista è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Colonna Identity che offre un numero di ID univoco per l'articolo.|  
|**creation_script**|**nvarchar(255)**|Script dello schema per l'articolo.|  
|**del_cmd**|**nvarchar(255)**|Comando da eseguire a seguito di un'istruzione DELETE; in caso contrario il comando viene ricostruito dal log.|  
|**description**|**nvarchar(255)**|Voce descrittiva per l'articolo.|  
|**dest_table**|**sysname**|Nome della tabella di destinazione.|  
|**filter**|**int**|ID della stored procedure, utilizzato per il partizionamento orizzontale.|  
|**filter_clause**|**ntext**|Clausola WHERE dell'articolo, utilizzata per il filtro orizzontale.|  
|**ins_cmd**|**nvarchar(255)**|Comando da eseguire a seguito di un'istruzione INSERT; in caso contrario il comando viene ricostruito dal log.|  
|**nome**|**sysname**|Nome associato all'articolo, univoco all'interno della pubblicazione.|  
|**objid**|**int**|ID dell'oggetto di tabella pubblicato.|  
|**pubid**|**int**|ID della pubblicazione a cui appartiene l'articolo.|  
|**pre_creation_cmd**|**tinyint**|Comando preliminare per l'istruzione DROP TABLE, DELETE TABLE o TRUNCATE:<br /><br /> **0** = nessuna.<br /><br /> **1** = Elimina.<br /><br /> **2** = Delete.<br /><br /> **3** = troncamento.|  
|**Stato**|**tinyint**|Maschera di bit delle opzioni e dello stato dell'articolo, che può corrispondere al risultato dell'applicazione dell'operatore OR logico bit per bit a uno o più dei valori seguenti:<br /><br /> **1** = l'articolo è attivo.<br /><br /> **8** = include il nome della colonna nelle istruzioni INSERT.<br /><br /> **16** = usa istruzioni con parametri.<br /><br /> **24** = include il nome della colonna nelle istruzioni INSERT e usa istruzioni con parametri.<br /><br /> **64** = la partizione orizzontale per l'articolo è definita da una sottoscrizione trasformabile.<br /><br /> Ad esempio, un articolo attivo che utilizza istruzioni con parametri avrà il valore **17** in questa colonna. Il valore **0** indica che l'articolo è inattivo e non è stata definita alcuna proprietà aggiuntiva.|  
|**sync_objid**|**int**|ID della tabella o della vista che rappresenta la definizione dell'articolo.|  
|**type**|**tinyint**|Tipo di articolo:<br /><br /> **1** = articolo basato su log.<br /><br /> **3** = articolo basato su log con filtro manuale.<br /><br /> **5** = articolo basato su log con vista manuale.<br /><br /> **7** = articolo basato su log con filtro manuale e vista manuale.<br /><br /> **8** = esecuzione di stored procedure.<br /><br /> **24** = esecuzione stored procedure serializzabile.<br /><br /> **32** = stored procedure (solo schema).<br /><br /> **64** = View (solo schema).<br /><br /> **128** = funzione (solo schema).|  
|**upd_cmd**|**nvarchar(255)**|Comando da eseguire a seguito di un'istruzione UPDATE; in caso contrario il comando viene ricostruito dal log.|  
|**schema_option**|**binario (8)**|Maschera di bit delle opzioni di generazione dello schema per l'articolo, che controllano le parti dello schema dell'articolo inserite nello script per il recapito al Sottoscrittore. Per altre informazioni sulle opzioni di schema, vedere [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Proprietario della tabella nel database di destinazione.|  
|**ins_scripting_proc**|**int**|Stored procedure o script personalizzato registrato che viene eseguito durante la replica di un'istruzione INSERT.|  
|**del_scripting_proc**|**int**|Stored procedure o script o personalizzato registrato che viene eseguito durante la replica di un'istruzione DELETE.|  
|**upd_scripting_proc**|**int**|Stored procedure o script personalizzato registrato che viene eseguito durante la replica di un'istruzione UPDATE.|  
|**custom_script**|**nvarchar(2048)**|Stored procedure o script personalizzato registrato che viene eseguito alla fine del trigger DDL.|  
|**fire_triggers_on_snapshot**|**bit**|Indica se i trigger replicati vengono eseguiti o meno quando lo snapshot viene applicato. I possibili valori sono i seguenti.<br /><br /> **0** = i trigger non vengono eseguiti.<br /><br /> **1** = i trigger vengono eseguiti.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sysarticles &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sysarticles-transact-sql.md)  
  
  
