---
description: managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL)
title: managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_get_backup_diagnostics_TSQL
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics_TSQL
- smart_admin.sp_get_backup_diagnostics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics
ms.assetid: 2266a233-6354-464b-91ec-824ca4eb9ceb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fbda9abd944665d3f0d52c174e0b1a5c1e488fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205283"
---
# <a name="managed_backupsp_get_backup_diagnostics-transact-sql"></a>managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Restituisce gli eventi estesi registrati da Smart Admin.  
  
 Usare questa stored procedure per monitorare gli eventi estesi registrati da Smart admin. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] gli eventi vengono registrati in questo sistema e possono essere esaminati e monitorati usando questo stored procedure.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
managed_backup.sp_get_backup_diagnostics [@xevent_channel = ] 'event type' [, [@begin_time = ] 'time1' ] [, [@end_time = ] 'time2'VARCHAR(255) = 'Xevent',@begin_time DATETIME = NULL,@end_time DATETIME = NULL  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 @xevent_channel  
 Tipo di evento esteso. Il valore predefinito è impostato per restituire tutti gli eventi registrati per i 30 minuti precedenti. Gli eventi registrati dipendono dal tipo di eventi estesi abilitati. È possibile utilizzare questo parametro per filtrare la stored procedure per mostrare solo gli eventi di un determinato tipo. È possibile specificare il nome completo dell'evento o specificare una sottostringa, ad esempio: **' admin**', **' Analytical '**, **' Operational '** e **' debug '**. È di tipo @event_channel **VARCHAR (255)**.  
  
 Per ottenere un elenco dei tipi di evento attualmente abilitati, usare la funzione **managed_backup. fn_get_current_xevent_settings** .  
  
 [@begin_time  
 Inizio del periodo di tempo a partire dal quale devono essere visualizzati gli eventi. Il @begin_time parametro è di tipo DateTime e il valore predefinito è null. Se non è specificato, vengono visualizzati gli eventi a partire dai 30 minuti precedenti.  
  
 @end_time  
 Fine del periodo di tempo fino al quale devono essere visualizzati gli eventi. Il @end_time parametro è DataTime e il valore predefinito è null.  Se non è specificato, vengono visualizzati gli eventi fino all'ora corrente.  
  
## <a name="table-returned"></a>Tabella restituita  
 Questa stored procedure restituisce una tabella con le informazioni seguenti:  
  
| Nome colonna | Tipo di dati | Descrizione |  
| ----------- | --------- | ----------- |
|event_type|NVARCHAR (512)|Tipo di evento esteso.|  
|Evento|NVARCHAR (512)|Riepilogo dei registri eventi.|  
|Timestamp|timestamp|Timestamp dell'evento che mostra quando è stato generato l'evento.|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Sono necessarie le autorizzazioni **Execute** per la stored procedure. Sono inoltre necessarie le autorizzazioni **View Server state** , poiché vengono chiamati internamente altri oggetti di sistema che richiedono questa autorizzazione.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti tutti gli eventi registrati per i 30 minuti precedenti.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics  
  
```  
  
 Nell'esempio seguente vengono restituiti tutti gli eventi registrati per un intervallo di tempo specifico.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Admin',  
  @begin_time = '2013-06-01', @end_time = '2013-06-10'  
  
```  
  
 Nell'esempio seguente vengono restituiti tutti gli eventi analitici registrati per i 30 minuti precedenti.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Analytic'  
  
```  
  
  
