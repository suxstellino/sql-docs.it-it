---
title: Eseguire SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Informazioni sui programmi e sui menu dai quali è possibile avviare SQL Server Profiler e informazioni su contesti di connessione, modelli e filtri usati con l'output di traccia.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 22e57ffa-63b0-4de3-b92e-df297dda1226
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 9b4bf16638b361059bf82cb090a1705bb8cf103b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354446"
---
# <a name="run-sql-server-profiler"></a>Eseguire SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

È possibile eseguire [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] in modi diversi per supportare la raccolta dell'output di traccia in svariati scenari. È possibile avviare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] dal menu **Start** di Windows 10, dal menu **Strumenti** in Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] e da diversi punti all'interno di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Quando si avvia [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per la prima volta e si sceglie **Nuova traccia** dal menu **File **, l'applicazione visualizza la finestra di dialogo** Connetti al server[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è possibile specificare l'istanza di**  a cui connettersi.  
## <a name="to-start-sql-server-profiler-from-the-windows-10-start-menu"></a>Per avviare SQL Server Profiler dal menu Start di Windows 10  
-  Fare clic sull'icona **Start** di Windows oppure premere il tasto WINDOWS e digitare "SQL Server Profiler 17". Fare clic, quando viene visualizzato il riquadro **SQL Server Profiler 17**.   

## <a name="to-start-sql-server-profiler-in-database-engine-tuning-advisor"></a>Per avviare SQL Server Profiler in Ottimizzazione guidata motore di database  
-  Selezionare [!INCLUDE[ssDE](../../includes/ssde-md.md)] SQL Server Profiler **dal menu** Tools **di Ottimizzazione guidata**.  

## <a name="to-start-sql-server-profiler-in-sql-server-management-studio"></a>Avviare SQL Server Profiler in SQL Server Management Studio  
 È possibile avviare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] da diverse posizioni in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Quando [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] viene avviato, carica il contesto di connessione, il modello di traccia e il contesto del filtro del punto di avvio. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Avvia ogni sessione di SQL Server Profiler nella rispettiva istanza e Profiler continua a essere eseguito se si arresta [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
### <a name="to-start-sql-server-profiler-from-the-tools-menu"></a>Per avviare SQL Server Profiler dal menu Strumenti  
-  Dal menu **Strumenti** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] scegliere **SQL Server Profiler**.  

### <a name="to-start-sql-server-profiler-from-the-query-editor"></a>Per avviare SQL Server Profiler dall'editor di query  
- In Editor di query fare clic con il pulsante destro del mouse e scegliere **Traccia query in SQL Server Profiler**.  

  > [!NOTE]  
  >  Il contesto di connessione è la connessione dell'editor, il modello di traccia è TSQL_SPs e il filtro applicato è SPID = finestra della query SPID.  
    
### <a name="to-start-sql-server-profiler-from-activity-monitor"></a>Per avviare SQL Server Profiler da Monitor attività  
- In Monitoraggio attività fare clic sul riquadro **Processi**, fare clic con il pulsante destro del mouse sul processo di cui si vuole modificare il profilo e quindi scegliere **Processo di traccia in SQL Server Profiler**.  

    > [!NOTE]  
    >  Quando viene selezionato un processo, il contesto di connessione è la connessione Esplora oggetti stabilita all'apertura di Monitor attività. Il modello di traccia è l'impostazione predefinita basata sul tipo di server e lo SPID corrisponde a quello del processo selezionato.  
    
## <a name="net-framework-security"></a>Sicurezza di .NET Framework  
- Nella modalità di autenticazione di Windows l'account utente usato per l'esecuzione di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] deve avere l'autorizzazione per la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- Per eseguire tracce con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], gli utenti devono inoltre possedere l'autorizzazione ALTER TRACE.  

## <a name="next-steps"></a>Passaggi successivi  
 [Panoramica di SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Utilizzo di SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)