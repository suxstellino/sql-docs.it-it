---
description: Proprietà Item (Cellset - ADO MD)
title: Proprietà Item (ADO MD Cellt) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Item
- Cellset::Item
helpviewer_keywords:
- Item property [ADO MD]
ms.assetid: 0e93d79b-b12e-4e98-889e-c2dfcca20fd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d2fe7bb3d13b988b7bc25b6d3f17a5b2ccf945a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164533"
---
# <a name="item-property-ado-md-cellset"></a>Proprietà Item (Cellset - ADO MD)
Recupera una cella da un oggetto [Cell](./cellset-object-ado-md.md) usando le coordinate.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Set  
Cell = Cellset.Item ( Positions)  
```  
  
## <a name="parameters"></a>Parametri  
 *Posizioni*  
 **VariantArray** di valori che specificano una cella in modo univoco. Le *posizioni* possono essere una delle seguenti:  
  
-   Matrice di numeri di posizione  
  
-   Matrice di nomi di membri  
  
-   Posizione ordinale  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **Item** per restituire un oggetto [cella](./cell-object-ado-md.md) all'interno di un oggetto [cellt](./cellset-object-ado-md.md) . Se la proprietà **Item** non riesce a trovare la cella corrispondente all'argomento *Positions* , si verificherà un errore.  
  
 La proprietà **Item** è la proprietà predefinita per l'oggetto **cellt** . I formati di sintassi seguenti sono intercambiabili:  
  
```  
  
Cellset.Item ( Positions )Cellset ( Positions )  
```  
  
 L'argomento *Positions* specifica la cella da restituire. È possibile specificare la cella in base alla posizione ordinale o alla posizione lungo ogni asse. Quando si specifica la cella per posizione lungo ogni asse, è possibile specificare il valore numerico della posizione o i nomi dei membri per ogni posizione.  
  
 La posizione ordinale è un numero che identifica in modo univoco una cella all'interno del **celle**. Dal punto di vista concettuale, le celle sono numerate in un insieme di **celle** come se il numero di **celle** fosse una matrice *p*-dimensionale, dove *p* è il numero di assi. Le celle sono indirizzate in ordine di riga. Di seguito è riportata la formula per il calcolo del numero ordinale di una cella:  
  
 Se i nomi dei membri vengono passati come stringhe all' **elemento**, i membri devono essere elencati in ordine crescente degli identificatori dell'asse numerico. All'interno di un asse, i membri devono essere elencati in ordine crescente di annidamento delle dimensioni, ovvero il membro della dimensione più esterna viene innanzitutto seguito da membri di dimensioni interne. Ogni dimensione deve essere rappresentata da una stringa separata e l'elenco di stringhe membro deve essere separato da virgole.  
  
> [!NOTE]
>  Il recupero di celle in base al nome del membro potrebbe non essere supportato dal provider di dati. Per ulteriori informazioni, vedere la documentazione relativa al provider.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Cellset (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Cell (ADO MD)](./cell-object-ado-md.md)   
 [Oggetto Cellset (ADO MD)](./cellset-object-ado-md.md)