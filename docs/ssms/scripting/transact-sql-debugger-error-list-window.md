---
title: Finestra Elenco errori
description: Informazioni su come usare l'Elenco errori per visualizzare gli errori e gli avvisi generati da IntelliSense durante la modifica di script Transact-SQL.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- error list window
- SQL Server Management Studio [SQL Server], error list window
ms.assetid: fae6327d-e268-44ae-a474-4a8f8f843129
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5045ff2aa128b9e1c34f3031ae1fcd487b511481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354517"
---
# <a name="transact-sql-debugger---error-list-window"></a>Debugger Transact-SQL - Finestra Elenco errori

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La finestra **Elenco errori** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] mostra gli errori semantici e di sintassi generati dal codice IntelliSense nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="features-of-the-error-list"></a>Caratteristiche della finestra Elenco errori  

In **Elenco errori** sono disponibili le funzionalità seguenti:  
  
-   Durante la modifica degli script, in **Elenco errori** vengono visualizzati gli errori e gli avvisi prodotti da IntelliSense nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
-   È possibile fare doppio clic su una voce del messaggio di errore per attivare la scheda del file script che ha generato l'errore e spostarsi nella posizione dello stesso.  
  
-   È possibile filtrare le voci e le colonne di informazioni che si desidera visualizzare per ogni voce.  
  
-   Dopo avere corretto un errore, la relativa voce viene rimossa da **Elenco errori**.  
  
-   Quando si chiude la scheda di un file script [!INCLUDE[tsql](../../includes/tsql-md.md)] , gli errori relativi al file vengono rimossi da **Elenco errori**.  
  
## <a name="working-with-the-error-list"></a>Utilizzo dell'elenco degli errori  
 Per visualizzare **Elenco errori**, eseguire una delle operazioni seguenti:  
  
-   Scegliere **Elenco errori** dal menu **Visualizza**.  
  
-   Utilizzare i tasti di scelta rapida CTRL+\\, CTRL+E.  
  
 Dopo avere aperto **Elenco errori**, è possibile personalizzare la visualizzazione eseguendo le azioni seguenti:  
  
-   Per ordinare l'elenco, fare clic sull'intestazione di una colonna. Per ordinare nuovamente l'elenco in base a un'altra colonna, fare clic sull'intestazione di un'altra colonna tenendo premuto il tasto MAIUSC.  
  
-   Per selezionare le colonne da visualizzare e quelle da nascondere, scegliere **Mostra colonne** dal menu di scelta rapida.  
  
-   Per modificare l'ordine di visualizzazione delle colonne, trascinare l'intestazione di una colonna verso sinistra o verso destra.  
  
 **Elenco errori** non offre collegamenti a informazioni aggiuntive su errori specifici.  
  
## <a name="transact-sql-errors-in-management-studio"></a>Errori Transact-SQL in Management Studio  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] mostra gli errori relativi agli script [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle seguenti posizioni:  
  
-   **Elenco errori** contiene tutti gli errori semantici e di sintassi individuati da IntelliSense nell'editor del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . L'elenco degli errori viene aggiornato dinamicamente durante la modifica di script [!INCLUDE[tsql](../../includes/tsql-md.md)] e comprende tutti gli errori individuati dall'editor in ogni script [!INCLUDE[tsql](../../includes/tsql-md.md)] . L'editor non arresta l'analisi di un file dopo avere rilevato errori in uno script. In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]IntelliSense non supporta tutti gli elementi della sintassi del [!INCLUDE[ssDE](../../includes/ssde-md.md)] nell'editor [!INCLUDE[tsql](../../includes/tsql-md.md)] . **Elenco errori** contiene solo gli errori della sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] supportata da IntelliSense.  
  
-   La scheda **Messaggi** nella parte inferiore della finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] visualizza tutti gli errori e messaggi restituiti da [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] durante l'esecuzione di uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Questo elenco non subisce modifiche se non viene eseguito nuovamente lo script. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] arresta l'analisi di un batch dopo l'individuazione di uno o due errori di compilazione; pertanto la scheda **Messaggi** potrebbe non elencare tutti gli errori presenti in uno script.  
  
 Talvolta gli errori sono elencati in entrambe le posizioni. Ad esempio, in un file script potrebbe essere presente un errore di sintassi elencato in **Elenco errori**. Se lo script viene eseguito prima di correggere l'errore, il parser [!INCLUDE[ssDE](../../includes/ssde-md.md)] può rilevare la stessa condizione e restituire un'altra copia del messaggio di errore nella scheda **Messaggi** .  
  
> [!NOTE]  
>  **Elenco errori** visualizza solo gli errori dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , non gli errori degli editor MDX, DMX o XML/A. Questi errori vengono visualizzati nella scheda **Messaggi** dei rispettivi editor.  
  
## <a name="ui-element-list"></a>Elenco di elementi dell'interfaccia utente  
 All'apertura di **Elenco errori** le informazioni sono visualizzate nelle colonne seguenti:  
  
 **Ordine predefinito**  
 Visualizza un valore intero che indica l'ordine in cui le voci sono state generate.  
  
 **Descrizione**  
 Visualizza il testo della voce di errore. Le descrizioni più lunghe vengono suddivise in più righe.  
  
 **File**  
 Visualizza il nome del file script che ha generato l'errore.  
  
 **Linea**  
 Visualizza un numero intero che indica la riga del codice che contiene l'errore.  
  
 **Colonna**  
 Visualizza un numero intero che indica la posizione dell'errore nella riga del codice.  
  
 **Progetto**  
 Visualizza il nome del progetto che comprende il file script.  
