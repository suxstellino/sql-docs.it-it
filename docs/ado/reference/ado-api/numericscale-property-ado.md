---
description: Proprietà NumericScale (ADO)
title: Proprietà NumericScale (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::NumericScale
- Field20::NumericScale
helpviewer_keywords:
- NumericScale property [ADO]
ms.assetid: 29a02992-64be-4fcd-be13-445cba205893
author: rothja
ms.author: jroth
ms.openlocfilehash: 265b5a1c0e435bb9eef9fedd5b28f5db2e4a7258
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041491"
---
# <a name="numericscale-property-ado"></a>Proprietà NumericScale (ADO)
Indica la scala dei valori numerici in un [parametro](./parameter-object.md) o in un oggetto [campo](./field-object.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **byte** che indica il numero di posizioni decimali in cui verranno risolti i valori numerici.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **NumericScale** per determinare il numero di cifre a destra del separatore decimale che verranno utilizzate per rappresentare i valori per un **parametro** numerico o un oggetto **campo** .  
  
 Per gli oggetti **Parameter** , la proprietà **NumericScale** è di lettura/scrittura.  
  
 Per un oggetto **Field**, **NumericScale** è in genere di sola lettura. Tuttavia, per i nuovi oggetti **campo** aggiunti alla raccolta [Fields](./fields-collection-ado.md) di un [record](./record-object-ado.md), **NumericScale** è di lettura/scrittura solo dopo che è stata specificata la proprietà [value](./value-property-ado.md) per il **campo** e il provider di dati ha aggiunto correttamente il nuovo **campo** chiamando il metodo [Update](./update-method.md) della raccolta [Fields](./fields-collection-ado.md) .  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Oggetto Parameter](./parameter-object.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà NumericScale e Precision (VB)](./numericscale-and-precision-properties-example-vb.md)   
 [Esempio di proprietà NumericScale e Precision (VC + +)](./numericscale-and-precision-properties-example-vc.md)   
 [Proprietà Precision (ADO)](./precision-property-ado.md)