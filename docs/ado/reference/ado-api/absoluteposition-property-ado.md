---
description: Proprietà AbsolutePosition (ADO)
title: Proprietà AbsolutePosition (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AbsolutePosition
helpviewer_keywords:
- AbsolutePosition property [ADO]
ms.assetid: 79f8ee5e-fc70-46d8-8c29-ebf943c66592
author: rothja
ms.author: jroth
ms.openlocfilehash: b394ab30d4ee8d73e879abe2687e1208ba3188ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161020"
---
# <a name="absoluteposition-property-ado"></a>Proprietà AbsolutePosition (ADO)
Indica la posizione ordinale del record corrente di un oggetto [Recordset](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Per il codice a 32 bit, imposta o restituisce un valore **Long** compreso tra 1 e il numero di record nell'oggetto **Recordset** ([RecordCount](./recordcount-property-ado.md)) oppure restituisce uno dei valori [PositionEnum](./positionenum.md) .  
  
 Per il codice a 64 bit, usare un tipo di dati che fornisce l'archiviazione di un valore a 64 bit. Ad esempio, è possibile usare un valore Long o un altro valore con una lunghezza di 64 bit, ad esempio DBORDINAL. Non usare valori **PositionEnum** perché sono limitati alla lunghezza di 32 bit.  
  
## <a name="remarks"></a>Commenti  
 Per impostare la proprietà **AbsolutePosition** , ADO richiede che il provider OLE DB in uso implementi l'interfaccia [IRowsetLocate: IRowset](/previous-versions/windows/desktop/ms721190(v=vs.85)) .  
  
 L'accesso alla proprietà **AbsolutePosition** di un **Recordset** aperto con un cursore di tipo "solo" o "di tipo" di tipo " **adErrFeatureNotAvailable**" genera l'errore. Con gli altri tipi di cursori, la posizione corretta verrà restituita purché il provider di OLE DB supporti l'interfaccia **IRowsetScroll: IRowsetLocate** . Se il provider non supporta l'interfaccia **IRowsetScroll** , la proprietà viene impostata su **adPosUnknown**. Per determinare se supporta **IRowsetScroll**, vedere la documentazione del provider.  
  
 Utilizzare la proprietà **AbsolutePosition** per spostarsi in un record in base alla relativa posizione ordinale nell'oggetto **Recordset** o per determinare la posizione ordinale del record corrente. Il provider deve supportare la funzionalità appropriata affinché questa proprietà sia disponibile.  
  
 Analogamente alla proprietà [AbsolutePage](./absolutepage-property-ado.md) , **AbsolutePosition** è in base 1 ed è uguale a 1 quando il record corrente è il primo record nel **Recordset**. È possibile ottenere il numero totale di record nell'oggetto **Recordset** dalla proprietà [RecordCount](./recordcount-property-ado.md) .  
  
 Quando si imposta la proprietà **AbsolutePosition** , anche se si tratta di un record nella cache corrente, ADO ricarica la cache con un nuovo gruppo di record a partire dal record specificato. La proprietà [CacheSize](./cachesize-property-ado.md) determina le dimensioni di questo gruppo.  
  
> [!NOTE]
>  Non usare la proprietà **AbsolutePosition** come numero di record surrogato. La posizione di un determinato record viene modificata quando si elimina un record precedente. Non esiste inoltre alcuna garanzia che un determinato record avrà lo stesso **AbsolutePosition** se l'oggetto **Recordset** viene nuovamente sottoposto a query o riaperto. I segnalibri sono ancora la modalità consigliata per mantenere e tornare a una posizione specificata e sono l'unico modo per eseguire il posizionamento in tutti i tipi di oggetti **Recordset** .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà AbsolutePosition e CursorLocation (VB)](./absoluteposition-and-cursorlocation-properties-example-vb.md)   
 [Esempio di proprietà AbsolutePosition e CursorLocation (VC + +)](./absoluteposition-and-cursorlocation-properties-example-vc.md)   
 [Proprietà AbsolutePage (ADO)](./absolutepage-property-ado.md)   
 [Proprietà RecordCount (ADO)](./recordcount-property-ado.md)