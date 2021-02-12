---
title: Identificazione dello stato di esecuzione | Microsoft Docs
description: Informazioni su come usare Reporting Services per identificare lo stato di esecuzione in modo che sia possibile interagire con il report in diversi modi.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- session states [Reporting Services]
- lifetimes [Reporting Services]
- sessions [Reporting Services]
- SessionHeader SOAP header
ms.assetid: d8143a4b-08a1-4c38-9d00-8e50818ee380
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fbd189a85121d2ef18c69a1d7886f6b3c3294978
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045060"
---
# <a name="identifying-execution-state"></a>Identificazione dello stato di esecuzione
  HTTP (Hypertext Transfer Protocol) è un protocollo senza connessione e senza stato, ovvero non indica automaticamente se diverse richieste provengono dallo stesso client o se una singola istanza di un browser continua a visualizzare attivamente una pagina o un sito. Le sessioni creano una connessione logica per gestire lo stato tra server e client tramite HTTP. Le informazioni specifiche dell'utente relative a una particolare sessione sono note come stato della sessione.  
  
 La gestione della sessione implica la correlazione di una richiesta HTTP con le altre richieste precedenti generate dalla stessa sessione. In assenza di gestione della sessione, queste richieste appaiono non correlate al servizio Web ReportServer a causa della natura senza connessione e senza stato del protocollo HTTP.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non espone un concetto olistico di stato della sessione simile a quello esposto da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Quando, tuttavia, si eseguono i report, il server di report gestisce lo stato tra chiamate ai metodi sotto forma di **esecuzione**. Un'esecuzione consente all'utente di interagire con il report in diversi modi, ad esempio caricando il report dal server di report, impostando le credenziali e i parametri per il report ed eseguendo il rendering del report.  
  
 Mentre comunicano con un server di report, i client utilizzano l'esecuzione per gestire la visualizzazione dei report e la navigazione degli utenti ad altre pagine di un report, nonché per mostrare o nascondere le sezioni di un report. Per ogni report eseguito dall'applicazione client è disponibile un'unica esecuzione.  
  
 In generale, un'esecuzione inizia quando un utente passa a un browser o a un'applicazione client e seleziona un report da visualizzare. L'esecuzione viene eliminata dopo un breve periodo di timeout dopo la ricezione dell'ultima richiesta (l'intervallo di timeout predefinito è di 20 minuti).  
  
 Dal punto di vista del servizio Web, la durata ha inizio quando viene chiamato il metodo <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A>, <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> o <xref:ReportExecution2005.ReportExecutionService.Render%2A> del servizio Web ReportServer. Per modificare l'esecuzione attiva (ad esempio, impostando i parametri e le origini dati) possono venire utilizzati altri metodi. L'esecuzione viene eliminata dopo un breve periodo di timeout dopo la ricezione dell'ultima richiesta (l'intervallo di timeout predefinito è di 20 minuti).  
  
 Un'applicazione tiene traccia di più esecuzioni attive tra le chiamate ai metodi <xref:ReportExecution2005.ReportExecutionService.Render%2A> e <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A> del servizio Web, salvando l'oggetto <xref:ReportExecution2005.ExecutionHeader.ExecutionID%2A>, restituito nell'intestazione SOAP dai metodi <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> e <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A>.  
  
 Nel diagramma seguente vengono illustrati i percorsi di elaborazione e di rendering per i report.  
  
 ![Percorso di elaborazione/rendering del report](../../reporting-services/report-server-web-service-net-framework-soap-headers/media/rs-render-process-diagram.gif "Percorso di elaborazione/rendering del report")  
  
 Per supportare le funzioni descritte in precedenza, il metodo di rendering SOAP corrente è stato suddiviso in più metodi che includono le fasi di inizializzazione dell'esecuzione, elaborazione e rendering.  
  
 Per eseguire il rendering di un report a livello di programmazione, è necessario eseguire le operazioni seguenti:  
  
-   Caricare il report o la definizione del report utilizzando <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> o <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A>.  
  
-   Controllare se per il report sono necessari credenziali o parametri verificando i valori delle proprietà <xref:ReportExecution2005.ExecutionInfo.CredentialsRequired%2A> e <xref:ReportExecution2005.ExecutionInfo.ParametersRequired%2A> dell'oggetto <xref:ReportExecution2005.ExecutionInfo> restituito da <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> o <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A>  
  
-   Se necessario, impostare le credenziali e/o i parametri utilizzando i metodi <xref:ReportExecution2005.ReportExecutionService.SetExecutionCredentials%2A> e <xref:ReportExecution2005.ReportExecutionService.SetExecutionParameters%2A>.  
  
-   Chiamare il metodo <xref:ReportExecution2005.ReportExecutionService.Render%2A> per eseguire il rendering del report.  
  
 Durante la sessione di un report, il report sottostante archiviato nel database del server di report può cambiare. La definizione del report e le autorizzazioni utente possono cambiare oppure il report può essere eliminato o spostato. Se il report è in una sessione attiva, non viene interessato dalle modifiche apportate al report sottostante (ovvero il report archiviato nel database del server di report).  
  
 È anche possibile gestire una sessione del report utilizzando i comandi di accesso con URL.  
  
## <a name="see-also"></a>Vedere anche  
 <xref:ReportExecution2005.ReportExecutionService.Render%2A>   
 [Riferimento tecnico &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Uso di intestazioni SOAP di Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
