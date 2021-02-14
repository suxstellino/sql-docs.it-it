---
description: Gestione dei pacchetti in esecuzione a livello di programmazione
title: Gestione dei pacchetti in esecuzione a livello di programmazione| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], managing
- running packages [Integration Services]
ms.assetid: 0e91f4ac-6f29-40d7-8c28-9b82e4802c35
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e73d7c3c29ab5e22f45795b9746baedf00bcb413
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347442"
---
# <a name="managing-running-packages-programmatically"></a>Gestione dei pacchetti in esecuzione a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Quando si utilizzano i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] a livello di programmazione, può essere necessario determinare quali sono attualmente in esecuzione. La classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> dello spazio dei nomi <xref:Microsoft.SqlServer.Dts.Runtime> fornisce metodi e classi per soddisfare questi requisiti.  
  
 Per altre informazioni sul monitoraggio dei pacchetti, vedere [Gestione dei pacchetti &#40;servizio SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 Tutti i metodi descritti in questo argomento richiedono un riferimento all'assembly **Microsoft.SqlServer.ManagedDTS**. Dopo aver aggiunto il riferimento in un nuovo progetto, importare lo spazio dei nomi <xref:Microsoft.SqlServer.Dts.Runtime> con un'istruzione **using** o **Imports**.  
  
> [!IMPORTANT]  
>  I metodi della classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> per l'utilizzo dell'archivio pacchetti SSIS supportano solo ".", localhost o il nome del server locale. Non è possibile utilizzare "(local)".  
  
## <a name="determining-which-packages-are-currently-running"></a>Identificazione dei pacchetti in esecuzione  
 Per determinare quali pacchetti sono attualmente in esecuzione in un server specificato, chiamare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Application.GetRunningPackages%2A>. Il metodo restituisce una raccolta <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages> di oggetti <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage>.  
  
> [!NOTE]  
>  Gli amministratori vedono tutti i pacchetti attualmente in esecuzione nel computer, mentre gli altri utenti vedono solo quelli che hanno avviato.  
  
## <a name="working-with-running-packages"></a>Utilizzo dei pacchetti in esecuzione  
 Dopo aver determinato quali pacchetti sono attualmente in esecuzione, è possibile recuperare le relative informazioni e richiedere l'arresto di un pacchetto.  
  
### <a name="getting-information-about-a-running-package"></a>Recupero di informazioni su un pacchetto in esecuzione  
 Mentre si scorre la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages>, è possibile utilizzare le proprietà dell'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> per individuare un pacchetto o per ottenere ulteriori informazioni sui pacchetti in esecuzione:  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionDuration%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionStartTime%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.InstanceID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageDescription%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageName%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.UserName%2A>  
  
### <a name="stopping-a-running-package"></a>Arresto di un pacchetto in esecuzione  
 È possibile chiamare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.Stop%2A> di un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> per richiedere l'arresto del pacchetto. È possibile che si verifichi un ritardo tra il momento in cui viene emessa una richiesta di arresto e il momento dell'arresto effettivo del pacchetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestione dei pacchetti &#40;servizio SSIS&#41;](../../integration-services/service/package-management-ssis-service.md)   
 [Enumerazione dei pacchetti disponibili a livello di codice](../../integration-services/run-manage-packages-programmatically/enumerating-available-packages-programmatically.md)  
  
  
