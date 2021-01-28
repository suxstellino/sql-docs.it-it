---
title: Riferimenti a codice personalizzato e ad assembly in espressioni in Progettazione report | Microsoft Docs
description: Informazioni su come aggiungere riferimenti al codice personalizzato incorporato in un report. Compilare e salvare nel computer e distribuire nel server di report in Generatore report.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- items [Reporting Services], expressions
- data [Reporting Services], expressions
- expressions [Reporting Services], about expressions
- expressions [Reporting Services]
- SSRS, expressions
- formulas [Reporting Services]
- data manipulation [Reporting Services]
- SQL Server Reporting Services, expressions
ms.assetid: ae8a0166-2ccc-45f4-8d28-c150da7b73de
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31e92b49186e79c78cbaa2d13859d8dd3abe5c5d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596344"
---
# <a name="custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs"></a>Riferimenti a codice personalizzato e ad assembly in espressioni in Progettazione report (SSRS)
  È possibile aggiungere riferimenti a codice personalizzato incorporato in un report o ad assembly personalizzati compilati e salvati nel computer in uso e distribuiti nel server di report. Usare codice incorporato per costanti personalizzate, funzioni complesse o funzioni usate più volte in un singolo report. Usare gli assembly di codice personalizzati per mantenere il codice in un'unica posizione e condividerne l'utilizzo in più report. Il codice personalizzato può includere nuove costanti, variabili, funzioni o subroutine personalizzate. È possibile includere riferimenti di sola lettura a raccolte predefinite come ad esempio Parameters. Non è tuttavia possibile passare alle funzioni personalizzate set di valori di dati. In particolare, non sono supportate le aggregazioni personalizzate.  
  
> [!IMPORTANT]  
>  Per i calcoli che dipendono dal tempo, valutati una volta sola in fase di esecuzione e di cui si desidera mantenere inalterato il valore durante tutta l'elaborazione del report, considerare se utilizzare una variabile di report o una variabile di gruppo. Per altre informazioni, vedere [Riferimenti a raccolte di variabili di report e di gruppo &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md).  
  
 Progettazione report è l'ambiente di creazione preferito da usare per l'aggiunta di codice personalizzato a un report. Generatore report supporta l'elaborazione di report che dispongono di espressioni valide o in cui sono inclusi riferimenti agli assembly personalizzati in un server di report. Generatore report non consente di aggiungere un riferimento a un assembly personalizzato.  
  
> [!NOTE]  
>  Tenere presente che durante un aggiornamento di un server di report, i report che dipendono dagli assembly personalizzati potrebbero richiedere passaggi aggiuntivi per il completamento dell'aggiornamento.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="working-with-custom-code-in-report-builder"></a><a name="RB3"></a> Utilizzo di codice personalizzato in Generatore report  
 In Generatore report è possibile aprire un report da un server di report in cui sono inclusi riferimenti agli assembly personalizzati. Ad esempio, è possibile modificare report creati e distribuiti tramite Progettazione report in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Gli assembly personalizzati devono essere distribuiti nel server di report.  
  
 Non è possibile effettuare le operazioni seguenti:  
  
1.  Aggiungere riferimenti o istanze del membro della classe a un report.  
  
2.  Visualizzare in anteprima un report con i riferimenti agli assembly personalizzati in modalità locale.  
  
##  <a name="including-references-to-commonly-used-functions"></a><a name="Common"></a> Inclusione di riferimenti a funzioni usate di frequente  
 Usare la finestra di dialogo **Espressione** per visualizzare un elenco per categoria di funzioni comuni predefinite di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Quando si espande **Funzioni comuni** e fa clic su una categoria, nel riquadro **Elemento** viene visualizzato l'elenco di funzioni da includere in un'espressione. Le funzioni comuni includono classi degli spazi dei nomi [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.Math> e <xref:System.Convert> e funzioni della libreria run-time di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Per praticità, è possibile visualizzare le funzioni usate più di frequente nella finestra di dialogo **Espressione** , in cui sono elencate per categoria: Testo, Data e ora, Matematiche, Ispezione, Flusso programma, Aggregazione, Finanziarie, Conversione e Varie. Le funzioni usate meno di frequente non sono riportate nell'elenco, ma possono comunque essere usate in un'espressione.  
  
 Per utilizzare una funzione predefinita, fare doppio clic sul relativo nome nel riquadro Elemento. Nel riquadro Descrizione verrà visualizzata una descrizione della funzione e nel riquadro Esempio un esempio della chiamata alla funzione. Nel riquadro del codice, quando si digita il nome della funzione seguito da una parentesi aperta **(** , tramite IntelliSense verranno visualizzate tutte le sintassi valide per la chiamata alla funzione. Ad esempio per calcolare il valore massimo per un campo denominato `Quantity` in una tabella, aggiungere l'espressione semplice `=Max(` nel riquadro del codice, quindi usare gli smart tag per visualizzare tutte le possibili sintassi valide per la chiamata alla funzione. Per completare questo esempio, digitare `=Max(Fields!Quantity.Value)`.  
  
 Per altre informazioni su ogni funzione, vedere <xref:System.Math>, <xref:System.Convert>e la pagina relativa ai [membri delle librerie run-time di Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) in MSDN.  
  
##  <a name="including-references-to-less-commonly-used-functions"></a><a name="NotCommon"></a> Inclusione di riferimenti a funzioni usate meno di frequente  
 Per includere un riferimento ad altri spazi dei nomi CLR usati meno di frequente, è necessario usare un riferimento completo, ad esempio <xref:System.Text.StringBuilder>. IntelliSense non è supportato nel riquadro del codice della finestra di dialogo **Espressione** per queste funzioni usate meno di frequente.  
  
 Per altre informazioni, vedere la pagina relativa ai [membri delle librerie di runtime di Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) in MSDN.  
  
##  <a name="including-references-to-external-assemblies"></a><a name="External"></a> Inclusione di riferimenti ad assembly esterni  
 Per includere un riferimento a una classe in un assembly esterno, è necessario identificare l'assembly per il componente Elaborazione report. Usare la pagina **Riferimenti** della finestra di dialogo **Proprietà report** per specificare il nome completo dell'assembly da aggiungere al report. Nell'espressione è necessario usare il nome completo per la classe nell'assembly. Le classi di un assembly esterno non vengono visualizzate nella finestra di dialogo **Espressione** ; è necessario fornire il relativo nome corretto. Un nome completo include lo spazio dei nomi, il nome della classe e il nome del membro.  
  
##  <a name="including-embedded-code"></a><a name="Embedded"></a> Inclusione di codice incorporato  
 Per aggiungere codice incorporato a un report, usare la scheda Codice della finestra di dialogo **Proprietà report** . Il blocco di codice creato può contenere più metodi. È necessario che i metodi del codice incorporato siano scritti in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] e siano basati su istanze. Tramite il componente Elaborazione report, vengono aggiunti automaticamente i riferimenti per gli spazi dei nomi System.Convert e System.Math. Usare la pagina **Riferimenti** della finestra di dialogo **Proprietà report** per aggiungere altri riferimenti ad assembly. Per altre informazioni, vedere [Aggiungere un riferimento a un assembly in un report &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md).  
  
 I metodi nel codice incorporato sono disponibili tramite un membro **Code** definito a livello globale. Per accedere ai metodi, fare riferimento al membro **Code** e al nome del metodo. L'esempio seguente consente di chiamare il metodo **ToUSD** per la conversione del valore del campo `StandardCost` in un valore in dollari:  
  
```  
=Code.ToUSD(Fields!StandardCost.Value)  
```  
  
 Per fare riferimento a raccolte predefinite nel codice personalizzato, includere un riferimento all'oggetto **Report** predefinito:  
  
```  
=Report.Parameters!Param1.Value  
```  
  
 Negli esempi seguenti viene illustrato come definire alcune variabili e costanti personalizzate.  
  
```  
Public Const MyNote = "Authored by Bob"  
Public Const NCopies As Int32 = 2  
Public Dim  MyVersion As String = "123.456"  
Public Dim MyDoubleVersion As Double = 123.456  
```  
  
 Sebbene le variabili e le costanti personalizzate non siano presenti nella categoria **Costanti** nella finestra di dialogo **Espressioni** , in cui vengono visualizzate solo costanti predefinite, è possibile farvi riferimento da qualsiasi espressione, come illustrato negli esempi seguenti. In un'espressione una costante personalizzata viene considerata come un elemento **Variant**.  
  
```  
=Code.MyNote  
=Code.NCopies  
=Code.MyVersion  
=Code.MyDoubleVersion  
```  
  
 Nell'esempio seguente sono inclusi sia il riferimento al codice sia l'implementazione del codice della funzione **FixSpelling** che consente la sostituzione del testo `"Bicycle"` per tutte le occorrenze del testo "Bike" nel campo `SubCategory` .  
  
 `=Code.FixSpelling(Fields!SubCategory.Value)`  
  
 Il codice seguente, se incorporato nel blocco di codice di una definizione del report, indica un'implementazione del metodo **FixSpelling** . Questo esempio illustra come usare un riferimento completo alla classe [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] **StringBuilder**.  
  
```vb  
Public Function FixSpelling(ByVal s As String) As String  
   Dim strBuilder As New System.Text.StringBuilder(s)  
   If s.Contains("Bike") Then  
      strBuilder.Replace("Bike", "Bicycle")  
      Return strBuilder.ToString()  
      Else : Return s  
   End If  
End Function  
```  
  
 Per altre informazioni sulle raccolte di oggetti predefiniti e sull'inizializzazione, vedere [Riferimenti alle raccolte predefinite Globals e Users &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md) e [Inizializzazione di oggetti assembly personalizzati](../../reporting-services/custom-assemblies/initializing-custom-assembly-objects.md).  
  
##  <a name="including-references-to-parameters-from-code"></a><a name="Parameters"></a> Inclusione di riferimenti ai parametri da codice  
 È possibile fare riferimento alla raccolta globale di parametri tramite codice personalizzato in un blocco di codice della definizione del report oppure in un assembly personalizzato specificato dall'utente. La raccolta di parametri è di sola lettura e non include iteratori pubblici. Non è possibile usare un costrutto **For Each** di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] per analizzare la raccolta. È necessario conoscere il nome del parametro definito nella definizione del report per potervi fare riferimento nel codice personalizzato. È tuttavia possibile scorrere tutti i valori di un parametro multivalore.  
  
 Nella tabella seguente sono inclusi esempi di riferimenti alla raccolta predefinita `Parameters` da codice personalizzato:  
  
 **Passaggio di un'intera raccolta globale di parametri al codice personalizzato.** Questa funzione restituisce il valore di un parametro di report *MyParameter* specifico.  
  
 Riferimento nell'espressione `=Code.DisplayAParameterValue(Parameters)`  
  
 Definizione del codice personalizzato  
  
```  
Public Function DisplayAParameterValue(ByVal parameters as Parameters) as Object  
Return parameters("MyParameter").Value  
End Function  
```  
  
 **Passaggio di un singolo parametro al codice personalizzato.**  
  
 Riferimento nell'espressione `=Code.ShowParametersValues(Parameters!DayOfTheWeek)`  
  
 In questo esempio viene restituito il valore del parametro passato. Se il parametro è un parametro multivalore, la stringa restituita è una concatenazione di tutti i valori.  
  
 Definizione del codice personalizzato  
  
```  
Public Function ShowParameterValues(ByVal parameter as Parameter)  
 as String  
   Dim s as String   
   If parameter.IsMultiValue then  
      s = "Multivalue: "   
      For i as integer = 0 to parameter.Count-1  
         s = s + CStr(parameter.Value(i)) + " "   
      Next  
   Else  
      s = "Single value: " + CStr(parameter.Value)  
   End If  
   Return s  
End Function  
```  
  
##  <a name="including-references-to-code-from-custom-assemblies"></a><a name="Custom"></a> Inclusione di riferimenti al codice da assembly personalizzati  
 Per usare assembly personalizzati in un report, è innanzitutto necessario creare l'assembly, renderlo disponibile in Progettazione report, aggiungere un riferimento all'assembly nel report e quindi usare un'espressione nel report per fare riferimento ai metodi contenuti in tale assembly. Quando il report viene distribuito al server di report, è necessario distribuire anche l'assembly personalizzato al server di report.  
  
 Per informazioni su come creare un assembly personalizzato e renderlo disponibile per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Uso di assembly personalizzati con i report](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md).  
  
 Per fare riferimento al codice personalizzato in un'espressione, è necessario chiamare il membro di una classe nell'assembly. La modalità di esecuzione di tale operazione dipende dal tipo di metodo, ovvero statico o basato su istanze. I metodi statici all'interno di un assembly personalizzato sono disponibili globalmente all'interno del report. È possibile accedere ai metodi statici nelle espressioni specificando lo spazio dei nomi, la classe e il nome del metodo. Nell'esempio seguente viene chiamato il metodo **ToGBP**, che consente di convertire il valore del campo **StandardCost** da dollari a sterline:  
  
```  
=CurrencyConversion.DollarCurrencyConversion.ToGBP(Fields!StandardCost.Value)  
```  
  
 I metodi basati su istanze sono disponibili tramite un membro **Code** definito a livello globale. Per accedere ai metodi, fare riferimento al membro **Code** , aggiungendo l'istanza e il nome del metodo. Nell'esempio seguente viene chiamato il metodo di istanza **ToEUR**, che consente di convertire il valore del campo **StandardCost** da dollari a euro:  
  
```  
=Code.m_myDollarCoversion.ToEUR(Fields!StandardCost.Value)  
```  
  
> [!NOTE]  
>  In Progettazione report un assembly personalizzato viene caricato una volta e non viene scaricato finché non si chiude [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Se si visualizza in anteprima un report, si apportano modifiche a un assembly personalizzato usato nel report e quindi si visualizza di nuovo il report in anteprima, le modifiche non compariranno nella seconda anteprima. Per ricaricare l'assembly, chiudere e riaprire [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , quindi visualizzare l'anteprima del report.  
  
 Per altre informazioni sull'accesso al codice, vedere [Accessing Custom Assemblies Through Expressions](../../reporting-services/custom-assemblies/accessing-custom-assemblies-through-expressions.md).  
  
##  <a name="passing-built-in-collections-into-custom-assemblies"></a><a name="collections"></a> Passaggio di raccolte predefinite in assembly personalizzati  
 Se si vuole passare raccolte predefinite, ad esempio la raccolta *Globals* o *Parameters* , in un assembly personalizzato per l'elaborazione, è necessario aggiungere nel progetto di codice un riferimento all'assembly che definisce le raccolte predefinite e accede allo spazio dei nomi corretto. A seconda del fatto che l'assembly personalizzato venga sviluppato per un report eseguito in un server di report (report del server) o un report eseguito localmente in un'applicazione .NET (report locale), l'assembly a cui è necessario fare riferimento è diverso. Vedere di seguito per altri dettagli.  
  
-   **Spazio dei nomi:** Microsoft.ReportingServices.ReportProcessing.ReportObjectModel  
  
-   **Assembly (report locale):** Microsoft.ReportingServices.ProcessingObjectModel.dll  
  
-   **Assembly (report del server):** Microsoft.ReportViewer.ProcessingObjectModel.dll  
  
 Poiché il contenuto delle raccolte *Fields* e *ReportItems* può cambiare dinamicamente in fase di esecuzione, non è consigliabile mantenere i riferimenti tra diverse chiamate nell'assembly personalizzato (ad esempio in una variabile membro). Lo stesso consiglio si applica in genere a tutte le raccolte predefinite.  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere codice a un report &#40;SSRS&#41;](../../reporting-services/report-design/add-code-to-a-report-ssrs.md)   
 [Utilizzo di assembly personalizzati con i report](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [Aggiungere un riferimento a un assembly in un report &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md)   
 [Esercitazioni su Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)   
 [Esempi di espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Esempi di report (Generatore report e SSRS)](https://go.microsoft.com/fwlink/?LinkId=198283)  
  
