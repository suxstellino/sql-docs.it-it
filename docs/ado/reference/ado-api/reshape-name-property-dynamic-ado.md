---
description: Proprietà dinamica Reshape Name (ADO)
title: Property-Dynamic di modifica della forma del nome (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Reshape Name property [ADO]
ms.assetid: 690229d1-46cc-42e6-a57d-4438251fe248
author: rothja
ms.author: jroth
ms.openlocfilehash: 1678bcbae00c7d1022bfeffbb72a3e9b1ee63401
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637978"
---
# <a name="reshape-name-property-dynamic-ado"></a>Proprietà dinamica Reshape Name (ADO)
Specifica un nome per l'oggetto [Recordset](./recordset-object-ado.md) .  
  
## <a name="return-values"></a>Valori restituiti  
 Restituisce un valore **stringa** che rappresenta il nome del **Recordset**.  
  
## <a name="remarks"></a>Osservazioni  
 I nomi vengono mantenuti per la durata della connessione o fino alla chiusura del **Recordset** .  
  
 La proprietà **nome nuova forma** è destinata principalmente all'uso con la funzionalità di rimodellazione del [servizio data shaping Microsoft per OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) provider di servizi. I nomi devono essere univoci per partecipare alla ridefinizione.  
  
 Questa proprietà è di sola lettura, ma può essere impostata indirettamente quando viene creato un **Recordset** . Se, ad esempio, una clausola di un comando Shape crea un **Recordset** e lo assegna un nome di alias tramite la parola chiave **As** , l'alias viene assegnato alla proprietà **Reformate Name** . Se non viene dichiarato alcun alias, alla proprietà **nome riforma** viene assegnato un nome univoco generato dal servizio Data Shaping. Se il nome dell'alias corrisponde al nome di un **Recordset** esistente, non è possibile modificare la forma del **Recordset** fino a quando non viene rilasciato uno di essi. Il comportamento predefinito può essere modificato impostando un nome univoco nella proprietà modifica **nome** della connessione ADO su **true**. L'impostazione di questa proprietà consente all'autorizzazione del servizio Data Shaping di modificare il nome assegnato dall'utente, se necessario, per garantire l'univocità. Per ulteriori informazioni sul ridimensionamento, vedere [Microsoft Data Shaping Service per OLE DB (provider di servizi ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md).  
  
 Utilizzare la proprietà **riforma nome** quando si desidera fare riferimento a un **Recordset** in un comando Shape o quando non si conosce il nome perché è stato generato dal servizio Data Shaping. In tal caso, è possibile generare un comando SHAPE concatenando il comando intorno alla stringa restituita dalla proprietà **Reformate Name** .  
  
 Il **nome della riforma** è una proprietà dinamica aggiunta alla raccolta [Properties](./properties-collection-ado.md) dell'oggetto **Recordset** quando la proprietà [CursorLocation](./cursorlocation-property-ado.md) è impostata su **adUseClient**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Servizio Data Shaping Microsoft per OLE DB (provider di servizi ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)   
 [Comandi per la forma in generale](../../guide/data/shape-commands-in-general.md)   
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)