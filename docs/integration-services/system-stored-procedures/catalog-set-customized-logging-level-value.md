---
description: catalog.set_customized_logging_level_value
title: catalog.set_customized_logging_level_value | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: d83fb763-c7c6-4e20-bd10-0f995598b198
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3f5e101e0413381a2b633e758e0d67a0c4c8167b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129703"
---
# <a name="catalogset_customized_logging_level_value"></a>catalog.set_customized_logging_level_value 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Cambia le statistiche o gli eventi registrati da un livello di registrazione personalizzato esistente. Per altre informazioni sui livelli di registrazione personalizzati, vedere [Registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.set_customized_logging_level_value [ @level_name = ] level_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @level_name = ] *level_name*  
 Nome di un livello di registrazione personalizzato esistente.  
  
 *level_name* è di tipo **nvarchar(128)**.  
  
 [ @property_name = ] *property_name*  
 Nome della proprietà da modificare. I valori validi sono **PROFILE** ed **EVENTS**.  
  
 *property_name* è di tipo **nvarchar(128)**.  
  
 [ @property_value = ] *property_value*  
 Nuovo valore della proprietà specificata del livello di registrazione personalizzato specificato.  
  
 Per un elenco dei valori validi per profilo ed eventi, vedere [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md).  
  
 *property_value* è di tipo **bigint**.  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="return-codes"></a>Codici restituiti  
 0 (esito positivo)  
  
 Quando la stored procedure ha esito negativo viene generato un errore.  
  
## <a name="result-set"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte le condizioni che causano la mancata riuscita della stored procedure.  
  
-   L'utente non ha le autorizzazioni necessarie.  
  
  
