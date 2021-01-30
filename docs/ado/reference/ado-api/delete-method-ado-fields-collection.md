---
description: Metodo Delete (raccolta Fields ADO)
title: Metodo Delete (raccolta di campi ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Fields20::Delete
- Fields20::raw_Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 25bedc25-c51c-4cab-96ce-930b959965d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 310a3c208aba82db3fb425b18122bceab593977c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167575"
---
# <a name="delete-method-ado-fields-collection"></a>Metodo Delete (raccolta Fields ADO)
Elimina un oggetto dalla raccolta di [campi](../../../ado/reference/ado-api/fields-collection-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Fields.Delete Field  
```  
  
#### <a name="parameters"></a>Parametri  
 *Campo*  
 **Variant** che designa l'oggetto [campo](../../../ado/reference/ado-api/field-object.md) da eliminare. Questo parametro può essere il nome dell'oggetto **campo** o la posizione ordinale dell'oggetto **campo** .  
  
## <a name="remarks"></a>Commenti  
 La chiamata del metodo **Fields. Delete** su un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) aperto genera un errore di run-time.  
  
## <a name="applies-to"></a>Si applica a  
 [Raccolta Fields (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Delete (raccolta Parameters ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [Metodo Delete (recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Metodo DeleteRecord (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
