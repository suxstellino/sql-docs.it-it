---
description: sp_helpfile (Transact-SQL)
title: sp_helpfile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfile
- sp_helpfile_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfile
ms.assetid: 1546e0ae-5a99-4e01-9eb9-d147fa65884c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 87bf389e2cd28fdf031308c116c32105d2f3c215
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204424"
---
# <a name="sp_helpfile-transact-sql"></a>sp_helpfile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce i nomi fisici e gli attributi dei file associati al database corrente. Utilizzare questa stored procedure per determinare i nomi dei file da collegare o scollegare dal server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpfile [ [ @filename= ] 'name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @filename = ] 'name'` Nome logico di qualsiasi file nel database corrente. *Name* è di **tipo sysname** e il valore predefinito è null. Se il *nome* non è specificato, vengono restituiti gli attributi di tutti i file nel database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome logico del file.|  
|**fileid**|**smallint**|Identificatore numerico del file. Se il *nome* è specificato, non viene restituito *.*|  
|**filename**|**nchar (260)**|Nome fisico del file.|  
|**filegroup**|**sysname**|Filegroup a cui appartiene il file.<br /><br /> NULL = Il file è un file di log. Questo tipo di file non viene mai incluso in un filegroup.|  
|**size**|**nvarchar (15)**|Dimensione del file in kilobyte.|  
|**MaxSize**|**nvarchar (15)**|Dimensioni massime consentite per il file. Se questo campo include il valore UNLIMITED, le dimensioni del file possono aumentare fino a riempire il disco.|  
|**crescita**|**nvarchar (15)**|Incremento per l'aumento delle dimensioni del file. Indica la quantità di spazio aggiunta al file ogni volta che è richiesto spazio aggiuntivo.<br /><br /> 0 = la dimensione del file è fissa e non aumenterà.|  
|**utilizzo**|**varchar (9)**|Per il file di dati, il valore è **"solo dati"** e per il file di log il valore è **"log only"**.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono restituite informazioni sui file nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfile;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfilegroup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Filegroup e file di database](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
