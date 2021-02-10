---
description: Informazioni sugli errori correlati ai recordset
title: Recordset-Related informazioni sull'errore | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset-related errors [ADO]
- errors [ADO], Recordset-related
ms.assetid: 7e103574-59ad-4790-b5f9-fa8d715e711e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2fcd596455cc1074d46ea6ee45d6c1f037553741
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037101"
---
# <a name="recordset-related-error-information"></a>Informazioni sugli errori correlati ai recordset
Durante l'elaborazione batch, la proprietà **status** dell'oggetto **Recordset** fornisce informazioni sui singoli record nel **Recordset**. Prima di eseguire un aggiornamento batch, la proprietà **status** del **Recordset** riflette le informazioni sui record da aggiungere, modificare ed eliminare. Dopo la chiamata di **UpdateBatch** , la proprietà **status** indica l'esito positivo o negativo dell'operazione. Quando si passa da record a record nel **Recordset**, il valore della proprietà **status** viene modificato per descrivere lo stato del record corrente.
