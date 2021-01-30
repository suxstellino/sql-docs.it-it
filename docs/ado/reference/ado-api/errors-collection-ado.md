---
description: Raccolta Errors (ADO)
title: Raccolta Errors (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Errors
- Connection15::get_Errors
- Connection15::GetErrors
helpviewer_keywords:
- Errors collection [ADO]
ms.assetid: 290819e1-7b39-4e1e-a93b-801257138b00
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f75d1a068a3e86ecb9e0d5b1e27afae6ac4a6e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171189"
---
# <a name="errors-collection-ado"></a>Raccolta Errors (ADO)
Contiene tutti gli oggetti [Error](../../../ado/reference/ado-api/error-object.md) creati in risposta a un singolo errore correlato al provider.  
  
## <a name="remarks"></a>Commenti  
 Tutte le operazioni che coinvolgono oggetti ADO possono generare uno o più errori del provider. Quando si verifica un errore, è possibile inserire uno o più oggetti **Error** nella raccolta **Errors** dell'oggetto [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Quando un'altra operazione ADO genera un errore, la raccolta **Errors** viene cancellata e il nuovo set di oggetti **Error** può essere inserito nella raccolta **Errors** .  
  
 Ogni oggetto **Error** rappresenta un errore specifico del provider, non un errore ADO. Gli errori ADO vengono esposti al meccanismo di gestione delle eccezioni in fase di esecuzione. In Microsoft Visual Basic, ad esempio, l'occorrenza di un errore specifico di ADO attiverà un evento [OnError](../../../ado/reference/rds-api/onerror-event-rds.md) e verrà visualizzato nell'oggetto **Err** .  
  
 Le operazioni ADO che non generano un errore non hanno effetto sulla raccolta di **errori** . Usare il metodo [Clear](../../../ado/reference/ado-api/clear-method-ado.md) per cancellare manualmente la raccolta **Errors** .  
  
 Il set di oggetti **Error** nella raccolta **Errors** descrive tutti gli errori che si sono verificati in risposta a una singola istruzione. L'enumerazione degli errori specifici nella raccolta di **errori** consente alle routine di gestione degli errori di determinare in modo più preciso la causa e l'origine di un errore e di eseguire le operazioni appropriate per il ripristino.  
  
 Alcune proprietà e metodi restituiscono avvisi visualizzati come oggetti **Error** nella raccolta **Errors** ma non interrompono l'esecuzione di un programma. Prima di chiamare i metodi [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)o [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) su un oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) , il metodo [Open](../../../ado/reference/ado-api/open-method-ado-connection.md) su un oggetto **Connection** o la proprietà [Filter](../../../ado/reference/ado-api/filter-property.md) per un oggetto **Recordset** , chiamare il metodo **Clear** sulla raccolta **Errors** . In questo modo è possibile leggere la proprietà [count](../../../ado/reference/ado-api/count-property-ado.md) della raccolta **Errors** per verificare la presenza di avvisi restituiti.  
  
> [!NOTE]
>  Per una spiegazione più dettagliata del modo in cui una singola operazione ADO può generare più errori, vedere l'argomento relativo all'oggetto **Error** .  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi della raccolta Errors](../../../ado/reference/ado-api/errors-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Error (oggetto)](../../../ado/reference/ado-api/error-object.md)   
 [Appendice A: Provider](../../../ado/guide/appendixes/appendix-a-providers.md)
