---
description: Proprietà CursorType (ADO)
title: Proprietà CursorType (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CursorType
helpviewer_keywords:
- CursorType property [ADO]
ms.assetid: b62c66ca-58d5-430e-9257-eb38c65e48c2
author: rothja
ms.author: jroth
ms.openlocfilehash: 703c89fb8f5e1479e09162429ac93fa43c66e0be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167622"
---
# <a name="cursortype-property-ado"></a>Proprietà CursorType (ADO)
Indica il tipo di cursore utilizzato in un oggetto [Recordset](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore [CursorTypeEnum](./cursortypeenum.md) . Il valore predefinito è **adOpenForwardOnly**.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **CursorType** per specificare il tipo di cursore da utilizzare per l'apertura dell'oggetto **Recordset** .  
  
 Se la proprietà [CursorLocation](./cursorlocation-property-ado.md) è impostata su **adUseClient**, è supportata solo un'impostazione di **adOpenStatic** . Se viene impostato un valore non supportato, non verrà generato alcun errore. verrà invece usato il **CursorType** supportato più vicino.  
  
 Se un provider non supporta il tipo di cursore richiesto, può restituire un altro tipo di cursore. La proprietà **CursorType** verrà modificata in modo da corrispondere al tipo di cursore effettivo in uso quando l'oggetto [Recordset](./recordset-object-ado.md) è aperto. Per verificare funzionalità specifiche del cursore restituito, utilizzare il metodo [Supports](./supports-method.md) . Dopo aver chiuso il **Recordset**, viene ripristinata l'impostazione originale della proprietà **CursorType** .  
  
 Il grafico seguente mostra la funzionalità del provider (identificata da **supporta** le costanti di metodo) obbligatoria per ogni tipo di cursore.  
  
|Per un recordset di questo CursorType|Il metodo Supports deve restituire true per tutte queste costanti|  
|----------------------------------------|---------------------------------------------------------------------|  
|**adOpenForwardOnly**|Nessuno|  
|**adOpenKeyset**|**adBookmark**, **adHoldRecords**, **adMovePrevious**, **adResync**|  
|**adOpenDynamic**|**adMovePrevious**|  
|**adOpenStatic**|**adBookmark**, **adHoldRecords**, **adMovePrevious**, **adResync**|  
  
> [!NOTE]
>  Sebbene **Supports**(**adUpdateBatch**) possa essere true per i cursori dinamici e di tipo "Only", per gli aggiornamenti in batch è necessario usare un cursore keyset o static. Impostare la proprietà [LockType](./locktype-property-ado.md) su **adLockBatchOptimistic** e la proprietà **CursorLocation** su **adUseClient** per abilitare il servizio Cursor per OLE DB, che è necessario per gli aggiornamenti in batch.  
  
 La proprietà **CursorType** è in lettura/scrittura quando il **Recordset** è chiuso e in sola lettura quando è aperto.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Se utilizzata su un oggetto **Recordset** sul lato client, la proprietà **CursorType** può essere impostata solo su **adOpenStatic**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà CursorType, LockType e EditMode (VB)](./cursortype-locktype-and-editmode-properties-example-vb.md)   
 [Esempio di proprietà CursorType, LockType e EditMode (VC + +)](./cursortype-locktype-and-editmode-properties-example-vc.md)   
 [Metodo Supports](./supports-method.md)