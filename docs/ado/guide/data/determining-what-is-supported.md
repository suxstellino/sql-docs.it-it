---
description: Determinazione delle funzionalità supportate
title: Determinazione degli elementi supportati | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], Supports method
- Supports method [ADO]
ms.assetid: 65090cba-6d46-4775-8d61-f6838e7752a6
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a20514318de8222d016f2d345bd6758a075ab5e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037531"
---
# <a name="determining-what-is-supported"></a>Determinazione delle funzionalità supportate
Il metodo **Supports** viene utilizzato per determinare se un oggetto **Recordset** specificato supporta un particolare tipo di funzionalità. Ha la sintassi seguente:  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="remarks"></a>Commenti  
 Il metodo **Supports** restituisce un valore booleano che indica se il provider supporta tutte le funzionalità identificate dall'argomento CursorOptions. È possibile utilizzare il metodo **Supports** per determinare i tipi di funzionalità supportati da un oggetto **Recordset** . Se l'oggetto **Recordset** supporta le funzionalità le cui costanti corrispondenti si trovano in *CursorOptions*, il metodo **Supports** restituisce **true**. In caso contrario, restituisce **false**.  
  
 Utilizzando il metodo **Supports** , è possibile verificare la capacità dell'oggetto **Recordset** di aggiungere nuovi record, utilizzare i segnalibri, utilizzare il metodo **Find** , utilizzare lo scorrimento, utilizzare la proprietà **index** e per eseguire gli aggiornamenti in batch. Per un elenco completo delle costanti e dei relativi significati, vedere [CursorOptionEnum](../../reference/ado-api/cursoroptionenum.md).  
  
 Sebbene il metodo **Supports** possa restituire **true** per una determinata funzionalità, non garantisce che il provider possa rendere la funzionalità disponibile in tutte le circostanze. Il metodo **Supports** restituisce semplicemente un valore che indica se il provider può supportare la funzionalità specificata, supponendo che vengano soddisfatte determinate condizioni. Il metodo **Supports** , ad esempio, può indicare che un oggetto **Recordset** supporta gli aggiornamenti, anche se il cursore è basato su un join a più tabelle, alcune colonne di che non sono aggiornabili.