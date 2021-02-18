---
title: Modelli
titleSuffix: SQl Server Profiler
description: Informazioni sui modelli predefiniti forniti da SQL Server Profiler e su come usarli. Informazioni su come creare modelli definiti dall'utente e modificare il modello predefinito.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: b674e491-dc58-47a1-acdd-7028e9a201fc
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: ef486049c3a50d2e0b9ba216adcfbcc3881b31b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340557"
---
# <a name="sql-server-profiler-templates"></a>Modelli di SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

È possibile utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per creare modelli per la definizione delle classi di evento e delle colonne di dati da includere nelle tracce. Dopo aver definito e salvato il modello, è possibile eseguire una traccia per la registrazione dei dati relativi a ogni classe di evento selezionata. È possibile utilizzare un modello per più tracce. Il modello non viene eseguito direttamente.  

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] offre modelli di tracce predefiniti che consentono di configurare facilmente le classi di eventi che saranno probabilmente necessarie per tracce specifiche. Il modello Standard, ad esempio, consente di creare una traccia generica per la registrazione degli accessi, delle disconnessioni, dei batch completati e delle informazioni per la connessione. È possibile utilizzare questo modello per l'esecuzione di tracce senza modifiche oppure come punto di partenza per modelli aggiuntivi con configurazioni di evento diverse.  
  
> [!NOTE]  
>  Oltre che da modelli predefiniti, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] consente inoltre di creare le tracce da un modello vuoto, che per impostazione predefinita non include classi di eventi. L'utilizzo di un modello di traccia vuoto può essere utile quando una traccia pianificata non somiglia alle configurazioni di nessuno dei modelli predefiniti.  
  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] può tracciare vari tipi di server. È ad esempio possibile tracciare [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Le classi di evento che è possibile includere, tuttavia, non sono le stesse per ogni tipo di server. Pertanto, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] mantiene modelli diversi per server diversi e rende disponibile il modello specifico corrispondente al tipo server selezionato.  
  
## <a name="predefined-templates"></a>Modelli predefiniti  
 Oltre al modello Standard (predefinito), [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] include vari modelli predefiniti per il monitoraggio di determinati tipi di eventi. Nella tabella seguente vengono elencati i modelli predefiniti, il loro scopo e le classi di eventi per le quali acquisiscono informazioni.  
  
|Nome modello|Scopo del modello|Classi di eventi|  
|-------------------|----------------------|-------------------|  
|SP_Counts|Acquisisce il comportamento di esecuzione delle stored procedure nel tempo.|**SP:Starting**|  
|Standard|Punto di partenza generico per la creazione di una traccia. Acquisisce tutte le stored procedure e i batch di [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguiti. Utilizzabile per il monitoraggio dell'attività generale del server di database.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Completed**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL|Acquisisce tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] inviate a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dai client e l'ora dell'invio. Utilizzabile per il debug delle applicazioni client.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Duration|Acquisisce tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] inviate a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dai client e il relativo tempo di esecuzione (in millisecondi), quindi le raggruppa per durata. Utilizzabile per identificare query lente.|**RPC:Completed**<br /><br /> **SQL:BatchCompleted**|  
|TSQL_Grouped|Acquisisce tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] inviate a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'ora dell'invio. Raggruppa le informazioni per utente o client che ha inviato l'istruzione. Utilizzabile per analizzare le query dal punto di vista di un client o utente particolare.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Locks|Acquisisce tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] inviate a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dai client insieme a eventi del blocco insoliti. Utilizzabile per risolvere eventi di deadlock, di timeout di blocco e di escalation blocchi.|**Blocked Process Report**<br /><br /> **SP:StmtCompleted**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:StmtCompleted**<br /><br /> **SQL:StmtStarting**<br /><br /> **Deadlock Graph**<br /><br /> **Lock:Cancel**<br /><br /> **Lock:Deadlock**<br /><br /> **Lock:Deadlock Chain**<br /><br /> **Lock:Escalation**<br /><br /> **Lock:Timeout (timeout>0)**|  
|TSQL_Replay|Acquisisce informazioni dettagliate sulle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] necessarie per l'eventuale riproduzione della traccia. Utilizzabile per eseguire l'ottimizzazione iterativa, ad esempio per test di benchmark.|**CursorClose**<br /><br /> **CursorExecute**<br /><br /> **CursorOpen**<br /><br /> **CursorPrepare**<br /><br /> **CursorUnprepare**<br /><br /> **Audit Login**<br /><br /> **Audit Logout**<br /><br /> **Existing Connection**<br /><br /> **RPC Output Parameter**<br /><br /> **RPC:Completed**<br /><br /> **RPC:Starting**<br /><br /> **Exec Prepared SQL**<br /><br /> **Prepare SQL**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL_SPs|Acquisisce informazioni dettagliate relative a tutte le stored procedure in esecuzione. Utilizzabile per analizzare i passaggi dei componenti delle stored procedure. Aggiungere l'evento **SP:Recompile** se si ritiene che le stored procedure vengano ricompilate.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SP:Completed**<br /><br /> **SP:Starting**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:BatchStarting**|  
|Ottimizzazione|Acquisisce informazioni sulle stored procedure e l'esecuzione dei batch [!INCLUDE[tsql](../../includes/tsql-md.md)] . Consente di generare un output di traccia utilizzabile in Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] come carico di lavoro per l'ottimizzazione dei database.|**RPC:Completed**<br /><br /> **SP:StmtCompleted**<br /><br /> **SQL:BatchCompleted**|  
  
 Per informazioni sulle classi di evento, vedere [Guida di riferimento alle classi di evento di SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
## <a name="default-template"></a>Modello predefinito  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] specifica automaticamente il modello **Standard** come modello predefinito applicato a qualsiasi nuova traccia. Tuttavia è possibile modificare il modello predefinito in qualsiasi altro modello, predefinito o definito dall'utente. Per modificare il modello predefinito, selezionare la casella di controllo **Usa come modello predefinito per il tipo di server selezionato** quando si crea o modifica un modello utilizzando la scheda **Generale** della finestra di dialogo **Proprietà modello di traccia** .  
  
 Per spostarsi alla finestra di dialogo **Proprietà modello di traccia**, scegliere **Modelli** dal menu **File** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] e quindi fare clic su **Nuovo modello** o su **Modifica modello**.  
  
> [!NOTE]  
>  Il modello predefinito è specifico per un determinato tipo di server. La modifica del modello predefinito per un tipo di server non ha effetto sul modello predefinito per gli altri tipi. Per altre informazioni sull'impostazione di un modello predefinito per un server specifico, vedere [Impostare i valori predefiniti per una definizione di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Creare un modello di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [Modificare modello di traccia &#40;SQL Server Profiler&#41;](./modify-trace-templates.md)   
 [Esportare un modello di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [Esportare un modello di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
