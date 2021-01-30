---
description: sp_showrowreplicainfo (Transact-SQL)
title: sp_showrowreplicainfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showrowreplicainfo_TSQL
- sp_showrowreplicainfo
helpviewer_keywords:
- sp_showrowreplicainfo
ms.assetid: 6a9dbc1a-e1e1-40c4-97cb-8164a2288f76
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f2a66e8e6123adef864ca2beadef31cd4a9cbffe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193485"
---
# <a name="sp_showrowreplicainfo-transact-sql"></a>sp_showrowreplicainfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza informazioni su una riga di una tabella utilizzata come articolo in repliche di tipo merge. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_showrowreplicainfo [ [ @ownername = ] 'ownername' ]  
    [ , [ @tablename =] 'tablename' ]   
        , [ @rowguid =] rowguid   
    [ , [ @show = ] 'show' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @ownername = ] 'ownername'` Nome del proprietario della tabella. *OwnerName* è di **tipo sysname** e il valore predefinito è null. Questo parametro risulta utile per differenziare le tabelle quando un database contiene più tabelle aventi lo stesso nome ma appartenenti a proprietari diversi.  
  
`[ @tablename = ] 'tablename'` Nome della tabella contenente la riga per la quale vengono restituite le informazioni. *TableName* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @rowguid = ] rowguid` Identificatore univoco della riga. *rowguid* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito.  
  
`[ @show = ] 'show'` Determina la quantità di informazioni da restituire nel set di risultati. *show* è di **tipo nvarchar (20)** e il valore predefinito è both. Se **Row**, vengono restituite solo le informazioni sulla versione di riga. Se **colonne**, vengono restituite solo le informazioni sulla versione della colonna. Se sono **entrambi**, vengono restituite entrambe le informazioni di riga e colonna.  
  
## <a name="result-sets-for-row-information"></a>Set di risultati per informazioni sulla riga  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**server_name**|**sysname**|Nome del server che include il database in cui è stata immessa la voce sulla versione di riga.|  
|**db_name**|**sysname**|Nome del database in cui è stata immessa la voce.|  
|**db_nickname**|**binary(6)**|Nome alternativo del database in cui è stata immessa la voce.|  
|**version**|**int**|Versione della voce.|  
|**current_state**|**nvarchar (9)**|Restituisce informazioni sullo stato corrente della riga.<br /><br /> i dati della riga **y** rappresentano lo stato corrente della riga.<br /><br /> i dati di **n** righe non rappresentano lo stato corrente della riga.<br /><br /> **\<n/a>** -Non applicabile.<br /><br /> **\<unknown>** -Impossibile determinare lo stato corrente.|  
|**rowversion_table**|**nchar (17)**|Indica se le versioni di riga vengono archiviate nella tabella [MSmerge_contents](../../relational-databases/system-tables/msmerge-contents-transact-sql.md) o nella tabella [MSmerge_tombstone](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md) .|  
|**Commento**|**nvarchar(255)**|Informazioni aggiuntive relative alla voce sulla versione di riga. Questo campo è in genere vuoto.|  
  
## <a name="result-sets-for-column-information"></a>Set di risultati per informazioni sulla colonna  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**server_name**|**sysname**|Nome del server che include il database in cui è stata immessa la voce sulla versione di colonna.|  
|**db_name**|**sysname**|Nome del database in cui è stata immessa la voce.|  
|**db_nickname**|**binary(6)**|Nome alternativo del database in cui è stata immessa la voce.|  
|**version**|**int**|Versione della voce.|  
|**colname**|**sysname**|Nome della colonna di articolo rappresentata dalla voce sulla versione di colonna.|  
|**Commento**|**nvarchar(255)**|Informazioni aggiuntive relative alla voce sulla versione di colonna. Questo campo è in genere vuoto.|  
  
## <a name="result-set-for-both"></a>Set di risultati per informazioni su riga e colonna  
 Se il valore viene scelto per *Mostra* **, vengono restituiti** entrambi i set di risultati di riga e colonna.  
  
## <a name="remarks"></a>Commenti  
 **sp_showrowreplicainfo** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 **sp_showrowreplicainfo** possono essere eseguite solo da membri del ruolo predefinito del database **db_owner** nel database di pubblicazione o dai membri dell'elenco di accesso alla pubblicazione nel database di pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Rilevare e risolvere i conflitti di replica di tipo merge](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
