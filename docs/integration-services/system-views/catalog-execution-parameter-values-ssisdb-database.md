---
description: catalog.execution_parameter_values (database SSISDB)
title: catalog.execution_parameter_values (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ec93e67b-04ce-4aae-ab96-3ad20e9793ad
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae198e909c64c84b2aa02f074b903dd4587c6c41
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835161"
---
# <a name="catalogexecution_parameter_values-ssisdb-database"></a>catalog.execution_parameter_values (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Vengono visualizzati i valori effettivi del parametro utilizzati dai pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] durante un'istanza di esecuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|execution_parameter_id|**bigint**|Identificatore (ID) univoco del parametro di esecuzione.|  
|execution_id|**bigint**|ID univoco per l'istanza di esecuzione.|  
|object_type|**smallint**|Quando il valore è `20`, il parametro è un parametro del progetto. Quando il valore è `30`, il parametro è un parametro del pacchetto. Quando il valore è `50`, il parametro è uno tra quelli indicati di seguito:<br /><br /> **LOGGING_LEVEL**<br /><br /> **DUMP_ON_ERROR**<br /><br /> **DUMP_ON_EVENT**<br /><br /> **DUMP_EVENT_CODE**<br /><br /> **CALLER_INFO**<br /><br /> **SYNCHRONIZED**|  
|parameter_data_type|**nvarchar(128)**|Tipo di dati del parametro.|  
|parameter_name|**sysname**|Nome del parametro.|  
|parameter_value|**sql_variant**|Valore del parametro. Quando sensitive è `0`, viene visualizzato il valore non crittografato. Quando sensitive è `1`, viene visualizzato il valore **NULL**.|  
|sensitive|**bit**|Quando il valore è `1`, il valore del parametro è importante. In caso contrario, il valore è `0`.|  
|necessario|**bit**|Quando il valore è `1`, il valore del parametro è necessario per avviare l'esecuzione. In caso contrario, il valore è `0`.|  
|value_set|**bit**|Quando il valore è `1`, il valore del parametro è stato assegnato. In caso contrario, il valore è `0`.|  
|runtime_override|**bit**|Quando il valore è `1`, il valore del parametro è stato modificato rispetto a quello originale prima di avviare l'esecuzione. Quando il valore è `0`, il valore del parametro è il valore originale impostato.|  
  
## <a name="remarks"></a>Osservazioni  
 In questa vista viene visualizzata una riga per ogni parametro di esecuzione nel catalogo. Un valore del parametro di esecuzione è il valore assegnato al parametro del progetto o a quello del pacchetto durante una singola istanza di esecuzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  Quando si dispone delle autorizzazioni per eseguire un'operazione nel server, si dispone anche delle autorizzazioni per visualizzare le informazioni sull'operazione. È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  
