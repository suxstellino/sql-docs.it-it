---
title: Traccia
description: Il file di Web.config contiene una sezione di traccia, nuova SQL Server Master Data Services 2016. Informazioni sul comportamento di traccia predefinito.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 45823fc8-723a-49f2-9a11-94d241245cfd
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: 8deacb8250d640e33140203d4d6d2c8eeb5d6acb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236856"
---
# <a name="tracing-master-data-services"></a>Traccia (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Il file Web.config contiene una sezione di traccia, come illustrato di seguito. Questa sezione è nuova in [!INCLUDE[sssql15-md](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]  
  
```  
<sources>  
      <!-- Adjust the switch value to control the types of messages that should be logged.   
           https://msdn.microsoft.com/library/system.diagnostics.sourcelevels  
           Use the a switchValue of Verbose to generate a full log. Please be aware that   
           the trace file can get quite large very quickly -->  
      <source name="MDS" switchType="System.Diagnostics.SourceSwitch" switchValue="Warning, ActivityTracing">  
        <listeners>  
          <!-- Set a directory path where the service account you chose while setting up Master Data Services has read and write privileges.  
               Default path is Logs in WebApplication folder, for example C:\Program Files\Microsoft SQL Server\130\Master Data Services\WebApplication  
               New log file will be created every day or every 10 mb.  
               When directory size hits the 200mb limitation, the oldest file will be deleted.-->  
          <add name="FileTraceListener"  
               type="Microsoft.MasterDataServices.Core.Logging.FileTraceListener, Microsoft.MasterDataServices.Core"   
               initializeData="DirectoryPath = Logs; FileSizeInMb = 10; MaxDirectorySizeInMb = 200"/>  
          <remove name="Default"/>  
        </listeners>  
      </source>  
    </sources>  
  
```  
  
 Di seguito viene illustrato il comportamento di traccia predefinito.  
  
-   La traccia è abilitata per i messaggi Warning e ActivityTracing.  
  
     Per altre informazioni, vedere [Enumerazione SourceLevels](/dotnet/api/system.diagnostics.sourcelevels).  
  
-   I log vengono salvati nella cartella dei log nella cartella WebApplication. Il percorso predefinito è C:\Programmi\Microsoft SQL Server\130\Master Data Services\WebApplication\Logs.  
  
-   Il file viene creato per ogni giorno o per ogni 10 MB.  
  
-   Quando le dimensioni della directory raggiungono i 200 MB, il log meno recente viene eliminato.  
  
-   Il formato del log è CSV. La tabella seguente descrive il formato del log.  
  
    |Elemento|Descrizione|  
    |-------------|-----------------|  
    |Tempo|La data e l'ora della voce di traccia.|  
    |CorrelationID|Per ogni richiesta viene assegnato un ID di correlazione. Tutte le tracce attivate dalla richiesta condividono lo stesso ID di correlazione.<br /><br /> Quando si verifica un errore nell'interfaccia utente, l'ID di correlazione viene visualizzato nel messaggio di errore.|  
    |Operazione|Nome dell'operazione di richiesta. Se la richiesta è una richiesta dell'interfaccia utente Web, il nome dell'operazione è l'URL. Se la richiesta è una richiesta API, il nome dell'operazione è il nome del servizio.|  
    |Level|Livello di questa voce di traccia.|  
    |Message|Corpo del messaggio della traccia|  
  
## <a name="external-resources"></a>Risorse esterne  
 Post di blog sulla [risoluzione dei problemi relativi al miglioramento della registrazione](https://techcommunity.microsoft.com/t5/sql-server-integration-services/troubleshooting-logging-improvement/ba-p/388214)su msdn.com.  
  
