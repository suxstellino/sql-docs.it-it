---
description: ObjectStateEnum
title: ObjectStateEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ObjectStateEnum
helpviewer_keywords:
- ObjectStateEnum enumeration [ADO]
ms.assetid: 32746558-097b-4749-989e-519aadf7e3f4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d8c2b95aced31a7c5945a846cb9631da5f202b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041471"
---
# <a name="objectstateenum"></a>ObjectStateEnum
Specifica se un oggetto è aperto o chiuso, connettendosi a un'origine dati, eseguendo un comando o recuperando i dati.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adStateClosed**|0|Indica che l'oggetto è chiuso.|  
|**adStateOpen**|1|Indica che l'oggetto è aperto.|  
|**adStateConnecting**|2|Indica che l'oggetto sta effettuando la connessione.|  
|**adStateExecuting**|4|Indica che l'oggetto sta eseguendo un comando.|  
|**adStateFetching**|8|Indica che le righe dell'oggetto vengono recuperate.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. ObjectState. CLOSED|  
|AdoEnums. ObjectState. OPEN|  
|AdoEnums. ObjectState. connessione|  
|AdoEnums.ObjectState.EXECUTING|  
|AdoEnums. ObjectState. FETCHing|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Proprietà State (ADO)](./state-property-ado.md)  
    :::column-end:::
    :::column:::
        [Proprietà State (ADO MD)](../ado-md-api/state-property-ado-md.md)  
    :::column-end:::
:::row-end:::