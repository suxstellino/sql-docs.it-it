---
description: sp_helpdb (Transact-SQL)
title: sp_helpdb (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdb
- sp_helpdb_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdb
ms.assetid: 4c3e3302-6cf1-4b2b-8682-004049b578c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb199d0fc846bc37a8f7c1272563de99507c795
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176440"
---
# <a name="sp_helpdb-transact-sql"></a>sp_helpdb (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni su un database specifico o su tutti i database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdb [ [ @dbname= ] 'name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'name'` Nome del database per il quale vengono restituite informazioni. *Name* è di **tipo sysname** e non prevede alcun valore predefinito. Se il *nome* non è specificato, **sp_helpdb** segnala tutti i database nella vista del catalogo **sys. databases** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|nome del database.|  
|**db_size**|**nvarchar (13)**|Dimensioni totali del database.|  
|**proprietario**|**sysname**|Proprietario del database, ad esempio **sa**.|  
|**dbid**|**smallint**|ID del database.|  
|**created**|**nvarchar(11)**|Data di creazione del database.|  
|**Stato**|**nvarchar (600)**|Elenco separato da virgola dei valori delle opzioni impostate nel database.<br /><br /> Le opzioni con valori booleani vengono elencate solo se sono abilitate. Le opzioni non booleane sono elencate con i valori corrispondenti nel formato *option_name* = *valore*.<br /><br /> Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).|  
|**compatibility_level**|**tinyint**|Livello di compatibilità del database (60, 65, 70, 80 o 90).|  
  
 Se si specifica *Name* , esiste un set di risultati aggiuntivo che mostra l'allocazione di file per il database specificato.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**nchar (128)**|Nome logico del file.|  
|**fileid**|**smallint**|ID di file.|  
|**filename**|**nchar (260)**|Nome del file del sistema operativo, ovvero nome fisico del file.|  
|**filegroup**|**nvarchar(128)**|Filegroup a cui appartiene il file.<br /><br /> NULL = Il file è un file di log. Questo tipo di file non viene mai incluso in un filegroup.|  
|**size**|**nvarchar (18)**|Dimensione del file espressa in megabyte.|  
|**MaxSize**|**nvarchar (18)**|Dimensioni massime consentite per il file. Se questo campo include il valore UNLIMITED, le dimensioni del file possono aumentare fino a riempire il disco.|  
|**crescita**|**nvarchar (18)**|Incremento per l'aumento delle dimensioni del file. Indica la quantità di spazio aggiunta al file ogni volta che è necessario spazio aggiuntivo.|  
|**utilizzo**|**varchar (9)**|Utilizzo del file. Per un file di dati, il valore è **"solo dati"** e per il file di log il valore è **"log only"**.|  
  
## <a name="remarks"></a>Commenti  
 La colonna **stato** del set di risultati indica le opzioni impostate su on nel database. Tutte le opzioni di database non vengono segnalate dalla colonna **stato** . Per visualizzare un elenco completo delle impostazioni dell'opzione di database correnti, utilizzare la vista del catalogo **sys. databases** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Quando viene specificato un singolo database, è necessaria l'appartenenza al ruolo **public** nel database. Se non si specifica alcun database, è necessaria l'appartenenza al ruolo **public** nel database **Master** .  
  
 Se non è possibile accedere a un database, in **sp_helpdb** viene visualizzato il messaggio di errore 15622 e il maggior volume possibile di informazioni sul database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-information-about-a-single-database"></a>R. Restituzione di informazioni su un solo database  
 Nell'esempio seguente vengono visualizzate informazioni sul database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_helpdb N'AdventureWorks2012';  
```  
  
### <a name="b-returning-information-about-all-databases"></a>B. Restituzione di informazioni su tutti i database  
 Nell'esempio seguente vengono visualizzate informazioni su tutti i database nel server che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
EXEC sp_helpdb;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.master_files &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
