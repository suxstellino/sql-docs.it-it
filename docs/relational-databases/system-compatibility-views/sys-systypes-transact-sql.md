---
description: sys.systypes (Transact-SQL)
title: Tipi di sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.systypes_TSQL
- systypes_TSQL
- sys.systypes
- systypes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.systypes compatibility view
- systypes system table
ms.assetid: 1b0b1d0c-5f7b-470b-bd52-8bfa922d7889
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7f00dab371048ed8a1b39cb702897ce5f481c377
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160646"
---
# <a name="syssystypes-transact-sql"></a>sys.systypes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni tipo di dati di sistema o definito dall'utente nel database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del tipo di dati.|  
|**xtype**|**tinyint**|Tipo di dati per l'archiviazione fisica.|  
|**Stato**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|Tipo di dati esteso definito dall'utente. Causa un errore di overflow o restituisce NULL se il numero di tipi di dati è maggiore di 32.767.|  
|**length**|**smallint**|Lunghezza fisica del tipo di dati.|  
|**xprec**|**tinyint**|Precisione interna utilizzata dal server, da non utilizzare nelle query.|  
|**xscale**|**tinyint**|Scala interna utilizzata dal server, da non utilizzare nelle query.|  
|**tdefault**|**int**|ID della stored procedure che include i controlli di integrità per questo tipo di dati.|  
|**dominio**|**int**|ID della stored procedure che include i controlli di integrità per questo tipo di dati.|  
|**UID**|**smallint**|ID dello schema del proprietario del tipo.<br /><br /> Per i database aggiornati da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'ID dello schema corrisponde all'ID utente del proprietario.<br /><br /> Importante se si utilizza una delle istruzioni DDL seguenti, è necessario utilizzare la vista del catalogo **\* sys. Types anziché \*sys.systipi. \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) <br /><br /> ALTER AUTHORIZATION ON TYPE<br /><br /> CREATE TYPE<br /><br /> Causa un errore di overflow o restituisce NULL se il numero di utenti e ruoli è maggiore di 32.767.|  
|**riservati**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**CollationID**|**int**|Se è basato su caratteri, **CollationID** è l'ID delle regole di confronto del database corrente. in caso contrario, è NULL.|  
|**UserType**|**smallint**|ID tipo utente. Causa un errore di overflow o restituisce NULL se il numero di tipi di dati è maggiore di 32.767.|  
|**variable**|**bit**|Tipo di dati a lunghezza variabile.<br /><br /> 1 = True<br /><br /> 0 = False|  
|**allownulls**|**bit**|Indica l'impostazione predefinita relativa al supporto dei valori Null per questo tipo di dati. Questo valore predefinito viene sottoposto a override da se viene specificato il supporto di valori null tramite [Create Table](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).|  
|**type**|**tinyint**|Tipo di dati per l'archiviazione fisica.|  
|**printfmt**|**varchar(255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Livello di precisione per il tipo di dati.<br /><br /> -1 = **XML** o tipi di valore di grandi dimensioni.|  
|**scale**|**tinyint**|Scala per il tipo di dati, basata sulla precisione.<br /><br /> NULL = Tipo di dati non numerico.|  
|**confronto**|**sysname**|Se è basato su caratteri, le regole **di confronto sono** le regole di confronto del database corrente. in caso contrario, è NULL.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste di compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
