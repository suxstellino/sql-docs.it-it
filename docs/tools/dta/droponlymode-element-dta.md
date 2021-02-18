---
title: Elemento DropOnlyMode (DTA)
description: Nell'utilità dta l'elemento DropOnlyMode specifica che Ottimizzazione guidata motore di database considera solo la rimozione degli indici, delle viste indicizzate o delle partizioni esistenti.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DropOnlyMode element
ms.assetid: 80960676-7581-4074-889b-80ee665963dd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 27d38719f0ee9572a86a7b196655489b33824071
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351104"
---
# <a name="droponlymode-element-dta"></a>Elemento DropOnlyMode (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Specifica che Ottimizzazione guidata motore di database deve considerare la rimozione degli indici, delle viste indicizzate o delle partizioni esistenti solo durante la sessione di ottimizzazione. Se è specificata questa opzione di ottimizzazione non viene considerata alcuna nuova struttura di progettazione fisica.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <DropOnlyMode>...</DropOnlyMode>  
```  
  
## <a name="element-characteristics"></a>Caratteristiche elemento  
 **Tipo di dati e lunghezza**  
  
 **Valore predefinito**  
  
 **Occorrenza**: Facoltativa. È possibile usarlo solo una volta per ogni elemento **TuningOptions** . Non è possibile usarlo se nell'elemento **TuningOptions** sono specificati gli elementi seguenti:  
  
-   [Elemento FeatureSet &#40;DTA&#41;](../../tools/dta/featureset-element-dta.md)  
  
-   [Elemento Partitioning &#40;DTA&#41;](../../tools/dta/partitioning-element-dta.md)  
  
-   [Elemento KeepExisting &#40;DTA&#41;](../../tools/dta/keepexisting-element-dta.md) impostato su **ALL**  
  
## <a name="element-relationships"></a>Relazioni elemento  
 **Elemento padre**: [Elemento TuningOptions &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)  
  
 **Elementi figlio**  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra la sezione **TuningOptions** di un file di input XML di Ottimizzazione guidata motore di database in cui è specificata la modalità **DropOnlyMode** . In questo esempio, il tempo di ottimizzazione è limitato a 24 ore (1440 minuti) e tutti gli indici esistenti cluster e non cluster verranno presi in considerazione per l'eliminazione:  
  
```xml  
<TuningOptions  
  <TuningTimeInMin>1440</Name>  
  <KeepExisting>ALIGNED</KeepExisting>  
  <DropOnlyMode />  
</TuningOptions>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento ai file di input XML&#40; (Ottimizzazione guidata motore di database)&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
