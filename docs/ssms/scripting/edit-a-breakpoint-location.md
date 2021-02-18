---
title: Modifica della posizione di un punto di interruzione
description: Informazioni su come spostare un punto di interruzione in un file di script Transact-SQL in un'altra posizione nello script o in uno script diverso.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint location
ms.assetid: 5c28e411-0377-4210-a7ce-2a5c13dcdf74
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a5cc11057258c1a85f6b185187055a04e05b7689
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354650"
---
# <a name="edit-a-breakpoint-location"></a>Modifica della posizione di un punto di interruzione

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La posizione del punto di interruzione specifica la riga e il carattere in cui si trova il punto di interruzione in un file di script [!INCLUDE[tsql](../../includes/tsql-md.md)] . È possibile modificare la posizione del punto di interruzione per spostare il punto di interruzione in un'altra posizione nello script o in uno script diverso.

## <a name="editing-a-location"></a>Modifica di una posizione

Quando si modifica la posizione di un punto di interruzione, il punto di interruzione viene spostato nella nuova posizione, insieme a tutte le proprietà esistenti, ad esempio un numero di passaggi o una condizione.  

#### <a name="to-edit-a-breakpoint-location"></a>Per modificare la posizione di un punto di interruzione

1. Nella finestra dell'editor fare clic con il pulsante destro del mouse sul glifo del punto di interruzione, quindi scegliere **Posizione** dal menu di scelta rapida.  
  
     -oppure-  
  
     Nella finestra **Punti di interruzione** fare clic con il pulsante destro del mouse sul glifo del punto di interruzione e quindi scegliere **Posizione** dal menu di scelta rapida.  
  
2. Nella finestra di dialogo **Punto di interruzione del file** modificare **File** per specificare un nuovo file, **Riga** per specificare una nuova riga o **Carattere** per specificare una nuova posizione all'interno della riga. Se il nuovo file specificato è già aperto in una finestra dell'editor di query, il punto di interruzione viene spostato in tale finestra. Se il file non è aperto, viene aperta una nuova finestra dell'editor, il file viene caricato e il punto di interruzione viene spostato nella nuova posizione.  
  
     L'opzione **Il codice sorgente può essere diverso dalla versione originale** non ha effetto nel caso di debug di [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="see-also"></a>Vedere anche

- [Specificare un numero di passaggi](./specify-a-hit-count.md)
- [Specificare un'azione del punto di interruzione](./specify-a-breakpoint-action.md)
- [Impostare una condizione del punto di interruzione](./specify-a-breakpoint-condition.md)
- [Specificare un filtro per un punto di interruzione](./specify-a-breakpoint-filter.md)