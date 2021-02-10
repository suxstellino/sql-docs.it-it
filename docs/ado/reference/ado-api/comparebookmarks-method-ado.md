---
description: Metodo CompareBookmarks (ADO)
title: Metodo CompareBookmarks (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CompareBookmarks
- Recordset20::CompareBookmarks
- Recordset20::raw_CompareBookmarks
helpviewer_keywords:
- CompareBookmarks method [ADO]
ms.assetid: d0b64286-2cc4-4a22-8f1d-9aefeebbcbc6
author: rothja
ms.author: jroth
ms.openlocfilehash: 41a286cc53e03947f0fa37a2378627de880cebd3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026579"
---
# <a name="comparebookmarks-method-ado"></a>Metodo CompareBookmarks (ADO)
Confronta due segnalibri e restituisce un'indicazione dei valori relativi.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
result = recordset.CompareBookmarks(Bookmark1, Bookmark2)  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore [CompareEnum](./compareenum.md) che indica la posizione della riga relativa di due record rappresentati dai relativi segnalibri.  
  
#### <a name="parameters"></a>Parametri  
 *Segnalibro1*  
 Segnalibro della prima riga.  
  
 *Segnalibro2*  
 Segnalibro della seconda riga.  
  
## <a name="remarks"></a>Commenti  
 I segnalibri devono essere applicati allo stesso oggetto [Recordset](./recordset-object-ado.md) o a un oggetto **Recordset** e al relativo [Clone](./clone-method-ado.md). Non è possibile confrontare in modo affidabile i segnalibri da oggetti **Recordset** diversi, anche se sono stati creati dalla stessa origine o comando. Non è inoltre possibile confrontare i segnalibri per un oggetto **Recordset** il cui provider sottostante non supporta i confronti.  
  
 Un segnalibro identifica in modo univoco una riga in un oggetto **Recordset** . Per ottenere il relativo segnalibro, utilizzare la proprietà [Bookmark](./bookmark-property-ado.md) della riga corrente.  
  
 Poiché il tipo di dati di un segnalibro è specifico per ogni provider, ADO lo espone come **Variant**. Ad esempio, SQL Server segnalibri sono di tipo DBTYPE_R8 (**Double**). ADO esporrà questo tipo come **Variant** con un sottotipo di **Double**.  
  
 Quando si confrontano i segnalibri, ADO non tenta alcun tipo di coercizione. I valori vengono semplicemente passati al provider in cui si verifica il confronto. Se i segnalibri passati al metodo **CompareBookmarks** sono archiviati in variabili di tipi diversi, è possibile che venga generato il seguente errore di mancata corrispondenza del tipo: "gli argomenti sono di tipo errato, non sono compresi nell'intervallo accettabile o sono in conflitto tra loro".  
  
 Un segnalibro non valido o formato in modo errato provocherà un errore.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo CompareBookmarks (VB)](./comparebookmarks-method-example-vb.md)   
 [Esempio di metodo CompareBookmarks (VC + +)](./comparebookmarks-method-example-vc.md)   
 [Proprietà Bookmark (ADO)](./bookmark-property-ado.md)