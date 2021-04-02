---
title: Risolvere i problemi di Launchpad per gli script Python e R
description: Questo articolo fornisce indicazioni per la risoluzione di molti problemi che impediscono l'avvio del servizio Launchpad di SQL Server, inclusi problemi di configurazione o modifiche o protocolli di rete mancanti. Il servizio Launchpad supporta l'esecuzione di script esterni per R e Python.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/31/2021
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: b015e4181f8d8cfe9388c609f749dc5979463d3a
ms.sourcegitcommit: 2db7412d30722f198cbafcd683bd4da206b33996
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106099960"
---
# <a name="troubleshoot-issues-with-launchpad-service-executing-python-and-r-scripts-in-sql-server-machine-learning-services"></a>Risolvere i problemi relativi al servizio Launchpad che eseguono script Python e R in SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo fornisce indicazioni per la risoluzione dei problemi che coinvolgono il [servizio launchpad di SQL Server](../security/sql-server-launchpad-service-account.md) usato con [Machine Learning Services](../sql-server-machine-learning-services.md). Il servizio Launchpad supporta l'esecuzione di script esterni per R e Python. Vari problemi possono impedire l'avvio di Launchpad, tra cui problemi o modifiche della configurazione o protocolli di rete mancanti.  

## <a name="determine-whether-launchpad-is-running"></a>Determinare se Launchpad è in esecuzione

1. Aprire il riquadro **Servizi** (Services. msc). In alternativa, nella riga di comando digitare **SQLServerManager13.msc** o **SQLServerManager14 msc** per aprire [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

2. Prendere nota dell'account del servizio in cui è in esecuzione Launchpad. Ogni istanza in cui è abilitato R o Python dovrebbe avere una propria istanza del servizio Launchpad. Il servizio per un'istanza denominata, ad esempio, potrebbe avere un nome simile a _MSSQLLaunchpad$NomeIstanza_.

3. Se il servizio è stato interrotto, riavviarlo. Al riavvio, se sono presenti problemi di configurazione viene pubblicato un messaggio nel registro eventi di sistema e il servizio viene arrestato nuovamente. Consultare il registro eventi di sistema per informazioni dettagliate sui motivi per cui il servizio è stato arrestato.

4. Esaminare il contenuto di RSetup.log e verificare che non siano presenti errori di installazione. Ad esempio, il messaggio *Uscita con codice 0* indica l'impossibilità di avviare il servizio.

5. Per cercare altri errori, esaminare il contenuto di rlauncher.log.

## <a name="check-the-launchpad-service-account"></a>Controllare l'account del servizio Launchpad

L'account del servizio predefinito potrebbe essere "NT Service\$SQL2016" o "NT Service\$SQL2017". La parte finale può variare in base al nome dell'istanza di SQL.

Il servizio Launchpad (Launchpad.exe) viene eseguito usando un account del servizio con privilegi limitati. Tuttavia, per avviare R e Python e comunicare con l'istanza di database, l'account del servizio Launchpad richiede i diritti utente seguenti:

- Accesso come servizio (SeServiceLogonRight)
- Sostituzione di token a livello di processo (SeAssignPrimaryTokenPrivilege)
- Ignorare controllo incrociato (SeChangeNotifyPrivilege)
- Regolazione limite risorse memoria per un processo (SeIncreaseQuotaSizePrivilege)

Per informazioni su questi diritti utente, vedere la sezione "Privilegi e diritti di Windows" in [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

> [!TIP]
> Se si ha familiarità con l'uso dello strumento SDP (Support Diagnostics Platform) per la diagnostica di SQL Server, è possibile usarlo per esaminare il file di output denominato MachineName_UserRights.txt.

## <a name="user-group-for-launchpad-cannot-log-on-locally"></a>Il gruppo utenti per Launchpad non può accedere localmente

Durante l'installazione di Machine Learning Services, SQL Server crea il gruppo di utenti di Windows **SQLRUserGroup** e assegna a questo gruppo tutti i diritti necessari per Launchpad per la connessione a SQL Server e l'esecuzione di processi di script esterni. Se questo gruppo di utenti è abilitato, viene usato anche per l'esecuzione di script Python.

Tuttavia, nelle organizzazioni in cui vengono applicati criteri di sicurezza più severi, i diritti richiesti da questo gruppo potrebbero essere stati rimossi manualmente o revocati automaticamente dai criteri. Se i diritti sono stati rimossi, Launchpad non può più connettersi a SQL Server e SQL Server non può chiamare il runtime esterno.

Per correggere il problema, assicurarsi che il gruppo **SQLRUserGroup** abbia il diritto di sistema **Consenti accesso locale**.

Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

## <a name="permissions-to-run-external-scripts"></a>Autorizzazioni per l'esecuzione di script esterni

Anche se Launchpad è configurato correttamente, restituisce un errore se l'utente non ha le autorizzazioni necessarie per eseguire script R o Python.

Se si è installato SQL Server come amministratore di database o se si è proprietari del database, l'autorizzazione viene concessa automaticamente. Gli altri utenti, tuttavia, in genere dispongono di autorizzazioni più limitate. Se provano a eseguire uno script R, ricevono un errore di Launchpad.

Per risolvere il problema, in SQL Server Management Studio un amministratore della sicurezza può modificare l'account di accesso SQL o l'account utente di Windows eseguendo lo script seguente:

```sql
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

Per altre informazioni, vedere [GRANT (Transact-SQL](../../t-sql/statements/grant-transact-sql.md).

## <a name="common-launchpad-errors"></a>Errori comuni di Launchpad

Questa sezione contiene un elenco dei messaggi di errore più comuni restituiti dal servizio Launchpad.

::: moniker range=">=sql-server-2016"
## <a name="unable-to-launch-runtime-for-r-script"></a>"Non è stato possibile avviare il runtime per lo script R"

Se il gruppo di Windows per gli utenti di R (usato anche per Python) non può accedere all'istanza che esegue R Services, potrebbero essere visualizzati gli errori seguenti:

- Errori generati quando si prova a eseguire script R:

    * *Non è stato possibile avviare il runtime per lo script 'R'. Verificare la configurazione del runtime di 'R'.*

    * *Si è verificato un errore dello script esterno. Non è stato possibile avviare il runtime.*

- Errori generati dal servizio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]:

    * *Failed to initialize the launcher RLauncher.dll*

    * *No launcher dlls were registered!*

    * *I registri di sicurezza indicano che non è stato possibile accedere con l'account NT SERVICE*

Per informazioni su come concedere le autorizzazioni necessarie a questo gruppo di utenti, vedere [Installare R Services per SQL Server](../install/sql-r-services-windows-install.md).

> [!NOTE]
> Questa limitazione non si applica se si usano account di accesso SQL per eseguire script R da una workstation remota.
::: moniker-end

## <a name="logon-failure-the-user-has-not-been-granted-the-requested-logon-type"></a>"Errore durante l'accesso. L'utente non ha ottenuto il tipo di accesso richiesto"

Per impostazione predefinita, all'avvio [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] usa l'account seguente: `NT Service\MSSQLLaunchpad`. L'account viene configurato dal programma di installazione di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] in modo da avere tutte le autorizzazioni necessarie.

Se si assegna a Launchpad un account diverso o il diritto viene rimosso da un criterio nel computer di SQL Server, è possibile che l'account non abbia le autorizzazioni necessarie e che venga visualizzato questo errore:

>*ERROR_LOGON_TYPE_NOT_GRANTED 1385 (0x569) Errore durante l'accesso. L'utente non ha ottenuto il tipo di accesso richiesto a questo computer.*

Per concedere le autorizzazioni necessarie al nuovo account del servizio, usare l'applicazione Criteri di sicurezza locali e aggiornare le autorizzazioni per l'account in modo da includere le seguenti:

+ Regolazione quote di memoria per un processo (SeIncreaseQuotaPrivilege)
+ Ignorare controllo incrociato (SeChangeNotifyPrivilege)
+ Accesso come servizio (SeServiceLogonRight)
+ Sostituzione di token a livello di processo (SeAssignPrimaryTokenPrivilege)

## <a name="unable-to-communicate-with-the-launchpad-service"></a>"SQL Server non è riuscito a comunicare con il servizio LaunchPad"

Se è stato installato e abilitato il Machine Learning, ma si riceve questo errore quando si prova a eseguire uno script R o Python, il servizio Launchpad per l'istanza potrebbe avere smesso di funzionare.

1. Dal prompt dei comandi di Windows aprire Gestione configurazione SQL Server. Per altre informazioni, vedere [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

2. Fare clic con il pulsante destro del mouse sul servizio Launchpad di SQL Server per l'istanza e quindi scegliere **Proprietà**.

3. Selezionare la scheda **Servizio** e verificare che il servizio sia in esecuzione. Se non è in esecuzione, modificare **Modalità di avvio** specificando **Automatica** e quindi selezionare **Applica**.

4. Il riavvio del servizio in genere corregge il problema, permettendo l'esecuzione di script di Machine Learning. Se il riavvio non risolve il problema, prendere nota del percorso e degli argomenti nella proprietà **Percorso binario**, quindi procedere come segue:

    a. Esaminare il file .config dell'utilità di avvio e verificare che la directory di lavoro sia valida.

    b. Verificare che il gruppo di Windows usato da Launchpad possa connettersi all'istanza di SQL Server.

    c. Se si apportano modifiche alle proprietà del servizio, riavviare Launchpad.

## <a name="fatal-error-creation-of-tmpfile-failed"></a>"Errore irreversibile: creazione di tmpFile non riuscita"

In questo scenario, le funzionalità di Machine Learning sono state installate correttamente e Launchpad è in esecuzione. Si prova a eseguire un semplice codice R o Python, ma Launchpad non riesce con un errore simile al seguente: 

>*Non è stato possibile comunicare con il runtime per lo script R. Verificare i requisiti del runtime di R.*

Allo stesso tempo, il runtime dello script esterno scrive il messaggio seguente come parte del messaggio STDERR: 

>*Errore irreversibile: creazione di tmpFile non riuscita*.

Questo errore indica che l'account che Launchpad sta tentando di usare non è autorizzato ad accedere al database. Questa situazione può verificarsi quando vengono implementati criteri di sicurezza molto restrittivi. Per determinare se questo è il caso, esaminare i log di SQL Server e verificare se all'account MSSQLSERVER01 è stato negato l'accesso. Le stesse informazioni sono disponibili nei log specifici di R\_SERVICES o di PYTHON\_SERVICES. Cercare ExtLaunchError.log.

Per impostazione predefinita, 20 account vengono configurati e associati al processo Launchpad.exe, i cui nomi vanno da MSSQLSERVER01 a MSSQLSERVER20. Se si fa un uso intenso di R o Python, è possibile aumentare il numero di account.

Per risolvere il problema, verificare che il gruppo disponga delle autorizzazioni *Consenti accesso locale* per l'istanza locale in cui sono state installate e abilitate le funzionalità di Machine Learning. In alcuni ambienti, questo livello di autorizzazione potrebbe richiedere un'eccezione per un oggetto Criteri di gruppo impostata dall'amministratore di rete.

## <a name="not-enough-quota-to-process-this-command"></a>"Non c'è abbastanza disponibilità per elaborare il comando"

Questo errore può avere diversi significati:

- Launchpad potrebbe non avere utenti esterni sufficienti per eseguire la query esterna. Se ad esempio si stanno eseguendo più di 20 query esterne simultaneamente e sono presenti solo 20 utenti predefiniti, una o più query potrebbero avere esito negativo.

- La memoria disponibile non è sufficiente per elaborare l'attività R. Questo errore si verifica in genere in un ambiente predefinito, in cui SQL Server potrebbe usare fino al 70% delle risorse del computer. Per informazioni su come modificare la configurazione del server in modo da supportare un uso superiore di risorse da parte di R, vedere [Operazionalizzazione del codice R](../r/operationalizing-your-r-code.md).

## <a name="cant-find-package"></a>"Impossibile trovare il pacchetto"

Se si esegue codice R in SQL Server e si riceve questo messaggio, ma non si riceve il messaggio eseguendo lo stesso codice all'esterno di SQL Server, significa che il pacchetto non è stato installato nel percorso di libreria predefinito usato da SQL Server.

L'errore può avere diverse cause:

- È stato installato un nuovo pacchetto nel server, ma l'accesso è stato negato, quindi R ha installato il pacchetto in una libreria utente.

- È stato installato R Services, quindi è stato installato un altro set di librerie o uno strumento R, tra cui Microsoft R Server (autonomo), Microsoft R Client, RStudio e così via.

Per determinare il percorso della libreria di pacchetti R usata dall'istanza, aprire SQL Server Management Studio (o qualsiasi altro strumento di query di database), connettersi all'istanza e quindi eseguire la stored procedure seguente:

```sql
EXEC sp_execute_external_script @language = N'R',  
@script = N' print(normalizePath(R.home())); print(.libPaths());'; 
```

#### <a name="sample-results"></a>Risultati dell'esempio

*Messaggi STDOUT dallo script esterno:*

*[1] "C:\\Programmi\\Microsoft SQL Server\\MSSQL13.SQL2016\\R_SERVICES"*

*[1] "C:/Programmi/Microsoft SQL Server/MSSQL13.SQL2016/R_SERVICES/library"*

Per risolvere il problema è necessario reinstallare il pacchetto nella libreria dell'istanza di SQL Server.

::: moniker range=">=sql-server-2016"
>[!NOTE]
>Se è stata aggiornata un'istanza di SQL Server 2016 per usare l'ultima versione di Microsoft R, il percorso predefinito della libreria è diverso. Per altre informazioni, vedere l'articolo su come [usare sqlBindR.exe per aggiornare un'istanza di R Services](../install/upgrade-r-and-python.md).
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="launchpad-shuts-down-due-to-mismatched-dlls"></a>Il servizio Launchpad viene arrestato a causa di una mancata corrispondenza fra DLL

Se si installa il motore di database con altre funzionalità, si applicano patch al server e successivamente si aggiunge la funzionalità Machine Learning usando il supporto originale, potrebbe essere installata la versione errata dei componenti di Machine Learning. Quando Launchpad rileva una mancata corrispondenza delle versioni, si arrestata e crea un file di dump.

Per evitare questo problema, assicurarsi di installare tutte le nuove funzionalità allo stesso livello di patch dell'istanza del server.

**Procedura sbagliata per eseguire l'aggiornamento:**

1. Installare SQL Server 2016 senza R Services.
2. Aggiornare SQL Server 2016 con l'aggiornamento cumulativo 2.
3. Installare R Services (In-Database) usando il supporto RTM.

**Procedura corretta per eseguire l'aggiornamento:**

1. Installare SQL Server 2016 senza R Services.
2. Aggiornare SQL Server 2016 al livello di patch desiderato. Ad esempio, installare il Service Pack 1 e quindi l'aggiornamento cumulativo 2.
3. Per aggiungere la funzionalità al livello di patch corretto, eseguire di nuovo il programma di installazione di SP1 e CU2, quindi scegliere R Services (in-database). 

## <a name="launchpad-fails-to-start-if-8dot3-notation-is-required"></a>Se è necessaria la notazione 8.3, l'avvio di Launchpad non riesce

> [!NOTE] 
> Nei sistemi precedenti, l'avvio di Launchpad può non riuscire se l'uso della notazione 8.3 è un requisito. Questo requisito è stato rimosso nelle versioni successive. I clienti di R Services per SQL Server 2016 devono installare uno degli elementi seguenti:
> * SQL Server 2016 SP1 e CU1: [Aggiornamento cumulativo 1 per SQL Server](https://support.microsoft.com/help/3208177/cumulative-update-1-for-sql-server-2016-sp1).
> * SQL Server 2016 RTM, aggiornamento cumulativo 3 e questo [hotfix](https://support.microsoft.com/help/3210110/on-demand-hotfix-update-package-for-sql-server-2016-cu3), disponibile su richiesta.

Per la compatibilità con R, R Services per SQL Server 2016 (In-Database) richiede che l'unità in cui è installata la funzionalità supporti la creazione di nomi file brevi usando la *notazione 8.3*. Un nome file 8.3 è definito anche *nome file breve* e viene usato per la compatibilità con le versioni precedenti di Microsoft Windows o come alternativa ai nomi file lunghi.

Se il volume in cui si installa R non supporta i nomi file brevi, è possibile che i processi che avviano R da SQL Server non siano in grado di individuare il file eseguibile corretto e di conseguenza Launchpad non verrà avviato.

Come soluzione alternativa, è possibile abilitare la notazione 8.3 nel volume in cui sono installati SQL Server e R Services. È quindi necessario specificare il nome breve per la directory di lavoro nel file di configurazione di R Services.

1. Per abilitare la notazione 8.3, eseguire l'utilità fsutil con l'argomento *8dot3name* come descritto qui: [fsutil 8dot3name](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff621566(v=ws.11)).

2. Dopo aver abilitato la notazione 8.3, aprire il file RLauncher.config e prendere nota della proprietà di `WORKING_DIRECTORY`. Per informazioni su come trovare questo file, vedere [Raccolta di dati per la risoluzione dei problemi di Machine Learning](data-collection-ml-troubleshooting-process.md).

3. Usare l'utilità fsutil con l'argomento *file* per specificare un percorso di file breve per la cartella specificata in WORKING_DIRECTORY.

4. Modificare il file di configurazione per specificare la stessa directory di lavoro immessa nella proprietà WORKING_DIRECTORY. In alternativa, è possibile specificare una directory di lavoro diversa e scegliere un percorso esistente già compatibile con la notazione 8.3.
::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

[Raccolta di dati per la risoluzione dei problemi di Machine Learning](data-collection-ml-troubleshooting-process.md)

[Installare Machine Learning Services per SQL Server](../install/sql-machine-learning-services-windows-install.md)

[Risolvere i problemi di connessione al motore di database](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)