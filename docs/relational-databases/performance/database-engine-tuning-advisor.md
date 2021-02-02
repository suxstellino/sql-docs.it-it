---
title: Ottimizzazione guidata motore di database | Microsoft Docs
description: Informazioni su come usare Ottimizzazione guidata motore di database per risolvere i problemi, ottimizzare un set di query di grandi dimensioni, analizzare le modifiche di progettazione e gestire lo spazio di archiviazione in SQL Server.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.general.f1
ms.assetid: 50dd0a0b-a407-4aeb-bc8b-b02a793aa30a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b7e9a6c6aed8af08b565bfc5af46448c81e54ed2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236651"
---
# <a name="database-engine-tuning-advisor"></a>Database Engine Tuning Advisor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Ottimizzazione guidata motore di database (DTA) di [!INCLUDE[msCoName](../../includes/msconame-md.md)] analizza i database e fornisce consigli da utilizzare per ottimizzare le prestazioni di query. È possibile utilizzare Ottimizzazione guidata motore di database per selezionare e creare un set ottimale di indici, viste indicizzate e partizioni di tabella senza che sia necessario conoscere in modo approfondito la struttura del database o le caratteristiche interne di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Con DTA, è possibile eseguire le attività seguenti.  
  
-   Risolvere i problemi relativi alle prestazioni di una query del problema specifico  
  
-   Ottimizzare un grande set di query in uno o più database  
  
-   Eseguire un'analisi di simulazione esplorativa delle possibili modifiche della progettazione fisica  
  
-   Gestire lo spazio di archiviazione  
  
## <a name="database-engine-tuning-advisor-benefits"></a>Vantaggi di Ottimizzazione guidata motore di database  
 L'ottimizzazione della prestazione delle query può essere difficile senza una conoscenza approfondita della struttura del database e delle query eseguite nel database. **Ottimizzazione guidata motore di database (DTA)** può rendere più facile questa attività grazie all'analisi della cache dei piano di query corrente o di un carico di lavoro delle query [!INCLUDE[tsql](../../includes/tsql-md.md)] creato e consigliando una progettazione fisica adatta. Per gli amministratori di database più avanzati, DTA espone un meccanismo potente per eseguire analisi di simulazione esplorativa di diverse alternative di progettazione fisica. DTA può fornire le informazioni seguenti.  
  
-   Consigliare la combinazione di indici rowstore e [columnstore](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md) ottimale per i database usando Query Optimizer per analizzare le query in un carico di lavoro.  
  
-   Consigliare partizioni allineate o non allineate per i database a cui si fa riferimento in un carico di lavoro.  
  
-   Consigliare viste indicizzate per i database a cui si fa riferimento in un carico di lavoro.  
  
-   Analizzare gli effetti delle modifiche proposte, tra cui l'utilizzo degli indici, la distribuzione delle query tra le tabelle e le prestazioni delle query nel carico di lavoro.  
  
-   Consigliare i metodi per ottimizzare il database per un set ridotto di query problematiche.  
  
-   Consentire all'utente di personalizzare le indicazioni mediante le opzioni avanzate, quali i vincoli di spazio su disco.  
  
-   Offrire report in cui sono riassunti gli effetti dell'implementazione delle indicazioni per un determinato carico di lavoro.  

-   Considerare le alternative in cui vengono indicate possibili scelte di progettazione sotto forma di configurazioni ipotetiche da sottoporre alla valutazione di Ottimizzazione guidata motore di database.

-   Ottimizzare i carichi di lavoro da un'ampia gamma di origini, tra cui archivio query di SQL Server, cache dei piani, file o tabella di traccia di SQL Server Profiler o un file .SQL.

  
Ottimizzazione guidata motore di database consente di gestire i seguenti tipi di carico di lavoro delle query:  
  
-   Solo query di elaborazione delle transazioni online (OLTP)  
  
-   Solo query di elaborazione analitica online (OLAP)  
  
-   Query miste OLTP e OLAP  
  
-   Carichi di lavoro elevati in termini di query (più query che modifiche di dati)  
  
-   Carichi di lavoro elevati in termini di aggiornamento (più modifiche di dati che query)  
  
## <a name="dta-components-and-concepts"></a>Componenti e concetti DTA  
 **Interfaccia utente grafica di Ottimizzazione guidata motore di database**  
 Un'interfaccia di facile utilizzo nella quale è possibile specificare il carico di lavoro e selezionare diverse opzioni di ottimizzazione.  
  
 Utilità **dta**  
 Versione del prompt dei comandi di Ottimizzazione guidata motore di database. L'utilità **dta** è stata sviluppata per consentire l'utilizzo della funzionalità Ottimizzazione guidata motore di database in applicazioni e script.  
  
 **Carico di lavoro**  
 File script Transact-SQL, file di traccia, o tabella di traccia che contiene un carico di lavoro rappresentativo per i database che si desidera ottimizzare. A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], è possibile specificare la cache dei piani come carico di lavoro.  A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile [specificare Query Store come carico di lavoro](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md). 
  
 **File di input XML**  
 File in formato XML che Ottimizzazione guidata motore di database può usare per ottimizzare i carichi di lavoro. Il file di input XML supporta le opzioni di ottimizzazione avanzate che non sono disponibili nella GUI o nell'utilità **dta** .  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Ottimizzazione guidata motore di database presenta le seguenti limitazioni e restrizioni.  
  
-   Non può aggiungere o rilasciare gli indici univoci o gli indici che impongono vincoli `PRIMARY KEY` o `UNIQUE`.  
  
-   Non può analizzare un database impostato su modalità utente singolo.  
  
-   Se si specifica uno spazio massimo su disco per le indicazioni di ottimizzazione e tale spazio supera lo spazio disponibile effettivo, verrà comunque utilizzato il valore specificato. Quando si esegue lo script delle indicazioni, è tuttavia possibile che non venga eseguito se prima non è stato aumentato lo spazio su disco. Per specificare lo spazio massimo su disco è possibile usare l'opzione **-B** dell'utilità **dta** oppure specificare un valore nella finestra di dialogo **Opzioni di ottimizzazione avanzate** .  
  
-   Per motivi di sicurezza, Ottimizzazione guidata motore di database non è in grado di ottimizzare un carico di lavoro in una tabella di traccia che si trova su un server remoto. Per risolvere temporaneamente questa limitazione, è possibile utilizzare un file di traccia anziché una tabella di traccia o copiare quest'ultima nel server remoto.  
  
-   Se si impongono vincoli, ad esempio si specifica uno spazio massimo su disco per le indicazioni di ottimizzazione (tramite l'opzione **-B** o la finestra di dialogo **Opzioni di ottimizzazione avanzate** ), l'Ottimizzazione guidata motore di database potrebbe essere obbligata a eliminare indici esistenti specifici. In questo caso, è possibile che l'indicazione risultante generi un miglioramento previsto negativo.  
  
-   Se si specifica un vincolo per limitare il tempo di ottimizzazione tramite l'opzione **-A** dell'utilità **dta** o selezionando l'opzione **Limita tempo di ottimizzazione** della scheda **Opzioni di ottimizzazione** , è possibile che l'Ottimizzazione guidata motore di database superi tale limite di tempo per creare un miglioramento previsto accurato e che i report di analisi relativi a una parte del carico di lavoro vengano esauriti.  
  
-   È possibile che Ottimizzazione guidata motore di database non crei indicazioni nei seguenti casi:  
  
    1.  La tabella da ottimizzare contiene meno di 10 pagine di dati.  
  
    2.  Gli indici inseriti nelle indicazioni non offrono un miglioramento delle prestazioni delle query adeguato per la progettazione fisica del database corrente.  
  
    3.  L'utente che esegue l'Ottimizzazione guidata motore di database non è un membro del ruolo del database **db_owner** o del ruolo predefinito del server **sysadmin** . Le query nel carico di lavoro vengono analizzate nel contesto di sicurezza dell'utente che esegue Ottimizzazione guidata motore di database. L'utente deve essere un membro del ruolo del database **db_owner** .  
  
-   L'Ottimizzazione guidata motore di database memorizza i dati delle sessioni di ottimizzazione e le altre informazioni nel database **msdb** . Se vengono apportate modifiche al database **msdb** esiste il rischio di perdere dati delle sessioni di ottimizzazione. Per eliminare tale rischio, implementare una strategia di backup appropriata per il database **msdb** .  
  
## <a name="performance-considerations"></a>Considerazioni sulle prestazioni  
 Ottimizzazione guidata motore di database può impegnare una notevole quantità di risorse dei processori e di memoria durante l'analisi. Per evitare rallentamenti del server di produzione, è possibile applicare una delle strategie seguenti:  
  
-   Ottimizzare i database quando il carico di lavoro del server è minimo. Ottimizzazione guidata motore di database può influire sulle prestazioni delle attività di manutenzione.  
  
-   Utilizzare la strategia che prevede l'utilizzo combinato di un server di prova e un server di produzione. Per altre informazioni, vedere  [Riduzione del carico di ottimizzazione del server di produzione](../../relational-databases/performance/reduce-the-production-server-tuning-load.md).  
  
-   Specificare unicamente le strutture di progettazione fisica del database che si desidera vengano analizzate da Ottimizzazione guidata motore di database. La procedura guidata offre numerose opzioni, ma specifica solo quelle necessarie.  
  
## <a name="dependency-on-xp_msver-extended-stored-procedure"></a>Dipendenza dalla stored procedure estesa xp_msver  
 Per offrire funzionalità complete, Ottimizzazione guidata motore di database dipende dalla stored procedure estesa **xp_msver** . Questa stored procedure estesa è attiva per impostazione. Questa stored procedure estesa viene utilizzata da Ottimizzazione guidata motore di database per recuperare il numero di processori e la memoria disponibile sul computer che contiene il database da ottimizzare. Se **xp_msver** non è disponibile, l'Ottimizzazione guidata motore di database prende in considerazione le caratteristiche hardware del computer in cui è in esecuzione . Se le caratteristiche hardware del computer in cui è in esecuzione Ottimizzazione guidata motore di database non sono disponibili, vengono considerati un processore e 1024 megabyte (MB) di memoria.  
  
 La relazione di dipendenza influisce sulle indicazioni relative al partizionamento, in quanto il numero di partizioni consigliate dipende da questi due valori (numero di processori e memoria disponibile). La dipendenza influisce inoltre sui risultati dell'ottimizzazione quando si utilizza un server di prova per ottimizzare il server di produzione. In questo scenario Ottimizzazione guidata motore di database usa **xp_msver** per recuperare le proprietà hardware del server di produzione. Dopo avere ottimizzato il carico di lavoro nel server di prova, Ottimizzazione guidata motore di database utilizza queste proprietà hardware per generare un'indicazione. Per altre informazioni, vedere [xp_msver &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md).  
  
## <a name="database-engine-tuning-advisor-tasks"></a>Attività di Ottimizzazione guidata motore di database  
 Nella tabella seguente vengono elencate attività di Ottimizzazione guidata motore di database comuni e argomenti che illustrano come eseguirle.  
  
|Attività di Ottimizzazione guidata motore di database|Argomento|  
|-----------------------------------------|-----------|  
|Inizializzare e avviare Ottimizzazione guidata motore di database.<br /><br /> Creare un carico di lavoro specificando la cache dei piani, creando uno script o generando un file di traccia o una tabella di traccia.<br /><br /> Ottimizzare un database tramite lo strumento dell'interfaccia utente grafica Ottimizzazione guidata motore di database.<br /><br /> Creare file input XML per l'ottimizzazione di carichi di lavoro.<br /><br /> Visualizzare le descrizioni delle opzioni dell'interfaccia utente di Ottimizzazione guidata motore di database.|[Avviare e usare Ottimizzazione guidata motore di database](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)|  
|Visualizzare i risultati dell'operazione di ottimizzazione del database.<br /><br /> Selezionare e implementare le indicazioni relative all'ottimizzazione.<br /><br /> Eseguire l'analisi di simulazione esplorativa nel carico di lavoro.<br /><br /> Rivedere le sessioni di ottimizzazione esistenti, clonare le sessioni in base a quelle esistenti <br />o modificare le indicazioni di ottimizzazione per un'ulteriore valutazione o implementazione.<br /><br /> Visualizzare le descrizioni delle opzioni dell'interfaccia utente di Ottimizzazione guidata motore di database.|[Visualizzare e usare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)|  
  
  
