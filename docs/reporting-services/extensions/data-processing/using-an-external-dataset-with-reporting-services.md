---
title: Using an External Dataset with Reporting Services (Uso di un set di dati esterni con Reporting Services) | Microsoft Docs
description: Creare estensioni per l'elaborazione dati personalizzate in Reporting Services per poter integrare oggetti DataSet personalizzati creati dalle applicazioni esterne.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- DataSet objects [Reporting Services]
- data processing extensions [Reporting Services], custom DataSet objects
- custom DataSet objects [Reporting Services]
- external DataSet objects [Reporting Services]
ms.assetid: 11daa013-ec17-4760-80e3-6d84cd8d5722
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f34dee113abaf59f54db5983d39353bb45fc0641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060946"
---
# <a name="using-an-external-dataset-with-reporting-services"></a>Utilizzo di un set di dati esterno con Reporting Services
  L'oggetto **DataSet** è fondamentale per il supporto di scenari di dati disconnessi e distribuiti con [!INCLUDE[vstecado](../../../includes/vstecado-md.md)]. L'oggetto **DataSet** è una rappresentazione di dati residente in memoria che offre un modello di programmazione relazionale coerente indipendentemente dall'origine dati. Questo oggetto può essere utilizzato con più origini dati diverse, con dati XML o per gestire i dati locali dell'applicazione. L'oggetto **DataSet** rappresenta un set di dati completo, che include tabelle correlate, vincoli e relazioni tra le tabelle. Grazie alla versatilità dell'oggetto **DataSet** nell'archiviazione e nell'esposizione dei dati, i dati possono spesso essere elaborati e trasformati in un oggetto **DataSet** prima della creazione di qualsiasi report con tali dati.  
  
 Con le estensioni per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è possibile integrare qualsiasi oggetto **DataSet** personalizzato creato dalle applicazioni esterne. A tale scopo, è possibile creare un'estensione per l'elaborazione dati personalizzata in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] che funga da ponte tra l'oggetto **DataSet** e il server di report. La maggior parte del codice per l'elaborazione di questo oggetto **DataSet** è inclusa nella classe **DataReader** creata.  
  
 Il primo passaggio nell'esposizione dell'oggetto **DataSet** nel server di report consiste nell'implementare un metodo specifico del provider nella classe **DataReader** che consente di popolare un oggetto **DataSet**. Nell'esempio seguente viene illustrato come caricare dati statici in un oggetto **DataSet** usando un metodo specifico del provider nella classe **DataReader**.  
  
```vb  
'Private members of the DataReader class  
Private m_dataSet As System.Data.DataSet  
Private m_currentRow As Integer  
  
'Method to create a dataset  
Friend Sub CreateDataSet()  
   ' Create a dataset.  
   Dim ds As New System.Data.DataSet("myDataSet")  
   ' Create a data table.   
   Dim dt As New System.Data.DataTable("myTable")  
   ' Create a data column and set various properties.   
   Dim dc As New System.Data.DataColumn()  
   dc.DataType = System.Type.GetType("System.Decimal")  
   dc.AllowDBNull = False  
   dc.Caption = "Number"  
   dc.ColumnName = "Number"  
   dc.DefaultValue = 25  
   ' Add the column to the table.   
   dt.Columns.Add(dc)  
   ' Add 10 rows and set values.   
   Dim dr As System.Data.DataRow  
   Dim i As Integer  
   For i = 0 To 9  
      dr = dt.NewRow()  
      dr("Number") = i + 1  
      ' Be sure to add the new row to the DataRowCollection.   
      dt.Rows.Add(dr)  
   Next i  
  
   ' Fill the dataset.  
   ds.Tables.Add(dt)  
  
   ' Use a private variable to store the dataset in your  
   ' DataReader.  
   m_dataSet = ds  
  
   ' Set the current row to -1.  
   m_currentRow = - 1  
End Sub 'CreateDataSet  
```  
  
```csharp  
// Private members of the DataReader class  
private System.Data.DataSet m_dataSet;  
private int m_currentRow;  
  
// Method to create a dataset  
internal void CreateDataSet()  
{  
   // Create a dataset.  
   System.Data.DataSet ds = new System.Data.DataSet("myDataSet");  
   // Create a data table.   
   System.Data.DataTable dt = new System.Data.DataTable("myTable");  
   // Create a data column and set various properties.   
   System.Data.DataColumn dc = new System.Data.DataColumn();   
   dc.DataType = System.Type.GetType("System.Decimal");   
   dc.AllowDBNull = false;   
   dc.Caption = "Number";   
   dc.ColumnName = "Number";   
   dc.DefaultValue = 25;   
   // Add the column to the table.   
   dt.Columns.Add(dc);   
   // Add 10 rows and set values.   
   System.Data.DataRow dr;   
   for(int i = 0; i < 10; i++)  
   {   
      dr = dt.NewRow();   
      dr["Number"] = i + 1;   
      // Be sure to add the new row to the DataRowCollection.   
      dt.Rows.Add(dr);  
   }  
  
   // Fill the dataset.  
   ds.Tables.Add(dt);  
  
   // Use a private variable to store the dataset in your  
   // DataReader.  
   m_dataSet = ds;  
  
   // Set the current row to -1.  
   m_currentRow = -1;  
}  
```  
  
```csharp  
public bool Read()  
{  
   m_currentRow++;  
   if (m_currentRow >= m_dataSet.Tables[0].Rows.Count)   
   {  
      return (false);  
   }   
   else   
   {  
      return (true);  
   }  
}  
  
public int FieldCount  
{  
   // Return the count of the number of columns, which in  
   // this case is the size of the column metadata  
   // array.  
   get { return m_dataSet.Tables[0].Columns.Count; }  
}  
  
public string GetName(int i)  
{  
   return m_dataSet.Tables[0].Columns[i].ColumnName;  
}  
  
public Type GetFieldType(int i)  
{  
   // Return the actual Type class for the data type.  
   return m_dataSet.Tables[0].Columns[i].DataType;  
}  
  
public Object GetValue(int i)  
{  
   return m_dataSet.Tables[0].Rows[m_currentRow][i];  
}  
  
public int GetOrdinal(string name)  
{  
   // Look for the ordinal of the column with the same name and return it.  
   // Returns -1 if not found.  
   return m_dataSet.Tables[0].Columns[name].Ordinal;  
}  
```  
  
 Dopo aver creato o recuperato il set di dati, è possibile usare l'oggetto **DataSet** nelle implementazioni dei membri **Read**, **GetValue**, **GetName**, **GetOrdinal**,**GetFieldType** e **FieldCount** della classe **DataReader**.  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementazione di un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
