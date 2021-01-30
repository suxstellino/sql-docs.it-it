---
description: sp_helpmergepartition (Transact-SQL)
title: sp_helpmergepartition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepartition
- sp_helpmergepartition_TSQL
helpviewer_keywords:
- sp_helpmergepartition
ms.assetid: 184188cc-f519-445d-97ce-aae38f1eb550
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 06994bb4f0e606f0d67e797603e6f4a23899076b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185730"
---
# <a name="sp_helpmergepartition-transact-sql"></a>sp_helpmergepartition (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulla partizione per la pubblicazione di tipo merge specificata. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergepartition [ @publication= ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]  
    [ , [ @host_name = ] 'host_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @suser_sname = ] 'suser_sname'` Valore SUSER_SNAME utilizzato per definire una partizione. *SUSER_SNAME* è di **tipo sysname** e il valore predefinito è null. Specificare questo parametro per limitare il set di risultati alle partizioni in cui SUSER_SNAME corrisponde al valore specificato.  
  
> [!NOTE]  
>  Quando viene specificato *SUSER_SNAME* , *HOST_NAME* deve essere null  
  
`[ @host_name = ] 'host_name'` Valore HOST_NAME utilizzato per definire una partizione. *HOST_NAME* è di **tipo sysname** e il valore predefinito è null. Specificare questo parametro per limitare il set di risultati alle partizioni in cui HOST_NAME corrisponde al valore specificato.  
  
> [!NOTE]  
>  Quando viene specificato *SUSER_SNAME* , *HOST_NAME* deve essere null  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**partizione**|**int**|Identifica la partizione del Sottoscrittore.|  
|**host_name**|**sysname**|Valore utilizzato durante la creazione della partizione per una sottoscrizione filtrata in base al valore della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) nel Sottoscrittore.|  
|**suser_sname**|**sysname**|Valore utilizzato durante la creazione della partizione per una sottoscrizione filtrata in base al valore della funzione [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) nel Sottoscrittore.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Percorso dello snapshot dei dati filtrati per la partizione del Sottoscrittore.|  
|**date_refreshed**|**datetime**|Data dell'ultima esecuzione del processo snapshot per generare lo snapshot dei dati filtrati per la partizione.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifica il processo per la creazione dello snapshot dei dati filtri per una partizione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergepartition** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** e del ruolo predefinito del database **db_owner** possono eseguire **sp_helpmergepartition**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addmergepartition &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_dropmergepartition &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md)  
  
  
