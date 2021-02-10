---
description: Proprietà CursorLocation (ADO)
title: Proprietà CursorLocation (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::CursorLocation
- Recordset15::CursorLocation
helpviewer_keywords:
- CursorLocation property [ADO]
ms.assetid: 39c8d86e-7ee9-4182-be5e-aad5ce952f84
author: rothja
ms.author: jroth
ms.openlocfilehash: c43a6565fd02196a1aed2385c2857b7c32dd40a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025955"
---
# <a name="cursorlocation-property-ado"></a>Proprietà CursorLocation (ADO)
Indica la posizione del servizio del cursore.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Long** che può essere impostato su uno dei valori [CursorLocationEnum](./cursorlocationenum.md) .  
  
## <a name="remarks"></a>Commenti  
 Questa proprietà consente di scegliere tra varie librerie di cursori accessibili per il provider. In genere, è possibile scegliere di utilizzare una libreria di cursori sul lato client o un'altra che si trova nel server.  
  
 Questa impostazione di proprietà influiscono sulle connessioni stabilite solo dopo l'impostazione della proprietà. La modifica della proprietà **CursorLocation** non ha effetto sulle connessioni esistenti.  
  
 I cursori restituiti dal metodo [Execute](./execute-method-ado-connection.md) ereditano questa impostazione. Gli oggetti **Recordset** erediteranno automaticamente questa impostazione dalle connessioni associate.  
  
 Questa proprietà è di lettura/scrittura in una [connessione](./connection-object-ado.md) o in un [Recordset](./recordset-object-ado.md)chiuso e in sola lettura in un **Recordset** aperto.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Se utilizzata in un **Recordset** lato client o in un oggetto **connessione** , la proprietà **CursorLocation** può essere impostata solo su **adUseClient**.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Connection (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Appendice A: Provider](../../guide/appendixes/appendix-a-providers.md)