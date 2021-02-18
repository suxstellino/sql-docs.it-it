---
title: Elemento TuningTimeInMin (DTA)
description: Nell'utilità dta l'elemento TuningTimeInMin specifica la lunghezza massima di una sessione di ottimizzazione espressa in minuti.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- TuningTimeInMin element
ms.assetid: 4973d9ac-20fd-4ac3-bc9f-5d60e39fdb7d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 18bd9b4578a6221a6754f7ef906039633e3ed082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354429"
---
# <a name="tuningtimeinmin-element-dta"></a>Elemento TuningTimeInMin (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Specifica la lunghezza massima di una sessione di ottimizzazione espressa in minuti.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <TuningTimeInMin>...</TuningTimeInMin>  
```  
  
## <a name="element-characteristics"></a>Caratteristiche elemento  
  
|Caratteristica|Descrizione|  
|--------------------|-----------------|  
|**Tipo di dati e lunghezza**|**unsignedInt**, lunghezza illimitata.|  
|**Valore predefinito**|480 minuti (8 ore).|  
|**Occorrenza**|Obbligatorio a meno che non sia stato specificato un valore per l'elemento **NumberOfEvents** .|  
  
## <a name="element-relationships"></a>Relazioni elemento  
  
|Relazione|Elementi|  
|------------------|--------------|  
|**Elemento padre**|[Elemento TuningOptions &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Elementi figlio**|nessuno|  
  
## <a name="example"></a>Esempio  
  
## <a name="description"></a>Descrizione  
 Nell'esempio di codice seguente viene illustrato come impostare 12 ore come tempo di ottimizzazione massimo:  
  
## <a name="code"></a>Codice  
  
```  
<DTAInput>  
  <Server>...</Server>  
  <Workload>...</Workload>  
  <TuningOptions>  
    <TuningTimeInMin>720</TuningTimeInMin>  
...code removed here...  
</DTAInput>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento ai file di input XML&#40; (Ottimizzazione guidata motore di database)&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
