---
description: Proprietà Status (Recordset - ADO)
title: Proprietà Status (recordset ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::GetStatus
- Recordset15::Status
helpviewer_keywords:
- Status property [ADO Recordset]
ms.assetid: 41d70d89-880f-4850-9d17-19d9790cc8eb
author: rothja
ms.author: jroth
ms.openlocfilehash: e09b3d093f58890efd893ceb0ac204ab6ed12a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170192"
---
# <a name="status-property-ado-recordset"></a>Proprietà Status (Recordset - ADO)
Indica lo stato del record corrente rispetto agli aggiornamenti batch o ad altre operazioni bulk.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce una somma di uno o più valori [RecordStatusEnum](./recordstatusenum.md) .  
  
## <a name="remarks"></a>Commenti  
 Usare la proprietà **status** per vedere quali modifiche sono in sospeso per i record modificati durante l'aggiornamento del batch. È inoltre possibile utilizzare la proprietà **status** per visualizzare lo stato dei record che hanno esito negativo durante le operazioni bulk, ad esempio quando si chiamano i metodi [Resync](./resync-method.md), [UpdateBatch](./updatebatch-method.md)o [CancelBatch](./cancelbatch-method-ado.md) su un oggetto [Recordset](./recordset-object-ado.md) oppure si imposta la proprietà [Filter](./filter-property.md) per un oggetto **Recordset** su una matrice di segnalibri. Con questa proprietà è possibile determinare il modo in cui un determinato record ha avuto esito negativo e risolverlo di conseguenza.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Status (recordset) (VB)](./status-property-example-recordset-vb.md)   
 [Esempio della proprietà Status (VC++)](./status-property-example-vc.md)