---
title: Tracciare i dati su un asse secondario (Generatore report) | Microsoft Docs
description: Informazioni sugli usi per il tipo di asse secondario per il confronto di due intervalli di dati distinti in Generatore report.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 094f39bf-3634-4852-9fc3-3adec4b266e5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f9a02dcb3088dd81bf2c2ca6b96c90e5040b1e37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014755"
---
# <a name="plot-data-on-a-secondary-axis-report-builder-and-ssrs"></a>Traccia di dati su un asse secondario (Generatore report e SSRS)

Nel grafico sono inclusi due tipi di asse: principale e secondario. L'asse secondario risulta utile quando si confrontano due set di valori con due intervalli di dati diversi che condividono una categoria comune.  
  
 Si supponga ad esempio che in un grafico vengano calcolati i valori relativi ai ricavi rispetto alle imposte relative all'anno 2008. In questo caso, il periodo di tempo 2008 è comune a entrambi i set di valori. Tuttavia, quando le due serie vengono tracciate sullo stesso asse Y, non è possibile effettuare un confronto utile, perché la scala di quest'asse è ottimizzata per i valori maggiori nel set di dati. Indicando i ricavi sull'asse principale e le imposte su quello secondario, è possibile visualizzare ogni serie su un proprio asse Y con una scala di valori specifica. Le serie condividono un asse X comune.  
  
 Nelle situazioni in cui è necessario confrontare più di due serie, è consigliabile un approccio diverso per il confronto e la visualizzazione di più serie in un grafico. Per altre informazioni, vedere [Più serie in un grafico](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md).  
  
 Un esempio di questo grafico è disponibile come report di esempio. Per altre informazioni sul download di questo e di altri report di esempio, vedere la pagina relativa ai [report di esempio per Generatore report e Progettazione report](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-plot-a-series-on-the-secondary-axis"></a>Per tracciare una serie sull'asse secondario  
  
1.  Fare clic con il pulsante destro del mouse sulla serie nel grafico oppure in un campo dell'area **Valori** che si vuole visualizzare sull'asse secondario, quindi scegliere **Proprietà serie**. Verrà visualizzata la finestra di dialogo **Proprietà serie** .  
  
2.  Fare clic su **Assi e area grafico**, quindi selezionare quale degli assi secondari si desidera abilitare, l'asse dei valori o l'asse delle categorie.  

## <a name="next-steps"></a>Passaggi successivi

[Formattazione delle etichette degli assi di un grafico](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
[Specificare un intervallo dell'asse](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
