---
description: sys.sysdatabases (Transact-SQL)
title: Database sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdatabases_TSQL
- sys.sysdatabases_TSQL
- sys.sysdatabases
- sysdatabases
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysdatabases compatibility view
- sysdatabases system table
ms.assetid: 60a93880-62f1-4eda-a886-f046706ba90c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 890ef3b430b44a2ea5c8f407d12fa1c0a709fe86
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753441"
---
# <a name="syssysdatabases-transact-sql"></a>sys.sysdatabases (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni database in un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene installato per la prima volta, **sysdatabases** contiene le voci per i database **Master**, **Model**, **msdb** e **tempdb** .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome database|  
|**dbid**|**smallint**|ID database|  
|**SID**|**varbinary(85)**|ID di sistema del creatore del database|  
|**mode**|**smallint**|Per uso interno. Blocca un database mentre viene creato.|  
|**Stato**|**int**|Bit di stato, alcuni dei quali possono essere impostati utilizzando [ALTER database](../../t-sql/statements/alter-database-transact-sql.md) come indicato di seguito:<br /><br /> 1 = **AutoClose** (alter database)<br /><br /> 4 = **select into/bulkcopy** (alter database using set Recovery)<br /><br /> 8 = **tronca. log in chkpt** (alter database tramite set Recovery)<br /><br /> 16 = **rilevamento pagine incomplete** (alter database)<br /><br /> 32 = **caricamento** in corso<br /><br /> 64 = **pre-ripristino**<br /><br /> 128 = **ripristino**<br /><br /> 256 = **non recuperato**<br /><br /> 512 = **offline** (alter database)<br /><br /> 1024 = **sola lettura** (alter database)<br /><br /> 2048 = **solo dbo use** (alter database using set RESTRICTED_USER)<br /><br /> 4096 = **utente singolo** (alter database)<br /><br /> 32768 = **modalità di emergenza**<br /><br /> 65536 = **checksum** (alter database)<br /><br /> 4194304 = **compattazione automatica** (alter database)<br /><br /> 1073741824 = **chiusura** normale<br /><br /> È possibile attivare più bit contemporaneamente.|  
|**status2**|**int**|16384 = **valore predefinito ANSI null** (alter database)<br /><br /> 65536 = **concat null restituisce null** (alter database)<br /><br /> 131072 = **trigger ricorsivi** (alter database)<br /><br /> 1048576 = **cursore locale predefinito** (alter database)<br /><br /> 8388608 = **identificatore tra virgolette** (alter database)<br /><br /> 33554432 = **chiusura cursore al commit** (alter database)<br /><br /> 67108864 = **valori Null ANSI** (alter database)<br /><br /> 268435456 = **Avvisi ANSI** (alter database)<br /><br /> 536870912 = **full-text enabled** (impostato tramite **sp_fulltext_database**)|  
|**crdate**|**datetime**|Data di creazione|  
|**riservati**|**datetime**|Riservato per utilizzi futuri.|  
|**category**|**int**|Include una mappa di bit di informazioni utilizzate per la replica.<br /><br /> 1 = Pubblicata per una replica snapshot o transazionale.<br /><br /> 2 = Sottoscritta a una pubblicazione snapshot o transazionale.<br /><br /> 4 = Pubblicata per una replica di tipo merge.<br /><br /> 8 = Sottoscritta a una pubblicazione di tipo merge.<br /><br /> 16 = Database di distribuzione.|  
|**cmptlevel**|**tinyint**|Livello di compatibilità del database. Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**filename**|**nvarchar(260)**|Percorso del sistema operativo e nome del file primario del database.<br /><br /> **filename** è visibile a **dbcreator**, **sysadmin**, proprietario del DATABASE con le autorizzazioni create any database o agli utenti autorizzati che dispongono di una delle autorizzazioni seguenti: ALTER ANY database, create any database, View any Definition. Per restituire il percorso e il nome del file, eseguire una query sulla vista di compatibilità dei [ filesys.sys](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md) o sulla visualizzazione [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) .|  
|**version**|**smallint**|Numero di versione interno del codice [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con cui è stato creato il database. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
