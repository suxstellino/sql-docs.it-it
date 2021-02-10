---
title: Acquisire una traccia per gli aggiornamenti di SQL Server
description: Utilizzare Database Experimentation Assistant (DEA) per creare un file di traccia con un log di eventi del server acquisiti.
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 9bc1dafd5242dc4be8a9a2947465ed48a01fcb8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042421"
---
# <a name="capture-a-trace-in-database-experimentation-assistant"></a>Acquisire una traccia in Database Experimentation Assistant

È possibile utilizzare Database Experimentation Assistant (DEA) per creare un file di traccia con un log di eventi del server acquisiti. Un evento del server acquisito è un evento che si verifica in un server specifico durante un periodo di tempo specifico. Un'acquisizione di traccia deve essere eseguita una sola volta per ogni server.

Prima di avviare un'acquisizione di traccia, assicurarsi di eseguire il backup di tutti i database di destinazione.

La memorizzazione nella cache delle query in SQL Server potrebbe influire sui risultati della valutazione. È consigliabile riavviare il servizio SQL Server (MSSQLSERVER) nell'applicazione Servizi per migliorare la coerenza dei risultati della valutazione.

## <a name="configure-a-trace-capture"></a>Configurare un'acquisizione di traccia

1. In DEA, sulla barra di spostamento a sinistra, selezionare l'icona della fotocamera, quindi nella pagina **tutte le acquisizioni** selezionare **nuova acquisizione**.

    ![Creare un'acquisizione in DEA](./media/database-experimentation-assistant-capture-trace/dea-initiate-capture.png)

2. Nella pagina **nuova acquisizione** , in **Dettagli acquisizione**, immettere o selezionare le informazioni seguenti:

    - **Nome acquisizione**: immettere un nome per il file di traccia per l'acquisizione.
    - **Format**: specificare il formato (Trace o XEvent) per l'acquisizione.
    - **Durata**: selezionare l'intervallo di tempo (in minuti) in cui si desidera che l'acquisizione di traccia venga eseguita.
    - **Percorso di acquisizione**: selezionare il percorso di destinazione per il file di traccia.

        > [!NOTE]
        > Il percorso del file di traccia deve trovarsi nel computer in cui è in esecuzione SQL Server. Se il servizio SQL Server non è impostato per un account specifico, potrebbe essere necessario che il servizio disponga delle autorizzazioni di scrittura per la cartella specificata per il file di traccia da scrivere.

3. Verificare di aver effettuato un backup selezionando **Sì, ho effettuato manualmente il backup...** .

4. In **Dettagli acquisizione** immettere o selezionare le informazioni seguenti:

    - **Tipo di server**: specificare il tipo di SQL Server (**SqlServer**, **AzureSqlDb**, **AzureSqlManagedInstance**).
    - **Nome server**: specificare il nome del server o l'indirizzo IP del SQL Server.
    - **Tipo di autenticazione**: per il tipo di autenticazione selezionare **Windows**.
    - **Nome database**: immettere un nome per un database in cui avviare una traccia del database. Se non si specifica un database, la traccia viene acquisita in tutti i database del server.

5. Selezionare o deselezionare le caselle di controllo **Crittografa la connessione** e il **certificato del server attendibile** in base allo scenario.

    ![Pagina nuova acquisizione](./media/database-experimentation-assistant-capture-trace/dea-new-capture.png)

## <a name="start-the-trace-capture"></a>Avviare l'acquisizione della traccia

1. Dopo aver immesso o selezionato le informazioni necessarie, selezionare **Avvia** per avviare l'acquisizione della traccia.

    Se le informazioni immesse sono valide, viene avviato il processo di acquisizione della traccia. In caso contrario, le caselle di testo con voci non valide vengono evidenziate in rosso. Se si verificano errori, correggere le voci necessarie, quindi selezionare di nuovo **Avvia** .

    Mentre l'acquisizione della traccia è in esecuzione, in **Dettagli acquisizione** vengono visualizzati lo stato e l'avanzamento del processo di acquisizione della traccia.

    ![Stato acquisizione monitoraggio](./media/database-experimentation-assistant-capture-trace/dea-capture-running.png)

2. Al termine dell'esecuzione dell'acquisizione della traccia, il nuovo file di traccia (. trc) viene salvato nel **percorso di acquisizione** specifico durante la configurazione iniziale.

    ![Acquisizione traccia completata](./media/database-experimentation-assistant-capture-trace/dea-capture-complete.png)

    Il file di traccia include i risultati della traccia dell'attività di un database di SQL Server. i file con estensione TRC sono progettati per fornire ulteriori informazioni sugli errori rilevati e segnalati da SQL Server.

## <a name="frequently-asked-questions-about-trace-capture"></a>Domande frequenti sull'acquisizione di tracce

Di seguito sono riportate alcune domande frequenti sull'acquisizione di tracce in DEA.

**D: quali eventi vengono acquisiti quando si esegue un'acquisizione di traccia in un database di produzione?**

La tabella seguente elenca gli eventi e i dati di colonna corrispondenti raccolti da DEA per le tracce:
  
|Nome evento|Dati di testo (1)|Dati binari (2)|ID database (3)|Nome host (8)|Nome applicazione (10)|Nome account di accesso (11)|SPID (12)|Ora di inizio (14)|Ora di fine (15)|Nome database (35)|Sequenza di eventi (51)|Sistema (60)|  
|---|---|---|---|---|---|---|---|---|---|---|---|---|  
|**RPC: completato (10)**||*|*|*|*|*|*|*|*|*|*|*|  
|**RPC: avvio (11)**||*|*|*|*|*|*|*||*|*|*|  
|**Parametro di output RPC (100)**|*||*|*|*|*|*|*||*|*|*|  
|**SQL: BatchCompleted (12)**|*||*|*|*|*|*|*|*|*|*|*|  
|**SQL: BatchStarting (13)**|*||*|*|*|*|*|*||*|*|*|  
|**Controlla account di accesso (14)**|*|*|*|*|*|*|*|*||*|*|*|  
|**Controllo disconnessione (15)**|*||*|*|*|*|*|*|*|*|*|*|  
|**ExistingConnection (17)**|*|*|*|*|*|*|*|*||*|*|*|  
|**CursorOpen (53)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorPrepare (70)**|*||*|*|*|*|*|*||*|*|*|  
|**Preparare SQL (71)**|||*|*|*|*|*|*||*|*|*|  
|**SQL preparato per exec (72)**|||*|*|*|*|*|*||*|*|*|  
|**CursorExecute (74)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorUnprepare (77)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorClose (78)**|*||*|*|*|*|*|*||*|*|*|  

**D: esiste un effetto sulle prestazioni del server di produzione quando l'acquisizione della traccia è in esecuzione?**

Sì, si verifica un effetto minimo sulle prestazioni durante la raccolta delle tracce. Nei test è stato rilevato un numero eccessivo di richieste di memoria al 3%.

**D: quali tipi di autorizzazioni sono necessari per l'acquisizione di tracce su un carico di lavoro di produzione?**

- L'utente di Windows che esegue l'operazione di traccia nell'applicazione DEA deve disporre dei diritti di amministratore di sistema nel computer in cui è in esecuzione SQL Server.
- L'account di servizio utilizzato nel computer che esegue SQL Server deve disporre dell'accesso in scrittura al percorso del file di traccia specificato.

**D: è possibile acquisire le tracce per l'intero server o solo per un singolo database?**

È possibile utilizzare DEA per acquisire tracce per tutti i database nel server o per un singolo database.

**D: è stato configurato un server collegato nell'ambiente di produzione. Queste query vengono visualizzate nelle tracce?**

Se si sta eseguendo un'acquisizione di traccia per l'intero server, la traccia acquisisce tutte le query, incluse le query del server collegato. Per eseguire un'acquisizione di traccia per l'intero server, lasciare vuota la casella **nome database** in **nuova acquisizione** .

**D: qual è il tempo minimo consigliato per le tracce del carico di lavoro di produzione?**

Si consiglia di scegliere un'ora che meglio rappresenti l'intero carico di lavoro. In questo modo, l'analisi viene eseguita su tutte le query del carico di lavoro.

**D: quanto è importante eseguire un backup del database prima di avviare un'acquisizione di traccia?**

Prima di avviare un'acquisizione di traccia, assicurarsi di eseguire il backup di tutti i database di destinazione. La traccia acquisita in target 1 e target 2 viene rieseguita. Se lo stato del database non è lo stesso, i risultati della sperimentazione sono sbilanciati.

**D: è possibile raccogliere XEvent anziché tracce ed è possibile riprodurre XEvent?**

Sì. DEA supporta XEvent. Scaricare la versione più recente di DEA e assegnarle un tentativo.

## <a name="troubleshoot-trace-captures"></a>Risolvere i problemi relativi alle acquisizioni di traccia

Se viene visualizzato un errore durante l'esecuzione di un'acquisizione di traccia, verificare che:

- Il nome del computer in cui è in esecuzione SQL Server è valido. Per confermare, provare a connettersi al computer che esegue SQL Server usando SQL Server Management Studio (SSMS).
- La configurazione del firewall non blocca le connessioni al computer in cui è in esecuzione SQL Server.
- L'utente dispone delle autorizzazioni elencate nelle [domande frequenti sulla riproduzione](./database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay).
- Il nome della traccia non segue la convenzione di rollover standard (Capture \_ 1). In alternativa, provare i nomi di traccia come Capture \_ 1a o Capture1.

Di seguito sono riportati alcuni possibili errori che possono essere visualizzati e le soluzioni per risolverli:

|Possibili errori|Soluzione|  
|---|---|  
|Impossibile avviare la traccia sul SQL Server di destinazione. verificare se si dispone delle autorizzazioni necessarie e che l'account di SQL Server disponga dell'accesso in scrittura al percorso del file di traccia specificato codice errore SQL (53)|L'utente che esegue lo strumento DEA deve disporre dell'accesso al computer in cui è in esecuzione SQL Server. All'utente deve essere assegnato il ruolo sysadmin.|  
|Impossibile avviare la traccia sul SQL Server di destinazione. verificare se si dispone delle autorizzazioni necessarie e che l'account di SQL Server disponga dell'accesso in scrittura al percorso del file di traccia specificato codice errore SQL (19062)|Il percorso di traccia specificato potrebbe non esistere o la cartella non dispone delle autorizzazioni di scrittura per l'account con cui vengono eseguiti SQL Server servizi, ad esempio servizio di rete. Il percorso deve esistere e deve disporre delle autorizzazioni necessarie per l'avvio della traccia.|  
|Una traccia DEA è attualmente in esecuzione nel server di destinazione.|Una traccia attiva è già in esecuzione nel server di destinazione. Non è possibile avviare una nuova traccia quando è già in esecuzione una traccia a livello di server.|  
|Impossibile aprire il database richiesto per l'acquisizione della traccia. Questo errore potrebbe essere causato da un nome di database errato.|Il database specificato non esiste o non è accessibile all'utente corrente. Usare il nome del database corretto.|  

Se vengono visualizzati altri errori con l'etichetta *codice errore SQL*, vedere [motore di database errori](../relational-databases/errors-events/database-engine-events-and-errors.md) per descrizioni dettagliate.

## <a name="see-also"></a>Vedi anche

- Per informazioni su come configurare gli strumenti di Riesecuzione distribuita in SQL Server prima di riprodurre una traccia acquisita, vedere [configurare riesecuzione distribuita per database Experimentation Assistant](database-experimentation-assistant-configure-replay.md).