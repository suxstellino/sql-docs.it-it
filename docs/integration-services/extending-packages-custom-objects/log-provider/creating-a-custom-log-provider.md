---
description: Creazione di un provider di log personalizzato
title: Creazione di un provider di log personalizzato | Microsoft Docs
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
- custom log providers [Integration Services], creating
ms.assetid: fc20af96-9eb8-4195-8d3f-8a4d7c753f24
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1f6de779a67df0bdbd81086c1b34992d0fe896ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354372"
---
# <a name="creating-a-custom-log-provider"></a>Creazione di un provider di log personalizzato

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  L'ambiente di runtime di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] include funzionalità estese di registrazione. Un log consente di acquisire gli eventi che si verificano durante l'esecuzione di pacchetti. In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] è inclusa una varietà di provider di log che consentono di creare e archiviare log in più formati quali XML, testo, database o nel registro eventi di Windows. Se uno di questi provider o formati di output non soddisfano specifiche esigenze, è possibile creare un provider di log personalizzato.  
  
 I passaggi per la creazione di un provider di log personalizzato sono simili a quelli richiesti per la creazione di qualsiasi altro oggetto personalizzato per [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]:  
  
-   Creare una nuova classe che eredita dalla classe di base. Per un provider di log, la classe di base è <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>.  
  
-   Applicare alla classe l'attributo che identifica il tipo di oggetto. Per un provider di log, l'attributo è <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>.  
  
-   Eseguire l'override dell'implementazione dei metodi e delle proprietà della classe di base. Per un provider di log, questi includono la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> e i metodi <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
-   Le interfacce utente personalizzate per i provider di log personalizzati non sono implementate in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
## <a name="getting-started-with-a-custom-log-provider"></a>Introduzione ai provider di log personalizzati  
  
### <a name="creating-projects-and-classes"></a>Creazione di progetti e classi  
 Poiché tutti i provider di log gestiti derivano dalla classe di base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>, il primo passaggio da completare quando si crea un provider di log personalizzato consiste nel creare un progetto di libreria di classi nel linguaggio di programmazione gestito preferito e creare una classe che eredita dalla classe di base. In questa classe derivata si eseguirà l'override dei metodi e delle proprietà della classe di base per implementare la funzionalità personalizzata.  
  
 Configurare il progetto per firmare l'assembly che verrà generato utilizzando un file di chiave con nome sicuro.  
  
> [!NOTE]  
>  Molti provider di log di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] hanno un'interfaccia utente personalizzata che implementa <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI> e sostituisce la casella di testo **Configurazione** nella finestra di dialogo **Configura log SSIS** con un elenco a discesa filtrato di gestioni connessioni disponibili. Tuttavia, le interfacce utente personalizzate per i provider di log personalizzati non sono implementate in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
### <a name="applying-the-dtslogprovider-attribute"></a>Applicazione dell'attributo DtsLogProvider  
 Applicare l'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> alla classe creata per identificarla come provider di log. Questo attributo fornisce informazioni in fase di progettazione, ad esempio il nome e la descrizione del provider di log. Le proprietà **DisplayName** e **Description** dell'attributo corrispondono alle colonne **Nome** e **Descrizione** visualizzate nell'editor **Configura log SSIS**, visualizzato durante la configurazione della registrazione per un pacchetto in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
> [!IMPORTANT]  
>  La proprietà <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.LogProviderType%2A> dell'attributo non viene utilizzata. È tuttavia necessario immettere un valore per tale proprietà, altrimenti il provider di log personalizzato non verrà visualizzato nell'elenco di provider di log disponibili.  
  
> [!NOTE]  
>  Poiché le interfacce utente personalizzate per i provider di log personalizzati non vengono implementate in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], l'impostazione di un valore per la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.UITypeName%2A> di <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> non ha alcun effetto.  
  
```vb  
<DtsLogProvider(DisplayName:="MyLogProvider", Description:="A simple log provider.", LogProviderType:="Custom")> _  
Public Class MyLogProvider  
     Inherits LogProviderBase  
    ' TODO: Override the base class methods.  
End Class  
```  
  
```csharp  
[DtsLogProvider(DisplayName="MyLogProvider", Description="A simple log provider.", LogProviderType="Custom")]  
public class MyLogProvider : LogProviderBase  
{  
    // TODO: Override the base class methods.  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-log-provider"></a>Compilazione, distribuzione e debug di un provider di log personalizzato  
 I passaggi per la compilazione, la distribuzione e il debug di un provider di log personalizzato in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] sono molto simili a quelli richiesti per altri tipi di oggetti personalizzati. Per altre informazioni, vedere [Compilazione, distribuzione e debug di oggetti personalizzati](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Scrittura del codice di un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)   
 [Sviluppo di un'interfaccia utente per un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
