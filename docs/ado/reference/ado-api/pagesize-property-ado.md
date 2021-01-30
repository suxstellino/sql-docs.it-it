---
description: Proprietà PageSize (ADO)
title: Proprietà PageSize (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageSize
helpviewer_keywords:
- PageSize property [ADO]
ms.assetid: e57930a6-46c4-4a17-a3b6-f79e94d5c9c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b4e3a85d2c496f88939100a79d464bf1076fd61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170626"
---
# <a name="pagesize-property-ado"></a>Proprietà PageSize (ADO)
Indica il numero di record che costituiscono una pagina nel [Recordset](./recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **Long** che indica il numero di record presenti in una pagina. Il valore predefinito è **10**.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **pageSize** per determinare il numero di record che costituiscono una pagina logica di dati. La definizione di una dimensione di pagina consente di usare la proprietà [AbsolutePage](./absolutepage-property-ado.md) per passare al primo record di una pagina specifica. Questa operazione è utile negli scenari server Web quando si desidera consentire all'utente di eseguire il paging dei dati, visualizzando un determinato numero di record alla volta.  
  
 Questa proprietà può essere impostata in qualsiasi momento e il relativo valore verrà usato per calcolare la posizione del primo record di una pagina specifica.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà AbsolutePage, PageCount e PageSize (VB)](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [Esempio di proprietà AbsolutePage, PageCount e PageSize (VC + +)](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [Proprietà AbsolutePage (ADO)](./absolutepage-property-ado.md)   
 [Proprietà PageCount (ADO)](./pagecount-property-ado.md)