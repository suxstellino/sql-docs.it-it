---
description: Proprietà Status (Field - ADO)
title: Proprietà Status (campo ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field::Status
- Field::get_Status
- Field::GetStatus
helpviewer_keywords:
- Status property [ADO Field]
ms.assetid: 8cd1f7f4-0a3a-4f07-b8ba-6582e70140ad
author: rothja
ms.author: jroth
ms.openlocfilehash: b0ae4ed8f72dca085edd0c82220296d65795d6ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166466"
---
# <a name="status-property-ado-field"></a>Proprietà Status (Field - ADO)
Indica lo stato di un oggetto [campo](./field-object.md) .  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore [FieldStatusEnum](./fieldstatusenum.md) . Il valore predefinito è **adFieldOK**.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="record-field-status"></a>Stato campo record  
 Le modifiche apportate al valore di un oggetto **campo** nella raccolta Fields di un oggetto [record](./record-object-ado.md) vengono memorizzate nella cache fino a quando non viene chiamato il metodo [Update](./update-method.md) dell'oggetto. A questo punto, se la modifica apportata al valore del campo ha causato un errore, OLE DB genera l'errore **DB_E_ERRORSOCCURRED** (2147749409). La proprietà Status di uno qualsiasi degli oggetti **Field** nella raccolta **Fields** che ha causato l'errore conterrà un valore di [FieldStatusEnum](./fieldstatusenum.md) che descrive la causa del problema.  
  
 Per migliorare le prestazioni, le aggiunte e le eliminazioni per le raccolte di [campi](./fields-collection-ado.md) dell'oggetto **record** vengono memorizzate nella cache fino a quando non viene chiamato il metodo di **aggiornamento** , quindi le modifiche vengono apportate in un aggiornamento ottimistico batch. Se il metodo di **aggiornamento** non viene chiamato, il server non viene aggiornato. Se gli aggiornamenti hanno esito negativo, viene restituito un errore del provider OLE DB (DB_E_ERRORSOCCURRED) e la proprietà **status** indica i valori combinati dell'operazione e il codice di stato dell'errore. Ad esempio, **ADFIELDPENDINGINSERT o adFieldPermissionDenied**. È possibile utilizzare la proprietà **status** per ogni **campo** per determinare il motivo per cui il **campo** non è stato aggiunto, modificato o eliminato.  
  
 Molti tipi di problemi riscontrati durante l'aggiunta, la modifica o l'eliminazione di un **campo** vengono segnalati tramite la proprietà **status** . Se, ad esempio, l'utente elimina un **campo**, viene contrassegnato per l'eliminazione dalla raccolta **Fields** . Se l' **aggiornamento** successivo restituisce un errore perché l'utente ha tentato di eliminare un **campo** per il quale non dispone dell'autorizzazione, il **campo** avrà **lo stato** **adFieldPermissionDenied o adFieldPendingDelete**. La chiamata al metodo [CancelUpdate](./cancelupdate-method-ado.md) consente di ripristinare i valori originali e di impostare lo **stato** su **adFieldOK**.  
  
 Analogamente, il metodo **Update** può restituire un errore perché è stato aggiunto un nuovo **campo** e viene fornito un valore non appropriato. In tal caso, il nuovo **campo** sarà presente nella raccolta **Fields** e avrà lo stato **adFieldPendingInsert** e probabilmente **adFieldCantCreate** (a seconda del provider). È possibile specificare un valore appropriato per il nuovo **campo** e chiamare di nuovo **Update** .  
  
## <a name="recordset-field-status"></a>Stato del campo recordset  
 Le modifiche apportate al valore di un oggetto **campo** nella raccolta Fields di un [Recordset](./recordset-object-ado.md) vengono memorizzate nella cache fino a quando non viene chiamato il metodo [Update](./update-method.md) dell'oggetto. A questo punto, se la modifica apportata al valore del campo ha causato un errore, OLE DB genera l'errore **DB_E_ERRORSOCCURRED** (2147749409). La proprietà Status di uno qualsiasi degli oggetti **Field** nella raccolta **Fields** che ha causato l'errore conterrà un valore di [FieldStatusEnum](./fieldstatusenum.md) che descrive la causa del problema.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Field](./field-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Status (Field) (VB)](./status-property-example-field-vb.md)   
 [Esempio della proprietà Status (VC++)](./status-property-example-vc.md)