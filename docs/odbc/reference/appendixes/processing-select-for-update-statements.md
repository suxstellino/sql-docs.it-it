---
description: Elaborazione di istruzioni SELECT FOR UPDATE
title: Elaborazione della selezione per le istruzioni UPDATE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], statement processing
- SQL statements [ODBC], select for update statements
- select for update [ODBC]
- SQL statements [ODBC], cursor library
- cursor library [ODBC], select for update statements
- ODBC cursor library [ODBC], select for update statements
- cursor library [ODBC], statement processing
ms.assetid: 8d2e79a4-5daf-458e-a536-d8b6e588753e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 116a87e12d1f765e3e33aa5b446743acfd8d429b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189735"
---
# <a name="processing-select-for-update-statements"></a>Elaborazione di istruzioni SELECT FOR UPDATE
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 Per garantire la massima interoperabilità, le applicazioni devono generare set di risultati che verranno aggiornati con un'istruzione UPDATE posizionata eseguendo un'istruzione **Select for Update** . Sebbene la libreria di cursori non lo richieda, è richiesta dalla maggior parte delle origini dati che supportano le istruzioni UPDATE posizionate.  
  
 La libreria di cursori ignora le colonne nella clausola **for Update** di un'istruzione **Select for Update** . Questa clausola viene rimossa prima di passare l'istruzione al driver. Nella libreria di cursori, l'attributo SQL_ATTR_CONCURRENCY Statement, insieme alle restrizioni indicate nella sezione precedente, determina se è possibile aggiornare le colonne di un set di risultati.
