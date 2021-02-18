---
title: Apertura di un editor (SQL Server Management Studio)
description: Informazioni su come aprire gli editor di query del motore di database, MDX, DMX e XML/A in SQL Server Management Studio.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 5d654a60-d205-49d2-a831-b3d986d60024
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce81ec69f717f10c705b0d4e7fc1f5b1c6d6ea70
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354589"
---
# <a name="open-an-editor-sql-server-management-studio"></a>Apertura di un editor (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In questo argomento viene descritto come aprire gli editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , MDX, DMX o XML/A in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Ogni finestra dell'editor quando viene aperta viene visualizzata come una scheda nel riquadro centrale di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
## <a name="before-you-begin"></a>Prima di iniziare  
 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] supporta quattro editor: editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] per la modifica degli script [!INCLUDE[tsql](../../includes/tsql-md.md)] , editor DMX e MDX per la modifica degli script mediante tali linguaggi ed editor XML/A per la modifica degli script XML/A o dei file XML. Tali editor possono essere utilizzati anche per modificare i file di testo.  
  
### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Se i file vengono condivisi con utenti in altri siti che utilizzano tabelle codici diverse, è necessario salvare il file con la tabella codici Unicode appropriata per evitare errori di lettura del file. Per il salvataggio dei file per gli ambienti UNIX o Macintosh è inoltre necessario assicurarsi di utilizzare il formato di documento appropriato. Nel menu **File** fare clic su **Salva con nome**, **Salva con codifica** dalla freccia a discesa accanto al pulsante **Salva** e quindi scegliere **Unix** o **Macintosh** in **Terminazioni riga**.  
  
### <a name="permissions"></a>Autorizzazioni  
 Le operazioni eseguite in un editor del codice sono soggette alle autorizzazioni concesse all'account di autenticazione utilizzato per l'accesso. Se ad esempio si apre una finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizzando l'autenticazione di Windows, non sarà possibile eseguire le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che fanno riferimento a oggetti per i quali l'account di Windows non dispone delle autorizzazioni di accesso.  
  
## <a name="how-to-open-editors"></a>Procedura: Aprire gli editor  
 In questa sezione viene illustrato come aprire i vari editor in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="using-the-filenew-menu"></a>Utilizzo del menu File/Nuovo  
 Scegliere **Nuovo** dal menu **File** e quindi selezionare una delle opzioni dell'editor di query:  
  
-   **Query con connessione corrente**: consente di aprire una nuova finestra dell'editor del tipo associato alla connessione corrente in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La finestra dell'editor utilizza le stesse informazioni di autenticazione della connessione corrente. Se ad esempio si seleziona un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in Esplora oggetti e quindi si usa **Query con connessione corrente**, in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] viene aperto un editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] connesso alla stessa istanza usando le stesse informazioni di autenticazione.  
  
-   **Query del motore di database**: consente di aprire un nuovo editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   **Query MDX di Analysis Services** : consente di aprire un nuovo editor di query MDX di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Query DMX di Analysis Services** : consente di aprire un nuovo editor di query DMX di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Query XML/A di Analysis Services** : consente di aprire un nuovo editor di query XML/A di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="using-the-fileopen-menu"></a>Utilizzo del menu File/Apri  
 Scegliere **Apri** dal menu **File** quindi selezionare un file e aprirlo. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] apre il tipo di editor appropriato per l'estensione del file, copia il contenuto del file nella finestra dell'editor e apre anche una finestra di dialogo di connessione, se necessario. Se ad esempio si apre un file con estensione sql, in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] viene aperta la finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , viene copiato il contenuto del file sql nella finestra dell'editor e viene inoltre aperta una finestra di dialogo di connessione. Se si apre un file con un'estensione non associata a un editor specifico, in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] viene aperta una finestra dell'editor di testo e viene copiato il contenuto del file nella finestra dell'editor.  
  
 Per altre informazioni, vedere [Associazione di estensioni di file a un editor di codice](./associate-file-extensions-to-a-code-editor.md).  
  
### <a name="using-the-toolbar"></a>Utilizzo della barra degli strumenti  
 Nella barra degli strumenti **Standard** fare clic su uno dei pulsanti seguenti:  
  
-   **Nuova query**: consente di aprire una nuova finestra dell'editor del tipo associato alla connessione corrente in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La finestra dell'editor utilizza le stesse informazioni di autenticazione della connessione corrente. Se ad esempio si seleziona un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in Esplora oggetti e quindi si fa clic sul pulsante **Nuova query** , [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] apre un editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] connesso alla stessa istanza usando le stesse informazioni di autenticazione.  
  
-   **Query del motore di database**: consente di aprire un nuovo editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   **Query MDX di Analysis Services** : consente di aprire un nuovo editor di query MDX di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Query DMX di Analysis Services** : consente di aprire un nuovo editor di query DMX di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Query XML/A di Analysis Services** : consente di aprire un nuovo editor di query XML/A di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e una finestra di dialogo per ottenere le informazioni necessarie per connettersi a un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="using-object-explorer"></a>Utilizzo di Esplora oggetti  
 Da **Esplora oggetti**:  
  
-   Fare clic con il pulsante destro del mouse sul nodo server connesso a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)], quindi scegliere **Nuova query**. Verrà aperta una finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] connessa alla stessa istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e il contesto di database della finestra verrà impostato sul database predefinito per l'accesso.  
  
-   Fare clic con il pulsante destro del mouse su un nodo di database, quindi scegliere **Nuova query**. Verrà aperta una finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] connessa alla stessa istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e il contesto di database della finestra verrà impostato sullo stesso database.  
  
### <a name="using-solution-explorer"></a>Utilizzo di Esplora soluzioni  
 Da **Esplora soluzioni** espandere una cartella, fare clic con il pulsante destro del mouse su un elemento della cartella, quindi scegliere **Apri** o fare doppio clic sull'elemento o sul file.  
  
### <a name="using-template-browser-to-open-the-database-engine-query-editor"></a>Utilizzo del Visualizzatore modelli per aprire l'editor di query del Motore di database  
  
-   Scegliere **Esplora modelli** dal menu **Visualizza**.  
  
-   La finestra **Visualizzatore modelli** verrà visualizzata nel riquadro destro.  
  
-   Fare doppio clic su un modello per aprire una finestra Query del motore di database con il testo del modello. Per aprire un modello CREATE DATABASE, ad esempio, aprire le cartelle **Modelli di SQL Server** e **Database** , quindi fare doppio clic su **Crea database**.