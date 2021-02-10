---
description: Proprietà dinamica Update Resync (ADO)
title: Property-Dynamic di aggiornamento della risincronizzazione (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Update Resync property [ADO]
ms.assetid: 8a3bb608-66d7-4128-a3ef-84cb0556de0d
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e52c07a4f776c740079ae1460a2890d7aac02ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056296"
---
# <a name="update-resync-property-dynamic-ado"></a>Proprietà dinamica Update Resync (ADO)
Specifica se il metodo [UpdateBatch](./updatebatch-method.md) è seguito da un'operazione implicita di [Risincronizzazione](./resync-method.md) del metodo e, in tal caso, dall'ambito di tale operazione.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce uno o più valori [ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md) .  
  
## <a name="remarks"></a>Commenti  
 È possibile combinare i valori di ADCPROP_UPDATERESYNC_ENUM, ad eccezione di adResyncAll, che rappresenta già la combinazione degli altri valori.  
  
 La costante **adResyncConflicts** archivia i valori di risincronizzazione come valori sottostanti, ma non esegue l'override delle modifiche in sospeso.  
  
 La **Risincronizzazione degli aggiornamenti** è una proprietà dinamica aggiunta alla raccolta delle [proprietà](./properties-collection-ado.md) dell'oggetto [Recordset](./recordset-object-ado.md) quando la proprietà [CursorLocation](./cursorlocation-property-ado.md) è impostata su **adUseClient**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)