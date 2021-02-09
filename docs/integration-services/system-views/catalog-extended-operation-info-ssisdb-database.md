---
description: catalog.extended_operation_info (database SSISDB)
title: catalog.extended_operation_info (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: db299b45-557d-4c62-8e14-355cdb051f63
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d08a1df29dbea83704165122a16d8daaff110e76
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835214"
---
# <a name="catalogextended_operation_info-ssisdb-database"></a>catalog.extended_operation_info (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Vengono visualizzate le informazioni estese per tutte le operazioni nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|info_id|**bigint**|Identificatore (ID) univoco delle informazioni estese.|  
|operation_id|**bigint**|ID univoco dell'operazione che corrisponde alle informazioni estese.|  
|object_name|**nvarchar(260)**|Nome dell'oggetto.|  
|object_type|**smallint**|Tipo di oggetto interessato dall'operazione. L'oggetto può essere una cartella (`10`), un progetto (`20`), un pacchetto (`30`), un ambiente (`40`) o un'istanza di esecuzione (`50`).|  
|reference_id|**bigint**|ID univoco del riferimento utilizzato nell'operazione.|  
|status|**int**|Stato dell'operazione. I valori possibili sono Creata (`1`), In esecuzione (`2`), Operazione annullata (`3`) Operazione non riuscita (`4`), In sospeso (`5`), Terminata in modo inatteso (`6`), Operazione riuscita (`7`), Arresto in corso (`8`) Operazione completata (`9`).|  
|start_time|**datetimeoffset(7)**|Data e ora di inizio dell'operazione.|  
|end_time|**datetimeoffset(7)**|Data e ora di fine della dell'operazione.|  
  
## <a name="remarks"></a>Commenti  
 Una singola operazione può disporre di più righe di informazioni estese.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'operazione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  Quando si dispone delle autorizzazioni per eseguire un'operazione nel server, si dispone anche delle autorizzazioni per visualizzare le informazioni sull'operazione. È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  
