---
description: Simulazione di un output degli errori per il componente script
title: Simulazione di un output degli errori per il componente script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script component [Integration Services], error output
- error outputs [Integration Services], Script component
ms.assetid: f8b6ecff-ac99-4231-a0e7-7ce4ad76bad0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 88af189347fe799f58939f167e6713c90b3a8275
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344915"
---
# <a name="simulating-an-error-output-for-the-script-component"></a>Simulazione di un output degli errori per il componente script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Anche se non è possibile configurare direttamente un output come output degli errori nel componente script per la gestione automatica delle righe di errori, è possibile riprodurre la funzionalità di un output degli errori incorporato creando un output aggiuntivo e utilizzando la logica condizionale nello script per indirizzare le righe a questo output quando è appropriato. È necessario imitare il comportamento di un output degli errori incorporato aggiungendo due colonne di output aggiuntive per ricevere il numero di errore e l'ID della colonna nella quale si è verificato un errore.  
  
 Se si desidera aggiungere la descrizione dell'errore che corrisponde a un codice di errore [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] predefinito specifico, è possibile utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.GetErrorDescription%2A> dell'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>, disponibile tramite la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> del componente script.  
  
## <a name="example"></a>Esempio  
 Nell'esempio illustrato di seguito viene utilizzato un componente script configurato come trasformazione che dispone di due output sincroni. Lo scopo del componente script è di filtrare le righe di errore dai dati relativi agli indirizzi nel database di esempio AdventureWorks. Questo esempio fittizio presuppone che si stia preparando una promozione per i clienti dell'America del nord e che sia necessario filtrare gli indirizzi non relativi all'America del nord.  
  
#### <a name="to-configure-the-example"></a>Per configurare l'esempio  
  
1.  Prima di creare il nuovo componente script, creare una gestione connessione e configurare un'origine del flusso di dati che seleziona i dati relativi agli indirizzi dal database di esempio AdventureWorks. Per questo esempio, in cui viene analizzata solo la colonna CountryRegionName, è possibile utilizzare semplicemente la vista Person.vStateCountryProvinceRegion oppure selezionare i dati unendo in join le tabelle Person.Address, Person.StateProvince e Person.CountryRegion.  
  
2.  Aggiungere un nuovo componente script all'area di progettazione del flusso di dati e configurarlo come trasformazione. Aprire l'**Editor trasformazione Script**.  
  
3.  Nella pagina **Script** impostare la proprietà **ScriptLanguage** sul linguaggio di scripting che si vuole usare per codificare lo script.  
  
4.  Fare clic su **Modifica script** per aprire [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA).  
  
5.  Nel metodo **Input0_ProcessInputRow** digitare o incollare il codice di esempio di seguente.  
  
6.  Chiudere VSTA.  
  
7.  Nella pagina **Colonne di input** selezionare le colonne che si vogliono elaborare nella trasformazione Script. In questo esempio viene utilizzata solo la colonna CountryRegionName. Le colonne di input disponibili lasciate deselezionate verranno semplicemente passate nel flusso di dati senza alcuna modifica.  
  
8.  Nella pagina **Input e output** aggiungere un altro nuovo output e impostarne il valore **SynchronousInputID** sull'ID dell'input, che corrisponde anche al valore della proprietà **SynchronousInputID** dell'output predefinito. Impostare la proprietà **ExclusionGroup** di entrambi gli output sullo stesso valore diverso da zero (ad esempio, 1) per indicare che ogni riga verrà diretta a uno solo dei due output. Assegnare al nuovo output degli errori un nome distintivo, ad esempio "MyErrorOutput".  
  
9. Aggiungere altre colonne di output al nuovo output degli errori per acquisire le informazioni desiderate sull'errore, tra cui il codice di errore, l'ID della colonna nella quale si è verificato l'errore ed eventualmente la descrizione dell'errore. In questo esempio vengono create le nuove colonne, ErrorColumn ed ErrorMessage. Se si acquisiscono errori di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] predefiniti nell'implementazione, assicurarsi di aggiungere una colonna ErrorCode per il numero di errore.  
  
10. Prendere nota del valore di ID di una o più colonne di input in cui il componente script verificherà le condizioni di errore. In questo esempio viene utilizzato questo identificatore di colonna per popolare il valore ErrorColumn.  
  
11. Chiudere l'**Editor trasformazione Script.**  
  
12. Allegare gli output del componente script alle destinazioni adatte. Le destinazioni file flat sono le più semplici da configurare per test ad hoc.  
  
13. Eseguire il pacchetto.  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
  If Row.CountryRegionName <> "Canada" _  
      And Row.CountryRegionName <> "United States" Then  
  
    Row.ErrorColumn = 68 ' ID of CountryRegionName column  
    Row.ErrorMessage = "Address is not in North America."  
    Row.DirectRowToMyErrorOutput()  
  
  Else  
  
    Row.DirectRowToOutput0()  
  
  End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
{  
  
  if (Row.CountryRegionName!="Canada"&&Row.CountryRegionName!="United States")  
  
  {  
    Row.ErrorColumn = 68; // ID of CountryRegionName column  
    Row.ErrorMessage = "Address is not in North America.";  
    Row.DirectRowToMyErrorOutput();  
  
  }  
  else  
  {  
  
    Row.DirectRowToOutput0();  
  
  }  
  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md)   
 [Uso degli output degli errori in un componente flusso di dati](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md)   
 [Creazione di una trasformazione sincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
