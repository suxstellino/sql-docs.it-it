---
description: Sviluppo di un provider di log personalizzato
title: Sviluppo di un provider di log personalizzato | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- SSIS packages, log providers
- custom log providers [Integration Services]
- SQL Server Integration Services packages, log providers
- log providers [Integration Services]
- packages [Integration Services], logs
- Integration Services packages, log providers
ms.assetid: 3f715b95-7074-4f5c-8ae2-246998052e78
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ed384b5c9c83a1233167ff3c1caeaf3b37fb8070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354345"
---
# <a name="developing-a-custom-log-provider"></a>Sviluppo di un provider di log personalizzato

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] sono disponibili funzionalità di registrazione estese che consentono di acquisire eventi che si verificano durante l'esecuzione dei pacchetti. In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] è inclusa una varietà di provider di log che consentono di creare e archiviare log in formati quali XML, testo, database o nel registro eventi di Windows. Se i provider di log e i formati di output disponibili non soddisfano completamente specifici requisiti, è possibile creare un provider di log personalizzato.  
  
 A tale scopo, è necessario creare una classe che eredita dalla classe di base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>, applicare l'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> alla nuova classe ed eseguire l'override dei metodi e delle proprietà importanti della classe di base, tra cui la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> e il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 In questa sezione viene descritto come creare, configurare e scrivere codice per un provider di log personalizzato.  
  
 [Creazione di un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)  
 Viene descritto come creare le classi per un progetto di provider di log personalizzato.  
  
 [Scrittura del codice di un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)  
 Viene descritto come implementare un provider di log personalizzato eseguendo l'override dei metodi e delle proprietà della classe di base.  
  
 [Sviluppo di un'interfaccia utente per un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
 Le interfacce utente personalizzate per i provider di log personalizzati non sono supportate in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
## <a name="related-topics"></a>Argomenti correlati  
  
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
  
 [Sviluppo di una gestione connessione personalizzata](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Viene descritto come programmare gestioni connessioni personalizzate.  
  
 [Sviluppo di un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Viene descritto come programmare enumeratori personalizzati.  
  
 [Sviluppo di un componente flusso di dati personalizzato](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Viene descritto come programmare origini, trasformazioni e destinazioni personalizzate del flusso di dati.  
  
