---
title: Implementazione di una classe Connection per un'estensione per l'elaborazione dati | Microsoft Docs
description: Implementare un oggetto Connection per l'estensione per l'elaborazione dati in Reporting Services. Vedere le interfacce da implementare e cosa richiedere ai client.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- connections [Reporting Services], data processing extensions
- Connection class
- data processing extensions [Reporting Services], connections
ms.assetid: 7047d29e-a2c9-4e6f-ad02-635851a38ed7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d5b0c6cc93ea6b934be718e88873cf736f31d679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061686"
---
# <a name="implementing-a-connection-class-for-a-data-processing-extension"></a>Implementazione di una classe Connection per un'estensione per l'elaborazione dati
  L'oggetto **Connection** rappresenta una connessione di database o una risorsa simile ed è il punto di partenza per gli utenti di un'estensione per l'elaborazione dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Questo oggetto rappresenta le connessioni ai server di database, sebbene qualsiasi entità con un comportamento simile possa essere esposta come oggetto **Connection**.  
  
 Per implementare un oggetto **Connection**, creare una classe che implementa <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> e, facoltativamente, <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>.  
  
 Nell'implementazione è necessario assicurarsi che prima dell'esecuzione dei comandi venga creata e aperta una connessione. Assicurarsi che l'implementazione richieda ai client di aprire e chiudere le connessioni in modo esplicito, anziché lasciare che l'implementazione apra e chiuda le connessioni in modo implicito per il client. Dopo aver stabilito la connessione, eseguire i controlli di sicurezza. Richiedendo una connessione esistente per le altre classi nell'estensione per l'elaborazione dati di [!INCLUDE[ssRS](../../../includes/ssrs.md)], è possibile garantire che vengano sempre eseguiti i controlli di sicurezza quando si utilizza l'origine dati.  
  
 Le proprietà della connessione desiderata vengono rappresentate come stringa di connessione. È consigliabile che l'estensione per l'elaborazione dati di [!INCLUDE[ssRS](../../../includes/ssrs.md)] supporti la proprietà <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection.ConnectionString%2A> utilizzando il sistema familiare di coppia nome/valore definito da OLE DB.  
  
> [!NOTE]  
>  Per ottenere gli oggetti **Connection** è spesso necessario un impiego elevato delle risorse, pertanto è consigliabile considerare la creazione di pool di connessioni o l'applicazione di altre tecniche che prevedano un minor sovraccarico delle risorse.  
  
 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> eredita da <xref:Microsoft.ReportingServices.Interfaces.IExtension>. È necessario implementare l'interfaccia <xref:Microsoft.ReportingServices.Interfaces.IExtension> come parte dell'implementazione della classe Connection. L'interfaccia <xref:Microsoft.ReportingServices.Interfaces.IExtension> consente a una classe di implementare un nome di estensione localizzato e di elaborare le informazioni di configurazione specifiche dell'estensione archiviate nel file di configurazione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 L'oggetto **Connection** contiene la proprietà <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A> tramite l'implementazione di <xref:Microsoft.ReportingServices.Interfaces.IExtension>. È consigliabile che le estensioni per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] supportino la proprietà <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A>, in modo che gli utenti possano visualizzare un nome familiare e localizzato per l'estensione in un'interfaccia utente, ad esempio Gestione report.  
  
 <xref:Microsoft.ReportingServices.Interfaces.IExtension> consente anche all'oggetto **Connection** di recuperare ed elaborare i dati di configurazione personalizzati archiviati nel file RSReportServer.config. Per ulteriori informazioni sull'elaborazione dei dati di configurazione personalizzati, vedere il metodo <xref:Microsoft.ReportingServices.Interfaces.IExtension.SetConfiguration%2A>.  
  
 La classe che implementa <xref:Microsoft.ReportingServices.Interfaces.IExtension> non viene scaricata dalla memoria quando vengono scaricate le altre classi delle estensioni per l'elaborazione dati. Per questo motivo, è possibile usare la classe **Extension** per archiviare le informazioni sullo stato tra le connessioni o i dati che possono essere memorizzati nella cache. La classe **Extension** rimane in memoria fino a quando il server di report è in esecuzione.  
  
 È possibile estendere la classe **Connection** per includere il supporto per le credenziali in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] implementando <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>. Quando si implementano le proprietà <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.IntegratedSecurity%2A>, <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.UserName%2A> e <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.Password%2A> dell'interfaccia <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>, si abilitano la casella di controllo **Sicurezza integrata** e le caselle di testo **Nome utente** e **Password** della finestra di dialogo **Origine dati** di Progettazione report. In questo modo, è possibile consentire a Progettazione report di archiviare e recuperare le credenziali per le origini dati che supportano l'autenticazione. Le credenziali vengono archiviate in modo protetto e utilizzate quando viene eseguito il rendering dei report in modalità di anteprima.  
  
> [!NOTE]  
>  L'implementazione di <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> richiede in modo implicito che vengano implementati i membri delle interfacce <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> e <xref:Microsoft.ReportingServices.Interfaces.IExtension>.  
>   
>  Per un'implementazione di esempio della classe **Connection**, vedere la pagina degli [esempi di prodotto SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementazione di un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
