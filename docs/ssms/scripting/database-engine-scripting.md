---
title: Script del motore di database
description: Informazioni su come usare l'ambiente di scripting di Microsoft PowerShell per gestire le istanze del motore di database di SQL Server e su come è possibile creare ed eseguire query del motore di database che contengono istruzioni Transact-SQL e XQuery.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], PowerShell
- scripts [SQL Server]
- scripting [SQL Server Database Engine]
- scripting [SQL Server Database Engine], PowerShell
ms.assetid: 9978a884-59a2-4e7f-a82a-335149f3a261
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 54297946ffc20ec7699536e5bd5c0504497d1f84
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642405"
---
# <a name="database-engine-scripting"></a>Script del motore di database
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] supporta l'ambiente di scripting di [!INCLUDE[msCoName](../../includes/msconame-md.md)] PowerShell per gestire le istanze del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e gli oggetti nelle istanze. È anche possibile compilare ed eseguire query nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] che contengono [!INCLUDE[tsql](../../includes/tsql-md.md)] e XQuery in ambienti molto simili agli ambienti di scripting.  
  
## <a name="sql-server-powershell"></a>SQL Server PowerShell  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono inclusi due snap-in PowerShell che implementano:  
  
-   Un provider PowerShell per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che espone le gerarchie dei modelli SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects) come percorsi di PowerShell simili ai percorsi del file system. È possibile utilizzare le classi del modello SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects) per gestire gli oggetti rappresentati in ciascun nodo del percorso.  
  
-   Un set di cmdlet di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che implementa i comandi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Uno dei cmdlet è **Invoke-Sqlcmd**, che viene usato per eseguire gli script delle query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] da eseguire con l'utilità **sqlcmd** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] include le caratteristiche seguenti per l'esecuzione di PowerShell:  
  
-   Modulo di PowerShell **sqlps** , che può essere importato in una sessione di PowerShell. Il modulo carica quindi gli snap-in di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile eseguire in modo interattivo comandi ad hoc di PowerShell. È possibile eseguire file script utilizzando un comando come .\MyFolder\MyScript.ps1.  
  
-   I file script di PowerShell possono essere utilizzati come input dei passaggi del processo di PowerShell per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che eseguono gli script in base a intervalli pianificati o in risposta a eventi di sistema.  
  
-   Utilità **sqlps** , che avvia PowerShell e importa il modulo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È quindi possibile eseguire tutte le azioni supportate dal modulo. È possibile avviare l'utilità **sqlps** in un prompt dei comandi o facendo clic con il pulsante destro del mouse sui nodi dell'albero di Esplora oggetti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio e scegliendo **Avvia PowerShell**.  
  
## <a name="database-engine-queries"></a>Query del motore di database  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Gli script delle query contengono tre tipi di elementi:  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni del linguaggio.  
  
-   Istruzioni del linguaggio XQuery.  
  
-   Comandi e variabili dell'utilità **sqlcmd** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offre tre ambienti per la compilazione e l'esecuzione di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] :  
  
-   Le query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e il relativo debug possono essere eseguiti in modo interattivo nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È possibile codificare ed eseguire il debug di varie istruzioni in una sessione, quindi salvare tutte le istruzioni in un solo file script.  
  
-   L'utilità della riga di comando **sqlcmd** consente di eseguire in modo interattivo le query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , nonché di eseguire i file script delle query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] esistenti.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] In genere, i file script delle query del [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vengono codificati in modo interattivo usando l'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Il file può essere aperto in un secondo momento in uno degli ambienti seguenti:  
  
-   Usare il menu di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **File**/**Apri** per aprire il file in una nuova finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Usare il parametro **-i**_input_file_ per eseguire il file con l'utilità **sqlcmd** .  
  
-   Usare il parametro **-QueryFromFile** per eseguire il file con il cmdlet **Invoke-Sqlcmd** negli script di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell.  
  
-   Utilizzare i passaggi del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di [!INCLUDE[tsql](../../includes/tsql-md.md)] Agent per eseguire gli script a intervalli pianificati o in risposta a eventi di sistema.  
  
 È anche possibile usare la Generazione guidata script di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per generare script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Fare clic con il pulsante destro del mouse su Esplora oggetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , quindi selezionare la voce di menu **Genera script** . Con **Genera script** viene avviata la procedura guidata, che consente di eseguire in modo semplificato i passaggi necessari per creare uno script.  
  
## <a name="database-engine-scripting-tasks"></a>Attività di scripting del Motore di database  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Descrive come utilizzare gli editor di codice e di testo in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] da sviluppare in modo interattivo, eseguire il debug ed eseguire script [!INCLUDE[tsql](../../includes/tsql-md.md)]|[Editor di query e di testo &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)|  
|Descrive come usare l'utilità **sqlcmd** per eseguire script [!INCLUDE[tsql](../../includes/tsql-md.md)] dal prompt dei comandi, inclusa la possibilità di sviluppare script in modo interattivo.|[Procedure correlate a sqlcmd](./sqlcmd-start-the-utility.md)|  
|Descrive come integrare i componenti di SQL Server in un ambiente Windows PowerShell e compilare script di PowerShell per la gestione di istanze e oggetti di SQL Server.|[SQL Server PowerShell](../../powershell/sql-server-powershell.md)|  
|Descrive come usare la **procedura guidata Genera e pubblica script** per creare script [!INCLUDE[tsql](../../includes/tsql-md.md)] per ricreare uno o più degli oggetti da un database.|[Generazione di script &#40;SQL Server Management Studio&#41;](./generate-scripts-sql-server-management-studio.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità sqlcmd](../../tools/sqlcmd-utility.md)   
 [Esercitazione: Scrittura di istruzioni Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
