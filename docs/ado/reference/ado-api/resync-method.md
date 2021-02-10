---
description: Metodo Resync
title: Metodo di risincronizzazione | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::raw_Resync
- Fields::Resync
- Recordset20::Resync
- Fields::raw_Resync
helpviewer_keywords:
- Resync method [ADO]
ms.assetid: 73b355d4-a4c0-434b-bfc4-039b1c76b32e
author: rothja
ms.author: jroth
ms.openlocfilehash: b9d31d01510e80e9ad402e775bb182246a3c7956
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051562"
---
# <a name="resync-method"></a>Metodo Resync
Aggiorna i dati nell'oggetto [Recordset](./recordset-object-ado.md) corrente o nella raccolta di [campi](./fields-collection-ado.md) di un oggetto [record](./record-object-ado.md) dal database sottostante.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Recordset.Resync AffectRecords, ResyncValues Record.Fields.Resync ResyncValues  
```  
  
#### <a name="parameters"></a>Parametri  
 *AffectRecords*  
 facoltativo. Valore [AffectEnum](./affectenum.md) che determina il numero di record su cui influirà il metodo di **Risincronizzazione** . Il valore predefinito è **adAffectAll**. Questo valore non è disponibile con il metodo **Resync** della raccolta **Fields** di un oggetto **record** .  
  
 *ResyncValues*  
 facoltativo. Valore [ResyncEnum](./resyncenum.md) che specifica se i valori sottostanti vengono sovrascritti. Il valore predefinito è **adResyncAllValues**.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="recordset"></a>recordset  
 Utilizzare il metodo **Resync** per risincronizzare i record del **Recordset** corrente con il database sottostante. Questa operazione è utile se si utilizza un cursore statico o di tipo "solo di tipo", ma si desidera visualizzare eventuali modifiche nel database sottostante.  
  
 Se si imposta la proprietà [CursorLocation](./cursorlocation-property-ado.md) su **adUseClient**, la **Risincronizzazione** è disponibile solo per gli oggetti **Recordset** non di sola lettura.  
  
 A differenza del metodo [Requery](./requery-method.md) , il metodo **Resync** non esegue di nuovo il comando sottostante dell'oggetto **Recordset** . I nuovi record nel database sottostante non saranno visibili.  
  
 Se il tentativo di risincronizzazione non riesce a causa di un conflitto con i dati sottostanti (ad esempio, un record è stato eliminato da un altro utente), il provider restituisce avvisi alla raccolta [Errors](./errors-collection-ado.md) e si verifica un errore di run-time. Utilizzare la proprietà [Filter](./filter-property.md) (**adFilterConflictingRecords**) e la proprietà [status](./status-property-ado-recordset.md) per individuare i record con conflitti.  
  
 Se vengono impostate le proprietà dinamiche della tabella e della [Risincronizzazione](./resync-command-property-dynamic-ado.md) [univoca](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) e il **Recordset** è il risultato dell'esecuzione di un'operazione di join su più tabelle, il metodo **Resync** eseguirà il comando specificato nella proprietà di **comando resync** solo nella tabella denominata nella proprietà **Unique Table** .  
  
## <a name="fields"></a>Campi  
 Utilizzare il metodo **Resync** per risincronizzare i valori della raccolta **Fields** di un oggetto **record** con l'origine dati sottostante. Questo metodo non ha effetto sulla proprietà [count](./count-property-ado.md) .  
  
 Se *ResyncValues* è impostato su **adResyncAllValues** (valore predefinito), le proprietà [UnderlyingValue](./underlyingvalue-property.md), [value](./value-property-ado.md)e [OriginalValue](./originalvalue-property-ado.md) degli oggetti [Field](./field-object.md) della raccolta sono sincronizzate. Se *ResyncValues* è impostato su **adResyncUnderlyingValues**, viene sincronizzata solo la proprietà **UnderlyingValue** .  
  
 Il valore della proprietà **status** per ogni oggetto **Field** al momento della chiamata influiscono anche sul comportamento della **Risincronizzazione**. Per gli oggetti **campo** con  valori di stato **adFieldPendingUnknown** o **adFieldPendingInsert**, la **Risincronizzazione** non ha alcun effetto. Per i valori di **stato** di **adFieldPendingChange** o **adFieldPendingDelete**, la **Risincronizzazione** sincronizza i valori dei dati per i campi che esistono ancora nell'origine dati.  
  
 La **Risincronizzazione** non modifica i valori di **stato** degli oggetti **campo** a meno che non si verifichi un errore quando viene chiamata la **Risincronizzazione** . Se, ad esempio, il campo non esiste più, il provider restituirà un valore di **stato** appropriato per l'oggetto **campo** , ad esempio **adFieldDoesNotExist**. I valori di **stato** restituiti possono essere combinati in modo logico all'interno del valore della proprietà **status** .  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Raccolta Fields (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Resync (VB)](./resync-method-example-vb.md)   
 [Esempio di metodo Resync (VC + +)](./resync-method-example-vc.md)   
 [Metodo Clear (ADO)](./clear-method-ado.md)   
 [Proprietà UnderlyingValue](./underlyingvalue-property.md)