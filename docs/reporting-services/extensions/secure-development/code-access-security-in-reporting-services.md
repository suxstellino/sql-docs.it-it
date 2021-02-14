---
title: Sicurezza dall'accesso di codice in Reporting Services | Microsoft Docs
description: Informazioni sulla sicurezza dall'accesso di codice in Reporting Services. Informazioni sul ruolo di evidenza, gruppi di codice e set di autorizzazioni denominati nei criteri di sicurezza.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- code access security [Reporting Services]
- permission sets [Reporting Services]
- evidence [Reporting Services]
- code access security [Reporting Services], about code access security
- named permission sets [Reporting Services]
ms.assetid: 97480368-1fc3-4c32-b1b0-63edfb54e472
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dccf6bacfaf431cb69ba4ec8dc031f4e817f4650
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065296"
---
# <a name="code-access-security-in-reporting-services"></a>Sicurezza dall'accesso di codice in Reporting Services
  La sicurezza dall'accesso di codice si basa su tre concetti principali, ovvero evidenza, gruppi di codice e set di autorizzazione denominati. In [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] i componenti Gestione report, Progettazione report e server di report dispongono ognuno di un file di criteri che configura la sicurezza dall'accesso di codice per assembly personalizzati ed estensioni per i dati, il recapito, il rendering e di sicurezza. Nelle sezioni seguenti viene fornita una panoramica sulla sicurezza dall'accesso di codice. Per altre informazioni sugli argomenti trattati in questa sezione, vedere la sezione relativa al modello dei criteri di sicurezza nella documentazione di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK.  
  
 In [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] la sicurezza dall'accesso di codice viene utilizzata perché, anche se il server di report è compilato in base a tecnologia [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)], è presente una differenza sostanziale tra un'applicazione [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] tipica e il server di report. Mentre un'applicazione [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] tipica non esegue codice utente, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] usa un'architettura aperta ed estensibile che consente agli utenti di programmare in base ai file di definizione del report usando l'elemento **Code** di Report Definition Language e di sviluppare funzionalità specifiche in un assembly personalizzato da usare nei report. Gli sviluppatori possono inoltre progettare e distribuire estensioni potenti che consentono di ottimizzare le funzionalità del server di report. Queste caratteristiche di potenza e flessibilità determinano la necessità di utilizzare il maggior livello di sicurezza possibile.  
  
 Gli sviluppatori di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] possono utilizzare qualsiasi assembly di [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] nei report e sfruttare tutte le funzionalità degli assembly distribuiti nella Global Assembly Cache (CAG). L'unico elemento che il server di report può controllare sono le autorizzazioni concesse per le espressioni del report e per gli assembly personalizzati caricati. In [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] per impostazione predefinita agli assembly personalizzati vengono concesse solo le autorizzazioni **Execute**.  
  
## <a name="evidence"></a>Evidenza  
 Per evidenza si intendono le informazioni utilizzate da Common Language Runtime (CLR) per stabilire criteri di sicurezza per gli assembly del codice. In fase di esecuzione l'evidenza indica che al codice sono associate caratteristiche specifiche. Forme comuni di evidenza includono ad esempio le firme digitali e il percorso di un assembly. È possibile inoltre personalizzare l'evidenza per rappresentare altre informazioni significative per l'applicazione.  
  
 Sia agli assembly che ai domini applicazione vengono concesse autorizzazioni sulla base dell'evidenza. Il percorso di un assembly cui [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sta eseguendo l'accesso è ad esempio una forma comune di evidenza per assembly con nome non sicuro, nota come evidenza URL. L'evidenza URL per un'estensione per l'elaborazione dei dati personalizzata distribuita in un server di report potrebbe essere "C:\Programmi\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\Microsoft.Samples.ReportingServices.FsiDataExtension.dll". Il nome sicuro o firma digitale di un assembly è un altra forma comune di evidenza. In questo caso, l'evidenza è rappresentata dalle informazioni sulla chiave pubblica per un assembly.  
  
## <a name="code-groups"></a>Gruppi di codice  
 Un gruppo di codice è un raggruppamento logico di codice per appartenere al quale è necessario rispettare a una specifica condizione. Ogni codice che soddisfa la condizione di appartenenza viene incluso nel gruppo. Gli amministratori configurano i criteri di sicurezza mediante la gestione dei gruppi di codice e dei set di autorizzazioni associati.  
  
 Una condizione di appartenenza per un gruppo di codice si basa sull'evidenza. Un'appartenenza URL per un gruppo di codice, ad esempio, si basa sull'evidenza URL. Per descrivere il codice e per verificare se è stata soddisfatta la condizione di appartenenza a un gruppo, Common Language Runtime utilizza le caratteristiche di identificazione, ovvero l'evidenza URL. Ad esempio, se la condizione di appartenenza di un gruppo di codice è "codice nell'assembly C:\Programmi\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\Microsoft.Samples.ReportingServices.FsiDataExtension.dll", in fase di esecuzione viene esaminata l'evidenza per determinare se il codice ha origine in quel percorso. Un esempio di una voce di configurazione per questo tipo di gruppo di codice potrebbe essere analogo al seguente:  
  
```  
<CodeGroup class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="FullTrust"  
   Name="MyCodeGroup"  
   Description="Code group for my data processing extension">  
      <IMembershipCondition class="UrlMembershipCondition"  
         version="1"  
         Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\Microsoft.Samples.ReportingServices.FsiDataExtension.dll"  
       />  
</CodeGroup>  
```  
  
 Per determinare il tipo di sicurezza dall'accesso di codice e i gruppi di codice necessari agli assembly personalizzati o alle estensioni di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è necessario rivolgersi all'amministratore di sistema o agli esperti di distribuzione di applicazioni.  
  
## <a name="named-permission-sets"></a>Set di autorizzazioni denominati  
 Un set di autorizzazioni denominato è un set di autorizzazioni che gli amministratori possono associare a un gruppo di codice. La maggior parte dei set di autorizzazioni è costituito almeno da un'autorizzazione, un nome e una descrizione per il set stesso. Gli amministratori possono utilizzare i set di autorizzazioni denominati per stabilire o modificare i criteri di sicurezza per i gruppi di codice. A uno stesso set di autorizzazioni denominati possono essere associati più gruppi di codice. In CLR sono disponibili set di autorizzazioni denominati, ad esempio **Nothing**, **Execution**, **Internet**, **LocalIntranet**, **Everything** e **FullTrust**.  
  
> [!NOTE]  
>  Le estensioni per dati personalizzati, recapito, rendering e di sicurezza in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] devono essere eseguite con il set di autorizzazioni **FullTrust**. Per aggiungere il gruppo di codice appropriato e le condizioni di appartenenza per le estensioni di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], rivolgersi all'amministratore di sistema.  
  
 È possibile associare livelli personalizzati di autorizzazioni per assembly personalizzati utilizzati con i report. Se ad esempio si desidera consentire a un assembly di accedere a un file specifico, è possibile creare un nuovo set di autorizzazioni denominato con accesso I/O al file specifico e assegnare quindi il set di autorizzazioni al gruppo di codice. Nel set di autorizzazioni seguente viene concesso l'accesso in sola lettura al file MyFile.xml:  
  
```  
<PermissionSet class="NamedPermissionSet"  
   version="1"  
   Name="MyNewFilePermissionSet"  
   Description="A special permission set that grants read access to my file.">  
    <IPermission class="FileIOPermission"  
       version="1"  
       Read="C:\MyFile.xml"/>  
    <IPermission class="SecurityPermission"  
       version="1"  
       Flags="Assertion, Execution"/>  
</PermissionSet>  
```  
  
 Un gruppo di codice cui si concede questo set di autorizzazioni potrebbe essere analogo al seguente:  
  
```  
<CodeGroup class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="MyNewFilePermissionSet"  
   Name="MyNewCodeGroup"  
   Description="A special code group for my custom assembly.">  
   <IMembershipCondition class="UrlMembershipCondition"  
      version="1"  
      Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\MyCustomAssembly.dll"/>  
</CodeGroup>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo sicuro &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
  
