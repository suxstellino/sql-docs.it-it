---
description: Creazione di un'origine con il componente script
title: Creazione di un'origine con il componente script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script component [Integration Services], source components
- output columns [Integration Services]
- sources [Integration Services], components
ms.assetid: 547c4179-ea82-4265-8c6f-04a2aa77a3c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1aa4ed0d717e8b2d6dfe7f2b6b7d331936e00100
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344870"
---
# <a name="creating-a-source-with-the-script-component"></a>Creazione di un'origine con il componente script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilizzare un componente di origine nel flusso di dati di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per caricare dati da un'origine dati da passare a trasformazioni e destinazioni a valle. Normalmente, ci si connette all'origine dati tramite una gestione connessione esistente.  
  
 Per una panoramica sulla programmazione del componente Script, vedere [Estensione del flusso di dati con il componente script](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Il componente script e il codice dell'infrastruttura che genera semplificano in modo significativo il processo di sviluppo di un componente del flusso di dati personalizzato. Tuttavia, per comprendere il funzionamento del componente script, può risultare utile leggere informazioni sui passaggi necessari per lo sviluppo di un componente del flusso di dati personalizzato. Vedere la sezione [Developing a Custom Data Flow Component](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) (Sviluppo di un componente del flusso di dati personalizzato) e in particolare l'argomento [Developing a Custom Source Component](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md) (Sviluppo di un componente di origine personalizzato).  
  
## <a name="getting-started-with-a-source-component"></a>Introduzione ai componenti di origine  
 Quando si aggiunge un componente script nel riquadro Flusso di dati di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] viene visualizzata la finestra di dialogo **Seleziona tipo componente script** in cui si richiede di selezionare uno script Origine, Destinazione o Trasformazione. In questa finestra di dialogo selezionare **Origine**.  
  
## <a name="configuring-a-source-component-in-metadata-design-mode"></a>Configurazione di un componente di origine in modalità di progettazione metadati  
 Dopo aver selezionato la creazione di un componente di origine, configurare il componente tramite **Editor trasformazione Script**. Per altre informazioni, vedere [Configurazione del componente script nell'editor corrispondente](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Un componente di origine del flusso di dati non include input e supporta uno o più output. La configurazione degli output per il componente è uno dei passaggi che vanno completati in modalità di progettazione metadati tramite **Editor trasformazione Script** prima di creare lo script personalizzato.  
  
 È anche possibile specificare il linguaggio di scripting mediante l'impostazione della proprietà **ScriptLanguage** nella pagina **Script** di **Editor trasformazione Script**.  
  
> [!NOTE]  
>  Per impostare il linguaggio di scripting predefinito per componenti Script e attività Script, usare l'opzione **Linguaggio di scripting** nella pagina **Generale** della finestra di dialogo **Opzioni**. Per ulteriori informazioni, vedere [General Page](~/integration-services/control-flow/script-task-editor-general-page.md).  
  
### <a name="adding-connection-managers"></a>Aggiunta di gestioni connessioni  
 Normalmente, un componente di origine utilizza una gestione connessione esistente per connettersi all'origine dati da cui carica dati nel flusso di dati. Nella pagina **Gestioni connessioni** di **Editor trasformazione Script** fare clic su **Aggiungi** per aggiungere la gestione connessione appropriata.  
  
 Tuttavia, una gestione connessione è solo un'unità pratica che incapsula e archivia le informazioni necessarie per la connessione a un'origine dati di un determinato tipo. È necessario scrivere codice personalizzato per caricare o salvare i dati e possibilmente anche per aprire e chiudere la connessione all'origine dati.  
  
 Per informazioni generali sull'uso delle gestioni connessioni con il componente Script, vedere [Connessione a origini dati nel componente Script](../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md).  
  
 Per altre informazioni sulla pagina **Gestioni connessioni** di **Editor trasformazione Script**, vedere [Editor trasformazione Script &#40;pagina Gestioni connessioni&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="configuring-outputs-and-output-columns"></a>Configurazione di output e colonne di output  
 Un componente di origine non include input e supporta uno o più output. Nella pagina **Input e output** di **Editor trasformazione Script** è stato creato un singolo output per impostazione predefinita, ma non sono state create colonne di output. In questa pagina dell'editor può essere necessario o consigliabile configurare gli elementi seguenti.  
  
-   È necessario aggiungere e configurare manualmente le colonne di output per ogni output. Selezionare la cartella Colonne di output per ogni output, quindi usare i pulsanti **Aggiungi colonna** e **Rimuovi colonna** per gestire le colonne di output per ogni output del componente di origine. In seguito, si farà riferimento alle colonne di output nello script in base ai nomi assegnati in questo passaggio, utilizzando le proprietà delle funzioni di accesso tipizzate create nel codice generato automaticamente.  
  
-   È possibile creare uno o più output aggiuntivi, ad esempio un output degli errori simulati per le righe che contengono valori imprevisti. Usare i pulsanti **Aggiungi output** e **Rimuovi output** per gestire gli output del componente di origine. Tutte le righe di input vengono indirizzate a tutti gli output disponibili, a meno che non si specifichi anche un valore identico diverso da zero per la proprietà **ExclusionGroup** di tali output nei casi in cui si intende indirizzare ogni riga a uno solo degli output che condividono lo stesso valore di **ExclusionGroup**. Il valore intero specifico selezionato per identificare **ExclusionGroup** non è significativo.  
  
    > [!NOTE]  
    >  È anche possibile usare un valore diverso da zero per la proprietà **ExclusionGroup** con un singolo output se non si vuole restituire tutte le righe come output. In questo caso, tuttavia, è necessario chiamare in modo esplicito il metodo **DirectRowTo\<outputbuffer>** per ogni riga che si vuole inviare all'output.  
  
-   È necessario assegnare un nome descrittivo agli output. In seguito, si farà riferimento agli output in base ai nomi presenti nello script, utilizzando le proprietà delle funzioni di accesso tipizzate create nel codice generato automaticamente.  
  
-   Normalmente, più output nello stesso oggetto **ExclusionGroup** dispongono delle stesse colonne di output. Tuttavia, se si crea un output degli errori simulati, è possibile aggiungere più colonne per archiviare le informazioni sugli errori. Per informazioni su come il motore flusso di dati elabora le righe di errore, vedere [Uso degli output degli errori in un componente del flusso di dati](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md). Nel componente script, tuttavia, è necessario scrivere codice personalizzato per inserire le informazioni appropriate sugli errori nelle colonne aggiuntive. Per altre informazioni, vedere [Simulazione di un output degli errori per il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Per altre informazioni sulla pagina **Input e output** di **Editor trasformazione Script**, vedere [Editor trasformazione Script &#40;pagina Input e output&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="adding-variables"></a>Aggiunta di variabili  
 Se si vuole usare valori di variabili esistenti nello script, è possibile aggiungerli nei campi delle proprietà **ReadOnlyVariables** e **ReadWriteVariables** della pagina **Script** di **Editor trasformazione Script**.  
  
 Quando si immettono più variabili nei campi delle proprietà, separare i relativi nomi con virgole. È anche possibile immettere più variabili facendo clic sul pulsante con i puntini di sospensione ( **...** ) accanto ai campi delle proprietà **ReadOnlyVariables** e **ReadWriteVariables** e selezionando le variabili nella finestra di dialogo **Seleziona variabili**.  
  
 Per informazioni generali sull'uso delle variabili con il componente script, vedere [Uso di variabili nel componente script](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Per altre informazioni sulla pagina **Script** di **Editor trasformazione Script**, vedere [Editor trasformazione Script &#40;pagina Script&#41;](../data-flow/transformations/script-component.md).  
  
## <a name="scripting-a-source-component-in-code-design-mode"></a>Generazione di script per un componente di origine in modalità di progettazione codice  
 Dopo aver configurato i metadati per il componente, aprire l'IDE di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) per scrivere il codice dello script personalizzato. Per aprire VSTA fare clic su **Modifica script** nella pagina **Script** di **Editor trasformazione Script**. È possibile creare lo script usando [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#, a seconda del linguaggio di scripting selezionato per la proprietà **ScriptLanguage**.  
  
 Per importanti informazioni applicabili a tutti i tipi di componenti creati tramite il componente script, vedere [Codifica e debug del componente script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Informazioni sul codice generato automaticamente  
 Quando si apre l'IDE di VSTA dopo la creazione e la configurazione di un componente di origine, la classe **ScriptMain** modificabile viene visualizzata nell'editor del codice. Scrivere il codice personalizzato nella classe **ScriptMain**.  
  
 La classe **ScriptMain** include uno stub per il metodo **CreateNewOutputRows**. **CreateNewOutputRows** è il metodo più importante di un componente di origine.  
  
 Se si apre la finestra **Esplora progetti** in VSTA, è possibile rilevare che il componente script ha generato anche gli elementi del progetto **BufferWrapper** e **ComponentWrapper** di sola lettura. La classe **ScriptMain** eredita dalla classe **UserComponent** nell'elemento di progetto **ComponentWrapper**.  
  
 In fase di esecuzione il motore flusso di dati chiama il metodo **PrimeOutput** nella classe **UserComponent**, che esegue l'override del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PrimeOutput%2A> della classe padre <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. Il metodo **PrimeOutput** chiama a sua volta i metodi seguenti:  
  
1.  Metodo **CreateNewOutputRows**, di cui si esegue l'override in **ScriptMain** per aggiungere righe dall'origine dati ai buffer di output, che sono inizialmente vuoti.  
  
2.  Metodo **FinishOutputs**, che per impostazione predefinita è vuoto. Eseguire l'override di questo metodo in **ScriptMain** per eseguire l'elaborazione necessaria al completamento dell'output.  
  
3.  Metodo privato **MarkOutputsAsFinished**, che chiama il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer.SetEndOfRowset%2A> della classe padre <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> per indicare al motore flusso di dati che l'output è completo. Non è necessario chiamare in modo esplicito **SetEndOfRowset** nel codice.  
  
### <a name="writing-your-custom-code"></a>Scrittura di codice personalizzato  
 Per completare la creazione di un componente di origine personalizzato, è possibile creare script nei metodi seguenti disponibili nella classe **ScriptMain**.  
  
1.  Eseguire l'override del metodo **AcquireConnections** per connettersi all'origine dati esterna. Estrarre l'oggetto connessione o le informazioni di connessione necessarie dalla gestione connessione.  
  
2.  Eseguire l'override del metodo **PreExecute** per caricare dati, se è possibile caricare contemporaneamente tutti i dati di origine. Ad esempio, è possibile eseguire **SqlCommand** su una connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e caricare contemporaneamente tutti i dati di origine in un oggetto **SqlDataReader**. Se è necessario caricare i dati di origine una riga alla volta (ad esempio nel caso della lettura di un file di testo), è possibile caricare i dati mentre si esegue il ciclo delle righe in **CreateNewOutputRows**.  
  
3.  Usare il metodo **CreateNewOutputRows** sottoposto a override per aggiungere nuove righe ai buffer di output vuoti e per inserire i valori di ogni colonna nelle nuove righe di output. Usare il metodo **AddRow** di ogni buffer di output per aggiungere una nuova riga vuota, quindi impostare i valori di ogni colonna. In genere si copiano i valori dalle colonne caricate dall'origine esterna.  
  
4.  Eseguire l'override del metodo **PostExecute** per completare l'elaborazione dei dati. Ad esempio, è possibile chiudere l'elemento **SqlDataReader** usato per il caricamento dei dati.  
  
5.  Eseguire l'override del metodo **ReleaseConnections** per disconnettersi dall'origine dati esterna, se necessario.  
  
## <a name="examples"></a>Esempi  
 Negli esempi seguenti è illustrato il codice personalizzato necessario nella classe **ScriptMain** per creare un componente di origine.  
  
> [!NOTE]  
>  Gli esempi usano la tabella **Person.Address** del database di esempio **AdventureWorks** e passano la prima e la quarta colonna, ovvero le colonne **intAddressID** e **nvarchar(30)City**, attraverso il flusso di dati. Gli stessi dati vengono utilizzati negli esempi relativi a origine, trasformazione e destinazione in questa sezione. Per ogni esempio, sono documentati ulteriori prerequisiti e presupposti.  
  
### <a name="adonet-source-example"></a>Esempio di origine ADO.NET  
 In questo esempio è illustrato un componente di origine che usa una gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] esistente per caricare dati da una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel flusso di dati.  
  
 Se si desidera eseguire questo codice di esempio, è necessario configurare il pacchetto e il componente come segue:  
  
1.  Creare una gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] che usa il provider **SqlClient** per connettersi al database **AdventureWorks**.  
  
2.  Aggiungere un nuovo componente script all'area di progettazione del flusso di dati e configurarlo come origine.  
  
3.  Aprire l'**Editor trasformazione Script**. Nella pagina **Input e output** rinominare l'output predefinito con un nome più descrittivo, ad esempio **MyAddressOutput**, quindi aggiungere e configurare le due colonne di output, **AddressID** e **City**.  
  
    > [!NOTE]  
    >  Assicurarsi di modificare il tipo di dati della colonna di output **City** in DT_WSTR.  
  
4.  Nella pagina **Gestioni connessioni** aggiungere o creare la gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] e specificare un nome, ad esempio **MyADONETConnection**.  
  
5.  Nella pagina **Script** fare clic su **Modifica script** e immettere lo script seguente. Chiudere quindi l'ambiente di sviluppo dello script e **Editor trasformazione Script**.  
  
6.  Creare e configurare un componente di destinazione, ad esempio una destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il componente di destinazione di esempio illustrato in [Creazione di una destinazione con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md), in cui sono previste le colonne **AddressID** e **City**. Quindi, connettere il componente di origine alla destinazione. È possibile connettere direttamente un'origine a una destinazione senza alcuna trasformazione. È possibile creare una tabella di destinazione eseguendo il comando [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente nel database **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Eseguire l'esempio.  
  
    ```vb  
    Imports System.Data.SqlClient  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim connMgr As IDTSConnectionManager100  
        Dim sqlConn As SqlConnection  
        Dim sqlReader As SqlDataReader  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            connMgr = Me.Connections.MyADONETConnection  
            sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim cmd As New SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn)  
            sqlReader = cmd.ExecuteReader  
  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Do While sqlReader.Read  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = sqlReader.GetInt32(0)  
                    .City = sqlReader.GetString(1)  
                End With  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
  
            sqlReader.Close()  
  
        End Sub  
  
        Public Overrides Sub ReleaseConnections()  
  
            connMgr.ReleaseConnection(sqlConn)  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.Data.SqlClient;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        IDTSConnectionManager100 connMgr;  
        SqlConnection sqlConn;  
        SqlDataReader sqlReader;  
  
        public override void AcquireConnections(object Transaction)  
        {  
            connMgr = this.Connections.MyADONETConnection;  
            sqlConn = (SqlConnection)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
  
            SqlCommand cmd = new SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn);  
            sqlReader = cmd.ExecuteReader();  
  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            while (sqlReader.Read())  
            {  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = sqlReader.GetInt32(0);  
                    MyAddressOutputBuffer.City = sqlReader.GetString(1);  
                }  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            sqlReader.Close();  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            connMgr.ReleaseConnection(sqlConn);  
  
        }  
  
    }  
    ```  
  
### <a name="flat-file-source-example"></a>Esempio di origine file flat  
 In questo esempio è illustrato un componente di origine che utilizza una gestione connessione file flat esistente per caricare dati da un file flat nel flusso di dati. I dati di origine del file flat vengono creati esportandoli da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se si desidera eseguire questo codice di esempio, è necessario configurare il pacchetto e il componente come segue:  
  
1.  Usare l'Importazione/Esportazione guidata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per esportare la tabella **Person.Address** dal database di esempio **AdventureWorks** a un file flat delimitato da virgole. In questo esempio viene utilizzato il nome file ExportedAddresses.txt.  
  
2.  Creare una gestione connessione file flat che si connette al file di dati esportato.  
  
3.  Aggiungere un nuovo componente script all'area di progettazione del flusso di dati e configurarlo come origine.  
  
4.  Aprire l'**Editor trasformazione Script**. Nella pagina **Input e output** rinominare l'output predefinito con un nome più descrittivo, ad esempio **MyAddressOutput**. Aggiungere e configurare le due colonne di output, **AddressID** e **City**.  
  
5.  Nella pagina **Gestioni connessioni** aggiungere o creare la gestione connessione file flat usando un nome descrittivo, ad esempio **MyFlatFileSrcConnectionManager**.  
  
6.  Nella pagina **Script** fare clic su **Modifica script** e immettere lo script seguente. Chiudere quindi l'ambiente di sviluppo dello script e **Editor trasformazione Script**.  
  
7.  Creare e configurare un componente di destinazione, ad esempio una destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il componente di destinazione di esempio illustrato in [Creazione di una destinazione con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Quindi, connettere il componente di origine alla destinazione. È possibile connettere direttamente un'origine a una destinazione senza alcuna trasformazione. È possibile creare una tabella di destinazione eseguendo il comando [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente nel database **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
8.  Eseguire l'esempio.  
  
    ```vb  
    Imports System.IO  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Private textReader As StreamReader  
        Private exportedAddressFile As String  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connMgr As IDTSConnectionManager100 = _  
                Me.Connections.MyFlatFileSrcConnectionManager  
            exportedAddressFile = _  
                CType(connMgr.AcquireConnection(Nothing), String)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
            MyBase.PreExecute()  
            textReader = New StreamReader(exportedAddressFile)  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Dim nextLine As String  
            Dim columns As String()  
  
            Dim delimiters As Char()  
            delimiters = ",".ToCharArray  
  
            nextLine = textReader.ReadLine  
            Do While nextLine IsNot Nothing  
                columns = nextLine.Split(delimiters)  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = columns(0)  
                    .City = columns(3)  
                End With  
                nextLine = textReader.ReadLine  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
            MyBase.PostExecute()  
            textReader.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.IO;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        private StreamReader textReader;  
        private string exportedAddressFile;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            IDTSConnectionManager100 connMgr = this.Connections.MyFlatFileSrcConnectionManager;  
            exportedAddressFile = (string)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
            base.PreExecute();  
            textReader = new StreamReader(exportedAddressFile);  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            string nextLine;  
            string[] columns;  
  
            char[] delimiters;  
            delimiters = ",".ToCharArray();  
  
            nextLine = textReader.ReadLine();  
            while (nextLine != null)  
            {  
                columns = nextLine.Split(delimiters);  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = columns[0];  
                    MyAddressOutputBuffer.City = columns[3];  
                }  
                nextLine = textReader.ReadLine();  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            base.PostExecute();  
            textReader.Close();  
  
        }  
  
    }  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di una destinazione con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)   
 [Sviluppo di un componente di origine personalizzato](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)  
  
