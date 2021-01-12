---
description: Viste di compatibilità del sistema (Transact-SQL)
title: Viste di compatibilità del sistema (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- compatibility views [SQL Server]
- system tables [SQL Server], compatibility views
- type IDs [SQL Server]
- system views [SQL Server], compatibility
- metadata [SQL Server], views
- backward compatibility [SQL Server], compatibility views
- compatibility [SQL Server], compatibility views
- backward compatibility [SQL Server], system tables
- compatibility [SQL Server], system tables
- user IDs [SQL Server]
ms.assetid: 8e4624f5-9d36-4ce7-9c9e-1fe010fa2122
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7ba0c3b5c023d1d2b6a3d025de7f96f49a4b3c1a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095300"
---
# <a name="system-compatibility-views-transact-sql"></a>Viste di compatibilità del sistema (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Molte delle tabelle di sistema incluse nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono implementate come set di viste. Queste viste sono note come viste di compatibilità e sono disponibili solo per compatibilità con le versioni precedenti. Le viste di compatibilità espongono gli stessi metadati disponibili in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], ma non espongono i metadati correlati a funzionalità introdotte in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive. Quando si utilizzano nuove funzionalità, ad esempio [!INCLUDE[ssSB](../../includes/sssb-md.md)] o il partizionamento, è pertanto necessario passare all'utilizzo delle viste del catalogo.  
  
 Un altro motivo per eseguire l'aggiornamento alle viste del catalogo è rappresentato dal fatto che le colonne delle viste di compatibilità in cui sono archiviati ID utente e ID di tipo possono restituire NULL o attivare overflow aritmetici, dal momento che è possibile creare oltre 32.767 utenti, gruppi e ruoli e 32.767 tipi di dati. Se ad esempio è necessario creare 32.768 utenti e quindi eseguire la query `SELECT * FROM sys.sysusers`, in caso di impostazione di ARITHABORT su ON la query ha esito negativo e viene generato un errore di overflow aritmetico. Se ARITHABORT è impostato su OFF, la colonna **UID** restituisce null.  
  
 Per evitare questi problemi, è consigliabile usare le nuove viste del catalogo in grado di gestire un numero più elevato di ID utente e ID di tipo. Nella tabella seguente sono elencate le colonne soggette a questo tipo di overflow.  
  
|Nome colonna|Vista di compatibilità|Vista di SQL Server 2005|  
|-----------------|------------------------|--------------------------|  
|**xusertype**|**syscolumns**|**sys.columns**|  
|**UserType**|**syscolumns**|**sys.columns**|  
|**memberuid**|**sysmembers**|**sys.database_role_members**|  
|**groupuid**|**sysmembers**|**sys.database_role_members**|  
|**UID**|**sysobjects**|**sys.objects**|  
|**UID**|**sysprotects**|**sys.database_permissions**<br /><br /> **sys.server_permissions**|  
|**concedente**|**sysprotects**|**sys.database_permissions**<br /><br /> **sys.server_permissions**|  
|**xusertype**|**systypes**|**sys.types**|  
|**UID**|**systypes**|**sys.types**|  
|**UID**|**sysusers**|**sys.database_principals**|  
|**altuid**|**sysusers**|**sys.database_principals**|  
|**gid**|**sysusers**|**sys.database_principals**|  
|**UID**|**syscacheobjects**|**sys.dm_exec_plan_attributes**|  
|**UID**|**sysprocesses**|**sys.dm_exec_requests**|  
  
 Quando si fa riferimento a un database utente, le tabelle di sistema che sono state annunciate come deprecate in SQL Server 2000 (ad esempio **syslanguages** o **syscacheobjects**) vengono ora associate alla vista di compatibilità back nello schema **sys** . Poiché le tabelle di sistema di SQL Server 2000 sono state deprecate per più versioni, tale modifica non viene considerata una modifica di rilievo.  
  
 Esempio: se un utente crea una tabella utente denominata **syslanguages** in un database utente, in SQL Server 2008, l'istruzione `SELECT * from dbo.syslanguages;` in tale database restituirà i valori della tabella utente. A partire da SQL Server 2012, questa procedura restituirà i dati della vista di sistema **sys.sysle lingue**.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
