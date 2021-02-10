---
description: Proprietà ActualSize (ADO)
title: Proprietà ActualSize (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::ActualSize
helpviewer_keywords:
- ActualSize property [ADO]
ms.assetid: 722803d0-cef5-4d4c-b79d-3f2f58052229
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fa2172e43b1c3af015b8483a8740641a0e443d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035861"
---
# <a name="actualsize-property-ado"></a>Proprietà ActualSize (ADO)
Indica la lunghezza effettiva del valore di un campo in byte.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Restituisce un valore **Long** .  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **ActualSize** per restituire la lunghezza effettiva del valore di un oggetto [campo](./field-object.md) . Per tutti i campi, la proprietà **ActualSize** è di sola lettura. Se ADO non è in grado di determinare la lunghezza del valore dell'oggetto **campo** , la proprietà **ActualSize** restituisce **adUnknown**.  
  
 Le proprietà **ActualSize** e [DefinedSize](./definedsize-property.md) sono diverse, come illustrato nell'esempio seguente. Un oggetto **campo** con un tipo dichiarato di **adVarChar** e una lunghezza massima di 50 caratteri restituisce un valore di proprietà **DefinedSize** pari a 50, ma il valore della proprietà **ActualSize** restituito è la lunghezza dei dati archiviati nel campo per il record corrente. I **campi** con un **DefinedSize** maggiore di 255 byte vengono trattati come colonne a lunghezza variabile.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Field](./field-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActualSize e DefinedSize (VB)](./actualsize-and-definedsize-properties-example-vb.md)   
 [Esempio di proprietà ActualSize e DefinedSize (VC + +)](./actualsize-and-definedsize-properties-example-vc.md)   
 [Proprietà DefinedSize](./definedsize-property.md)