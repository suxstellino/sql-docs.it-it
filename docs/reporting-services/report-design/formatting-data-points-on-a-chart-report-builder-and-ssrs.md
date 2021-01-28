---
title: Formattazione dei punti dati di un grafico (Generatore report) | Microsoft Docs
description: Informazioni sui diversi tipi di formattazione dei punti dati nei grafici dei report in Generatore report.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10248"
- sql13.rtp.rptdesigner.serieslabelproperties.general.f1
ms.assetid: 08ec3818-f63a-4e89-b52c-750e47f48b85
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0d9f3036e7d8c580e770ded7bbbec6e5f29b2750
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596907"
---
# <a name="formatting-data-points-on-a-chart-report-builder-and-ssrs"></a>Formattazione dei punti dati di un grafico (Generatore report e SSRS)
In un report impaginato di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , per punto dati si intende l'entità singola più piccola di un grafico. Nei grafici senza forme la rappresentazione dei punti dati dipende dal relativo tipo di grafico. Una serie di linee è ad esempio costituita da uno o più punti dati connessi. Nei grafici con forme i punti dati sono rappresentati dai singoli segmenti o sezioni che costituiscono l'intero grafico. In un grafico a torta, ad esempio, ogni parte è un punto dati. Per altre informazioni, vedere [Tipi di grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md).  
  
 Uno o più punti dati formano una serie. Per impostazione predefinita, tutte le opzioni di formattazione vengono applicate a tutti i punti dati di una serie. Se si desidera specificare proprietà per i singoli punti dati, è possibile indicare un campo o un'espressione sulla serie che consenta di formattare, in fase di esecuzione, il singolo punto desiderato in base al set di dati.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="adding-tooltips-and-drillthrough-actions-to-data-points"></a>Aggiunta di descrizioni comandi e di azioni drill-through ai punti dati  
 Per aggiungere descrizioni comandi a ogni punto dati è possibile impostare il valore della proprietà **ToolTip** sulla serie. Le descrizioni comandi consentono agli utenti la visualizzazione di informazioni correlate ai punti dati, quali il nome del gruppo, il valore del punto dati e la percentuale del punto dati rispetto alla serie totale. Per altre informazioni, vedere [Visualizzazione di descrizioni comandi in una serie &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md).  
  
 È inoltre possibile specificare un'azione drill-through per i punti dati sulla serie in modo da visualizzare un altro report o un URL. È possibile passare parametri per mostrare informazioni relative al punto dati selezionato. Per altre informazioni, vedere [Aggiungere un'azione drill-through a un report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-a-drillthrough-action-on-a-report-report-builder-and-ssrs.md).  
  
## <a name="highlighting-individual-data-points-in-a-series"></a>Evidenziazione di singoli punti dati in una serie  
 In qualsiasi grafico senza forme è possibile evidenziare i singoli punti dati specificando un'espressione per la proprietà Color. Per evidenziare ad esempio il valore del punto dati più elevato di una serie, denominato `MyField` , con un colore diverso da quello degli altri punti dati, l'espressione sarà simile alla seguente:  
  
 `=Iif(Fields!MyField.Value >= Max(Fields!MyField.Value, "MyDataSet"), "Red", "Green")`  
  
 Nell'esempio il valore massimo di `MyField` assumerà il colore rosso mentre tutti gli altri punti dati saranno evidenziati in verde. Quando si specifica un colore per la serie usando la relativa proprietà **Fill** , nel grafico vengono sostituiti i colori specificati nella tavolozza. Per altre informazioni, vedere [Formattazione dei colori delle serie in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md).  
  
## <a name="positioning-data-point-labels-on-a-chart"></a>Posizionamento di etichette dei punti dati in un grafico  
 Per tutti i tipi di grafico è possibile visualizzare etichette dei punti dati facendo clic con il pulsante destro del mouse sul grafico e quindi scegliendo **Mostra etichette dati**. La posizione delle etichette dei punti dati specificata dipende dal tipo di grafico:  
  
-   In un grafico a barre è possibile modificare la posizione dell'etichetta del punto dati usando l'attributo personalizzato **BarLabelStyle** , che può assumere quattro posizioni, ovvero Esterno, A sinistra, Al centro e A destra. Quando lo stile delle etichette del grafico a barre viene impostato su Esterno, le etichette vengono posizionate all'esterno della barra, purché quest'ultima rientri nell'area del grafico. Se l'etichetta non può essere posizionata esternamente alla barra e internamente all'area del grafico, viene inserita nella barra.  
  
-   In un grafico a torta è possibile modificare la posizione dell'etichetta del punto dati usando l'attributo personalizzato **PieLabelStyle** . Quando si posizionano le etichette dei punti dati in un grafico a torta, è necessario tenere presente diversi fattori, quali la dimensione del grafico stesso, lo spazio disponibile tra il grafico e la legenda corrispondente, nonché la dimensione delle etichette. Per altre informazioni, vedere [Visualizzare le etichette dei punti dati al di fuori di un grafico a torta &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md).  
  
-   In un grafico a piramide o a imbuto è possibile modificare la posizione delle etichette dei punti dati usando gli attributi personalizzati **FunnelLabelStyle** e **PyramidLabelStyle** . È possibile impostare questi attributi nel riquadro **Proprietà** dopo aver selezionato un tipo di grafico a piramide o a imbuto.  
  
-   Nei grafici in pila le etichette dei punti dati vengono sempre posizionate all'interno della serie e la proprietà **Position** sull'etichetta della serie viene ignorata.  
  
-   In tutti gli altri tipi di grafico è possibile modificare la posizione dell'etichetta del punto dati usando la proprietà **Position** sull'etichetta della serie. Per impostazione predefinita, nel grafico la posizione delle etichette dei punti dati viene calcolata automaticamente in modo da evitare conflitti tra le etichette. Quando si imposta un valore per la proprietà **Position**, tutte le etichette dei punti dati vengono inserite nella stessa posizione con il rischio che si sovrappongano. Usare questo approccio solo in presenza di un numero ridotto di punti dati.  
  
 Per altre informazioni, vedere [Posizionamento di etichette in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/position-labels-in-a-chart-report-builder-and-ssrs.md).  
  
## <a name="adding-keywords-for-data-point-labels-tooltips-and-legend-text"></a>Aggiunta di parole chiave per le etichette dei punti dati, le descrizioni comandi e per il testo legenda  
 Per rappresentare un elemento presente nel grafico è possibile utilizzare parole chiave specifiche con distinzione tra maiuscole e minuscole. Tali parole sono applicabili solo alle descrizioni comandi, al testo personalizzato della legenda e alle proprietà delle etichette dei punti dati. In molti casi una parola chiave del grafico presenta un'espressione semplice equivalente, con la differenza che la parola chiave è più veloce e più facile da digitare. Di seguito viene fornito un elenco delle parole chiave del grafico.  
  
|Parola chiave del grafico|Descrizione|Applicabile al tipo di grafico|Esempio di espressione semplice equivalente|  
|-------------------|-----------------|------------------------------|------------------------------------------------|  
|#VALY|Valore Y del punto dati.|Tutti|`=Fields!MyDataField.Value`|  
|#VALY2|Valore Y #2 del punto dati.|Con intervalli, a bolle|nessuno|  
|#VALY3|Valore Y #3 del punto dati.|Azionario, a candela|nessuno|  
|#VALY4|Valore Y #4 del punto dati.|Azionario, a candela|nessuno|  
|#SERIESNAME|Nome della serie.|Tutti|nessuno|  
|#LABEL|Etichetta del punto dati.|Tutti|nessuno|  
|#AXISLABEL|Etichetta del punto dati dell'asse.|Con forme|`=Fields!MyDataField.Value`|  
|#INDEX|Indice del punto dati.|Tutti|nessuno|  
|#PERCENT|Percentuale del valore Y del punto dati.|Tutti|`=FormatPercent(Fields!MyDataField.Value/Sum(Fields!MyDataField.Value, "MyDataSet"),2)`|  
|#TOTAL|Totale di tutti i valori Y della serie|Tutti|`=Sum(Fields!MyDataField.Value)`|  
|#LEGENDTEXT|Testo corrispondente al testo dell'elemento della legenda.|Tutti|nessuno|  
|#AVG|Media di tutti i valori Y della serie|Tutti|`=Avg(Fields!MyDataField.Value)`|  
|#MIN|Minimo di tutti i valori Y della serie|Tutti|`=Min(Fields!MyDataField.Value)`|  
|#MAX|Massimo di tutti i valori Y della serie|Tutti|`=Max(Fields!MyDataField.Value)`|  
|#FIRST|Primo di tutti i valori Y della serie|Tutti|`=First(Fields!MyDataField.Value)`|  
  
 Per formattare la parola chiave, racchiudere tra parentesi una stringa di formato di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Per specificare il valore del punto dati in una descrizione comandi come numero con due posizioni decimali, includere la stringa di formato "N2" tra parentesi, indicando ad esempio il valore "#VALY {N2}" per la proprietà **ToolTip** della serie. Per altre informazioni sulle stringhe di formato di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , vedere [Formattazione dei tipi di dati](/previous-versions/) sul sito MSDN. Per altre informazioni sulla formattazione dei numeri in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Formattazione di numeri e date &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md).  
  
 Per altre informazioni sull'aggiunta di parole chiave a un grafico, vedere [Visualizzazione di descrizioni comandi in una serie &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md) e [Modificare il testo di un elemento legenda &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md).  
  
## <a name="increasing-readability-in-a-chart-with-multiple-data-points"></a>Maggiore leggibilità in un grafico con più punti dati  
 Se nel grafico in uso sono presenti più serie, la leggibilità dei punti dati del grafico potrebbe ridursi. Quando si aggiungono più serie al grafico, è consigliabile usare una tecnica che consenta di individuare le procedure ottimali di lettura e interpretazione per ogni serie del grafico. Per altre informazioni, vedere [Più serie in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md).  
  
 Per semplicità, quando si usa un grafico con forme, aggiungere un solo campo dati e un solo campo categoria. Per altre informazioni, vedere [Grafici con forme &#40;Generatore report e SSRSS&#41;](../../reporting-services/report-design/shape-charts-report-builder-and-ssrs.md). Se il grafico necessita di più campi dati e di un campo categoria, è possibile modificare il tipo di grafico. È possibile fare clic con il pulsante destro del mouse sulla serie e scegliere **Modifica tipo di grafico**.  
  
## <a name="inserting-data-point-markers"></a>Inserimento di marcatore dei punti dati  
 Un marcatore del punto dati è un indicatore visivo usato per segnalare ogni punto dati di una serie. In un grafico a dispersione il marcatore viene usato per determinare la forma e la dimensione dei singoli punti dati. La dimensione del marcatore specificata dipende dal tipo di grafico. È possibile modificare la dimensione, il colore o lo stile del marcatore. I marcatori non sono disponibili per tipi di grafico con intervalli e forme o per sottotipi di grafico in pila.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Visualizzazione di descrizioni comandi in una serie &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
 [Visualizzare le etichette dei punti dati al di fuori di un grafico a torta &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)  
  
 [Visualizzazione di valori percentuale in un grafico a torta &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Formattazione di un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Formattazione delle etichette degli assi in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Grafici &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Formattazione delle etichette degli assi come date o valute &#40;Generatore report e SSRSSSRS&#41;](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [Esercitazione: Aggiungere un grafico a torta al report &#40;Generatore report&#41;](../../reporting-services/tutorial-add-a-pie-chart-to-your-report-report-builder.md)   
 [Esempi di espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
