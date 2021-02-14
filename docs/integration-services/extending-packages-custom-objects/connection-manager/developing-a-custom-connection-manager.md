---
description: Sviluppo di una gestione connessione personalizzata
title: Sviluppo di una gestione connessione personalizzata | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], connections
- custom connection managers [Integration Services], about custom connection managers
- connection managers [Integration Services], custom
- Integration Services packages, connection managers
- SSIS packages, connection managers
- SQL Server Integration Services packages, connection managers
- custom connection managers [Integration Services]
ms.assetid: bda0b29e-57f5-4879-b04d-1396dc56daa8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d92ee03836944fd3a98f89c86369d2c442ee6ea5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348924"
---
# <a name="developing-a-custom-connection-manager"></a>Sviluppo di una gestione connessione personalizzata

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] le gestioni connessioni vengono utilizzate per incapsulare le informazioni necessarie per la connessione a un'origine dati esterna. In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] è inclusa una varietà di gestioni connessione che supportano connessioni alle origini dati più utilizzate, dai database aziendali a file di testo e fogli di lavoro di Excel. Se le gestioni connessioni e le origini dati esterne supportate da [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] non soddisfano completamente specifici requisiti, è possibile creare una gestione connessione personalizzata.  
  
 Per creare una gestione connessione personalizzata, è necessario creare una classe che eredita dalla classe di base <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>, applicare l'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> alla nuova classe ed eseguire l'override dei metodi e delle proprietà importanti della classe di base, tra cui la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> e il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>.  
  
> [!IMPORTANT]  
>  La maggior parte delle attività, delle origini e delle destinazioni incluse in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] funziona solo con tipi specifici di gestioni connessioni predefinite. Prima di sviluppare una gestione connessione personalizzata da utilizzare con attività e componenti predefiniti, controllare se tali componenti restringono l'elenco di gestioni connessioni rendendo disponibili solo quelli di un tipo specifico. Se la soluzione richiede una gestione connessione personalizzata, potrebbe essere necessario sviluppare anche un'attività personalizzata oppure un'origine o una destinazione personalizzata da utilizzare con la gestione connessione.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 In questa sezione viene descritto come creare, configurare e scrivere il codice di una gestione connessione personalizzata e della relativa interfaccia utente personalizzata facoltativa. I frammenti di codice illustrati in questa sezione sono tratti dall'esempio di gestione connessione personalizzata SQL Server.  
  
 [Creazione di una gestione connessione personalizzata](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)  
 Viene descritto come creare le classi per un progetto di gestione connessione personalizzata.  
  
 [Scrittura del codice di una gestione connessione personalizzata](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)  
 Viene descritto come implementare una gestione connessione personalizzata eseguendo l'override dei metodi e delle proprietà della classe di base.  
  
 [Sviluppo di un'interfaccia utente per una gestione connessione personalizzata](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
 Viene descritto come implementare la classe dell'interfaccia utente e il form utilizzato per configurare la gestione connessione personalizzata.  
  
## <a name="related-sections"></a>Sezioni correlate  
  
### <a name="information-common-to-all-custom-objects"></a>Informazioni comuni per tutti gli oggetti personalizzati  
 Per informazioni comuni a tutti i tipi di oggetti personalizzati che è possibile creare in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vedere gli argomenti seguenti:  
  
 [Sviluppo di oggetti personalizzati per Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 Vengono descritti i passaggi di base per implementare tutti i tipi di oggetti personalizzati in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 [Persistenza degli oggetti personalizzati](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 Viene descritta la persistenza personalizzata e vengono illustrati i casi in cui è necessaria.  
  
 [Compilazione, distribuzione e debug di oggetti personalizzati](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 Vengono descritte le tecniche per la compilazione, la firma, la distribuzione e il debug di oggetti personalizzati.  
  
### <a name="information-about-other-custom-objects"></a>Informazioni su altri oggetti personalizzati  
 Per informazioni sugli altri tipi di oggetti personalizzati che è possibile creare in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vedere gli argomenti seguenti:  
  
 [Sviluppo di un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)  
 Viene descritto come programmare attività personalizzate.  
  
 [Sviluppo di un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Viene descritto come programmare provider di log personalizzati.  
  
 [Sviluppo di un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Viene descritto come programmare enumeratori personalizzati.  
  
 [Sviluppo di un componente flusso di dati personalizzato](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Viene descritto come programmare origini, trasformazioni e destinazioni personalizzate del flusso di dati.  
  
