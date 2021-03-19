---
title: Eseguire l'installazione in Windows
description: Informazioni su come installare Machine Learning Services per SQL Server in Windows. È possibile usare Machine Learning Services per eseguire gli script Python e R nel database.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/17/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: d209daa0932068b41974c7e3597980b15657b1b2
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611211"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-windows"></a>Installare SQL Server Machine Learning Services (Python e R) in Windows

[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Informazioni su come installare Machine Learning Services per SQL Server in Windows. È possibile usare Machine Learning Services per eseguire gli script Python e R nel database.

## <a name="pre-install-checklist"></a><a name="bkmk_prereqs"> </a> Elenco di controllo preliminare all'installazione

+ È necessaria un'istanza del motore di database. Non è possibile installare solo le funzionalità di Python o R, ma è possibile aggiungerle in modo incrementale a un'istanza esistente.

+ Per assicurare la continuità aziendale, per Machine Learning Services è disponibile il supporto di [Gruppi di disponibilità AlwaysOn](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Installare Machine Learning Services e configurare i pacchetti in ogni nodo.

+ L'installazione di Machine Learning Services *non è supportata* in un'[istanza di cluster di failover Always On](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) in SQL Server 2017. È supportata con SQL Server 2019 e versioni successive.
 
+ Non installare Machine Learning Services in un controller di dominio. La parte del programma di installazione relativa a Machine Learning Services avrà esito negativo.

+ Non installare **Funzionalità condivise** > **Machine Learning Server (Standalone)** nello stesso computer su cui è in esecuzione un'istanza nel database. Un server autonomo entrerebbe in competizione per le stesse risorse, riducendo le prestazioni di entrambe le installazioni.

+ L'installazione side-by-side con altre versioni di Python e R è supportata ma sconsigliata. Questa installazione è supportata perché l'istanza di SQL Server usa le proprie copie delle distribuzioni R e Anaconda open source. È sconsigliata perché l'esecuzione di codice che usa Python e R nel computer SQL Server al di fuori di SQL Server può causare vari problemi:
    
  + L'uso di una libreria e di file eseguibili diversi creerà risultati incoerenti rispetto a quelli eseguiti in SQL Server.
  + Gli script R e Python in esecuzione in librerie esterne non possono essere gestiti da SQL Server, con conseguenti conflitti di risorse.

::: moniker range=">=sql-server-ver15"
> [!NOTE]
> Machine Learning Services viene installato per impostazione predefinita nei **cluster Big Data di SQL Server**. Non è necessario seguire la procedura descritta in questo articolo se si usa un **cluster Big Data**. Per altre informazioni, vedere [Usare Machine Learning Services (Python e R) in cluster Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

> [!IMPORTANT]
> Al termine dell'installazione, assicurarsi di completare i passaggi di post-configurazione descritti in questo articolo, tra cui l'abilitazione di SQL Server per l'uso di script esterni e l'aggiunta degli account necessari per consentire a SQL Server di eseguire i processi R e Python per conto dell'utente. Per completare le modifiche alla configurazione è in genere necessario riavviare l'istanza o il servizio Launchpad.

## <a name="get-the-installation-media"></a>Ottenere il supporto di installazione

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range="=sql-server-2017"
Per altre informazioni su quali edizioni di SQL Server supportano l'integrazione di Python e R con Machine Learning Services, vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
::: moniker-end

::: moniker range="=sql-server-ver15"
Per altre informazioni su quali edizioni di SQL Server supportano l'integrazione di Python e R con Machine Learning Services, vedere [Edizioni e funzionalità supportate di SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).
::: moniker-end

## <a name="run-setup"></a>Eseguire l'installazione.

Per le installazioni locali, è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura ed esecuzione relative a tale condivisione.

1. Avviare l'Installazione guidata di SQL Server.
  
1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

   ::: moniker range="=sql-server-2017"
   ![Nuova installazione autonoma di SQL Server](media/2017setup-installation-page-mlsvcs.png)
   ::: moniker-end

   ::: moniker range="=sql-server-ver15"
   ![Nuova installazione autonoma di SQL Server](media/2019setup-installation-page-mlsvcs.png)
   ::: moniker-end

1. Nella pagina **Selezione funzionalità** selezionare queste opzioni:

   ::: moniker range="=sql-server-2017"

   - **Servizi motore di database**
     
     Per usare R e Python con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza predefinita oppure un'istanza denominata.

   - **Machine Learning Services (In-Database)**
     
     Questa opzione consente di installare i servizi di database che supportano l'esecuzione di script R e Python.

   ::: moniker-end

   ::: moniker range="=sql-server-ver15"

   - **Servizi motore di database**
     
     Per usare R o Python con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza predefinita oppure un'istanza denominata.

   - **Machine Learning Services (In-Database)**
     
     Questa opzione consente di installare i servizi di database che supportano l'esecuzione di script R e Python.

   ::: moniker-end

   - **R**
     
     Selezionare questa opzione per aggiungere i pacchetti Microsoft R, l'interprete e la distribuzione di R open source. 
     
   - **Python**
     
     Selezionare questa opzione per aggiungere i pacchetti Microsoft Python, il file eseguibile di Python 3.5 e alcune librerie dalla distribuzione Anaconda.
     
   ::: moniker range="=sql-server-ver15"
   Per informazioni sull'installazione e l'uso di Java, vedere [Installare le estensioni del linguaggio SQL Server in Windows](../../language-extensions/install/windows-java.md).
   ::: moniker-end
   
   ::: moniker range="=sql-server-2017"
   ![Opzioni delle funzionalità per R e Python](media/2017setup-features-page-mls-rpy.PNG "Opzioni di installazione per R e Python")
   ::: moniker-end
   
   ::: moniker range="=sql-server-ver15"
   ![Opzioni delle funzionalità per R e Python](media/2019setup-features-page-mls-rpy.png "Opzioni di installazione per R e Python")
   ::: moniker-end
   
   > [!NOTE]
   > 
   > Non selezionare l'opzione per **Machine Learning Server (Standalone)** . L'opzione di installazione di Machine Learning Server inclusa in **Funzionalità condivise** è destinata all'uso in un computer separato.

::: moniker range="=sql-server-2017"

4. Nella pagina **Consenso per installare Microsoft R Open** selezionare **Accetta** e quindi **Avanti**. 

Il contratto di licenza copre:
+ Microsoft R Open
+ Pacchetti e strumenti di base R Open Source
+ Pacchetti R avanzati e provider di connettività del team di sviluppo Microsoft.

1. Nella pagina **Consenso per installare Python** selezionare **Accetta** e quindi **Avanti**. Il contratto di licenza open source per Python disciplina anche l'uso di Anaconda e degli strumenti correlati, oltre ad alcune nuove librerie Python del team di sviluppo Microsoft.

   > [!NOTE]
   >  Se il computer in uso non ha accesso a Internet, è possibile sospendere l'installazione a questo punto e scaricare i programmi di installazione separatamente. Per altre informazioni, vedere [Installare i componenti di Machine Learning in computer senza accesso a Internet](../install/sql-ml-component-install-without-internet-access.md).

1. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.
  
   + Servizi motore di database
   + Machine Learning Services (In-Database)
   + R o Python oppure entrambi

   Si noti la posizione della cartella nel percorso `..\Setup Bootstrap\Log` in cui sono archiviati i file di configurazione. Al termine dell'installazione, è possibile esaminare i componenti installati nel file Summary.

1. Dopo che l'installazione è completata, riavviare il computer, se richiesto. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

::: moniker range="=sql-server-ver15"

1. Nella pagina **Consenso per installare Microsoft R Open** selezionare **Accetta** e quindi **Avanti**. Questo contratto di licenza disciplina l'uso di Microsoft R Open, che include una distribuzione di pacchetti e strumenti R open source di base, insieme a pacchetti R avanzati e a provider di connettività del team di sviluppo Microsoft.

2. Nella pagina **Consenso per installare Python** selezionare **Accetta** e quindi **Avanti**. Il contratto di licenza open source per Python disciplina anche l'uso di Anaconda e degli strumenti correlati, oltre ad alcune nuove librerie Python del team di sviluppo Microsoft.

3. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.
  
   + Servizi motore di database
   + Machine Learning Services (In-Database)
   + R e/o Python

   Si noti la posizione della cartella nel percorso `..\Setup Bootstrap\Log` in cui sono archiviati i file di configurazione. Al termine dell'installazione, è possibile esaminare i componenti installati nel file Summary.

4. Dopo che l'installazione è completata, riavviare il computer, se richiesto. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

## <a name="set-environment-variables"></a>Impostare le variabili di ambiente

Per integrare solo le funzionalità di R, è necessario impostare la variabile di ambiente **MKL_CBWR** per avere la certezza di [ottenere un output coerente](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) dai calcoli di Intel Math Kernel Library (MKL).

1. Nel Pannello di controllo fare clic su **Sistema e sicurezza** > **Sistema** > **Impostazioni di sistema avanzate** > **Variabili d'ambiente**.

2. Creare una nuova variabile dell'utente o di sistema. 

   + Impostare `MKL_CBWR` come nome della variabile
   + Impostare `AUTO` come valore della variabile

Per questo passaggio è necessario riavviare il server. Se si intende abilitare l'esecuzione di script, è possibile sospendere il riavvio fino a quando non sono state completate tutte le operazioni di configurazione.

<a name="bkmk_enableFeature"></a>

## <a name="enable-script-execution"></a>Abilitare l'esecuzione di script

1. Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

    > [!TIP]
    > È possibile scaricare e installare la versione appropriata da questa pagina: [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
    > 
    > È anche possibile usare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), che supporta l'esecuzione di attività amministrative e query in SQL Server.
  
2. Connettersi all'istanza in cui è stato installato Machine Learning Services, fare clic su **Nuova query** per aprire una finestra di query ed eseguire il comando seguente:

    ```sql
    sp_configure
    ```

    A questo punto, il valore della proprietà `external scripts enabled` deve essere **0**. La funzionalità è disattivata per impostazione predefinita. Prima di poter eseguire script R o Python, la funzionalità deve essere abilitata in modo esplicito da un amministratore.
    
3.  Per abilitare la funzionalità per script esterni, eseguire l'istruzione seguente:
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    Se questa funzionalità è già stata abilitata per il linguaggio R, non ripetere la configurazione per Python. La piattaforma di estendibilità sottostante supporta entrambi i linguaggi.

## <a name="restart-the-service"></a>Riavviare il servizio.

Al termine dell'installazione riavviare il motore di database.

Il riavvio del servizio ha l'effetto di riavviare automaticamente anche il servizio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] correlato.

Per riavviare il servizio è possibile eseguire il comando **Riavvia** del menu di scelta rapida per l'istanza in SSMS oppure usare il pannello **Servizi** del Pannello di controllo o [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="verify-installation"></a>Verificare l'installazione

Usare la procedura seguente per verificare che tutti i componenti usati per avviare lo script esterno siano in esecuzione.

1. In SQL Server Management Studio aprire una nuova finestra di query ed eseguire il comando seguente:
    
   ```sql
   EXECUTE sp_configure  'external scripts enabled'
   ```

   **run_value** è impostato su 1.
    
2. Aprire il pannello **Servizi** o Gestione configurazione SQL Server e verificare che il **servizio Launchpad di SQL Server** sia in esecuzione. Dovrebbe essere in esecuzione un solo servizio per ogni istanza del motore di database in cui è installato R o Python. Per altre informazioni sul servizio, vedere [Framework di estendibilità](../concepts/extensibility-framework.md). 
   
3. Se Launchpad è in esecuzione, è possibile eseguire script Python e R semplici per verificare che i runtime di script esterni possano comunicare con SQL Server.

   Aprire una nuova finestra **Query** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi eseguire uno script come:
   + Per R
   
     ```sql
     EXEC sp_execute_external_script  @language =N'R',
     @script=N'
     OutputDataSet <- InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
     
   + Per Python
     
     ```sql
     EXEC sp_execute_external_script  @language =N'Python',
     @script=N'
     OutputDataSet = InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
   
   **Risultati**

   La prima volta che viene caricato il runtime di uno script esterno, l'esecuzione dello script può richiedere un po' di tempo. I risultati saranno simili ai seguenti:

   | hello |
   |----|
   | 1|

> [!NOTE]
> Le colonne o le intestazioni usate nello script Python non vengono restituite automaticamente. Per aggiungere nomi di colonna per l'output, è necessario specificare lo schema per il set di dati restituito. A tale scopo, usare il parametro WITH RESULTS della stored procedure, assegnando un nome alle colonne e specificando il tipo di dati SQL.
>
> È ad esempio possibile aggiungere la riga seguente per generare un nome di colonna arbitrario: `WITH RESULT SETS ((Col1 AS int))`

::: moniker range=">=sql-server-2017"

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Applicare gli aggiornamenti

### <a name="existing-installation"></a>Installazione esistente

Se è stato aggiunto Machine Learning Services a un'istanza di SQL Server esistente e in precedenza è stato applicato un aggiornamento cumulativo (CU), le versioni del motore di database e della funzionalità Machine Learning Services potrebbero essere diverse. Questo può causare un comportamento imprevisto o errori. 

Attenersi alla seguente procedura per portare il Machine Learning Services alla stessa versione del motore di database.

1. Determinare l'aggiornamento cumulativo (CU) per il motore di database. Eseguire questa istruzione T-SQL:

   ```sql
   SELECT @@VERSION
   ```
 
   Di seguito è riportato un esempio di output di SQL Server 2019 aggiornamento cumulativo (CU) 8:
 
   *Microsoft SQL Server 2019 (RTM-CU8-GDR) (KB4583459)-15.0.4083.2 (x64) Nov 2 2020 18:35:09 Copyright (C) 2019 Microsoft Corporation Developer Edition (64-bit) in Windows 10 Enterprise 10,0 <X64> (Build 19042:) (hypervisor)*

   Per ulteriori informazioni, vedere [determinare la versione, l'edizione e il livello di aggiornamento delle SQL Server e dei relativi componenti](/troubleshoot/sql/general/determine-version-edition-update-level#machine-learning-services).

1. Scaricare l' [aggiornamento cumulativo (Cu)](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) installato per il motore di database.

1. Eseguire l'installazione dell'aggiornamento cumulativo (CU) e seguire le istruzioni per installare il CU per Machine Learning Services.

### <a name="new-installation"></a>Nuova installazione

Se si installa Machine Learning Services con una nuova installazione di SQL Server motore di database, è consigliabile applicare l'aggiornamento cumulativo più recente ai componenti del motore di database e di machine learning.

Nei dispositivi connessi a Internet, gli aggiornamenti cumulativi vengono in genere applicati tramite Windows Update, ma è anche possibile usare la procedura seguente per eseguire aggiornamenti controllati. Quando si applica l'aggiornamento per il motore di database, il programma di installazione esegue il pull degli aggiornamenti cumulativi per le funzionalità di Python o R installate nella stessa istanza. 

I server che non sono connessi a Internet richiedono alcuni passaggi aggiuntivi. Per altre informazioni, vedere [Installare in computer senza accesso a Internet > Applicare aggiornamenti cumulativi](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Inizia con un'istanza di base già installata: SQL Server versione iniziale.

2. Passare all'elenco di aggiornamenti cumulativi: [aggiornamenti più recenti per Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

3. Selezionare l'aggiornamento cumulativo più recente. L'eseguibile viene scaricato ed estratto automaticamente.

4. Eseguire il programma di installazione. Accettare le condizioni di licenza e nella pagina Selezione funzionalità esaminare le funzionalità per le quali vengono applicati gli aggiornamenti cumulativi. Dovrebbero essere visualizzate tutte le funzionalità installate per l'istanza corrente, incluse quelle di Machine Learning. Il programma di installazione scarica i file CAB necessari per aggiornare tutte le funzionalità.

   ![Riepilogo delle funzionalità installate](media/cumulative-update-feature-selection.png)

5. Continuare con la procedura guidata, accettando le condizioni di licenza per le distribuzioni R e Python. 

::: moniker-end

## <a name="additional-configuration"></a>Configurazione aggiuntiva

Se la verifica degli script esterni ha esito positivo, è possibile eseguire comandi R o Python da SQL Server Management Studio, Visual Studio Code o qualsiasi altro client che può inviare istruzioni T-SQL al server.

Se durante l'esecuzione del comando è stato restituito un errore, rivedere i passaggi di configurazione aggiuntivi in questa sezione. Potrebbe essere necessario eseguire specifiche configurazioni aggiuntive per il servizio o il database.

A livello di istanza, la configurazione aggiuntiva può includere:

* [Configurare il firewall per SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Abilitare protocolli di rete aggiuntivi](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Abilitare connessioni remote](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Creare un account di accesso per SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)
* [Gestire le quote disco](/windows/desktop/fileio/managing-disk-quotas) per evitare che gli script esterni eseguano attività che esauriscono lo spazio su disco

::: moniker range=">=sql-server-ver15"
In SQL Server 2019 su Windows è stato modificato il meccanismo di isolamento. Questo meccanismo ha effetto su **SQLRUserGroup**, sulle regole del firewall, sulle autorizzazioni per i file e sull'autenticazione implicita. Per altre informazioni, vedere [Modifiche all'isolamento per Machine Learning Services](sql-server-machine-learning-services-2019.md).
::: moniker-end

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

Nel database potrebbero essere necessari gli aggiornamenti di configurazione seguenti:

* [Concedere agli utenti l'autorizzazione per SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)

> [!NOTE]
> La necessità di una configurazione aggiuntiva dipende dallo schema di sicurezza, dal percorso in cui è stato installato SQL Server e dalla modalità presumibilmente adottata dagli utenti per connettersi al database ed eseguire script esterni.

## <a name="suggested-optimizations"></a>Ottimizzazioni suggerite

Ora che tutto il sistema funziona, può essere necessario ottimizzare il server per supportare le funzionalità di Machine Learning o installare un modello di Machine Learning già sottoposto a un training preliminare.

::: moniker range="=sql-server-2017"

### <a name="add-more-worker-accounts"></a>Aggiungere altri account di lavoro

Se si prevede che molti utenti eseguano script contemporaneamente, è possibile aumentare il numero di account di lavoro assegnati al servizio Launchpad. Per altre informazioni, vedere [Ridimensionare l'esecuzione simultanea di script esterni in SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).
::: moniker-end

### <a name="optimize-the-server-for-script-execution"></a>Ottimizzare il server per l'esecuzione di script

Le impostazioni predefinite per il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consentono di ottimizzare il bilanciamento del server per un'ampia gamma di servizi supportati dal motore di database, che possono includere processi di estrazione, trasformazione e caricamento (ETL), servizi di controllo e creazione di report e applicazioni che usano dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Con le impostazioni predefinite, alle risorse per il Machine Learning vengono applicate talvolta restrizioni o limitazioni, in particolare nel caso di operazioni che richiedono molta memoria.

Per assicurarsi che i processi di Machine Learning siano considerati come prioritari e dispongano delle risorse appropriate, è consigliabile usare la funzionalità Resource Governor di SQL Server per configurare un pool di risorse esterne. Può inoltre essere opportuno modificare la quantità di memoria allocata al motore di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aumentare il numero di account eseguiti con il servizio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Per configurare un pool di risorse per la gestione delle risorse esterne, vedere [Creare un pool di risorse esterne](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Per modificare la quantità di memoria riservata per il database, vedere [Opzioni di configurazione server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Per modificare il numero di account R che possono essere avviati da [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], vedere [Ridimensionare l'esecuzione simultanea di script esterni in SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Se si usa l'edizione Standard e non è disponibile Resource Governor, per gestire le risorse del server è possibile usare le viste a gestione dinamica (DMV) e gli eventi estesi, oltre che il monitoraggio eventi di Windows.

### <a name="install-additional-python-and-r-packages"></a>Installare pacchetti Python e R aggiuntivi

Le soluzioni Python e R create per SQL Server possono chiamare funzioni di base, funzioni dei pacchetti proprietari installati con SQL Server e pacchetti di terze parti compatibili con la versione di R e Python open source installata da SQL Server.

I pacchetti di SQL Server da usare devono essere installati nella libreria predefinita usata dall'istanza. Se nel computer si usa un'installazione separata di Python o R oppure se i pacchetti sono stati installati nelle librerie utente, non è possibile usare tali pacchetti da T-SQL.

Per installare e gestire pacchetti aggiuntivi, è possibile configurare gruppi di utenti per condividere i pacchetti a livello di singolo database oppure configurare ruoli di database per consentire agli utenti di installare i propri pacchetti. Per altre informazioni, vedere [Installare pacchetti Python](../package-management/install-additional-python-packages-on-sql-server.md) e [Installare nuovi pacchetti R](../package-management/install-additional-r-packages-on-sql-server.md).

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori Python possono apprendere come usare Python con SQL Server seguendo queste esercitazioni:

+ [Esercitazione su Python: Stimare il noleggio di sci con la regressione lineare in Machine Learning Services per SQL Server](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Esercitazione su Python: Categorizzazione dei clienti tramite clustering K-Means con Machine Learning Services per SQL Server](../tutorials/python-clustering-model.md)

Gli sviluppatori R possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di R con SQL Server. Per il passaggio successivo, vedere i collegamenti seguenti:

+ [Avvio rapido: Eseguire R in T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Esercitazione: Analisi nel database per sviluppatori R](../tutorials/r-taxi-classification-introduction.md)