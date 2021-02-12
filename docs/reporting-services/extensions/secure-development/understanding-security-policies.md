---
title: Informazioni sui criteri di sicurezza | Microsoft Docs
description: Informazioni sui criteri di sicurezza di Reporting Services, che specificano le autorizzazioni per gli assembly, le estensioni e le espressioni del report in base ai gruppi di codice.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- code access security [Reporting Services], security policies
- Execution permission set
- custom assemblies [Reporting Services], security
- permission sets [Reporting Services]
- expressions [Reporting Services], security
- evidence [Reporting Services]
- FullTrust permission set
- security policies [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: a9bf043a-139a-4929-9a58-244815323df0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 072a2f88ce87c5330614961dc1aed842aae2e058
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065276"
---
# <a name="understanding-security-policies"></a>Informazioni sui criteri di sicurezza
  Qualsiasi codice eseguito da un server di report deve far parte di criteri di sicurezza dall'accesso di codice specifici. Tali criteri di sicurezza sono costituiti da gruppi di codice che eseguono il mapping dell'evidenza a un insieme di set di autorizzazioni denominati. I gruppi di codice sono spesso associati a un set di autorizzazioni denominato che specifica le autorizzazioni consentite per il codice appartenente al gruppo specifico. In fase di esecuzione l'evidenza viene usata da un host trusted o dal caricatore per determinare i gruppi di codice cui il codice appartiene e, di conseguenza, le autorizzazioni per concedere il codice. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] aderisce a questo tipo di architettura relativa ai criteri di sicurezza come definito da Common Language Runtime (CLR) di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Nelle sezioni seguenti vengono descritti i diversi tipi di codice in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e le regole dei criteri associate.  
  
## <a name="report-server-assemblies"></a>Assembly del server di report  
 Gli assembly del server di report contengono codice incluso nel prodotto [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] viene scritto usando assembly del codice gestito. Tutti questi assembly hanno un nome sicuro, ossia sono firmati digitalmente. I gruppi di codice per questi assembly vengono definiti usando **StrongNameMembershipCondition**, che offre evidenza basata sulle informazioni relative alla chiave pubblica per il nome sicuro dell'assembly. Al gruppo di codice viene concesso il set di autorizzazioni **FullTrust**.  
  
## <a name="report-server-extensions-rendering-data-delivery-and-security"></a>Estensioni del server di report (rendering, dati, recapito e sicurezza)  
 Le estensioni del server di report sono estensioni per dati personalizzati, per il recapito, per il rendering e di sicurezza create dall'utente o da terze parti per estendere le funzionalità di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. È necessario concedere l'autorizzazione **FullTrust** a queste estensioni o al codice assembly nei file di configurazione dei criteri associati al componente di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] a cui viene applicata l'estensione. Le estensioni incluse in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sono firmate con la chiave pubblica del server di report e ricevono il set di autorizzazioni **FullTrust**.  
  
> [!IMPORTANT]  
>  Per concedere l'autorizzazione **FullTrust** alle estensioni di terze parti è necessario modificare i file di configurazione dei criteri di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Se non si aggiunge un gruppo di codice con autorizzazione **FullTrust** per le estensioni personalizzate, queste non potranno essere usate dal server di report.  
  
 Per altre informazioni sui file di configurazione dei criteri in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vedere [Uso di file di criteri di sicurezza di Reporting Services](../../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).  
  
## <a name="expressions-used-in-reports"></a>Espressioni usate nei report  
 Le espressioni del report sono espressioni di codice inline o metodi definiti dall'utente contenuti nell'elemento **Code** di un file RDL. Nei file dei criteri è disponibile un gruppo di codice già configurato che concede a queste espressioni il set di autorizzazioni **Execution** per impostazione predefinita. Il gruppo di codice è analogo al seguente:  
  
```  
<CodeGroup  
   class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="Execution"  
   Name="Report_Expressions_Default_Permissions"  
   Description="This code group grants default permissions for code in report expressions and Code element. ">  
    <IMembershipCondition  
       class="StrongNameMembershipCondition"  
       version="1"  
       PublicKeyBlob="002400..."  
    />  
</CodeGroup>  
```  
  
 L'autorizzazione **Execution** consente l'esecuzione del codice che non può tuttavia usare le risorse protette. Tutte le espressioni presenti all'interno di un report vengono compilate in un assembly (denominato assembly "delle espressioni") archiviato come parte del report compilato. Quando il report viene eseguito, il server di report carica l'assembly delle espressioni ed effettua chiamate in tale assembly per eseguire le espressioni. Gli assembly delle espressioni vengono firmati con una chiave specifica usata per definire il gruppo di codice per tutte le espressioni.  
  
 Le espressioni del report fanno riferimento a raccolte di modelli a oggetti del report (campi, parametri e così via) ed eseguono semplici attività, ad esempio operazioni aritmetiche e di stringa. Per il codice che esegue queste semplici operazioni è necessaria solo l'autorizzazione **Execution**. Per impostazione predefinita, ai metodi definiti dall'utente nell'elemento **Code** e agli assembly personalizzati viene concessa l'autorizzazione **Execution** in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Di conseguenza, per la maggior parte delle espressioni, non è necessario modificare alcun file di criteri di sicurezza per la configurazione corrente. Per concedere autorizzazioni aggiuntive agli assembly delle espressioni, è necessario che un amministratore modifichi i file di configurazione dei criteri del server di report e di Progettazione report e il gruppo di codice delle espressioni del report. Poiché è un'impostazione globale, la modifica di autorizzazioni predefinite per le espressioni influisce su tutti i report. Per tale ragione, è consigliabile posizionare tutto il codice per cui è necessario usare sicurezza aggiuntiva in un assembly personalizzato. Le autorizzazioni necessarie verranno concesse solo a tale assembly.  
  
> [!IMPORTANT]  
>  Affinché sia possibile usarlo nei report, il codice che effettua chiamate ad assembly esterni oppure a risorse protette deve essere incorporato in un assembly personalizzato. In questo modo è possibile esercitare maggiore controllo sulle autorizzazioni richieste e asserite dal codice. È consigliabile non effettuare chiamate ai metodi protetti all'interno dell'elemento **Code**. Per farlo è infatti necessario concedere l'autorizzazione **FullTrust** all'host delle espressioni del report e a tutto il codice personalizzato verrà concesso l'accesso completo a CLR.  
  
> [!CAUTION]  
>  Non concedere l'autorizzazione **FullTrust** al gruppo di codice per un host delle espressioni del report. In caso contrario, a tutte le espressioni del report viene consentito di effettuare chiamate di sistema protette.  
  
## <a name="custom-assemblies-referenced-in-reports"></a>Assembly personalizzati cui si fa riferimento nei report  
 Alcune espressioni del report possono chiamare assembly del codice di terze parti, denominati assembly personalizzati in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Per essere usati in un server di report, è necessario che a questi assembly sia concessa almeno l'autorizzazione **Execution** nei file di configurazione dei criteri. Per impostazione predefinita, i file dei criteri inclusi in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] concedono l'autorizzazione **Execution** a tutti gli assembly avviati dall'area Risorse del computer. È possibile concedere autorizzazioni aggiuntive agli assembly personalizzati in base alle proprie esigenze.  
  
 In alcuni casi, potrebbe essere necessario eseguire un'operazione che richiede autorizzazioni per il codice specifiche in un'espressione del report. Se si verifica questa situazione, in genere un'espressione del report deve effettuare una chiamata a un metodo della libreria CLR protetto, ad esempio un metodo che esegue l'accesso ai file oppure al Registro di sistema. Nella documentazione di [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] vengono descritte le autorizzazioni per il codice necessarie per effettuare la chiamata protetta. Al codice che esegue quest'ultima operazione è necessario concedere tali autorizzazioni di sicurezza specifiche. Se si effettua una chiamata da un'espressione del report o dall'elemento **Code** è necessario concedere all'assembly delle espressioni le autorizzazioni appropriate. Una volta concesse alle espressioni, tuttavia, le autorizzazioni specifiche vengono concesse a tutto il codice eseguito in tutte le espressioni di qualsiasi report. L'esecuzione della chiamata da un assembly personalizzato cui sono state concesse le autorizzazioni specifiche è molto più sicura a livello di sicurezza.  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza dall'accesso di codice in Reporting Services](../../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)   
 [Sviluppo sicuro &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
  
