---
description: Sviluppo di un'attività personalizzata
title: Sviluppo di un'attività personalizzata | Microsoft Docs
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
- custom tasks [Integration Services], about custom tasks
- Task class
- custom tasks [Integration Services]
- SSIS custom tasks
- SSIS custom tasks, about custom tasks
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- tasks [Integration Services], custom
- TaskHost object
ms.assetid: dcbd8615-fa6d-4ddb-b8a5-0b19dddd6239
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 37a5d7a613acbe6a8a9894c985ba3aee8ade53f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337601"
---
# <a name="developing-a-custom-task"></a>Sviluppo di un'attività personalizzata

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] vengono utilizzate attività per eseguire unità di lavoro a supporto dell'estrazione, della trasformazione e del caricamento dei dati. In [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] è inclusa una varietà di attività per l'esecuzione delle azioni più frequenti, dall'esecuzione di un'istruzione SQL al download di un file da un sito FTP. Se le attività incluse e le azioni supportate non soddisfano completamente specifici requisiti, è possibile creare un'attività personalizzata.  
  
 A tale scopo, è necessario creare una classe che eredita dalla classe di base [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task), applicare l'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> alla nuova classe ed eseguire l'override dei metodi e delle proprietà importanti della classe di base, tra cui il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 In questa sezione viene descritto come creare, configurare e scrivere il codice di un'attività personalizzata e della relativa interfaccia utente personalizzata facoltativa.  
  
 [Creazione di un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)  
 Viene descritto il primo passaggio, ovvero la creazione dell'attività personalizzata.  
  
 [Scrittura del codice di un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)  
 Viene descritto come scrivere il codice dei principali metodi di un'attività personalizzata.  
  
 [Connessione alle origini dati in un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/connecting-to-data-sources-in-a-custom-task.md)  
 Viene descritto come connettere un'attività personalizzata a un'origine dati.  
  
 [Generazione e definizione di eventi in un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/raising-and-defining-events-in-a-custom-task.md)  
 Viene descritto come generare eventi e definire eventi personalizzati dall'attività personalizzata.  
  
 [Aggiunta di supporto per il debug in un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/adding-support-for-debugging-in-a-custom-task.md)  
 Viene descritto come creare destinazioni di punti di interruzione nell'attività personalizzata.  
  
 [Sviluppo di un'interfaccia utente per un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
 Viene descritto come creare un'interfaccia utente visualizzata in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] per configurare le proprietà nell'attività personalizzata.  
  
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
  
 [Sviluppo di una gestione connessione personalizzata](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Viene descritto come programmare gestioni connessioni personalizzate.  
  
 [Sviluppo di un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Viene descritto come programmare provider di log personalizzati.  
  
 [Sviluppo di un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Viene descritto come programmare enumeratori personalizzati.  
  
 [Sviluppo di un componente flusso di dati personalizzato](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Viene descritto come programmare origini, trasformazioni e destinazioni personalizzate del flusso di dati.  
  
## <a name="see-also"></a>Vedere anche  
 [Estensione del pacchetto con l'attività Script](../../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)   
 [Confronto tra soluzioni di scripting e oggetti personalizzati](../../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md)  
  
  
