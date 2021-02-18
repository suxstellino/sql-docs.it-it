---
title: Abilitazione, disabilitazione ed eliminazione di punti di interruzione
description: Informazioni su come usare la finestra Punti di interruzione per visualizzare, eliminare, disabilitare e abilitare i punti di interruzione.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 357b5874-273f-43a9-8e30-83872bdea5dc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fec0b9a118326b0afade7d7bab0a26777d8e523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354640"
---
# <a name="enable-disable-and-delete-breakpoints"></a>Abilitazione, disabilitazione ed eliminazione di punti di interruzione

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Per visualizzare e gestire tutti i punti di interruzione impostati, è possibile utilizzare la finestra **Punti di interruzione** . Utilizzare questa finestra per visualizzare informazioni sui punti di interruzione e per effettuare azioni quali l'eliminazione, la disabilitazione o l'abilitazione di punti di interruzione.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
## <a name="the-breakpoints-window"></a>Finestra Punti di interruzione  
 Nella finestra **Punti di interruzione** vengono elencate informazioni quali la riga di codice sulla quale è impostato il punto di interruzione. Nella finestra **Punti di interruzione** è inoltre possibile eliminare, disabilitare e abilitare punti di interruzione. Per ulteriori informazioni sulla finestra **Punti di interruzione** , vedere [Punti di interruzione Window](./transact-sql-debugger-breakpoints-window.md).  
  
 La disabilitazione di un punto di interruzione previene la sospensione dell'esecuzione, tuttavia il punto di interruzione rimane presente qualora si desideri riattivarlo in un secondo momento. L'eliminazione di un punto di interruzione ne comporta la rimozione permanente. È necessario attivare o disattivare un nuovo punto di interruzione per mettere in pausa esecuzione sull'istruzione.  
  
## <a name="to-open-the-breakpoints-window"></a>Per aprire la finestra Punti di interruzione  
 **To open the Breakpoints window**  
  
 È possibile aprire la finestra **Punti di interruzione** in uno dei modi seguenti:  
  
-   Scegliere **Finestre** dal menu **Debug**, quindi fare clic su **Punti di interruzione**.  
  
-   Nella barra degli strumenti **Debug** fare clic sul pulsante **Punti di interruzione** .  
  
-   Premere CTRL+ALT+B.  
  
## <a name="to-disable-a-single-breakpoint"></a>Per disabilitare un solo punto di interruzione  
 **To disable a single breakpoint**  
  
 È possibile disabilitare un singolo punto di interruzione in uno dei modi seguenti:  
  
-   Nella finestra dell'editor di query, fare clic con il pulsante destro del mouse sul punto di interruzione, quindi fare clic su **Disabilita punto di interruzione**.  
  
-   Nella finestra Punti di interruzione, deselezionare la casella di controllo a sinistra del punto di interruzione.  
  
## <a name="to-disable-all-breakpoints"></a>Per disabilitare tutti i punti di interruzione  
 **To disable all breakpoints**  
  
 È possibile disabilitare tutti i punti di interruzione in uno dei modi seguenti:  
  
-   Scegliere **Disabilita tutti i punti di interruzione** dal menu **Debug**.  
  
-   Nella barra degli strumenti della finestra **Punti di interruzione** , fare clic sul pulsante **Disabilita tutti i punti di interruzione** .  
  
## <a name="to-enable-a-single-breakpoint"></a>Per abilitare un solo punto di interruzione  
 **To enable a single breakpoint**  
  
 È possibile abilitare un singolo punto di interruzione in uno dei modi seguenti:  
  
-   Nella finestra dell'editor di query, fare clic con il pulsante destro del mouse sul punto di interruzione, quindi fare clic su **Abilita punto di interruzione**.  
  
-   Nella finestra Punti di interruzione, selezionare la casella di controllo a sinistra del punto di interruzione.  
  
## <a name="to-enable-all-breakpoints"></a>Per abilitare tutti i punti di interruzione  
 **To enable all breakpoints**  
  
 È possibile abilitare tutti i punti di interruzione in uno dei modi seguenti:  
  
-   Scegliere **Abilita tutti i punti di interruzione** dal menu **Debug**.  
  
-   Nella barra degli strumenti della finestra **Punti di interruzione** , fare clic sul pulsante **Abilita tutti i punti di interruzione** .  
  
## <a name="to-delete-a-single-breakpoint"></a>Per eliminare un solo punto di interruzione  
 **To delete a single breakpoint**  
  
 È possibile eliminare un singolo punto di interruzione in uno dei modi seguenti:  
  
-   Nella finestra dell'editor di query, fare clic con il pulsante destro del mouse sul punto di interruzione, quindi fare clic su **Elimina punto di interruzione**.  
  
-   Nella finestra Punti di interruzione fare clic con il pulsante destro del mouse su un punto di interruzione e scegliere **Elimina** dal menu di scelta rapida.  
  
-   Nella finestra Punti di interruzione, selezionare il punto di interruzione, quindi premere CANC.  
  
## <a name="to-delete-all-breakpoints"></a>Per eliminare tutti i punti di interruzione  
 **To delete all breakpoints**  
  
 È possibile eliminare tutti i punti di interruzione in uno dei modi seguenti:  
  
-   Scegliere **Elimina tutti i punti di interruzione** dal menu **Debug**.  
  
-   Sulla barra degli strumenti della finestra **Punti di interruzione** fare clic sul pulsante **Elimina tutti i punti di interruzione** .  
  
## <a name="see-also"></a>Vedere anche  
 [Attivare/disattivare un punto di interruzione](./toggle-a-breakpoint.md)  
  
