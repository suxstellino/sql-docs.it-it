---
description: Metodo AppendChunk (ADO)
title: Metodo AppendChunk (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::AppendChunk
- Field20::AppendChunk
helpviewer_keywords:
- AppendChunk method [ADO]
ms.assetid: c648b5a8-d4f1-4d16-836e-3957feb03617
author: rothja
ms.author: jroth
ms.openlocfilehash: 937f70d50abb1422f069d555695744c26639dccf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035601"
---
# <a name="appendchunk-method-ado"></a>Metodo AppendChunk (ADO)
Accoda i dati a un [campo](./field-object.md)di dati di testo o binario di grandi dimensioni o a un oggetto [Parameter](./parameter-object.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.AppendChunk Data  
```  
  
#### <a name="parameters"></a>Parametri  
 *object*  
 Oggetto **campo** o **parametro** .  
  
 *Dati*  
 **Variante** che contiene i dati da accodare all'oggetto.  
  
## <a name="remarks"></a>Commenti  
 Usare il metodo **AppendChunk** su un oggetto **Field** o **Parameter** per inserire dati di tipo binary o character lunghi. Nelle situazioni in cui la memoria di sistema è limitata, è possibile usare il metodo **AppendChunk** per modificare i valori Long in parti piuttosto che interamente.  
  
## <a name="field"></a>Campo  
 Se il bit **adFldLong** nella proprietà [Attributes](./attributes-property-ado.md) di un oggetto **Field** è impostato su **true**, è possibile usare il metodo **AppendChunk** per quel campo.  
  
 La prima chiamata di **AppendChunk** su un oggetto **campo** scrive i dati nel campo, sovrascrivendo eventuali dati esistenti. Le chiamate successive a **AppendChunk** vengono aggiunte ai dati esistenti. Se si aggiungono dati a un campo e si imposta o si legge il valore di un altro campo nel record corrente, ADO presuppone che sia stata completata l'aggiunta dei dati al primo campo. Se si chiama nuovamente il metodo **AppendChunk** sul primo campo, ADO interpreta la chiamata come nuova operazione **AppendChunk** e sovrascrive i dati esistenti. L'accesso ai campi di altri oggetti [Recordset](./recordset-object-ado.md) che non sono cloni del primo oggetto **Recordset** non interferisce con le operazioni **AppendChunk** .  
  
 Se non è presente alcun record corrente quando si chiama **AppendChunk** su un oggetto **campo** , si verifica un errore.  
  
> [!NOTE]
>  Il metodo **AppendChunk** non opera su oggetti **Field** di un oggetto [record (ADO)](./record-object-ado.md) . Non esegue alcuna operazione e produrrà un errore di run-time.  
  
## <a name="parameter"></a>Parametro  
 Se il bit **adParamLong** nella proprietà **Attributes** di un oggetto **Parameter** è impostato su **true**, è possibile usare il metodo **AppendChunk** per il parametro.  
  
 La prima chiamata di **AppendChunk** su un oggetto **Parameter** scrive i dati nel parametro, sovrascrivendo eventuali dati esistenti. Le chiamate **AppendChunk** successive su un oggetto **Parameter** aggiungono ai dati dei parametri esistenti. Una chiamata **AppendChunk** che passa un valore null Elimina tutti i dati dei parametri.  
  
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
 [Esempio di metodi AppendChunk e GetChunk (VB)](./appendchunk-and-getchunk-methods-example-vb.md)   
 [Esempio di metodi AppendChunk e GetChunk (VC + +)](./appendchunk-and-getchunk-methods-example-vc.md)   
 [Proprietà Attributes (ADO)](./attributes-property-ado.md)   
 [Metodo GetChunk (ADO)](./getchunk-method-ado.md)