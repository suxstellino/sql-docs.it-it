---
description: Cache della libreria di cursori
title: Cache della libreria di cursori | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: d6a91cd6-3905-4e3a-98ab-37fce893dbe1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0de0f04795cd2e0ad8f007155cad2b9329c3d082
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165367"
---
# <a name="cursor-library-cache"></a>Cache della libreria di cursori
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 Per ogni riga di dati nel set di risultati, la libreria di cursori memorizza nella cache i dati per ogni colonna associata, la lunghezza dei dati in ogni colonna associata e lo stato della riga. La libreria di cursori utilizza i valori nella cache per restituire i valori tramite **SQLFetch** e **SQLFetchScroll** e per costruire istruzioni ricercate per le operazioni posizionate. Per ulteriori informazioni, vedere [creazione di istruzioni ricercate](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Dati della colonna](../../../odbc/reference/appendixes/column-data.md)  
  
-   [Lunghezza dei dati di colonna](../../../odbc/reference/appendixes/length-of-column-data.md)  
  
-   [Stato riga](../../../odbc/reference/appendixes/row-status.md)  
  
-   [Percorso della cache](../../../odbc/reference/appendixes/location-of-cache.md)
