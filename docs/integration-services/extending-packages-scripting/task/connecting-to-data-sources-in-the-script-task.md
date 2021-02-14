---
description: Connessione a origini dati nell'attività Script
title: Connessione a origini dati nell'attività Script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- connections [Integration Services], scripts
- Integration Services packages, connections
- connection managers [Integration Services], scripts
- scripts [Integration Services], connections
- SSIS packages, connections
- packages [Integration Services], connections
- Script task [Integration Services], connections
- Connections property
- SQL Server Integration Services packages, connections
- SSIS Script task, connections
ms.assetid: 9c008380-715b-455b-9da7-22572d67c388
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2fca697da29c3ca4d914a5d4a7abd848b4eca966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351577"
---
# <a name="connecting-to-data-sources-in-the-script-task"></a>Connessione a origini dati nell'attività Script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Le gestioni connessioni forniscono accesso a origini dati configurate nel pacchetto. Per altre informazioni, vedere [Connessioni in Integration Services &#40;SSIS&#41;](../../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
 L'attività Script può accedere a queste gestioni connessioni tramite la proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A> dell'oggetto **Dts**. Ogni gestione connessione nella raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Connections> archivia informazioni su come eseguire la connessione all'origine dati sottostante.  
  
 Quando si chiama il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> di una gestione connessione, la gestione connessione si connette all'origine dati, se non è già connessa, e restituisce la connessione o le informazioni di connessione appropriate da utilizzare nel codice dell'attività Script.  
  
> [!NOTE]  
>  È necessario conoscere il tipo di connessione restituito dalla gestione connessione prima di chiamare **AcquireConnection**. Poiché l'oggetto **Option Strict** dell'attività Script è abilitato, è necessario eseguire il cast della connessione, che viene restituita come tipo **Object**, nel tipo di connessione appropriato prima dell'uso.  
  
 È possibile utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Contains%2A> della raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Connections> restituita dalla proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A> per cercare una connessione esistente prima di utilizzare la connessione nel codice.  
  
> [!IMPORTANT]  
>  Non è possibile chiamare il metodo AcquireConnection delle gestioni connessioni che restituiscono oggetti non gestiti, ad esempio la gestione connessione OLE DB e la gestione connessione Excel, nel codice gestito di un'attività Script. È tuttavia possibile leggere la proprietà ConnectionString di queste gestioni connessioni e connettersi direttamente all'origine dati nel codice usando la stringa di connessione con **OledbConnection** dallo spazio dei nomi **System.Data.OleDb**.  
>   
>  Se è necessario chiamare il metodo AcquireConnection di una gestione connessione che restituisce un oggetto non gestito, usare una gestione connessione [!INCLUDE[vstecado](../../../includes/vstecado-md.md)]. Quando si configura la gestione connessione [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] per l'utilizzo di un provider OLE DB, la connessione viene eseguita tramite il provider di dati [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] per OLE DB. In questo caso il metodo AcquireConnection restituisce un oggetto **System.Data.OleDb.OleDbConnection** invece di un oggetto non gestito. Per configurare una gestione connessione [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] per l'uso con un'origine dati Excel, selezionare il provider OLE DB [!INCLUDE[msCoName](../../../includes/msconame-md.md)] per Jet, specificare un file di Excel e immettere `Excel 8.0` (per Excel 97 e versioni successive) come valore di **Proprietà estese** nella pagina **Tutte** della finestra di dialogo **Gestione connessione**.  
  
## <a name="connections-example"></a>Esempio di connessioni  
 Nell'esempio seguente viene illustrato come accedere alle gestioni connessioni dall'attività Script. Nell'esempio si presuppone che sia sta creata e configurata una gestione connessione [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] denominata **Test ADO.NET Connection** e una gestione connessione file flat denominata **Test Flat File Connection**. Si noti che la gestione connessione [!INCLUDE[vstecado](../../../includes/vstecado-md.md)] restituisce un oggetto **SqlConnection** che è possibile usare immediatamente per connettersi all'origine dati. La gestione connessione file flat, al contrario, restituisce solo una stringa che contiene il percorso e il nome di file. È necessario usare i metodi dello spazio dei nomi **System.IO** per aprire e usare il file flat.  
  
```vb  
    Public Sub Main()

        Dim myADONETConnection As SqlClient.SqlConnection =
            DirectCast(Dts.Connections("Test ADO.NET Connection").AcquireConnection(Dts.Transaction),
                SqlClient.SqlConnection)
        MsgBox(myADONETConnection.ConnectionString,
            MsgBoxStyle.Information, "ADO.NET Connection")

        Dim myFlatFileConnection As String =
            DirectCast(Dts.Connections("Test Flat File Connection").AcquireConnection(Dts.Transaction),
                String)
        MsgBox(myFlatFileConnection, MsgBoxStyle.Information, "Flat File Connection")

        Dts.TaskResult = ScriptResults.Success

    End Sub
```  
  
```csharp  
        public void Main()
        {
            SqlConnection myADONETConnection = 
                Dts.Connections["Test ADO.NET Connection"].AcquireConnection(Dts.Transaction)
                as SqlConnection;
            MessageBox.Show(myADONETConnection.ConnectionString, "ADO.NET Connection");

            string myFlatFileConnection = 
                Dts.Connections["Test Flat File Connection"].AcquireConnection(Dts.Transaction) 
                as string;
            MessageBox.Show(myFlatFileConnection, "Flat File Connection");

            Dts.TaskResult = (int)ScriptResults.Success;
        }
```  
  
## <a name="see-also"></a>Vedere anche  
 [Connessioni in Integration Services &#40;SSIS&#41;](../../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Creare gestioni connessioni](../../connection-manager/integration-services-ssis-connections.md)  
  
