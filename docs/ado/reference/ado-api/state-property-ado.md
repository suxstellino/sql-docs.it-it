---
description: Proprietà State (ADO)
title: Proprietà state (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command25::State
helpviewer_keywords:
- State property [ADO]
ms.assetid: 0b993bac-2653-40b1-bcbb-5b57b6aae2bf
author: rothja
ms.author: jroth
ms.openlocfilehash: 7357d21aa58bca15e1fbbeb3c9cec542ef40ce8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051322"
---
# <a name="state-property-ado"></a>Proprietà State (ADO)
Indica per tutti gli oggetti applicabili se lo stato dell'oggetto è aperto o chiuso. Se l'oggetto esegue un metodo asincrono, indica se lo stato corrente dell'oggetto è la connessione, l'esecuzione o il recupero.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **Long** che può essere un valore [ObjectStateEnum](./objectstateenum.md) . Il valore predefinito è **adStateClosed**.  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare la proprietà **state** per determinare lo stato corrente di un determinato oggetto in qualsiasi momento.  
  
 La proprietà **state** dell'oggetto può avere una combinazione di valori. Se, ad esempio, un'istruzione è in esecuzione, questa proprietà avrà un valore combinato di **adStateOpen** e **adStateExecuting**.  
  
 La proprietà **state** è di sola lettura.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Command (ADO)](./command-object-ado.md)  
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Record (ADO)](./record-object-ado.md)  
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Stream (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ConnectionString, ConnectionTimeout e state (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Esempio di proprietà ConnectionString, ConnectionTimeout e state (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)