---
title: Definire le impostazioni locali per un report o una casella di testo (Reporting Services) | Microsoft Docs
description: Usare la proprietà Language in una casella di testo per specificare le impostazioni locali per i formati che visualizzano i dati che differiscono per lingua e area in Generatore report.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- locales [Reporting Services]
ms.assetid: df115b01-184b-47f0-b5ec-0ad965ff9bee
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 345be03e2156e9097d9a7e60681ce901bd1e87b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045013"
---
# <a name="set-the-locale-for-a-report-or-text-box-reporting-services"></a>Definizione delle impostazioni locali per un report o una casella di testo (Reporting Services)
  Nella proprietà **Language** di un report o di una casella di testo sono contenute le impostazioni locali che determinano i formati predefiniti per la visualizzazione dei dati del report che differiscono in base alla lingua e al paese, ad esempio i valori relativi alla data o alla valuta o quelli numerici. La proprietà **Language** di una casella di testo ha la priorità rispetto alla proprietà **Language** impostata per il report. Se non viene specificato alcun valore per **Language**, in Reporting Services vengono utilizzate le impostazioni locali del sistema operativo nel server di report per i report pubblicati o del computer di creazione di report per l'anteprima del report.  
  
 Per i report HTML, è possibile ignorare il valore **Language** predefinito e usare la lingua specificata dall'intestazione HTTP del client del browser utilizzando il campo predefinito User!Language in un'espressione per la proprietà **Language** di un report o di una casella di testo.  
  
 È inoltre possibile specificare la proprietà **Language** per un report in un URL. Per altre informazioni, vedere [Impostare la lingua per i parametri del report in un URL](../../reporting-services/set-the-language-for-report-parameters-in-a-url.md).  
  
### <a name="to-set-the-locale-for-a-report"></a>Per definire le impostazioni locali per un report  
  
1.  Nella visualizzazione della struttura fare clic all'esterno dell'area di progettazione del report per selezionarlo.  
  
2.  Nel riquadro Proprietà digitare o selezionare per la proprietà **Language** la lingua da utilizzare per il report.  
  
### <a name="to-set-the-locale-for-a-text-box"></a>Per definire le impostazioni locali per una casella di testo  
  
1.  Nella visualizzazione della struttura selezionare la casella di testo cui si desidera applicare le impostazioni locali.  
  
2.  Nel riquadro Proprietà effettuare le operazioni seguenti:  
  
    -   Per la proprietà **Calendar** , digitare o selezionare il calendario che si desidera utilizzare per le date.  
  
    -   Per la proprietà **Direction** digitare o selezionare la direzione in cui è scritto il testo.  
  
    -   Per la proprietà **Language** , digitare o selezionare la lingua che si desidera utilizzare per la casella di testo. Questo valore ha la priorità rispetto alla proprietà **Language** per il report.  
  
    -   Per la proprietà **NumeralLanguage** , digitare o selezionare il formato da utilizzare per i numeri presenti nella casella di testo.  
  
    -   Per la proprietà **NumeralVariant** digitare o selezionare la variante del formato da utilizzare per i numeri nella casella di testo.  
  
    -   Per la proprietà **UnicodeBiDi** selezionare il livello di incorporamento bidirezionale da utilizzare nella casella di testo.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo delle espressioni nei report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Considerazioni sulla progettazione di soluzioni per distribuzioni multilingue o globali (Reporting Services)](/previous-versions/sql/)  
  
