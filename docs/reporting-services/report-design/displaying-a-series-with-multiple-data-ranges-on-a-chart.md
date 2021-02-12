---
title: Visualizzazione di una serie con più intervalli di dati in un grafico | Microsoft Docs
description: Individuare modi per migliorare la leggibilità dei grafici usando cambi di scala e filtri e separando gli intervalli di dati in Generatore report.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 45da3d39-278e-4760-a4b3-9932c9547cf2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b808f4b7641d11f98fe8a2be877b0485fbdff5b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042046"
---
# <a name="displaying-a-series-with-multiple-data-ranges-on-a-chart"></a>Visualizzazione di una serie con più intervalli di dati in un grafico

  Nel grafico verranno usati i valori minimo e massimo di una serie per calcolare la scala dell'asse. Se una serie nel grafico contiene più intervalli di dati, è possibile che i punti dati vengano nascosti e che solo alcuni di essi risultino facilmente visibili nel grafico. Si supponga ad esempio che in un report vengano visualizzati i totali delle vendite giornaliere per un periodo di 30 giorni.  
  
 ![Grafico con più intervalli di dati](../../reporting-services/report-design/media/rs-multipledatarangeschart.gif "Grafico con più intervalli di dati")  
  
 Per la maggior parte del mese, le vendite sono comprese tra 10 e 40. Una campagna di marketing per le vendite di una settimana ha tuttavia causato un improvviso aumento delle vendite all'inizio di aprile. Questa modifica nei dati di vendita produce una distribuzione non uniforme di punti dati che riduce la leggibilità complessiva del grafico.  
  
 È possibile aumentare la leggibilità in diversi modi:  
  
-   **Abilitare i cambi di scala**. Se i dati formano due o più set di intervalli di dati, usare un cambio di scala per rimuovere il gap tra gli intervalli. Un cambio di scala è una striscia disegnata attraverso l'area del tracciato per indicare un cambio tra i valori minimo e massimo di una serie.  
  
-   **Filtrare i valori non necessari**. Se si dispone di punti dati che nascondono l'intervallo di dati importante da visualizzare nel grafico, rimuovere i punti non desiderati usando un filtro del report. Per informazioni su come aggiungere un filtro al grafico in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Aggiungere filtri per set di dati, aree dati e gruppi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
-   **Tracciare ogni intervallo di dati come una serie separata per il confronto tra più serie**. Se si dispone di più di due intervalli di dati, dividere gli intervalli in serie separate. Per altre informazioni, vedere [Più serie in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="displaying-multiple-data-ranges-using-scale-breaks"></a>Visualizzazione di più intervalli di dati usando i cambi di scala  
 Quando si abilita un cambio di scala, viene calcolato il punto in cui disegnare una riga nel grafico. Per disegnare un cambio di scala, è necessario disporre di una separazione sufficiente tra gli intervalli. Per impostazione predefinita, un cambio di scala può essere aggiunto solo se è presente una separazione tra gli intervalli di dati pari ad almeno il 25% del grafico.  
  
 ![Grafico con cambio di scala](../../reporting-services/report-design/media/rs-multipledatarangeschart-scalebreak.gif "Grafico con cambio di scala")  
  
> [!NOTE]  
>  Non è possibile specificare il punto in cui posizionare un cambio di scala su un grafico. È tuttavia possibile modificare la modalità di calcolo del cambio di scala, come descritto più avanti in questo argomento.  
  
 Se un cambio di scala abilitato non viene visualizzato sebbene sia disponibile una distanza sufficiente tra gli intervalli di dati, è possibile impostare la proprietà CollapsibleSpaceThreshold su un valore minore di 25. La proprietà CollapsibleSpaceThreshold specifica la percentuale di spazio comprimibile richiesto tra gli intervalli di dati. Per altre informazioni, vedere [Aggiungere cambi di scala a un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-scale-breaks-to-a-chart-report-builder-and-ssrs.md).  
  
 È supportato un numero massimo di cinque cambi di scala per grafico. La visualizzazione di più di un cambio di scala può tuttavia rendere illeggibile il grafico. Se si dispone di più di due intervalli di dati, usare un metodo diverso per la visualizzazione di tali dati. Per altre informazioni, vedere [Più serie in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md).  
  
## <a name="unsupported-scale-break-scenarios"></a>Scenari in cui i cambi di scala non sono supportati  
 I cambi di scala non sono supportati negli scenari relativi ai grafici seguenti:  
  
-   Per il grafico è abilitata la funzionalità 3D.  
  
-   È stato specificato un asse dei valori logaritmico.  
  
-   I valori minimo e massimo dell'asse dei valori sono stati specificati in modo esplicito.  
  
-   Il grafico è di tipo polare, radar, a torta, ad anello, a imbuto, a piramide o un tipo qualsiasi di grafico in pila.  
  
 Un esempio di grafico con cambi di scala è disponibile come report di esempio. Per altre informazioni sul download di questo e di altri report di esempio, vedere la pagina relativa ai [report di esempio per Generatore report e Progettazione report](https://go.microsoft.com/fwlink/?LinkId=198283).  

## <a name="next-steps"></a>Passaggi successivi

[Più serie in un grafico](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md)   
[Formattazione di un grafico](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Effetti 3D, smussature e altri effetti in un grafico](../../reporting-services/report-design/chart-effects-3d-bevel-and-other-report-builder.md)   
[Grafici](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Finestra di dialogo Proprietà asse, Opzioni asse](/previous-versions/sql/)   
[Raccolta di piccole sezioni in un grafico a torta](../../reporting-services/report-design/collect-small-slices-on-a-pie-chart-report-builder-and-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)