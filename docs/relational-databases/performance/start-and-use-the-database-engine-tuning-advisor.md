---
title: Avviare e usare Ottimizzazione guidata motore di database | Microsoft Docs
description: Informazioni su come avviare e usare Ottimizzazione guidata motore di database in SQL Server per creare carichi di lavoro, ottimizzare i database e creare file di input XML.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.workload.f1
- sql13.dta.general.f1
- sql13.dta.advancedtuningoptions.f1
- sql13.dta.tuningoptions.f1
- sql13.dta.progress.f1
- sql13.dta.options.f1
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], starting
ms.assetid: a4e3226a-3917-4ec8-bdf0-472879d231c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5b83b5781d8c40c0acc6ee79ab0730c249def7c5
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172523"
---
# <a name="start-and-use-the-database-engine-tuning-advisor"></a>Avvio e utilizzo di Ottimizzazione guidata motore di database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come avviare e utilizzare Ottimizzazione guidata motore di database in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Per informazioni su come visualizzare e usare i risultati dopo l'ottimizzazione di un database, vedere [Visualizzare e utilizzare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
##  <a name="initialize-the-database-engine-tuning-advisor"></a><a name="Initialize"></a> Inizializzare Ottimizzazione guidata motore di database  
 Al primo utilizzo, è necessario che lo strumento Ottimizzazione guidata motore di database sia avviato da un utente membro del ruolo predefinito del server **sysadmin** . Questa operazione viene richiesta perché è necessario creare diverse tabelle di sistema nel database **msdb** per supportare operazioni di ottimizzazione. L'inizializzazione consente anche agli utenti membri del ruolo predefinito del database **db_owner** di ottimizzare carichi di lavoro nelle tabelle dei database di cui sono proprietari.  
  
 Un utente con le autorizzazioni di amministratore di sistema deve eseguire una delle azioni seguenti.  
  
-   Utilizzare l'interfaccia utente grafica di Ottimizzazione guidata motore di database per connettersi a un'istanza di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Per ulteriori informazioni, vedere [Avviare Ottimizzazione guidata motore di database](#Start) più avanti in questo argomento.  
  
-   Utilizzare l'utilità **dta** per ottimizzare il primo carico di lavoro. Per ulteriori informazioni, vedere [Utilizzare l'utilità dta](#dta) , più avanti in questo argomento.  
  
##  <a name="start-the-database-engine-tuning-advisor"></a><a name="Start"></a> Avviare Ottimizzazione guidata motore di database  
 Esistono diverse modalità per avviare l'interfaccia utente grafica (GUI) di Ottimizzazione guidata motore di database e supportare l'ottimizzazione dei database in vari scenari. Ottimizzazione guidata motore di database può essere avviato: dal menu **Start** , dal menu **Strumenti** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dall'editor di query in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e dal menu **Strumenti** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Quando Ottimizzazione guidata motore di database viene avviato per la prima volta, viene visualizzata la finestra di dialogo **Connetti al server** in cui è possibile specificare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si desidera connettersi.  
  
> [!WARNING]  
>  Non avviare Ottimizzazione guidata motore di database quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione in modalità utente singolo. Se l'applicazione viene avviata mentre il server è in modalità utente singolo, verrà restituito un errore e Ottimizzazione guidata motore di database non verrà avviata. Per altre informazioni sulla modalità utente singolo, vedere [Avvio di SQL Server in modalità utente singolo](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).  
  
#### <a name="to-start-database-engine-tuning-advisor-from-the-windows-start-menu"></a>Per avviare Ottimizzazione guidata motore di database dal menu Start di Windows  
  
1.  Nel menu **Start** scegliere **Tutti i programmi**, **Microsoft SQL Server 2005**, **Strumenti per le prestazioni** e fare clic su **Ottimizzazione guidata motore di database**.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-management-studio"></a>Per avviare Ottimizzazione guidata motore di database in SQL Server Management Studio  
  
1.  Scegliere **Ottimizzazione guidata motore di database** dal menu **Strumenti** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-start-the-database-engine-tuning-advisor-from-the-sql-server-management-studio-query-editor"></a>Per avviare Ottimizzazione guidata motore di database dall'Editor di query di SQL Server Management Studio  
  
1.  Aprire un file script [!INCLUDE[tsql](../../includes/tsql-md.md)] in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere [Editor di query e di testo &#40;SQL Server Management Studio&#41;](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
2.  Selezionare una query nello script [!INCLUDE[tsql](../../includes/tsql-md.md)] o selezionare l'intero script, fare clic con il pulsante destro del mouse sulla selezione e scegliere **Analizza query in Ottimizzazione guidata motore di database**. Viene aperta l'interfaccia utente grafica di Ottimizzazione guidata motore di database e lo script viene importato come file XML del carico di lavoro. Per ottimizzare le query [!INCLUDE[tsql](../../includes/tsql-md.md)] selezionate come carico di lavoro è possibile specificare un nome di sessione e le opzioni di ottimizzazione.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-profiler"></a>Per avviare Ottimizzazione guidata motore di database in SQL Server Profiler  
  
1.  Scegliere **Ottimizzazione guidata motore di database** dal menu **Strumenti** di SQL Server Profiler.  
  
##  <a name="create-a-workload"></a><a name="Create"></a> Creare un carico di lavoro  
 Un carico di lavoro è un set di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite sui database che si desidera ottimizzare. In Ottimizzazione guidata motore di database questi carichi di lavoro vengono analizzati allo scopo di fornire indicazioni sugli indici o sulle strategie di partizionamento che consentono di migliorare le prestazioni di esecuzione delle query nel server.  
  
 È possibile creare un carico di lavoro con uno dei metodi seguenti:  
  
-   Usare [Archivio query](../../relational-databases/performance/how-query-store-collects-data.md) come carico di lavoro. In questo modo, è possibile evitare di dover creare manualmente un carico di lavoro. Per altre informazioni, vedere [Ottimizzazione del database tramite un carico di lavoro dell'archivio query](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).  

      ||  
      |-|  
      |**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.|  

  
-   Utilizzare la cache dei piani come carico di lavoro. In questo modo, è possibile evitare di dover creare manualmente un carico di lavoro. Per ulteriori informazioni, vedere [Ottimizzare un database](#Tune) di seguito in questo argomento.  
  
-   Utilizzare l'editor di query in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o un editor di testo a scelta per creare manualmente carichi di lavoro di script [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   Utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per creare carichi di lavoro di tabelle di traccia o file di traccia  
  
    > [!NOTE]  
    >  Quando come carico di lavoro si utilizza una tabella di traccia, è necessario che la tabella sia inclusa nel server in cui viene eseguito Ottimizzazione guidata motore di database. Se la tabella di traccia viene creata in un altro server, è necessario spostarla nel server in cui viene eseguita l'ottimizzazione guidata.  
  
-   I carichi di lavoro inoltre possono essere incorporati in un file di input XML, in cui è possibile specificare anche la ponderazione per ogni evento. Per ulteriori informazioni sulla specifica di carichi di lavoro incorporati, vedere [Creare un file di input XML](#XMLInput) più avanti in questo argomento.  
  
###  <a name="to-create-transact-sql-script-workloads"></a><a name="SSMS"></a> Per creare carichi di lavoro di script Transact-SQL  
  
1.  Avviare l'editor di query in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere [Editor di query e di testo &#40;SQL Server Management Studio&#41;](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
2.  Digitare lo script [!INCLUDE[tsql](../../includes/tsql-md.md)] nell'editor di query. Lo script deve includere un set di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite sui database che si desidera ottimizzare.  
  
3.  Salvare il file con l'estensione **sql** . L'interfaccia grafica di Ottimizzazione guidata motore di database e l'utilità della riga di comando **dta** possono usare questo script [!INCLUDE[tsql](../../includes/tsql-md.md)] come carico di lavoro.  
  
###  <a name="to-create-trace-file-and-trace-table-workloads"></a><a name="Profiler"></a> Per creare carichi di lavoro di file di traccia o di tabelle di traccia  
  
1.  Avviare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] in uno dei modi seguenti:  
  
    -   Nel menu **Start** scegliere **Tutti i programmi**, **Microsoft SQL Server**, **Strumenti per le prestazioni** e quindi **SQL Server Profiler**.  
  
    -   In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]scegliere **SQL Server Profiler** dal menu **Strumenti**.  
  
2.  Creare un file o una tabella di traccia come descritto nelle procedure seguenti in cui viene usato il modello **Tuning** di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]:  
  
    -   [Creare una traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
  
    -   [Salvare i risultati della traccia in un file &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
         In Ottimizzazione guidata motore di database viene presunto che il file di traccia del carico di lavoro sia un file di rollover. Per ulteriori informazioni sui file di rollover, vedere [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
    -   [Salvare i risultati della traccia in una tabella &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)  
  
         Prima di utilizzare una tabella di traccia come carico di lavoro, verificare che la traccia sia stata arrestata.  
  
 Per l'acquisizione di carichi di lavoro per Ottimizzazione guidata motore di database è consigliabile utilizzare il modello Tuning di SQL Server Profiler.  
  
 Se si desidera utilizzare un modello personalizzato, verificare che siano acquisiti gli eventi di traccia seguenti:  
  
-   **RPC:Completed**  
  
-   **SQL:BatchCompleted**  
  
-   **SP:StmtCompleted**  
  
 È inoltre possibile utilizzare la versione **Starting** degli eventi di traccia, ad esempio, **SQL:BatchStarting**. La versione **Completed** tuttavia include la colonna **Duration** che consente un'ottimizzazione più efficiente del carico di lavoro. Ottimizzazione guidata motore di database non supporta l'ottimizzazione di altri tipi di eventi di traccia. Per ulteriori informazioni su questi eventi di traccia, vedere [Stored Procedures Event Category](../../relational-databases/event-classes/stored-procedures-event-category.md) e [TSQL Event Category](../../relational-databases/event-classes/tsql-event-category.md). Per informazioni sull'uso di stored procedure di Traccia SQL per la creazione di un carico di lavoro di file di traccia, vedere [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
### <a name="trace-file-or-trace-table-workloads-that-contain-the-loginname-data-column"></a>Carichi di lavoro di file o tabella di traccia che includono la colonna di dati LoginName  
 Ottimizzazione guidata motore di database inoltra le richieste Showplan come parte del processo di ottimizzazione. Quando una tabella o file di traccia che include la colonna di dati **LoginName** viene utilizzata come carico di lavoro, Ottimizzazione guidata motore di database rappresenta l'utente specificato in **LoginName**. Se tale utente non dispone dell'autorizzazione SHOWPLAN, che consente di eseguire e generare Showplan per le istruzioni incluse nella traccia, Ottimizzazione guidata motore di database non eseguirà l'ottimizzazione di queste istruzioni.  
  
##### <a name="to-avoid-granting-the-showplan-permission-to-each-user-specified-in-the-loginname-column-of-the-trace"></a>Per evitare di concedere l'autorizzazione SHOWPLAN a ogni utente specificato nella colonna LoginName della traccia  
  
1.  Ottimizzare il carico di lavoro del file o della tabella di traccia. Per ulteriori informazioni, vedere [Ottimizzare un database](#Tune) di seguito in questo argomento.  
  
2.  Verificare nel log di ottimizzazione eventuali istruzioni non ottimizzate a causa di autorizzazioni non adeguate. Per altre informazioni, vedere [Visualizzare e utilizzare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
3.  Creare un nuovo carico di lavoro eliminando la colonna **LoginName** dagli eventi non ottimizzati e quindi salvando solo gli eventi non ottimizzati in un nuovo file o tabella di traccia. Per altre informazioni sull'eliminazione di colonne di dati da una traccia, vedere [Specificare eventi e colonne di dati per un file di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md) o [Modificare una traccia esistente &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md).  
  
4.  Ripetere l'inoltro del nuovo carico di lavoro senza la colonna **LoginName** a Ottimizzazione guidata motore di database.  
  
 Ottimizzazione guidata motore di database eseguirà l'ottimizzazione del nuovo carico di lavoro dato che le informazioni di accesso non sono specificate nella traccia. Se **LoginName** non esiste per un'istruzione, Ottimizzazione guidata motore di database ottimizza l'istruzione rappresentando l'utente che ha avviato la sessione di ottimizzazione, ovvero un membro del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** .  
  
##  <a name="tune-a-database"></a><a name="Tune"></a> Ottimizzare un database  
 Per ottimizzare un database è possibile utilizzare la GUI o l'utilità da riga di comando **dta** di Ottimizzazione guidata motore di database.  
  
> [!NOTE]  
>  Prima di utilizzare una tabella di traccia come carico di lavoro per Ottimizzazione guidata motore di database, verificare che la traccia non sia più in esecuzione. Ottimizzazione guidata motore di database non consente di utilizzare come carico di lavoro una tabella di traccia nella quale è ancora in corso la scrittura di eventi di traccia.  
  
### <a name="use-the-database-engine-tuning-advisor-graphical-user-interface"></a>Utilizzare l'interfaccia utente grafica di Ottimizzazione guidata motore di database  
 Nell'interfaccia utente grafica (GUI) di Ottimizzazione guidata motore di database è possibile ottimizzare un database tramite la cache dei piani, i file o le tabelle del carico di lavoro. È possibile utilizzare GUI di Ottimizzazione guidata motore di database per visualizzare in modo semplice i risultati delle sessioni di ottimizzazione corrente e precedenti. Per ulteriori informazioni sulle opzioni dell'interfaccia utente, vedere [Descrizioni dell'interfaccia utente](#UI) più avanti in questo argomento. Per altre informazioni sull'utilizzo dell'output dopo l'ottimizzazione di un database, vedere [Visualizzare e utilizzare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  

####  <a name="to-tune-a-database-by-using-the-query-store"></a><a name="PlanCache"></a> Per ottimizzare un database tramite l'archivio query
Per altre informazioni, vedere [Ottimizzazione del database tramite un carico di lavoro dell'archivio query](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
  
####  <a name="to-tune-a-database-by-using-the-plan-cache"></a><a name="PlanCache"></a> Per ottimizzare un database tramite la cache dei piani  
  
1.  Avviare Ottimizzazione guidata motore di database e accedere a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [Avviare Ottimizzazione guidata motore di database](#Start) più indietro in questo argomento.  
  
2.  Nella scheda **Generale** digitare un nome in **Nome sessione** per creare una nuova sessione di ottimizzazione. Prima di avviare una sessione di ottimizzazione è necessario configurare i campi contenuti nella scheda **Generale** , Non è necessario modificare le impostazioni contenute nella scheda **Opzioni di ottimizzazione** prima di avviare una sessione di ottimizzazione.  
  
3.  Selezionare **Cache dei piani** come opzione del carico di lavoro. Ottimizzazione guidata motore di database seleziona i 1.000 eventi più importanti dalla cache dei piani da utilizzare per l'analisi.  
  
4.  Selezionare il o i database che si desidera ottimizzare e facoltativamente scegliere uno o più tabelle da ogni database da **Tabelle selezionate**. Per includere voci della cache per tutti i database, da **Opzioni di ottimizzazione** fare clic su **Opzioni avanzate** , quindi controllare **Includi eventi della cache dei piani da tutti i database**.  
  
5.  Selezionare **Salva log di ottimizzazione** per salvare una copia del log di ottimizzazione. Deselezionare la casella di controllo se non si desidera salvare una copia del log di ottimizzazione.  
  
     È possibile visualizzare il log di ottimizzazione in seguito all'analisi aprendo la sessione e selezionando la scheda **Stato** .  
  
6.  Selezionare la scheda **Opzioni di ottimizzazione** e le opzioni desiderate disponibili nella scheda.  
  
7.  Fare clic su **Avvia analisi**.  
  
     Per arrestare la sessione di ottimizzazione una volta avviata, scegliere una delle opzioni seguenti dal menu **Azioni** :  
  
    -   **Arresta analisi (con indicazioni)** per arrestare la sessione di ottimizzazione e scegliere se si vuole che l'Ottimizzazione guidata motore di database generi indicazioni basate sull'analisi eseguita fino a questo punto.  
  
    -   **Arresta analisi** per arrestare la sessione di ottimizzazione senza che vengano generate indicazioni.  
  
> [!NOTE]  
>  La sospensione di Ottimizzazione guidata motore di database non è supportata. Se si fa clic sul pulsante della barra degli strumenti **Avvia analisi** dopo aver fatto clic sui pulsanti della barra degli strumenti **Arresta analisi** o **Arresta analisi (con indicazioni)** , Ottimizzazione guidata motore di database avvia una nuova sessione di ottimizzazione.  
  
#### <a name="to-tune-a-database-using-a-workload-file-or-table-as-input"></a>Per ottimizzare un database utilizzando un file o una tabella del carico di lavoro come input  
  
1.  Stabilire quali funzionalità del database (indici, viste indicizzate, partizionamento) si desidera aggiungere, rimuovere o mantenere durante l'analisi eseguita con Ottimizzazione guidata motore di database.  
  
2.  Creare un carico di lavoro. Per ulteriori informazioni, vedere [Creare un carico di lavoro](#Create) più indietro in questo argomento.  
  
3.  Avviare l'Ottimizzazione guidata motore di database e accedere a un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [Avviare Ottimizzazione guidata motore di database](#Start) più indietro in questo argomento.  
  
4.  Nella scheda **Generale** digitare un nome in **Nome sessione** per creare una nuova sessione di ottimizzazione.  
  
5.  Selezionare **File del carico di lavoro** o **Tabella** e digitare il percorso del file oppure il nome della tabella nella casella di testo adiacente.  
  
     Il formato per specificare una tabella è  
  
    ```  
    database_name.schema_name.table_name  
    ```  
  
     Per cercare un file o una tabella del carico di lavoro, fare clic su **Sfoglia**. In Ottimizzazione guidata motore di database i file del carico di lavoro vengono considerati file di rollover. Per ulteriori informazioni sui file di rollover, vedere [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
     Quando come carico di lavoro viene utilizzata una tabella di traccia, è necessario che la tabella sia presente nel server in cui viene eseguito Ottimizzazione guidata motore di database. Se la tabella di traccia viene creata in un server diverso, spostarla nel server ottimizzato da Ottimizzazione guidata motore di database prima di utilizzarla come carico di lavoro.  
  
6.  Selezionare i database e le tabelle in cui si desidera eseguire il carico di lavoro selezionato nel passaggio 5. Per selezionare le tabelle, fare clic sulla freccia accanto a **Tabelle selezionate** .  
  
7.  Selezionare **Salva log di ottimizzazione** per salvare una copia del log di ottimizzazione. Deselezionare la casella di controllo se non si desidera salvare una copia del log di ottimizzazione.  
  
     È possibile visualizzare il log di ottimizzazione in seguito all'analisi aprendo la sessione e selezionando la scheda **Stato** .  
  
8.  Selezionare la scheda **Opzioni di ottimizzazione** e le opzioni desiderate disponibili nella scheda.  
  
9. Fare clic sul pulsante **Avvia analisi** sulla barra degli strumenti.  
  
     Per arrestare la sessione di ottimizzazione una volta avviata, scegliere una delle opzioni seguenti dal menu **Azioni** :  
  
    -   **Arresta analisi (con indicazioni)** per arrestare la sessione di ottimizzazione e scegliere se si vuole che l'Ottimizzazione guidata motore di database generi indicazioni basate sull'analisi eseguita fino a questo punto.  
  
    -   **Arresta analisi** per arrestare la sessione di ottimizzazione senza che vengano generate indicazioni.  
  
> [!NOTE]  
>  La sospensione di Ottimizzazione guidata motore di database non è supportata. Se si fa clic sul pulsante della barra degli strumenti **Avvia analisi** dopo aver fatto clic sui pulsanti della barra degli strumenti **Arresta analisi** o **Arresta analisi (con indicazioni)** , Ottimizzazione guidata motore di database avvia una nuova sessione di ottimizzazione.  
  
###  <a name="use-the-dta-utility"></a><a name="dta"></a> Utilizzare l'utilità dta  
 L'utilità [dta](../../tools/dta/dta-utility.md) fornisce un file eseguibile dal prompt dei comandi che consente di ottimizzare i database utilizzando le funzionalità di Ottimizzazione guidata motore di database in file batch e script. L'utilità **può essere utilizzato con l'interfaccia grafica di Ottimizzazione guidata motore di database e mediante l'utilità del prompt dei comandi** utilizza le voci della cache dei piani, i file di traccia, le tabelle di traccia e gli script [!INCLUDE[tsql](../../includes/tsql-md.md)] come carichi di lavoro. Utilizza inoltre l'input XML conforme a XML Schema di Ottimizzazione guidata motore di database, disponibile presso il [sito Web di Microsoft specificato](https://go.microsoft.com/fwlink/?linkid=43100).  
  
 Prima di ottimizzare un carico di lavoro tramite l'utilità **dta** , considerare quanto segue:  
  
-   Quando come carico di lavoro viene utilizzata una tabella di traccia, è necessario che la tabella sia presente nel server in cui viene eseguito Ottimizzazione guidata motore di database. Se la tabella di traccia viene creata in un altro server, è necessario spostarla nel server in cui viene eseguita Ottimizzazione guidata motore di database.  
  
-   Prima di utilizzare una tabella di traccia come carico di lavoro per Ottimizzazione guidata motore di database, verificare che la traccia non sia più in esecuzione. Ottimizzazione guidata motore di database non consente di utilizzare come carico di lavoro una tabella di traccia nella quale è ancora in corso la scrittura di eventi di traccia.  
  
-   Se una sessione di ottimizzazione dura più a lungo del previsto, è possibile premere CTRL+C per arrestarla e generare indicazioni in base alle analisi eseguite dall'utilità **dta** fino a quel momento. Verrà richiesto di decidere se generare le indicazioni o meno. Premere nuovamente CTRL+C per arrestare la sessione di ottimizzazione senza generare le indicazioni.  
  
 Per ulteriori informazioni sulla sintassi e per esempi dell'utilità **dta** , vedere [dta Utility](../../tools/dta/dta-utility.md).  
  
#### <a name="to-tune-a-database-by-using-the-plan-cache"></a>Per ottimizzare un database tramite la cache dei piani  
  
1.  Specificare l'opzione **-ip** . Vengono analizzati i primi 1.000 eventi della cache dei piani per i database selezionati.  
  
     Dal prompt dei comandi digitare quanto segue:  
  
    ```  
    dta -E -D DatabaseName -ip -s SessionName  
    ```  
  
2.  Per modificare il numero di eventi da usare per l'analisi, specificare l'opzione **-n**. Nell'esempio seguente viene aumentato a 2.000 il numero di voci della cache.  
  
    ```  
    dta -E -D DatabaseName -ip -n 2000-s SessionName1  
    ```  
  
3.  Per analizzare eventi per tutti i database nell'istanza, specificare l'opzione **-ipf** .  
  
    ```  
    dta -E -D DatabaseName -ip -ipf -n 2000 -s SessionName2  
    ```  
  
#### <a name="to-tune-a-database-by-using-a-workload-and-dta-utility-default-settings"></a>Per ottimizzare un database utilizzando un carico di lavoro e le impostazioni predefinite dell'utilità dta  
  
1.  Stabilire quali funzionalità del database (indici, viste indicizzate, partizionamento) si desidera aggiungere, rimuovere o mantenere durante l'analisi eseguita con Ottimizzazione guidata motore di database.  
  
2.  Creare un carico di lavoro. Per ulteriori informazioni, vedere [Creare un carico di lavoro](#Create) più indietro in questo argomento.  
  
3.  Dal prompt dei comandi digitare quanto segue:  
  
    ```  
    dta -E -D DatabaseName -if WorkloadFile -s SessionName  
    ```  
  
     dove `-E` specifica che la sessione di ottimizzazione utilizza una connessione trusted anziché un ID di accesso e una password e `-D` specifica il nome del database da ottimizzare. Per impostazione predefinita, l'utilità si connette all'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer locale. Per specificare un database remoto come illustrato nella procedura seguente o per specificare un'istanza denominata, è possibile utilizzare l'opzione `-S` . L'opzione `-if` consente di specificare il nome e il percorso del file del carico di lavoro (uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] o un file di traccia) e `-s` consente di specificare il nome della sessione di ottimizzazione.  
  
     Le quattro opzioni illustrate (nome del database, carico di lavoro, tipo di connessione e nome della sessione) sono obbligatorie.  
  
#### <a name="to-tune-a-remote-database-or-a-named-instance-for-a-specific-duration"></a>Per ottimizzare un database remoto o un'istanza denominata per un periodo di tempo specifico  
  
1.  Stabilire quali funzionalità del database (indici, viste indicizzate, partizionamento) si desidera aggiungere, rimuovere o mantenere durante l'analisi eseguita con Ottimizzazione guidata motore di database.  
  
2.  Creare un carico di lavoro. Per ulteriori informazioni, vedere [Creare un carico di lavoro](#Create) più indietro in questo argomento.  
  
3.  Dal prompt dei comandi digitare quanto segue:  
  
    ```  
    dta -S ServerName\Instance -D DatabaseName -it WorkloadTableName   
    -U LoginID -P Password -s SessionName -A TuningTimeInMinutes  
    ```  
  
     dove `-S` specifica il nome e l'istanza di un server remoto (o un'istanza denominata nel server locale) e `-D` specifica il nome del database da ottimizzare. L'opzione `-it` specifica il nome della tabella del carico di lavoro, `-U` e `-P` specificano l'ID e la password di accesso al database remoto, `-s` specifica il nome della sessione di ottimizzazione e `-A` specifica la durata della sessione di ottimizzazione espressa in minuti. La durata di ottimizzazione predefinita dell'utilità **dta** è 8 ore. Per impostare una durata illimitata per l'ottimizzazione di un carico di lavoro eseguita da Ottimizzazione guidata motore di database, specificare **0** (zero) tramite l'opzione `-A` .  
  
#### <a name="to-tune-a-database-using-an-xml-input-file"></a>Per ottimizzare un database tramite un file di input XML  
  
1.  Stabilire quali funzionalità del database (indici, viste indicizzate, partizionamento) si desidera aggiungere, rimuovere o mantenere durante l'analisi eseguita con Ottimizzazione guidata motore di database.  
  
2.  Creare un carico di lavoro. Per ulteriori informazioni, vedere [Creare un carico di lavoro](#Create) più indietro in questo argomento.  
  
3.  Creare un file di input XML. Per ulteriori informazioni, vedere [Creare un file di input XML](#XMLInput) di seguito in questo argomento.  
  
4.  Dal prompt dei comandi digitare quanto segue:  
  
    ```  
    dta -E -S ServerName\Instance -s SessionName -ix PathToXMLInputFile  
    ```  
  
     dove `-E` specifica una connessione trusted, `-S` specifica un server e un'istanza remoti oppure un'istanza denominata nel server locale, `-s` specifica il nome di una sessione di ottimizzazione e `-ix` specifica il file di input XML da utilizzare per la sessione di ottimizzazione.  
  
5.  Dopo che l'utilità ha completato l'ottimizzazione del carico di lavoro, è possibile visualizzare i risultati delle sessioni tramite la GUI di Ottimizzazione guidata motore di database. In alternativa, è possibile specificare tramite l'opzione **-ox** che le indicazioni relative all'ottimizzazione verranno scritte in un file XML. Per altre informazioni, vedere [dta Utility](../../tools/dta/dta-utility.md).  
  
##  <a name="create-an-xml-input-file"></a><a name="XMLInput"></a> Creare un file di input XML  
 Gli sviluppatori XML esperti possono creare file XML utilizzabili per l'ottimizzazione dei carichi di lavoro tramite Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Per creare questi file XML, utilizzare gli strumenti XML desiderati per modificare un file di esempio o per generare un'istanza in base all'XML Schema di Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Lo schema XML dell'Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] è disponibile nella directory di installazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel percorso seguente:  
  
 `C:\Program Files\Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd` 
  
 L'XML Schema di Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] è inoltre disponibile online in questo [sito Web Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
 in cui sono disponibili molti XML Schema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Scorrere la pagina verso il basso fino a visualizzare la riga corrispondente a Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
#### <a name="to-create-an-xml-input-file-to-tune-workloads"></a>Per creare un file input XML per l'ottimizzazione di carichi di lavoro  
  
1.  Creare un carico di lavoro. A tale scopo è possibile utilizzare un file o una tabella di traccia tramite il modello di ottimizzazione di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]oppure creare uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] che riproduca un carico di lavoro rappresentativo per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [Creare un carico di lavoro](#Create) più indietro in questo argomento.  
  
2.  Creare un file input XML seguendo una delle procedure seguenti:  
  
    -   Copiare e incollare nell'editor XML preferito uno degli [Esempi di file di input XML &#40;DTA&#41;](../../tools/dta/xml-input-file-samples-dta.md). Modificare i valori degli argomenti in modo appropriato per l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in uso e quindi salvare il file XML.  
  
    -   Utilizzando lo strumento XML desiderato, generare un'istanza in base all'XML Schema di Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
3.  Usare il file input XML creato come input dell'utilità da riga di comando **dta** per ottimizzare il carico di lavoro. Per informazioni sull'utilizzo di file di input di XML con questa utilità, vedere la sezione [Utilizzare l'utilità dta](#dta) più indietro in questo argomento.  
  
> [!NOTE]  
>  Se si vuole usare un carico di lavoro inline, ovvero un carico di lavoro specificato direttamente nel file input XML, usare il file di esempio disponibile in [Esempio di file di input XML con carico di lavoro inline &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-inline-workload-dta.md).  
  
##  <a name="user-interface-descriptions"></a><a name="UI"></a> Descrizioni dell'interfaccia utente  
  
### <a name="tools-menuoptions-page"></a>Menu Strumenti/Pagina Opzioni  
 Utilizzare questa finestra di dialogo per specificare i parametri di configurazione generale di Ottimizzazione guidata motore di database.  
  
 **All'avvio**  
 Consente di specificare l'operazione che deve essere eseguita da Ottimizzazione guidata motore di database all'avvio, ovvero avvio senza connessione al database, visualizzazione di una finestra di dialogo **Nuova connessione** , visualizzazione di una nuova sessione oppure caricamento dell'ultima sessione caricata.  
  
 **Modifica carattere**  
 Consente di specificare il tipo di carattere utilizzato nelle tabelle di Ottimizzazione guidata motore di database.  
  
 **Numero degli elementi negli elenchi degli ultimi elementi utilizzati**  
 Consente di specificare il numero di sessioni o file da visualizzare sotto la voce **Sessioni recenti** o **File recenti** del menu **File** .  
  
 **Memorizza le ultime opzioni di ottimizzazione specificate**  
 Consente di mantenere le impostazioni delle opzioni di ottimizzazione tra più sessioni. L'opzione è selezionata per impostazione predefinita. Deselezionare questa casella di controllo per avviare ogni sessione con i valori predefiniti di Ottimizzazione guidata motore di database.  
  
 **Chiedi conferma prima di eliminare definitivamente le sessioni**  
 Consente di visualizzare una finestra di dialogo di conferma prima dell'eliminazione delle sessioni.  
  
 **Richiedi conferma prima di arrestare l'analisi della sessione**  
 Consente di visualizzare una finestra di dialogo di conferma prima di arrestare l'analisi di un carico di lavoro.  
  
#### <a name="general-tab-options"></a>Opzioni della scheda Generale  
 Prima di avviare una sessione di ottimizzazione è necessario configurare i campi contenuti nella scheda **Generale** , Non è necessario modificare le impostazioni contenute nella scheda **Opzioni di ottimizzazione** prima di avviare una sessione di ottimizzazione.  
  
 **Nome sessione**  
 Consente di specificare un nome per la sessione. Questa opzione consente di associare un nome a una sessione di ottimizzazione. In seguito sarà possibile fare riferimento a questo nome per controllare la sessione di ottimizzazione.  
  
 **File**  
 Consente di specificare uno script sql o un file di traccia per un carico di lavoro. Digitare il percorso e il nome del file nell'apposita casella di testo. In Ottimizzazione guidata motore di database viene presunto che il file di traccia del carico di lavoro sia un file di rollover. Per ulteriori informazioni sui file di rollover, vedere [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
 **Tabella**  
 Consente di specificare una tabella di traccia per un carico di lavoro. Specificare il nome completo della tabella di traccia nell'apposita casella, come illustrato nell'esempio seguente:  
  
```  
database_name.owner_name.table_name  
```  
  
-   Prima di utilizzare una tabella di traccia come carico di lavoro, verificare che la traccia sia stata arrestata.  
  
-   La tabella di traccia deve trovarsi sullo stesso server per il quale è in esecuzione Ottimizzazione guidata motore di database. Se la tabella di traccia viene creata in un altro server, è necessario spostarla nel server in cui viene eseguita Ottimizzazione guidata motore di database.  
  
 **Cache dei piani**  
 Specificare la cache dei piani come carico di lavoro. In questo modo, è possibile evitare di dover creare manualmente un carico di lavoro. Ottimizzazione guidata motore di database seleziona i 1.000 eventi più importanti da utilizzare per l'analisi.  
  
 **Xml**  
 Se non si importa una query del carico di lavoro da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], questa opzione non viene visualizzata.  
  
 Per importare una query del carico di lavoro da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
1.  Digitare una query nell'editor di query ed evidenziarla.  
  
2.  Fare clic con il pulsante destro del mouse sulla query evidenziata e scegliere **Analizza query in Ottimizzazione guidata motore di database**.  
  
 **Consente di cercare un file di carico di lavoro/Consente di cercare una tabella di carico di lavoro**  
 Quando è selezionato **File** o **Tabella** come origine del carico di lavoro, usare questo pulsante per selezionare la destinazione.  
  
 **Anteprima del carico di lavoro XML**  
 Consente di visualizzare un carico di lavoro in formato XML importato da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Database per l'analisi del carico di lavoro**  
 Consente di specificare il primo database al quale Ottimizzazione guidata motore di database si connette durante l'ottimizzazione di un carico di lavoro. Dopo l'inizio dell'ottimizzazione, Ottimizzazione guidata motore di database si connette ai database specificati dalle istruzioni `USE DATABASE` contenute nel carico di lavoro.  
  
 **Selezionare i database e le tabelle da ottimizzare**  
 Consente di specificare i database e le tabelle da ottimizzare. Per specificare tutti i database, selezionare la casella di controllo nell'intestazione della colonna **Nome** . Per specificare solo determinati database, selezionare le caselle di controllo accanto ai nomi dei database desiderati. Per impostazione predefinita, tutte le tabelle per i database selezionati vengono incluse automaticamente nella sessione di ottimizzazione. Per escludere determinate tabelle, fare clic sulla freccia nella colonna **Tabelle selezionate** e quindi deselezionare le caselle di controllo accanto alle tabelle che non si desidera ottimizzare.  
  
 Freccia in giù **Tabelle selezionate**  
 Consente di espandere l'elenco delle tabelle in cui è possibile selezionare singole tabelle per l'ottimizzazione.  
  
 **Salva log di ottimizzazione**  
 Consente di creare un log e di registrare gli errori verificatisi durante la sessione.  
  
> [!NOTE]  
>  Le informazioni delle righe relative alle tabelle visualizzate nella scheda **Generale** non vengono aggiornate automaticamente in Ottimizzazione guidata motore di database. A tale scopo vengono utilizzati invece i metadati del database. Se si ritiene che le informazioni delle righe siano obsolete, eseguire il comando DBCC UPDATEUSAGE per i relativi oggetti.  
  
##### <a name="tuning-tab-options"></a>Scheda Opzioni di ottimizzazione  
 La scheda **Opzioni di ottimizzazione** consente di modificare le impostazioni predefinite delle opzioni di ottimizzazione generali. Non è necessario modificare le impostazioni contenute nella scheda **Opzioni di ottimizzazione** prima di avviare una sessione di ottimizzazione.  
  
 **Limita tempo di ottimizzazione**  
 Consente di limitare il tempo per la sessione di ottimizzazione corrente. L'impostazione di un tempo di ottimizzazione maggiore migliora la qualità delle indicazioni. Per garantire la migliore qualità delle indicazioni, non selezionare questa opzione.  
  
> [!NOTE]  
> [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizza risorse di sistema. Utilizzare l'opzione **Limita tempo di ottimizzazione** per arrestare l'ottimizzazione prima che venga eseguita l'ottimizzazione di periodi di notevoli carichi di lavoro previsti nel server.  
  
 **Opzioni avanzate**  
 Usare la finestra di dialogo **Opzioni di ottimizzazione avanzate** per configurare le indicazioni relative allo spazio massimo, alle colonne chiave massime e all'indice online.  
  
 **Spazio massimo per le indicazioni (MB)**  
 Digitare la quantità massima di spazio che deve essere utilizzato dalle strutture di progettazione fisica consigliate da Ottimizzazione guidata motore di database.  
  
 Se non viene immesso alcun valore, Ottimizzazione guidata motore di database utilizzerà il valore più piccolo tra i limiti di spazio seguenti:  
  
-   Valore triplo della dimensione corrente dei dati non elaborati, incluse le dimensioni totali degli heap e degli indici cluster nelle tabelle del database.  
  
-   Lo spazio disponibile su tutte le unità disco collegate sommato alle dimensioni dei dati non elaborati.  
  
 **Includi eventi della cache dei piani da tutti i database**  
 Specifica che vengono analizzati gli eventi della cache dei piani di tutti i database.  
  
 **Numero massimo di colonne per indice**  
 Consente di specificare il numero massimo di colonne da includere in ogni indice. Il valore predefinito è 1023.  
  
 **Tutte le indicazioni offline**  
 Consente di generare le migliori indicazioni possibili senza indicare la creazione online di alcuna struttura di progettazione fisica.  
  
 **Genera indicazioni online quando possibile**  
 Durante la creazione di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per l'implementazione delle indicazioni, questa opzione consente di scegliere metodi che possono essere implementati quando il server è online, anche se è disponibile un metodo offline più veloce.  
  
 **Genera solo indicazioni online**  
 Utilizzare questa opzione per generare solo indicazioni che consentono al server di rimanere online.  
  
 **Data e ora arresto**  
 Consente di specificare la data e l'ora di arresto dell'Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 **Indici e viste indicizzate**  
 Selezionare questa casella per includere indicazioni per l'aggiunta di indici cluster, indici non cluster e viste indicizzate.  
  
 **Viste indicizzate**  
 Vengono incluse solo le indicazioni per l'aggiunta di viste indicizzate. Non verranno generate indicazioni per gli indici cluster e non cluster.  
  
 **Includi indici filtrati**  
 Vengono incluse le indicazioni per l'aggiunta di indici filtrati. Questa opzione è disponibile se si seleziona una di queste strutture di progettazione fisica: **Indici e viste indicizzate**, **Indici** o  **Indici non cluster**.  
  
 **Indici**  
 Vengono incluse solo le indicazioni per l'aggiunta di indici cluster e non cluster. Non verranno generate indicazioni per le viste indicizzate.  
  
 **Indici non cluster**  
 Vengono incluse solo le indicazioni per gli indici non cluster. Non verranno generate indicazioni per gli indici cluster e le viste indicizzate.  
  
 **Valuta l'utilizzo delle sole strutture di progettazione fisica esistenti**  
 Consente di valutare l'efficacia degli indici correnti senza che vengano generate indicazioni per le viste indicizzate o gli indici aggiuntivi.  
  
 **Nessun partizionamento.**  
 Non vengono generate indicazioni per il partizionamento.  
  
 **Partizionamento completo**  
 Vengono incluse le indicazioni per il partizionamento.  
  
 **Partizionamento allineato**  
 Le nuove partizioni consigliate verranno allineate in modo da rendere più semplice la manutenzione.  
  
 **Non mantenere alcuna struttura di progettazione fisica esistente**  
 Viene consigliata l'eliminazione degli indici, delle viste e delle partizioni esistenti non necessari. Se il carico di lavoro utilizza una determinata struttura di progettazione fisica esistente, nell'Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne viene sconsigliata l'eliminazione.  
  
 **Mantieni solo gli indici**  
 Tutti gli indici esistenti vengono mantenuti, ma viene consigliata l'eliminazione delle viste indicizzate e delle partizioni non necessarie.  
  
 **Mantieni tutte le strutture di progettazione fisica esistenti**  
 Vengono mantenuti tutti gli indici, le viste indicizzate e le partizioni esistenti.  
  
 **Mantieni solo gli indici cluster**  
 Tutti gli indici esistenti vengono mantenuti, ma viene consigliata l'eliminazione delle viste indicizzate, delle partizioni e degli indici non cluster non necessari.  
  
 **Mantieni partizionamento allineato**  
 Vengono mantenute le strutture di partizionamento attualmente allineate, ma viene consigliata l'eliminazione delle viste indicizzate, degli indici e delle partizioni non allineate non necessari. Qualsiasi partizionamento aggiuntivo consigliato verrà allineato allo schema di partizione corrente.  
  
###### <a name="progress-tab-options"></a>Opzioni della scheda Stato  
 La scheda **Stato** dell'Ottimizzazione guidata motore di database viene visualizzata dopo che l'Ottimizzazione guidata motore di database ha iniziato l'analisi di un carico di lavoro.  
  
 Per arrestare la sessione di ottimizzazione una volta avviata, scegliere una delle opzioni seguenti dal menu **Azioni** :  
  
-   **Arresta analisi (con indicazioni)** per arrestare la sessione di ottimizzazione e scegliere se si vuole che l'Ottimizzazione guidata motore di database generi indicazioni basate sull'analisi eseguita fino a questo punto.  
  
-   **Arresta analisi** per arrestare la sessione di ottimizzazione senza che vengano generate indicazioni.  
  
 **Stato ottimizzazione**  
 Indica lo stato di ottimizzazione corrente. Sono inclusi il numero di azioni eseguite e il numero di messaggi di errore, di esito positivo e di avviso ricevuti.  
  
 **Dettagli**  
 Include un'icona che indica lo stato.  
  
 **Azione**  
 Visualizza i passaggi in esecuzione.  
  
 **Status**  
 Visualizza lo stato del passaggio dell'azione.  
  
 **Messaggio**  
 Include gli eventuali messaggi restituiti dai passaggi dell'azione.  
  
 **Log di ottimizzazione**  
 Include informazioni sulla sessione di ottimizzazione corrente. Per stampare il log fare clic con il pulsante destro del mouse sul log e quindi scegliere **Stampa**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e utilizzare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [Utilità dta](../../tools/dta/dta-utility.md)    
 [Esercitazione: Ottimizzazione guidata motore di database](../../tools/dta/tutorial-database-engine-tuning-advisor.md)
