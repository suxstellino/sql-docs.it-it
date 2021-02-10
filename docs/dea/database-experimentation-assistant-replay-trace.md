---
title: Riprodurre una traccia per gli aggiornamenti di SQL Server
description: Informazioni su come riprodurre una traccia acquisita con Database Experimentation Assistant per SQL Server aggiornamenti.
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
ms.openlocfilehash: 105f471afc10e03c8d6371aa22cce1f6e74ca8c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065916"
---
# <a name="replay-a-trace-in-database-experimentation-assistant"></a>Riprodurre una traccia in Database Experimentation Assistant

In Database Experimentation Assistant (DEA) è possibile riprodurre un file di traccia acquisito su un ambiente di testing aggiornato. Si consideri, ad esempio, un carico di lavoro di produzione in esecuzione in SQL Server 2008 R2. Il file di traccia del carico di lavoro deve essere riprodotto due volte: una volta in un ambiente con la stessa versione di SQL Server eseguita in produzione e una seconda volta in un ambiente in cui è presente la versione di destinazione dell'aggiornamento SQL Server, ad esempio SQL Server 2016.

> [!NOTE]
> Per riprodurre una traccia, è necessario configurare manualmente le macchine virtuali o i computer fisici per l'esecuzione di Riesecuzione distribuita tracce. Per ulteriori informazioni, vedere [Configure riesecuzione distribuita for database Experimentation Assistant](database-experimentation-assistant-configure-replay.md).
>

## <a name="configure-a-trace-replay-for-target-1"></a>Configurare una riproduzione della traccia per la destinazione 1

In primo luogo, è necessario eseguire una riproduzione della traccia sulla destinazione 1, che rappresenta l'ambiente di produzione esistente.

1. In DEA, sulla barra di spostamento a sinistra, selezionare l'icona della freccia, quindi nella pagina **tutte le riproduzione** selezionare **nuova riproduzione**.

    ![Creare una riproduzione in DEA](./media/database-experimentation-assistant-replay-trace/dea-create-replay.png)

    > [!NOTE]
    > Il computer del controller di Riesecuzione distribuita richiede le autorizzazioni per l'account utente usato per la connessione remota.

2. Nella pagina **nuova riproduzione** , in **Dettagli riproduzione**, immettere o selezionare le seguenti informazioni:

    - **Nome riproduzione**: immettere un nome per la riproduzione della traccia.
    - **Formato traccia di origine**: specificare il formato (Trace o XEvent) del file di traccia di origine.
    - **Percorso completo del file di origine**: specificare il percorso completo del file di traccia di origine. Se si usa DReplay, il file deve esistere nel computer che funge da controller DReplay e l'account utente richiede l'accesso al file e alla cartella.
    - **Strumento di riproduzione**: specificare lo strumento di riproduzione (DReplay o built).
    - **Nome computer controller**: specificare il nome del computer che funge da controller di riesecuzione distribuita.
    - **Percorso traccia riproduzione**: specificare il percorso in cui archiviare i file di traccia/XEvent associati alla riproduzione della traccia.

        > [!NOTE]
        > Per un database SQL di Azure o un Istanza gestita SQL di Azure, è necessario specificare l'URI di firma di accesso condiviso dell'account di archiviazione BLOB di Azure.

3. Verificare che il database sia stato ripristinato selezionando la casella di controllo **Sì, ho ripristinato manualmente i database** .

4. In **SQL Server Dettagli connessione** immettere o selezionare le informazioni seguenti:

    - **Tipo di server**: specificare il tipo di SQL Server (**SqlServer**, **AzureSqlDb**, **AzureSqlManagedInstance**).
    - **Nome server**: specificare il nome del server o l'indirizzo IP del SQL Server.
    - **Tipo di autenticazione**: per il tipo di autenticazione selezionare **Windows**.
    - **Nome database**: immettere un nome per un database in cui avviare una traccia sul lato server. Se non si specifica un database, la traccia viene acquisita in tutti i database del server.

5. Selezionare o deselezionare le caselle di controllo **Crittografa la connessione** e il **certificato del server attendibile** in base allo scenario.

    ![Nuova pagina di riproduzione](./media/database-experimentation-assistant-replay-trace/dea-new-replay.png)

## <a name="start-the-trace-replay-on-target-1"></a>Avviare la riproduzione della traccia nella destinazione 1

- Dopo aver immesso o selezionato le informazioni necessarie, selezionare **Avvia** per avviare la riproduzione della traccia.

  Se le informazioni immesse sono valide, viene avviato il processo Riesecuzione distribuita. In caso contrario, le caselle di testo contenenti informazioni non corrette vengono evidenziate con il rosso. Verificare che i valori immessi siano corretti, quindi selezionare **Avvia**.

  ![Riproduzione dello stato di avanzamento rispetto alla destinazione 1](./media/database-experimentation-assistant-replay-trace/dea-run-replay-target1.png)

  È possibile monitorare il processo nel modo necessario. Al termine dell'esecuzione della riproduzione, DEA archivia i risultati in un file nel percorso specificato.

  ![Riproduzione a fronte di destinazione 1 completata](./media/database-experimentation-assistant-replay-trace/dea-replay-target1-complete.png)

## <a name="perform-the-trace-replay-against-target-2"></a>Esegui la riproduzione della traccia sulla destinazione 2

Al termine dell'esecuzione della riproduzione della traccia sulla destinazione 1, è necessario eseguire la stessa operazione sulla seconda destinazione, che rappresenta l'ambiente di aggiornamento previsto.

1. Configurare una riproduzione della traccia, questa volta usando i dettagli associati all'ambiente di destinazione 2.
2. Avviare la riproduzione della traccia nella destinazione 2.

   È possibile monitorare il processo nel modo necessario. Al termine dell'esecuzione della riproduzione, DEA archivia i risultati in un file nel percorso specificato.

## <a name="frequently-asked-questions-about-trace-replay"></a>Domande frequenti sulla riproduzione di tracce

**D: quali autorizzazioni di sicurezza sono necessarie per avviare un'acquisizione di riproduzione nel server di destinazione?**

- L'utente di Windows che esegue l'operazione di traccia nell'applicazione DEA deve disporre dei diritti di amministratore di sistema nel computer di destinazione che esegue SQL Server. Questi diritti utente sono necessari per avviare una traccia.
- L'account di servizio in cui è in esecuzione SQL Server il computer di destinazione deve disporre dell'accesso in scrittura al percorso del file di traccia specificato.
- L'account di servizio in cui sono in esecuzione i servizi client di Riesecuzione distribuita deve disporre dei diritti utente per connettersi al computer di destinazione che esegue SQL Server e per eseguire le query.

**D: è possibile avviare più di una riproduzione nella stessa sessione?**

Sì, è possibile avviare più Riproduci e tenerne traccia fino al completamento nella stessa sessione.

**D: è possibile avviare più di una riproduzione in parallelo?**

Sì, ma non con lo stesso set di computer selezionato nei **client controller Plus**. Il controller e i client saranno occupati. Configurare un set di computer separato in **controller Plus client** per avviare una riproduzione parallela.

**D: quanto tempo è necessario in genere per completare una riproduzione?**

Una riproduzione richiede in genere la stessa quantità di tempo della traccia di origine oltre alla quantità di tempo necessaria per la pre-elaborazione della traccia di origine. Tuttavia, se i computer client registrati con il controller non sono sufficienti per gestire il carico prodotto dalla riproduzione, il completamento della riproduzione potrebbe richiedere più tempo. È possibile registrare fino a 16 computer client con il controller.

**D: qual è la dimensione dei file di traccia di destinazione?**

I file di traccia di destinazione possono essere compresi tra 5 e 15 volte le dimensioni della traccia di origine. Le dimensioni del file sono basate sul numero di query eseguite. Ad esempio, i BLOB del piano di query possono essere di grandi dimensioni. Se le statistiche per queste query cambiano spesso, vengono acquisiti più eventi.

**D: perché è necessario ripristinare I database?**

SQL Server è un sistema di gestione di database relazionali con stato. Per eseguire correttamente un test A/B, è necessario mantenere sempre lo stato del database. In caso contrario, è possibile che vengano visualizzati errori nelle query durante la riproduzione che non verranno visualizzati nell'ambiente di produzione. Per evitare questi errori, è consigliabile eseguire un backup immediatamente prima dell'acquisizione dell'origine. Analogamente, è necessario ripristinare il backup nel computer di destinazione che esegue SQL Server per evitare errori durante la riproduzione.

**D: cosa significa "Pass%" nella pagina di riproduzione?**

Il **passaggio%** indica che è stata passata solo una percentuale di query. È possibile diagnosticare se il numero di errori è previsto. Gli errori potrebbero essere previsti oppure gli errori potrebbero verificarsi perché il database ha perso l'integrità. Se il valore di **pass%** non è quello previsto, è possibile arrestare la traccia ed esaminare il file di traccia in SQL Profiler per vedere quali query non sono state eseguite correttamente.

**D: come è possibile esaminare gli eventi di traccia raccolti durante la riproduzione?**

Aprire un file di traccia di destinazione e visualizzarlo in SQL Profiler. In alternativa, se si desidera apportare modifiche all'acquisizione di riproduzione, tutti gli script di SQL Server sono disponibili in C: \\ programmi (x86) \\ Microsoft Corporation \\ database Experimentation Assistant \\ Scripts \\ StartReplayCapture. SQL.

**D: quali eventi di traccia vengono raccolti da DEA durante la riproduzione?**

DEA acquisisce gli eventi di traccia che contengono informazioni relative alle prestazioni. La configurazione di acquisizione si trova nello script StartReplayCaptureTrace. SQL. Questi eventi sono tipici SQL Server eventi di traccia elencati nella documentazione di [riferimento di sp_trace_setevent (Transact-SQL)](../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).

## <a name="troubleshoot-trace-replay"></a>Risoluzione dei problemi di riproduzione della traccia

**D: perché non è possibile connettersi al computer in cui è in esecuzione SQL Server?**

- Verificare che il nome del computer in cui è in esecuzione SQL Server sia valido. Per confermare, provare a connettersi al server usando SQL Server Management Studio (SSMS).
- Verificare che la configurazione del firewall non blocchi le connessioni al computer in cui è in esecuzione SQL Server.
- Verificare che l'utente disponga dei diritti utente necessari.
- Verificare che l'account del servizio Riesecuzione distribuita client disponga dell'accesso al computer in cui è in esecuzione SQL Server.

È possibile ottenere altri dettagli nei log in% temp% \\ dea. Se il problema persiste, contattare il team del prodotto.

**D: perché non è possibile connettersi al controller di Riesecuzione distribuita?**

- Verificare che il servizio Riesecuzione distribuita controller sia in esecuzione nel computer del controller. Per verificare, usare gli strumenti di gestione di Riesecuzione distribuita (eseguire il comando `dreplay.exe status -f 1` ).
- Se la riproduzione viene avviata in modalità remota:
  - Verificare che il computer che esegue DEA possa eseguire correttamente il ping del controller. Verificare che le impostazioni del firewall consentano le connessioni in base alle istruzioni nella pagina **Configura ambiente di riproduzione** . Per ulteriori informazioni, vedere l'articolo [SQL Server riesecuzione distribuita](../tools/distributed-replay/sql-server-distributed-replay.md).
  - Assicurarsi che l'avvio remoto e l'attivazione remota DCOM siano consentiti per l'utente del controller di Riesecuzione distribuita.
  - Verificare che i diritti utente di accesso remoto DCOM siano consentiti per l'utente del controller di Riesecuzione distribuita.

**D: il percorso del file di traccia è presente nel computer. Perché non è possibile trovare Riesecuzione distribuita controller?**

Riesecuzione distribuita possibile accedere solo alle risorse del disco locale. È necessario copiare i file di traccia di origine nel computer del controller di Riesecuzione distribuita prima di avviare la riproduzione. Inoltre, è necessario specificare il percorso nella nuova pagina di **riproduzione** di dea.

I percorsi UNC non sono compatibili con Riesecuzione distribuita. Riesecuzione distribuita percorsi devono essere percorsi locali assoluti del primo file di traccia di origine, inclusa l'estensione.

**D: perché non è possibile cercare i file nella nuova pagina di riproduzione?**

Poiché non è possibile esplorare le cartelle in un computer remoto, l'esplorazione dei file non è utile. È più efficiente copiare e incollare i percorsi assoluti.

**D: è stata avviata la riproduzione con una traccia ma Riesecuzione distribuita non ha riprodotto alcun evento. Perché?**

Questo problema può verificarsi perché il file di traccia non dispone degli eventi riproducibili oppure contiene informazioni su come riprodurre gli eventi. Verificare che il percorso del file di traccia fornito punti a un file di traccia di origine. Il file di traccia di origine viene creato usando la configurazione fornita nello script StartCaptureTrace. SQL.

**D: si è verificato un errore imprevisto. quando si tenta di pre-elaborare i file di traccia utilizzando il controller SQL Server 2017 Riesecuzione distribuita. Perché?**

Questo problema è noto nella versione RTM di SQL Server 2017. Per ulteriori informazioni, vedere [errore imprevisto quando si utilizza la funzionalità DReplay per riprodurre una traccia acquisita in SQL Server 2017](https://support.microsoft.com/help/4045678/fix-unexpected-error-when-you-use-the-dreplay-feature-to-replay-a).  
  
Il problema è stato risolto nell'aggiornamento cumulativo 1 più recente per SQL Server 2017. Scaricare la versione più recente dell' [aggiornamento cumulativo 1 per SQL Server 2017](https://support.microsoft.com/help/4038634/cumulative-update-1-for-sql-server-2017).

## <a name="see-also"></a>Vedi anche

- Per creare un report di analisi che consenta di ottenere informazioni dettagliate sulle modifiche proposte, vedere [creare report](database-experimentation-assistant-create-report.md).
