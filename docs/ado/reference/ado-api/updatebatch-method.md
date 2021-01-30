---
description: Metodo UpdateBatch
title: Metodo UpdateBatch | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::UpdateBatch
- Recordset15::raw_UpdateBatch
helpviewer_keywords:
- UpdateBatch method [ADO]
ms.assetid: 23f9314c-b027-4a51-aeae-50caa2977740
author: rothja
ms.author: jroth
ms.openlocfilehash: 8bc846e849df5980efca8f5e28982c964276f8bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166313"
---
# <a name="updatebatch-method"></a>Metodo UpdateBatch
Scrive tutti gli aggiornamenti batch in sospeso sul disco.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.UpdateBatch AffectRecords, PreserveStatus  
```  
  
#### <a name="parameters"></a>Parametri  
 *AffectRecords*  
 facoltativo. Valore [AffectEnum](./affectenum.md) che indica il numero di record su cui avrà effetto il metodo **UpdateBatch** .  
  
 *PreserveStatus*  
 facoltativo. Valore **booleano** che specifica se è necessario eseguire il commit delle modifiche locali, come indicato dalla proprietà [status](./status-property-ado-recordset.md) . Se questo valore è impostato su **true**, la proprietà **status** di ogni record rimarrà invariata al termine dell'aggiornamento.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare il metodo **UpdateBatch** quando si modifica un oggetto **Recordset** in modalità di aggiornamento batch per trasmettere tutte le modifiche apportate in un oggetto **Recordset** al database sottostante.  
  
 Se l'oggetto **Recordset** supporta l'aggiornamento in batch, è possibile memorizzare nella cache più modifiche a uno o più record localmente fino a quando non si chiama il metodo **UpdateBatch** . Se si modifica il record corrente o si aggiunge un nuovo record quando si chiama il metodo **UpdateBatch** , ADO chiamerà automaticamente il metodo [Update](./update-method.md) per salvare le modifiche in sospeso apportate al record corrente prima di trasmettere le modifiche in batch al provider. È consigliabile utilizzare l'aggiornamento batch solo con un keyset o un cursore statico.  
  
> [!NOTE]
>  Se si specifica **adAffectGroup** come valore per questo parametro, verrà generato un errore quando nel **Recordset** corrente non sono presenti record visibili, ad esempio un filtro per il quale non è presente alcun record corrispondente.  
  
 Se il tentativo di trasmettere le modifiche non riesce per uno o tutti i record a causa di un conflitto con i dati sottostanti (ad esempio, un record è già stato eliminato da un altro utente), il provider restituisce avvisi alla raccolta degli [errori](./errors-collection-ado.md) e si verifica un errore di run-time. Utilizzare la proprietà [Filter](./filter-property.md) (**adFilterAffectedRecords**) e la proprietà [status](./status-property-ado-recordset.md) per individuare i record con conflitti.  
  
 Per annullare tutti gli aggiornamenti batch in sospeso, usare il metodo [CancelBatch](./cancelbatch-method-ado.md) .  
  
 Se le proprietà dinamiche della [tabella](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) e della [Risincronizzazione degli aggiornamenti](./update-resync-property-dynamic-ado.md) sono impostate e il **Recordset** è il risultato dell'esecuzione di un'operazione di join su più tabelle, l'esecuzione del metodo **UpdateBatch** viene seguita in modo implicito dal metodo di [Risincronizzazione](./resync-method.md) , a seconda delle impostazioni della proprietà [Aggiorna risincronizzazione](./update-resync-property-dynamic-ado.md) .  
  
 L'ordine in cui i singoli aggiornamenti di un batch vengono eseguiti sull'origine dati non corrisponde necessariamente all'ordine in cui sono stati eseguiti sul **Recordset** locale. L'ordine di aggiornamento dipende dal provider. Prendere in considerazione questo problema quando si codificano gli aggiornamenti correlati tra loro, ad esempio vincoli di chiave esterna per un inserimento o un aggiornamento.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi UpdateBatch e CancelBatch (VB)](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [Esempio di metodi UpdateBatch e CancelBatch (VC + +)](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [Metodo CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [Metodo Clear (ADO)](./clear-method-ado.md)   
 [Proprietà LockType (ADO)](./locktype-property-ado.md)   
 [Metodo Update](./update-method.md)