---
description: Panoramica della programmazione di Integration Services
title: Panoramica della programmazione di Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Integration Services, programming
- architecture [Integration Services]
- assemblies [Integration Services]
- SSIS, programming
- SQL Server Integration Services, programming
- run-time engine [Integration Services]
- packages [Integration Services], programming
- data flow engine [Integration Services]
- languages [Integration Services]
ms.assetid: 262babc6-eea5-4609-bc65-07d64cbcfee9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae083f7e594025449da685ce5f18ed777f88a8a1
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835118"
---
# <a name="integration-services-programming-overview"></a>Panoramica della programmazione di Integration Services

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  L'architettura di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] separa lo spostamento e la trasformazione dei dati dal flusso di controllo e dalla gestione dei pacchetti. Due motori distinti definiscono questa architettura, che possono essere automatizzati ed estesi quando si programma [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Il motore di runtime implementa l'infrastruttura del flusso di controllo e di gestione dei pacchetti che consente agli sviluppatori di controllare il flusso di esecuzione e impostare le opzioni per la registrazione, la gestione degli eventi e le variabili. Il motore flusso di dati è un motore speciale, a elevate prestazioni, dedicato esclusivamente all'estrazione, alla trasformazione e al caricamento di dati. Quando si programma [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], si utilizzeranno questi due motori.  
  
 Nell'immagine seguente è illustrata l'architettura di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
 ![Architettura di Integration Services](../integration-services/media/mw-dts-01.gif "Architettura di Integration Services")  
  
## <a name="integration-services-run-time-engine"></a>Motore di runtime di Integration Services  
 Il motore di runtime di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] controlla la gestione e l'esecuzione di pacchetti, implementando l'infrastruttura che abilita l'ordine di esecuzione, la registrazione, le variabili e la gestione degli eventi. Programmando il motore di runtime di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], gli sviluppatori possono automatizzare la creazione, la configurazione e l'esecuzione di pacchetti, nonché creare attività personalizzate e altre estensioni.  
  
 Per altre informazioni, vedere [Estensione del pacchetto con l'attività Script](../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md), [Sviluppo di un'attività personalizzata](../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md) e [Compilazione di pacchetti a livello di programmazione](../integration-services/building-packages-programmatically/building-packages-programmatically.md).  
  
## <a name="integration-services-data-flow-engine"></a>Motore flusso di dati di Integration Services  
 Il motore flusso di dati gestisce l'attività Flusso di dati, un'attività speciale a elevate prestazioni dedicata allo spostamento e alla trasformazione di dati da origini disparate. A differenza di altre attività, l'attività Flusso di dati contiene oggetti aggiuntivi, i componenti flusso di dati, che possono essere origini, trasformazioni o destinazioni. Questi componenti costituiscono le parti mobili principali dell'attività. Definiscono lo spostamento e la trasformazione dei dati. Programmando il motore flusso di dati, gli sviluppatori possono automatizzare la creazione e la configurazione dei componenti in un'attività Flusso di dati, nonché creare componenti personalizzati.  
  
 Per altre informazioni, vedere [Estensione del flusso di dati con il componente script](../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md), [Sviluppo di un componente flusso di dati personalizzato](../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) e [Compilazione di pacchetti a livello di programmazione](../integration-services/building-packages-programmatically/building-packages-programmatically.md).  
  
## <a name="supported-languages"></a>Lingue supportate  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] include il supporto completo di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)]. In questo modo gli sviluppatori possono programmare [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] nel linguaggio conforme a .NET che preferiscono. Sebbene il motore di runtime e il motore flusso di dati siano scritti in codice nativo, sono entrambi disponibili tramite un modello a oggetti completamente gestito.  
  
 È possibile programmare pacchetti di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], attività personalizzate e componenti in [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] o in un altro editor di codice o di testo. [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] offre allo sviluppatore numerosi strumenti e funzionalità per semplificare e accelerare i cicli iterativi di codifica, debug e test. [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] semplifica la distribuzione. Tuttavia, non è necessario [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] per compilare e ottenere progetti di codice di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK include i compilatori [!INCLUDE[vbprvb](../includes/vbprvb-md.md)] e [!INCLUDE[csprcs](../includes/csprcs-md.md)] e gli strumenti correlati.  
  
> [!IMPORTANT]  
>  Per impostazione predefinita, [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] viene installato con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], a differenza di [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK. Se l'SDK non è installato nel computer e la documentazione associata non è inclusa nella documentazione online, non è possibile utilizzare i collegamenti al contenuto dell'SDK presenti in questa sezione. Dopo aver installato [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK, è possibile aggiungere la documentazione associata alla documentazione online e al sommario attenendosi alle istruzioni descritte in [Aggiungere o rimuovere la documentazione del prodotto per SQL Server](https://www.microsoft.com/download/details.aspx?id=42557).  
  
 L'attività Script e il componente script di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] usano [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA) come ambiente di scripting incorporato. VSTA support [!INCLUDE[msCoName](../includes/msconame-md.md)] Visual Basic e [!INCLUDE[msCoName](../includes/msconame-md.md)] Visual C#  
  
> [!NOTE]  
>  Le API di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] non sono compatibili con linguaggi di scripting basati su COM come VBScript.  
  
## <a name="locating-assemblies"></a>Individuazione di assembly  
 In [!INCLUDE[ssSQL19](../includes/sssql19-md.md)]gli assembly [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sono stati aggiornati a .NET 4.0. È disponibile una Global Assembly Cache separata per .NET 4 in *\<drive>* :\Windows\Microsoft.NET\assembly. Tutti gli assembly di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] possono essere individuati in questo percorso, generalmente nella cartella GAC_MSIL.  
  
 Come nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], i file DLL di estendibilità di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] principali sono anche disponibili in *\<drive>* :\Programmi\Microsoft SQL Server\100\SDK\Assemblies.  
  
## <a name="commonly-used-assemblies"></a>Assembly di uso comune  
 Nella tabella seguente sono elencati gli assembly utilizzati di frequente quando si programma [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] utilizzando [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)].  
  
|Assembly|Descrizione|  
|--------------|-----------------|  
|Microsoft.SqlServer.ManagedDTS.dll|Contiene il motore di runtime gestito.|  
|Microsoft.SqlServer.RuntimeWrapper.dll|Contiene l'assembly di interoperabilità primario, o wrapper, per il motore di runtime nativo.|  
|Microsoft.SqlServer.PipelineHost.dll|Contiene il motore flusso di dati gestito.|  
|Microsoft.SqlServer.PipelineWrapper.dll|Contiene l'assembly di interoperabilità primario, o wrapper, per il motore flusso di dati nativo.|  
  
  
