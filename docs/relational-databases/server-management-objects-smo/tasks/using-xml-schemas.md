---
description: Utilizzo di XML Schema
title: Utilizzo di XML Schema | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- XML [SMO]
ms.assetid: 9d04de01-efeb-4b2d-8c28-3234bc7ff2f3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f76bc99be907bdf3b46d478802c3c71b1d7028d6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467202"
---
# <a name="using-xml-schemas"></a>Utilizzo di XML Schema

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  La programmazione XML in SMO è limitata alla fornitura di tipi di dati XML, spazi dei nomi XML e indicizzazione semplice in colonne di tipi di dati XML.  
  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]fornisce l'archiviazione nativa per le istanze di documenti XML. Gli elementi XML Schema consentono di definire tipi di dati XML complessi da utilizzare per convalidare documenti XML allo scopo di assicurare l'integrità dei dati. L'elemento XML Schema è definito nell'oggetto <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection>.  
  
## <a name="example"></a>Esempio  
 Per usare qualsiasi esempio di codice fornito, è necessario scegliere l'ambiente di programmazione, il modello di programmazione e il linguaggio di programmazione per la creazione dell'applicazione. Per altre informazioni, vedere [creare un progetto Visual C&#35; SMO in Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-an-xml-schema-in-visual-basic"></a>Creazione di un elemento XML Schema in Visual Basic  
 In questo esempio di codice viene illustrato come creare un elemento XML Schema tramite l'oggetto <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection>. La proprietà <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection.Text%2A>, che definisce la raccolta di XML Schema, contiene numerose virgolette doppie. Queste ultime vengono sostituite dalla stringa `chr(34)`.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server()
'Reference the AdventureWorks2012 2008R2 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Define an XmlSchemaCollection object by supplying the parent database and name arguments in the constructor.
Dim xsc As XmlSchemaCollection
xsc = New XmlSchemaCollection(db, "MySampleCollection")
xsc.Text = "\<schema xmlns=" + Chr(34) + "http://www.w3.org/2001/XMLSchema" + Chr(34) + "  xmlns:ns=" + Chr(34) + "http://ns" + Chr(34) + ">\<element name=" + Chr(34) + "e" + Chr(34) + " type=" + Chr(34) + "dateTime" + Chr(34) + "/></schema>"
'Create the XML schema collection on the instance of SQL Server.
xsc.Create()
```
  
## <a name="creating-an-xml-schema-in-visual-c"></a>Creazione di un elemento XML Schema in Visual C#  
 In questo esempio di codice viene illustrato come creare un elemento XML Schema tramite l'oggetto <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection>. La proprietà <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection.Text%2A>, che definisce la raccolta di XML Schema, contiene numerose virgolette doppie. Queste ultime vengono sostituite dalla stringa `chr(34)`.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv = default(Server);  
            srv = new Server();  
            //Reference the AdventureWorks2012 database.   
            Database db = default(Database);  
            db = srv.Databases["AdventureWorks2012"];  
            //Define an XmlSchemaCollection object by supplying the parent  
            // database and name arguments in the constructor.   
            XmlSchemaCollection xsc = default(XmlSchemaCollection);  
            xsc = new XmlSchemaCollection(db, "MySampleCollection");  
            xsc.Text = "\<schema xmlns=" + Strings.Chr(34) + "http://www.w3.org/2001/XMLSchema" + Strings.Chr(34) + " xmlns:ns=" + Strings.Chr(34) + "http://ns" + Strings.Chr(34) + ">\<element name=" + Strings.Chr(34) + "e" + Strings.Chr(34) + " type=" + Strings.Chr(34) + "dateTime" + Strings.Chr(34) + "/></schema>";  
            //Create the XML schema collection on the instance of SQL Server.   
            xsc.Create();  
        }  
```  
  
## <a name="creating-an-xml-schema-in-powershell"></a>Creazione di un elemento XML Schema in PowerShell  
 In questo esempio di codice viene illustrato come creare un elemento XML Schema tramite l'oggetto <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection>. La proprietà <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection.Text%2A>, che definisce la raccolta di XML Schema, contiene numerose virgolette doppie. Queste ultime vengono sostituite dalla stringa `chr(34)`.  
  
```powershell   
#Get a server object which corresponds to the default instance replace LocalMachine with the physical server  
cd \sql\LocalHost  
$srv = get-item default  
  
#Reference the AdventureWorks database.  
$db = $srv.Databases["AdventureWorks2012"]  
  
#Create a new schema collection  
$xsc = New-Object -TypeName Microsoft.SqlServer.Management.SMO.XmlSchemaCollection `  
-argumentlist $db,"MySampleCollection"  
  
#Add the xml  
$dq = '"' # the double quote character  
$xsc.Text = "<schema xmlns=" + $dq + "http://www.w3.org/2001/XMLSchema" + $dq + `  
"  xmlns:ns=" + $dq + "http://ns" + $dq + "><element name=" + $dq + "e" + $dq +`  
 " type=" + $dq + "dateTime" + $dq + "/></schema>"  
  
#Create the XML schema collection on the instance of SQL Server.  
$xsc.Create()  
```  
  
  
