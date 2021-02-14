---
description: Creazione di una destinazione ODBC con il componente script
title: Creazione di una destinazione ODBC con il componente script | Microsoft Docs
ms.custom: ''
ms.date: 10/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], destination components
- ODBC destination [Integration Services]
- destinations [Integration Services], components
- Script component [Integration Services], examples
ms.assetid: d198c866-78f4-4a50-ae15-333160645815
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 446a4ba272d80d13c8884d3a2a0eb143c44980f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344879"
---
# <a name="creating-an-odbc-destination-with-the-script-component"></a>Creazione di una destinazione ODBC con il componente script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] i dati vengono in genere salvati in una destinazione ODBC tramite una destinazione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] e il provider di dati [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] per ODBC. È possibile, tuttavia, creare anche una destinazione ODBC ad hoc da utilizzare in un solo pacchetto. Per creare questa destinazione ODBC ad hoc, si utilizza il componente script come illustrato nell'esempio seguente.  
  
> [!NOTE]  
>  Se si desidera creare un componente da riutilizzare più facilmente con più attività Flusso di dati e più pacchetti, è possibile utilizzare il codice di questo esempio di componente script come punto iniziale per un componente del flusso di dati personalizzato. Per altre informazioni, vedere [Sviluppo di un componente del flusso di dati personalizzato](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra come creare un componente di destinazione che usa una gestione connessione ODBC esistente per salvare i dati del flusso di dati in una tabella di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Questo esempio è una versione modificata della destinazione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] personalizzata visualizzata nell'argomento [Creazione di una destinazione con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). In questo esempio, tuttavia, la destinazione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] personalizzata è stata modificata per l'utilizzo di una gestione connessione ODBC e il salvataggio dei dati in una destinazione ODBC. Queste modifiche includono anche le seguenti:  
  
-   Non è possibile chiamare il metodo **AcquireConnection** della gestione connessione ODBC dal codice gestito, perché restituisce un oggetto nativo. In questo esempio viene pertanto utilizzata la stringa di connessione della gestione connessione per connettersi direttamente all'origine dati tramite il provider di dati [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] ODBC gestito.  
  
-   Per l'elemento **OdbcCommand** sono previsti parametri posizionali. Le posizioni dei parametri sono indicate dai punti interrogativi (?) nel testo del comando. Al contrario, per un elemento **SqlCommand** sono previsti parametri denominati.  
  
 In questo esempio viene usata la tabella **Person.Address** del database di esempio **AdventureWorks**. Nell'esempio vengono passate la prima e la quarta colonna, ovvero le colonne **int _AddressID_** e **nvarchar(30) _City_** di questa tabella attraverso il flusso di dati. Questi stessi dati vengono usati negli esempi di origine, trasformazione e destinazione dell'argomento [Sviluppo di tipi specifici di componenti script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md).  
  
#### <a name="to-configure-this-script-component-example"></a>Per configurare l'esempio di componente script  
  
1.  Creare una gestione connessione ODBC che si connette al database **AdventureWorks**.  
  
2.  Creare una tabella di destinazione eseguendo il comando Transact-SQL seguente nel database **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
3.  Aggiungere un nuovo componente script all'area di progettazione del flusso di dati e configurarlo come destinazione.  
  
4.  Connettere l'output di un'origine o di una trasformazione a monte al componente di destinazione in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)]. È possibile connettere direttamente un'origine a una destinazione senza alcuna trasformazione. Per assicurarsi che questo esempio funzioni, l'output del componente a monte deve includere almeno le colonne **AddressID** e **City** della tabella **Person.Address** del database di esempio **AdventureWorks**.  
  
5.  Aprire l'**Editor trasformazione Script**. Nella pagina **Colonne di input** selezionare le colonne **AddressID** e **City**.  
  
6.  Nella pagina **Input e output** rinominare l'input con un nome più descrittivo, ad esempio **MyAddressInput**.  
  
7.  Nella pagina **Gestioni connessioni** aggiungere o creare la gestione connessione ODBC con un nome descrittivo, ad esempio **MyODBCConnectionManager**.  
  
8.  Nella pagina **Script** fare clic su **Modifica script**, quindi immettere lo script seguente nella classe **ScriptMain**.  
  
9. Chiudere l'ambiente di sviluppo dello script e **Editor trasformazione Script**, quindi eseguire l'esempio.  
  
    ```vb  
    Imports System.Data.Odbc  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim odbcConn As OdbcConnection  
        Dim odbcCmd As OdbcCommand  
        Dim odbcParam As OdbcParameter  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connectionString As String  
            connectionString = Me.Connections.MyODBCConnectionManager.ConnectionString  
            odbcConn = New OdbcConnection(connectionString)  
            odbcConn.Open()  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            odbcCmd = New OdbcCommand("INSERT INTO Person.Address2(AddressID, City) " & _  
                "VALUES(?, ?)", odbcConn)  
            odbcParam = New OdbcParameter("@addressid", OdbcType.Int)  
            odbcCmd.Parameters.Add(odbcParam)  
            odbcParam = New OdbcParameter("@city", OdbcType.NVarChar, 30)  
            odbcCmd.Parameters.Add(odbcParam)  
  
        End Sub  
  
        Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
            With odbcCmd  
                .Parameters("@addressid").Value = Row.AddressID  
                .Parameters("@city").Value = Row.City  
                .ExecuteNonQuery()  
            End With  
  
        End Sub  
  
        Public Overrides Sub ReleaseConnections()  
  
            odbcConn.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.Data.Odbc;  
    ...  
    public class ScriptMain :  
        UserComponent  
    {  
        OdbcConnection odbcConn;  
        OdbcCommand odbcCmd;  
        OdbcParameter odbcParam;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            string connectionString;  
            connectionString = this.Connections.MyODBCConnectionManager.ConnectionString;  
            odbcConn = new OdbcConnection(connectionString);  
            odbcConn.Open();  
  
        }  
  
        public override void PreExecute()  
        {  
  
            odbcCmd = new OdbcCommand("INSERT INTO Person.Address2(AddressID, City) " +  
                "VALUES(?, ?)", odbcConn);  
            odbcParam = new OdbcParameter("@addressid", OdbcType.Int);  
            odbcCmd.Parameters.Add(odbcParam);  
            odbcParam = new OdbcParameter("@city", OdbcType.NVarChar, 30);  
            odbcCmd.Parameters.Add(odbcParam);  
  
        }  
  
        public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
        {  
  
            {  
                odbcCmd.Parameters["@addressid"].Value = Row.AddressID;  
                odbcCmd.Parameters["@city"].Value = Row.City;  
                odbcCmd.ExecuteNonQuery();  
            }  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            odbcConn.Close();  
  
        }  
    }  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di una destinazione con il componente script](../extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)  
  
  
