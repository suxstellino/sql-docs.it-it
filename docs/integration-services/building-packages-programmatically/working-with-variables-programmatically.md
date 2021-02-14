---
description: Utilizzo delle variabili a livello di programmazione
title: Uso di variabili a livello di programmazione | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- System namespace
- scope [Integration Services]
- variables [Integration Services], programmatically
- configuration files [Integration Services]
- Variables collection
- User namespace
- custom variables [Integration Services]
- variables [Integration Services], customizing
ms.assetid: c4b76a3d-94ca-4a8e-bb45-cb8bd0ea3ec1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8258d74d7f81b17d1e3e7973011a036031bd78da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355016"
---
# <a name="working-with-variables-programmatically"></a>Utilizzo delle variabili a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le variabili consentono di impostare valori in modo dinamico e di controllare i processi in pacchetti, contenitori, attività e gestori eventi. Possono inoltre essere utilizzate dai vincoli di precedenza per controllare la direzione del flusso di dati verso attività diverse. Le variabili possono essere utilizzate per effettuare le operazioni seguenti:  
  
-   Aggiornare le proprietà di un pacchetto in fase di esecuzione.  
  
-   Popolare i valori dei parametri per le istruzioni Transact-SQL in fase di esecuzione.  
  
-   Controllare il flusso di un ciclo Foreach. Per altre informazioni, vedere [Aggiungere un'enumerazione a un flusso di controllo](../control-flow/foreach-loop-container.md).  
  
-   Controllare un vincolo di precedenza in base al relativo utilizzo in un'espressione. Un vincolo di precedenza può includere variabili nella definizione del vincolo. Per altre informazioni, vedere [Aggiunta di espressioni ai vincoli di precedenza](../control-flow/precedence-constraints.md).  
  
-   Controllare la ripetizione condizionale di un contenitore Ciclo For. Per altre informazioni, vedere [Aggiungere un'enumerazione a un flusso di controllo](../control-flow/for-loop-container.md).  
  
-   Compilare espressioni che includono valori di variabili.  
  
-   È possibile creare variabili personalizzate per tutti i tipi di contenitori, ovvero pacchetti, contenitori **Ciclo Foreach**, contenitori **Ciclo For**, contenitori **Sequenza**, TaskHost e gestori di eventi. Per altre informazioni, vedere [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) e [Utilizzo di variabili nei pacchetti](../integration-services-ssis-variables.md).  
  
## <a name="scope"></a>Scope  
 Ogni contenitore dispone di una propria raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Variables>. Ogni nuova variabile creata si trova nell'ambito del relativo contenitore padre. Poiché il contenitore del pacchetto costituisce il livello principale della gerarchia dei contenitori, le variabili con ambito pacchetto sono variabili globali e sono visibili a tutti i contenitori del pacchetto. Alla raccolta di variabili per il contenitore possono accedere anche gli elementi figlio del contenitore tramite la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Variables>, utilizzando il nome della variabile o il relativo indice nella raccolta.  
  
 Poiché la visibilità di una variabile ha un ambito dall'alto in basso, le variabili dichiarate a livello di pacchetto sono visibili a tutti i contenitori nel pacchetto. Pertanto, la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Variables> in un contenitore include tutte le variabili che appartengono al relativo oggetto padre in aggiunta alle proprie variabili.  
  
 Viceversa, le variabili contenute in un'attività sono limitate in termini di ambito e visibilità e sono visibili solo all'attività.  
  
 Se un pacchetto esegue altri pacchetti, le variabili definite nell'ambito del pacchetto chiamante sono disponibili per il pacchetto chiamato. L'unica eccezione si verifica quando nel pacchetto chiamato esiste una variabile con lo stesso nome. Quando si verifica questa collisione, il valore della variabile nel pacchetto chiamato sostituisce il valore del pacchetto chiamante. Le variabili definite nell'ambito del pacchetto chiamato non sono mai nuovamente disponibili per il pacchetto chiamante.  
  
 Nell'esempio di codice seguente viene creata una variabile a livello di programmazione, `myCustomVar`, nell'ambito del pacchetto, quindi vengono scorse tutte le variabili visibili al pacchetto e ne vengono stampati il nome, il tipo di dati e il valore.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application app = new Application();  
      // Load a sample package that contains a variable that sets the file name.  
      Package pkg = app.LoadPackage(  
        @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx",  
        null);  
      Variables pkgVars = pkg.Variables;  
      Variable myVar = pkg.Variables.Add("myCustomVar", false, "User", "3");  
      foreach (Variable pkgVar in pkgVars)  
      {  
        Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name,  
          pkgVar.DataType, pkgVar.Value.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim app As Application = New Application()  
    ' Load a sample package that contains a variable that sets the file name.  
    Dim pkg As Package = app.LoadPackage( _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx", _  
      Nothing)  
    Dim pkgVars As Variables = pkg.Variables  
    Dim myVar As Variable = pkg.Variables.Add("myCustomVar", False, "User", "3")  
    Dim pkgVar As Variable  
    For Each pkgVar In pkgVars  
      Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name, _  
        pkgVar.DataType, pkgVar.Value.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Esempio di output**  
  
 `Variable: CancelEvent, Int32, 0`  
  
 `Variable: CreationDate, DateTime, 4/18/2003 11:57:00 AM`  
  
 `Variable: CreatorComputerName, String,`  
  
 `Variable: CreatorName, String,`  
  
 `Variable: ExecutionInstanceGUID, String, {237AB5A4-7E59-4FC9-8D61-E8F20363DF25}`  
  
 `Variable: FileName, String, Junk`  
  
 `Variable: InteractiveMode, Boolean, False`  
  
 `Variable: LocaleID, Int32, 1033`  
  
 `Variable: MachineName, String, MYCOMPUTERNAME`  
  
 `Variable: myCustomVar, String, 3`  
  
 `Variable: OfflineMode, Boolean, False`  
  
 `Variable: PackageID, String, {F0D2E396-A6A5-42AE-9467-04CE946A810C}`  
  
 `Variable: PackageName, String, DTSPackage1`  
  
 `Variable: StartTime, DateTime, 1/28/2005 7:55:39 AM`  
  
 `Variable: UserName, String, <domain>\<userid>`  
  
 `Variable: VersionBuild, Int32, 198`  
  
 `Variable: VersionComments, String,`  
  
 `Variable: VersionGUID, String, {90E105B4-B4AF-4263-9CBD-C2050C2D6148}`  
  
 `Variable: VersionMajor, Int32, 1`  
  
 `Variable: VersionMinor, Int32, 0`  
  
 Si noti che tutte le variabili che hanno come ambito lo spazio dei nomi **System** sono disponibili per il pacchetto. Per altre informazioni, vedere [Variabili di sistema](../../integration-services/system-variables.md).  
  
## <a name="namespaces"></a>Spazi dei nomi  
 In [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ([!INCLUDE[ssIS](../../includes/ssis-md.md)]) sono disponibili due spazi dei nomi predefiniti in cui risiedono le variabili, ovvero gli spazi dei nomi **User** e **System**. Per impostazione predefinita, tutte le variabili personalizzate create dallo sviluppatore vengono aggiunte allo spazio dei nomi **User**. Le variabili di sistema risiedono nello spazio dei nomi **System**. È possibile creare altri spazi dei nomi diversi da **User** in cui includere variabili personalizzate, nonché modificare il nome dello spazio dei nomi **User**, ma non è possibile aggiungere o modificare variabili nello spazio dei nomi **System**, né assegnare variabili di sistema a uno spazio dei nomi diverso.  
  
 Le variabili di sistema disponibili variano a seconda del tipo di contenitore. Per un elenco delle variabili di sistema disponibili per pacchetti, contenitori, attività e gestori di eventi, vedere [Variabili di sistema](../../integration-services/system-variables.md).  
  
## <a name="value"></a>Valore  
 Il valore di una variabile personalizzata può essere un valore letterale o un'espressione:  
  
-   Se si desidera che la variabile contenga un valore letterale, impostare il valore della relativa proprietà <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A>.  
  
-   Se si vuole che la variabile contenga un'espressione, in modo che sia possibile usare i risultati dell'espressione come valore, impostare la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> della variabile su **true** e specificare un'espressione nella proprietà <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Expression%2A>. In fase di esecuzione l'espressione verrà valutata e il relativo risultato verrà utilizzato come valore della variabile. Se ad esempio la proprietà Expression di una variabile è `"100 * 2""100 * 2"`, la variabile restituirà il valore 200.  
  
 Non è possibile impostare in modo esplicito il valore <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> di una variabile. Il valore <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> è derivato dal valore iniziale assegnato alla variabile e non può essere modificato in seguito. Per altre informazioni sui tipi di dati della variabile, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Nell'esempio di codice seguente viene creata una nuova variabile, la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> viene impostata su **true**, viene assegnata l'espressione `"100 * 2"` alla proprietà Expression della variabile, quindi viene restituito il valore della variabile.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package pkg = new Package();  
      Variable v100 = pkg.Variables.Add("myVar", false, "", 1);  
      v100.EvaluateAsExpression = true;  
      v100.Expression = "100 * 2";  
      Console.WriteLine("Expression for myVar: {0}",   
        v100.Properties["Expression"].GetValue(v100));  
      Console.WriteLine("Value of myVar: {0}", v100.Value.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim pkg As Package = New Package  
    Dim v100 As Variable = pkg.Variables.Add("myVar", False, "", 1)  
    v100.EvaluateAsExpression = True  
    v100.Expression = "100 * 2"  
    Console.WriteLine("Expression for myVar: {0}", _  
      v100.Properties("Expression").GetValue(v100))  
    Console.WriteLine("Value of myVar: {0}", v100.Value.ToString)  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Esempio di output**  
  
 `Expression for myVar: 100 * 2`  
  
 `Value of myVar: 200`  
  
 È necessario utilizzare un'espressione valida che utilizza la sintassi delle espressioni di [!INCLUDE[ssIS](../../includes/ssis-md.md)]. I valori letterali sono consentiti nelle espressioni delle variabili, in aggiunta agli operatori e alle funzioni disponibili nella sintassi delle espressioni, ma le espressioni non possono fare riferimento ad altre variabili o colonne. Per altre informazioni, vedere [Espressioni di Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
## <a name="configuration-files"></a>File di configurazione  
 Se un file di configurazione include una variabile personalizzata, la variabile può essere aggiornata in fase di esecuzione. Questo significa che quando il pacchetto viene eseguito, il valore della variabile che si trovava originariamente nel pacchetto viene sostituito con un nuovo valore del file di configurazione. Questa tecnica di sostituzione risulta utile quando un pacchetto viene distribuito in più server che richiedono valori di variabili diversi. Ad esempio, una variabile può specificare il numero di volte in cui un contenitore **Ciclo Foreach** ripete il flusso di lavoro o elencare i destinatari a cui un gestore di eventi invia un messaggio di posta elettronica quando viene generato un errore oppure modificare il numero di errori che possono verificarsi prima che l'esecuzione del pacchetto abbia esito negativo. Queste variabili vengono fornite dinamicamente nei file di configurazione per ogni ambiente. Pertanto, nei file di configurazione sono consentite solo variabili di lettura/scrittura. Per altre informazioni, vedere [Creazione di configurazioni dei pacchetti](../packages/legacy-package-deployment-ssis.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)   
 [Uso di variabili nei pacchetti](../integration-services-ssis-variables.md)  
  
