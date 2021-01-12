---
description: sysmergepartitioninfo (Transact-SQL)
title: sysmergepartitioninfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepartitioninfo_TSQL
- sysmergepartitioninfo
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfo system table
ms.assetid: 7429ad2c-dd33-4f7d-89cc-700e083af518
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a94f14212f2603639c1d272da4d35e9bea9fb29
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100499"
---
# <a name="sysmergepartitioninfo-transact-sql"></a>sysmergepartitioninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fornisce informazioni sulle partizioni per ogni articolo. Contiene una riga per ogni articolo di merge definito nel database locale. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**artid**|**uniqueidentifier**|Identificatore univoco per l'articolo specificato.|  
|**pubid**|**uniqueidentifier**|Numero di identificazione univoco per la pubblicazione specificata. Viene generato quando si aggiunge la pubblicazione.|  
|**partition_view_id**|**int**|ID della vista di partizione sulla tabella corrente. Nella vista viene riportato il mapping tra ogni riga nell'articolo e l'ID di partizione corrispondente a cui appartiene.|  
|**repl_view_id**|**int**|Da aggiungere.|  
|**partition_deleted_view_rule**|**nvarchar(4000)**|Istruzione SQL utilizzata all'interno di un trigger di replica di tipo merge per recuperare l'ID di partizione per ogni riga eliminata o aggiornata in base ai relativi valori di colonna precedenti.|  
|**partition_inserted_view_rule**|**nvarchar(4000)**|Istruzione SQL utilizzata all'interno di un trigger di replica di tipo merge per recuperare l'ID di partizione per ogni riga inserita o aggiornata in base ai relativi nuovi valori di colonna.|  
|**membership_eval_proc_name**|**sysname**|Nome della procedura che valuta gli ID di partizione correnti delle righe in **MSmerge_contents**.|  
|**column_list**|**nvarchar(4000)**|Elenco separato da virgole delle colonne replicate in un articolo.|  
|**column_list_blob**|**nvarchar(4000)**|Elenco separato da virgole delle colonne replicate in un articolo, comprese le colonne BLOB.|  
|**expand_proc**|**sysname**|Nome della procedura che restituisce nuovamente gli ID di partizione per tutte le righe figlio di una riga padre appena inserita e per le righe padre sottoposte a modifica a livello di partizione oppure che sono state eliminate.|  
|**logical_record_parent_nickname**|**int**|Nome alternativo del padre di livello principale di un articolo specifico in un record logico.|  
|**logical_record_view**|**int**|Vista che ha come output la colonna rowguid dell'articolo padre di livello principale corrispondente a ogni colonna rowguid figlio.|  
|**logical_record_deleted_view_rule**|**nvarchar(4000)**|Simile a **logical_record_view**, ad eccezione del fatto che visualizza le righe figlio nella tabella "Deleted" nei trigger UPDATE e DELETE.|  
|**logical_record_level_conflict_detection**|**bit**|Indica se è necessario rilevare i conflitti a livello di record logico oppure a livello di riga o colonna.<br /><br /> **0** = viene utilizzato il rilevamento dei conflitti a livello di riga o di colonna.<br /><br /> **1** = viene utilizzato il rilevamento dei conflitti tra record logici, in cui una modifica in una riga del server di pubblicazione e la modifica in una riga distinta lo stesso record logico nel Sottoscrittore viene gestita come un conflitto.<br /><br /> Se questo valore è **1**, è possibile utilizzare solo la risoluzione dei conflitti a livello di record logico.|  
|**logical_record_level_conflict_resolution**|**bit**|Indica se è necessario risolvere i conflitti a livello di record logico oppure a livello di riga o colonna.<br /><br /> **0** = viene utilizzata la risoluzione a livello di riga o di colonna.<br /><br /> **1** = in caso di conflitto, l'intero record logico del vincitore sovrascrive l'intero record logico sul lato perduto.<br /><br /> Il valore **1** può essere utilizzato sia con il rilevamento a livello di record logico che con il rilevamento a livello di riga o di colonna.|  
|**partition_options**|**tinyint**|Definisce il modo in cui vengono partizionati i dati nell'articolo. Ciò consente di ottimizzare le prestazioni se tutte le righe appartengono a un'unica partizione o a un'unica sottoscrizione. *partition_options* può essere uno dei valori seguenti.<br /><br /> **0** = il filtro per l'articolo è statico oppure non restituisce un subset univoco di dati per ogni partizione, ovvero una partizione "sovrapposta".<br /><br /> **1** = le partizioni sono sovrapposte e gli aggiornamenti DML eseguiti nel Sottoscrittore non possono modificare la partizione a cui appartiene una riga.<br /><br /> **2** = il filtro per l'articolo restituisce partizioni non sovrapposte, ma più sottoscrittori possono ricevere la stessa partizione.<br /><br /> **3** = il filtro per l'articolo restituisce partizioni non sovrapposte univoche per ogni sottoscrizione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
