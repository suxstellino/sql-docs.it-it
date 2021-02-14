---
description: Creazione di un enumeratore Foreach personalizzato
title: Creazione di un enumeratore Foreach personalizzato | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom foreach enumerators [Integration Services], creating
ms.assetid: 050e8455-2ed0-4b6d-b3ea-4e80e6c28487
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ab6fe83aca3ddcbf5aad1826016bdc2f017537d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350285"
---
# <a name="creating-a-custom-foreach-enumerator"></a>Creazione di un enumeratore Foreach personalizzato

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  I passaggi per la creazione di un enumeratore Foreach personalizzato sono simili a quelli richiesti per la creazione di qualsiasi altro oggetto personalizzato per [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]:  
  
-   Creare una nuova classe che eredita dalla classe di base. Per un enumeratore Foreach, la classe di base è <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>.  
  
-   Applicare alla classe l'attributo che identifica il tipo di oggetto. Per un enumeratore Foreach, l'attributo è <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>.  
  
-   Eseguire l'override dell'implementazione dei metodi e delle proprietà della classe di base. Per un enumeratore Foreach, il più importante è il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>.  
  
-   Se si desidera, sviluppare un'interfaccia utente personalizzata. Per un enumeratore Foreach, questa operazione richiede una classe che implementi l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSForEachEnumeratorUI>.  
  
 Un enumeratore personalizzato è ospitato dal contenitore <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>. In fase di esecuzione il contenitore <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> chiama il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> dell'enumeratore personalizzato. L'enumeratore personalizzato restituisce un oggetto che implementa l'interfaccia **IEnumerable**, ad esempio **ArrayList**. Tramite l'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> viene quindi scorso ogni elemento della raccolta, viene fornito il valore dell'elemento corrente al flusso di controllo tramite una variabile definita dall'utente e viene eseguito il flusso di controllo nel contenitore.  
  
## <a name="getting-started-with-a-custom-foreach-enumerator"></a>Introduzione a un enumeratore Foreach personalizzato  
  
### <a name="creating-projects-and-classes"></a>Creazione di progetti e classi  
 Poiché tutti gli enumeratori Foreach gestiti derivano dalla classe di base <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>, il primo passaggio da completare quando si crea un enumeratore Foreach personalizzato consiste nel creare un progetto di libreria di classi nel linguaggio di programmazione gestito preferito e creare una classe che eredita dalla classe di base. In questa classe derivata si eseguirà l'override dei metodi e delle proprietà della classe di base per implementare la funzionalità personalizzata.  
  
 Nella stessa soluzione creare un secondo progetto di libreria di classi per l'interfaccia utente personalizzata. Per semplificare lo sviluppo, si consiglia di utilizzare un assembly distinto per l'interfaccia utente, perché in questo modo è possibile e ridistribuire l'enumeratore Foreach o la relativa interfaccia utente in modo indipendente.  
  
 Configurare entrambi i progetti per firmare gli assembly che verranno generati durante la compilazione utilizzando un file di chiave con nome sicuro.  
  
### <a name="applying-the-dtsforeachenumerator-attribute"></a>Applicazione dell'attributo DtsForEachEnumerator  
 Applicare l'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> alla classe creata per identificarla come enumeratore Foreach. Questo attributo fornisce informazioni in fase di progettazione, ad esempio il nome e la descrizione dell'enumeratore Foreach. La proprietà **Name** viene visualizzata nell'elenco a discesa degli enumeratori disponibili nella scheda **Raccolta** della finestra di dialogo **Editor ciclo Foreach**.  
  
 Utilizzare la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A> per collegare l'enumeratore Foreach alla relativa interfaccia utente personalizzata. Per ottenere il token di chiave pubblica richiesto per questa proprietà, è possibile usare **sn.exe -t** per visualizzare il token di chiave pubblica dal file della coppia di chiavi (con estensione snk) che si intende usare per firmare l'assembly dell'interfaccia utente.  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Namespace Microsoft.Samples.SqlServer.Dts  
    <DtsForEachEnumerator(DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")> _   
    Public Class MyEnumerator  
     Inherits ForEachEnumerator  
        'Insert code here.  
    End Class  
End Namespace  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsForEachEnumerator( DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")]  
    public class MyEnumerator : ForEachEnumerator  
    {  
        //Insert code here.  
    }  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-enumerator"></a>Compilazione, distribuzione e debug di un enumeratore personalizzato  
 I passaggi per la compilazione, la distribuzione e il debug di un enumeratore Foreach personalizzato in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] sono molto simili a quelli richiesti per altri tipi di oggetti personalizzati. Per altre informazioni, vedere [Compilazione, distribuzione e debug di oggetti personalizzati](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Scrittura del codice di un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)   
 [Sviluppo di un'interfaccia utente per un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
  
  
