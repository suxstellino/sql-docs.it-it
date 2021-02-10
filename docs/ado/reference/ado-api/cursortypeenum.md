---
description: CursorTypeEnum
title: CursorTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CursorTypeEnum
helpviewer_keywords:
- CursorTypeEnum enumeration [ADO]
ms.assetid: ffc6e245-4471-42ae-84dd-e85bddfce983
author: rothja
ms.author: jroth
ms.openlocfilehash: 9ce49416e2cb298e4829022b7da0840a5e2c516a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025867"
---
# <a name="cursortypeenum"></a>CursorTypeEnum
Specifica il tipo di cursore utilizzato in un oggetto [Recordset](./recordset-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adOpenDynamic**|2|Utilizza un cursore dinamico. Sono visibili aggiunte, modifiche ed eliminazioni da parte di altri utenti e tutti i tipi di spostamento tramite il **Recordset** sono consentiti, ad eccezione dei segnalibri, se il provider non li supporta.|  
|**adOpenForwardOnly**|0|Valore predefinito. Utilizza un cursore di sola trasmissione. Identico a un cursore statico, tranne per il fatto che è possibile scorrere i record in modo semplice. In questo modo è possibile migliorare le prestazioni quando è necessario effettuare un solo passaggio attraverso un **Recordset**.|  
|**adOpenKeyset**|1|Utilizza un cursore keyset. Come un cursore dinamico, ad eccezione del fatto che non è possibile visualizzare i record aggiunti da altri utenti, anche se i record eliminati da altri utenti non sono accessibili dal **Recordset**. Le modifiche dei dati da parte di altri utenti sono ancora visibili.|  
|**adOpenStatic**|3|Usa un cursore statico, ovvero una copia statica di un set di record che è possibile usare per trovare i dati o generare rapporti. Aggiunte, modifiche o eliminazioni da altri utenti non sono visibili.|  
|**adOpenUnspecified**|-1|Non specifica il tipo di cursore.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. CursorType. DYNAMIC|  
|AdoEnums. CursorType. FORWARDONLY|  
|AdoEnums. CursorType. KEYSET|  
|AdoEnums. CursorType. STATIC|  
|AdoEnums. CursorType. Unspecified|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà CursorType (ADO)](./cursortype-property-ado.md)