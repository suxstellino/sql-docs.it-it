---
title: Creare un database del server di report, Gestione configurazione | Microsoft Docs
description: La modalità nativa di SQL Server Reporting Services usa due database relazionali di SQL Server per l'archiviazione di metadati e oggetti del server di report. Un database è utilizzato per l'archiviazione primaria e l'altro per l'archiviazione dei dati temporanei.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 9/2/2020
ms.openlocfilehash: 70a92bf37285e9af58dcef2e7a69abfb88b37178
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067096"
---
# <a name="create-a-report-server-database-report-server-configuration-manager"></a>Creare un database del server di report (Gestione configurazione del server di report)  

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

La modalità nativa di SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa due database relazionali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'archiviazione di metadati e oggetti del server di report. Un database è utilizzato per l'archiviazione primaria e l'altro per l'archiviazione dei dati temporanei. 

I database vengono creati assieme e associati in base al nome. Con un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] predefinita, i database sono denominati **reportserver** e **reportservertempdb**. I due database vengono detti collettivamente **database del server di report** o **catalogo del server di report**.

::: moniker range="=sql-server-2016"

La [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]modalità SharePoint **di SQL Server** include un terzo database usato per i metadati di avviso dei dati. Vengono creati tre database per ciascuna applicazione di servizio SSRS. I nomi dei database includono per impostazione predefinita un GUID che rappresenta l'applicazione di servizio. 

Di seguito sono riportati nomi di esempio dei tre database della modalità SharePoint:

- ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  

::: moniker-end
  
> [!IMPORTANT]  
> Non scrivere applicazioni che eseguono query sul database del server di report. Il database del server di report non è uno schema pubblico. La struttura della tabella potrebbe cambiare da una versione all'altra. Se si scrive un'applicazione che richiede l'accesso al database del server di report, usare sempre le API di SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per accedere al database del server di report.  
>
> Le viste del log di esecuzione rappresentano un'eccezione a questa regola. Per altre informazioni, vedere [Vista ExecutionLog ed ExecutionLog3 del server di report](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
## <a name="ways-to-create-the-report-server-database"></a>Modi per creare il database del server di report

 ### <a name="native-mode"></a>Modalità nativa
 È possibile creare il database del server di report in modalità nativa nei modi seguenti:  
  
- **Automatico** Usare la configurazione guidata di SQL Server, se si sceglie l'opzione di configurazione predefinita per l'installazione. Nell'Installazione guidata di SQL Server, si tratta dell'opzione **Installazione e configurazione** nella pagina delle **opzioni di installazione del server di report**. Se si sceglie l'opzione **Solo installazione**, per creare il database è necessario usare Gestione configurazione del server di report di SQL Server.  
  
- **Manuale**. Usare Gestione configurazione SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Creare manualmente il database del server di report se per ospitare il database si usa un [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] remoto. Per altre informazioni, vedere [Creare un database del server di report in modalità nativa](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-mode"></a>Modalità SharePoint 
Nella pagina delle **opzioni di installazione del server di report** è disponibile solo l'opzione **Solo installazione** per la modalità SharePoint. Questa opzione consente di installare tutti i file di SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e il servizio condiviso SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Il passaggio successivo prevede la creazione di almeno un'applicazione di servizio SSRS in una delle modalità seguenti:  
  
- Passare ad Amministrazione centrale SharePoint per creare un'applicazione di servizio SSRS. Per altre informazioni, vedere la sezione **Creare un'applicazione di servizio** in [Installare il primo server di report in modalità SharePoint](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication).  
  
- Usare i cmdlet PowerShell di SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per creare un'applicazione di servizio e i database del server di report. Per altre informazioni, vedere l'esempio per la creazione di applicazioni di servizio nell'argomento [Cmdlet di PowerShell per la modalità SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  

::: moniker-end
  
## <a name="database-server-version-requirements"></a>Requisiti relativi alla versione del server di database

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene usato per ospitare i database del server di report. L'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] può essere locale o remota. Le versioni supportate seguenti di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] possono ospitare i database del server di report:  
::: moniker range=">=sql-server-ver15"

- Istanza gestita di SQL di Azure

- SQL Server 2019

::: moniker-end
::: moniker range=">=sql-server-2017"

- SQL Server 2017  
::: moniker-end

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

Se s crea il database del server di report in un computer remoto, configurare la connessione per l'uso di un account utente di dominio o un account di servizio con accesso alla rete. Se si usa un'istanza remota di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], valutare attentamente le credenziali che il server di report dovrà usare per connettersi all'istanza. Per altre informazioni, vedere [Configurare una connessione del database del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
> [!IMPORTANT]  
> Il server di report e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita il database del server di report possono trovarsi in domini diversi. Per la distribuzione in Internet, è pratica comune usare un server protetto da firewall. 
>
> Se si configura un server di report per l'accesso a Internet, usare credenziali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] protetta dal firewall. Proteggere la connessione con IPSec.  
  
## <a name="edition-requirements-for-a-database-server"></a>Requisiti relativi all'edizione per un server di database 

 Quando si crea un database del server di report, non tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere usate per ospitare il database. Per altre informazioni, vedere [Requisiti relativi all'edizione per il database del server di report](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md#edition-requirements-for-the-report-server-database) in [Funzionalità di SQL Server Reporting Services supportate nelle diverse edizioni](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).  

## <a name="next-steps"></a>Passaggi successivi

Leggere le informazioni su [Gestione configurazione del server di report](reporting-services-configuration-manager-native-mode.md).  

Altre domande? Visitare il [forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).