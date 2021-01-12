---
description: sys.syscacheobjects (Transact-SQL)
title: sys.syscacheobjects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscacheobjects_TSQL
- sys.syscacheobjects
- syscacheobjects
- syscacheobjects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscacheobjects system table
- sys.syscacheobjects compatibility view
ms.assetid: 9b14f37c-b7f5-4f71-b070-cce89a83f69e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4b33b97a5b3753d63e1df4759d26970cf6359ee2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097809"
---
# <a name="syssyscacheobjects-transact-sql"></a>sys.syscacheobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni sull'utilizzo della cache.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**bucketid**|**int**|ID bucket. I valori sono compresi tra 0 e le dimensioni della directory -1. Le dimensioni della directory corrispondono a quelle della tabella hash.|  
|**cacheobjtype**|**nvarchar(17)**|Tipo di oggetto nella cache:<br /><br /> Piano compilato<br /><br /> Piano eseguibile<br /><br /> Albero di analisi<br /><br /> Cursore<br /><br /> Stored procedure estesa|  
|**objtype**|**nvarchar(8)**|Tipo di oggetto:<br /><br /> Stored procedure<br /><br /> Istruzione preparata<br /><br /> Query ad hoc ( [!INCLUDE[tsql](../../includes/tsql-md.md)] inviate come eventi di linguaggio dalle utilità **SQLCMD** o **osql** , anziché da chiamate a procedure remote)<br /><br /> ReplProc (procedura della replica)<br /><br /> Trigger<br /><br /> Visualizza<br /><br /> Predefinito<br /><br /> Tabella utente<br /><br /> Tabella di sistema<br /><br /> Controllo<br /><br /> Regola|  
|**objid**|**int**|Una delle chiavi principali utilizzate per la ricerca di un oggetto nella cache. Si tratta dell'ID oggetto archiviato in **sysobjects** per gli oggetti di database (procedure, viste, trigger e così via). Per gli oggetti della cache come SQL ad hoc o preparata, **objid** è un valore generato internamente.|  
|**dbid**|**smallint**|ID del database in cui è stato compilato l'oggetto della cache.|  
|**dbidexec**|**smallint**|ID del database da cui viene eseguita la query.<br /><br /> Per la maggior parte degli oggetti, **dbidexec** ha lo stesso valore di **dbid**.<br /><br /> Per le viste di sistema, **dbidexec** è l'ID del database da cui viene eseguita la query.<br /><br /> Per le query ad hoc, **dbidexec** è 0. Questo significa che **dbidexec** ha lo stesso valore di **dbid**.|  
|**UID**|**smallint**|Indica il creatore dei piani per le query ad hoc e dei piani preparati.<br /><br /> -2 = Il batch inviato non dipende dalla risoluzione implicita del nome e può essere condiviso da diversi utenti. Questo è il metodo preferito. Qualsiasi altro valore rappresenta l'ID dell'utente che invia la query al database.<br /><br /> Causa un errore di overflow o restituisce NULL se il numero di utenti e ruoli è maggiore di 32.767.|  
|**refcounts**|**int**|Numero degli altri oggetti della cache che fanno riferimento a questo oggetto della cache. Il valore di base è 1.|  
|**usecounts**|**int**|Numero di utilizzi dell'oggetto della cache dall'inizio.|  
|**pagesused**|**int**|Numero di pagine utilizzate dall'oggetto della cache.|  
|**setopts**|**int**|Impostazioni delle opzioni SET che hanno effetto su un piano compilato. Queste impostazioni fanno parte della chiave della cache. Eventuali modifiche dei valori di questa colonna indicano che gli utenti hanno modificato le opzioni SET. Di seguito vengono descritte alcune di queste opzioni:<br /><br /> **ANSI_PADDING**<br /><br /> **FORCEPLAN**<br /><br /> **CONCAT_NULL_YIELDS_NULL**<br /><br /> **ANSI_WARNINGS**<br /><br /> **ANSI_NULLS**<br /><br /> **QUOTED_IDENTIFIER**<br /><br /> **ANSI_NULL_DFLT_ON**<br /><br /> **ANSI_NULL_DFLT_OFF**|  
|**langid**|**smallint**|ID della lingua. ID della lingua della connessione in cui è stato creato l'oggetto della cache.|  
|**DateFormat**|**smallint**|Formato della data della connessione in cui è stato creato l'oggetto della cache.|  
|**Stato**|**int**|Indica se l'oggetto della cache è un piano di cursore. Attualmente viene utilizzato solo il bit meno significativo.|  
|**lasttime**|**bigint**|Disponibile solo per compatibilità con le versioni precedenti. Restituisce sempre 0.|  
|**maxexectime**|**bigint**|Disponibile solo per compatibilità con le versioni precedenti. Restituisce sempre 0.|  
|**avgexectime**|**bigint**|Disponibile solo per compatibilità con le versioni precedenti. Restituisce sempre 0.|  
|**lastreads**|**bigint**|Disponibile solo per compatibilità con le versioni precedenti. Restituisce sempre 0.|  
|**lastwrites**|**bigint**|Disponibile solo per compatibilità con le versioni precedenti. Restituisce sempre 0.|  
|**sqlbytes**|**int**|Lunghezza in byte della definizione della procedura o del batch inviato.|  
|**sql**|**nvarchar (3900)**|Definizione del modulo o primi 3900 caratteri del batch inviato.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  

