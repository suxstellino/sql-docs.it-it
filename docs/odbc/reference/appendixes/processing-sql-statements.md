---
description: Elaborazione di istruzioni SQL
title: Elaborazione di istruzioni SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], statement processing
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
ms.assetid: 54dad6a3-e86c-477b-ba7c-4e95e0385ec1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed0e9bddba0b23d17e6e880e2b83af2bff359fe9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199879"
---
# <a name="processing-sql-statements"></a>Elaborazione di istruzioni SQL
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 La libreria di cursori ODBC passa tutte le istruzioni SQL direttamente al driver, ad eccezione di quanto segue:  
  
-   Istruzioni Update e Delete posizionate  
  
-   **Select for Update** -istruzioni  
  
-   Istruzioni SQL in batch  
  
 Per eseguire le istruzioni Update e Delete posizionate e per posizionare il cursore su una riga per chiamare **SQLGetData** per tale riga, la libreria di cursori crea un'istruzione di ricerca che identifica la riga.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Elaborazione di istruzioni di eliminazione e aggiornamento posizionato](../../../odbc/reference/appendixes/processing-positioned-update-and-delete-statements.md)  
  
-   [Elaborazione di istruzioni SELECT FOR UPDATE](../../../odbc/reference/appendixes/processing-select-for-update-statements.md)  
  
-   [Elaborazione di batch di istruzioni SQL](../../../odbc/reference/appendixes/processing-batches-of-sql-statements.md)  
  
-   [Costruzione di istruzioni di ricerca](../../../odbc/reference/appendixes/constructing-searched-statements.md)
