---
description: Metodo GetChildren (ADO)
title: Metodo GetChildren (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_GetChildren
- _Record::GetChildren
helpviewer_keywords:
- GetChildren method [ADO]
ms.assetid: b3f09bac-4f66-49f6-aa5a-6fbb4fb28338
author: rothja
ms.author: jroth
ms.openlocfilehash: 3bf4e7c68bbab5c5452a2eb0dabc790ab3cc5e15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021233"
---
# <a name="getchildren-method-ado"></a>Metodo GetChildren (ADO)
Restituisce un [Recordset](./recordset-object-ado.md) le cui righe rappresentano gli elementi figlio di un [record](./record-object-ado.md)di raccolta.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Set recordset = record.GetChildren  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto **Recordset** per il quale ogni riga rappresenta un elemento figlio dell'oggetto **record** corrente. Ad esempio, gli elementi figlio di un **record** che rappresenta una directory sono i file e le sottodirectory contenuti nella directory padre.  
  
## <a name="remarks"></a>Commenti  
 Il provider determina le colonne presenti nel **Recordset** restituito. Un provider di origine del documento restituisce, ad esempio, sempre un **Recordset** di risorsa.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::