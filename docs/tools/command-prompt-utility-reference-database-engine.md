---
title: Utilità del prompt dei comandi SQL (motore di database)
description: Le utilità del prompt dei comandi consentono di generare script di operazioni SQL Server. Questo articolo descrive numerose utilità del prompt dei comandi incluse in SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server]
- command prompt utilities [SQL Server], about command prompt utilities
- command prompt [SQL Server]
- utilities [SQL Server], command prompt
- command prompt [SQL Server], utilities
ms.assetid: 48364bd9-6ea7-45e9-a332-acf3d81bbfae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 39e73066aad7c8913225c9b7c0e06f123e53a123
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644163"
---
# <a name="sql-command-prompt-utilities-database-engine"></a>Utilità del prompt dei comandi SQL (motore di database)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Le utilità del prompt dei comandi consentono di generare script di operazioni [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La tabella seguente contiene un elenco delle utilità del prompt dei comandi fornite con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Per informazioni sui *principali* strumenti dell'interfaccia utente grafica e della riga di comando di SQL, vedere [Panoramica degli strumenti di SQL](overview-sql-tools.md).

|**Utilità**|**Descrizione**|**Posizione di installazione**|  
|-----------------|---------------------|----------------------|  
|[Utilità bcp](../tools/bcp-utility.md)|Usata per copiare i dati tra un'istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e un file di dati in un formato specificato dall'utente.|\<*drive*:>\Programmi\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[Utilità dta](../tools/dta/dta-utility.md)|Consente di analizzare un carico di lavoro e proporre strutture di progettazione fisica per ottimizzare le prestazioni del server per il carico di lavoro specifico.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità dtexec](../integration-services/packages/dtexec-utility.md)|Utilizzata per configurare ed eseguire un pacchetto di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Una versione di interfaccia utente di questa utilità del prompt dei comandi è denominata **DTExecUI** e visualizza l'Utilità di esecuzione pacchetti.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilità dtutil](../integration-services/dtutil-utility.md)|Consente di gestire i pacchetti SSIS.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Distribuire soluzioni di modelli con l'utilità di distribuzione](/analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility)|Consente di distribuire progetti di [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] in istanze di [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VShell\Common7\IDE|   
|[Utilità osql](../tools/osql-utility.md)|Consente di immettere istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] , procedure di sistema e file script al prompt dei comandi.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità profiler](../tools/profiler-utility.md)|Consente di avviare [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] dal prompt dei comandi.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità RS.exe &#40;SSRS&#41;](../reporting-services/tools/rs-exe-utility-ssrs.md)|Consente di eseguire script progettati per la gestione di server di report [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità rsconfig &#40;SSRS&#41;](../reporting-services/tools/rsconfig-utility-ssrs.md)|Consente di configurare una connessione a un server di report.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità rskeymgmt &#40;SSRS&#41;](../reporting-services/tools/rskeymgmt-utility-ssrs.md)|Consente di gestire le chiavi di crittografia in un server di report.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Applicazione sqlagent90](../tools/sqlagent90-application.md)|Utilizzata per avviare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent da un prompt dei comandi.|\<drive>:\Programmi\Microsoft SQL Server\\<*nome_istanza*>\MSSQL\Binn|  
|[Utilità sqlcmd](../tools/sqlcmd-utility.md)|Consente di immettere istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] , procedure di sistema e file script al prompt dei comandi.|\<*drive*:>\Programmi\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[Utilità SQLdiag](../tools/sqldiag-utility.md)|Utilizzata per raccogliere informazioni di diagnostica per il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Applicazione sqllogship](../tools/sqllogship-application.md)|Consente di eseguire operazioni di backup, copia e ripristino da altre applicazioni, nonché di eseguire le attività di pulizia associate per una configurazione per il log shipping, senza eseguire i processi di backup, copia e ripristino.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità SqlLocalDB](../tools/sqllocaldb-utility.md)|Modalità di esecuzione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] destinata agli sviluppatori.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità sqlmaint](../tools/sqlmaint-utility.md)|Utilizzata per eseguire i piani di manutenzione dei database creati nelle precedenti versioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|\<drive>:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilità sqlps](../tools/sqlps-utility.md)|Consente di eseguire comandi e script di PowerShell, nonché di caricare e registrare il provider e i cmdlet di PowerShell per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlservr](../tools/sqlservr-application.md)|Consente di avviare e arrestare un'istanza di [!INCLUDE[ssDE](../includes/ssde-md.md)] dal prompt dei comandi per la risoluzione dei problemi.|\<drive>:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilità Ssms](../ssms/ssms-utility.md)|Consente di avviare [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] dal prompt dei comandi.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VSShell\Common7\IDE|  
|[Utilità tablediff](../tools/tablediff-utility.md)|Consente di confrontare i dati di due tabelle e rilevare l'eventuale non convergenza dei dati. Risulta particolarmente utile per la risoluzione dei problemi relativi alla topologia di replica.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]COM|  

## <a name="command-prompt-utilities-syntax-conventions"></a>Convenzioni di sintassi per le utilità del del prompt dei comandi  
  
|**Convenzione**|**Usata per**|  
|--------------------|------------------|  
|LETTERE MAIUSCOLE|Istruzioni e termini utilizzati a livello di sistema operativo.|  
|`monospace`|Esempi di comandi e codice di programmazione.|  
|*corsivo*|Parametri specificati dall'utente.|  
|**grassetto**|Comandi, parametri e altri elementi della sintassi che devono essere digitati esattamente come indicato.|  

## <a name="see-also"></a>Vedere anche

* [Replication Distribution Agent](../relational-databases/replication/agents/replication-distribution-agent.md)
* [Agente lettura log repliche](../relational-databases/replication/agents/replication-log-reader-agent.md)
* [Replication Merge Agent](../relational-databases/replication/agents/replication-merge-agent.md)
* [Agente di lettura coda repliche](../relational-databases/replication/agents/replication-queue-reader-agent.md)
* [Replication Snapshot Agent](../relational-databases/replication/agents/replication-snapshot-agent.md)