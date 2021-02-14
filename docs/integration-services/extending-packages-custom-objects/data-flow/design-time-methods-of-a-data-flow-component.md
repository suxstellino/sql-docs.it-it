---
description: Metodi della fase di progettazione di un componente del flusso di dati
title: Metodi della fase di progettazione di un componente flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], method execution sequence
- ProcessInput method
- method execution sequence [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], method execution sequence
ms.assetid: b5a121a1-b87c-441b-a42c-2cec628dc81c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 44a6aa6eb512307f8a4c99c15137f246ea7ec8cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348916"
---
# <a name="design-time-methods-of-a-data-flow-component"></a>Metodi della fase di progettazione di un componente del flusso di dati

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Prima dell'esecuzione, l'attività Flusso di dati si trova nel cosiddetto stato della fase di progettazione, quando viene sottoposta a modifiche incrementali. Tali modifiche possono includere l'aggiunta o la rimozione di componenti, l'aggiunta o la rimozione degli oggetti percorso che connettono i componenti e modifiche ai metadati dei componenti. Quando si verificano modifiche ai metadati, il componente può monitorarle e rispondere. Ad esempio, un componente può impedire determinate modifiche o aggiungerne altre in risposta a una modifica. In fase di progettazione la finestra di progettazione interagisce con un componente tramite l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100> della fase di progettazione.  
  
## <a name="design-time-implementation"></a>Implementazione della fase di progettazione  
 L'interfaccia della fase di progettazione di un componente viene descritta dall'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>. Anche se questa interfaccia non viene implementata in modo esplicito, una maggiore familiarità con i metodi definiti al suo interno consente di identificare i metodi della classe di base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> che corrispondono all'istanza della fase di progettazione di un componente.  
  
 Quando un componente viene caricato in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], viene creata un'istanza dell'istanza della fase di progettazione del componente e vengono chiamati i metodi dell'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100> quando il componente viene modificato. L'implementazione della classe di base consente di eseguire l'override solo dei metodi richiesti dal componente. In molti casi, è possibile eseguire l'override di tali metodi per evitare modifiche non corrette a un componente. Ad esempio, per impedire agli utenti di aggiungere un output a un componente, eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.InsertOutput%2A>. In caso contrario, quando la classe di base chiama l'implementazione di questo metodo, viene aggiunto un output al componente.  
  
 Indipendentemente dallo scopo o dalla funzionalità del componente, è necessario eseguire l'override dei metodi <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>. Per altre informazioni su <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>, vedere [Convalida di un componente del flusso di dati](../../../integration-services/extending-packages-custom-objects/data-flow/validating-a-data-flow-component.md).  
  
## <a name="providecomponentproperties-method"></a>Metodo ProvideComponentProperties  
 L'inizializzazione di un componente si verifica nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>. Questo metodo viene chiamato da Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] quando un componente viene aggiunto all'attività Flusso di dati ed è simile a un costruttore della classe. Gli sviluppatori di componenti devono creare e inizializzare gli input, gli output e le proprietà personalizzate durante la chiamata a questo metodo. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> è diverso da un costruttore perché non viene chiamato ogni volta che viene creata un'istanza dell'istanza della fase di progettazione o dell'istanza di runtime del componente.  
  
 L'implementazione della classe di base del metodo aggiunge un input e un output al componente e assegna l'ID dell'input alla proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A>. Tuttavia, in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], gli oggetti input e output aggiunti dalla classe di base non sono denominati. I pacchetti che contengono un componente con oggetti input o output la cui proprietà Name non è impostata non vengono caricati correttamente. Pertanto, quando si usa l'implementazione di base, è necessario assegnare in modo esplicito i valori alla proprietà Name dell'input e dell'output predefiniti.  
  
```csharp  
public override void ProvideComponentProperties()  
{  
    /// TODO: Reset the component.  
    /// TODO: Add custom properties.  
    /// TODO: Add input objects.  
    /// TODO: Add output objects.  
}  
```  
  
```vb  
Public Overrides Sub ProvideComponentProperties()  
    ' TODO: Reset the component.  
    ' TODO: Add custom properties.  
    ' TODO: Add input objects.  
    ' TODO: Add output objects.  
End Sub  
```  
  
### <a name="creating-custom-properties"></a>Creazione di proprietà personalizzate.  
 La chiamata al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> è il punto in cui gli sviluppatori di componenti devono aggiungere proprietà personalizzate (<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100>) al componente. Le proprietà personalizzate non includono una proprietà del tipo di dati. Il tipo di dati di una proprietà personalizzata viene impostato dal tipo di dati del valore assegnato alla relativa proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.Value%2A>. Tuttavia, dopo aver assegnato un valore iniziale alla proprietà personalizzata, non è possibile assegnare un valore con un tipo di dati diverso.  
  
> [!NOTE]  
>  L'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> offre un supporto limitato per i valori delle proprietà di tipo **Object**. L'unico oggetto che è possibile archiviare come valore di una proprietà personalizzata è una matrice di tipi semplici come stringhe o numeri interi.  
  
 È possibile indicare che la proprietà personalizzata supporta espressioni di proprietà impostando il valore della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.ExpressionType%2A> su **CPET_NOTIFY** dall'enumerazione <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSCustomPropertyExpressionType>, come illustrato nell'esempio seguente. Non è necessario aggiungere codice per gestire o convalidare l'espressione di proprietà immessa dall'utente. È possibile impostare un valore predefinito per la proprietà, convalidarlo, quindi leggerlo e utilizzarlo normalmente.  
  
```csharp  
IDTSCustomProperty100 myCustomProperty;  
...  
myCustomProperty.ExpressionType = DTSCustomPropertyExpressionType.CPET_NOTIFY;  
```  
  
```vb  
Dim myCustomProperty As IDTSCustomProperty100  
...  
myCustomProperty.ExpressionType = DTSCustomPropertyExpressionType.CPET_NOTIFY  
```  
  
 È possibile limitare gli utenti alla selezione di un valore della proprietà personalizzata da un'enumerazione usando la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A>, come illustrato nell'esempio seguente, in cui si presuppone che sia stata definita un'enumerazione pubblica denominata **MyValidValues**.  
  
```csharp  
IDTSCustomProperty100 customProperty = outputColumn.CustomPropertyCollection.New();  
customProperty.Name = "My Custom Property";  
// This line associates the type with the custom property.  
customProperty.TypeConverter = typeof(MyValidValues).AssemblyQualifiedName;  
// Now you can use the enumeration values directly.  
customProperty.Value = MyValidValues.ValueOne;    
```  
  
```vb  
Dim customProperty As IDTSCustomProperty100 = outputColumn.CustomPropertyCollection.New   
customProperty.Name = "My Custom Property"   
' This line associates the type with the custom property.  
customProperty.TypeConverter = GetType(MyValidValues).AssemblyQualifiedName   
' Now you can use the enumeration values directly.  
customProperty.Value = MyValidValues.ValueOne  
```  
  
 Per altre informazioni, vedere gli argomenti relativi alla conversione di tipi generalizzata e all'implementazione del convertitore di tipi in [MSDN Library](../../../sql-server/index.yml).  
  
 È possibile specificare una finestra di dialogo dell'editor personalizzata per il valore della proprietà personalizzata tramite la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A>, come illustrato nell'esempio seguente. Creare prima di tutto un editor di tipo personalizzato che eredita da **System.Drawing.Design.UITypeEditor**, se non è possibile individuare una classe di editor di tipo dell'interfaccia utente che soddisfa le esigenze specifiche.  
  
```csharp  
public class MyCustomTypeEditor : UITypeEditor  
{  
...  
}  
```  
  
```vb  
Public Class MyCustomTypeEditor  
  Inherits UITypeEditor   
  ...  
End Class  
```  
  
 Specificare questa classe come valore della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A> della proprietà personalizzata.  
  
```csharp  
IDTSCustomProperty100 customProperty = outputColumn.CustomPropertyCollection.New();  
customProperty.Name = "My Custom Property";  
// This line associates the editor with the custom property.  
customProperty.UITypeEditor = typeof(MyCustomTypeEditor).AssemblyQualifiedName;  
```  
  
```vb  
Dim customProperty As IDTSCustomProperty100 = outputColumn.CustomPropertyCollection.New   
customProperty.Name = "My Custom Property"   
' This line associates the editor with the custom property.  
customProperty.UITypeEditor = GetType(MyCustomTypeEditor).AssemblyQualifiedName  
```  
  
 Per altre informazioni, vedere "implementazione di un editor di tipi con interfaccia utente" in [MSDN Library](../../../sql-server/index.yml).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di runtime di un componente del flusso di dati](../../../integration-services/extending-packages-custom-objects/data-flow/run-time-methods-of-a-data-flow-component.md)  
  
