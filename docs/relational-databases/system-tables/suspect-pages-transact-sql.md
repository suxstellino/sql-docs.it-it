---
description: suspect_pages (Transact-SQL)
title: suspect_pages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- suspect_page_table
- suspect_page_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- suspect_pages system table
- suspect pages [SQL Server]
ms.assetid: 119c8d62-eea8-44fb-bf72-de469c838c50
author: cawrites
ms.author: chadam
ms.openlocfilehash: c2b5952f12c857a7bf58c066744e1c366279b1e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096132"
---
# <a name="suspect_pages-transact-sql"></a>suspect_pages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per pagina in cui si è verificato un errore 823 secondario o un errore 824. Le pagine sono elencate in questa tabella perché è possibile che siano errate. Quando viene ripristinata una pagina sospetta, il relativo stato viene aggiornato nella colonna **event_type** .  
  
 La tabella seguente, che ha un limite di 1.000 righe, viene archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database al quale è associata la pagina corrente.|  
|**file_id**|**int**|ID del file nel database.|  
|**page_id**|**bigint**|ID della pagina sospetta. Ogni pagina possiede un ID, rappresentato da un valore a 32 bit, che ne identifica la posizione all'interno del database. Il **page_id** è l'offset nel file di dati della pagina da 8 KB. Ogni ID di pagina è univoco in un file.|  
|**event_type**|**int**|Tipo di errore. I tipi possibili sono:<br /><br /> 1 = errore 823 che indica una pagina sospetta (ad esempio, un errore del disco) o errore 824 diverso da un checksum errato o una pagina incompleta (ad esempio, un ID di pagina errato).<br /><br /> 2 = checksum errato.<br /><br /> 3 = pagina incompleta.<br /><br /> 4 = ripristinata (la pagina è stata ripristinata dopo essere stata contrassegnata come non corretta).<br /><br /> 5 = corretta (la pagina è stata corretta da DBCC).<br /><br /> 7 = deallocata da DBCC.|  
|**error_count**|**int**|Numero di volte che si è verificato l'errore.|  
|**last_update_date**|**datetime**|Timestamp relativo all'ultimo aggiornamento.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Chiunque abbia accesso a **msdb** può leggere i dati nella tabella **suspect_pages** . Chiunque disponga dell'autorizzazione UPDATE nella tabella suspect_pages può aggiornare i relativi record. I membri del ruolo predefinito del database **db_owner** in **msdb** o del ruolo predefinito del server **sysadmin** possono inserire, aggiornare ed eliminare i record.  
  
## <a name="see-also"></a>Vedere anche  
 [Ripristino di pagine &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Classe di evento database sospetto data page](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)   
 [Tabelle di sistema &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [Gestione della tabella suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
