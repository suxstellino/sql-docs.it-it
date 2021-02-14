---
title: Trovare errori con la replica transazionale
description: Viene descritto come individuare e identificare gli errori con la replica transazionale, nonché la metodologia di risoluzione dei problemi per risolvere i problemi relativi alla replica.
ms.custom: seo-lt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], tutorials
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d39286c1bc228a9da76a163a39e76dbb2022d807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348643"
---
# <a name="troubleshooter-find-errors-with-sql-server-transactional-replication"></a>Risoluzione dei problemi: Trovare gli errori con la replica transazionale di SQL Server 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

La risoluzione degli errori di replica può risultare frustrante senza una conoscenza di base del funzionamento della replica transazionale. Il primo passaggio per la creazione di una pubblicazione prevede che l'agente di snapshot crei lo snapshot e lo salvi nella cartella degli snapshot. Successivamente, l'agente di distribuzione applica lo snapshot al sottoscrittore. 

Questo processo crea la pubblicazione e la pone nello stato di *sincronizzazione in corso*. La sincronizzazione funziona in tre fasi:
1. Le transazioni vengono eseguite su oggetti replicati e contrassegnati "per la replica" nel log delle transazioni. 
2. L'agente di lettura log esegue l'analisi del log delle transazioni ed esegue una ricerca delle transazioni contrassegnate "per la replica." Queste transazioni vengono quindi salvate nel database di distribuzione. 
3. L'agente di distribuzione esegue un'analisi del database di distribuzione tramite il thread di lettura. Quindi, usando il thread di scrittura, questo agente si connette al sottoscrittore per applicare tali modifiche nel sottoscrittore.

Gli errori possono verificarsi in qualsiasi passaggio del processo. Trovare questi errori può essere l'aspetto più complesso della risoluzione dei problemi di sincronizzazione. Fortunatamente, l'uso di Monitoraggio replica semplifica questo processo. 

>[!NOTE]
> - Lo scopo di questa guida alla risoluzione dei problemi è insegnare una metodologia di risoluzione dei problemi. Non è progettata per risolvere errori specifici, ma per fornire indicazioni generali per l'individuazione degli errori con la replica. Vengono forniti alcuni esempi specifici, ma la loro risoluzione può variare a seconda dell'ambiente. 
> - Gli errori riportati in questa guida come esempi sono basati sull'esercitazione [Configurare la replica tra due server sempre connessi (replica transazionale)](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md).



## <a name="troubleshooting-methodology"></a>Metodologia di risoluzione degli errori 

### <a name="questions-to-ask"></a>Domande da porsi
1. In quale fase del processo di sincronizzazione si verificano errori della replica?
2. Per quale agente si verifica un errore?
1. Quando ha funzionato correttamente la replica per l'ultima volta? Cosa è cambiato da allora?  

### <a name="steps-to-take"></a>Passaggi da eseguire
1. Usare Monitoraggio replica per identificare il punto in cui la replica riscontra l'errore (quale agente?):
   - Se gli errori si verificano nella sezione **Dal server di pubblicazione al database di distribuzione**, il problema riguarda l'agente di lettura log. 
   - Se gli errori si verificano nella sezione **Dal database di distribuzione al Sottoscrittore**, il problema riguarda l'agente di distribuzione.  
2. Esaminare la cronologia dei processi dell'agente in Monitoraggio attività processi per identificare i dettagli dell'errore. Se la cronologia processo non mostra dettagli sufficienti, è possibile [abilitare la registrazione dettagliata](#enable-verbose-logging-on-any-agent) per tale agente specifico.
3. Provare a determinare una soluzione per l'errore.


## <a name="find-errors-with-the-snapshot-agent"></a>Trovare gli errori che riguardano l'agente di snapshot
L'agente di snapshot genera lo snapshot e lo scrive nella cartella degli snapshot specificata. 

1. Visualizzare lo stato dell'agente di snapshot:

    a. In Esplora oggetti espandere il nodo **Pubblicazione locale** in **Replica**.

    b. Fare clic con il pulsante destro del mouse sulla pubblicazione **AdvWorksProductTrans** > **Visualizza stato agente snapshot**. 

    ![Comando "Visualizza stato agente snapshot "del menu di scelta rapida](media/troubleshooting-tran-repl-errors/view-snapshot-agent-status.png)

1. Se nello stato dell'agente di snapshot viene segnalato un errore, è possibile trovare altri dettagli nella cronologia processo dell'agente di snapshot:

    a. Espandere **SQL Server Agent** in Esplora oggetti e aprire Monitoraggio attività processi. 

    b. Ordinare per **Categoria** e identificare l'agente di snapshot in base alla categoria **REPL-Snapshot**.

    c. Fare clic con il pulsante destro del mouse sull'agente di snapshot e quindi scegliere **Visualizza cronologia**. 

   ![Selezioni per l'apertura della cronologia dell'agente di snapshot](media/troubleshooting-tran-repl-errors/snapshot-agent-history.png)
    
1. Nella cronologia dell'agente di snapshot selezionare la voce di log pertinente, in genere una o due righe *prima* della voce che segnala l'errore. (Una X rossa indica errori.) Esaminare il testo del messaggio nella casella sotto i log: 

    ![Errore dell'agente di snapshot per accesso negato](media/troubleshooting-tran-repl-errors/snapshot-access-denied.png)

    ```console
    The replication agent had encountered an exception.
    Exception Message: Access to path '\\node1\repldata.....' is denied.
    ```

Se le autorizzazioni di Windows dell'utente per la cartella degli snapshot non sono configurate correttamente, verrà visualizzato l'errore "accesso negato" per l'agente di snapshot. È necessario verificare le autorizzazioni per la cartella in cui viene archiviato lo snapshot e assicurarsi che l'account usato per eseguire l'agente di snapshot abbia le autorizzazioni per accedere alla condivisione.  

## <a name="find-errors-with-the-log-reader-agent"></a>Trovare gli errori che riguardano l'agente di lettura log
L'agente di lettura log si connette al database del server di pubblicazione e analizza il log delle transazioni per tutte le transazioni contrassegnate "per la replica". Aggiunge quindi queste transazioni al database di distribuzione. 

1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Espandere il nodo server, fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi selezionare **Avvia Monitoraggio replica**:  

    ![Comando "Avvia Monitoraggio replica" nel menu di scelta rapida](media/troubleshooting-tran-repl-errors/launch-repl-monitor.png)
  
    Si apre Monitoraggio replica: ![Monitoraggio replica](media/troubleshooting-tran-repl-errors/repl-monitor.png) 
   
2. La X rossa indica che la pubblicazione non viene sincronizzata. Espandere **Server di pubblicazione personali** sul lato sinistro e quindi espandere il server di pubblicazione pertinente.  
  
3.  Selezionare la pubblicazione **AdvWorksProductTrans** a sinistra e quindi cercare la X rossa in una delle schede per identificare dove risiede il problema. In questo caso, la X rossa è nella scheda **Agenti** e indica che uno degli agenti sta riscontrando un errore: 

    ![X rossa nella scheda "Agenti"](media/troubleshooting-tran-repl-errors/agent-error.png)

4. Selezionare la scheda **Agenti** per identificare l'agente per il quale è stato rilevato l'errore: 

    ![X rossa per l'agente di lettura log con errore](media/troubleshooting-tran-repl-errors/log-reader-agent-failure.png)


5. Questa visualizzazione mostra due agenti, l'agente di snapshot e l'agente di lettura log. Quello che ha riscontrato un errore è contrassegnato con la X rossa. In questo caso, si tratta dell'agente di lettura log. 

    Fare doppio clic sulla riga con la segnalazione dell'errore per aprire la cronologia per l'agente di lettura log. Questa cronologia offre altre informazioni sull'errore: 
    
    ![Dettagli dell'errore per l'agente di lettura log](media/troubleshooting-tran-repl-errors/log-reader-error.png)

    ```console
    Status: 0, code: 20011, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.
    The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.
    Status: 0, code: 15517, text: 'Cannot execute as the database principal because the principal "dbo" does not exist, this type of principal cannot be impersonated, or you do not have permission.'.
    Status: 0, code: 22037, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.        
    ```

6. L'errore si verifica in genere quando il proprietario del server di pubblicazione non è impostato correttamente. Questa situazione può verificarsi quando un database viene ripristinato. Per verificare ciò:

    a. Espandere **Database** in Esplora oggetti.

    b. Fare clic con il pulsante destro del mouse su **AdventureWorks2012** > **Proprietà**. 

    c. Verificare l'esistenza di un proprietario nella pagina **File**. Se questa casella è vuota, questa è la causa probabile del problema. 

   ![Pagina "File" nelle proprietà del database, con una casella "Proprietario" vuota](media/troubleshooting-tran-repl-errors/db-properties.png)

7. Se il proprietario è vuoto nella pagina **File**, aprire una finestra **Nuova query** all'interno del contesto del database AdventureWorks2012. Eseguire il codice T-SQL seguente:

    ```sql
    -- set the owner of the database to 'sa' or a specific user account, without the brackets. 
    EXECUTE sp_changedbowner '<useraccount>'
    -- example for sa: exec sp_changedbowner 'sa'
    -- example for user account: exec sp_changedbowner 'sqlrepro\administrator' 
    ```

8. Potrebbe essere necessario riavviare l'agente di lettura log:

    a. Espandere il nodo **SQL Server Agent** in Esplora oggetti e aprire Monitoraggio attività processi.

    b. Ordinare per **Categoria** e identificare l'agente di lettura log in base alla categoria **REPL-LogReader**. 

    c. Fare clic con il pulsante destro del mouse sul processo dell'**agente di lettura log** e scegliere **Inizia processo al passaggio**. 

    ![Selezioni per riavviare l'agente di lettura log](media/troubleshooting-tran-repl-errors/start-job-at-step.png)

9. Verificare che la pubblicazione venga ora sincronizzata aprendo di nuovo Monitoraggio replica. Se non è già aperto, è possibile trovarlo facendo clic con il pulsante destro del mouse su **Replica** in Esplora oggetti. 
10. Selezionare la pubblicazione **AdvWorksProductTrans**, selezionare la scheda **Agenti** e fare doppio clic sull'agente di lettura log per aprire la cronologia dell'agente. È ora possibile vedere l'agente di lettura log in esecuzione, nonché i comandi di replica oppure il messaggio "Nessuna transazione replicata disponibile":

    ![Agente di lettura log in esecuzione con nessuna transazione replicata](media/troubleshooting-tran-repl-errors/log-reader-running.png)

## <a name="find-errors-with-the-distribution-agent"></a>Trovare gli errori che riguardano l'agente di distribuzione
L'agente di distribuzione trova i dati nel database di distribuzione e quindi li applica al sottoscrittore. 

1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Espandere il nodo del server, fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi scegliere **Avvia Monitoraggio replica**.  
2. In **Monitoraggio replica** selezionare la pubblicazione **AdvWorksProductTrans** e selezionare la scheda **Tutte le sottoscrizioni**. Fare clic con il pulsante destro del mouse sulla sottoscrizione e scegliere **Visualizza dettagli**:

    ![Comando "Visualizza dettagli" nel menu di scelta rapida](media/troubleshooting-tran-repl-errors/view-details.png)

2. Verrà visualizzata la finestra di dialogo **Cronologia database di distribuzione - Sottoscrittore** con chiarimenti sull'errore riscontrato dall'agente: 

     ![Dettagli dell'errore per l'agente di distribuzione](media/troubleshooting-tran-repl-errors/dist-history-error.png)

    ```console
    Error messages:
    Agent 'NODE1\SQL2016-AdventureWorks2012-AdvWorksProductTrans-NODE2\SQL2016-7' is retrying after an error. 89 retries attempted. See agent job history in the Jobs folder for more details.
    ```

3. L'errore indica che l'agente di distribuzione esegue nuovi tentativi. Per trovare altre informazioni, controllare la cronologia processo dell'agente di distribuzione: 

    a. Espandere **SQL Server Agent** in Esplora oggetti > **Monitoraggio attività processi**. 
    
    b. Ordinare i processi per **Categoria**. 

    c. Identificare l'agente di distribuzione in base alla categoria **REPL-Distribution**. Fare clic con il pulsante destro del mouse sull'agente e scegliere **Visualizza cronologia**.

    ![Selezioni per la visualizzazione della cronologia dell'agente di distribuzione](media/troubleshooting-tran-repl-errors/view-dist-agent-history.png)

5. Selezionare una delle voci dell'errore e visualizzare il testo corrispondente nella parte inferiore della finestra:  

    ![Testo dell'errore che indica una password errata per l'agente di distribuzione](media/troubleshooting-tran-repl-errors/dist-pw-wrong.png)

    ```console
    Message:
    Unable to start execution of step 2 (reason: Error authenticating proxy NODE1\repl_distribution, system error: The user name or password is incorrect.)
    ```

6. Questo errore indica che la password usata dall'agente di distribuzione non è corretta. Per risolvere questo errore:

    a. Espandere il nodo **Replica** in Esplora oggetti.
    
    b. Fare clic con il pulsante destro del mouse sulla sottoscrizione > **Proprietà**.
    
    c. Selezionare i puntini di sospensione (...) accanto ad **Account processo agente** e modificare la password.

    ![Selezioni per la modifica della password per l'agente di distribuzione](media/troubleshooting-tran-repl-errors/dist-agent-pw-change.png)

7. Controllare di nuovo Monitoraggio replica facendo clic con il pulsante destro del mouse su **Replica** in Esplora oggetti. Una X rossa in **Tutte le sottoscrizioni** indica che l'agente di distribuzione sta ancora riscontrando un errore. 

    Aprire la cronologia **Da database di distribuzione a Sottoscrittore** facendo clic con il pulsante destro del mouse sulla sottoscrizione in **Monitoraggio replica** > **Visualizza dettagli**. In questo caso, l'errore è diverso: 

    ![Errore che indica che l'agente di distribuzione non può connettersi](media/troubleshooting-tran-repl-errors/dist-agent-cant-connect.png)

    ```console
    Connecting to Subscriber 'NODE2\SQL2016'        
    Agent message code 20084. The process could not connect to Subscriber 'NODE2\SQL2016'.
    Number:  18456
    Message: Login failed for user 'NODE2\repl_distribution'.
    ```

8. Questo errore indica che l'agente di distribuzione non è in grado di connettersi al sottoscrittore, perché l'accesso non è riuscito per l'utente **NODE2\repl_distribution**. Per analizzare ulteriormente il problema, connettersi al sottoscrittore e aprire il log degli errori di SQL Server *corrente* nel nodo **Gestione** in Esplora oggetti: 

    ![Errore che indica che l'accesso non è riuscito per il sottoscrittore](media/troubleshooting-tran-repl-errors/login-failed.png)
    
    Se viene visualizzato questo errore, l'account di accesso risulta mancante nel sottoscrittore. Per correggere l'errore, vedere [Autorizzazioni per la replica](../../relational-databases/replication/security/security-role-requirements-for-replication.md).

9. Dopo aver risolto l'errore di accesso, controllare di nuovo Monitoraggio replica. Se tutti i problemi sono stati risolti, si noterà una freccia verde accanto al **nome della pubblicazione** e lo stato **In esecuzione** in **Tutte le sottoscrizioni**. 

    Fare clic con il pulsante destro del mouse sulla sottoscrizione per avviare di nuovo la cronologia **Dal database di distribuzione al Sottoscrittore** e verificare l'esito positivo. Se si tratta della prima esecuzione dell'agente di distribuzione, si noterà che è stata eseguita la copia bulk dello snapshot nel sottoscrittore: 

     ![Agente di distribuzione con stato "In esecuzione" e un messaggio sulla copia bulk](media/troubleshooting-tran-repl-errors/dist-agent-success.png)   


## <a name="enable-verbose-logging-on-any-agent"></a>Abilitare la registrazione dettagliata per qualsiasi agente
È possibile usare la registrazione dettagliata per visualizzare informazioni più dettagliate sugli errori che si verificano con qualsiasi agente nella topologia di replica. I passaggi sono gli stessi per ogni agente. È sufficiente assicurarsi di selezionare l'agente corretto in Monitoraggio attività processi. 

   >[!NOTE]   
   > Gli agenti possono essere nel server di pubblicazione o nel sottoscrittore, a seconda che si tratti di una sottoscrizione pull o push. Se non è possibile trovare l'agente che si sta cercando nel server in esame, controllare nell'altro server.  

1. Decidere dove si vuole salvare la registrazione dettagliata e verificare che la cartella esista. Questo esempio usa c:\temp. 
2. Espandere il nodo **SQL Server Agent** in Esplora oggetti e aprire Monitoraggio attività processi. 

    ![Comando "Visualizza attività processi" nel menu di scelta rapida per Monitoraggio attività processi](media/troubleshooting-tran-repl-errors/job-activity-monitor.png)    

1. Ordinare per **Categoria** e identificare l'agente di interesse. Questo esempio usa l'agente di lettura log. Fare clic con il pulsante destro del mouse sull'agente di interesse e scegliere **Proprietà**.

    ![Selezioni per l'apertura delle proprietà dell'agente](media/troubleshooting-tran-repl-errors/log-agent-properties.png)

1. Selezionare la pagina **Passaggi** ed evidenziare il passaggio **Esecuzione dell'agente**. Selezionare **Modifica**. 

    ![Selezioni per la modifica del passaggio "Esecuzione dell'agente"](media/troubleshooting-tran-repl-errors/edit-steps.png)

1. Nella casella **Comando** iniziare una nuova riga, immettere il testo seguente e selezionare **OK**: 

    ```console
    -Output C:\Temp\OUTPUTFILE.txt -Outputverboselevel 3
    ```

    È possibile modificare il percorso e il livello di dettaglio in base alle proprie preferenze.

    ![Output dettagliato nelle proprietà per il passaggio del processo](media/troubleshooting-tran-repl-errors/verbose.png)

   > [!NOTE]
   > Questi elementi potrebbero causare errori dell'agente o la mancanza del file di output, quando si aggiunge il parametro di output dettagliato:
   > - È presente un problema di formattazione a causa del quale il trattino diventa un segno meno. 
   > - Il percorso non esiste nel disco o l'account che esegue l'agente non ha le autorizzazioni per scrivere nel percorso specificato. 
   > - Manca uno spazio tra l'ultimo parametro e il parametro `-Output`. 
   > - Agenti diversi supportano diversi livelli di dettaglio. Se si abilita la registrazione dettagliata, ma l'agente non viene avviato, provare a diminuire il livello di dettaglio specificato di 1. 

1. Riavviare l'agente di lettura log facendo clic con il pulsante destro del mouse sull'agente > **Arresta processo al passaggio**. Aggiornare selezionando l'icona **Aggiorna** sulla barra degli strumenti. Fare clic con il pulsante destro del mouse sull'agente > **Inizia processo al passaggio**.
2. Esaminare l'output su disco. 

    ![File di testo di output](media/troubleshooting-tran-repl-errors/output.png)

    
1. Per disabilitare la registrazione dettagliata, seguire gli stessi passaggi precedenti per rimuovere l'intera riga `-Output` aggiunta in precedenza. 

Per altre informazioni, vedere [How to enable replication agents for logging to output files in SQL Server](https://support.microsoft.com/help/312292/how-to-enable-replication-agents-for-logging-to-output-files-in-sql-se) (Come abilitare gli agenti di replica per la registrazione in file di output in SQL Server). 


## <a name="see-also"></a>Vedere anche
<br>[Panoramica della replica transazionale](../../relational-databases/replication/transactional/transactional-replication.md)
<br>[Esercitazioni sulla replica](../../relational-databases/replication/replication-tutorials.md)
<br>[Blog di ReplTalk](https://blogs.msdn.microsoft.com/repltalk)

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]

