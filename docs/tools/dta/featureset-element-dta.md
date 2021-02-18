---
title: Elemento FeatureSet (DTA)
description: Nell'utilità dta l'elemento FeatureSet contiene le strutture di progettazione fisica usate da Ottimizzazione guidata motore di database durante l'analisi.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- FeatureSet element
ms.assetid: f2070c53-4a5c-4c11-ac38-96ee200c84f0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 64dc5efd785497298f838d5655702185517d1794
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336007"
---
# <a name="featureset-element-dta"></a>Elemento FeatureSet (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene le strutture di progettazione fisica, ad esempio indici o viste indicizzate, che dovranno essere utilizzate da Ottimizzazione guidata motore di database durante l'analisi.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <FeatureSet>...</FeatureSet>  
```  
  
## <a name="element-characteristics"></a>Caratteristiche elemento  
  
|Caratteristica|Descrizione|  
|--------------------|-----------------|  
|**Tipo di dati e lunghezza**|**string**, lunghezza illimitata.|  
|**Valori consentiti**|**IDX_IV**<br /> Indici e viste indicizzate.<br /><br /> **IDX**<br /> Solo indici.<br /><br /> **IV**<br /> Solo viste indicizzate.<br /><br /> **NCL_IDX**<br /> Solo indici non cluster.<br /><br /> Con questo elemento utilizzare solo uno di questi valori.|  
|**Valore predefinito**|**IDX**|  
|**Occorrenza**|Obbligatorio una sola volta per ogni elemento **TuningOptions** , a meno che non venga usato l'elemento **DropOnlyMode** . Se si usa **DropOnlyMode** , non è possibile usare **FeatureSet**. Questi elementi si escludono a vicenda.|  
  
## <a name="element-relationships"></a>Relazioni elemento  
  
|Relazione|Elementi|  
|------------------|--------------|  
|**Elemento padre**|[Elemento TuningOptions &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Elementi figlio**|No.|  
  
## <a name="example"></a>Esempio  
 Per un esempio di utilizzo di questo elemento, vedere [Esempio di file di input XML semplice &#40;DTA&#41;](../../tools/dta/simple-xml-input-file-sample-dta.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento ai file di input XML&#40; (Ottimizzazione guidata motore di database)&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
