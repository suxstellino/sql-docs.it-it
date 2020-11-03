---
title: Specificare un intervallo dell'asse (Generatore report) | Microsoft Docs
description: Informazioni su come modificare il numero di etichette e segni di graduazione sull'asse delle categorie (X) in un grafico impostando l'intervallo dell'asse in un report impaginato in Generatore report.
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: ae46712d-a5bf-44c0-9929-e30ccc1e7e33
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6b756d7db3a41787da2a9a1a30f1c82b9a72317c
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907179"
---
# <a name="specify-an-axis-interval-report-builder-and-ssrs"></a>Specificare un intervallo dell'asse (Generatore report e SSRS)
Informazioni su come modificare il numero di etichette e segni di graduazione sull'asse delle categorie (X) in un grafico impostando l'intervallo asse in un report impaginato di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] .
 
Sull'asse dei valori (di solito l'asse Y) gli intervalli forniscono una misura coerente dei punti dati nel grafico. 

Sull'asse delle categorie (di solito l'asse X), invece, in alcuni casi un intervallo asse automatico determina l'assenza di etichette dell'asse nelle categorie. È possibile specificare il numero di intervalli desiderato nella proprietà intervallo dell'asse. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] il numero degli intervalli viene calcolato in fase di esecuzione, in base ai dati nel set di risultati. Per altre informazioni su come vengono calcolati gli intervalli degli assi, vedere [Formattazione delle etichette degli assi di un grafico](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  

Per provare a impostare l'intervallo dell'asse con dati di esempio, vedere [Esercitazione: Aggiungere un istogramma al report (Generatore report)](../tutorial-add-a-column-chart-to-your-report-report-builder.md).
  
> [!NOTE]  
>  L'asse delle categorie è in genere l'asse orizzontale, ovvero l'asse X. Tuttavia, per i grafici a barre, l'asse delle categorie è l'asse verticale, ovvero l'asse y.  
>
> Questo argomento non si applica a:
>-   Valori di data o ora sull'asse delle categorie. Per impostazione predefinita, i valori **DateTime** vengono visualizzati come giorni. È possibile specificare un intervallo di data o ora diverso, ad esempio intervallo di mesi o di ore. Per altre informazioni, vedere [Formattazione delle etichette degli assi come date o valute](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md).  
>-  Grafici a torta, ad anello, a imbuto o a piramide che non includono assi. 
  
## <a name="to-show-all-the-category-labels-on-the-x-axis"></a>Per visualizzare tutte le etichette delle categorie sull'asse X  

In questo istogramma, l'intervallo di etichette orizzontali è impostato su Automatico.

![Screenshot dell'anteprima di un grafico a colonne di Generatore report con l'intervallo dell'asse X impostato su Automatico.](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-auto.png)
  
1.  Fare clic con il pulsante destro del mouse sull'asse delle categorie e scegliere **Proprietà asse orizzontale**.   

    ![Screenshot di un grafico a colonne di Generatore report che mostra come impostare le etichette dell'asse X.](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-labels.png)
  
2.  Nella scheda **Opzioni asse** della finestra di dialogo **Proprietà asse orizzontale** impostare **Intervallo** su **1** per visualizzare tutte le etichette dei gruppi di categorie. Per visualizzare tutte le etichette dei gruppi di categorie sull'asse X, digitare **2**. 

     ![Screenshot di un grafico a colonne di Generatore report che mostra come impostare l'intervallo dell'asse X su 1.](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-interval-one.png)
  
3. [!INCLUDE[clickOK](../../includes/clickok-md.md)]
     
     Nell'istogramma verranno visualizzate tutte le etichette dell'asse orizzontale.
     
     ![Screenshot dell'anteprima del grafico a colonne di Generatore report che mostra le etichette dell'asse X.](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-one.png)
     
     > [!NOTE]  
     >  Quando si imposta un intervallo dell'asse, tutte le funzionalità di assegnazione automatica di etichette vengono disabilitate. Se si specifica un valore per l'intervallo dell'asse, potrebbe verificarsi un comportamento imprevisto delle etichette, a seconda del numero di categorie presenti sull'asse delle categorie.  

## <a name="change-the-label-interval-in-properties-pane"></a>Modificare l'intervallo di etichette nel riquadro Proprietà

È anche possibile impostare l'intervallo di etichette nel riquadro Proprietà.

1.  Nella visualizzazione di progettazione report fare clic sul grafico, quindi selezionare le etichette dell'asse orizzontale.

3. Nel riquadro Proprietà impostare LabelInterval su **1**.

    ![Screenshot del grafico a colonne di Generatore report che mostra come impostare l'intervallo di etichette.](../../reporting-services/media/report-builder-column-chart-set-label-interval.png)

    Il grafico ha lo stesso aspetto nella visualizzazione Progettazione. 
    
5.  Fare clic su **Esegui** per visualizzare l'anteprima del report.

    ![Screenshot dell'anteprima del grafico a colonne di Generatore report che mostra l'intervallo di etichette impostato su 1.](../../reporting-services/media/report-builder-column-chart-label-interval-one-preview.png)
    
    Ora il grafico visualizza tutte le etichette.
  
## <a name="to-enable-a-variable-interval-calculation-on-an-axis"></a>Per abilitare il calcolo di intervalli variabili su un asse  

Per impostazione predefinita, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] imposta l'intervallo dell'asse su Automatico. Questa procedura descrive come ripristinare l'impostazione predefinita. 
  
1.  Fare clic con il pulsante destro del mouse sull'asse da modificare, quindi scegliere **Proprietà asse**. 
  
2.  Nella scheda **Opzioni asse** della finestra di dialogo **Proprietà asse orizzontale** impostare **Intervallo** su **Automatico**. Verrà visualizzato il numero ottimale di etichette di categorie che è possibile adattare lungo l'asse.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>Vedere anche  
 [Formattazione di un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Formattazione dei punti dati di un grafico (Generatore report e SSRS)](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [Ordinamento dei dati in un'area dati (Generatore report e SSRS)](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md)   
 [Finestra di dialogo Proprietà asse, Opzioni asse &#40;Generatore report e SSRS&#41;](/previous-versions/sql/)   
 [Specificare una scala logaritmica &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)   
 [Traccia di dati su un asse secondario &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/plot-data-on-a-secondary-axis-report-builder-and-ssrs.md)  
  
