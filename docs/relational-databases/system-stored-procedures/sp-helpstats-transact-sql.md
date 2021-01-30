---
description: sp_helpstats (Transact-SQL)
title: sp_helpstats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpstats
- sp_helpstats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpstats
ms.assetid: 00ab3cfd-2736-4fc0-b1b2-16dd49fb2fe5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c3b0d76b08d8be5aefcbfceee570d893d6ca4659
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198721"
---
# <a name="sp_helpstats-transact-sql"></a>sp_helpstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni statistiche sulle colonne e gli indici della tabella specificata.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] Per ottenere informazioni sulle statistiche, eseguire una query sulle viste del catalogo [sys. stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) e [sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpstats[ @objname = ] 'object_name'   
     [ , [ @results = ] 'value' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @objname = ] 'object_name'` Specifica la tabella in cui fornire le informazioni sulle statistiche. *object_name* è di **tipo nvarchar (520)** e non può essere null. È possibile specificare un nome composto da una o due parti.  
  
`[ @results = ] 'value'` Specifica l'estensione delle informazioni da fornire. Le voci valide sono **All** e **stats**. **All** elenca le statistiche per tutti gli indici e le colonne con statistiche create. **Statistiche** elenca solo le statistiche non associate a un indice. *value* è di **tipo nvarchar (5)** e il valore predefinito è stats.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Nella tabella seguente vengono descritte le colonne del set di risultati.  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**statistics_name**|Nome delle statistiche. Restituisce **sysname** e non può essere null.|  
|**statistics_keys**|Chiavi su cui sono basate le statistiche. Restituisce **nvarchar (2078)** e non può essere null.|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare DBCC SHOW_STATISTICS per visualizzare informazioni statistiche dettagliate su indici o statistiche specifici. Per ulteriori informazioni, vedere [DBCC SHOW_STATISTICS &#40;Transact-sql&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) e [sp_helpindex &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono create statistiche a colonna singola per tutte le colonne appropriate di tutte le tabelle utente nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] eseguendo la stored procedure `sp_createstats`. Viene poi eseguita la stored procedure `sp_helpstats` per recuperare le statistiche risultanti create nella tabella `Customer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_createstats;  
GO  
EXEC sp_helpstats   
@objname = 'Sales.Customer',  
@results = 'ALL';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `statistics_name               statistics_keys`  
  
 `----------------------------  ----------------`  
  
 `_WA_Sys_00000003_22AA2996     AccountNumber`  
  
 `AK_Customer_AccountNumber     AccountNumber`  
  
 `AK_Customer_rowguid           rowguid`  
  
 `CustomerType                  CustomerType`  
  
 `IX_Customer_TerritoryID       TerritoryID`  
  
 `ModifiedDate                  ModifiedDate`  
  
 `PK_Customer_CustomerID        CustomerID`  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
