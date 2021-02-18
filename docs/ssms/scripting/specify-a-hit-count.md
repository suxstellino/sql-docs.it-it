---
title: Specifica di un numero di passaggi
description: Informazioni su come impostare un numero di passaggi per un punto di interruzione, in modo che il debugger non si interrompa in corrispondenza di tale punto di interruzione fino a quando non raggiunge il numero di passaggi.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.breakpt.hitcount
helpviewer_keywords:
- Transact-SQL debugger, breakpoint hit count
ms.assetid: 24836939-94ed-4e57-aa85-5d6938d859e4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7856ce9244f1a8767a4ce46b889fd39ca6a720f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353916"
---
# <a name="specify-a-hit-count"></a>Specifica di un numero di passaggi

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Un numero di passaggi di un punto di interruzione è un contatore incrementato dal debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] ogni volta che viene raggiunto il punto di interruzione. Se viene raggiunto il numero di passaggi specificato e viene soddisfatta qualsiasi condizione per il punto di interruzione, il debugger esegue l'azione specificata per il punto di interruzione.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="hit-count-considerations"></a>Considerazioni relative al numero di passaggi

 Per impostazione predefinita, l'esecuzione viene interrotta ogni volta che viene rilevato un punto di interruzione. È possibile scegliere tra una delle opzioni seguenti:  
  
-   Interrompi sempre (impostazione predefinita)  
  
-   interrompi quando il numero di passaggi è uguale a [valore specificato]  
  
-   interrompi quando il numero di passaggi è un multiplo di [valore specificato]  
  
-   Interrompi quando il numero di passaggi è maggiore o uguale a [valore specificato]  
  
 I numeri di passaggi dei punti di interruzione vengono incrementati nell'ambito di una sessione di debug. Tutti i numeri di passaggi vengono impostati su zero all'inizio di ogni sessione di debug.  
  
 Se si desidera tenere traccia del numero di volte in cui viene rilevato un punto di interruzione senza che il punto di interruzione interrompa l'esecuzione, specificare un numero di passaggi con un valore molto elevato in modo che il punto di interruzione non effettui mai l'interruzione.  
  
 L'azione predefinita per un punto di interruzione consiste nell'interrompere l'esecuzione una volta raggiunto il numero di passaggi e soddisfatta la condizione per il punto di interruzione. Per informazioni su come specificare altre azioni, vedere [Specificare un'azione del punto di interruzione](./specify-a-breakpoint-action.md).  
  
#### <a name="to-specify-a-hit-count"></a>Per specificare un numero di passaggi  
  
1.  Nella finestra dell'editor fare clic con il pulsante destro del mouse sul glifo del punto di interruzione e scegliere **Passaggi** dal menu di scelta rapida.  
  
     -oppure-  
  
     Nella finestra **Punti di interruzione** fare clic con il pulsante destro del mouse sul glifo del punto di interruzione e scegliere **Passaggi** dal menu di scelta rapida.  
  
2.  Nella finestra di dialogo **Passaggi punto di interruzione** selezionare il comportamento desiderato nella casella **Quando il punto di interruzione viene raggiunto** .  
  
     Se si sceglie un'impostazione diversa da **Interrompi sempre**, a destra dell'elenco verrà visualizzata una casella di testo. Immettere un numero intero nella casella di testo per specificare il numero di passaggi desiderato.  
  
3.  Fare clic su **OK** per implementare le modifiche o su **Annulla** per uscire senza applicare le modifiche.  
  
#### <a name="to-view-or-reset-the-current-hit-count"></a>Per visualizzare o reimpostare il numero di passaggi corrente  
  
1.  Nella finestra dell'editor fare clic con il pulsante destro del mouse sul glifo del punto di interruzione e scegliere **Passaggi** dal menu di scelta rapida.  
  
     -oppure-  
  
     Nella finestra **Punti di interruzione** fare clic con il pulsante destro del mouse sul glifo del punto di interruzione e scegliere **Passaggi** dal menu di scelta rapida.  
  
2.  Nella finestra di dialogo **Passaggi punto di interruzione** verrà visualizzata l'opzione **Passaggi correnti:** al di sopra del pulsante **Reimposta** .  
  
3.  Fare clic su **Reimposta** se si vuole impostare il numero di passaggi corrente su zero.  
  
4.  Fare clic su **OK** o su **Annulla** per chiudere la finestra di dialogo.  
  
## <a name="see-also"></a>Vedere anche  
 [Impostare una condizione del punto di interruzione](./specify-a-breakpoint-condition.md)  
  
