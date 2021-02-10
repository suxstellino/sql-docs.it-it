---
description: Proprietà Name (ADO)
title: Proprietà Name (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Name
- Field20::Name
helpviewer_keywords:
- Name property [ADO]
ms.assetid: cfd0e29c-8310-44ab-85c3-5761184b865d
author: rothja
ms.author: jroth
ms.openlocfilehash: 150951c91a58e966bbcd02e8a11a5a495026a129
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041621"
---
# <a name="name-property-ado"></a>Proprietà Name (ADO)
Indica il nome di un oggetto.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **stringa** che indica il nome di un oggetto.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **Name** per assegnare un nome a o recuperare il nome di un **comando**, di una **proprietà**, di un **campo** o di un oggetto **Parameter** .  
  
 Il valore è in lettura/scrittura su un oggetto **Command** e in sola lettura su un oggetto **Property** .  
  
 Per un oggetto **campo** , il **nome** è in genere di sola lettura. Tuttavia, per i nuovi oggetti **campo** aggiunti alla raccolta [Fields](./fields-collection-ado.md) di un [record](./record-object-ado.md), il **nome** è di lettura/scrittura solo dopo che è stata specificata la proprietà [value](./value-property-ado.md) per il **campo** e il provider di dati ha aggiunto correttamente il nuovo **campo** chiamando il metodo [Update](./update-method.md) della raccolta **Fields** .  
  
 Per gli oggetti **Parameter** non ancora aggiunti alla raccolta [Parameters](./parameters-collection-ado.md) , la proprietà **Name** è di lettura/scrittura. Per gli oggetti **parametro** accodati e per tutti gli altri oggetti, la proprietà **Name** è di sola lettura. Non è necessario che i nomi siano univoci all'interno di una raccolta.  
  
 È possibile recuperare la proprietà **Name** di un oggetto in base a un riferimento ordinale, dopo di che è possibile fare riferimento all'oggetto direttamente in base al nome. Se, ad esempio, `rstMain.Properties(20).Name` restituisce `Updatability` , è possibile fare riferimento a questa proprietà come `rstMain.Properties("Updatability")` .  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Command (ADO)](./command-object-ado.md)  
        [Oggetto Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Oggetto Parameter](./parameter-object.md)  
        [Oggetto Property (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Attributes e Name (VB)](./attributes-and-name-properties-example-vb.md)   
 [Esempio di proprietà Attributes e Name (VC + +)](./attributes-and-name-properties-example-vc.md)