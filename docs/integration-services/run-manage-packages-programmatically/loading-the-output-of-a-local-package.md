---
description: Caricamento dell'output di un pacchetto locale
title: Caricamento dell'output di un pacchetto locale | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow [Integration Services], loading results
- loading data flow results
ms.assetid: aba8ecb7-0dcf-40d0-a2a8-64da0da94b93
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f53a5bc1707e8f806d766a611b7792ab056d6f54
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495558"
---
# <a name="loading-the-output-of-a-local-package"></a>Caricamento dell'output di un pacchetto locale

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le applicazioni client possono leggere l'output dei pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] quando viene salvato nelle destinazioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[vstecado](../../includes/vstecado-md.md)] o quando viene salvato in una destinazione file flat usando le classi dello spazio dei nomi **System.IO**. Tuttavia, un'applicazione client può anche leggere l'output di un pacchetto direttamente dalla memoria, senza la necessità di un passaggio intermedio per rendere persistenti i dati. La chiave per questa soluzione è lo spazio dei nomi **Microsoft.SqlServer.Dts.DtsClient**, che contiene implementazioni speciali delle interfacce **IDbConnection**, **IDbCommand**, e **IDbDataParameter** dello spazio dei nomi **System. Data**. L'assembly Microsoft.SqlServer.Dts.DtsClient.dll è installato per impostazione predefinita in **%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn**.  

> [!IMPORTANT]
> La procedura descritta in questo articolo, che usa la libreria `DtsClient`, funziona solo per i pacchetti distribuiti con il modello di distribuzione del pacchetto (vale a dire, con l'opzione `/SQL`, `/DTS`, o `/File`). Questa procedura non funziona per i pacchetti distribuiti con il modello di distribuzione server (vale a dire, con l'opzione `/ISServer`). Per usare l'output di un pacchetto locale distribuito con il modello di distribuzione server (vale a dire, con l'opzione `/ISServer`), usare la [Destinazione flusso di dati](../data-flow/data-streaming-destination.md) invece della procedura descritta in questo articolo.

> [!NOTE]  
> Per la procedura descritta in questo argomento, è necessario che la proprietà DelayValidation dell'attività Flusso di dati e di eventuali oggetti padre sia impostata sul valore predefinito, ovvero **False**.
  
## <a name="description"></a>Descrizione  
 In questa procedura viene illustrato lo sviluppo di un'applicazione client in codice gestito che carica l'output di un pacchetto con una destinazione DataReader direttamente dalla memoria. I passaggi riepilogati in questa sezione sono illustrati nel codice di esempio seguente.  
  
#### <a name="to-load-data-package-output-into-a-client-application"></a>Per caricare l'output del pacchetto di dati in un'applicazione client  
  
1.  Nel pacchetto configurare una destinazione DataReader in modo da ricevere l'output che si desidera leggere nell'applicazione client. Assegnare alla destinazione DataReader un nome descrittivo, che verrà utilizzato più avanti nell'applicazione client. Prendere nota di tale nome.  
  
2.  Nel progetto di sviluppo, impostare un riferimento allo spazio dei nomi **Microsoft.SqlServer.Dts.DtsClient** per l'individuazione dell'assembly **Microsoft.SqlServer.Dts.DtsClient.dll**. Per impostazione predefinita, questo assembly è installato in **C:\Program Files\Microsoft SQL Server\100\DTS\Binn**. Importare lo spazio dei nomi nel codice usando l'istruzione C# **Using** o [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] **Imports**.  
  
3.  Nel codice creare un oggetto di tipo **DtsClient.DtsConnection** con una stringa di connessione che contiene i parametri della riga di comando richiesti da **dtexec.exe** per eseguire il pacchetto. Per altre informazioni, vedere [dtexec Utility](../../integration-services/packages/dtexec-utility.md). Aprire la connessione con questa stringa di connessione. È anche possibile usare l'utilità **dtexecui** per creare visivamente la stringa di connessione richiesta.  
  
    > [!NOTE]  
    >  Nel codice di esempio è illustrato il caricamento del pacchetto dal file system tramite la sintassi `/FILE <path and filename>`. Tuttavia, è anche possibile caricare il pacchetto dal database MSDB utilizzando la sintassi `/SQL <package name>` o dall'archivio pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] tramite la sintassi `/DTS \<folder name>\<package name>`.  
  
4.  Creare un oggetto di tipo **DtsClient.DtsCommand** che usi l'oggetto **DtsConnection** creato in precedenza e impostare la relativa proprietà **CommandText** sul nome della destinazione DataReader nel pacchetto. Chiamare quindi il metodo **ExecuteReader** dell'oggetto comando per caricare i risultati del pacchetto in un nuovo DataReader.  
  
5.  Facoltativamente, è possibile parametrizzare indirettamente l'output del pacchetto usando la raccolta di oggetti **DtsDataParameter** nell'oggetto **DtsCommand** per passare i valori alle variabili definite nel pacchetto. All'interno del pacchetto è possibile utilizzare queste variabili come parametri di query o in espressioni per influire sui risultati restituiti alla destinazione DataReader. È necessario definire queste variabili nel pacchetto nello spazio dei nomi **DtsClient** affinché sia possibile usarli con l'oggetto **DtsDataParameter** da un'applicazione client. Può essere necessario fare clic sul pulsante della barra degli strumenti **Selezione colonne finestra Variabili** nella finestra **Variabili** per visualizzare la colonna **Spazio dei nomi**. Nel codice client, quando si aggiunge un oggetto **DtsDataParameter** alla raccolta **Parametri** di **DtsCommand** omettere il riferimento allo spazio dei nomi DtsClient dal nome della variabile. Ad esempio:  
  
    ```  
    command.Parameters.Add(new DtsDataParameter("MyVariable", 1));  
    ```  
  
6.  Chiamare il metodo **Read** del DataReader più volte secondo necessità per eseguire il ciclo delle righe di dati di output. Utilizzare i dati o salvarli per un utilizzo successivo nell'applicazione client.  
  
    > [!IMPORTANT]  
    >  Il metodo **Read** di questa implementazione del DataReader restituisce **true** ancora una volta dopo la lettura dell'ultima riga di dati. Per questo motivo risulta difficile usare il solito codice che esegue il ciclo del DataReader mentre **Read** restituisce **true**. Se il codice tenta di chiudere il DataReader o la connessione dopo la lettura del numero previsto di righe, senza una chiamata finale aggiuntiva al metodo **Read**, il codice genererà un'eccezione non gestita. Se tuttavia il codice tenta di leggere i dati in questa iterazione finale di un ciclo, quando **Read** restituisce ancora **true** ma l'ultima riga è stata passata, il codice genererà un'eccezione **ApplicationException** non gestita con il messaggio "L'interfaccia IDataReader SSIS è oltre la fine del risultato". Questo comportamento è diverso da quello di altre implementazioni di DataReader. Pertanto, quando si usa un ciclo per leggere le righe nel DataReader mentre **Read** restituisce **true**, è necessario scrivere il codice per individuare, testare ed eliminare questo oggetto **ApplicationException** anticipato nell'ultima chiamata riuscita al metodo **Read**. In alternativa, se si conosce in anticipo il numero di righe previste, è possibile elaborare le righe, quindi chiamare ancora una volta il metodo **Read** prima di chiudere il DataReader e la connessione.  
  
7.  Chiamare il metodo **Dispose** dell'oggetto **DtsCommand**. Questo passaggio è particolarmente importante se si usano oggetti **DtsDataParameter**.  
  
8.  Chiudere il DataReader e gli oggetti connessione.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene eseguito un pacchetto che calcola un singolo valore di aggregazione e lo salva in una destinazione DataReader, quindi legge questo valore dal DataReader e lo visualizza in una casella di testo in un Windows Form.  
  
 L'utilizzo di parametri non è necessario quando si carica l'output di un pacchetto in un'applicazione client. Se non si vuole usare un parametro, è possibile evitare la variabile nello spazio dei nomi **DtsClient** e omettere il codice che usa l'oggetto **DtsDataParameter**.  
  
#### <a name="to-create-the-test-package"></a>Per creare un pacchetto di test  
  
1.  Creare un nuovo pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Nel codice di esempio viene utilizzato "DtsClientWParamPkg.dtsx" come nome del pacchetto.  
  
2.  Aggiungere una variabile di tipo String nello spazio dei nomi DtsClient. Nell'esempio di codice viene utilizzato Country come nome della variabile. Può essere necessario fare clic sul pulsante della barra degli strumenti **Selezione colonne finestra Variabili** nella finestra **Variabili** per visualizzare la colonna **Spazio dei nomi**.  
  
3.  Aggiungere una gestione connessione OLE DB che si connette al database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
4.  Aggiungere un'attività Flusso di dati al pacchetto e passare all'area di progettazione Flusso di dati.  
  
5.  Aggiungere un'origine OLE DB al flusso di dati e configurarla per l'utilizzo della gestione connessione OLE DB creata in precedenza, oltre al comando SQL seguente.  
  
    ```  
    SELECT * FROM Sales.vIndividualCustomer WHERE CountryRegionName = ?  
    ```  
  
6.  Fare clic su **Parametri** e, nella finestra di dialogo **Imposta parametri query**, eseguire il mapping dell'unico parametro di input nella query, Parameter0, alla variabile DtsClient::Country.  
  
7.  Aggiungere una trasformazione Aggregazione al flusso di dati, quindi connettere l'output dell'origine OLE DB alla trasformazione. Aprire Editor trasformazione Aggregazione e configurarlo per eseguire un'operazione "COUNT ALL" su tutte le colonne di input (*) e per restituire come output il valore aggregato con l'alias CustomerCount.  
  
8.  Aggiungere una destinazione DataReader al flusso di dati e connettere l'output della trasformazione Aggregazione alla destinazione DataReader. Nel codice di esempio viene utilizzato "DataReaderDest" come nome del DataReader. Selezionare l'unica colonna di input disponibile, CustomerCount, per la destinazione.  
  
9. Salvare il pacchetto. L'applicazione di test creata di seguito eseguirà il pacchetto e recupererà il relativo output direttamente dalla memoria.  
  
#### <a name="to-create-the-test-application"></a>Per creare l'applicazione di test  
  
1.  Creare una nuova applicazione Windows Form.  
  
2.  Aggiungere un riferimento allo spazio dei nomi **Microsoft.SqlServer.Dts.DtsClient** passando all'assembly con lo stesso nome in **%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn**.  
  
3.  Copiare e incollare il codice di esempio seguente nel modulo di codice per il form.  
  
4.  Modificare il valore della variabile **dtexecArgs** secondo necessità in modo che contenga i parametri della riga di comando richiesti da **dtexec.exe** per eseguire il pacchetto. Il codice di esempio carica il pacchetto dal file system.  
  
5.  Modificare il valore della variabile **dataReaderName** secondo necessità in modo che contenga il nome della destinazione DataReader nel pacchetto.  
  
6.  Inserire un pulsante e una casella di testo nel form. Nel codice di esempio si usano **btnRun** come nome del pulsante e **txtResults** come nome della casella di testo.  
  
7.  Eseguire l'applicazione e fare clic sul pulsante. Dopo una breve pausa durante l'esecuzione del pacchetto, nella casella di testo del form dovrebbe essere visualizzato il valore di aggregazione calcolato dal pacchetto, ovvero il conteggio di clienti in Canada.  
  
### <a name="sample-code"></a>Codice di esempio  
  
```vb  
Imports System.Data  
Imports Microsoft.SqlServer.Dts.DtsClient  
  
Public Class Form1  
  
  Private Sub btnRun_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnRun.Click  
  
    Dim dtexecArgs As String  
    Dim dataReaderName As String  
    Dim countryName As String  
  
    Dim dtsConnection As DtsConnection  
    Dim dtsCommand As DtsCommand  
    Dim dtsDataReader As IDataReader  
    Dim dtsParameter As DtsDataParameter  
  
    Windows.Forms.Cursor.Current = Cursors.WaitCursor  
  
    dtexecArgs = "/FILE ""C:\...\DtsClientWParamPkg.dtsx"""  
    dataReaderName = "DataReaderDest"  
    countryName = "Canada"  
  
    dtsConnection = New DtsConnection()  
    With dtsConnection  
      .ConnectionString = dtexecArgs  
      .Open()  
    End With  
  
    dtsCommand = New DtsCommand(dtsConnection)  
    dtsCommand.CommandText = dataReaderName  
  
    dtsParameter = New DtsDataParameter("Country", DbType.String)  
    dtsParameter.Direction = ParameterDirection.Input  
    dtsCommand.Parameters.Add(dtsParameter)  
  
    dtsParameter.Value = countryName  
  
    dtsDataReader = dtsCommand.ExecuteReader(CommandBehavior.Default)  
  
    With dtsDataReader  
      .Read()  
      txtResults.Text = .GetInt32(0).ToString("N0")  
    End With  
  
    'After reaching the end of data rows,  
    ' call the Read method one more time.  
    Try  
      dtsDataReader.Read()  
    Catch ex As Exception  
      MessageBox.Show("Exception on final call to Read method:" & ControlChars.CrLf & _  
      ex.Message & ControlChars.CrLf & _  
      ex.InnerException.Message, "Exception on final call to Read method", _  
      MessageBoxButtons.OK, MessageBoxIcon.Error)  
    End Try  
  
    ' The following method is a best practice, and is  
    '  required when using DtsDataParameter objects.  
    dtsCommand.Dispose()  
  
    Try  
      dtsDataReader.Close()  
    Catch ex As Exception  
      MessageBox.Show("Exception closing DataReader:" & ControlChars.CrLf & _  
      ex.Message & ControlChars.CrLf & _  
      ex.InnerException.Message, "Exception closing DataReader", _  
      MessageBoxButtons.OK, MessageBoxIcon.Error)  
    End Try  
  
    Try  
      dtsConnection.Close()  
    Catch ex As Exception  
      MessageBox.Show("Exception closing connection:" & ControlChars.CrLf & _  
      ex.Message & ControlChars.CrLf & _  
      ex.InnerException.Message, "Exception closing connection", _  
      MessageBoxButtons.OK, MessageBoxIcon.Error)  
    End Try  
  
    Windows.Forms.Cursor.Current = Cursors.Default  
  
  End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Windows.Forms;  
using System.Data;  
using Microsoft.SqlServer.Dts.DtsClient;  
  
namespace DtsClientWParamCS  
{  
  public partial class Form1 : Form  
  {  
    public Form1()  
    {  
      InitializeComponent();  
      this.btnRun.Click += new System.EventHandler(this.btnRun_Click);  
    }  
  
    private void btnRun_Click(object sender, EventArgs e)  
    {  
      string dtexecArgs;  
      string dataReaderName;  
      string countryName;  
  
      DtsConnection dtsConnection;  
      DtsCommand dtsCommand;  
      IDataReader dtsDataReader;  
      DtsDataParameter dtsParameter;  
  
      Cursor.Current = Cursors.WaitCursor;  
  
      dtexecArgs = @"/FILE ""C:\...\DtsClientWParamPkg.dtsx""";  
      dataReaderName = "DataReaderDest";  
      countryName = "Canada";  
  
      dtsConnection = new DtsConnection();  
      {  
        dtsConnection.ConnectionString = dtexecArgs;  
        dtsConnection.Open();  
      }  
  
      dtsCommand = new DtsCommand(dtsConnection);  
      dtsCommand.CommandText = dataReaderName;  
  
      dtsParameter = new DtsDataParameter("Country", DbType.String);  
      dtsParameter.Direction = ParameterDirection.Input;  
      dtsCommand.Parameters.Add(dtsParameter);  
  
      dtsParameter.Value = countryName;  
  
      dtsDataReader = dtsCommand.ExecuteReader(CommandBehavior.Default);  
  
      {  
        dtsDataReader.Read();  
        txtResults.Text = dtsDataReader.GetInt32(0).ToString("N0");  
      }  
  
      //After reaching the end of data rows,  
      // call the Read method one more time.  
      try  
      {  
        dtsDataReader.Read();  
      }  
      catch (Exception ex)  
      {  
        MessageBox.Show(  
          "Exception on final call to Read method:\n" + ex.Message + "\n" + ex.InnerException.Message,  
          "Exception on final call to Read method", MessageBoxButtons.OK, MessageBoxIcon.Error);  
      }  
  
      // The following method is a best practice, and is  
      //  required when using DtsDataParameter objects.  
      dtsCommand.Dispose();  
  
      try  
      {  
        dtsDataReader.Close();  
      }  
      catch (Exception ex)  
      {  
        MessageBox.Show(  
          "Exception closing DataReader:\n" + ex.Message + "\n" + ex.InnerException.Message,  
          "Exception closing DataReader", MessageBoxButtons.OK, MessageBoxIcon.Error);  
      }  
  
      try  
      {  
        dtsConnection.Close();  
      }  
      catch (Exception ex)  
      {  
        MessageBox.Show(  
          "Exception closing connection:\n" + ex.Message + "\n" + ex.InnerException.Message,  
          "Exception closing connection", MessageBoxButtons.OK, MessageBoxIcon.Error);  
      }  
  
      Cursor.Current = Cursors.Default;  
  
    }  
  }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Differenze tra l'esecuzione locale e remota](../../integration-services/run-manage-packages-programmatically/understanding-the-differences-between-local-and-remote-execution.md)   
 [Caricamento ed esecuzione di un pacchetto locale a livello di programmazione](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-local-package-programmatically.md)   
 [Caricamento ed esecuzione di un pacchetto remoto a livello di programmazione](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-remote-package-programmatically.md)  
  
  
