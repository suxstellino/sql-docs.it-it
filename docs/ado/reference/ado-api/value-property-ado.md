---
description: Proprietà Value (ADO)
title: Proprietà Value (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::Value
- _Parameter::Value
helpviewer_keywords:
- Value property [ADO]
ms.assetid: 48919c74-86d4-462e-99b9-8854ceb8d683
author: rothja
ms.author: jroth
ms.openlocfilehash: b809a24429b490961e1d0d009656539db9fe81d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172403"
---
# <a name="value-property-ado"></a>Proprietà Value (ADO)

Indica il valore assegnato a un [campo](./field-object.md), un [parametro](./parameter-object.md)o un oggetto [proprietà](./property-object-ado.md) .
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti

Imposta o restituisce un valore **Variant** che indica il valore dell'oggetto. Il valore predefinito dipende dalla proprietà [Type](./type-property-ado.md) .
  
## <a name="remarks"></a>Commenti

Utilizzare la proprietà **value** per impostare o restituire dati da oggetti **campo** , per impostare o restituire valori di parametro con oggetti **Parameter** oppure per impostare o restituire le impostazioni delle proprietà con oggetti **Property** . Il fatto che la proprietà **value** sia di lettura/scrittura o di sola lettura dipenda da numerosi fattori. Per ulteriori informazioni, vedere gli argomenti relativi agli oggetti.

ADO consente di impostare e restituire dati binari lunghi con la proprietà **value** .
  
> [!NOTE]
> Per gli oggetti **Parameter** , ADO legge la proprietà **value** solo una volta dal provider. Se un comando contiene un **parametro** la cui proprietà **value** è vuota e si crea un [Recordset](./recordset-object-ado.md) dal comando, assicurarsi di chiudere prima il **Recordset** prima di recuperare la proprietà **value** . In caso contrario, per alcuni provider, la proprietà **value** può essere vuota e non conterrà il valore corretto.
> 
> Per i nuovi oggetti **campo** aggiunti alla raccolta [Fields](./fields-collection-ado.md) di un oggetto [record](./record-object-ado.md) , è necessario impostare la proprietà **value** prima di poter specificare altre proprietà del **campo** . Per prima cosa, è necessario assegnare un valore specifico per la proprietà **value** e [aggiornarlo](./update-method.md) nella raccolta **Fields** denominata. È quindi possibile accedere ad altre proprietà, ad esempio il [tipo](./type-property-ado.md) o [gli attributi](./attributes-property-ado.md) .
  
## <a name="applies-to"></a>Si applica a

:::row:::
    :::column:::
        [Oggetto Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Oggetto Parameter](./parameter-object.md)  
    :::column-end:::
    :::column:::
        [Oggetto Property (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche

[Esempio di proprietà Value (VB)](./value-property-example-vb.md) 
 [Esempio di proprietà Value (VC + +)](./value-property-example-vc.md)