---
description: Implementazione della ricerca full-text
title: Implementazione della ricerca full-text
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- full-text search [SMO]
ms.assetid: 9ce9ad9c-f671-4760-90b5-e0c8ca051473
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 828b677210b00dae3a09d4f73b67fb73f0470edd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352500"
---
# <a name="implementing-full-text-search"></a>Implementazione della ricerca full-text
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  La ricerca full-text è disponibile per istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ed è rappresentata in SMO dall'oggetto <xref:Microsoft.SqlServer.Management.Smo.Server.FullTextService%2A>. L' <xref:Microsoft.SqlServer.Management.Smo.FullTextService> oggetto risiede nell'oggetto **Server** . Viene utilizzato per gestire le opzioni di configurazione per il servizio di ricerca full-text di [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. L'oggetto <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalogCollection> appartiene all'oggetto <xref:Microsoft.SqlServer.Management.Smo.Database> e si tratta di una raccolta di oggetti <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalog> che rappresentano cataloghi full-text definiti per il database. È possibile disporre di un solo indice full-text definito per ogni tabella, a differenza degli indici normali. Questo indice è rappresentato da un oggetto <xref:Microsoft.SqlServer.Management.Smo.FullTextIndexColumn> nell'oggetto <xref:Microsoft.SqlServer.Management.Smo.Table>.  
  
 Per creare un servizio di ricerca full-text, è necessario disporre di un catalogo full-text definito nel database e di un indice di ricerca full-text definito in una delle tabelle del database.  
  
 Creare innanzitutto un catalogo full-text nel database chiamando il costruttore <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalog> e specificando il nome del catalogo. Successivamente, creare l'indice full-text chiamando il costruttore e specificando la tabella nella quale deve essere creato. È quindi possibile aggiungere colonne per l'indice full-text tramite l'oggetto <xref:Microsoft.SqlServer.Management.Smo.FullTextIndexColumn> e fornendo il nome della colonna all'interno della tabella. Successivamente, impostare la proprietà <xref:Microsoft.SqlServer.Management.Smo.FullTextIndex.CatalogName%2A> sul catalogo creato. Infine, chiamare il metodo <xref:Microsoft.SqlServer.Management.Smo.FullTextIndex.Create%2A> e creare l'indice full-text nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a>Esempio  
 Per usare qualsiasi esempio di codice fornito, è necessario scegliere l'ambiente di programmazione, il modello di programmazione e il linguaggio di programmazione per la creazione dell'applicazione. Per altre informazioni, vedere [creare un progetto Visual C&#35; SMO in Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-a-full-text-search-service-in-visual-basic"></a>Creazione di un servizio di ricerca full-text in Visual Basic  
 Nel codice di esempio seguente viene creato un catalogo di ricerca full-text per la tabella `ProductCategory` nel database di esempio [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)]. Successivamente, viene creato un indice di ricerca full-text nella colonna Name della tabella `ProductCategory`. Per l'indice di ricerca full-text è necessario che nella colonna sia già definito un indice univoco.  
  
```  
' compile with:   
' /r:Microsoft.SqlServer.SqlEnum.dll   
' /r:Microsoft.SqlServer.Smo.dll   
' /r:Microsoft.SqlServer.ConnectionInfo.dll   
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Sdk.Sfc  
Imports Microsoft.SqlServer.Management.Common  
  
Public Class A  
   Public Shared Sub Main()  
      ' Connect to the local, default instance of SQL Server.  
      Dim srv As Server = Nothing  
      srv = New Server()  
  
      ' Reference the AdventureWorks database.  
      Dim db As Database = Nothing  
      db = srv.Databases("AdventureWorks")  
  
      ' Reference the ProductCategory table.  
      Dim tb As Table = Nothing  
      tb = db.Tables("ProductCategory", "Production")  
  
      ' Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
      Dim ftc As FullTextCatalog = Nothing  
      ftc = New FullTextCatalog(db, "Test_Catalog")  
      ftc.IsDefault = True  
  
      ' Create the Full-Text Search catalog on the instance of SQL Server.  
      ftc.Create()  
  
      ' Define a FullTextIndex object varaible by supplying the parent table argument in the constructor.  
      Dim fti As FullTextIndex = Nothing  
      fti = New FullTextIndex(tb)  
  
      ' Define a FullTextIndexColumn object variable by supplying the parent index and column name arguments in the constructor.  
      Dim ftic As FullTextIndexColumn = Nothing  
      ftic = New FullTextIndexColumn(fti, "Name")  
  
      ' Add the indexed column to the index.  
      fti.IndexedColumns.Add(ftic)  
      fti.ChangeTracking = ChangeTracking.Automatic  
  
      ' Specify the unique index on the table that is required by the Full Text Search index.  
      fti.UniqueIndexName = "AK_ProductCategory_Name"  
  
      ' Specify the catalog associated with the index.  
      fti.CatalogName = "Test_Catalog"  
  
      ' Create the Full Text Search index on the instance of SQL Server.  
      fti.Create()  
   End Sub  
End Class  
```  
  
## <a name="creating-a-full-text-search-service-in-visual-c"></a>Creazione di un servizio di ricerca full-text in Visual C#  
 Nel codice di esempio seguente viene creato un catalogo di ricerca full-text per la tabella `ProductCategory` nel database di esempio [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)]. Successivamente, viene creato un indice di ricerca full-text nella colonna Name della tabella `ProductCategory`. Per l'indice di ricerca full-text è necessario che nella colonna sia già definito un indice univoco.  
  
```  
// compile with:   
// /r:Microsoft.SqlServer.SqlEnum.dll   
// /r:Microsoft.SqlServer.Smo.dll   
// /r:Microsoft.SqlServer.ConnectionInfo.dll   
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Sdk.Sfc;  
using Microsoft.SqlServer.Management.Common;  
  
public class A {  
   public static void Main() {  
      // Connect to the local, default instance of SQL Server.  
      Server srv = default(Server);  
      srv = new Server();  
  
      // Reference the AdventureWorks database.  
      Database db = default(Database);  
      db = srv.Databases ["AdventureWorks"];  
  
      // Reference the ProductCategory table.  
      Table tb = default(Table);  
      tb = db.Tables["ProductCategory", "Production"];  
  
      // Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
      FullTextCatalog ftc = default(FullTextCatalog);  
      ftc = new FullTextCatalog(db, "Test_Catalog");  
      ftc.IsDefault = true;  
  
      // Create the Full-Text Search catalog on the instance of SQL Server.  
      ftc.Create();  
  
      // Define a FullTextIndex object varaible by supplying the parent table argument in the constructor.  
      FullTextIndex fti = default(FullTextIndex);  
      fti = new FullTextIndex(tb);  
  
      // Define a FullTextIndexColumn object variable by supplying the parent index and column name arguments in the constructor.  
      FullTextIndexColumn ftic = default(FullTextIndexColumn);  
      ftic = new FullTextIndexColumn(fti, "Name");  
  
      // Add the indexed column to the index.  
      fti.IndexedColumns.Add(ftic);  
      fti.ChangeTracking = ChangeTracking.Automatic;  
  
      // Specify the unique index on the table that is required by the Full Text Search index.  
      fti.UniqueIndexName = "AK_ProductCategory_Name";  
  
      // Specify the catalog associated with the index.  
      fti.CatalogName = "Test_Catalog";  
  
      // Create the Full Text Search index on the instance of SQL Server.  
      fti.Create();  
   }  
}  
```  
  
## <a name="creating-a-full-text-search-service-in-powershell"></a>Creazione di un servizio di ricerca full-text in PowerShell  
 Nel codice di esempio seguente viene creato un catalogo di ricerca full-text per la tabella `ProductCategory` nel database di esempio [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)]. Successivamente, viene creato un indice di ricerca full-text nella colonna Name della tabella `ProductCategory`. Per l'indice di ricerca full-text è necessario che nella colonna sia già definito un indice univoco.  
  
```  
# Example of implementing a full text search on the default instance.  
# Set the path context to the local, default instance of SQL Server and database tables  
  
CD \sql\localhost\default\databases  
$db = get-item AdventureWorks2012  
  
CD AdventureWorks\tables  
  
#Get a reference to the table  
$tb = get-item Production.ProductCategory  
  
# Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
  
$ftc = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextCatalog -argumentlist $db, "Test_Catalog2"  
$ftc.IsDefault = $true  
  
# Create the Full Text Search catalog on the instance of SQL Server.  
$ftc.Create()  
  
# Define a FullTextIndex object variable by supplying the parent table argument in the constructor.  
$fti = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextIndex -argumentlist $tb  
  
#  Define a FullTextIndexColumn object variable by supplying the parent index   
#  and column name arguments in the constructor.  
  
$ftic = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextIndexColumn -argumentlist $fti, "Name"  
  
# Add the indexed column to the index.  
$fti.IndexedColumns.Add($ftic)  
  
# Set change tracking  
$fti.ChangeTracking = [Microsoft.SqlServer.Management.SMO.ChangeTracking]::Automatic  
  
# Specify the unique index on the table that is required by the Full Text Search index.  
$fti.UniqueIndexName = "AK_ProductCategory_Name"  
  
# Specify the catalog associated with the index.  
$fti.CatalogName = "Test_Catalog2"  
  
# Create the Full Text Search Index  
$fti.Create()  
```  
  
  
