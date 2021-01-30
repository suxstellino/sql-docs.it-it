---
description: sys.fn_virtualfilestats (Transact-SQL)
title: sys.fn_virtualfilestats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_virtualfilestats_TSQL
- fn_virtualfilestats
dev_langs:
- TSQL
helpviewer_keywords:
- I/O [SQL Server], statistics
- fn_virtualfilestats function
- sys.fn_virtualfilestats function
- statistical information [SQL Server], I/O
ms.assetid: 96b28abb-b059-48db-be2b-d60fe127f6aa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 755ec3806e72ab535588c6da15075ebf0beb4043
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187393"
---
# <a name="sysfn_virtualfilestats-transact-sql"></a>sys.fn_virtualfilestats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce le statistiche di I/O per i file di database, compresi i file di log. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] queste informazioni sono disponibili anche nella vista a gestione dinamica [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) .  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_virtualfilestats ( { database_id | NULL } , { file_id | NULL } )  
```  
  
## <a name="arguments"></a>Argomenti  
 *database_id* | NULL  
 ID del database. *database_id* è di tipo **int** e non prevede alcun valore predefinito. Specificare NULL per restituire informazioni per tutti i database presenti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *file_id* | NULL  
 ID del file. *file_id* è di **tipo int** e non prevede alcun valore predefinito. Specificare NULL per restituire informazioni per tutti i file del database.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**DbId**|**smallint**|ID del database.|  
|**FileId**|**smallint**|ID di file.|  
|**TimeStamp**|**bigint**|Timestamp del prelevamento dei dati. **int** nelle versioni precedenti a [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] . |  
|**NumberReads**|**bigint**|Numero di letture eseguite nel file.|  
|**BytesRead**|**bigint**|Numero di letture di byte eseguite nel file.|  
|**IoStallReadMS**|**bigint**|Periodo di tempo totale, in millisecondi, durante il quale gli utenti attendono il completamento delle operazioni di lettura I/O nel file.|  
|**NumberWrites**|**bigint**|Numero di scritture eseguite nel file.|  
|**BytesWritten**|**bigint**|Numero di scritture di byte eseguite nel file.|  
|**IoStallWriteMS**|**bigint**|Periodo di tempo totale, in millisecondi, durante il quale gli utenti attendono il completamento delle operazioni di scrittura I/O nel file.|  
|**IoStallMS**|**bigint**|Somma di **IoStallReadMS** e **IoStallWriteMS**.|  
|**FileHandle**|**bigint**|Valore dell'handle di file.|  
|**BytesOnDisk**|**bigint**|Dimensioni fisiche del file (numero di byte) su disco.<br /><br /> Per i file di database, si tratta dello stesso valore delle **dimensioni** in **sys.database_files**, ma è espresso in byte anziché in pagine.<br /><br /> Per i file sparse di snapshot del database, è lo spazio utilizzato dal sistema operativo per il file.|  
  
## <a name="remarks"></a>Commenti  
 **fn_virtualfilestats** è una funzione con valori di tabella di sistema che fornisce informazioni statistiche, ad esempio il numero totale di operazioni di i/o eseguite su un file. Questa funzione consente di tenere traccia della durata dell'attesa da parte degli utenti per la lettura o la scrittura in un file. Consente inoltre di identificare i file in cui si verifica un elevato numero di operazioni di I/O.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-statistical-information-for-a-database"></a>R. Visualizzazione di informazioni statistiche per un database  
 Nell'esempio seguente vengono visualizzate informazioni statistiche per il file con ID 1 nel database con ID `1`.  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(1, 1);  
GO  
```  
  
### <a name="b-displaying-statistical-information-for-a-named-database-and-file"></a>B. Visualizzazione di informazioni statistiche per un database e un file con nome  
 Nell'esempio seguente vengono visualizzate informazioni statistiche per il file di log nel database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La funzione `DB_ID` di sistema viene utilizzata per specificare il parametro *database_id* .  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="c-displaying-statistical-information-for-all-databases-and-files"></a>C. Visualizzazione di informazioni statistiche per tutti i database e i file  
 Nell'esempio seguente vengono visualizzate informazioni statistiche per tutti i file di tutti i database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(NULL,NULL);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DB_ID &#40;&#41;Transact-SQL ](../../t-sql/functions/db-id-transact-sql.md)   
 [FILE_IDEX &#40;Transact-SQL&#41;](../../t-sql/functions/file-idex-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
