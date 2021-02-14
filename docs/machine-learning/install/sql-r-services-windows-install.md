---
title: Installare R Services per SQL Server 2016
titleSuffix: ''
description: Informazioni su come installare R Services per SQL Server 2016 in Windows. È possibile usare R Services per eseguire script R all'interno del database.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: =sql-server-2016
ms.openlocfilehash: 56efa5151e4d72019cb126c69e3d3138829dbbd2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082452"
---
# <a name="install-sql-server-2016-r-services"></a>Installare R Services per SQL Server 2016

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

Informazioni su come installare R Services per SQL Server 2016 in Windows. È possibile usare R Services per eseguire script R all'interno del database.

> [!NOTE]
> In SQL Server 2017 e versioni successive R è incluso in [Machine Learning Services](../sql-server-machine-learning-services.md) insieme a Python. Se si vuole R e si dispone di SQL Server 2017 o versione successiva, vedere [Installare Machine Learning Services di SQL Server](sql-machine-learning-services-windows-install.md) per aggiungere la funzionalità.

<a name="bkmk_prereqs"></a>

## <a name="pre-install-checklist"></a>Elenco di controllo preliminare all'installazione

+ È necessaria un'istanza del motore di database. Non è possibile installare solo R, ma è possibile aggiungerlo in modo incrementale a un'istanza esistente.

+ Per assicurare la continuità aziendale, per R Services è disponibile il supporto di [Gruppi di disponibilità AlwaysOn](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). È necessario installare R Services e configurare i pacchetti in ogni nodo.

+ Non installare R Services in un'istanza del cluster di failover Always On di SQL Server. Il meccanismo di sicurezza usato per isolare i processi R non è compatibile con un ambiente con istanza del cluster di failover Always On di SQL Server.

+ Non installare R Services in un controller di dominio. La parte del programma di installazione relativa a R Services avrà esito negativo.

+ Non installare **Funzionalità condivise** > **R Server (Standalone)** nello stesso computer in cui è in esecuzione un'istanza di tipo In-Database.

+ L'installazione side-by-side con altre versioni di R è supportata ma non consigliata. È supportata perché l'istanza di SQL Server usa le proprie copie della distribuzione R open source. Tuttavia, l'esecuzione di codice che usa R nel computer SQL Server al di fuori di SQL Server può causare diversi problemi:

  + Si usano una libreria e un file eseguibile diverso e quindi si ottengono risultati diversi rispetto all'esecuzione del codice in SQL Server.
  + Gli script R in esecuzione nelle librerie esterne non possono essere gestiti da SQL Server, con conseguente contesa di risorse.
  
> [!IMPORTANT]
> Al termine dell'installazione, assicurarsi di completare i passaggi aggiuntivi di post-configurazione descritti in questo articolo, tra cui l'abilitazione di SQL Server per l'uso di script esterni e l'aggiunta degli account necessari per consentire a SQL Server di eseguire i processi R per conto dell'utente. Per completare le modifiche alla configurazione è in genere necessario riavviare l'istanza o riavviare il servizio Launchpad.

## <a name="get-the-installation-media"></a>Ottenere il supporto di installazione

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

<a name="bkmk_ga_instalpatch"></a>

### <a name="install-patch-requirement"></a>Requisito di installazione di patch

Microsoft ha rilevato un problema con una versione specifica dei file binari di Microsoft VC++ 2013 Runtime installati come prerequisito da SQL Server. Se l'aggiornamento dei file binari di VC++ Runtime non viene installato, potrebbero verificarsi problemi di stabilità di SQL Server in determinati scenari. Prima di installare SQL Server, seguire le istruzioni in [Note sulla versione di SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) per vedere se il computer richiede una patch per i file binari di VC++ Runtime.  

<a name="bkmk2016top"></a>

## <a name="run-setup"></a>Eseguire l'installazione.

Per le installazioni locali è necessario eseguire il programma di installazione come amministratore. Se si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da una condivisione remota, è necessario utilizzare un account di dominio con autorizzazioni di lettura ed esecuzione relative a tale condivisione.

1. Avviare l'Installazione guidata di SQL Server 2016.

1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

    ![Installare R Services (In-Database)](media/2016-setup-installation-rsvcs.png "Avviare l'installazione del motore di database con R Services")

1. Nella pagina **Selezione funzionalità** selezionare le opzioni seguenti:

    + Selezionare **Servizi motore di database**. Il motore di database è necessario in ogni istanza che usa funzionalità di Machine Learning.
    + Selezionare **R Services (In-Database)** . Questa opzione consente di installare il supporto per l'uso nel database di R.

    ![Selezione della funzionalità R Services](media/2016setup-rsvcs-features.png "Selezionare queste funzionalità per R Services In-Database")

    > [!IMPORTANT]
    > Non installare contemporaneamente R Server e R Services. 

1. Nella pagina **Consenso per installare Microsoft R Open** fare clic su **Accetto**.
  
    Questo contratto di licenza deve essere accettato per il download di Microsoft R Open, che include una distribuzione di pacchetti e strumenti R open source di base, insieme a pacchetti R avanzati e a provider di connettività del team Microsoft dedicato allo sviluppo di soluzioni R.
  
1. Dopo l'accettazione del contratto di licenza, si verifica una breve pausa di preparazione del programma di installazione. Quando il pulsante diventa disponibile, fare clic su **Avanti**.

1. Nella pagina **Inizio installazione** verificare che siano incluse le opzioni seguenti e selezionare **Installa**.

    + Servizi motore di database
    + R Services (In-Database)

1. Dopo che l'installazione è completata, riavviare il computer, se richiesto. È importante leggere il messaggio visualizzato nell'Installazione guidata al termine dell'installazione. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="set-environment-variables"></a>Impostare le variabili di ambiente

Per integrare solo le funzionalità di R, è necessario impostare la variabile di ambiente **MKL_CBWR** per avere la certezza di [ottenere un output coerente](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) dai calcoli di Intel Math Kernel Library (MKL).

1. Nel Pannello di controllo fare clic su **Sistema e sicurezza** > **Sistema** > **Impostazioni di sistema avanzate** > **Variabili d'ambiente**.

1. Creare una nuova variabile dell'utente o di sistema.

    + Impostare `MKL_CBWR` come nome della variabile
    + Impostare `AUTO` come valore della variabile

Per questo passaggio è necessario riavviare il server. È possibile rimandare il riavvio fino a quando non sono state completate tutte le operazioni di configurazione.

<a name="bkmk_enableFeature"></a>

##  <a name="enable-script-execution"></a>Abilitare l'esecuzione di script

1. Aprire [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

1. Connettersi all'istanza in cui è stato installato R Services, fare clic su **Nuova query** per aprire una finestra di query ed eseguire il comando seguente:

   ```sql
   sp_configure
   ```

    A questo punto, il valore della proprietà `external scripts enabled` deve essere **0**. Questo perché la funzionalità è disattivata per impostazione predefinita. Per poter eseguire script R, la funzionalità deve essere abilitata in modo esplicito da un amministratore.

1. Per abilitare la funzionalità per script esterni, eseguire l'istruzione seguente:
  
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
  
## <a name="restart-the-service"></a>Riavviare il servizio.

Al termine dell'installazione, riavviare il motore di database prima di continuare con il passaggio successivo, abilitando l'esecuzione degli script.

Il riavvio del servizio ha l'effetto di riavviare automaticamente anche il servizio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] correlato.

È possibile riavviare il servizio facendo clic con il pulsante destro del mouse sul comando **Riavvia** per l'istanza in SSMS o usando [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="verify-installation"></a>Verificare l'installazione

Usare la procedura seguente per verificare che tutti i componenti usati per avviare lo script esterno siano in esecuzione.

1. In SQL Server Management Studio aprire una nuova finestra di query ed eseguire il comando seguente:

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    Il valore di **run_value** dovrebbe ora essere impostato su 1.

1. Aprire Gestione configurazione SQL Server e verificare che il **servizio Launchpad di SQL Server** sia in esecuzione. Sarà presente un servizio per ogni istanza del motore di database in cui è installato R. Per altre informazioni sul servizio, vedere [Framework di estendibilità](../concepts/extensibility-framework.md).

1. Se Launchpad è in esecuzione, dovrebbe essere possibile eseguire codice R semplice per verificare che i runtime di script esterni possano comunicare con SQL Server.

    Aprire una nuova finestra **Query** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o Azure Data Studio e quindi eseguire uno script come il seguente:

    ```sql
    EXEC sp_execute_external_script  @language =N'R',
    @script=N'
    OutputDataSet <- InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    La prima volta che viene caricato il runtime di uno script esterno, l'esecuzione dello script può richiedere un po' di tempo. I risultati saranno simili ai seguenti:

    | hello |
    |----|
    | 1|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Applicare gli aggiornamenti

È consigliabile applicare il Service Pack e l'aggiornamento cumulativo più recenti sia al motore di database sia ai componenti di Machine Learning.

Nei dispositivi connessi a Internet, gli aggiornamenti cumulativi vengono in genere applicati tramite Windows Update, ma è anche possibile usare la procedura seguente per eseguire aggiornamenti controllati. Quando si applica l'aggiornamento per il motore di database, il programma di installazione esegue il pull degli aggiornamenti cumulativi per le librerie R installate nella stessa istanza. 

Nei server che non sono connessi a Internet sono necessari alcuni passaggi aggiuntivi. Per altre informazioni, vedere [Installare in computer senza accesso a Internet > Applicare aggiornamenti cumulativi](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Iniziare con un'istanza di base già installata: la versione iniziale di SQL Server 2016 oppure SQL Server 2016 SP 1 o SQL Server 2016 SP 2.

1. Passare all'elenco degli aggiornamenti cumulativi: [Aggiornamenti più recenti per Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

1. Selezionare le versioni più recenti del Service Pack (se non è già installato come istanza di base) e dell'aggiornamento cumulativo. L'eseguibile viene scaricato ed estratto automaticamente.

1. Eseguire il programma di installazione. Accettare le condizioni di licenza e nella pagina Selezione funzionalità esaminare le funzionalità per le quali vengono applicati gli aggiornamenti cumulativi. Dovrebbero essere visualizzate tutte le funzionalità installate per l'istanza corrente, incluso R Services. Il programma di installazione scarica i file CAB necessari per aggiornare tutte le funzionalità.

1. Continuare con la procedura guidata, accettando le condizioni di licenza per la distribuzione R.

> [!NOTE]
> L'aggiornamento cumulativo 14 e le versioni successive per SQL Server 2016 SP2 includono una versione più recente del runtime di R. Per altre informazioni, vedere [Modificare la versione predefinita di runtime del linguaggio](change-default-language-runtime-version.md).

<a name="bkmk_FollowUp"></a>

## <a name="additional-configuration"></a>Configurazione aggiuntiva

Se il passaggio di verifica degli script esterni ha avuto esito positivo, è possibile eseguire comandi R da SQL Server Management Studio, Azure Data Studio o qualsiasi altro client che può inviare istruzioni T-SQL al server.

Se durante l'esecuzione del comando è stato restituito un errore, rivedere i passaggi di configurazione aggiuntivi in questa sezione. Potrebbe essere necessario eseguire specifiche configurazioni aggiuntive per il servizio o il database.

A livello di istanza, la configurazione aggiuntiva può includere:

* [Configurazione del firewall per Machine Learning Services per SQL Server](../../machine-learning/security/firewall-configuration.md).
* [Abilitare protocolli di rete aggiuntivi](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).
* [Abilitare connessioni remote](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md).
* [Gestire le quote disco](/windows/desktop/fileio/managing-disk-quotas) per evitare che gli script esterni eseguano attività che esauriscono lo spazio su disco.

<a name="bkmk_configureAccounts"></a>
<a name="bkmk_AllowLogon"></a>

Nel database potrebbero essere necessari gli aggiornamenti di configurazione seguenti:

* [Concedere agli utenti l'autorizzazione per SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)
* [Aggiungere SQLRUserGroup come utente del database](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

> [!NOTE]
> Non tutte le modifiche elencate sono obbligatorie ed è anche possibile che nessuna di queste lo sia. I requisiti dipendono dallo schema di sicurezza, dal percorso in cui è stato installato SQL Server e dalla modalità presumibilmente adottata dagli utenti per connettersi al database ed eseguire script esterni. Altre informazioni aggiuntive sull'installazione sono disponibili qui: [Installare Machine Learning Services per SQL Server](../install/sql-machine-learning-services-windows-install.md)

## <a name="suggested-optimizations"></a>Ottimizzazioni suggerite

È anche possibile ottimizzare il server per supportare le funzionalità di Machine Learning con R o installare modelli già sottoposti a training.

### <a name="add-more-worker-accounts"></a>Aggiungere altri account di lavoro

Se si pensa di usare R in modo intensivo o si prevede che molti utenti eseguano script contemporaneamente, è possibile aumentare il numero di account di lavoro assegnati al servizio Launchpad. Per altre informazioni, vedere [Ridimensionare l'esecuzione simultanea di script esterni in SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

<a name="bkmk_optimize"></a>

### <a name="optimize-the-server-for-external-script-execution"></a>Ottimizzare il server per l'esecuzione di script esterni

Le impostazioni predefinite per il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consentono di ottimizzare il bilanciamento del server per un'ampia gamma di servizi supportati dal motore di database, che possono includere processi di estrazione, trasformazione e caricamento (ETL), servizi di controllo e creazione di report e applicazioni che usano dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di conseguenza, con le impostazioni predefinite, è possibile che alle risorse per il Machine Learning siano applicate restrizioni o limitazioni, in particolare nel caso di operazioni che richiedono molta memoria.

Per assicurarsi che i processi di Machine Learning siano considerati come prioritari e dispongano delle risorse appropriate, è consigliabile usare la funzionalità Resource Governor di SQL Server per configurare un pool di risorse esterne. Può inoltre essere opportuno modificare la quantità di memoria allocata al motore di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aumentare il numero di account eseguiti con il servizio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Per configurare un pool di risorse per la gestione delle risorse esterne, vedere [Creare un pool di risorse esterne](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Per modificare la quantità di memoria riservata per il database, vedere [Opzioni di configurazione server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Per modificare il numero di account R che possono essere avviati da [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], vedere [Ridimensionare l'esecuzione simultanea di script esterni in SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Se si usa l'edizione Standard e non è disponibile Resource Governor, per gestire le risorse del server usate da R è possibile usare le viste a gestione dinamica (DMV) e gli eventi estesi, oltre al monitoraggio eventi di Windows.

### <a name="install-additional-r-packages"></a>Installare pacchetti R aggiuntivi

Le soluzioni R create per SQL Server possono chiamare funzioni R di base, funzioni dei pacchetti proprietari installati con SQL Server e pacchetti R di terze parti compatibili con la versione di R open source installata da SQL Server.

I pacchetti di SQL Server da usare devono essere installati nella libreria predefinita usata dall'istanza. Se si ha un'installazione separata di R nel computer oppure se i pacchetti sono stati installati nelle librerie utente, non sarà possibile usare tali pacchetti da T-SQL.

Il processo di installazione e gestione dei pacchetti R in SQL Server 2016 è diverso da quello in SQL Server 2017. In SQL Server 2016, un amministratore di database deve installare i pacchetti R necessari agli utenti. In SQL Server 2017, è possibile configurare gruppi di utenti per condividere i pacchetti a livello di singolo database oppure configurare ruoli di database per consentire agli utenti di installare i propri pacchetti. Per altre informazioni, vedere [Installare i pacchetti con gli strumenti R](../package-management/install-r-packages-standard-tools.md).

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori R possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di R con SQL Server. Per il passaggio successivo, vedere i collegamenti seguenti:

+ [Avvio rapido: Eseguire R in T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Esercitazioni di R per Machine Learning in SQL](../tutorials/r-tutorials.md)
+ [Documentazione per Machine Learning in SQL](../index.yml)