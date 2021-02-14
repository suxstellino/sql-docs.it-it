---
description: Guida per gli sviluppatori
title: Documentazione per gli sviluppatori
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 067b1f69-84eb-4a13-b220-120cd63704b4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4062f365552fbbb2076c68f5bf6d3d912b81a490
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350107"
---
# <a name="master-data-services-developer-documentation"></a>Guida per gli sviluppatori

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Vengono fornite informazioni su come scrivere il codice per personalizzare la modalità di interazione degli utenti con [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Viene illustrato come:  
  
-   Scrivere un programma che acceda al servizio Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Il servizio Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] è un servizio di Windows Communication Foundation (WCF) che gli sviluppatori utilizzano per controllare le funzionalità di [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] tramite codice.  
  
-   Incorporare le funzionalità di [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] in applicazioni esistenti.  
  
-   Scrivere il codice per eseguire azioni ripetitive o complesse che sono difficili o impossibili da eseguire con l'interfaccia utente di [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].  
  
-   Creare un flusso di lavoro personalizzato eseguito in risposta a una regola business specificata. Un flusso di lavoro personalizzato chiama il codice scritto, che può eseguire qualsiasi azione richiesta per elaborare il flusso di lavoro.  
  
## <a name="master-data-manager-web-service"></a>Servizio Web Gestione dati master  
 Il servizio Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] consente di utilizzare a livello di codice le caratteristiche di [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] da qualsiasi computer che può accedere al sito Web di [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Prima di iniziare a scrivere il codice per accedere al servizio Web, è necessario generare le classi proxy, che sono contenute in uno spazio dei nomi specificato dall'utente. Nella presente documentazione viene utilizzato <xref:Microsoft.MasterDataServices> come spazio dei nomi del proxy. La classe proxy principale utilizzata per eseguire le operazioni del servizio Web è la classe <xref:Microsoft.MasterDataServices.ServiceClient>, che implementa l'interfaccia <xref:Microsoft.MasterDataServices.IService>. Dal codice, chiamare i metodi della classe <xref:Microsoft.MasterDataServices.ServiceClient> per accedere al servizio Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Il resto delle classi nello spazio dei nomi viene utilizzato dalle operazioni del servizio Web.  
  
### <a name="web-service-content"></a>Contenuto del servizio Web  
 [Creare le classi proxy del servizio Web Gestione dati master](../../master-data-services/develop/create-master-data-manager-web-service-proxy-classes.md)  
 Viene descritto come abilitare la pubblicazione dei metadati dal sito Web di [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] e come creare le classi proxy che possono essere utilizzate per accedere a livello di programmazione alle operazioni del servizio Web.  
  
 [Operazioni del servizio Web per categoria &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
 Elenco per categoria delle operazioni del servizio Web della classe <xref:Microsoft.MasterDataServices.ServiceClient>.  
  
## <a name="custom-workflows"></a>Flussi di lavoro personalizzati  
 In [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] vengono utilizzate le regole business per creare soluzioni di base per il flusso di lavoro. È possibile aggiornare e convalidare automaticamente i dati, nonché inviare notifiche mediante posta elettronica in base alle condizioni specificate. Le regole business in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] vengono utilizzate per gestire gli scenari di flussi di lavoro più comuni. Se il flusso di lavoro richiede l'elaborazione di eventi più complessi, ad esempio approvazioni multilivello o alberi delle decisioni complessi, è possibile configurare [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] in modo da inviare i dati a un assembly personalizzato creato. Per gestire i flussi di lavoro personalizzati, è necessario configurare e avviare SQL Server servizio di integrazione del flusso di lavoro MDS nel computer dell'applicazione Web e creare un assembly che implementi l'interfaccia [MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) .  
  
### <a name="custom-workflow-content"></a>Contenuto del flusso di lavoro personalizzato  
 [Creare un flusso di lavoro personalizzato &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-master-data-services.md)  
 Istruzioni sulla creazione di un assembly del gestore dei flussi di lavoro, sulla configurazione e l'avvio di SQL Server MDS Workflow Integration Service e sulla creazione di una regola business in [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] per avviare un flusso di lavoro personalizzato.  
  
## <a name="web-server-namespaces"></a>Spazi dei nomi del server Web  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] installa un set di assembly nel computer del server Web. Questi assembly contengono spazi dei nomi utilizzabili per scenari avanzati che consentono di personalizzare il comportamento del computer del server Web. Nella tabella seguente vengono descritti tali spazi dei nomi.  
  
|Spazio dei nomi|Descrizione|  
|---------------|-----------------|  
|[Microsoft. MasterDataServices. Deployment](/previous-versions/sql/sql-server-2016/ff487448(v=sql.130))|Contiene classi utilizzabili per creare un pacchetto di distribuzione da un modello e distribuire un pacchetto in un database [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].|  
|<xref:Microsoft.MasterDataServices.Services>|Contiene una classe che riceve ed elabora operazioni del servizio Web eseguite sul computer del server Web tramite l'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|  
|<xref:Microsoft.MasterDataServices.Services.DataContracts>|Contiene classi che definiscono le modalità di passaggio dei dati dal computer client attraverso l'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] al computer del server Web.|  
|<xref:Microsoft.MasterDataServices.Services.MessageContracts>|Contiene classi che definiscono le modalità di passaggio di richieste e risposte dal computer client attraverso l'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] al computer del server Web.|  
|<xref:Microsoft.MasterDataServices.Services.ServiceContracts>|Contiene l'interfaccia che definisce le operazioni che possono essere chiamate tramite il servizio Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|  
  
  
