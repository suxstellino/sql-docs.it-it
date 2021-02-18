---
title: Impostare una condizione del punto di interruzione
description: Informazioni su come impostare una condizione del punto di interruzione per controllare se il debugger esegue un'azione del punto di interruzione quando viene raggiunto il punto di interruzione e viene raggiunto il numero di passaggi.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint conditions
ms.assetid: b43d8a2b-99a3-4fb7-8848-99c042ea7ef7
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.reviewer: ''
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 58b97d4a799a6ad5f79bb8d37d11bd516e08af4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344126"
---
# <a name="specify-a-breakpoint-condition"></a>Impostare una condizione del punto di interruzione

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Una condizione per il punto di interruzione è un'espressione [!INCLUDE[tsql](../../includes/tsql-md.md)] valutata dal debugger quando viene raggiunto il punto di interruzione. Se la condizione viene soddisfatta e viene raggiunto un numero di passaggi specificato, il debugger interrompe o esegue l'azione specificata per il punto di interruzione.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="specifying-conditions"></a>Definizione delle condizioni

L'espressione specificata deve essere un'espressione Transact-SQL valida che restituisce un valore booleano. Per altre informazioni, vedere [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 Se si specifica una condizione per il punto di interruzione con una sintassi non valida, viene immediatamente visualizzato un messaggio di avviso. Se si specifica una condizione con una sintassi valida ma una semantica non valida, viene visualizzato un messaggio di avviso la prima volta che viene raggiunto il punto di interruzione. In entrambi i casi, il debugger interrompe l'esecuzione al raggiungimento del punto di interruzione non valido.  
  
#### <a name="to-specify-a-condition"></a>Per specificare una condizione
  
1. Nella finestra dell'editor fare clic con il pulsante destro del mouse sul glifo del punto di interruzione, quindi scegliere **Condizione** dal menu di scelta rapida.  
  
     -oppure-  
  
     Nella finestra **Punti di interruzione** fare clic con il pulsante destro del mouse sul glifo del punto di interruzione e quindi scegliere **Condizione** dal menu di scelta rapida.  
  
2. Nella finestra di dialogo **Condizione punto di interruzione** immettere un'espressione booleana valida nella casella **Condizione** .  
  
3. Scegliere **È True** se si vuole interrompere l'esecuzione quando l'espressione restituisce **true** oppure scegliere **È stato modificato** se si vuole interrompere l'esecuzione quando viene modificato il valore dell'espressione.  
  
    > [!NOTE]  
    >  Il debugger non valuta l'espressione booleana fino a quando il primo il punto di interruzione non viene raggiunto. Se si sceglie **È stato modificato**, il debugger non considera la prima valutazione una modifica e quindi non interrompe l'esecuzione in corrispondenza della prima valutazione.  
  
## <a name="see-also"></a>Vedere anche

- [Specificare un numero di passaggi](./specify-a-hit-count.md)
- [Specificare un'azione del punto di interruzione](./specify-a-breakpoint-action.md)