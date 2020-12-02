---
description: catalog.execution_property_override_values
title: catalog.execution_property_override_values | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 83cbdd6f-ddde-47bf-abde-36bd24272621
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 230e2057c8257a37eb96c683abaf8348307a0cba
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129462"
---
# <a name="catalogexecution_property_override_values"></a>catalog.execution_property_override_values 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza i valori dell'override di proprietà che sono impostati durante l'esecuzione del pacchetto.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|property_id|**bigint**|ID univoco del valore di override di proprietà.|  
|execution_id|**bigint**|ID univoco per l'istanza di esecuzione.|  
|property_path|**nvarchar(4000)**|Percorso alla proprietà nel pacchetto.|  
|property_value|**nvarchar(max)**|Valore di override della proprietà.|  
|sensitive|**bit**|Quando il valore è 1, la proprietà è importante e viene crittografata quando viene archiviata. Quando il valore è 0, la proprietà non è importante e il valore viene archiviato non crittografato.|  
  
## <a name="remarks"></a>Osservazioni  
 Questa vista visualizza una riga per ogni esecuzione nella quale l'override di valori della proprietà è stato eseguito usando la sezione **Override di proprietà** nella scheda **Avanzate** della finestra di dialogo **Esegui pacchetto**. Il percorso della proprietà deriva dalla proprietà **Percorso del pacchetto** dell'attività del pacchetto.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  Quando si dispone delle autorizzazioni per eseguire un'operazione nel server, si dispone anche delle autorizzazioni per visualizzare le informazioni sull'operazione. È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  
