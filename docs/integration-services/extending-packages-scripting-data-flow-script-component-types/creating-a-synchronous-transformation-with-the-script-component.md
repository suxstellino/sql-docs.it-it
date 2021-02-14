---
description: Creazione di una trasformazione sincrona con il componente script
title: Creazione di una trasformazione sincrona con il componente script | Microsoft Docs
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
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: aa1bee1a-ab06-44d8-9944-4bff03d73016
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 08c7e20120ef5ff746b495535843bc93a46fea16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344888"
---
# <a name="creating-a-synchronous-transformation-with-the-script-component"></a>Creazione di una trasformazione sincrona con il componente script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilizzare un componente di trasformazione nel flusso di dati di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per modificare e analizzare i dati quando vengono passati dall'origine alla destinazione. Una trasformazione con output sincroni elabora ogni riga di input non appena viene passata attraverso il componente. Una trasformazione con output asincroni attende di aver ricevuto tutte le righe di input per completare la relativa elaborazione. In questo argomento viene descritta una trasformazione sincrona. Per informazioni sulle trasformazioni asincrone, vedere [Creazione di una trasformazione asincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md). Per altre informazioni sulla differenza tra componenti sincroni e asincroni, vedere [Informazioni sulle trasformazioni sincrone e asincrone](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Per una panoramica sulla programmazione del componente Script, vedere [Estensione del flusso di dati con il componente script](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Il componente script e il codice dell'infrastruttura che genera semplificano in modo significativo il processo di sviluppo di un componente del flusso di dati personalizzato. Tuttavia, per comprendere il funzionamento del componente script, può risultare utile leggere informazioni sui passaggi necessari per lo sviluppo di un componente del flusso di dati personalizzato nella sezione [Sviluppo di un componente del flusso di dati personalizzato](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) e in particolare in [Sviluppo di un componente di destinazione personalizzato con output sincroni](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md).  
  
## <a name="getting-started-with-a-synchronous-transformation-component"></a>Introduzione ai componenti di trasformazione sincroni  
 Quando si aggiunge un componente script nel riquadro Flusso di dati di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] viene visualizzata la finestra di dialogo **Seleziona tipo componente script** che richiede di selezionare un tipo di componente Origine, Destinazione o Trasformazione. In questa finestra di dialogo selezionare **Trasformazione**.  
  
## <a name="configuring-a-synchronous-transformation-component-in-metadata-design-mode"></a>Configurazione di un componente di trasformazione sincrono in modalità di progettazione metadati  
 Dopo aver selezionato l'opzione per creare un componente di trasformazione, configurare il componente mediante **Editor trasformazione Script**. Per altre informazioni, vedere [Configurazione del componente script nell'editor corrispondente](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Per impostare il linguaggio di scripting del componente script, impostare la proprietà **ScriptLanguage** nella pagina **Script** di **Editor trasformazione Script**.  
  
> [!NOTE]  
>  Per impostare il linguaggio di scripting predefinito per il componente script, usare l'opzione **Linguaggio di scripting** nella pagina **Generale** della finestra di dialogo **Opzioni**. Per altre informazioni, vedere [Pagina Generale](../general-page-of-integration-services-designers-options.md).  
  
 Un componente di trasformazione del flusso di dati include un input e supporta uno o più output. La configurazione dell'input e degli output per il componente è uno dei passaggi che è necessario completare in modalità di progettazione metadati tramite **Editor trasformazione Script** prima di creare lo script personalizzato.  
  
### <a name="configuring-input-columns"></a>Configurazione delle colonne di input  
 Un componente di trasformazione include un unico input.  
  
 Nella pagina **Colonne di input** di **Editor trasformazione Script** l'elenco di colonne contiene le colonne disponibili dell'output del componente a monte nel flusso di dati. Selezionare le colonne che si desidera trasformare o passare. Contrassegnare le colonne che si desidera trasformare sul posto come di lettura/scrittura.  
  
 Per altre informazioni sulla pagina **Colonne di input** di **Editor trasformazione Script**, vedere [Editor trasformazione Script &#40;pagina Colonne di input&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>Configurazione di input, output e colonne di output  
 Un componente di trasformazione supporta uno o più output.  
  
 Nella pagina **Input e output** di **Editor trasformazione Script** è possibile verificare che è stato creato un singolo output, che tuttavia non include colonne. In questa pagina dell'editor può essere necessario o consigliabile configurare gli elementi seguenti.  
  
-   Creare uno o più output aggiuntivi, ad esempio un output degli errori simulati per le righe che contengono valori imprevisti. Usare i pulsanti **Aggiungi output** e **Rimuovi output** per gestire gli output del componente di trasformazione sincrono. Tutte le righe di input vengono indirizzate a tutti gli output disponibili, a meno che non si indichi che si intende reindirizzare ogni riga a uno o all'altro output. Indicare che si intende reindirizzare le righe specificando un valore Integer diverso da zero per la proprietà **ExclusionGroup** degli output. Il valore Integer specifico immesso in **ExclusionGroup** per identificare gli output non è significativo, ma è necessario usare sempre lo stesso valore per il gruppo di output specificato.  
  
    > [!NOTE]  
    >  È anche possibile usare un valore diverso da zero per la proprietà **ExclusionGroup** con un singolo output se non si vuole restituire tutte le righe come output. In questo caso, tuttavia, è necessario chiamare in modo esplicito il metodo **DirectRowTo\<outputbuffer>** per ogni riga che si vuole inviare all'output.  
  
-   Assegnare un nome più descrittivo all'input e agli output. Il componente script utilizza questi nomi per generare le proprietà delle funzioni di accesso tipizzate che verranno utilizzate per fare riferimento all'input e agli output nello script.  
  
-   Lasciare le colonne nello stato in cui si trovano per le trasformazioni sincrone. In genere una trasformazione sincrona non aggiunge colonne al flusso di dati. I dati vengono modificati sul posto nel buffer, che viene quindi passato al componente successivo nel flusso di dati. In questo caso, non è necessario aggiungere e configurare in modo esplicito colonne di output negli output della trasformazione. Gli output vengono visualizzati nell'editor senza colonne definite in modo esplicito.  
  
-   Aggiungere le nuove colonne agli output degli errori simulati per gli errori a livello di riga. Normalmente, più output nello stesso oggetto **ExclusionGroup** dispongono dello stesso set di colonne di output. Tuttavia, se si crea un output degli errori simulati, è possibile aggiungere più colonne per contenere le informazioni sugli errori. Per informazioni su come il motore flusso di dati elabora le righe di errore, vedere [Uso degli output degli errori in un componente del flusso di dati](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md). Si noti che nel componente script è necessario scrivere codice personalizzato per inserire le informazioni appropriate sugli errori nelle colonne aggiuntive. Per altre informazioni, vedere [Simulazione di un output degli errori per il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Per altre informazioni sulla pagina **Input e output** di **Editor trasformazione Script**, vedere [Editor trasformazione Script &#40;pagina Input e output&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="adding-variables"></a>Aggiunta di variabili  
 Se si vuole usare variabili esistenti nello script, è possibile aggiungerle nei campi delle proprietà **ReadOnlyVariables** e **ReadWriteVariables** della pagina **Script** di **Editor trasformazione Script**.  
  
 Quando si aggiungono più variabili nei campi delle proprietà, separare i relativi nomi con virgole. È anche possibile selezionare più variabili facendo clic sul pulsante con i puntini di sospensione ( **...** ) accanto ai campi delle proprietà **ReadOnlyVariables** e **ReadWriteVariables** e selezionando le variabili nella finestra di dialogo **Seleziona variabili**.  
  
 Per informazioni generali sull'uso delle variabili con il componente script, vedere [Uso di variabili nel componente script](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Per altre informazioni sulla pagina **Script** di **Editor trasformazione Script**, vedere [Editor trasformazione Script &#40;pagina Script&#41;](../data-flow/transformations/script-component.md).  
  
## <a name="scripting-a-synchronous-transformation-component-in-code-design-mode"></a>Scripting di un componente di trasformazione sincrono in modalità di progettazione codice  
 Dopo aver configurato i metadati per il componente, è possibile scrivere lo script personalizzato. Nella pagina **Script** di **Editor trasformazione Script** fare clic su **Modifica script** per aprire l'IDE di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA), in cui è possibile aggiungere lo script personalizzato. Il linguaggio di scripting che si usa varia a seconda che sia stato selezionato [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# come linguaggio di scripting per la proprietà **ScriptLanguage** nella pagina **Script**.  
  
 Per importanti informazioni applicabili a tutti i tipi di componenti creati tramite il componente script, vedere [Codifica e debug del componente script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Informazioni sul codice generato automaticamente  
 Quando si apre l'IDE di VSTA dopo la creazione e la configurazione di un componente di trasformazione, la classe modificabile **ScriptMain** viene visualizzata nell'editor del codice con uno stub per il metodo **ProcessInputRow**. La classe **ScriptMain** è quella in cui si scriverà il codice personalizzato, mentre **ProcessInputRow** è il metodo più importante in un componente di trasformazione.  
  
 Se si apre la finestra **Esplora progetti** in VSTA, è possibile rilevare che il componente script ha generato anche gli elementi del progetto **BufferWrapper** e **ComponentWrapper** di sola lettura. La classe **ScriptMain** eredita dalla classe **UserComponent** nell'elemento di progetto **ComponentWrapper**.  
  
 In fase di esecuzione il motore flusso di dati chiama il metodo **ProcessInput** nella classe **UserComponent**, che esegue l'override del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> della classe padre <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. A sua volta il metodo **ProcessInput** esegue il ciclo delle righe nel buffer di input e chiama il metodo **ProcessInputRow** una volta per ogni riga.  
  
### <a name="writing-your-custom-code"></a>Scrittura di codice personalizzato  
 Un componente di trasformazione con output sincroni è il più semplice da scrivere tra tutti i componenti del flusso di dati. L'esempio a singolo output illustrato più avanti in questo argomento è costituito, ad esempio, dal codice personalizzato seguente:  
  
```vb  
Row.City = UCase(Row.City)  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
```  
  
 Per completare la creazione di un componente di trasformazione sincrono, è possibile usare il metodo sottoposto a override **ProcessInputRow** per trasformare i dati in ogni riga del buffer di input. Il motore flusso di dati passa questo buffer, quando è pieno, al componente successivo nel flusso di dati.  
  
 A seconda dei requisiti, è anche possibile creare script nei metodi **PreExecute** e **PostExecute**, disponibili nella classe **ScriptMain**, per eseguire l'elaborazione preliminare o finale.  
  
### <a name="working-with-multiple-outputs"></a>Utilizzo di più output  
 L'indirizzamento delle righe di input a uno dei due o più output possibili non richiede una quantità maggiore di codice personalizzato rispetto allo scenario a singolo output descritto in precedenza. L'esempio a due output illustrato più avanti in questo argomento è costituito, ad esempio, dal codice personalizzato seguente:  
  
```vb  
Row.City = UCase(Row.City)  
If Row.City = "REDMOND" Then  
    Row.DirectRowToMyRedmondAddresses()  
Else  
    Row.DirectRowToMyOtherAddresses()  
End If  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
if (Row.City=="REDMOND")  
{  
    Row.DirectRowToMyRedmondAddresses();  
}  
else  
{  
    Row.DirectRowToMyOtherAddresses();  
}  
```  
  
 In questo esempio il componente script genera automaticamente i metodi **DirectRowTo\<OutputBufferX>** , in base ai nomi degli output configurati. È possibile utilizzare codice simile per indirizzare le righe di errore a un output degli errori simulati.  
  
## <a name="examples"></a>Esempi  
 Negli esempi seguenti è illustrato il codice personalizzato necessario nella classe **ScriptMain** per creare un componente di trasformazione sincrono.  
  
> [!NOTE]  
>  Gli esempi usano la tabella **Person.Address** del database di esempio **AdventureWorks** e passano la prima e la quarta colonna, ovvero le colonne **intAddressID** e **nvarchar(30)City**, attraverso il flusso di dati. Gli stessi dati vengono utilizzati negli esempi relativi a origine, trasformazione e destinazione in questa sezione. Per ogni esempio, sono documentati ulteriori prerequisiti e presupposti.  
  
### <a name="single-output-synchronous-transformation-example"></a>Esempio di trasformazione sincrona a singolo output  
 In questo esempio viene illustrato un componente di trasformazione sincrono con un singolo output. La trasformazione passa la colonna **AddressID** e converte la colonna **City** in lettere maiuscole.  
  
 Se si desidera eseguire questo codice di esempio, è necessario configurare il pacchetto e il componente come segue:  
  
1.  Aggiungere un nuovo componente script all'area di progettazione del flusso di dati e configurarlo come trasformazione.  
  
2.  Connettere l'output di un'origine o di un'altra trasformazione al nuovo componente di trasformazione in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Questo output deve specificare i dati della tabella **Person.Address** del database di esempio **AdventureWorks** che contiene le colonne **AddressID** e **City**.  
  
3.  Aprire l'**Editor trasformazione Script**. Nella pagina **Colonne di input** selezionare le colonne **AddressID** e **City**. Contrassegnare la colonna **City** come Lettura/Scrittura.  
  
4.  Nella pagina **Input e output** rinominare l'input e l'output con nomi più descrittivi, ad esempio **MyAddressInput** e **MyAddressOutput**. Si noti che l'elemento **SynchronousInputID** dell'output corrisponde all'elemento **ID** dell'input. Pertanto, non è necessario aggiungere e configurare colonne di output.  
  
5.  Nella pagina **Script** fare clic su **Modifica script** e immettere lo script seguente. Chiudere quindi l'ambiente di sviluppo dello script e **Editor trasformazione Script**.  
  
6.  Creare e configurare un componente di destinazione in cui sono previste le colonne **AddressID** e **City**, ad esempio una destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], oppure il componente di destinazione di esempio illustrato in [Creazione di una destinazione con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Connettere quindi l'output della trasformazione al componente di destinazione. È possibile creare una tabella di destinazione eseguendo il comando [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente nel database **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Eseguire l'esempio.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
    }  
  
}  
```  
  
### <a name="two-output-synchronous-transformation-example"></a>Esempio di trasformazione sincrona a due output  
 In questo esempio viene illustrato un componente di trasformazione sincrona con due output. La trasformazione passa la colonna **AddressID** e converte la colonna **City** in lettere maiuscole. Se il nome della città è Redmond, indirizza la riga a un unico output e tutte le altre righe a un altro output.  
  
 Se si desidera eseguire questo codice di esempio, è necessario configurare il pacchetto e il componente come segue:  
  
1.  Aggiungere un nuovo componente script all'area di progettazione del flusso di dati e configurarlo come trasformazione.  
  
2.  Connettere l'output di un'origine o di un'altra trasformazione al nuovo componente di trasformazione in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Questo output deve specificare dati della tabella **Person.Address** del database di esempio **AdventureWorks** che contiene almeno le colonne **AddressID** e **City**.  
  
3.  Aprire l'**Editor trasformazione Script**. Nella pagina **Colonne di input** selezionare le colonne **AddressID** e **City**. Contrassegnare la colonna **City** come Lettura/Scrittura.  
  
4.  Nella pagina **Input e output** creare un secondo output. Dopo aver aggiunto il nuovo output, assicurarsi di impostare il relativo elemento **SynchronousInputID** sull'elemento **ID** dell'input. Questa proprietà è già impostata sul primo output, creato per impostazione predefinita. Per ogni output, impostare la proprietà **ExclusionGroup** sullo stesso valore diverso da zero per indicare che le righe di input verranno suddivise tra due output che si escludono a vicenda. Non è necessario aggiungere colonne di output agli output.  
  
5.  Rinominare l'input e gli output con nomi più descrittivi, ad esempio **MyAddressInput**, **MyRedmondAddresses** e **MyOtherAddresses**.  
  
6.  Nella pagina **Script** fare clic su **Modifica script** e immettere lo script seguente. Chiudere quindi l'ambiente di sviluppo dello script e **Editor trasformazione Script**.  
  
7.  Creare e configurare due componenti di destinazione in cui sono previste le colonne **AddressID** e **City**, ad esempio una destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], una destinazione file flat o il componente di destinazione di esempio illustrato in [Creazione di una destinazione con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Connettere quindi ogni output della trasformazione a uno dei componenti di destinazione. Per creare tabelle di destinazione, è possibile eseguire un comando [!INCLUDE[tsql](../../includes/tsql-md.md)] simile al seguente (con nomi di tabella univoci) nel database **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2](  
        [AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL  
    ```  
  
8.  Eseguire l'esempio.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
        If Row.City = "REDMOND" Then  
            Row.DirectRowToMyRedmondAddresses()  
        Else  
            Row.DirectRowToMyOtherAddresses()  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
        if (Row.City == "REDMOND")  
        {  
            Row.DirectRowToMyRedmondAddresses();  
        }  
        else  
        {  
            Row.DirectRowToMyOtherAddresses();  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sulle trasformazioni sincrone e asincrone](~/integration-services/understanding-synchronous-and-asynchronous-transformations.md)  
 [Creazione di una trasformazione asincrona con il componente script](~/integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
 [Sviluppo di un componente di trasformazione personalizzato con output sincroni](~/integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)
