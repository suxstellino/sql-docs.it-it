---
description: Proprietà Attributes (ADO)
title: Proprietà Attributes (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Attributes
- Field20::Attributes
- _Parameter::Attributes
helpviewer_keywords:
- Attributes property [ADO]
ms.assetid: acc15d40-68a6-4ba9-85bd-12d331aecaa6
author: rothja
ms.author: jroth
ms.openlocfilehash: 25899a7e6c22b119e58d21c51a514ebe311576f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027591"
---
# <a name="attributes-property-ado"></a>Proprietà Attributes (ADO)
Indica una o più caratteristiche di un oggetto.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Long** .  
  
 Per un oggetto [connessione](./connection-object-ado.md) , la proprietà **Attributes** è di lettura/scrittura e il relativo valore può essere la somma di uno o più valori [XactAttributeEnum](./xactattributeenum.md) . Il valore predefinito è zero (0).  
  
 Per un oggetto [Parameter](./parameter-object.md) , la proprietà **Attributes** è di lettura/scrittura e il relativo valore può essere la somma di uno o più valori [ParameterAttributesEnum](./parameterattributesenum.md) . Il valore predefinito è **adParamSigned**.  
  
 Per un oggetto [campo](./field-object.md) , la proprietà **Attributes** può essere la somma di uno o più valori [FieldAttributeEnum](./fieldattributeenum.md) . In genere è di sola lettura. Tuttavia, per i nuovi oggetti **campo** aggiunti alla raccolta [Fields](./fields-collection-ado.md) di un [record](./record-object-ado.md), **gli attributi** sono di lettura/scrittura solo dopo che è stata specificata la proprietà [value](./value-property-ado.md) per il **campo** e il nuovo **campo** è stato aggiunto correttamente dal provider di dati chiamando il metodo [Update](./update-method.md) della raccolta **Fields** .  
  
 Per un oggetto [Proprietà](./property-object-ado.md) , la proprietà **Attributes** è di sola lettura e il relativo valore può essere la somma di uno o più valori [PropertyAttributesEnum](./propertyattributesenum.md) .  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **Attributes** per impostare o restituire le caratteristiche di oggetti **connessione** , oggetti **parametro** , oggetti **campo** o oggetti **Property** .  
  
 Quando si impostano più attributi, è possibile sommare le costanti appropriate. Se si imposta il valore della proprietà su una somma, incluse le costanti incompatibili, si verificherà un errore.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Questa proprietà non è disponibile in un oggetto **connessione** sul lato client.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
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
 [Metodo AppendChunk (ADO)](./appendchunk-method-ado.md)   
 [Metodi BeginTrans, CommitTrans e RollbackTrans (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)   
 [Metodo GetChunk (ADO)](./getchunk-method-ado.md)