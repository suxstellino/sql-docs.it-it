---
description: fn_syscollector_get_execution_details (Transact-SQL)
title: fn_syscollector_get_execution_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_syscollector_get_execution_details_TSQL
- fn_syscollector_get_execution_details
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_details function
ms.assetid: d59ddf0c-72c0-4c57-bc83-aef260e4e105
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3cc8f5cd454fb92be15938858a221d2968baff59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350644"
---
# <a name="fn_syscollector_get_execution_details-transact-sql"></a>fn_syscollector_get_execution_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una parte del log di [!INCLUDE[ssIS](../../includes/ssis-md.md)] (sysssislog) corrispondente a package_execution_id per il pacchetto specificato. La tabella contiene una riga per ogni voce di log generata in fase di esecuzione dai pacchetti o dai relativi contenitori o attività.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_syscollector_get_execution_details ( log_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 *log_id*  
 Identificatore univoco locale del log di esecuzione. *log_id* è di **tipo int**.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|id|**int**|Identificatore univoco della voce del log.|  
|evento|**sysname**|Nome dell'evento che ha generato la voce del log.|  
|computer|**nvarchar**|Computer in cui era in esecuzione il pacchetto al momento della generazione della voce del log.|  
|Operatore|**nvarchar**|Nome utente della persona o dell'agente che ha eseguito il pacchetto che ha generato la voce di log.|  
|source|**nvarchar**|Nome del file eseguibile che ha generato la voce di log.|  
|sourceid|**uniqueidentifier**|GUID del file eseguibile che ha generato la voce di log.|  
|executionid|**uniqueidentifier**|GUID dell'istanza di esecuzione del file eseguibile che ha generato la voce del log.|  
|starttime|**datetime**|Ora di inizio dell'esecuzione del pacchetto.|  
|endtime|**datetime**|Ora di completamento dell'esecuzione del pacchetto.|  
|datacode|**int**|Valore intero che identifica l'evento associato alla voce di log. "0" indica che l'evento non ha restituito alcun identificatore.|  
|databytes|**image**|Matrice di byte che identifica un valore restituito.|  
|message|**nvarchar**|Descrizione dell'evento e delle informazioni associate all'evento.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per **dc_operator**.  
  
## <a name="see-also"></a>Vedere anche  
 [Abilitare la registrazione di pacchetti in SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)  
  
  
