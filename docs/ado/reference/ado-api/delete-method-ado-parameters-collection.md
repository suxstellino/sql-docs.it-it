---
description: Metodo Delete (raccolta Parameters ADO)
title: Metodo Delete (raccolta Parameters ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _DynaCollection::Delete
- _DynaCollection::raw_Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 160c575e-df63-4ade-a2d3-5fd8f72e70cc
author: rothja
ms.author: jroth
ms.openlocfilehash: 13963a0902ba0b1602b270f373a9e1f2d0e0a4cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025591"
---
# <a name="delete-method-ado-parameters-collection"></a>Metodo Delete (raccolta Parameters ADO)
Elimina un oggetto dalla raccolta di [parametri](../../../ado/reference/ado-api/parameters-collection-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Parameters.Delete Index  
```  
  
#### <a name="parameters"></a>Parametri  
 *Index*  
 Valore **stringa** che contiene il nome dell'oggetto che si desidera eliminare o la posizione ordinale (indice) dell'oggetto nella raccolta.  
  
## <a name="remarks"></a>Commenti  
 L'utilizzo del metodo **Delete** in una raccolta consente di rimuovere uno degli oggetti nella raccolta. Questo metodo è disponibile solo nella raccolta **Parameters** di un oggetto [Command](../../../ado/reference/ado-api/command-object-ado.md) . Quando si chiama il metodo **Delete** , è necessario utilizzare la proprietà [Name](../../../ado/reference/ado-api/name-property-ado.md) dell'oggetto [Parameter](../../../ado/reference/ado-api/parameter-object.md) o il relativo indice di raccolta. una variabile oggetto non è un argomento valido.  
  
## <a name="applies-to"></a>Si applica a  
 [Raccolta Parameters (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Delete (raccolta di campi ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [Metodo Delete (recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Metodo DeleteRecord (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
