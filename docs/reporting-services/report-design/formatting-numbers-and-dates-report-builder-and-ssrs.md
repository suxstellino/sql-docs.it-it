---
title: Formattazione di numeri e date (Generatore report) | Microsoft Docs
description: Informazioni su come formattare numeri e date nelle aree dati di Generatore report selezionando un formato nella pagina Numero della finestra di dialogo Proprietà dell'area dati corrispondente.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.placeholderproperties.number.f1
- "10127"
- sql13.rtp.rptdesigner.textboxproperties.number.f1
- "10130"
- "10286"
- sql13.rtp.rptdesigner.serieslabelproperties.number.f1
- "10285"
- sql13.rtp.rptdesigner.axisproperties.number.f1
ms.assetid: 6de1a725-9f06-4708-be26-2d55e442e344
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9df64577305006d406be0f10e060a7ac60ed10df
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596892"
---
# <a name="formatting-numbers-and-dates-report-builder-and-ssrs"></a>Formattazione di numeri e date (Generatore report e SSRS)
  È possibile formattare numeri e date nelle aree dati selezionando un formato nella pagina **Numero** della finestra di dialogo **Proprietà** dell'area dati corrispondente.  
  
 Per specificare stringhe di formato all'interno di un elemento del report casella di testo, è necessario selezionare l'elemento da formattare, fare clic con il pulsante destro del mouse, selezionare **Proprietà casella di testo**, quindi fare clic su **Numero**. È possibile formattare singole celle in un'area dati matrice o tabella secondo le stesse modalità perché le celle di una tabella o matrice sono singole caselle di testo.  
  
 Un'area dati grafico mostra in genere le date lungo l'asse delle categorie (x) e i valori lungo l'asse dei valori (y). Per specificare la formattazione in un grafico, fare clic con il pulsante destro del mouse su un asse e selezionare **Proprietà asse**. Sull'asse dei valori è possibile specificare formati validi solo per i numeri. Per altre informazioni, vedere [Formattazione delle etichette degli assi in un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  
  
 Per specificare la formattazione in un'area dati del misuratore, fare clic con il pulsante destro del mouse sulla scala del misuratore e selezionare **Proprietà scala radiale** o **Proprietà scala lineare**.  
  
> [!NOTE]  
>  Se le opzioni di formattazione che si desidera usare non sono disponibili, significa che tali opzioni non sono compatibili con il tipo di dati del campo, impostato nell'origine dati. Se, ad esempio, il campo contiene valori numerici ma i dati del campo sono di tipo stringa, non sarà possibile applicare le opzioni di formattazione dei dati numerici, quali valuta o decimali.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="considerations-for-formatting-numbers-and-dates"></a>Considerazioni sulla formattazione di numeri e date  
 Prima di formattare numeri e date nel report, considerare quanto segue:  
  
-   Per impostazione predefinita, i numeri vengono formattati in modo da riflettere le impostazioni relative alla lingua sul computer client. Usare le stringhe di formattazione per specificare la modalità di visualizzazione dei numeri in modo che la formattazione sia coerente indipendentemente dalla località in cui si trova l'utente che visualizza il report.  
  
-   I formati disponibili nella pagina **Numero** rappresentano un subset delle stringhe di formato numerico standard di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Per formattare un numero o una data usando un formato personalizzato non incluso nella finestra di dialogo, è possibile usare qualsiasi stringa di formato [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] per i numeri o le date. Per altre informazioni sulle stringhe di formato personalizzato, vedere l'argomento [Formattazione dei tipi di dati](/previous-versions/) su MSDN.  
  
-   Se è stata specificata una stringa di formato personalizzato, tale stringa avrà una priorità più elevata rispetto alle impostazioni predefinite che sono specifiche delle impostazioni cultura. Si supponga, ad esempio, di impostare la stringa di formato personalizzato "#, ###" per visualizzare il numero 1234 come 1,234. Questa impostazione potrebbe essere interpretata dagli utenti americani in modo diverso rispetto agli utenti Europai. Prima di specificare un formato personalizzato, considerare in che modo tale formato influirà sugli utenti con impostazioni cultura differenti che potrebbero visualizzare il report.  
  
-   Se si specifica una stringa di formato non valida, il testo formattato verrà interpretato come stringa letterale che esegue l'override della formattazione.  
  
-   Se si formatta una combinazione di numeri e caratteri nella stessa casella di testo, provare a usare un segnaposto per formattare il numero separatamente dal resto del testo. Per altre informazioni, vedere [Formattazione di testo e segnaposto &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md). Se si specifica una stringa di formato non valida per la proprietà Format nella casella di testo, la stringa di formato verrà ignorata. Se si specifica una stringa di formato non valida per la proprietà Format nel grafico o nel misuratore, la stringa di formato specificata verrà interpretata come stringa e la formattazione non verrà applicata.  
  
-   Se si seleziona **Valuta** in **Categoria** e si seleziona **Mostra valori in**, è possibile selezionare **Migliaia**, **Milioni** o **Miliardi** per visualizzare numeri usando formati finanziari. Se, ad esempio, il valore del campo è 1.789.905.394, si seleziona **Miliardi** e si specificano 2 cifre decimali, il valore visualizzato nel report è 1,78.  
  
## <a name="see-also"></a>Vedere anche  
 [Formattazione di testo e segnaposto &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [Formattazione di linee, colori e immagini &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-lines-colors-and-images-report-builder-and-ssrs.md)   
 [Formattazione di un grafico &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Formattazione delle etichette degli assi come date o valute &#40;Generatore report e SSRSSSRS&#41;](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [Formattazione di scale su un misuratore &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/formatting-scales-on-a-gauge-report-builder-and-ssrs.md)  
  
