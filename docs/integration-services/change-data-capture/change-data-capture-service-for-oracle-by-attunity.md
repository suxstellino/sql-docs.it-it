---
description: Servizio Change Data Capture per Oracle di Attunity
title: Servizio Change Data Capture per Oracle di Attunity | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 22ec8a5c-9550-4d38-8a4a-485ec3e53ea8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 65f610b26444f86de750b3f21b175df4fedc3792
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813162"
---
# <a name="change-data-capture-service-for-oracle-by-attunity"></a>Servizio Change Data Capture per Oracle di Attunity

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Il servizio CDC per Oracle è un servizio Windows tramite cui vengono analizzati i log delle transazioni Oracle e acquisite le modifiche apportate alle tabelle Oracle di interesse nelle tabelle delle modifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le tabelle delle modifiche SQL in cui vengono archiviate le modifiche acquisite da Oracle sono dello stesso tipo delle tabelle delle modifiche usate nella funzionalità Change Data Capture nativa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questo modo l'utilizzo delle modifiche è facile quanto l'utilizzo delle modifiche apportate ai database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="installation"></a>Installazione  

Scaricare la finestra di progettazione e il servizio Microsoft Change Data Capture per Oracle di Attunity per la versione di SQL Server corrispondente dai collegamenti seguenti:

- [Microsoft SQL Server 2012 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=51606)
- [Microsoft SQL Server 2016 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=55802)
- [Microsoft SQL Server 2017 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=56610)
- [Microsoft SQL Server 2019 Integration Services Feature Pack](https://www.microsoft.com/download/details.aspx?id=100303)
  
 È possibile installare il servizio CDC per Oracle in qualsiasi computer Windows supportato con accesso al o ai database Oracle di origine che vengono acquisiti e all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione in cui si trova il database CDC di destinazione. Per il servizio CDC non è necessaria un'installazione locale del database Oracle o del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ma solo dei relativi client supportati. Per altre informazioni sul percorso di installazione dei componenti di database richiesti, vedere **Prerequisiti del database** in questo argomento.  
  
 Con l'installazione del servizio CDC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per Oracle l'interfaccia utente della configurazione del servizio e il programma del servizio vengono posizionati nel percorso selezionato. Il servizio CDC per Oracle viene configurato separatamente tramite Oracle CDC Service Configuration Console. Per altre informazioni sulla configurazione del servizio Oracle CDC, vedere [Guida del servizio Change Data Capture per Oracle di Attunity](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md).  
  
 È possibile installare il servizio CDC per Oracle in qualsiasi computer Windows supportato in cui sia installato il client nativo di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ; non è necessario che sia installato nello stesso computer in cui si trova l'istanza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione.  
  
## <a name="supported-windows-environments"></a>Ambienti Windows supportati  
 È possibile eseguire il servizio Change Data Capture per Oracle di Attunity negli ambienti Windows seguenti:  
  
-   Windows 8 e 8.1  
-   Windows 10  
-   Windows Server 2012 e 2012 R2
-   Windows Server 2016
  
## <a name="database-prerequisites"></a>Prerequisiti del database  
 Per usare il servizio CDC per Oracle, è necessario installare il client Oracle compatibile con la versione del database Oracle. Si tratta di un prerequisito che è necessario ottenere da Oracle e installare prima dell'installazione del servizio Oracle CDC. Inoltre, è necessario installare il client ODBC di SQL Server usando il programma di installazione di SQL Server.  
  
 Il servizio CDC per Oracle supporta le versioni seguenti:  
  
### <a name="source-oracle-database"></a>Database Oracle di origine  
  
-   Oracle Database 10g Release 2
-   Oracle Database 11g Release 1 e Release 2
-   Oracle Database 12c nell'installazione classica. L'installazione multi-tenant non è supportata.  
-   Oracle Database 18c nell'installazione classica. L'installazione multi-tenant non è supportata. 
-   Oracle Database 19c nell'installazione classica. L'installazione multi-tenant non è supportata. 
  
### <a name="target-sql-server-database"></a>Database di SQL Server di destinazione  
 Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
## <a name="running-the-installation-program"></a>Esecuzione del programma di installazione  
 Per installare il servizio CDC per Oracle, aprire l'Installazione guidata per la piattaforma Windows in uso (32/64 bit) e attenersi alle indicazioni visualizzate sullo schermo.  
  
## <a name="uninstalling-change-data-capture-service-for-oracle-by-attunity"></a>Disinstallazione del servizio Change Data Capture per Oracle di Attunity  
 Per disinstallare il servizio CDC per Oracle, scegliere Pannello di controllo, Programmi e funzionalità.  
  
 Con la disinstallazione del servizio CDC, non vengono eliminati i database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creati. Per la rimozione completa dello strumento, è necessario rimuovere il database MSXDBCDC e i database CDC specifici creati nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione usata.  
  
 Se si disinstalla il software del servizio CDC da un computer e lo si installa in un altro computer, è sufficiente fornire le definizioni seguenti:  
  
-   Account di servizio  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - Stringa di connessione e credenziali  
  
-   Master password  
  
 Tutte le altre definizioni vengono archiviate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e sono disponibile dall'installazione precedente in un altro computer.  
  
## <a name="in-this-documentation"></a>Contenuto della documentazione  
  
-   [Architettura di sistema del servizio Change Data Capture per Oracle di Attunity](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-system-architecture.md)  
  
-   [Servizio Oracle CDC](../../integration-services/change-data-capture/the-oracle-cdc-service.md)  
  
-   [Guida del servizio Change Data Capture per Oracle di Attunity](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md)  
  
-   [Guida procedurale del servizio Change Data Capture per Oracle di Attunity](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-how-to-guide.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo del servizio Oracle CDC](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md)  
  
  
