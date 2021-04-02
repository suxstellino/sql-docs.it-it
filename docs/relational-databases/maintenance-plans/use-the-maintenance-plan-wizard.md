---
title: Usare la Creazione guidata piano di manutenzione | Microsoft Docs
description: Informazioni su come creare un piano di manutenzione con uno o più server usando la Creazione guidata piano di manutenzione in SQL Server.
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.ag.maintwiz.integrity.f1
- sql13.ag.maintwiz.order.f1
- sql13.ag.maintwiz.report.f1
- sql13.ag.maintwiz.updatestats.f1
- sql13.ag.maintwiz.indexdefrag.f1
- sql13.ag.maintwiz.progress.f1
- sql13.ag.maintwiz.maintcleanup.f1
- sql13.ag.maintwiz.backupfull.f1
- sql13.ag.maintwiz.task.f1
- sql13.ag.maintwiz.server.f1
- sql13.ag.maintwiz.shrinkdb.f1
- sql13.ag.maintwiz.execagentjob.f1
- sql13.ag.maintwiz.summary.f1
- sql13.ag.maintwiz.welcome.f1
- sql13.ag.maintwiz.planprop.f1
- sql13.ag.maintwiz.reindex.f1
- sql13.ag.maintwiz.histcleanup.f1
- sql13.ag.maintwiz.backuplog.f1
- sql13.ag.maintwiz.backupdiff.f1
helpviewer_keywords:
- Maintenance Plan Wizard
- Database Maintenance Plan Wizard
- Database Maintenance Plan Wizard, starting
ms.assetid: db65c726-9892-480c-873b-3af29afcee44
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 17c548cfc0133a9f08987e06dc4ffd2be4a930a9
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054656"
---
# <a name="use-the-maintenance-plan-wizard"></a>Utilizzare la Creazione guidata piano di manutenzione
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questo argomento descrive come creare un piano di manutenzione con uno o più server usando la Creazione guidata piano di manutenzione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Tramite la Creazione guidata piano di manutenzione è possibile creare un piano di manutenzione che potrà essere regolarmente eseguito in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. In questo modo è possibile eseguire a intervalli specificati varie attività di amministrazione di database, tra cui backup, controlli di integrità del database o aggiornamenti delle statistiche del database.  
    
 
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Per creare un piano di manutenzione multiserver, è necessario configurare un ambiente multiserver con un server master e uno o più server di destinazione. È necessario creare e gestire piani di manutenzione nel server master. È possibile visualizzare i piani nei server di destinazione.   

-   I membri dei ruoli **db_ssisadmin** e **dc_admin** possono essere in grado di elevare i privilegi a **sysadmin**. Questa elevazione dei privilegi può verificarsi perché tali ruoli possono modificare i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . I pacchetti possono essere eseguiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il contesto di sicurezza **sysadmin** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. 

Per impedire questa elevazione dei privilegi durante l'esecuzione di piani di manutenzione, set di raccolta dati e altri pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurare i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che eseguono pacchetti in modo che usino un account proxy con privilegi limitati o aggiungere solo i membri **sysadmin** ai ruoli **db_ssisadmin** e **dc_admin** .  

##  <a name="prerequisites"></a><a name="Prerequisite"></a> Prerequisiti 
È necessario abilitare [Opzione di configurazione del server Agent XPs](../../database-engine/configure-windows/agent-xps-server-configuration-option.md).
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per creare o gestire piani di manutenzione, è necessario essere membro del ruolo predefinito del server **sysadmin** . In Esplora oggetti il nodo **Piani di manutenzione** viene visualizzato solo per gli utenti membri del ruolo predefinito del server **sysadmin** .  
  
##  <a name="use-the-maintenance-plan-wizard"></a><a name="SSMSProcedure"></a> Utilizzare la Creazione guidata piano di manutenzione  
  
**Avviare la procedura guidata** 

  
1.  Espandere il server in cui si desidera creare il piano di manutenzione.  
  
2.  Espandere la cartella **Gestione** .  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Piani di manutenzione** e scegliere **Creazione guidata piano di manutenzione**.  
  
4.  Nella pagina **Creazione guidata piano di manutenzione di SQL Server** fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione proprietà piano** :  
  
    1.  Nella casella **Nome** immettere il nome del piano di manutenzione che si desidera creare.  
  
    2.  Nella casella **Descrizione** descrivere brevemente il piano di manutenzione.  
  
    3.  Nell'elenco **Esegui come** specificare le credenziali utilizzate da Microsoft SQL Server Agent per l'esecuzione del piano di manutenzione.  
  
    4.  Selezionare **Pianificazioni separate per ogni attività** o **Singola pianificazione per l'intero piano o nessuna pianificazione** per specificare la pianificazione periodica del piano di manutenzione.  
  
        > **NOTA** Se si seleziona **Pianificazioni separate per ogni attività**, è necessario eseguire i passaggi indicati in **e** . di seguito per ogni attività del piano di manutenzione.  
  
    5.  Se si seleziona **Singola pianificazione per l'intero piano o nessuna pianificazione**, scegliere **Cambia** in **Pianificazione**.  
  
        1.  Nella casella **Nome** della finestra di dialogo **Nuova pianificazione processo** immettere il nome della pianificazione del processo.  
  
        2.  Nell'elenco **Tipo pianificazione** selezionare il tipo di pianificazione:  
  
            -   **Avvia automaticamente all'avvio di SQL Server Agent**  
  
            -   **Avvia quando la CPU risulta inattiva**  
  
            -   **Periodica**. Si tratta della selezione predefinita.  
  
            -   **Una volta**  
  
        3.  Selezionare o deselezionare la casella di controllo **Abilitata** per abilitare o disabilitare la pianificazione.  
  
        4.  Se si seleziona **Periodica**:  
  
            1.  In **Frequenza** nell'elenco **Ricorrenza** specificare la frequenza di occorrenza:  
  
                -   Se si seleziona **Giornaliera**, nella casella **Ogni** immettere la frequenza in base alla quale si ripete la pianificazione del processo nei giorni.  
  
                -   Se si seleziona **Settimanale**, nella casella **Ogni** immettere la frequenza in base alla quale si ripete la pianificazione del processo nelle settimane. Selezionare i giorni della settimana durante i quali viene eseguita la pianificazione del processo.  
  
                -   Se si seleziona **Mensile**, selezionare **Giorno** oppure **Ogni**.  
  
                    -   Se si seleziona **Giorno**, immettere sia la data del mese in cui si desidera sia eseguita la pianificazione del processo sia la frequenza in base alla quale si ripete questa pianificazione nei mesi. Ad esempio, se si vuole che la pianificazione del processo sia eseguita il giorno 15 del mese a mesi alterni, selezionare **Giorno** e immettere "15" nella prima casella e "2" nella seconda casella. Si noti che il numero più grande consentito nella seconda casella è "99".  
  
                    -   Se si sceglie **Ogni**, selezionare il giorno specifico della settimana del mese in cui si desidera sia eseguita la pianificazione del processo e la frequenza in base alla quale si ripete questa pianificazione nei mesi. Ad esempio, se si vuole che la pianificazione del processo sia eseguita l'ultimo giorno feriale del mese a mesi alterni, selezionare **Giorno**, selezionare **ultimo** nel primo elenco e **giorno feriale** nel secondo elenco, quindi immettere "2" nell'ultima casella. Nei primi due elenchi è anche possibile selezionare **primo**, **secondo**, **terzo** o **quarto**, nonché i giorni della settimana specifici, ad esempio: domenica o mercoledì. Si noti che il numero più grande consentito nell'ultima casella è "99".  
  
            2.  In **Frequenza giornaliera** specificare la frequenza in base alla quale si ripete la pianificazione del processo in quel determinato giorno:  
  
                -   Se si seleziona **Una sola volta alle**, immettere l'ora specifica del giorno in cui deve essere eseguita la pianificazione del processo nella casella **Una sola volta alle** . Immettere l'ora, il minuto e il secondo del giorno, nonché AM o PM.  
  
                -   Se si seleziona **Ogni** specificare la frequenza in base alla quale la pianificazione del processo viene eseguita durante il giorno scelto in **Frequenza**. Ad esempio, se si vuole che la pianificazione del processo sia ripetuta ogni 2 ore durante il giorno scelto per questa pianificazione, selezionare **Ogni**, immettere "2" nella prima casella e quindi selezionare **ora/e** nell'elenco. In questo elenco è anche possibile selezionare **minuto/i** e **secondo/i**. Si noti che il numero più grande consentito nella prima casella è "100".  
  
                     Nella casella **A partire dalle** immettere l'ora in cui dovrebbe iniziare l'esecuzione della pianificazione del processo. Nella casella **Fino alle** immettere l'ora in cui dovrebbe terminare la ripetizione della pianificazione del processo. Immettere l'ora, il minuto e il secondo del giorno, nonché AM o PM.  
  
            3.  In **Durata** di **Data inizio** immettere la data in cui si desidera sia avviata l'esecuzione della pianificazione del processo. Selezionare **Data fine** o **Nessuna data di fine** per indicare quando dovrebbe terminare l'esecuzione della pianificazione del processo. Se si seleziona **Data fine** immettere la data in cui si desidera venga terminata l'esecuzione della pianificazione del processo.  
  
        5.  Se si seleziona **Singola occorrenza**, in **Singola occorrenza**, nella casella **Data** immettere la data in cui verrà eseguita la pianificazione del processo. Nella casella **Ora** immettere l'ora in cui verrà eseguita la pianificazione del processo. Immettere l'ora, il minuto e il secondo del giorno, nonché AM o PM.  
  
        6.  In **Descrizione** in **Riepilogo** verificare che tutte le impostazioni della pianificazione del processo siano corrette.  
  
        7.  Fare clic su **OK**.  
  
    6.  Fare clic su **Avanti**.  
  
6.  Nella pagina **Selezione server di destinazione** selezionare i server in cui si desidera eseguire il piano di manutenzione. Questa pagina viene visualizzata solo nelle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configurate come server master.  
  
    > **NOTA** Per creare un piano di manutenzione multiserver, è necessario configurare un ambiente multiserver che include un server master e uno o più server di destinazione. Il server locale deve essere inoltre configurato come server master. Negli ambienti multiserver, in questa pagina vengono visualizzati il server master **(local)** e tutti i server di destinazione corrispondenti.  
  
7.  Nella pagina **Selezione attività di manutenzione** selezionare una o più attività di manutenzione da aggiungere al piano. Dopo avere selezionato tutte le attività necessarie, scegliere **Avanti**.  
  
    > **NOTA** Le attività selezionate determinano quali pagine è necessario completare dopo la pagina **Selezione ordine attività di manutenzione**.  
  
8.  Nella pagina **Selezione ordine attività di manutenzione** selezionare un'attività e fare clic su **Sposta su...** o **Sposta giù...** per modificare il relativo ordine di esecuzione. Al termine, o dopo avere raggiunto l'ordine di attività desiderato, scegliere **Avanti**.  
  
    > **NOTA** Se si seleziona **Pianificazioni separate per ogni attività** nella pagina **Selezione proprietà piano**, non sarà possibile modificare l'ordine delle attività di manutenzione in questa pagina.  
  
## <a name="define-database-check-integrity-checkdb"></a>Definizione attività Controllo integrità database (CHECKDB)  
  
 Nella pagina **Definizione attività Controlla integrità database** scegliere il database o i database in cui verranno controllate l'allocazione e l'integrità strutturale degli indici e delle tabelle utente e di sistema. Con l'esecuzione dell'istruzione `DBCC CHECKDB`[!INCLUDE[tsql](../../includes/tsql-md.md)] , questa attività consente di segnalare i problemi di integrità del database in modo che possano essere gestiti successivamente da un amministratore di sistema o dal proprietario del database. Per altre informazioni, vedere [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)Al termine, fare clic su **Avanti**.  
  
In questa pagina sono disponibili le opzioni seguenti.  
  
 Elenco **Database**  
 Consente di specificare i database su cui verrà eseguita l'attività.  
  
 -  **Tutti i database**  
  
Viene generato un piano di manutenzione per l'esecuzione di questa attività in tutti i database di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a eccezione di **tempdb**.  
  
**Database di sistema**  
  
  - Viene generato un piano di manutenzione per l'esecuzione di questa attività sui database di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , eccetto **tempdb** e i database creati dall'utente.  
  
 **Tutti i database utente (diversi da master, model, msdb e tempdb)**  
  
 - Viene generato un piano di manutenzione per l'esecuzione di questa attività su tutti i database creati dall'utente. Nessuna attività di manutenzione viene eseguita sui database di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **I database seguenti**  
  
  - Viene generato un piano di manutenzione per l'esecuzione di questa attività solo sui database selezionati. Se si sceglie questa opzione, è necessario selezionare almeno un database nell'elenco.  
  
Casella di controllo **Includi indici**  
 - Viene controllata l'integrità di tutte le pagine di indice, nonché delle pagine dei dati della tabella.  
  
**Solo fisica**  
 - Limita il controllo di integrità alla struttura fisica della pagina, alle intestazioni dei record e alla coerenza di allocazione del database. L'uso di questa opzione può ridurre i tempi di esecuzione di DBCC CHECKDB in database di grandi dimensioni ed è quindi consigliato in caso di utilizzo frequente nei sistemi di produzione.  
  
**Tablock**  
 - Consente a DBCC CHECKDB di ottenere blocchi invece di utilizzare uno snapshot di database interno, incluso un blocco esclusivo (X) sul database di breve durata. Questa opzione consente un'esecuzione più rapida di DBCC CHECKDB in database con carico di lavoro elevato, ma comporta una diminuzione del livello di concorrenza del database durante l'esecuzione del comando.  
  
## <a name="define-database-shrink-tasks"></a>Definizione attività Compatta database  
  
1.  Nella pagina **Definizione attività Compatta database** creare un'attività che tenti di ridurre le dimensioni dei database selezionati tramite l'istruzione `DBCC SHRINKDATABASE` , con l'opzione `NOTRUNCATE` o `TRUNCATEONLY` . Per altre informazioni, vedere [DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md). Al termine, fare clic su **Avanti**.  
  
    > **AVVISO** I dati spostati per ridurre un file possono essere dispersi in qualsiasi percorso disponibile nel file, provocando la frammentazione dell'indice e rallentando le prestazioni di query che eseguono ricerche in un intervallo dell'indice Per eliminare la frammentazione, valutare la possibilità di ricompilare gli indici sul file dopo la compattazione.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     Elenco **Database**  
     Consente di specificare i database su cui verrà eseguita l'attività. Per ulteriori informazioni sulle opzioni disponibili in questo elenco, vedere il passaggio 9.  
  
     Casella **Compatta database quando le dimensioni superano**  
     Specificare le dimensioni in megabyte che causano l'esecuzione dell'attività.  
  
     Casella **Spazio che deve rimanere disponibile dopo la compattazione**  
     Arresta l'attività di compattazione quando lo spazio disponibile nei file del database raggiunge questa soglia (come percentuale).  
  
     **Mantieni spazio liberato nei file di database**  
     Il database viene organizzato in pagine contigue, ma queste ultime non vengono deallocate, né i file del database vengono compattati. Utilizzare questa opzione se si prevede una nuova espansione del database e non si desidera riallocare lo spazio. Con questa opzione, i file del database non verranno compattati al massimo. L'opzione utilizza l'istruzione NOTRUNCATE.  
  
     **Restituisci spazio liberato al sistema operativo**  
     Il database viene organizzato in pagine contigue e queste ultime vengono rilasciate al sistema operativo per essere utilizzate da altri programmi. L'opzione utilizza l'istruzione TRUNCATEONLY. Questa è l'opzione predefinita.  
  
## <a name="define-the-index-tasks"></a>Definizione delle attività dell'indice  
  
1.  Nella pagina **Definizione attività Riorganizza indice** selezionare il server o i server in cui le pagine dell'indice verranno spostate in un ordine di ricerca più efficiente. In questa attività viene utilizzata l'istruzione `ALTER INDEX ... REORGANIZE`. Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md). Al termine, fare clic su **Avanti**.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     Elenco **Database**  
     Consente di specificare i database su cui verrà eseguita l'attività. Per ulteriori informazioni sulle opzioni disponibili in questo elenco, vedere il passaggio 9.  
  
     Elenco **Oggetti**  
     Limitare l'elenco **Selezione** alla visualizzazione di tabelle, viste o entrambe. Questo elenco è disponibile solo se si sceglie un solo database nell'elenco **Database** .  
  
     Elenco **Selezione**  
     Specificare le tabelle o gli indici su cui verrà eseguita l'attività. Questa opzione non è disponibile quando si seleziona **Tabelle e viste** nella casella Oggetto.  
  
     Casella di controllo **Compatta oggetti di grandi dimensioni**  
     Dealloca spazio per tabelle e viste, se possibile. Questa opzione utilizza l'istruzione `ALTER INDEX ... LOB_COMPACTION = ON`  
  
2.  Nella pagina **Definizione attività Ricompila indice** selezionare il database o i database in cui verranno creati più indici. In questa attività viene utilizzata l'istruzione `ALTER INDEX ... REBUILD PARTITION`. Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)). Al termine, fare clic su **Avanti**.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     Elenco **Database**  
     Consente di specificare i database su cui verrà eseguita l'attività. Per ulteriori informazioni sulle opzioni disponibili in questo elenco, vedere il passaggio 9.  
  
     Elenco **Oggetti**  
     Limitare l'elenco **Selezione** alla visualizzazione di tabelle, viste o entrambe. Questo elenco è disponibile solo se si sceglie un solo database nell'elenco **Database** .  
  
     Elenco **Selezione**  
     Specificare le tabelle o gli indici su cui verrà eseguita l'attività. Questa opzione non è disponibile quando si seleziona **Tabelle e viste** nella casella Oggetto.  
  
     Area **Opzioni spazio disponibile**  
     Contiene opzioni relative all'applicazione del fattore di riempimento a indici e tabelle.  
  
     **Spazio libero predefinito per pagina**  
     Riorganizza le pagine mantenendo la quantità predefinita di spazio disponibile. Selezionando questa opzione verranno eliminati gli indici delle tabelle del database e verranno ricreati utilizzando il fattore di riempimento specificato al momento della creazione degli indici. Questa è l'opzione predefinita.  
  
     Casella **Modifica percentuale di spazio disponibile per pagina**  
     Elimina gli indici delle tabelle del database e li ricrea utilizzando un nuovo fattore di riempimento calcolato automaticamente, riservando in tal modo la quantità di spazio disponibile specificata nelle pagine dell'indice. Maggiore è la percentuale, maggiore sarà la quantità di spazio disponibile riservata nelle pagine dell'indice e maggiori saranno le dimensioni dell'indice. I valori validi sono compresi tra 0 e 100. Utilizza l'opzione `FILLFACTOR` .  
  
     Area **Opzioni avanzate**  
     Presenta opzioni aggiuntive per l'ordinamento degli indici e la reindicizzazione.  
  
     Casella di controllo **Ordina risultati in tempdb**  
     Utilizza l'opzione `SORT_IN_TEMPDB` che determina la posizione in cui i risultati intermedi dell'ordinamento, generati durante la creazione dell'indice, vengono memorizzati temporaneamente. Se non è necessario eseguire un'operazione di ordinamento o se l'ordinamento può essere eseguito in memoria, l'opzione `SORT_IN_TEMPDB` viene ignorata.  
  
     Casella di controllo **Riempi indice**  
     Utilizza l'opzione `PAD_INDEX` .  
  
     Casella di controllo **Mantieni indici online durante la reindicizzazione**  
     Utilizza l'opzione `ONLINE` per consentire agli utenti di accedere alla tabella o ai dati dell'indice cluster sottostanti, nonché agli eventuali indici non cluster associati durante le operazioni sugli indici. La selezione di questa opzione comporta l'attivazione di opzioni aggiuntive per la ricompilazione degli indici che non consentono le ricompilazioni online: **Non ricompilare indici** e **Ricompila indici offline**.  
  
     La selezione di questa opzione attiva anche Bassa priorità usata, che usa l'opzione `WAIT_AT_LOW_PRIORITY` . Le operazioni di ricompilazione dell'indice online rimangono in attesa dei blocchi con priorità bassa per `MAX_DURATION` minuti, consentendo la continuazione delle altre operazioni mentre quella di compilazione dell'indice online è in attesa.  
  
    > **NOTA** Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Casella di controllo **MAXDOP**  
     Esegue l'override dell'opzione di configurazione Massimo grado di parallelismo di sp_configure per DBCC CHECKDB. Per altre informazioni, vedere [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
  
#### <a name="define-the-update-statistics-task"></a>Definizione attività Aggiorna statistiche  
  
1.  Nella pagina **Definizione attività Aggiorna statistiche** definire il database o i database su cui verranno aggiornate le statistiche relative a tabelle e indici. In questa attività viene utilizzata l'istruzione `UPDATE STATISTICS`. Per altre informazioni, vedere [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md) Al termine fare clic su **Avanti**  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     Elenco **Database**  
     Consente di specificare i database su cui verrà eseguita l'attività. Per ulteriori informazioni sulle opzioni disponibili in questo elenco, vedere il passaggio 9.  
  
     Elenco **Oggetti**  
     Limitare l'elenco **Selezione** alla visualizzazione di tabelle, viste o entrambe. Questo elenco è disponibile solo se si sceglie un solo database nell'elenco **Database** .  
  
     Elenco **Selezione**  
     Specificare le tabelle o gli indici su cui verrà eseguita l'attività. Questa opzione non è disponibile quando si seleziona **Tabelle e viste** nella casella Oggetto.  
  
     **Tutte le statistiche esistenti**  
     Consente di aggiornare le statistiche sia per le colonne che per gli indici.  
  
     **Solo statistiche colonne**  
     Consente di aggiornare soltanto le statistiche relative alle colonne. Utilizza l'opzione `WITH COLUMNS` .  
  
     **Solo statistiche indici**  
     Consente di aggiornare soltanto le statistiche relative agli indici. Utilizza l'opzione `WITH INDEX` .  
  
     **Tipo analisi**  
     Tipo di analisi utilizzata per raccogliere statistiche aggiornate.  
  
     **Analisi completa**  
     Consente di leggere tutte le righe della tabella o della vista per raccogliere le statistiche.  
  
     **Campionamento per**  
     Specificare la percentuale della tabella o della vista indicizzata o il numero di righe da utilizzare come campione durante la raccolta di statistiche per tabelle o viste di grandi dimensioni.  
  
#### <a name="define-the-history-cleanup-task"></a>Definizione attività Pulizia contenuto cronologia  
  
1.  Nella pagina **Definizione attività Pulizia contenuto cronologia** definire il database o i database in cui si desidera rimuovere la cronologia attività obsoleta. In questa attività vengono utilizzate le istruzioni `EXEC sp_purge_jobhistory`, `EXEC sp_maintplan_delete_log`e `EXEC sp_delete_backuphistory` per rimuovere le informazioni cronologiche dalle tabelle di **msdb** . Al termine, fare clic su **Avanti**.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     **Selezionare i dati cronologici da eliminare**  
     Scegliere il tipo di dati attività da eliminare.  
  
     **Cronologia operazioni di backup e ripristino**  
     Se si mantengono i record relativi alla data di creazione di backup recenti, è possibile semplificare la creazione di un piano di recupero da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in caso si desideri ripristinare un database. Il periodo di memorizzazione deve essere almeno pari alla frequenza dei backup completi del database.  
  
     **Cronologia processo di SQL Server Agent**  
     Questa cronologia si rivela utile per risolvere problemi relativi a processi non riusciti o per individuare le cause di determinate azioni sul database.  
  
     **Cronologia piano di manutenzione**  
     Questa cronologia si rivela utile per risolvere problemi relativi a processi del piano di manutenzione non riusciti o per individuare le cause di determinate azioni sul database.  
  
     **Rimuovi dati presenti nella cronologia da più di**  
     Specifica il periodo di permanenza nella cronologia oltre il quale gli elementi devono essere eliminati. È possibile specificare **Ora/e**, **Giorno/i**, **Settimana/e** (impostazione predefinita), **Mese/i** o **Anno/i**  
  
#### <a name="define-the-execute-agent-job-task"></a>Definizione attività Esegui processo di SQL Server Agent  
  
1.  Nella pagina **Definizione attività Esegui processo di SQL Server Agent** scegliere il processo o i processi da eseguire in **Processi di SQL Server Agent disponibili**. Se non sono presenti processi di SQL Agent, non è possibile utilizzare questa opzione. In questa attività viene utilizzata l'istruzione `EXEC sp_start_job`. Per altre informazioni, vedere [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) Al termine fare clic su **Avanti**.  
  
#### <a name="define-backup-tasks"></a>Definizione attività Backup  
  
1.  Nella pagina **Definizione attività Backup database (completo)** selezionare il database o i database su cui eseguire un backup completo. In questa attività viene utilizzata l'istruzione `BACKUP DATABASE`. Per altre informazioni, vedere [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Al termine, fare clic su **Avanti**.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     Elenco **Tipo di backup**  
     Visualizza il tipo di backup da eseguire. Questo valore è di sola lettura.  
  
     Elenco **Database**  
     Consente di specificare i database su cui verrà eseguita l'attività. Per ulteriori informazioni sulle opzioni disponibili in questo elenco, vedere il passaggio 9.  
  
     **Componente di cui eseguire il backup**  
     Selezionare **Database** per eseguire il backup dell'intero database. Selezionare **File e filegroup** per eseguire il backup solo di una parte del database. Quando si seleziona questa opzione, è necessario specificare il nome del file o del filegroup. Se nella casella **Database** sono selezionati più database, è necessario specificare **Database** solo per **Componente di cui eseguire il backup**. Per eseguire i backup di file o filegroup, creare un'attività per ogni database. Queste opzioni sono disponibili solo se si sceglie un solo database nell'elenco **Database** .  
  
     Casella di controllo **Scadenza set di backup**  
     Indica quando è possibile sovrascrivere il set di backup per il backup specifico. Selezionare **Dopo** e immettere un numero di giorni alla scadenza oppure selezionare **Il** e immettere una data di scadenza. Questa opzione è disabilitata se è selezionato **URL** come destinazione di backup.  
  
     **Backup su**  
     Specifica il supporto su cui eseguire il backup del database. Selezionare **Disco**, **Nastro** o **URL**. Sono disponibili solo i dispositivi nastro collegati al computer in cui è archiviato il database.  
  
     **Backup database in uno o più file**  
     Fare clic su **Aggiungi** per aprire la finestra di dialogo **Seleziona destinazione di backup** . Questa opzione è disabilitata se è stato selezionato URL come destinazione di backup.  
  
     Fare clic su **Rimuovi** per rimuovere un file dalla casella.  
  
     Fare clic su **Contenuto** per leggere l'intestazione del file e visualizzare il contenuto del backup corrente del file.  
  
     Finestra di dialogo **Seleziona destinazione di backup**  
     Selezionare il file, l'unità nastro o il dispositivo di backup come destinazione. Questa opzione è disabilitata se è stato selezionato URL come destinazione di backup.  
  
     Elenco **Azione per file di backup esistenti**  
     Specifica il modo in cui devono essere gestiti i backup esistenti. Selezionare **Accoda** per aggiungere i nuovi backup dopo eventuali backup esistenti nel file o sul nastro. Selezionare **Sovrascrivi** per rimuovere il contenuto meno recente dal file o dal nastro e sostituirlo con il nuovo backup.  
  
     **Crea un file di backup per ogni database**  
     Creare un file di backup nel percorso specificato nella casella della cartella. Viene creato un file per ciascun database selezionato. Questa opzione è disabilitata se è stato selezionato URL come destinazione di backup.  
  
     Casella di controllo **Crea una sottodirectory per ogni database**  
     Crea una sottodirectory nella directory specificata che contiene il database di cui si esegue il backup nell'ambito del piano di manutenzione.  
  
    > **IMPORTANTE** La sottodirectory erediterà le autorizzazioni dalla relativa directory padre. Limitare le autorizzazioni per impedire l'accesso non autorizzato.  
  
     Casella **Cartella**  
     Specificare la cartella in cui inserire i file di database creati automaticamente. Questa opzione è disabilitata se è stato selezionato URL come destinazione di backup.  
  
     **Credenziali SQL**  
     Selezionare le credenziali SQL usate per l'autenticazione in Archiviazione di Azure. Se non si dispone di credenziali SQL esistenti utilizzabili, fare clic sul pulsante **Crea** per crearne delle nuove.  
  
    > **IMPORTANTE** La finestra di dialogo visualizzata quando si fa clic su **Crea** richiede un certificato di gestione o il profilo di pubblicazione per la sottoscrizione. Se non si dispone dell'accesso al certificato di gestione o al profilo di pubblicazione, è possibile creare le credenziali di SQL specificando il nome dell'account di archiviazione e le informazioni sulla chiave di accesso tramite Transact-SQL o SQL Server Management Studio. Vedere il codice di esempio nell'argomento [Creare credenziali](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) per creare le credenziali tramite Transact-SQL. In alternativa, utilizzando SQL Server Management Studio, dall'istanza del motore di database, fare clic con il pulsante destro del mouse su **Sicurezza**, scegliere **Nuovo** e selezionare **Credenziale**. Specificare il nome dell'account di archiviazione per **Identity** e la chiave di accesso nel campo **Password** .  
  
     **Contenitore di archiviazione di Azure**  
     Specificare il nome del contenitore di Archiviazione di Azure  
  
     **Prefisso URL**  
     Viene generato automaticamente in base alle informazioni sull'account di archiviazione archiviate nelle credenziali SQL e al nome del contenitore di archiviazione di Azure specificato. Si consiglia di non modificare le informazioni in questo campo a meno che non si usi un dominio con un formato diverso da **\<storage account>.blob.core.windows.net**.  
  
     Casella **Estensione file di backup**  
     Specificare l'estensione da utilizzare per i file di backup. L'estensione predefinita è bak.  
  
     Casella di controllo **Verifica integrità backup**  
     Consente di verificare che il set di backup sia completo e che tutti i volumi siano leggibili.  
  
     Casella di controllo **Esegui checksum**  
     Verifica in ogni pagina la presenza di checksum e di pagine incomplete, se abilitati e disponibili, e genera un checksum per l'intero backup.  
  
     Casella di controllo **Continua in caso di errori**  
     Imposta il proseguimento dell'istruzione BACKUP anche in caso di errori, ad esempio checksum non validi o pagine incomplete.  
  
     **Crittografia dei backup**  
     Per creare un backup crittografato, selezionare la casella di controllo **Crittografa backup** . Selezionare un algoritmo di crittografia da utilizzare per il passaggio di crittografia e specificare un certificato o una chiave asimmetrica da un elenco di chiavi asimmetriche o di certificati esistenti. Gli algoritmi di crittografia disponibili sono:  
  
    -   AES 128  
  
    -   AES 192  
  
    -   AES 256  
  
    -   Triple DES  
  
     L'opzione di crittografia è disabilitata se si è scelto di eseguire l'accodamento a un set di backup esistente.  
  
     È consigliabile eseguire il backup del certificato o delle chiavi e archiviarli in un percorso diverso da quello utilizzato per il backup crittografato.  
  
     Sono supportate solo le chiavi che si trovano in Extensible Key Management (EKM).  
  
     Casella di controllo **Dimensione blocco** , elenco  
  
     Specifica le dimensioni fisiche del blocco, in byte. In genere, questa opzione influisce sulle prestazioni durante la scrittura nei dispositivi nastro, nelle matrici RAID o nella rete SAN.  
  
     Casella di controllo **Dimensioni massime trasferimento** , elenco  
  
     Specifica le dimensioni massime, in byte, per il trasferimento tra SQL Server e i supporti di backup.  
  
     Elenco **Imposta compressione backup**  
     In [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] o versioni successive, selezionare uno dei valori di [compressione di backup](../../relational-databases/backup-restore/backup-compression-sql-server.md) seguenti:  
  
    |Valore|Descrizione|  
    |-|-|  
    |**Utilizza l'impostazione predefinita del server**|Fare clic su questa opzione per utilizzare l'impostazione predefinita a livello di server. Questa impostazione predefinita è specificata dall'opzione di configurazione del server **Valore predefinito di compressione backup** . Per informazioni su come visualizzare l'impostazione corrente di questa opzione, vedere [Visualizzare o configurare l'opzione di configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
    |**Comprimi backup**|Fare clic su questa opzione per comprimere il backup, indipendentemente dall'impostazione predefinita a livello di server.<br /><br /> **\*\* Importante \*\*** Per impostazione predefinita, la compressione aumenta significativamente l'uso della CPU e la CPU aggiuntiva usata dal processo di compressione può avere un impatto negativo sulle operazioni simultanee. Potrebbe pertanto essere necessario creare backup compressi con priorità bassa in una sessione in cui l'utilizzo della CPU è limitato da Resource Governor. Per ulteriori informazioni, vedere [Utilizzo di Resource Governor per limitare l'utilizzo della CPU da parte della compressione dei backup &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
    |**Non comprimere il backup**|Fare clic su questa opzione per creare un backup non compresso, indipendentemente dall'impostazione predefinita a livello di server.|  
  
2.  Nella pagina **Definizione attività Backup database (differenziale)** selezionare il database o i database su cui eseguire un backup parziale. Per ulteriori informazioni sulle opzioni disponibili in questa pagina, vedere l'elenco delle definizioni nel passaggio 16. In questa attività viene utilizzata l'istruzione `BACKUP DATABASE ... WITH DIFFERENTIAL`. Per altre informazioni, vedere [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  Al termine, fare clic su **Avanti**.  
  
3.  Nella pagina **Definizione attività Backup database (log trans)** selezionare il database o i database su cui eseguire un backup per un log delle transazioni. Per ulteriori informazioni sulle opzioni disponibili in questa pagina, vedere l'elenco delle definizioni nel passaggio 16. In questa attività viene utilizzata l'istruzione `BACKUP LOG`. Per altre informazioni, vedere [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Al termine, fare clic su **Avanti**.  
  
#### <a name="define-maintenance-cleanup-tasks"></a>Definizione attività Pulizia file manutenzione  
  
1.  Nella pagina **Definizione attività Pulizia file manutenzione** specificare i tipi di file da eliminare come parte del piano di manutenzione, inclusi report di testo creati dai piani di manutenzione e file di backup del database. In questa attività viene utilizzata l'istruzione `EXEC xp_delete_file`. Al termine, fare clic su **Avanti**.  
  
    > **IMPORTANTE** Tramite questa attività non si eliminano automaticamente i file nelle sottocartelle della directory specificata. Si tratta di una misura precauzionale per ridurre le probabilità di un attacco dannoso che utilizzi l'attività Pulizia file manutenzione per eliminare i file. Per eliminare i file nelle sottocartelle di primo livello è necessario selezionare **Includi sottocartelle di primo livello**.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     **Elimina i file del tipo seguente**  
     Specifica il tipo di file da eliminare.  
  
     **File di backup**  
     Elimina i file di backup di database.  
  
     **Report in formato testo piano di manutenzione**  
     Elimina i report in formato testo relativi a piani di manutenzione eseguiti in precedenza.  
  
     **Percorso del file**  
     Specifica il percorso dei file da eliminare.  
  
     **Elimina file specifico**  
     Elimina il file specifico indicato nella casella di testo **Nome file** .  
  
     **Cerca nella cartella ed elimina i file in base all'estensione**  
     Consente di eliminare tutti i file con l'estensione specificata contenuti nella cartella indicata. Utilizzare questa opzione per eliminare più file contemporaneamente, ad esempio tutti i file di backup con estensione bak contenuti nella cartella specificata.  
  
     Casella **Cartella**  
     Percorso e nome della cartella contenente i file da eliminare.  
  
     Casella **Estensione file**  
     Indica l'estensione dei file da eliminare. Per eliminare più file contemporaneamente, ad esempio tutti i file di backup con estensione bak contenuti nella cartella specificata, specificare l'estensione bak.  
  
     Casella di controllo **Includi sottocartelle di primo livello**  
     Vengono eliminati i file con l'estensione specificata in **Estensione file** dalle sottocartelle di primo livello nella cartella specificata in **Cartella**.  
  
     Casella di controllo **Elimina i file in base alla data del file al momento dell'esecuzione dell'attività**  
     Specificare il periodo di memorizzazione minimo trascorso il quale i file verranno eliminati, indicando un numero e un'unità di tempo nella casella **Elimina i file con data anteriore a** .  
  
     **Elimina i file con data anteriore a**  
     Specificare il periodo di memorizzazione minimo trascorso il quale i file verranno eliminati indicando un numero e un'unità di tempo (**Ora/e**, **Giorno/i**, **Settimana/e**, **Mese/i** o **Anno/i**). I file con data anteriore alla data specificata verranno eliminati.  
  
#### <a name="select-report-options"></a>Selezione opzioni report  
  
1.  Nella pagina **Selezione opzioni report** selezionare le opzioni per salvare o distribuire un report delle azioni del piano di manutenzione. In questa attività viene utilizzata l'istruzione `EXEC sp_notify_operator`. Per altre informazioni, vedere [sp_notify_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-notify-operator-transact-sql.md).Al termine fare clic su **Avanti**.  
  
     In questa pagina sono disponibili le opzioni seguenti.  
  
     Casella di controllo **Scrivi report in un file di testo**  
     Salva il report in un file.  
  
     Casella **Percorso cartella**  
     Specifica il percorso del file che conterrà il report.  
  
     Casella di controllo **Invia report tramite posta elettronica**  
     Inviare un messaggio di posta elettronica quando un'attività non viene completata in seguito a un errore. Per usare questa attività, l'opzione Posta elettronica database deve essere abilitata e configurata correttamente con MSDB come database host della posta elettronica e un operatore [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent deve avere un indirizzo di posta elettronica valido.  
  
     **Operatore agente**  
     Consente di specificare il destinatario del messaggio di posta elettronica.  
  
     **Profilo posta**  
     Specificare il profilo mediante il quale viene definito il mittente del messaggio di posta elettronica.  
  
#### <a name="complete-the-wizard"></a>Completamento procedura guidata  
  
1.  Nella pagina **Completamento procedura guidata** verificare le scelte effettuate nelle pagine precedenti, quindi scegliere **Fine**.  
  
2.  Nella pagina **Stato Creazione guidata piano di manutenzione** monitorare le informazioni di stato sulle azioni della Creazione guidata piano di manutenzione. A seconda delle opzioni selezionate nella procedura guidata, la pagina di stato può contenere una o più azioni. Nella casella superiore viene visualizzato lo stato complessivo della procedura guidata e viene indicato il numero di messaggi di stato, di errore e di avviso restituiti durante l'esecuzione della procedura guidata.  
  
     Nella pagina **Stato Creazione guidata piano di manutenzione** sono disponibili le opzioni seguenti:  
  
     **Dettagli**  
     Consente di visualizzare i messaggi di azione, di stato e di altro tipo restituiti dall'azione eseguita nella procedura guidata.  
  
     **Azione**  
     Specifica il tipo e il nome di ciascuna azione.  
  
     **Status**  
     Indica se l'intera azione della procedura guidata ha restituito il valore **Esito positivo** o **Esito negativo**.  
  
     **Messaggio**  
     Fornisce tutti i messaggi di errore o di avviso restituiti dal processo.  
  
     **Report**  
     Crea un report contenente i risultati della Creazione guidata partizione. Le opzioni sono **Visualizza report**, **Salva report su file**, **Copia report negli Appunti** e **Invia report per posta elettronica**.  
  
     **Visualizza report**  
     Apre la finestra di dialogo **Visualizza report** in cui è contenuto un report di testo dello stato della Creazione guidata partizione.  
  
     **Salva report su file**  
     Apre la finestra di dialogo **Salva report con nome** .  
  
     **Copia report negli Appunti**  
     Copia i risultati del report dello stato della procedura guidata negli Appunti.  
  
     **Invia report per posta elettronica**  
     Copia i risultati del report dello stato della procedura guidata in un messaggio di posta elettronica.  
  
  

