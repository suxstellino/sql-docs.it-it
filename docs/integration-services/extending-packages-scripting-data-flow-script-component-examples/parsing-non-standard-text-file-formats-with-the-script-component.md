---
description: Analisi di formati di file di testo non standard con il componente script
title: Analisi di formati di file di testo non standard con il componente script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- text file reading [Integration Services]
- Script component [Integration Services], non-standard text file formats
- transformations [Integration Services], components
- Script component [Integration Services], examples
ms.assetid: 1fda034d-09e4-4647-9a9f-e8d508c2cc8f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c48454ceec4f05f6b2386e4abb96092bbbbcb140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344867"
---
# <a name="parsing-non-standard-text-file-formats-with-the-script-component"></a>Analisi di formati di file di testo non standard con il componente script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Quando i dati di origine sono disposti in un formato non standard, può risultare utile consolidare tutta la logica di analisi in un singolo script anziché concatenare più trasformazioni di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per ottenere lo stesso risultato.  
  
 [Esempio 1: Analisi di record delimitati da righe](#example1)  
  
 [Esempio 2: Divisione di record padre e figlio](#example2)  
  
> [!NOTE]  
>  Se si desidera creare un componente da riutilizzare più facilmente con più attività Flusso di dati e più pacchetti, è possibile utilizzare il codice di questo esempio di componente script come punto iniziale per un componente del flusso di dati personalizzato. Per altre informazioni, vedere [Sviluppo di un componente del flusso di dati personalizzato](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
##  <a name="example-1-parsing-row-delimited-records"></a><a name="example1"></a>Esempio 1: analisi di record delimitati da righe  
 In questo esempio viene illustrato come utilizzare il componente script per analizzare in una tabella di destinazione un file di testo in cui ogni colonna di dati appare in una riga distinta.  
  
 Per altre informazioni su come configurare il componente script per usarlo come trasformazione nel flusso di dati, vedere [Creazione di una trasformazione sincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) e [Creazione di una trasformazione asincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md).  
  
#### <a name="to-configure-this-script-component-example"></a>Per configurare l'esempio di componente script  
  
1.  Creare e salvare un file di testo denominato **rowdelimiteddata.txt** che contenga l'origine dati seguente:  
  
    ```  
    FirstName: Nancy  
    LastName: Davolio  
    Title: Sales Representative  
    City: Seattle  
    StateProvince: WA  
  
    FirstName: Andrew  
    LastName: Fuller  
    Title: Vice President, Sales  
    City: Tacoma  
    StateProvince: WA  
  
    FirstName: Steven  
    LastName: Buchanan  
    Title: Sales Manager  
    City: London  
    StateProvince:  
  
    ```  
  
2.  Aprire [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Selezionare un database di destinazione e aprire una nuova finestra Query. Nella finestra Query eseguire lo script seguente per creare la tabella di destinazione:  
  
    ```sql
    create table RowDelimitedData  
    (  
    FirstName varchar(32),  
    LastName varchar(32),  
    Title varchar(32),  
    City varchar(32),  
    StateProvince varchar(32)  
    )  
  
    ```  
  
4.  Aprire [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] e creare un nuovo pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] denominato ParseRowDelim.dtsx.  
  
5.  Aggiungere una gestione connessione file flat al pacchetto, denominarla RowDelimitedData e configurarla per la connessione al file rowdelimiteddata.txt creato in un passaggio precedente.  
  
6.  Aggiungere una gestione connessione OLE DB al pacchetto e configurarla per la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e al database in cui è stata creata la tabella di destinazione.  
  
7.  Aggiungere un'attività Flusso di dati al pacchetto e fare clic sulla scheda **Flusso di dati** di Progettazione SSIS.  
  
8.  Aggiungere un'origine file flat al flusso di dati e configurarla per l'utilizzo della gestione connessione RowDelimitedData. Nella pagina **Colonne** dell'**Editor origine file flat** selezionare l'unica colonna esterna disponibile.  
  
9. Aggiungere un componente script al flusso di dati e configurarlo come trasformazione. Connettere l'output dell'origine file flat al componente script.  
  
10. Fare doppio clic sul componente script per visualizzare l'**Editor trasformazione Script**.  
  
11. Nella pagina **Colonne di input** dell'**Editor trasformazione Script** selezionare l'unica colonna di input disponibile.  
  
12. Nella pagina **Input e output** dell'**Editor trasformazione Script** selezionare Output 0 e impostare il relativo **SynchronousInputID** su Nessuno. Creare 5 colonne di output, tutte di tipo string [DT_STR] con lunghezza 32:  
  
    -   FirstName  
  
    -   LastName  
  
    -   Title  
  
    -   city  
  
    -   StateProvince  
  
13. Nella pagina **Script** dell'**Editor trasformazione Script** fare clic su **Modifica script** e immettere il codice illustrato nella classe **ScriptMain** dell'esempio. Chiudere l'ambiente di sviluppo dello script e l'**Editor trasformazione Script**.  
  
14. Aggiungere una destinazione SQL Server al flusso di dati. Configurarlo per l'utilizzo della gestione connessione OLE DB e della tabella RowDelimitedData. Connettere l'output del componente script a questa destinazione.  
  
15. Eseguire il pacchetto. Al termine dell'esecuzione del pacchetto, esaminare i record nella tabella di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
    Dim columnName As String  
    Dim columnValue As String  
  
    ' Check for an empty row.  
    If Row.Column0.Trim.Length > 0 Then  
        columnName = Row.Column0.Substring(0, Row.Column0.IndexOf(":"))  
        ' Check for an empty value after the colon.  
        If Row.Column0.Substring(Row.Column0.IndexOf(":")).TrimEnd.Length > 1 Then  
            ' Extract the column value from after the colon and space.  
            columnValue = Row.Column0.Substring(Row.Column0.IndexOf(":") + 2)  
            Select Case columnName  
                Case "FirstName"  
                    ' The FirstName value indicates a new record.  
                    Me.Output0Buffer.AddRow()  
                    Me.Output0Buffer.FirstName = columnValue  
                Case "LastName"  
                    Me.Output0Buffer.LastName = columnValue  
                Case "Title"  
                    Me.Output0Buffer.Title = columnValue  
                Case "City"  
                    Me.Output0Buffer.City = columnValue  
                Case "StateProvince"  
                    Me.Output0Buffer.StateProvince = columnValue  
            End Select  
        End If  
    End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
  
        string columnName;  
        string columnValue;  
  
        // Check for an empty row.  
        if (Row.Column0.Trim().Length > 0)  
        {  
            columnName = Row.Column0.Substring(0, Row.Column0.IndexOf(":"));  
            // Check for an empty value after the colon.  
            if (Row.Column0.Substring(Row.Column0.IndexOf(":")).TrimEnd().Length > 1)  
            // Extract the column value from after the colon and space.  
            {  
                columnValue = Row.Column0.Substring(Row.Column0.IndexOf(":") + 2);  
                switch (columnName)  
                {  
                    case "FirstName":  
                        // The FirstName value indicates a new record.  
                        this.Output0Buffer.AddRow();  
                        this.Output0Buffer.FirstName = columnValue;  
                        break;  
                    case "LastName":  
                        this.Output0Buffer.LastName = columnValue;  
                        break;  
                    case "Title":  
                        this.Output0Buffer.Title = columnValue;  
                        break;  
                    case "City":  
                        this.Output0Buffer.City = columnValue;  
                        break;  
                    case "StateProvince":  
                        this.Output0Buffer.StateProvince = columnValue;  
                        break;  
                }  
            }  
        }  
  
    }  
```  
  
##  <a name="example-2-splitting-parent-and-child-records"></a><a name="example2"></a> Esempio 2: Divisione di record padre e figlio  
 In questo esempio viene illustrato come utilizzare il componente script per analizzare in tabelle di destinazione padre e figlio correttamente normalizzate un file di testo in cui una riga del separatore precede una riga di record padre seguita da un numero indefinito di righe di record figlio. Questo semplice esempio può essere facilmente adottato per file di origine che utilizzano più di una riga o colonna per ogni record padre e figlio, purché esista la possibilità di identificare l'inizio e la fine di ogni record.  
  
> [!CAUTION]  
>  Questo esempio viene riportato a scopo puramente dimostrativo. Se viene eseguito più di una volta, verranno inseriti valori di chiave duplicati nella tabella di destinazione.  
  
 Per altre informazioni su come configurare il componente script per usarlo come trasformazione nel flusso di dati, vedere [Creazione di una trasformazione sincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) e [Creazione di una trasformazione asincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md).  
  
#### <a name="to-configure-this-script-component-example"></a>Per configurare l'esempio di componente script  
  
1.  Creare e salvare un file di testo denominato **parentchilddata.txt** che contenga l'origine dati seguente:  
  
    ```  
    **********  
    PARENT 1 DATA  
    child 1 data  
    child 2 data  
    child 3 data  
    child 4 data  
    **********  
    PARENT 2 DATA  
    child 5 data  
    child 6 data  
    child 7 data  
    child 8 data  
    **********  
  
    ```  
  
2.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Selezionare un database di destinazione e aprire una nuova finestra Query. Nella finestra Query eseguire lo script seguente per creare le tabelle di destinazione:  
  
    ```sql
    CREATE TABLE [dbo].[Parents]([ParentID] [int] NOT NULL,  
    [ParentRecord] [varchar](32) NOT NULL,  
     CONSTRAINT [PK_Parents] PRIMARY KEY CLUSTERED   
    ([ParentID] ASC))  
    GO  
    CREATE TABLE [dbo].[Children]([ChildID] [int] NOT NULL,  
    [ParentID] [int] NOT NULL,  
    [ChildRecord] [varchar](32) NOT NULL,  
     CONSTRAINT [PK_Children] PRIMARY KEY CLUSTERED   
    ([ChildID] ASC))  
    GO  
    ALTER TABLE [dbo].[Children] ADD CONSTRAINT [FK_Children_Parents] FOREIGN KEY([ParentID])  
    REFERENCES [dbo].[Parents] ([ParentID])  
  
    ```  
  
4.  Aprire [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] e creare un nuovo pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] denominato SplitParentChild.dtsx.  
  
5.  Aggiungere una gestione connessione file flat al pacchetto, denominarla ParentChildData e configurarla per la connessione al file parentchilddata.txt creato in un passaggio precedente.  
  
6.  Aggiungere una gestione connessione OLE DB al pacchetto e configurarla per la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e al database in cui sono state create le tabelle di destinazione.  
  
7.  Aggiungere un'attività Flusso di dati al pacchetto e fare clic sulla scheda **Flusso di dati** di Progettazione SSIS.  
  
8.  Aggiungere un'origine file flat al flusso di dati e configurarla per l'utilizzo della gestione connessione ParentChildData. Nella pagina **Colonne** dell'**Editor origine file flat** selezionare l'unica colonna esterna disponibile.  
  
9. Aggiungere un componente script al flusso di dati e configurarlo come trasformazione. Connettere l'output dell'origine file flat al componente script.  
  
10. Fare doppio clic sul componente script per visualizzare l'**Editor trasformazione Script**.  
  
11. Nella pagina **Colonne di input** dell'**Editor trasformazione Script** selezionare l'unica colonna di input disponibile.  
  
12. Nella pagina **Input e output** dell'**Editor trasformazione Script** selezionare Output 0, rinominarlo in ParentRecords e impostare il relativo **SynchronousInputID** su Nessuno. Creare 2 colonne di output:  
  
    -   ParentID (chiave primaria), di tipo integer con segno a quattro byte [DT_I4]  
  
    -   ParentRecord, di tipo stringa [DT_STR] con lunghezza 32.  
  
13. Creare un secondo output e denominarlo ChildRecords. L'elemento **SynchronousInputID** del nuovo output è già impostato su Nessuno. Creare 3 colonne di output:  
  
    -   ChildID (chiave primaria), di tipo integer con segno a quattro byte [DT_I4]  
  
    -   ParentID (chiave esterna), sempre di tipo integer con segno a 4 byte [DT_I4]  
  
    -   ChildRecord, di tipo stringa [DT_STR] con lunghezza 50.  
  
14. Nella pagina **Script** dell'**Editor trasformazione Script** fare clic su **Modifica script**. Nella classe **ScriptMain** immettere il codice illustrato nell'esempio. Chiudere l'ambiente di sviluppo dello script e l'**Editor trasformazione Script**.  
  
15. Aggiungere una destinazione SQL Server al flusso di dati. Connettere l'output ParentRecords del componente script a questa destinazione. Configurarlo per l'utilizzo della gestione connessione OLE DB e della tabella Parents.  
  
16. Aggiungere un'altra destinazione SQL Server al flusso di dati. Connettere l'output ChildRecords del componente script a questa destinazione. Configurarlo per l'utilizzo della gestione connessione OLE DB e della tabella Children.  
  
17. Eseguire il pacchetto. Al termine dell'esecuzione del pacchetto, esaminare i record padre e figlio nelle due tabelle di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
    Static nextRowIsParent As Boolean = False  
    Static parentCounter As Integer = 0  
    Static childCounter As Integer = 0  
  
    ' If current row starts with separator characters,  
    '  then following row contains new parent record.  
    If Row.Column0.StartsWith("***") Then  
        nextRowIsParent = True  
    Else  
        If nextRowIsParent Then  
            ' Current row contains parent record.  
            parentCounter += 1  
            Me.ParentRecordsBuffer.AddRow()  
            Me.ParentRecordsBuffer.ParentID = parentCounter  
            Me.ParentRecordsBuffer.ParentRecord = Row.Column0  
            nextRowIsParent = False  
        Else  
            ' Current row contains child record.  
            childCounter += 1  
            Me.ChildRecordsBuffer.AddRow()  
            Me.ChildRecordsBuffer.ChildID = childCounter  
            Me.ChildRecordsBuffer.ParentID = parentCounter  
            Me.ChildRecordsBuffer.ChildRecord = Row.Column0  
        End If  
    End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
  
    int static_Input0_ProcessInputRow_childCounter = 0;  
    int static_Input0_ProcessInputRow_parentCounter = 0;  
    bool static_Input0_ProcessInputRow_nextRowIsParent = false;  
  
        // If current row starts with separator characters,   
        // then following row contains new parent record.   
        if (Row.Column0.StartsWith("***"))  
        {  
            static_Input0_ProcessInputRow_nextRowIsParent = true;  
        }  
        else  
        {  
            if (static_Input0_ProcessInputRow_nextRowIsParent)  
            {  
                // Current row contains parent record.   
                static_Input0_ProcessInputRow_parentCounter += 1;  
                this.ParentRecordsBuffer.AddRow();  
                this.ParentRecordsBuffer.ParentID = static_Input0_ProcessInputRow_parentCounter;  
                this.ParentRecordsBuffer.ParentRecord = Row.Column0;  
                static_Input0_ProcessInputRow_nextRowIsParent = false;  
            }  
            else  
            {  
                // Current row contains child record.   
                static_Input0_ProcessInputRow_childCounter += 1;  
                this.ChildRecordsBuffer.AddRow();  
                this.ChildRecordsBuffer.ChildID = static_Input0_ProcessInputRow_childCounter;  
                this.ChildRecordsBuffer.ParentID = static_Input0_ProcessInputRow_parentCounter;  
                this.ChildRecordsBuffer.ChildRecord = Row.Column0;  
            }  
        }  
  
    }  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di una trasformazione sincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
 [Creazione di una trasformazione asincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
