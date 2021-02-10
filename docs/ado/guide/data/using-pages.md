---
description: Uso delle pagine
title: Uso di pagine | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- PageSize property [ADO]
- pages [ADO]
- Recordset object [ADO]
- AbsolutePage property [ADO]
- PageCount property [ADO]
ms.assetid: 442b08c5-ccc7-4192-a1cc-22f250867782
author: rothja
ms.author: jroth
ms.openlocfilehash: aae6cbd7d843771fbf88d6d257706ce66e424745
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036761"
---
# <a name="using-pages"></a>Uso delle pagine
Utilizzare la proprietà **PageCount** per determinare il numero di pagine di dati presenti nell'oggetto **Recordset** . Le *pagine* sono gruppi di record la cui dimensione è uguale all'impostazione della proprietà **pageSize** . Anche se l'ultima pagina è incompleta perché sono presenti meno record del valore **pageSize** , viene conteggiata come pagina aggiuntiva nel valore **PageCount** . Se l'oggetto **Recordset** non supporta questa proprietà, **PageCount** sarà-1 per indicare che **PageCount** è determinabile.  
  
 Utilizzare la proprietà **pageSize** per determinare il numero di record che costituiscono una pagina logica di dati. La definizione di una dimensione di pagina consente di usare la proprietà **AbsolutePage** per passare al primo record di una pagina specifica. Questa operazione è utile negli scenari server Web quando si desidera consentire all'utente di eseguire il paging dei dati, visualizzando un determinato numero di record alla volta.  
  
 Questa proprietà può essere impostata in qualsiasi momento e il relativo valore verrà usato per calcolare la posizione del primo record di una pagina specifica.  
  
 Usare la proprietà **AbsolutePage** per identificare il numero di pagina in cui si trova il record corrente. Anche in questo caso, il provider deve supportare la funzionalità appropriata affinché questa proprietà sia disponibile.  
  
 **AbsolutePage** è in base 1 e è uguale a 1 quando il record corrente è il primo record nel **Recordset**. Impostare questa proprietà per passare al primo record di una pagina specifica. Ottenere il numero totale di pagine dalla proprietà **PageCount** .
