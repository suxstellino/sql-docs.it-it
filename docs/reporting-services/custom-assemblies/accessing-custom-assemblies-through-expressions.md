---
title: Accesso agli assembly personalizzati tramite espressioni | Microsoft Docs
description: Dopo aver creato un assembly personalizzato, informazioni su come accedere alle classi nell'assembly personalizzato usando le espressioni di report.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- expressions [Reporting Services], custom assemblies
- static member calls
- instance member calls [Reporting Services]
- calling class members
- custom assemblies [Reporting Services], expressions
ms.assetid: 917c4d47-1a95-4f54-98b1-e8cb2165d90f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df929de930289f9683608ec42240f03a93983ccb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064636"
---
# <a name="accessing-custom-assemblies-through-expressions"></a>Accesso agli assembly personalizzati tramite espressioni
  Dopo aver creato un assembly personalizzato, averlo reso disponibile in Progettazione report o nel server di report, aver aggiunto i criteri di sicurezza appropriati e aver aggiunto un riferimento all'assembly personalizzato nella definizione del report, è possibile accedere ai membri delle classi nell'assembly utilizzando le espressioni di report. Per fare riferimento al codice personalizzato in un'espressione, è necessario chiamare il membro di una classe nell'assembly. La modalità di esecuzione di tale operazione dipende dal tipo di metodo, ovvero statico o basato su istanze.  
  
## <a name="calling-static-members-from-a-report-definition-file"></a>Chiamata a membri statici da un file di definizione del report  
 I membri statici appartengono alla classe o al tipo e non a un oggetto per il quale è stata creata un'istanza. È possibile accedere a questi membri chiamandoli direttamente dalla classe. È consigliabile utilizzare i membri statici per chiamare le funzioni personalizzate in un report ogni qualvolta è possibile, perché i membri statici offrono prestazioni migliori. Per chiamare un membro statico, è necessario farvi riferimento come espressione nel formato =*SpazioDeiNomi.Classe.Metodo*.  
  
#### <a name="to-call-static-members"></a>Per chiamare i membri statici  
  
-   Per chiamare un membro statico, impostare l'espressione in modo che corrisponda al nome completo del membro che include lo spazio dei nomi, il nome della classe e il nome del membro. Nell'esempio seguente viene chiamato il metodo **ToGBP**, che consente di convertire il valore del campo **StandardCost** da dollari a sterline e di visualizzarlo in un report:  
  
    ```  
    =CurrencyConversion.DollarCurrencyConversion.ToGBP(Fields!StandardCost.Value)  
    ```  
  
### <a name="important-information-regarding-static-fields-and-properties"></a>Informazioni importanti relative alle proprietà e ai campi statici  
 Attualmente, tutti i report vengono eseguiti nello stesso dominio dell'applicazione. Questo significa che i report con dati statici specifici dell'utente espongono questi dati alle altre istanze dello stesso report. In questo modo, i dati statici di un utente possono essere resi disponibili per tutti gli utenti che attualmente eseguono un report specifico. Per questo motivo, è consigliabile non usare proprietà o campi statici negli assembly personalizzati o nell'elemento **Code**, ma usare invece proprietà o campi di istanza nei report. I metodi statici possono comunque essere utilizzati, perché non comportano l'archiviazione dello stato o dei dati.  
  
## <a name="calling-instance-members-from-a-report-definition-file"></a>Chiamata a membri di istanza da un file di definizione del report  
 Se l'assembly personalizzato contiene membri di istanza a cui è necessario accedere in una definizione del report, è necessario aggiungere al report un nome di istanza per la classe. È possibile aggiungere un nome di istanza per una classe usando la scheda **Codice** della finestra di dialogo **Proprietà report**. Per altre informazioni sull'aggiunta di istanze di classi a un report, vedere [Riferimenti a codice personalizzato e ad assembly in espressioni in Progettazione report &#40;SSRS&#41;](../../reporting-services/report-design/custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs.md).  
  
 Per chiamare un membro statico, è necessario farvi riferimento come espressione nel formato =Code *.NomeIstanza.Metodo*.  
  
#### <a name="to-call-instance-members"></a>Per chiamare i membri dell'istanza  
  
-   Per chiamare un membro di istanza di un assembly personalizzato, è necessario fare riferimento alla parola chiave **Code** seguita dal nome dell'istanza e dal metodo. Nell'esempio seguente viene chiamato il metodo di istanza **ToEUR**, che consente di convertire il valore del campo **StandardCost** da dollari a euro e di visualizzarlo in un report:  
  
    ```  
    =Code.m_myDollarCoversion.ToEUR(Fields!StandardCost.Value)  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Uso di assembly personalizzati con i report](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
