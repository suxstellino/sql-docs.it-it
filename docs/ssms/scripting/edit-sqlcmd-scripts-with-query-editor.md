---
title: Modifica di script SQLCMD con l'editor di query
description: Gli script SQLCMD vengono usati quando è necessario elaborare comandi di sistema di Windows e istruzioni Transact-SQL nello stesso script. Informazioni su come scrivere e modificare script SQLCMD con l'editor di query del motore di database.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], SQLCMD scripts
- SQLCMD scripts
- modifying scripts
- Query Editor [Database Engine], SQLCMD scripts
- scripts [SQL Server], SQL Server Management Studio
ms.assetid: f77b866d-c330-47c9-9e74-0b8d8dff4b31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60c88bce82f6eb13d1441ded4cb815cecd0842ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354677"
---
# <a name="edit-sqlcmd-scripts-with-query-editor"></a>Modifica di script SQLCMD con l'editor di query

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] consente di scrivere e modificare query come script SQLCMD. Gli script SQLCMD vengono utilizzati quando è necessario elaborare comandi di sistema di Windows e istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] nello stesso script.  
  
## <a name="sqlcmd-mode"></a>Modalità SQLCMD  
 Per utilizzare l'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] per scrivere o modificare script SQLCMD, è necessario abilitare la modalità di scripting SQLCMD, che per impostazione predefinita non è abilitata nell'editor di query. Per attivare la modalità di scripting, fare clic sull'icona **Modalità SQLCMD** sulla barra degli strumenti oppure scegliere **Modalità SQLCMD** dal menu **Query** .  
  
> [!NOTE]  
>  L'abilitazione della modalità SQLCMD disattiva IntelliSense e il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Per gli script SQLCMD è possibile utilizzare nell'editor di query le stesse caratteristiche disponibili per tutti gli script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Di seguito vengono descritte alcune di queste caratteristiche:  
  
-   Codifica a colori  
  
-   Esecuzione di script  
  
-   Controllo del codice sorgente  
  
-   Analisi di script  
  
-   Showplan  
  
## <a name="enable-sqlcmd-scripting-in-query-editor"></a>Attivazione di scripting SQLCMD nell'editor di query  
 Per attivare script SQLCMD per una finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] attiva, utilizzare la procedura descritta di seguito.  
  
#### <a name="to-switch-a-database-engine-query-editor-window-to-sqlcmd-mode"></a>Per attivare la modalità SQLCMD per una finestra dell'editor di query del Motore di database  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse sul server, quindi scegliere **Nuova query** per aprire una nuova finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
2.  Scegliere **Modalità SQLCMD** dal menu **Query**.  
  
     Nel contesto dell'editor di query vengono eseguite le istruzioni **sqlcmd** .  
  
3.  Sulla barra degli strumenti **Editor SQL** , nell'elenco **Database disponibili** , selezionare [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
4.  Nella finestra dell'editor di query digitare le due istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e l'istruzione `!!DIR` **sqlcmd** seguenti:  
  
    ```  
    SELECT DISTINCT Type FROM Sales.SpecialOffer;  
    GO  
    !!DIR  
    GO  
    SELECT ProductCategoryID, Name FROM Production.ProductCategory;  
    GO  
    ```  
  
5.  Premere F5 per eseguire l'intera sezione di istruzioni miste [!INCLUDE[tsql](../../includes/tsql-md.md)] e MS-DOS.  
  
     Si notino i due riquadri dei risultati SQL relativi alla prima e alla terza istruzione.  
  
6.  Nel riquadro **Risultati** fare clic sulla scheda **Messaggi** per visualizzare i messaggi relativi alle tre istruzioni:  
  
    -   (Righe interessate: 6)  
  
    -   \<The directory information>  
  
    -   (Righe interessate: 4)  
  
> [!IMPORTANT]  
>  Quando viene eseguita dalla riga di comando, l'utilità **sqlcmd** consente un'interazione completa con il sistema operativo. Quando si usa l'editor di query in **Modalità SQLCMD**, è necessario assicurarsi che non vengano eseguite istruzioni interattive. L'editor di query non è in grado di rispondere alle richieste del sistema operativo.  
  
 Per altre informazioni sull'esecuzione di SQLCMD, vedere [Utilità sqlcmd](../../tools/sqlcmd-utility.md)oppure eseguire l'esercitazione relativa a SQLCMD.  
  
## <a name="enable-sqlcmd-scripting-by-default"></a>Attivazione di scripting SQLCMD per impostazione predefinita  
 Per attivare la modalità di scripting SQLCMD per impostazione predefinita, scegliere **Opzioni** dal menu **Strumenti**, espandere **Esecuzione query** e **SSQL Server**, fare clic sulla pagina **Generale** e quindi selezionare la casella di controllo **Per impostazione predefinita, apri le nuove query in modalità SQLCMD** .  
  
## <a name="writing-and-editing-sqlcmd-scripts"></a>Scrittura e modifica di script SQLCMD  
 Dopo avere attivato la modalità di scripting, sarà possibile immettere comandi SQLCMD e istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] . Sono applicabili le regole seguenti:  
  
-   I comandi SQLCMD devono essere specificati come prima istruzione di una riga.  
  
-   È consentito un solo comando SQLCMD per riga.  
  
-   I comandi SQLCMD possono essere preceduti da commenti o da spazio vuoto.  
  
-   I comandi SQLCMD all'interno di commenti non vengono eseguiti.  
  
-   I caratteri per i commenti a riga singola sono due segni meno (`--)` e devono essere visualizzati all'inizio della riga.  
  
-   I comandi del sistema operativo devono essere preceduti da due punti esclamativi (`!!`). Il comando formato da due punti esclamativi attiva l'esecuzione dell'istruzione immediatamente seguente tramite il processore dei comandi `cmd.exe` . Il testo dopo `!!` viene passato a `cmd.exe`come parametro e pertanto la riga di comando finale verrà eseguita come: `"%SystemRoot%\system32\cmd.exe /c <text after !!>"`  
  
-   Per una chiara distinzione tra i comandi SQLCMD e [!INCLUDE[tsql](../../includes/tsql-md.md)], è necessario che tutti i comandi SQLCMD utilizzino come prefisso un carattere due punti (`:`).  
  
-   Il comando `GO` può essere usato senza prefisso oppure può essere preceduto da `!!:`  
  
-   L'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] supporta variabili di ambiente e variabili definite come parte di uno script SQLCMD, ma non supporta variabili **osql** o SQLCMD predefinite. L'elaborazione di SQLCMD da parte di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] distingue tra maiuscole e minuscole nelle variabili. Ad esempio, PRINT '$ (COMPUTERNAME)' produce il risultato corretto, ma PRINT '$ (ComputerName)' restituisce un errore.  
  
> [!CAUTION]
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usa [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]SqlClient per l'esecuzione in modalità regolare e SQLCMD. Quando viene eseguito dalla riga di comando, SQLCMD utilizza il provider OLE DB. Poiché le opzioni predefinite che è possibile applicare sono diverse, l'esecuzione della stessa query in modalità SQLCMD di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e nell'utilità SQLCMD potrebbe generare risultati diversi.  
  
## <a name="supported-sqlcmd-syntax"></a>Sintassi SQLCMD supportata  
 L'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] supporta le parole chiave degli script SQLCMD seguenti:  
  
 `[!!:]GO[count]`  
  
 `!! <command>`  
  
 `:exit(statement)`  
  
 `:Quit`  
  
 `:r <filename>`  
  
 `:setvar <var> <value>`  
  
 `:connect server[\instance] [-l login_timeout] [-U user [-P password]]`  
  
 `:on error [ignore|exit]`  
  
 `:error <filename>|stderr|stdout`  
  
 `:out <filename>|stderr|stdout`  
  
> [!NOTE]  
>  Sia per `:error` sia per `:out`, `stderr` e `stdout` inviano l'output alla scheda Messaggi.  
  
 I comandi SQLCMD non inclusi nell'elenco precedente non sono supportati nell'editor di query. Quando viene eseguito uno script contenente parole chiave SQLCMD non supportate, l'editor di query invia un messaggio del tipo "Il comando *\<ignored command*> verrà ignorato" alla destinazione per ogni parola chiave non supportata. Lo script verrà eseguito, ma i comandi non supportati verranno ignorati.  
  
> [!CAUTION]  
>  Poiché SQLCMD non viene avviato dalla riga di comando, l'esecuzione dell'editor di query in modalità SQLCMD presenta alcune limitazioni. Non è possibile passare parametri della riga di comando come variabili e poiché l'editor di query non è in grado di rispondere ai prompt del sistema operativo, evitare di eseguire istruzioni interattive.  
  
## <a name="color-coding-in-sqlcmd-scripts"></a>Codifica a colori negli script SQLCMD  
 Quando è attivata la modalità di scripting SQLCMD, gli script avranno codifica a colori. La codifica a colori per le parole chiave [!INCLUDE[tsql](../../includes/tsql-md.md)] rimarrà invariata. I comandi SQLCMD vengono presentati con uno sfondo ombreggiato.  
  
## <a name="example"></a>Esempio  
 L'esempio seguente usa un'istruzione **sqlcmd** per creare un file di output denominato testoutput.txt ed esegue due istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT insieme a un comando del sistema operativo (per stampare la directory corrente). Il file risultante contiene l'output del messaggio dall'istruzione `DIR` , seguito dall'output dei risultati dalle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
```  
:out C:\testoutput.txt  
SELECT @@VERSION As 'Server Version'  
!!DIR  
!!:GO  
SELECT @@SERVERNAME AS 'Server Name'  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità sqlcmd](../../tools/sqlcmd-utility.md)  
  
  
