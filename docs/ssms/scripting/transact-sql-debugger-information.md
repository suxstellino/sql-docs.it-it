---
title: Informazioni del debugger Transact-SQL
description: Informazioni su come visualizzare l'output del debugger Transact-SQL, che include informazioni quali stack di chiamate, thread, punti di interruzione, codice, variabili e comandi.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, Locals Window
- Transact-SQL debugger, Watch Window
- Transact-SQL debugger, Threads Window
- Transact-SQL debugger, Call Stack Window
- Transact-SQL debugger, QuickWatch
- Transact-SQL debugger, viewing information
ms.assetid: b99819cc-f388-41a1-b304-36e78ce24147
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 412ae1788542d4fa00df6a17df1b6450f7605851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341007"
---
# <a name="transact-sql-debugger---information"></a>Debugger Transact-SQL - Informazioni

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Ogni volta che l'esecuzione viene sospesa dal debugger in corrispondenza di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] specifica, è possibile utilizzare le varie finestre del debugger per visualizzare lo stato corrente dell'esecuzione. 

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="debugger-windows"></a>Finestre del debugger  

In modalità di debug, il debugger apre due finestre nella parte inferiore della finestra principale di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . In queste due finestre vengono fornite tutte le informazioni del debugger. Ciascuna finestra dispone di schede che è possibile selezionare per controllare il set di informazioni che viene visualizzato nella finestra. La finestra di sinistra contiene le schede **Variabili locali**, **Espressione di controllo1**, **Espressione di controllo2**, **Espressione di controllo3** ed **Espressione di controllo4** . La finestra di destra contiene le schede **Stack di chiamate**, **Thread**, **Punti di interruzione**, **Finestra di comando** e **Output** .  
  
> [!NOTE]  
>  Le descrizioni precedenti indicano le posizioni predefinite delle finestre del debugger. È possibile trascinare una scheda per spostarla da una finestra all'altra oppure è possibile disancorare una scheda per creare una nuova finestra che potrà essere collocata ovunque si desideri.  
  
 Per impostazione predefinita, non tutte queste schede o finestre sono attive. È possibile aprire una particolare finestra utilizzando una qualsiasi delle procedure seguenti:  
  
-   Scegliere **Finestre** dal menu **Debug**, quindi selezionare la finestra desiderata.  
  
-   Fare clic su **Punti di interruzione** sulla barra degli strumenti **Debug**, quindi selezionare la finestra desiderata.  
  
## <a name="transact-sql-expressions"></a>Espressioni di Transact-SQL  
 Le espressioni sono clausole [!INCLUDE[tsql](../../includes/tsql-md.md)] che restituiscono un singolo valore scalare, ad esempio variabili o parametri. La finestra sinistra del debugger può visualizzare i valori di dati attualmente assegnati a espressioni in massimo cinque schede o finestre: **Variabili locali, Espressione di controllo1**, **Espressione di controllo2**, **Espressione di controllo3** ed **Espressione di controllo4**.  
  
 La finestra **Variabili locali** visualizza informazioni sulle variabili locali nell'ambito corrente del debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] . Il set di espressioni elencate nella finestra **Variabili locali** cambia man mano che il debugger viene eseguito sulle varie parti del codice.  
  
 Le espressioni incluse in **Controllo immediato** e nelle quattro finestre **Espressione di controllo** non si limitano semplicemente a elencare l'identificatore di una variabile. È possibile specificare un'espressione [!INCLUDE[tsql](../../includes/tsql-md.md)] che restituisce un valore singolo, ad esempio l'aggiunta di un numero a una variabile, oppure un'istruzione SELECT che restituisce un valore singolo. Tra gli esempi sono inclusi:  
  
-   Nome di una variabile, ad esempio @IntegerCounter.  
  
-   Operazione aritmetica in una variabile, ad esempio @IntegerCounter + 1.  
  
-   Operazione di stringa in due variabili di carattere, ad esempio @FirstName + @LastName.  
  
-   Istruzione SELECT che restituisce un valore singolo, ad esempio SELECT CharCol FROM MyTable WHERE PrimaryKey = 1.  
  
 È possibile usare la finestra **Controllo immediato** per visualizzare il valore di un'espressione [!INCLUDE[tsql](../../includes/tsql-md.md)] , quindi salvare quell'espressione in una finestra **Espressione di controllo** . Per selezionare un'espressione in **Controllo immediato**, selezionare o immettere il nome dell'espressione nella casella **Espressione** .  
  
 Le quattro finestre **Espressione di controllo** visualizzano informazioni relative alle variabili e alle espressioni che sono state selezionate. Il set di espressioni elencate nelle finestre **Espressione di controllo** non cambia fintanto che non vengono aggiunte o eliminate espressioni nell'elenco.  
  
 Per aggiungere un'espressione a una finestra **Espressione di controllo** , è possibile selezionare **Aggiungi espressione di controllo** nella finestra di dialogo **Controllo immediato** oppure immette il nome dell'espressione nella colonna **Nome** di una riga vuota in una finestra **Espressione di controllo** .  
  
 È possibile impostare i valori dei dati per le variabili nelle finestre **Variabili locali**, **Espressione di controllo** o **Controllo immediato** facendo clic con il pulsante destro del mouse nella riga e selezionando quindi **Modifica valore**. Le colonne **Valore** nelle finestre **Variabili locali** ed **Espressione di controllo** e nella finestra di dialogo **Controllo immediato** supportano tutte i visualizzatori di testo e di dati XML e HTML. I visualizzatori sono rappresentati da finestre di suggerimenti dati a forma di lente di ingrandimento sul lato destro della colonna **Valori** . È possibile utilizzare i visualizzatori per visualizzare valori di dati in formato testo, XML o HTML in visualizzazioni che corrispondono ai tipi di dati, ad esempio, per visualizzare file XML in una finestra del browser.  
  
 In modalità debug, se si sposta il puntatore del mouse su un identificatore, viene visualizzata una finestra popup di **informazioni rapide** con il nome dell'espressione e il relativo valore corrente. Per altre informazioni, vedere [Informazioni rapide &#40;IntelliSense&#41;](./quick-info-intellisense.md).  
  
## <a name="breakpoints"></a>Punti di interruzione  
 È possibile usare la finestra **Punti di interruzione** per visualizzare e gestire i punti di interruzione attualmente impostati. Per altre informazioni, vedere [Esecuzione istruzione per istruzione del codice Transact-SQL](./step-through-transact-sql-code.md).  
  
## <a name="call-stacks"></a>Stack di chiamate  
 Nella finestra **Stack di chiamate** vengono visualizzate la posizione di esecuzione corrente e informazioni sulla modalità con cui l'esecuzione passa dalla finestra dell'editor originale attraverso qualsiasi modulo [!INCLUDE[tsql](../../includes/tsql-md.md)] (funzione, stored procedure o trigger) per raggiungere la posizione di esecuzione corrente. Ciascuna riga nella finestra **Stack di chiamate** è chiamata stack frame e rappresenta uno qualsiasi degli elementi seguenti:  
  
-   Il percorso di esecuzione corrente.  
  
-   Una chiamata da un modulo a un altro.  
  
-   Una chiamata da una finestra dell'editor a un modulo [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 L'ordine dello stack è inverso rispetto all'ordine di chiamata dei moduli. La posizione di esecuzione corrente si trova in cima allo stack, mentre la chiamata originale si trova in fondo. Una freccia gialla sul margine sinistro dello stack frame identifica il frame nel quale il debugger ha sospeso l'esecuzione.  
  
 Nella colonna **Nome** sono registrate le informazioni seguenti:  
  
-   Il modulo di origine che contiene la riga di codice che ha eseguito la chiamata al livello successivo.  
  
-   La riga di codice che ha chiamato il modulo successivo nello stack.  
  
-   Se la chiamata è diretta a una stored procedure o una funzione che accetta parametri, vengono elencati anche i nomi, i tipi di dati e i valori di tutti i parametri.  
  
 Le espressioni nelle finestre **Variabili locali**, **Espressione di controllo** e **Controllo immediato** vengono valutate per lo stack frame corrente. Per impostazione predefinita, lo stack frame corrente è il frame in cima allo stack, dove il debugger ha sospeso l'esecuzione. Se si specifica un altro stack frame come frame corrente, le espressioni nelle finestre **Variabili locali**, **Espressione di controllo** e **Controllo immediato** vengono rivalutate per il nuovo stack frame. È possibile cambiare lo stack frame corrente facendo doppio clic su un frame oppure facendo clic su un frame e selezionando **Passa al frame**. A questo punto, le espressioni nelle finestre **Variabili locali**, **Espressione di controllo** e **Controllo immediato** vengono rivalutate per il nuovo frame. La presenza di una freccia verde sul margine sinistro dello stack frame identifica lo stack frame corrente, nei casi in cui lo stack frame corrente non è quello in cima allo stack.  
  
 Quando si fa clic con il pulsante destro del mouse su uno stack frame e si seleziona **Vai a codice sorgente**, il codice per quel frame viene visualizzato in una finestra dell'editor di query. Quel frame tuttavia non diventa il frame corrente e il contenuto delle finestre **Variabili locali**, **Espressione di controllo** e **Controllo immediato** non cambia.  
  
## <a name="system-information-and-transact-sql-results"></a>Informazioni di sistema e risultati di Transact-SQL  
 I messaggi di stato e di evento del debugger vengono elencati nella finestra **Output** del debugger. Tra le informazioni visualizzate vi sono ad esempio il collegamento del debugger ad altri processi o il termine dei thread del debugger.  
  
 In modalità di debug, le schede **Risultati** e **Messaggi** sono ancora attive nell'editor di query. La scheda **Risultati** continua a visualizzare i set dei risultati delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che vengono eseguite durante una sessione di debug. La scheda **Messaggi** continua a visualizzare messaggi di sistema, ad esempio "Righe interessate *xx* " e l'output delle istruzioni PRINT e RAISERROR.  
  
## <a name="see-also"></a>Vedere anche  
 [finestra Variabili locali](./transact-sql-debugger-locals-window.md)   
 [finestra Espressioni di controllo](./transact-sql-debugger-watch-window.md)   
 [Finestra di dialogo Controllo immediato](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Finestra Punti di interruzione](./transact-sql-debugger-breakpoints-window.md)   
 [Finestra Stack di chiamate](./transact-sql-debugger-call-stack-window.md)   
 [Finestra Thread](./transact-sql-debugger-threads-window.md)   
 [Finestra Output](./transact-sql-debugger-output-window.md)   
 [Debugger Transact-SQL](./transact-sql-debugger.md)  
  
