---
title: Esempi di espressioni (Generatore report) | Microsoft Docs
description: Controllare il contenuto e l'aspetto del report usando funzioni predefinite, codice personalizzato, variabili di report e di gruppo e variabili definite dall'utente in Generatore report.
ms.date: 04/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- page breaks [Reporting Services], expressions
- green-bar reports [Reporting Services]
- Visual Basic [Reporting Services]
- functions [Reporting Services], examples
- custom code [Reporting Services]
- appearance of reports
- formatting reports [Reporting Services], expressions
- show/hide [Reporting Services]
- parameters [Reporting Services], expressions
- visibility [Reporting Services], expressions
- page headers [Reporting Services]
- page footers [Reporting Services]
- dates [Reporting Services], expressions
- expressions [Reporting Services], examples
ms.assetid: 87ddb651-a1d0-4a42-8ea9-04dea3f6afa4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 40a05ec1ed6b7ffa05132e0cefbd5157b7d3ce92
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596329"
---
# <a name="expression-examples-report-builder-and-ssrs"></a>Esempi di espressioni (Generatore report e SSRS)
Le espressioni vengono usate di frequente nei report impaginati di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per controllare il contenuto e l'aspetto del report. Vengono scritte in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] e possono includere funzioni predefinite, codice personalizzato, variabili di gruppo e di report e variabili definite dall'utente. Le espressioni iniziano con un segno di uguale (=). Per altre informazioni sull'editor espressioni e sui tipi di riferimenti che è possibile includere, vedere [Uso delle espressioni nei report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md) e [Aggiungere un'espressione &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-an-expression-report-builder-and-ssrs.md).  
  
> [!IMPORTANT]  
>  Quando RDL Sandboxing è abilitato, al momento della pubblicazione del report nel testo dell'espressione è possibile utilizzare solo determinati tipi e membri. Per altre informazioni, vedere [Enable and Disable RDL Sandboxing](../../reporting-services/report-server-sharepoint/enable-and-disable-rdl-sandboxing.md).  
  
In questo argomento vengono forniti alcuni esempi di espressioni che è possibile utilizzare per attività comuni in un report.  
  
-   [Funzioni di Visual Basic](#VisualBasicFunctions) Esempi di funzioni di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] di tipo data, stringa, conversione e condizionale.  
  
-   [Funzioni di report](#ReportFunctions) Esempi relativi a funzioni di aggregazione e altre funzioni di report predefinite.  
  
-   [Aspetto dei dati del report](#AppearanceofReportData) Esempi relativi alla modifica dell'aspetto di un report.  
  
-   [Proprietà](#Properties) Esempi relativi all'impostazione delle proprietà degli elementi del report per il controllo del formato o della visibilità.  
  
-   [Parametri](#Parameters) Esempi relativi all'utilizzo di parametri in un'espressione.  
  
-   [Codice personalizzato](#CustomCode) Esempi di codice personalizzato incorporato.  
  
Per esempi di espressioni per utilizzi specifici, vedere gli argomenti seguenti:  
  
-   [Esempi di espressioni di raggruppamento &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)  
  
-   [Esempi di equazioni di filtro &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)  
  
-   [Filtri di uso comune &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md)  
  
-   [Riferimenti a raccolte di variabili di report e di gruppo &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md)  
  
Per altre informazioni sulle espressioni semplici e complesse, per sapere dove è possibile usare le espressioni e quali tipi di riferimenti è possibile includere in un'espressione, vedere gli argomenti contenuti in [Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md). Per altre informazioni sul contesto in cui le espressioni vengono valutate per calcolare le aggregazioni, vedere [Ambito di espressioni per totali, aggregazioni e raccolte predefinite &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
Per ulteriori informazioni su come scrivere espressioni in cui vengono utilizzati molti operatori e funzioni che si utilizzano anche per esempi di espressione in questo argomento, ma nel contesto di scrittura di un report, vedere [Tutorial: Introducing Expressions](../../reporting-services/tutorial-introducing-expressions.md).  

  
## <a name="functions"></a>Funzioni  
 Molte espressioni incluse in un report contengono funzioni. Con queste funzioni è possibile formattare dati, applicare logica e accedere ai metadati del report. È possibile scrivere espressioni che usano funzioni della libreria run-time di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] e degli spazi dei nomi <xref:System.Convert> e <xref:System.Math>. È possibile aggiungere riferimenti a funzioni da altri assembly o da codice personalizzato. È anche possibile usare classi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], tra cui <xref:System.Text.RegularExpressions>.  
  
##  <a name="visual-basic-functions"></a><a name="VisualBasicFunctions"></a> Funzioni di Visual Basic  
 È possibile utilizzare le funzioni di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] per modificare i dati visualizzati nelle caselle di testo o utilizzati per parametri, proprietà o altre aree del report. In questa sezione vengono forniti esempi che illustrano alcune di queste funzioni. Per altre informazioni, vedere la pagina relativa ai [membri delle librerie di runtime di Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) in MSDN.  
  
 In [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sono disponibili molte opzioni di formato personalizzato, ad esempio per formati di data specifici. Per ulteriori informazioni, vedere [Formattazione dei tipi di dati](/previous-versions/) in MSDN.  
  
### <a name="math-functions"></a>Funzioni matematiche  
  
-   La funzione **Round** risulta utile per l'arrotondamento dei numeri al numero intero più vicino. L'espressione seguente comporta un arrotondamento del valore 1.3 a 1:  
  
    ```  
    = Round(1.3)  
    ```  
  
     È anche possibile scrivere un'espressione per arrotondare un valore a un multiplo specificato, come avviene con la funzione **MRound** di Excel. Moltiplicare il valore di un fattore che crea un numero intero, arrotondare il numero e dividere il risultato per lo stesso fattore. Per arrotondare ad esempio 1.3 al multiplo più vicino di .2 (1.4), utilizzare l'espressione seguente:  
  
    ```  
    = Round(1.3*5)/5  
    ```  
  
###  <a name="date-functions"></a><a name="DateFunctions"></a> Funzioni di data  
  
-   La funzione **Today** fornisce la data corrente. Questa espressione può essere utilizzata in una casella di testo per visualizzare la data nel report oppure in un parametro per filtrare i dati in base alla data corrente.  
  
    ```  
    =Today()  
    ```  
  
-   Usare la funzione **DateInterval** per estrarre una parte specifica di una data. Ecco alcuni parametri **DateInterval** validi:

    -   DateInterval.Second
    -   DateInterval.Minute
    -   DateInterval.Hour
    -   DateInterval.Weekday
    -   DateInterval.Day
    -   DateInterval.DayOfYear
    -   DateInterval.WeekOfYear
    -   DateInterval.Month
    -   DateInterval.Quarter
    -   DateInterval.Year

    Ad esempio, questa espressione mostrerà il numero della settimana nell'anno corrente per la data corrente:
  
    ```  
    =DatePart(DateInterval.WeekOfYear, today()) 
    ```  
  
-   La funzione **DateAdd** è utile per fornire un intervallo di date in base a un solo parametro. L'espressione seguente restituisce una data successiva di sei mesi alla data di un parametro denominato *StartDate*.  
  
    ```  
    =DateAdd(DateInterval.Month, 6, Parameters!StartDate.Value)  
    ```  
  
-   La funzione **Year** visualizza l'anno per una determinata data. È possibile utilizzare questa espressione per raggruppare date oppure per visualizzare l'anno come etichetta di un set di date. Questa espressione restituisce l'anno per un gruppo specifico di date di ordini di vendita. Per modificare le date è possibile utilizzare anche la funzione **Month** e altre funzioni. Per altre informazioni, vedere la documentazione di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].  
  
    ```  
    =Year(Fields!OrderDate.Value)  
    ```  
  
-   È possibile combinare le funzioni in un'espressione per personalizzare il formato. L'espressione seguente cambia il formato di una data da mese-giorno-anno in mese-settimana-numero settimana, ad esempio 12/23/2009 in dicembre settimana 3:  
  
    ```  
    =Format(Fields!MyDate.Value, "MMMM") & " Week " &   
    (Int(DateDiff("d", DateSerial(Year(Fields!MyDate.Value),   
    Month(Fields!MyDate.Value),1), Fields!FullDateAlternateKey.Value)/7)+1).ToString  
    ```  
  
     Se utilizzata come campo calcolato in un set di dati, questa espressione consente di aggregare i valori di un grafico in base alla settimana in ogni mese.  
  
-   L'espressione seguente applica al valore SellStartDate il formato MMM-AA. Il campo SellStartDate è un tipo di dati datetime.  
  
    ```  
    =FORMAT(Fields!SellStartDate.Value, "MMM-yy")  
    ```  
  
-   L'espressione seguente applica al valore SellStartDate il formato gg/MM/aaaa. Il campo SellStartDate è un tipo di dati datetime.  
  
    ```  
    =FORMAT(Fields!SellStartDate.Value, "dd/MM/yyyy")  
    ```  
  
-   La funzione **CDate** converte il valore in una data. La funzione **Now** restituisce un valore di data che contiene la data e l'ora correnti in base al sistema. **DateDiff** restituisce un valore Long che specifica il numero di intervalli di tempo tra due valori di data.  
  
     Nell'esempio seguente viene visualizzata la data di inizio dell'anno in corso  
  
    ```  
    =DateAdd(DateInterval.Year,DateDiff(DateInterval.Year,CDate("01/01/1900"),Now()),CDate("01/01/1900"))  
    ```  
  
-   Nell'esempio seguente viene visualizzata la data di inizio del mese precedente basato sul mese corrente.  
  
    ```  
    =DateAdd(DateInterval.Month,DateDiff(DateInterval.Month,CDate("01/01/1900"),Now())-1,CDate("01/01/1900"))  
    ```  
  
-   L'espressione seguente genera gli anni di intervallo tra SellStartDate e LastReceiptDate. Questi campi si trovano in due set di dati diversi, DataSet1 e DataSet2. La funzione di aggregazione [First &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/report-builder-functions-first-function.md) restituisce il primo valore di SellStartDate in DataSet1 e il primo valore di LastReceiptDate in DataSet2.  
  
    ```  
    =DATEDIFF("yyyy", First(Fields!SellStartDate.Value, "DataSet1"), First(Fields!LastReceiptDate.Value, "DataSet2"))  
    ```  
  
-   La funzione **DatePart** restituisce un valore Integer contenente il componente specificato di un determinato valore Data. L'espressione seguente restituisce l'anno del primo valore di SellStartDate in DataSet1. L'ambito del set di dati è specificato poiché il report include più set di dati.  
  
    ```  
    =Datepart("yyyy", First(Fields!SellStartDate.Value, "DataSet1"))  
  
    ```  
  
-   La funzione **DateSerial** restituisce un valore Data che rappresenta un anno, un mese, un giorno specificato, con le informazioni sull'ora impostate su mezzanotte. Nell'esempio seguente viene visualizzata la data di fine del mese precedente, basato sul mese corrente.  
  
    ```  
    =DateSerial(Year(Now()), Month(Now()), "1").AddDays(-1)  
    ```  
  
-   Le espressioni seguenti mostrano diverse date, in base a un valore di parametro di data selezionato dall'utente.  
  
|Descrizione esempio|Esempio|  
|-------------------------|-------------|  
|Ieri|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value)-1)`|  
|Due giorni fa|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value)-2)`|  
|Un mese fa|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value)-1,Day(Parameters!TodaysDate.Value))`|  
|Due mesi fa|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value)-2,Day(Parameters!TodaysDate.Value))`|  
|Un anno fa|`=DateSerial(Year(Parameters!TodaysDate.Value)-1,Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value))`|  
|Due anni fa|`=DateSerial(Year(Parameters!TodaysDate.Value)-2,Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value))`|  
  
###  <a name="string-functions"></a><a name="StringFunctions"></a> Funzioni per i valori stringa  
  
-   È possibile combinare più campi utilizzando operatori di concatenazione e costanti di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] . L'espressione seguente restituisce due campi, ognuno su una riga distinta nella stessa casella di testo:  
  
    ```  
    =Fields!FirstName.Value & vbCrLf & Fields!LastName.Value   
    ```  
  
-   È possibile formattare date e numeri in una stringa tramite la funzione **Format** . L'espressione seguente visualizza il valore dei parametri *StartDate* e *EndDate* nel formato di data estesa:  
  
    ```  
    =Format(Parameters!StartDate.Value, "D") & " through " &  Format(Parameters!EndDate.Value, "D")    
    ```  
  
     Se la casella di testo contiene solo una data o un numero, per applicare la formattazione è consigliabile usare la proprietà di formattazione della casella di testo, anziché la funzione **Format** all'interno della casella di testo.  
  
-   Le funzioni **Right**, **Len** e **InStr** sono utili per ottenere sottostringhe, ad esempio per ottenere il solo nome utente dalla stringa *DOMAIN*\\*username* . L'espressione seguente restituisce la parte della stringa a destra del carattere barra rovesciata (\\) da un parametro denominato *User*:  
  
    ```  
    =Right(Parameters!User.Value, Len(Parameters!User.Value) - InStr(Parameters!User.Value, "\"))  
    ```  
  
     L'espressione seguente restituisce lo stesso valore dell'espressione precedente, ma usando membri della classe [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.String> invece di funzioni di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]:  
  
    ```  
    =Parameters!User.Value.Substring(Parameters!User.Value.IndexOf("\")+1, Parameters!User.Value.Length-Parameters!User.Value.IndexOf("\")-1)  
    ```  
  
-   È possibile visualizzare i valori selezionati di un parametro multivalore. Nell'esempio seguente viene utilizzata la funzione **Join** per concatenare i valori selezionati del parametro *MySelection* in un'unica stringa che può essere impostata come espressione per il valore di una casella di testo in un elemento del report.  
  
    ```  
    = Join(Parameters!MySelection.Value)  
    ```  
  
     Nell'esempio riportato di seguito viene eseguita la stessa operazione dell'esempio precedente e viene visualizzata una stringa di testo prima dell'elenco dei valori selezionati.  
  
    ```  
    ="Report for " & JOIN(Parameters!MySelection.Value, " & ")  
  
    ```  
  
-   Le funzioni **Regex** della classe <xref:System.Text.RegularExpressions> di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sono utili per modificare il formato di stringhe esistenti, ad esempio per formattare un numero di telefono. L'espressione seguente usa la funzione **Replace** per modificare il formato di un numero telefonico di dieci cifre in un campo da "*nnn*-*nnn*-*nnnn*" a "(*nnn*) *nnn*-*nnnn*":  
  
    ```  
    =System.Text.RegularExpressions.Regex.Replace(Fields!Phone.Value, "(\d{3})[ -.]*(\d{3})[ -.]*(\d{4})", "($1) $2-$3")  
    ```  
  
    > [!NOTE]  
    >  Verificare che nel valore di Fields!Phone.Value non siano contenuti spazi aggiuntivi e che sia di tipo <xref:System.String>.  
  
### <a name="lookup"></a>Ricerca  
  
-   Specificando un campo chiave è possibile usare la funzione **Lookup** per recuperare un valore da un set di dati per una relazione uno-a-uno, ad esempio una coppia chiave-valore. Nell'espressione seguente viene visualizzato il nome prodotto da un set di dati ("Product"), in base all'identificatore del prodotto con cui eseguire la corrispondenza:  
  
    ```  
    =Lookup(Fields!PID.Value, Fields!ProductID.Value, Fields!ProductName.Value, "Product")  
    ```  
  
### <a name="lookupset"></a>LookupSet  
  
-   Specificando un campo chiave è possibile usare la funzione **LookupSet** per recuperare un set di valori da un set di dati per una relazione uno-a-molti. Ad esempio una persona può disporre di più numeri di telefono. Nell'esempio seguente, si supponga che nel set di dati PhoneList sia contenuto un identificatore della persona e un numero di telefono in ogni riga. Tramite **LookupSet** viene restituita una matrice di valori. L'espressione seguente combina i valori restituiti in un'unica stringa e visualizza l'elenco di numeri di telefono per la persona specificata da ContactID:  
  
    ```  
    =Join(LookupSet(Fields!ContactID.Value, Fields!PersonID.Value, Fields!PhoneNumber.Value, "PhoneList"),",")  
    ```  
  
###  <a name="conversion-functions"></a><a name="ConversionFunctions"></a> Funzioni di conversione  
 È possibile utilizzare le funzioni di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] per convertire un campo da un tipo di dati a un altro. Le funzioni di conversione possono essere utilizzate per convertire il tipo di dati predefinito di un campo nel tipo di dati necessario per i calcoli oppure per combinare testo.  
  
-   L'espressione seguente converte la costante 500 nel tipo Decimal, in modo da confrontarla con un tipo di dati money di [!INCLUDE[tsql](../../includes/tsql-md.md)] nel campo Valore per un'espressione di filtro.  
  
    ```  
    =CDec(500)  
    ```  
  
-   L'espressione seguente visualizza il numero di valori selezionati per il parametro multivalore *MySelection*.  
  
    ```  
    =CStr(Parameters!MySelection.Count)  
    ```  
  
###  <a name="decision-functions"></a><a name="DecisionFunctions"></a> Funzioni condizionali  
  
-   La funzione **Iif** restituisce uno di due valori a seconda che l'espressione sia True o False. Nell'espressione seguente viene usata la funzione **Iif** per restituire un valore booleano **True** se il valore `LineTotal` è maggiore di 100. In caso contrario, viene restituito **False**:  
  
    ```  
    =IIF(Fields!LineTotal.Value > 100, True, False)  
    ```  
  
-   È possibile usare più funzioni **IIF** , note anche come funzioni IIF nidificate, per restituire uno di tre valori possibili a seconda del valore di `PctComplete`. L'espressione seguente può essere inserita nel colore di riempimento di una casella di testo per modificare il colore di sfondo in base al valore della casella di testo.  
  
    ```  
    =IIF(Fields!PctComplete.Value >= 10, "Green", IIF(Fields!PctComplete.Value >= 1, "Blue", "Red"))  
    ```  
  
     I valori maggiori o uguali a 10 vengono visualizzati con uno sfondo verde, quelli compresi tra 1 e 9 con uno sfondo blu e quelli minori di 1 con uno sfondo rosso.  
  
-   Per ottenere la stessa funzionalità, è anche possibile utilizzare la funzione **Switch** . La funzione **Switch** risulta utile quando è necessario testare tre o più condizioni. La funzione **Switch** restituisce il valore associato alla prima espressione in una serie che restituisce True:  
  
    ```  
    =Switch(Fields!PctComplete.Value >= 10, "Green", Fields!PctComplete.Value >= 1, "Blue", Fields!PctComplete.Value = 1, "Yellow", Fields!PctComplete.Value <= 0, "Red")  
    ```  
  
     I valori maggiori o uguali a 10 vengono visualizzati con uno sfondo verde, quelli compresi tra 1 e 9 con uno sfondo blu, quelli uguali a 1 con uno sfondo giallo e quelli minori o uguali a 0 con uno sfondo rosso.  
  
-   Viene verificato il valore del campo `ImportantDate` e viene restituito "Red" se è antecedente a una settimana e "Blue" in caso contrario. Questa espressione può essere utilizzata per controllare la proprietà Color di una casella di testo in un elemento del report:  
  
    ```  
    =IIF(DateDiff("d",Fields!ImportantDate.Value, Now())>7,"Red","Blue")  
    ```  
  
-   Viene verificato il valore del campo `PhoneNumber` e restituito "No Value" se **null** (**Nothing** in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]). In caso contrario, viene restituito il valore del numero di telefono. Questa espressione può essere utilizzata per controllare il valore di una casella di testo in un elemento del report.  
  
    ```  
    =IIF(Fields!PhoneNumber.Value Is Nothing,"No Value",Fields!PhoneNumber.Value)  
    ```  
  
-   Viene verificato il valore del campo `Department` e viene restituito il nome di un sottoreport o **null** (**Nothing** in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]). Questa espressione può essere utilizzata per i sottoreport drill-through condizionali.  
  
    ```  
    =IIF(Fields!Department.Value = "Development", "EmployeeReport", Nothing)  
    ```  
  
-   Viene verificato se il valore di un campo è Null. Questa espressione può essere utilizzata per controllare la proprietà **Hidden** di un elemento immagine del report. Nell'esempio seguente l'immagine specificata dal campo [LargePhoto] viene visualizzata solo se il valore del campo non è Null.  
  
    ```  
    =IIF(IsNothing(Fields!LargePhoto.Value),True,False)  
    ```  
  
-   La funzione **MonthName** restituisce il valore di stringa contenente il nome del mese specificato. Nell'esempio seguente viene visualizzato ND nel campo Mese quando il campo contiene il valore 0.  
  
    ```  
    IIF(Fields!Month.Value=0,"NA",MonthName(IIF(Fields!Month.Value=0,1,Fields!Month.Value)))  
  
    ```  
  
##  <a name="report-functions"></a><a name="ReportFunctions"></a> Funzioni di report  
 In un'espressione è possibile aggiungere un riferimento a funzioni per i report aggiuntive tramite le quali vengono modificati i dati di un report. In questa sezione vengono forniti esempi di due funzioni per i report. Per altre informazioni sulle funzioni di report ed esempi, vedere [Riferimento a funzioni di aggregazione &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md).  
  
###  <a name="sum"></a><a name="Sum"></a> Sum  
  
-   La funzione **Sum** consente di calcolare il totale dei valori di un gruppo o di un'area dati. Può risultare utile nell'intestazione o nel piè di pagina di un gruppo. L'espressione seguente visualizza la somma dei dati del gruppo o dell'area dati Order:  
  
    ```  
    =Sum(Fields!LineTotal.Value, "Order")  
    ```  
  
-   È possibile utilizzare la funzione **Sum** anche per i calcoli di aggregazione condizionali. Se ad esempio un set di dati include un campo denominato State, con i valori possibili Not Started, Started e Finished, l'espressione seguente, se inserita nell'intestazione di un gruppo, calcola la somma aggregata solo per il valore Finished:  
  
    ```  
    =Sum(IIF(Fields!State.Value = "Finished", 1, 0))  
    ```  
  
###  <a name="rownumber"></a><a name="RowNumber"></a> RowNumber  
  
-   La funzione **RowNumber** , se utilizzata in una casella di testo in un'area dati, visualizza il numero di riga di ogni istanza della casella di testo in cui compare l'espressione. Questa funzione può essere utile per numerare le righe di una tabella, ma anche per attività più complesse, ad esempio per l'inserimento di interruzioni di pagina in base al numero di righe. Per ulteriori informazioni, vedere [Interruzioni di pagina](#PageBreaks) di seguito in questo argomento.  
  
     L'ambito specificato per **RowNumber** controlla quando inizia la rinumerazione. La parola chiave **Nothing** indica che il conteggio inizierà dalla prima riga dell'area dati più esterna. Per iniziare il conteggio all'interno di aree dati nidificate, utilizzare il nome dell'area dati. Per iniziare il conteggio in un gruppo, utilizzare il nome del gruppo.  
  
    ```  
    =RowNumber(Nothing)  
    ```  
  
##  <a name="appearance-of-report-data"></a><a name="AppearanceofReportData"></a> Aspetto dei dati del report  
 È possibile utilizzare le espressioni per modificare l'aspetto dei dati in un report. È possibile, ad esempio, visualizzare i valori di due campi in una sola casella di testo, visualizzare informazioni sul report o intervenire sulla modalità di inserimento delle interruzioni di pagina nel report.  
  
###  <a name="page-headers-and-footers"></a><a name="PageHeadersandFooters"></a> Intestazioni di pagina e piè di pagina  
 Se si desidera che nel piè di pagina del report vengano visualizzati il nome del report e il numero di pagina, è possibile utilizzare le espressioni seguenti:  
  
-   L'espressione seguente restituisce il nome del report e la data di esecuzione. Può essere inserita in una casella di testo nel piè di pagina del report oppure nel corpo del report. Per la formattazione dell'ora viene utilizzata la stringa di formattazione di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] per la data breve:  
  
    ```  
    =Globals.ReportName & ", dated " & Format(Globals.ExecutionTime, "d")  
    ```  
  
-   L'espressione seguente, inserita in una casella di testo nel piè di pagina di un report, restituisce il numero di pagina e le pagine totali del report:  
  
    ```  
    =Globals.PageNumber & " of " & Globals.TotalPages  
    ```  
  
 Negli esempi seguenti viene illustrato come visualizzare il primo e l'ultimo valore di una pagina nell'intestazione di pagina. Si suppone che sia presente un'area dati contenente una casella di testo denominata `LastName`.  
  
-   L'espressione seguente, inserita in una casella di testo a sinistra dell'intestazione di pagina, restituisce il primo valore della casella di testo `LastName` nella pagina:  
  
    ```  
    =First(ReportItems("LastName").Value)  
    ```  
  
-   L'espressione seguente, se inserita in una casella di testo a destra dell'intestazione di pagina, restituisce l'ultimo valore della casella di testo `LastName` nella pagina:  
  
    ```  
    =Last(ReportItems("LastName").Value)  
    ```  
  
 Nell'esempio seguente viene illustrato come visualizzare un totale di pagina. Si suppone che sia presente un'area dati contenente una casella di testo denominata `Cost`.  
  
-   L'espressione seguente, inserita nell'intestazione o nel piè di pagina, restituisce la somma dei valori presenti nella casella di testo `Cost` nella pagina:  
  
    ```  
    =Sum(ReportItems("Cost").Value)  
    ```  
  
> [!NOTE]  
>  In un'intestazione o piè di pagina è possibile fare riferimento a un solo elemento del report in ogni espressione. Inoltre, nelle espressioni di intestazione e piè di pagina è possibile fare riferimento al nome della casella di testo, ma non all'espressione di dati effettiva al suo interno.  
  
###  <a name="page-breaks"></a><a name="PageBreaks"></a> Interruzioni di pagina  
 In alcuni report può essere necessario inserire un'interruzione di pagina alla fine di un numero di righe specificato anziché alla fine di gruppi o elementi del report oppure in aggiunta a questi. A tale scopo, creare un gruppo che contiene i gruppi o i record di dettaglio desiderati, aggiungere un'interruzione di pagina al gruppo, quindi aggiungere un'espressione di raggruppamento per eseguire il raggruppamento in base al numero di righe specificato.  
  
-   L'espressione seguente, se inserita nell'espressione di raggruppamento, assegna un numero a ogni set di 25 righe. Se si definisce un'interruzione di pagina per il gruppo, si ottiene un'interruzione di pagina ogni 25 righe.  
  
    ```  
    =Ceiling(RowNumber(Nothing)/25)  
    ```  
  
     Per consentire all'utente di impostare un valore relativo al numero di righe per pagina, creare un parametro denominato `RowsPerPage` su cui basare l'espressione di raggruppamento, come illustrato nell'espressione seguente:  
  
    ```  
    =Ceiling(RowNumber(Nothing)/Parameters!RowsPerPage.Value)  
    ```  
  
     Per altre informazioni sull'impostazione di interruzioni di pagina per un gruppo, vedere [Aggiunta di un'interruzione di pagina &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-a-page-break-report-builder-and-ssrs.md).  
  
##  <a name="properties"></a><a name="Properties"></a> Proprietà  
 Le espressioni non vengono utilizzate solo per visualizzare dati in caselle di testo. Possono essere utilizzate anche per modificare la modalità di applicazione delle proprietà agli elementi del report. È possibile modificare le informazioni sullo stile di un elemento del report oppure modificarne la visibilità.  
  
###  <a name="formatting"></a><a name="Formatting"></a> Formattazione  
  
-   L'espressione seguente, se usata nella proprietà Color di una casella di testo, cambia il colore del testo a seconda del valore del campo `Profit` :  
  
    ```  
    =Iif(Fields!Profit.Value < 0, "Red", "Black")  
    ```  
  
     È anche possibile utilizzare la variabile oggetto [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]`Me`. Questa variabile consente di fare riferimento in un altro modo al valore di una casella di testo.  
  
     `=Iif(Me.Value < 0, "Red", "Black")`  
  
-   L'espressione seguente, se usata nella proprietà BackgroundColor di un elemento del report in un'area dati, alterna il verde chiaro e il bianco come colore di sfondo di ogni riga:  
  
    ```  
    =Iif(RowNumber(Nothing) Mod 2, "PaleGreen", "White")  
    ```  
  
     Se si utilizza un'espressione per un ambito specifico, potrebbe essere necessario indicare il set di dati per la funzione di aggregazione.  
  
    ```  
    =Iif(RowNumber("Employees") Mod 2, "PaleGreen", "White")  
    ```  
  
> [!NOTE]  
>  I colori disponibili provengono dall'enumerazione KnownColor di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .  
  
### <a name="chart-colors"></a>Colori del grafico  
 Per specificare i colori per un grafico con forme, è possibile utilizzare codice personalizzato per controllare l'ordine in base al quale viene eseguito il mapping dei colori ai valori dei punti dati. In questo modo è possibile utilizzare colori coerenti per più grafici con gli stessi gruppi di categorie. Per altre informazioni, vedere [Specifica di colori coerenti in più grafici con forme &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md).  
  
###  <a name="visibility"></a><a name="Visibility"></a> Visibilità  
 È possibile visualizzare e nascondere elementi in un report utilizzando le proprietà di visibilità per gli elementi del report. In un'area dati, ad esempio una tabella, è possibile nascondere inizialmente le righe di dettaglio in base al valore di un'espressione.  
  
-   L'espressione seguente, se utilizzata per la visibilità iniziale delle righe di dettaglio in un gruppo, visualizza le righe di dettaglio per tutte le vendite con un valore superiore al 90 percento nel campo `PctQuota` :  
  
    ```  
    =Iif(Fields!PctQuota.Value>.9, False, True)  
    ```  
  
-   L'espressione seguente, se impostata nella proprietà Hidden di una tabella, visualizza la tabella solo se contiene più di 12 righe:  
  
    ```  
    =IIF(CountRows()>12,false,true)  
    ```  
  
-   L'espressione seguente, se impostata nella proprietà **Hidden** di una colonna, visualizza la colonna solo se il campo esiste nel set di dati del report dopo il recupero dei dati dall'origine dati:  
  
    ```  
    =IIF(Fields!Column_1.IsMissing, true, false)  
    ```  
  
###  <a name="urls"></a><a name="Hyperlinks"></a> URL  
 È possibile personalizzare gli URL utilizzando i dati del report e inoltre controllare in base a condizioni specifiche se gli URL vengono aggiunti come azione per una casella di testo.  
  
-   L'espressione seguente, se utilizzata come azione per una casella di testo, genera un URL personalizzato in cui è specificato il campo del set di dati `EmployeeID` come parametro.  
  
    ```  
    ="https://adventure-works/MyInfo?ID=" & Fields!EmployeeID.Value  
    ```  
  
     Per altre informazioni, vedere [Aggiunta di un collegamento ipertestuale a un URL &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-a-hyperlink-to-a-url-report-builder-and-ssrs.md).  
  
-   L'espressione seguente controlla in base a specifiche condizioni se aggiungere un URL in una casella di testo. L'espressione dipende da un parametro denominato `IncludeURLs` che consente all'utente di decidere se includere o meno URL attivi in un report. Questa espressione viene impostata come azione per una casella di testo. Se si imposta il parametro su False e quindi si visualizza il report, è possibile esportare il report in Microsoft Excel senza collegamenti ipertestuali.  
  
    ```  
    =IIF(Parameters!IncludeURLs.Value,"https://adventure-works.com/productcatalog",Nothing)  
    ```  
  
##  <a name="report-data"></a><a name="ReportData"></a> Dati del report  
 È possibile utilizzare le espressioni per modificare i dati utilizzati nei report. È possibile fare riferimento a parametri e ad altre informazioni dei report, nonché modificare la query utilizzata per recuperare i dati per il report.  
  
###  <a name="parameters"></a><a name="Parameters"></a> Parametri  
 È possibile utilizzare espressioni in un parametro per modificare il valore predefinito del parametro. Ad esempio, è possibile utilizzare un parametro per filtrare i dati relativi a un utente specifico sulla base dell'ID utente utilizzato per eseguire il report.  
  
-   L'espressione seguente, se utilizzata come valore predefinito di un parametro, recupera l'ID utente della persona che esegue il report:  
  
    ```  
    =User!UserID  
    ```  
  
-   Per fare riferimento a un parametro incluso in un parametro di query, un'espressione di filtro, una casella di testo o un'altra area del report, utilizzare la raccolta globale **Parameters** . In questo esempio si presuppone che il parametro sia denominato *Department*:  
  
    ```  
    =Parameters!Department.Value  
    ```  
  
-   I parametri possono essere creati in un report ma impostati come nascosti. Quando il report viene eseguito nel server di report, il parametro non viene visualizzato sulla barra degli strumenti e il lettore del report non può modificare il valore predefinito. È possibile utilizzare un parametro nascosto impostato su un valore predefinito come costante personalizzata. È possibile utilizzare questo valore in qualsiasi espressione, anche in un'espressione di campo. L'espressione seguente identifica il campo specificato dal valore del parametro predefinito per il parametro denominato *ParameterField*:  
  
    ```  
    =Fields(Parameters!ParameterField.Value).Value  
    ```  
  
##  <a name="custom-code"></a><a name="CustomCode"></a> Codice personalizzato  
 È possibile utilizzare codice personalizzato in un report. Il codice personalizzato può essere incorporato in un report o archiviato in un assembly personalizzato utilizzato nel report. Per altre informazioni sul codice personalizzato, vedere [Riferimenti a codice personalizzato e ad assembly in espressioni in Progettazione report &#40;SSRS&#41;](../../reporting-services/report-design/custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs.md).  
  
### <a name="using-group-variables-for-custom-aggregation"></a>Utilizzo delle variabili di gruppo per l'aggregazione personalizzata  
 È possibile inizializzare il valore di una variabile di gruppo locale all'interno di un particolare ambito del gruppo e quindi includere un riferimento a tale variabile nelle espressioni. Una delle modalità di utilizzo di una variabile di gruppo con codice personalizzato consiste nell'implementare un'aggregazione personalizzata. Per ulteriori informazioni, vedere [Using Group Variables in Reporting Services 2008 for Custom Aggregation](/archive/blogs/robertbruckner/).  
  
 Per altre informazioni sulle variabili, vedere [Riferimenti a raccolte di variabili di report e di gruppo &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md).  
  
## <a name="suppressing-null-or-zero-values-at-run-time"></a>Eliminazione di valori Null o zero in fase di esecuzione  
 Alcuni valori di un'espressione possono restituire un valore Null o non definito in fase di elaborazione del report. In questo modo possono verificarsi errori di run-time che generano la visualizzazione di **#Errore** nella casella di testo anziché dell'espressione valutata. Questo comportamento influisce in modo particolare sulla funzione **IIF** in quanto, a differenza di un'istruzione If-Then-Else, ogni parte dell'istruzione **IIF** (incluse le chiamate di funzione) viene valutata prima di essere passata alla routine che verifica se il risultato è **true** o **false**. L'istruzione `=IIF(Fields!Sales.Value is NOTHING, 0, Fields!Sales.Value)` genera **#Errore** nel report visualizzabile se `Fields!Sales.Value` è NOTHING.  
  
 Per evitare questa condizione, utilizzare una delle strategie seguenti:  
  
-   Impostare il numeratore su 0 e il denominatore su 1 se il valore per il campo B è 0 o non definito; in caso contrario, impostare il numeratore sul valore per il campo A e il denominatore sul valore per il campo B.  
  
    ```  
    =IIF(Field!B.Value=0, 0, Field!A.Value / IIF(Field!B.Value =0, 1, Field!B.Value))  
    ```  
  
-   Utilizzare una funzione di codice personalizzata per restituire il valore per l'espressione. Nell'esempio seguente viene restituita la differenza in percentuale tra un valore corrente e uno precedente. Questa funzione può essere usata per calcolare la differenza tra due valori successivi e consente di gestire il caso limite del primo confronto (quando non è disponibile alcun valore precedente) e i casi in cui il valore precedente o il valore corrente sia **null** (**Nothing** in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]).  
  
    ```  
    Public Function GetDeltaPercentage(ByVal PreviousValue, ByVal CurrentValue) As Object  
        If IsNothing(PreviousValue) OR IsNothing(CurrentValue) Then  
            Return Nothing  
        Else if PreviousValue = 0 OR CurrentValue = 0 Then  
            Return Nothing  
        Else   
            Return (CurrentValue - PreviousValue) / CurrentValue  
        End If  
    End Function  
    ```  
  
     Nell'espressione seguente viene mostrato come chiamare questo codice personalizzato da una casella di testo, per il contenitore "ColumnGroupByYear" (area dati o gruppo).  
  
    ```  
    =Code.GetDeltaPercentage(Previous(Sum(Fields!Sales.Value),"ColumnGroupByYear"), Sum(Fields!Sales.Value))  
    ```  
  
     In questo modo si evitano eccezioni in fase di esecuzione ed è ora possibile usare un'espressione come `=IIF(Me.Value < 0, "red", "black")` nella proprietà **Color** della casella di testo a seconda se i valori sono maggiori o minori di 0.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempi di equazioni di filtro &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)   
 [Esempi di espressioni di raggruppamento &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)   
 [Utilizzo delle espressioni nei report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Filtri di uso comune &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md)  
  
