---
title: Tipi di dati nelle espressioni (Generatore report) | Microsoft Docs
description: Esaminare il fatto che i tipi di dati rappresentano tipologie di dati diverse che consentono l'archiviazione e l'elaborazione dei dati in Generatore report.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 94fdf921-270c-4c12-87b3-46b1cc98fae5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0202703dec933871d6b6a611ff1139a68ee2c938
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595360"
---
# <a name="data-types-in-expressions-report-builder-and-ssrs"></a>Tipi di dati nelle espressioni (Generatore report e SSRS)
  I tipi di dati rappresentano tipologie di dati diversi che possono essere archiviati ed elaborati in modo efficiente. I tipi di dati standard includono testo, noto anche come stringhe, numeri con e senza posizioni decimali, date e ore e immagini. I valori in un report devono essere costituiti da un tipo di dati RDL (Report Definition Language). È possibile formattare un valore in base alle proprie preferenze quando si lo visualizza in un report. Un campo che rappresenta la valuta, ad esempio, viene archiviato nella definizione del report come numero a virgola mobile, ma può essere visualizzato in diversi formati a seconda della proprietà di formattazione scelta.  
  
 Per altre informazioni sui formati di visualizzazione, vedere [Formattazione degli elementi del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="report-definition-language-rdl-data-types-and-common-language-runtime-clr-data-types"></a>Tipi di dati RDL (Report Definition Language) e tipi di dati CLR (Common Language Runtime)  
 I valori specificati in un file RDL devono essere un tipo di dati RDL. Quando il report viene compilato ed elaborato, i tipi di dati RDL vengono convertiti in tipi di dati CLR. Nella tabella seguente viene visualizzata la conversione, contrassegnata come valore predefinito:  
  
|Tipo RDL|Tipi CLR|  
|--------------|---------------|  
|string|Predefinito: string<br /><br /> Chart, GUID, Timespan|  
|Boolean|Predefinito: Boolean|  
|Integer|Predefinito: Int64<br /><br /> Int16, Int32, Uint16, Uint64, Byte, Sbyte|  
|Datetime|Predefinito: Datetime<br /><br /> DateTimeOffset|  
|Float|Predefinito: Double<br /><br /> Single, Decimal|  
|Binary|Predefinito: Byte[]|  
|Variant|Uno qualsiasi tra quelli riportati in precedenza eccetto Byte []|  
|VariantArray|Matrice di Variant|  
|Serializable|Variant oppure tipi contrassegnati con Serializable o che consentono di implementare ISerializable.|  
  
## <a name="understanding-data-types-and-writing-expressions"></a>Informazioni sui tipi di dati e scrittura di espressioni  
 È importante comprendere i tipi di dati quando si scrivono espressioni per confrontare o combinare valori, ad esempio quando si definiscono espressioni di raggruppamento o di filtro o quando si calcolano le aggregazioni. Confronti e calcoli sono validi solo tra elementi dello stesso tipo di dati. Se i tipi di dati non corrispondono, è necessario convertire esplicitamente il tipo di dati nell'elemento del report utilizzando un'espressione.  
  
 Nell'elenco seguente sono descritti i casi in cui potrebbe essere necessario convertire i dati in un altro tipo di dati:  
  
-   Confronto del valore di un parametro del report di un tipo di dati con un campo del set di dati di un altro tipo di dati.  
  
-   Scrittura di espressioni di filtro che confrontano valori di tipi di dati differenti.  
  
-   Scrittura di espressioni di ordinamento che combinano campi di tipi di dati diversi.  
  
-   Scrittura di espressioni di raggruppamento che combinano campi di tipi di dati diversi.  
  
-   Conversione da un tipo di dati a un altro tipo di dati di un valore recuperato dall'origine dati.  
  
## <a name="determining-the-data-type-of-report-data"></a>Determinazione del tipo di dati del report  
 Per determinare il tipo di dati di un elemento del report è possibile scrivere un'espressione. Ad esempio, per visualizzare il tipo di dati del campo `MyField`, aggiungere l'espressione `=Fields!MyField.Value.GetType().ToString()`a una cella della tabella. Il risultato indica il tipo di dati CLR usato per rappresentare `MyField`, ad esempio **System.String** o **System.DateTime**.  
  
## <a name="converting-dataset-fields-to-a-different-data-type"></a>Conversione dei campi del set di dati in un altro tipo di dati  
 È inoltre possibile convertire i campi del set di dati prima di utilizzarli in un report. Nell'elenco seguente vengono descritte le modalità di conversione di un campo del set di dati esistente:  
  
-   Modificare la query del set di dati in modo da aggiungere un nuovo campo di query con i dati convertiti. Per le origini dati relazionali o multidimensionali, per eseguire la conversione vengono utilizzate le risorse dell'origine dati.  
  
-   Creare un campo calcolato basato su un campo del set di dati del report esistente scrivendo un'espressione che converte tutti i dati di una colonna del set di risultati in una nuova colonna con un tipo di dati differente. L'espressione seguente, ad esempio, converte il campo Year da un valore di tipo Integer in un valore di tipo String: `=CStr(Fields!Year.Value)`. Per altre informazioni, vedere [Aggiunta, modifica e aggiornamento di campi nel riquadro dei dati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
-   Controllare se l'estensione per l'elaborazione dati in uso include metadati per il recupero dei dati preformattati. Una query MDX di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] include ad esempio una proprietà estesa FORMATTED_VALUE per i valori del cubo già formattati durante l'elaborazione del cubo. Per altre informazioni, vedere [Proprietà di campo estese per un database di Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
## <a name="understanding-parameter-data-types"></a>Informazioni sui tipi di dati dei parametri  
 I parametri del report devono essere di uno dei cinque tipi di dati seguenti: Boolean, DateTime, Integer, Float o Text (anche noto come String). Quando una query del set di dati include parametri di query, i parametri del report vengono creati automaticamente e collegati ai parametri di query. Il tipo di dati predefinito per un parametro di report è String. Per modificare tale tipo di dati, selezionare il valore corretto nell'elenco a discesa **Tipo di dati** nella pagina **Generale** della finestra di dialogo **Proprietà parametri report** .  
  
> [!NOTE]  
>  I parametri di report di tipo DateTime non supportano i millisecondi. Sebbene sia possibile creare un parametro basato su valori che includono millisecondi, non è possibile selezionare un valore da un elenco a discesa di valori disponibili contenente valori di tipo Date o Time che includono millisecondi.  
  
## <a name="writing-expressions-that-convert-data-types-or-extract-parts-of-data"></a>Scrittura di espressioni che convertono tipi di dati o estraggono parti di dati  
 Quando si combinano testo e campi del set di dati utilizzando l'operatore di concatenazione (&), Common Language Runtime (CLR) fornisce in genere formati predefiniti. Per convertire in modo esplicito un parametro o un campo del set di dati in un tipo di dati specifico, è necessario utilizzare un metodo CLR o una funzione della libreria di runtime di Visual Basic.  
  
 Nella tabella seguente sono riportati alcuni esempi di conversione dei tipi di dati.  
  
|Tipo di conversione|Esempio|  
|------------------------|-------------|  
|Da DateTime a String|`=CStr(Fields!Date.Value)`|  
|Da String a DateTime|`=DateTime.Parse(Fields!DateTimeinStringFormat.Value)`|  
|Da String a DateTimeOffset|`=DateTimeOffset.Parse(Fields!DateTimeOffsetinStringFormat.Value)`|  
|Estrazione dell'anno|`=Year(Fields!TimeinStringFormat.Value)`<br /><br /> `-- or --`<br /><br /> `=Year(Fields!TimeinDateTimeFormat.Value)`|  
|Da Boolean a Integer|`=CInt(Parameters!BooleanField.Value)`<br /><br /> \- 1 è True e 0 è False.|  
|Da Boolean a Integer|`=System.Convert.ToInt32(Fields!BooleanFormat.Value)`<br /><br /> 1 è True e 0 è False.|  
|Solo la parte DateTime di un valore DateTimeOffset|`=Fields!MyDatetimeOffset.Value.DateTime`|  
|Solo la parte Offset di un valore DateTimeOffset|`=Fields!MyDatetimeOffset.Value.Offset`|  
  
 È inoltre possibile utilizzare la funzione Format per controllare il formato di visualizzazione del valore. Per altre informazioni, vedere [Funzioni (Visual Basic)](/dotnet/visual-basic/language-reference/functions/).  
  
## <a name="advanced-examples"></a>Esempi avanzati  
 Quando ci si connette a un'origine dati con un provider di dati che non fornisce il supporto di conversione per tutti i tipi di dati dell'origine dati, il tipo di dati predefinito per i tipi di origine dati non supportati corrisponde a String. Negli esempi seguenti sono fornite le soluzioni per i tipi di dati specifici restituiti come stringa.  
  
### <a name="concatenating-a-string-and-a-clr-datetimeoffset-data-type"></a>Concatenazione di una stringa e di un tipo di dati DateTimeOffset CLR  
 Per la maggior parte dei tipi di dati, CLR fornisce conversioni predefinite in modo da consentire la concatenazione di valori con tipi di dati diversi in una stringa mediante l'operatore &. Nell'espressione seguente, ad esempio, viene concatenato il testo "The date and time are: " con il campo del set di dati StartDate, che rappresenta un valore <xref:System.DateTime> : `="The date and time are: " & Fields!StartDate.Value`.  
  
 Per alcuni tipi di dati, potrebbe essere necessario includere la funzione ToString. Nell'espressione seguente, ad esempio, è riportato lo stesso esempio con il tipo di dati CLR <xref:System.DateTimeOffset>che include la data, l'ora e una differenza di fuso orario rispetto al fuso orario UTC: `="The time is: " & Fields!StartDate.Value.ToString()`.  
  
### <a name="converting-a-string-data-type-to-a-clr-datetime-data-type"></a>Conversione del tipo di dati String in un tipo di dati DateTime CLR  
 Se un'estensione per l'elaborazione dati non supporta tutti i tipi di dati definiti in un'origine dati, i dati possono essere recuperati come testo. Un valore del tipo di dati **datetimeoffset(7)** può essere, ad esempio, recuperato come dati di tipo String. A Perth, in Australia, il valore stringa per il 1 luglio 2008, alle 6:05:07.9999999 AM è simile al seguente:  
  
 `2008-07-01 06:05:07.9999999 +08:00`  
  
 In questo esempio viene mostrata la data (1 luglio 2008), seguita dall'ora espressa con una precisione di 7 cifre (06:05:07.9999999 AM), seguita da una differenza di fuso orario dall'ora UTC espressa in ore e minuti (più 8 ore e 0 minuti). Per l'esempio seguente, questo valore è stato inserito in un campo **String** denominato `MyDateTime.Value`.  
  
 Per convertire questi dati in uno o più valori CLR, è possibile adottare una delle strategie seguenti:  
  
-   In una casella di testo utilizzare un'espressione per estrarre parti della stringa. Ad esempio:  
  
    -   Nell'espressione seguente solo la parte relativa all'ora della differenza di fuso orario dall'ora UTC viene prima estratta e poi convertita in minuti: `=CInt(Fields!MyDateTime.Value.Substring(Fields!MyDateTime.Value.Length-5,2)) * 60`  
  
         Il risultato è `480`.  
  
    -   Nell'espressione seguente la stringa viene convertita in un valore di data e ora: `=DateTime.Parse(Fields!MyDateTime.Value)`  
  
         Se la stringa `MyDateTime.Value` include una differenza UTC, la funzione `DateTime.Parse` regola in primo luogo l'ora in base alla differenza UTC: 7 AM, - [`+08:00`] con l'ora UTC 11 PM. della notte precedente). La funzione `DateTime.Parse` applica quindi la differenza UTC del server di report locale e, se necessario, regola nuovamente l'ora in base all'ora legale. Ad esempio, a Redmond, Washington, la differenza tra ora locale e ora legale è `[-07:00]`, ovvero 7 ore prima delle 23.00. Il risultato è il valore **DateTime** seguente: `2007-07-06 04:07:07 PM` (6 luglio 2007 alle 16.07).  
  
 Per altre informazioni sulla conversione di stringhe in tipi di dati **DateTime** , vedere [Analisi delle stringhe di data e ora](https://go.microsoft.com/fwlink/?LinkId=89703), [Formattazione di data e ora per impostazioni cultura specifiche](https://go.microsoft.com/fwlink/?LinkId=89704)e [Scelta tra DateTime, DateTimeOffset e TimeZoneInfo](/dotnet/standard/datetime/choosing-between-datetime) in MSDN.  
  
-   Aggiungere un nuovo campo calcolato al set di dati del report che utilizza un'espressione per estrarre parti della stringa. Per altre informazioni, vedere [Aggiunta, modifica e aggiornamento di campi nel riquadro dei dati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
-   Modificare la query del set di dati del report per utilizzare le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per estrarre i valori di data e ora indipendentemente in modo da creare colonne separate. L'esempio seguente illustra come usare la funzione **DatePart** per aggiungere una colonna per l'anno e una colonna per il fuso orario UTC convertito in minuti:  
  
     `SELECT`  
  
     `MyDateTime,`  
  
     `DATEPART(year, MyDateTime) AS Year,`  
  
     `DATEPART(tz, MyDateTime) AS OffsetinMinutes`  
  
     `FROM MyDates`  
  
     Il set di risultati contiene tre colonne. La prima colonna indica la data e l'ora, la seconda indica l'anno e la terza indica la differenza UTC espressa in minuti. Nella riga seguente sono riportati dati di esempio:  
  
     `2008-07-01 06:05:07             2008                   480`  
  
 Per altre informazioni sui tipi di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) e [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Per altre informazioni sui tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vedere [Tipi di dati in Analysis Services](/analysis-services/multidimensional-models/olap-physical/data-types-in-analysis-services).  
  
## <a name="see-also"></a>Vedere anche  
 [Formattazione degli elementi del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)  
  
