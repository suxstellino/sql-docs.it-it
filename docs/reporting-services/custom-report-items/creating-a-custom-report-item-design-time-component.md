---
title: Creazione di un componente dell'elemento del report personalizzato per la fase di progettazione | Microsoft Docs
description: Informazioni su come creare un componente dell'elemento del report personalizzato per la fase di progettazione. Questo componente fornisce un'area di progettazione attivata che può accettare operazioni di trascinamento della selezione.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, creating
ms.assetid: 323fd58a-a462-4c48-b188-77ebc0b4212e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 237d056f8b42570fcab375330a9eb3172eb11758
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596958"
---
# <a name="creating-a-custom-report-item-design-time-component"></a>Creazione di un componente dell'elemento del report personalizzato per la fase di progettazione
  Un componente dell'elemento del report personalizzato per la fase di progettazione è un controllo che può essere utilizzato nell'ambiente Progettazione report di Visual Studio. Il componente dell'elemento del report personalizzato per la fase di progettazione fornisce un'area di progettazione attivata in grado di accettare operazioni di trascinamento della selezione, integrazione con il Visualizzatore proprietà di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e la possibilità di fornire editor di proprietà personalizzati.  
  
 Con un componente dell'elemento del report personalizzato per la fase di progettazione, è possibile posizionare un elemento del report personalizzato in un report nell'ambiente di progettazione, impostare proprietà dei dati personalizzate nell'elemento del report personalizzato, quindi salvare l'elemento del report personalizzato come parte del progetto report.  
  
 Le proprietà impostate mediante il componente per la fase di progettazione nell'ambiente di sviluppo vengono serializzate e deserializzate dall'ambiente di progettazione host, quindi archiviate come elementi nel file RDL (Report Definition Language). Quando il report viene eseguito dal componente Elaborazione report, le proprietà impostate mediante il componente per la fase di progettazione vengono passate dal componente Elaborazione report a un componente runtime dell'elemento del report personalizzato che esegue il rendering dell'elemento del report personalizzato e lo passa nuovamente a Elaborazione report.  
  
> [!NOTE]
>  Il componente dell'elemento del report personalizzato per la fase di progettazione viene implementato come componente [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. In questo documento vengono illustrati dettagli di implementazione specifici del componente dell'elemento del report personalizzato per la fase di progettazione. Per altre informazioni sullo sviluppo di componenti tramite [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vedere [Components in Visual Studio](https://go.microsoft.com/fwlink/?LinkId=116576) (Componenti in Visual Studio) in MSDN Library.  
  
 Per un esempio di elemento del report personalizzato completamente implementato, vedere [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Esempi del prodotto SQL Server Reporting Services).  
  
## <a name="implementing-a-design-time-component"></a>Implementazione di un componente per la fase di progettazione  
 La classe principale di un componente dell'elemento del report personalizzato per la fase di progettazione viene ereditata dalla classe **Microsoft.ReportDesigner.CustomReportItemDesigner**. Oltre agli attributi standard usati per un controllo [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], la classe del componente deve definire un attributo **CustomReportItem**. Questo attributo deve corrispondere al nome dell'elemento del report personalizzato secondo la definizione presente nel file reportserver.config. Per un elenco di attributi [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vedere Attributi nella documentazione di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  
  
 Nell'esempio di codice seguente vengono illustrati gli attributi applicati a un controllo dell'elemento del report personalizzato per la fase di progettazione:  
  
```csharp  
namespace PolygonsCRI  
{  
    [LocalizedName("Polygons")]  
    [Editor(typeof(CustomEditor), typeof(ComponentEditor))]  
        [ToolboxBitmap(typeof(PolygonsDesigner),"Polygons.ico")]  
        [CustomReportItem("Polygons")]  
  
    public class PolygonsDesigner : CustomReportItemDesigner  
    {  
...  
```  
  
### <a name="initializing-the-component"></a>Inizializzazione del componente  
 Le proprietà specificate dall'utente per un elemento del report personalizzato vengono passate mediante una classe <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData>. L'implementazione della classe **CustomReportItemDesigner** deve eseguire l'override del metodo **InitializeNewComponent** per creare una nuova istanza della classe <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> del componente e impostarla su valori predefiniti.  
  
 Nell'esempio di codice seguente viene illustrata una classe del componente dell'elemento del report personalizzato per la fase di progettazione che esegue l'override del metodo **CustomReportItemDesigner.InitializeNewComponent** per inizializzare la classe del componente <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData>:  
  
```csharp  
public override void InitializeNewComponent()  
        {  
            CustomData = new CustomData();  
            CustomData.DataRowHierarchy = new DataHierarchy();  
  
            // Shape grouping  
            CustomData.DataRowHierarchy.DataMembers.Add(new DataMember());  
            CustomData.DataRowHierarchy.DataMembers[0].Group = new Group();  
            CustomData.DataRowHierarchy.DataMembers[0].Group.Name = Name + "_Shape";  
            CustomData.DataRowHierarchy.DataMembers[0].Group.GroupExpressions.Add(new ReportExpression());  
  
            // Point grouping  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers.Add(new DataMember());  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group = new Group();  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group.Name = Name + "_Point";  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group.GroupExpressions.Add(new ReportExpression());  
  
            // Static column  
            CustomData.DataColumnHierarchy = new DataHierarchy();  
            CustomData.DataColumnHierarchy.DataMembers.Add(new DataMember());  
  
            // Points  
            IList<IList<DataValue>> dataValues = new List<IList<DataValue>>();  
            CustomData.DataRows.Add(dataValues);  
            CustomData.DataRows[0].Add(new List<DataValue>());  
            CustomData.DataRows[0][0].Add(NewDataValue("X", ""));  
            CustomData.DataRows[0][0].Add(NewDataValue("Y", ""));  
        }  
```  
  
### <a name="modifying-component-properties"></a>Modifica delle proprietà del componente  
 È possibile modificare le proprietà **CustomData** nell'ambiente di progettazione in diversi modi. È possibile modificare qualsiasi proprietà esposta dal componente per la fase di progettazione che sia contrassegnata con l'attributo <xref:System.ComponentModel.BrowsableAttribute> tramite il visualizzatore proprietà [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. È anche possibile modificare le proprietà trascinando elementi nell'area di progettazione dell'elemento del report personalizzato o facendo clic con il pulsante destro del mouse sul controllo nell'ambiente di progettazione e scegliendo **Proprietà** dal menu di scelta rapida per visualizzare una finestra delle proprietà personalizzata.  
  
 Nell'esempio di codice seguente viene illustrata una proprietà **Microsoft.ReportDesigner.CustomReportItemDesigner.CustomData** alla quale è applicato l'attributo <xref:System.ComponentModel.BrowsableAttribute>:  
  
```csharp  
[Browsable(true), Category("Data")]  
public string DataSetName  
{  
      get  
      {  
         return CustomData.DataSetName;  
      }  
      set  
      {  
         CustomData.DataSetName = value;  
      }  
   }  
  
```  
  
 È possibile associare il componente per la fase di progettazione a una finestra di dialogo dell'editor di proprietà personalizzato. L'implementazione dell'editor di proprietà personalizzato deve ereditare dalla classe <xref:System.ComponentModel.ComponentEditor> e creare un'istanza di una finestra di dialogo che può essere utilizzata per la modifica delle proprietà.  
  
 Nell'esempio seguente viene illustrata un'implementazione di una classe che eredita da <xref:System.ComponentModel.ComponentEditor> e che consente di visualizzare una finestra di dialogo dell'editor di proprietà personalizzato:  
  
```csharp  
internal sealed class CustomEditor : ComponentEditor  
{  
   public override bool EditComponent(  
      ITypeDescriptorContext context, object component)  
    {  
     PolygonsDesigner designer = (PolygonsDesigner)component;  
     PolygonProperties dialog = new PolygonProperties();  
     dialog.m_designerComponent = designer;  
     DialogResult result = dialog.ShowDialog();  
     if (result == DialogResult.OK)  
     {  
        designer.Invalidate();  
        designer.ChangeService().OnComponentChanged(designer, null, null, null);  
        return true;  
     }  
     else  
        return false;  
    }  
}  
```  
  
 La finestra di dialogo dell'editor di proprietà personalizzato può richiamare l'Editor espressioni di Progettazione report. Nell'esempio seguente l'Editor espressioni viene richiamato quando si seleziona il primo elemento nella casella combinata:  
  
```csharp  
private void EditableCombo_SelectedIndexChanged(object sender,   
    EventArgs e)  
{  
   ComboBox combo = (ComboBox)sender;  
   if (combo.SelectedIndex == 0 && m_launchEditor)  
   {  
      m_launchEditor = false;  
      ExpressionEditor editor = new ExpressionEditor();  
      string newValue;  
      newValue = (string)editor.EditValue(null, m_designerComponent.Site, m_oldComboValue);  
      combo.Items[0] = newValue;  
   }  
}  
  
```  
  
### <a name="using-designer-verbs"></a>Utilizzo dei verbi di progettazione  
 Un verbo di progettazione è un comando di menu collegato a un gestore evento. È possibile aggiungere verbi di progettazione da visualizzare nel menu di scelta rapida di un componente quando il controllo di runtime di un elemento del report personalizzato viene utilizzato nell'ambiente di progettazione. È possibile restituire l'elenco dei verbi di progettazione disponibili del componente runtime tramite la proprietà **Verbs**.  
  
 Nell'esempio di codice seguente vengono illustrati un verbo di progettazione e un gestore evento aggiunti a <xref:System.ComponentModel.Design.DesignerVerbCollection>, oltre al codice del gestore evento:  
  
```csharp  
public override DesignerVerbCollection Verbs  
{  
    get  
    {  
        if (m_verbs == null)  
        {  
            m_verbs = new DesignerVerbCollection();  
            m_verbs.Add(new DesignerVerb("Proportional Scaling", new EventHandler(OnProportionalScaling)));  
         m_verbs[0].Checked = (GetCustomProperty("poly:Proportional") == bool.TrueString);  
        }  
  
        return m_verbs;  
    }  
}  
  
private void OnProportionalScaling(object sender, EventArgs e)  
{  
   bool proportional = !  
        (GetCustomProperty("poly:Proportional") == bool.TrueString);  
   m_verbs[0].Checked = proportional;  
   SetCustomProperty("poly:Proportional", proportional.ToString());  
   ChangeService().OnComponentChanged(this, null, null, null);  
   Invalidate();  
}  
```  
  
### <a name="using-adornments"></a>Utilizzo delle aree di controllo  
 Le classi elemento report personalizzato possono anche implementare una classe **Microsoft.ReportDesigner.Design.Adornment**. Un'area di controllo consente al controllo dell'elemento del report personalizzato di fornire aree esterne al rettangolo principale dell'area di progettazione. Tali aree possono gestire eventi dell'interfaccia utente, quali clic del mouse e operazioni di trascinamento della selezione. La classe **Adornment** definita nello spazio dei nomi [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] **Microsoft.ReportDesigner** è un'implementazione pass-through della classe <xref:System.Windows.Forms.Design.Behavior.Adorner> presente in Windows Forms. Per la documentazione completa sulla classe **Adorner**, vedere [Cenni preliminari su BehaviorService](/previous-versions/ms171826(v=vs.140)) in MSDN Library. Per codice di esempio che implementa una classe **Microsoft.ReportDesigner.Design.Adornment**, vedere [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Esempi di prodotto SQL Server Reporting Services).  
  
 Per ulteriori informazioni sulla programmazione e sull'utilizzo di Windows Form in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], vedere i seguenti argomenti in MSDN Library:  
  
-   Attributi per componenti in fase di progettazione  
  
-   Componenti di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]  
  
-   Procedura dettagliata: Creazione di un controllo di Windows Form che usufruisca delle caratteristiche offerte da Visual Studio in fase di progettazione  
  
## <a name="see-also"></a>Vedere anche  
 [Architettura di un elemento del report personalizzato](../../reporting-services/custom-report-items/custom-report-item-architecture.md)   
 [Creazione di un componente runtime dell'elemento del report personalizzato](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Librerie di classi dell'elemento del report personalizzato](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)   
 [Procedura: Distribuire un elemento del report personalizzato](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
