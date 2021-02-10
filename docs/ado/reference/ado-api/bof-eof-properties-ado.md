---
description: Proprietà BOF ed EOF (ADO)
title: Proprietà BOF, EOF (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::BOF
- Recordset15::EOF
helpviewer_keywords:
- EOF property [ADO]
- BOF property [ADO]
ms.assetid: 36c31ab2-f3b6-4281-89b6-db7e04e38fd2
author: rothja
ms.author: jroth
ms.openlocfilehash: 8dd77c7bd3e7c92ec5eb970d0134db4e6e39258f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035491"
---
# <a name="bof-eof-properties-ado"></a>Proprietà BOF ed EOF (ADO)
-   **BOF** Indica che la posizione del record corrente è prima del primo record in un oggetto [Recordset](./recordset-object-ado.md) .  
  
-   **EOF** Indica che la posizione corrente del record è successiva all'ultimo record in un oggetto **Recordset** .  
  
## <a name="return-value"></a>Valore restituito  
 Le proprietà **BOF** e **EOF** restituiscono valori **booleani** .  
  
## <a name="remarks"></a>Commenti  
 Usare le proprietà **BOF** e **EOF** per determinare se un oggetto **Recordset** contiene record o se è stato superato il limite di un oggetto **Recordset** quando ci si sposta da un record a un altro.  
  
 La proprietà **BOF** restituisce **true** (-1) se la posizione corrente del record precede il primo record e **false** (0) se la posizione corrente del record è il primo record o dopo il primo.  
  
 La proprietà **EOF** restituisce **true** se la posizione corrente del record è successiva all'ultimo record e **false** se la posizione corrente del record è precedente o precedente all'ultimo record.  
  
 Se la proprietà **BOF** o **EOF** è **true**, non è presente alcun record corrente.  
  
 Se si apre un oggetto **Recordset** che non contiene record, le proprietà **BOF** e **EOF** sono impostate su **true** . per ulteriori informazioni su questo stato di un **Recordset**, vedere la proprietà [RecordCount](./recordcount-property-ado.md) . Quando si apre un oggetto **Recordset** che contiene almeno un record, il primo record è il record corrente e le proprietà **BOF** e **EOF** sono **false**.  
  
 Se si elimina l'ultimo record rimanente nell'oggetto **Recordset** , è possibile che le proprietà **BOF** e **EOF** rimangano **false** fino a quando non si tenta di riposizionare il record corrente.  
  
 Questa tabella mostra i metodi di **spostamento** consentiti con combinazioni diverse delle proprietà **BOF** e **EOF** .  
  
||MoveFirst<br /><br /> MoveLast|MovePrevious<br /><br /> Sposta < 0|Sposta 0|MoveNext<br /><br /> Sposta > 0|  
|------|-----------------------------|---------------------------------|------------|-----------------------------|  
|**BOF** = **True**, **EOF** = **false**|Consentito|Errore|Errore|Consentito|  
|**BOF** = **False**, **EOF** = **true**|Consentito|Consentito|Errore|Errore|  
|Entrambi **true**|Errore|Errore|Errore|Errore|  
|Entrambi **false**|Consentito|Consentito|Consentito|Consentito|  
  
 L'abilitazione di un metodo **Move** non garantisce che il metodo consenta di individuare correttamente un record; significa solo che la chiamata al metodo **Move** specificato non genererà un errore.  
  
 La tabella seguente mostra cosa accade alle impostazioni delle proprietà **BOF** e **EOF** quando si chiamano vari metodi di **spostamento** ma non è possibile individuare correttamente un record.  
  
||BOF|EOF|  
|------|---------|---------|  
|**MoveFirst**, **MoveLast**|Imposta su **true**|Imposta su **true**|  
|**Sposta** 0|Nessuna modifica|Nessuna modifica|  
|**MovePrevious**, **Sposta** < 0|Imposta su **true**|Nessuna modifica|  
|**MoveNext**, **spostamento** > 0|Nessuna modifica|Imposta su **true**|  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà BOF, EOF e Bookmark (VB)](./bof-eof-and-bookmark-properties-example-vb.md)   
 [Esempio di proprietà BOF, EOF e segnalibro (VC + +)](./bof-eof-and-bookmark-properties-example-vc.md)