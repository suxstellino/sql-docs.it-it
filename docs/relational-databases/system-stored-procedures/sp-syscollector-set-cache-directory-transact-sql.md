---
description: sp_syscollector_set_cache_directory (Transact-SQL)
title: sp_syscollector_set_cache_directory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_set_cache_directory_TSQL
- sp_syscollector_set_cache_directory
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_set_cache_directory stored procedure
ms.assetid: df56d5a5-8961-494f-a745-d752ca63805a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c77fa4764cc1456ba9a3cb2f9923182d0d384e0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185270"
---
# <a name="sp_syscollector_set_cache_directory-transact-sql"></a>sp_syscollector_set_cache_directory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Specifica la directory in cui vengono archiviati i dati raccolti prima di essere caricati nel data warehouse di gestione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syscollector_set_cache_directory [ @cache_directory = ] 'cache_directory'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @cache_directory = ] 'cache_directory'` Directory nella file system in cui i dati raccolti vengono archiviati temporaneamente. *cache_directory* è di **tipo nvarchar (255)** e il valore predefinito è null. Se non viene specificato alcun valore, viene utilizzata la directory temporanea predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario disabilitare l'agente di raccolta dati prima di modificare la configurazione della directory della cache. La stored procedure ha esito negativo se l'agente di raccolta dati è abilitato. Per altre informazioni, vedere [abilitare o disabilitare la raccolta di dati](../../relational-databases/data-collection/enable-or-disable-data-collection.md)e [gestire la raccolta dei dati](../../relational-databases/data-collection/manage-data-collection.md).  
  
 Non è necessario che la directory specificata esista al momento dell'esecuzione del sp_syscollector_set_cache_directory. Tuttavia, i dati non possono essere memorizzati nella cache e caricati fino a quando non viene creata la directory. È consigliabile pertanto creare la directory prima di eseguire questa stored procedure.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa procedura, è richiesta l'appartenenza al ruolo predefinito del database dc_admin (con autorizzazione EXECUTE) .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene disabilitato l'agente di raccolta dati, viene impostata la directory della cache per l'agente di raccolta dati su `D:\tempdata` e quindi viene abilitato l'agente di raccolta dati.  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXEC dbo.sp_syscollector_set_cache_directory N'D:\tempdata';  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_window &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)  
  
  
