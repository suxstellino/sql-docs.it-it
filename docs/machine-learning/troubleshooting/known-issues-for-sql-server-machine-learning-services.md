---
title: Problemi noti per Python e R
description: Questo articolo descrive i problemi noti o le limitazioni per i componenti Python e R forniti in Machine Learning Services per SQL Server e R Services per SQL Server 2016.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/31/2021
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3d81fea3cb2dcd7937dd752cf0fdfee899c1f94c
ms.sourcegitcommit: 2db7412d30722f198cbafcd683bd4da206b33996
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106099982"
---
# <a name="known-issues-for-python-and-r-in-sql-server-machine-learning-services"></a>Problemi noti per Python e R in SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo descrive i problemi noti o le limitazioni per i componenti Python e R forniti in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) e [R Services per SQL Server 2016](../r/sql-server-r-services.md).

## <a name="setup-and-configuration-issues"></a>Problemi di installazione e configurazione

Per una descrizione dei processi correlati all'installazione e alla configurazione iniziali, vedere [Installare Machine Learning Services di SQL Server ](../install/sql-machine-learning-services-windows-install.md). Contiene informazioni sugli aggiornamenti, l'installazione side-by-side e l'installazione di nuovi componenti R o Python.

### <a name="1-inconsistent-results-in-mkl-computations-due-to-missing-environment-variable"></a>1. Risultati incoerenti nei calcoli MKL a causa di una variabile di ambiente mancante

**Si applica a:** File binari di R_SERVER 9.0, 9.1, 9.2 o 9.3.

R_SERVER usa la libreria MKL (Math Kernel Library) di Intel. Per i calcoli che coinvolgono la libreria MKL, è possibile che si verifichino risultati incoerenti se nel sistema manca una variabile di ambiente. 

Impostare la variabile di ambiente `'MKL_CBWR'=AUTO` per garantire la riproducibilità numerica condizionale in R_SERVER. Per altre informazioni, vedere [Introduction to Conditional Numerical Reproducibility (CNR)](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) (Introduzione alla riproducibilità numerica condizionale).

**Soluzione alternativa**

1. Nel Pannello di controllo fare clic su **Sistema e sicurezza** > **Sistema** > **Impostazioni di sistema avanzate** > **Variabili d'ambiente**.

2. Creare una nuova variabile dell'utente o di sistema. 

   + Impostare il nome della variabile su 'MKL_CBWR'.
   + Impostare 'Valore' per la variabile su 'AUTO'.

3. Riavviare R_SERVER. In SQL Server è possibile riavviare il servizio Launchpad di SQL Server.

> [!NOTE]
> Se si esegue SQL Server 2019 in Linux, modificare o creare *.bash_profile* nella home directory dell'utente, aggiungendo la riga `export MKL_CBWR="AUTO"`. Eseguire questo file digitando `source .bash_profile` al prompt dei comandi di Bash. Riavviare R_SERVER digitando `Sys.getenv()` al prompt dei comandi di R.

### <a name="2-r-script-runtime-error-sql-server-2017-cu5-cu7-regression"></a>2. Errore di runtime per script R (regressione SQL Server 2017 CU5-CU7)

Negli aggiornamenti cumulativi da 5 a 7 di SQL Server 2017 esiste una regressione nel file **rlauncher.config** in cui il percorso della directory temporanea include uno spazio. Questa regressione è stata corretta nell'aggiornamento cumulativo 8.

L'errore che verrà visualizzato quando si esegue lo script R include i messaggi seguenti:

> *Non è stato possibile comunicare con il runtime per lo script 'R'. Verificare i requisiti del runtime di 'R'.*
>
> Messaggi STDERR dallo script esterno: 
>
> *Errore irreversibile: non è possibile creare 'R_TempDir'*

**Soluzione alternativa**

Applicare l'aggiornamento cumulativo 8 quando diventerà disponibile. In alternativa, è possibile ricreare **rlauncher.config** eseguendo **registerrext** con uninstall/install in un prompt dei comandi con privilegi elevati. 

```cmd
<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /uninstall /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>

<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /install /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>
```

L'esempio seguente mostra i comandi con l'istanza predefinita "MSSQL14.MSSQLSERVER" installata in "C:\Programmi\Microsoft SQL Server\":

```cmd
"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /uninstall /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER

"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /install /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER
```

### <a name="3-unable-to-install-sql-server-machine-learning-features-on-a-domain-controller"></a>3. Non è possibile installare le funzionalità di Machine Learning di SQL Server in un controller di dominio

Se si tenta di installare SQL Server 2016 R Services o Machine Learning Services di SQL Server in un controller di dominio, l'installazione non riesce con gli errori seguenti:

> *Errore durante il processo di installazione della funzionalità*
>
> *Impossibile trovare il gruppo con identità*
>
> *Codice errore componente: 0x80131509*

L'errore si verifica perché, in un controller di dominio, il servizio non è in grado di creare i 20 account locali necessari per eseguire Machine Learning. In generale, non è consigliabile installare SQL Server in un controller di dominio. Per altre informazioni, vedere il [bollettino del supporto tecnico 2032911](https://support.microsoft.com/help/2032911/you-may-encounter-problems-when-installing-sql-server-on-a-domain-cont).

### <a name="4-install-the-latest-service-release-to-ensure-compatibility-with-microsoft-r-client"></a>4. Installare la versione più recente del servizio per garantire la compatibilità con Microsoft R Client

Se si installa la versione più recente di Microsoft R Client e si usa tale versione per eseguire R su SQL Server in un contesto di calcolo remoto, è possibile che venga visualizzato un errore simile al seguente:

> *You are running version 9.x.x of Microsoft R Client on your computer, which is incompatible with the Microsoft R Server version 8.x.x. Download and install a compatible version.* (Sul computer è in esecuzione la versione 9.0.0 di Microsoft R, che non è compatibile con Microsoft R Server versione 8.0.3. Scaricare e installare una versione compatibile.)

SQL Server 2016 richiede che le librerie R nel client corrispondano esattamente alle librerie R nel server. La restrizione è stata rimossa per le versioni successive a R Server 9.0.1. Tuttavia, se si verifica questo errore, controllare la versione delle librerie R usate dal client e dal server e, se necessario, aggiornare il client in modo che corrisponda alla versione del server.

La versione di R installata con R Services per SQL Server viene aggiornata al momento dell'installazione di una nuova versione del servizio SQL Server. Per essere certi di avere sempre le versioni più aggiornate dei componenti R, assicurarsi di installare tutti i Service Pack.

Per garantire la compatibilità con Microsoft R Client 9.0.0, installare gli aggiornamenti descritti in questo [articolo del supporto tecnico](https://web.archive.org/web/20190415073655/https://support.microsoft.com/en-us/help/3210262/fix-version-of-r-client-is-incompatible-with-the-microsoft-r-server-ve).

Per evitare problemi con i pacchetti R, è anche possibile aggiornare la versione delle librerie R installate nel server, modificando il contratto di servizio in modo da usare i criteri moderni relativi al ciclo di vita del supporto, come descritto nella [sezione successiva](#bkmk_sqlbindr). Quando si esegue questa operazione, la versione di R installata con SQL Server viene aggiornata in base alla stessa pianificazione usata per gli aggiornamenti di Machine Learning Server (in precedenza Microsoft R Server).

**Si applica a:** SQL Server 2016 R Services, con R Server versione 9.0.0 o precedente

### <a name="5-r-components-missing-from-cu3-setup"></a>5. Componenti R mancanti dall'installazione di CU3

Per un numero limitato di macchine virtuali di Azure è stato effettuato il provisioning senza i file di installazione di R che dovrebbero essere inclusi in SQL Server. Il problema si applica alle macchine virtuali di cui è stato effettuato il provisioning nel periodo compreso tra il 05/01/2018 e il 23/01/2018. Questo problema può anche influire sulle installazioni locali se è stato applicato l'aggiornamento CU3 per SQL Server 2017 durante il periodo dal 05/01/2018 al 23/01/2018.

È stata fornita una versione del servizio che include la versione corretta dei file di installazione di R.

+ [Pacchetto di aggiornamento cumulativo 3 per SQL Server 2017 KB4052987](https://www.microsoft.com/download/details.aspx?id=56128)

Per installare i componenti e ripristinare SQL Server 2017 CU3, è necessario disinstallare l'aggiornamento cumulativo 3 e reinstallare la versione aggiornata:

1. Scaricare il file di installazione dell'aggiornamento cumulativo 3 aggiornato, che include i programmi di installazione di R.
2. Disinstallare l'aggiornamento cumulativo 3. Nel Pannello di controllo cercare **Disinstalla un aggiornamento** e quindi selezionare "Hotfix 3015 for SQL Server 2017 (KB4052987) (64-bit)". Procedere con i passaggi di disinstallazione.
3. Reinstallare l'aggiornamento CU3 facendo doppio clic sull'aggiornamento per KB4052987 appena scaricato: `SQLServer2017-KB4052987-x64.exe`. Seguire le istruzioni di installazione.

### <a name="6-unable-to-install-python-components-in-offline-installations-of-sql-server-2017-ctp-20-or-later"></a>6. Non è possibile installare i componenti Python nelle installazioni offline di SQL Server 2017 CTP 2.0 o versione successiva

Se si installa una versione provvisoria di SQL Server 2017 in un computer senza accesso a Internet, il programma di installazione potrebbe non riuscire a visualizzare la pagina che richiede il percorso dei componenti Python scaricati. In questa situazione è possibile installare la funzionalità Machine Learning Services, ma non i componenti Python.

Questo problema è stato risolto nella versione finale. Questa limitazione non si applica inoltre ai componenti R.

**Si applica a:** SQL Server 2017 con Python

### <a name="warning-of-incompatible-version-when-you-connect-to-an-older-version-of-sql-server-r-services-from-a-client-by-using-sssql17-md"></a><a name="bkmk_sqlbindr"></a> Avviso di versione non compatibile quando ci si connette a una versione precedente di R Services per SQL Server da un client che usa [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]

Quando si esegue codice R in un contesto di calcolo SQL Server 2016, è possibile che venga visualizzato l'errore seguente:

> *You are running version 9.0.0 of Microsoft R Client on your computer, which is incompatible with the Microsoft R Server version 8.0.3. Download and install a compatible version.* (Sul computer è in esecuzione la versione 9.0.0 di Microsoft R, che non è compatibile con Microsoft R Server versione 8.0.3. Scaricare e installare una versione compatibile.)

Questo messaggio viene visualizzato se una delle due condizioni seguenti è vera:

+ R Server (Standalone) è stato installato in un computer client usando l'installazione guidata di [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].
+ Microsoft R Server è stato installato usando il [programma di installazione di Windows separato](/machine-learning-server/install/r-server-install-windows).

Per assicurarsi che il server e il client usino la stessa versione, potrebbe essere necessario usare _binding_, supportato per Microsoft R Server 9.0 e versioni successive, per aggiornare i componenti di R nelle istanze di SQL Server 2016. Per determinare se è disponibile il supporto per gli aggiornamenti per la versione di R Services, vedere [Aggiornare un'istanza di R Services con SqlBindR.exe](../install/upgrade-r-and-python.md).

**Si applica a:** SQL Server 2016 R Services, con R Server versione 9.0.0 o precedente

### <a name="7-setup-for-sql-server-2016-service-releases-might-fail-to-install-newer-versions-of-r-components"></a>7. Durante l'installazione di alcune versioni del servizio SQL Server 2016 è possibile che le versioni più recenti dei componenti R non vengano installate

Quando si installa un aggiornamento cumulativo o un Service Pack per SQL Server 2016 in un computer che non è connesso a Internet, è possibile che durante l'Installazione guidata non venga visualizzato il prompt che consente di aggiornare i componenti R usando i file CAB scaricati. Questo errore si verifica in genere quando vengono installati più componenti insieme al motore di database.

Come soluzione alternativa, è possibile installare la versione del servizio usando la riga di comando e specificando l'argomento `MRCACHEDIRECTORY`, come illustrato in questo esempio relativo a CU1:

`C:\<path to installation media>\SQLServer2016-KB3164674-x64.exe /Action=Patch /IACCEPTROPENLICENSETERMS /MRCACHEDIRECTORY=<path to CU1 CAB files>`

Per ottenere i programmi di installazione più recenti, vedere [Installare i componenti di Machine Learning senza accesso a Internet](../install/sql-ml-component-install-without-internet-access.md).

**Si applica a:** SQL Server 2016 R Services, con R Server versione 9.0.0 o precedente

### <a name="8-launchpad-services-fails-to-start-if-the-version-is-different-from-the-r-version"></a>8. Il servizio Launchpad non viene avviato se la versione è diversa da quella di R

Se si installa R Services per SQL Server separatamente dal motore di database e le versioni delle build sono diverse, potrebbe essere visualizzato l'errore seguente nel registro eventi di sistema:

> *Il servizio Launchpad di SQL Server non è stato avviato per il seguente errore: Il servizio non ha risposto in tempo utile alla richiesta di avvio o di controllo.*

Questo errore può verificarsi, ad esempio, se si installa il motore di database usando la versione finale, si applica una patch per aggiornare il motore di database e quindi si aggiunge la funzionalità R Services usando la versione finale.

Per evitare questo problema, usare un'utilità come File Manager per confrontare le versioni di Launchpad.exe con la versione dei file binari SQL, ad esempio sqldk.dll. Tutti i componenti devono avere lo stesso numero di versione. Se si aggiorna un componente, assicurarsi di applicare lo stesso aggiornamento a tutti gli altri componenti installati.

Cercare Launchpad nella cartella `Binn` per l'istanza. Ad esempio, in un'installazione predefinita di SQL Server 2016, il percorso potrebbe essere `C:\Program Files\Microsoft SQL Server\MSSQL.13.InstanceNameMSSQL\Binn`. 

### <a name="9-remote-compute-contexts-are-blocked-by-a-firewall-in-sql-server-instances-that-are-running-on-azure-virtual-machines"></a>9. I contesti di calcolo remoto sono bloccati da un firewall nelle istanze di SQL Server in esecuzione su macchine virtuali di Azure

Se è stato installato [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] in una macchina virtuale di Azure, potrebbe non essere possibile usare contesti di calcolo che richiedono l'uso dell'area di lavoro della macchina virtuale. Il motivo è che, per impostazione predefinita, il firewall delle macchine virtuali di Azure include una regola che blocca l'accesso alla rete per gli account utente di R locali.

Come soluzione alternativa, nella macchina virtuale di Azure aprire **Windows Firewall con sicurezza avanzata**, selezionare **Regole in uscita** e disabilitare la regola seguente: **Block network access for R local user accounts in SQL Server instance MSSQLSERVER** (Blocca l'accesso alla rete per gli account utente locali di R nell'istanza di SQL Server MSSQLSERVER). È anche possibile lasciare la regola abilitata, ma modificare la proprietà di sicurezza in **Allow if secure** (Consenti se sicuro).

### <a name="10-implied-authentication-in-sqlexpress"></a>10. Autenticazione implicita in SQLEXPRESS

Quando si eseguono processi R da una workstation remota per data science tramite l'autenticazione integrata di Windows, SQL Server usa l'*autenticazione implicita* per generare eventuali chiamate ODBC locali richieste dallo script. Questa funzionalità, tuttavia, non funziona nella build RTM di SQL Server Express Edition.

Per risolvere il problema, è consigliabile eseguire l'aggiornamento a una versione più recente del servizio.

Se l'aggiornamento non è fattibile, come soluzione alternativa è possibile usare un account di accesso SQL per eseguire processi R remoti che potrebbero richiedere chiamate ODBC incorporate.

**Si applica a:** SQL Server 2016 R Services Express Edition

### <a name="11-performance-limits-when-libraries-used-by-sql-server-are-called-from-other-tools"></a>11. Limiti di prestazioni quando le librerie usate da SQL Server vengono chiamate da altri strumenti

È possibile chiamare le librerie di Machine Learning installate per SQL Server da un'applicazione esterna, ad esempio RGui. Questo potrebbe essere il modo più pratico per eseguire determinate attività, ad esempio l'installazione di nuovi pacchetti o l'esecuzione di test ad hoc su esempi di codice molto brevi. Tuttavia, al di fuori di SQL Server, le prestazioni potrebbero essere limitate.

Se si esegue il codice R usando strumenti esterni, R verrà eseguito in modalità a thread singolo anche se si usa ad esempio SQL Server Enterprise Edition. Per ottenere i vantaggi delle prestazioni in SQL Server, avviare una connessione di SQL Server e usare [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per chiamare il runtime di script esterno.

In generale, evitare di chiamare le librerie di Machine Learning usate da SQL Server da strumenti esterni. Se è necessario eseguire il debug di codice R o Python, in genere è più semplice farlo all'esterno di SQL Server. Per ottenere le stesse librerie incluse in SQL Server è possibile installare Microsoft R Client o [SQL Server 2017 Machine Learning Server (Standalone)](../install/sql-machine-learning-standalone-windows-install.md).

### <a name="12-sql-server-data-tools-does-not-support-permissions-required-by-external-scripts"></a>12. SQL Server Data Tools non supporta le autorizzazioni richieste da script esterni

Quando si usa Visual Studio o SQL Server Data Tools per pubblicare un progetto di database, se un'entità dispone di autorizzazioni specifiche per l'esecuzione di script esterni, è possibile che venga visualizzato un errore simile al seguente:

> *Modello TSQL: Errore rilevato durante il reverse engineering del database. L'autorizzazione non è stata riconosciuta e non è stata importata.*

Attualmente, il modello DACPAC non supporta le autorizzazioni usate da R Services o Machine Learning Services, ad esempio GRANT ANY EXTERNAL SCRIPT o EXECUTE ANY EXTERNAL SCRIPT. Questo problema verrà risolto in una versione futura.

Come soluzione alternativa, eseguire le istruzioni GRANT aggiuntive in uno script post-distribuzione.

### <a name="13-external-script-execution-is-throttled-due-to-resource-governance-default-values"></a>13. L'esecuzione di script esterni è limitata a causa dei valori predefiniti di governance delle risorse

In Enterprise Edition è possibile usare pool di risorse per gestire processi di script esterni. In alcune build preliminari non è possibile allocare ai processi R più del 20% della memoria complessiva. Di conseguenza, se il server ha 32 GB di RAM, i file eseguibili di R (RTerm.exe e BxlServer.exe) non possono usare più di 6,4 GB in un'unica richiesta.

Se si verificano limitazioni delle risorse, controllare l'impostazione predefinita corrente. Se il 20% non è sufficiente, vedere la documentazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su come modificare questo valore.

**Si applica a:** SQL Server 2016 R Services, Enterprise Edition

### <a name="14-error-when-using-sp_execute_external_script-without-libcso-on-linux"></a>14. Errore durante l'uso di `sp_execute_external_script` senza `libc++.so` in Linux

In un computer Linux pulito in cui non è installato `libc++.so`, l'esecuzione di una query `sp_execute_external_script` (SPEES) con Java o un linguaggio esterno ha esito negativo perché `commonlauncher.so` non è in grado di caricare `libc++.so`.

Ad esempio:

```sql
EXECUTE sp_execute_external_script @language = N'Java'
    , @script = N'JavaTestPackage.PassThrough'
    , @parallel = 0
    , @input_data_1 = N'select 1'
WITH RESULT SETS((col1 INT NOT NULL))
GO
```

Questa operazione ha esito negativo con un messaggio simile al seguente:

```text
Msg 39012, Level 16, State 14, Line 0

Unable to communicate with the runtime for 'Java' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Java' runtime.
```

Nei log `mssql-launchpadd` verrà visualizzato un messaggio di errore simile al seguente:

```text
Oct 18 14:03:21 sqlextmls launchpadd[57471]: [launchpad] 2019/10/18 14:03:21 WARNING: PopulateLauncher failed: Library /opt/mssql-extensibility/lib/commonlauncher.so not loaded. Error: libc++.so.1: cannot open shared object file: No such file or directory
```

**Soluzione alternativa**

È possibile usare una delle soluzioni alternative seguenti:

1. Copiare `libc++*` da `/opt/mssql/lib` al percorso di sistema predefinito `/lib64`

1. Aggiungere le voci seguenti a `/var/opt/mssql/mssql.conf` per esporre il percorso:

   ```text
   [extensibility]
   readabledirectories = /opt/mssql
   ```

**Si applica a:** SQL Server 2019 in Linux

### <a name="15-installation-or-upgrade-error-on-fips-enabled-servers"></a>15. Errori di installazione o aggiornamento nei server abilitati per FIPS

Se si installa SQL Server 2019 con la funzionalità **Machine Learning Services ed estensioni del linguaggio** o si aggiorna l'istanza di SQL Server in un server abilitato per [FIPS (Federal Information Processing Standard)](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), verrà visualizzato l'errore seguente:

> *Si è verificato un errore durante l'operazione di installazione della funzionalità di estendibilità con messaggio di errore: Creazione di AppContainer non riuscita con messaggio di errore NONE, stato Questa implementazione non fa parte degli algoritmi crittografici convalidati per Windows Platform FIPS.*

**Soluzione alternativa**

Disabilitare FIPS prima dell'installazione di SQL Server 2019 con la funzionalità **Machine Learning Services ed estensioni del linguaggio** o dell'aggiornamento dell'istanza di SQL Server. Una volta completato l'aggiornamento o l'installazione, è possibile abilitare di nuovo FIPS.

**Si applica a:** SQL Server 2019

## <a name="r-script-execution-issues"></a>Problemi di esecuzione di script R

Questa sezione contiene informazioni sui problemi noti specifici dell'esecuzione di R in SQL Server, oltre ad alcuni problemi correlati alle librerie e agli strumenti R pubblicati da Microsoft, tra cui RevoScaleR.

Per informazioni su altri problemi noti che potrebbero influire sulle soluzioni R, vedere il sito di [Machine Learning Server](/machine-learning-server/resources-known-issues).

### <a name="1-access-denied-warning-when-executing-r-scripts-on-sql-server-in-a-non-default-location"></a>1. Avviso di accesso negato durante l'esecuzione di script R in SQL Server in un percorso non predefinito

Se l'istanza di SQL Server è stata installata in un percorso non predefinito, ad esempio all'esterno della cartella `Program Files`, viene generato l'avviso ACCESS_DENIED quando si tenta di eseguire script che installano un pacchetto. Ad esempio:

> *In `normalizePath(path.expand(path), winslash, mustWork)`: path[2]="~ExternalLibraries/R/8/1": Accesso negato*

Il motivo è che una funzione R tenta di leggere il percorso e non riesce se il gruppo di utenti predefinito **SQLRUserGroup** non ha accesso in lettura. L'avviso generato non blocca l'esecuzione dello script R corrente, ma è possibile che l'avviso venga generato ripetutamente ogni volta che l'utente esegue qualsiasi altro script R.

Se SQL Server è stato installato nel percorso predefinito, questo errore non si verifica perché tutti gli utenti di Windows hanno autorizzazioni di lettura per la cartella `Program Files`.

Questo problema verrà risolto in una prossima versione del servizio. Come soluzione alternativa, concedere al gruppo **SQLRUserGroup** l'accesso in lettura per tutte le cartelle padre di `ExternalLibraries`.

### <a name="2-serialization-error-between-old-and-new-versions-of-revoscaler"></a>2. Errore di serializzazione tra le versioni precedenti e nuove di RevoScaleR

Quando si passa un modello usando un formato serializzato a un'istanza di SQL Server remota, è possibile ricevere l'errore: 

> *Errore in memDecompress(dati, tipo = decompressione) errore interno -3 in memDecompress(2).*

Questo errore viene generato se il modello è stato salvato usando una versione recente della funzione di serializzazione, [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel), ma l'istanza di SQL Server in cui si deserializza il modello ha una versione precedente delle API RevoScaleR, da SQL Server 2017 CU2 o versioni precedenti.

Come soluzione alternativa, è possibile aggiornare l'istanza di SQL Server 2017 a CU3 o versione successiva.

L'errore non viene visualizzato se la versione dell'API è la stessa o se si sta spostando un modello salvato con una funzione di serializzazione precedente in un server che usa una versione più recente dell'API di serializzazione.

In altre parole, usare la stessa versione di RevoScaleR per le operazioni di serializzazione e deserializzazione.

### <a name="3-real-time-scoring-does-not-correctly-handle-the-_learningrate_-parameter-in-tree-and-forest-models"></a>3. Il punteggio in tempo reale non gestisce correttamente il parametro _learningRate_ nei modelli di albero e foresta

Se si crea un modello usando un metodo di albero delle decisioni o di foresta delle decisioni e si specifica la velocità di apprendimento, è possibile che vengano visualizzati risultati incoerenti quando si usa `sp_rxpredict` o la funzione SQL `PREDICT`, rispetto all'uso di `rxPredict`.

La causa è un errore nell'API che elabora i modelli serializzati, limitato al parametro `learningRate`: ad esempio, in [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees) o

Questo problema verrà risolto in una prossima versione del servizio.

### <a name="4-limitations-on-processor-affinity-for-r-jobs"></a>4. Limitazioni relative all'affinità processori per i processi R

Nella build della versione iniziale di SQL Server 2016 è possibile impostare l'affinità processori solo per le CPU presenti nel primo gruppo K. Ad esempio, se il server è un computer a 2 socket con 2 gruppi K, per i processi R vengono usati solo i processori appartenenti al primo gruppo K. La stessa limitazione si applica quando si configura la governance delle risorse per processi di script R.

Questo problema è stato risolto in SQL Server 2016 Service Pack 1. Si consiglia di eseguire l'aggiornamento alla versione più recente del servizio.

**Si applica a:** Versione RTM di SQL Server 2016 R Services

### <a name="5-changes-to-column-types-cannot-be-performed-when-reading-data-in-a-sql-server-compute-context"></a>5. Non è possibile apportare modifiche ai tipi di colonna durante la lettura di dati in un contesto di calcolo di SQL Server

Se il contesto di calcolo è impostato sull'istanza di SQL Server, non è possibile usare l'argomento _colClasses_ (o altri argomenti simili) per modificare il tipo di dati delle colonne presenti nel codice R.

L'istruzione seguente, ad esempio, causerebbe un errore se la colonna CRSDepTimeStr non fosse già un numero intero:

```R
data <- RxSqlServerData(
  sqlQuery = "SELECT CRSDepTimeStr, ArrDelay  FROM AirlineDemoSmall", 
  connectionString = connectionString, 
  colClasses = c(CRSDepTimeStr = "integer"))
```

Come soluzione alternativa, è possibile riscrivere la query SQL in modo da usare CAST o CONVERT e presentare i dati in R usando il tipo di dati corretto. In generale, le prestazioni sono migliori quando si lavora con i dati usando SQL invece di modificare i dati nel codice R.

**Si applica a:** SQL Server 2016 R Services

### <a name="6-limits-on-size-of-serialized-models"></a>6. Limiti per le dimensioni dei modelli serializzati

Quando si salva un modello in una tabella di SQL Server, è necessario serializzare il modello e salvarlo in un formato binario. In teoria, le dimensioni massime di un modello che possono essere archiviate con questo metodo sono pari a 2 GB, ovvero le dimensioni massime delle colonne varbinary in SQL Server.

Se è necessario usare modelli più grandi, sono disponibili le soluzioni alternative seguenti:

+ Intervenire per ridurre le dimensioni del modello. Alcuni pacchetti R open source includono una grande quantità di informazioni nell'oggetto modello e gran parte di queste informazioni può essere rimossa per la distribuzione. 
+ Usare la selezione delle funzionalità per rimuovere le colonne non necessarie.
+ Se si usa un algoritmo open source, prendere in considerazione un'implementazione simile usando l'algoritmo corrispondente in MicrosoftML o RevoScaleR. Questi pacchetti sono stati ottimizzati per gli scenari di distribuzione.
+ Dopo aver razionalizzato il modello e aver ridotto le dimensioni usando i passaggi precedenti, verificare se è possibile usare la funzione [memCompress](https://www.rdocumentation.org/packages/base/versions/3.4.1/topics/memCompress) in R di base per ridurre le dimensioni del modello prima di passarlo a SQL Server. Questa opzione è consigliata quando il modello ha dimensioni prossime al limite di 2 GB.
+ Per i modelli più grandi, è possibile usare la funzionalità [FileTable](../../relational-databases/blob/filetables-sql-server.md) di SQL Server per archiviare i modelli, anziché usare una colonna varbinary.

    Per usare le tabelle FileTable, è necessario aggiungere un'eccezione del firewall, perché i dati archiviati nelle tabelle FileTable vengono gestiti dal driver del file system Filestream in SQL Server e le regole del firewall predefinite bloccano l'accesso ai file di rete. Per altre informazioni, vedere [Abilitare i prerequisiti per la tabella FileTable](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md).

    Dopo aver abilitato FileTable, per scrivere il modello è necessario ottenere un percorso da SQL tramite l'API FileTable e quindi scrivere il modello in tale posizione dal codice. Quando è necessario leggere il modello, ottenere il percorso da SQL e quindi chiamare il modello usando il percorso dallo script. Per altre informazioni, vedere [Accedere alle tabelle FileTable con API di input/output dei file](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md).

### <a name="7-avoid-clearing-workspaces-when-you-execute-r-code-in-a-ssnoversion-compute-context"></a>7. Evitare di cancellare i dati delle aree di lavoro durante l'esecuzione di codice R in un contesto di calcolo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Se si usa un comando R per cancellare oggetti dall'area di lavoro durante l'esecuzione di codice R in un contesto di calcolo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure si cancellano i dati dell'area di lavoro nell'ambito di uno script R chiamato tramite [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), è possibile che venga visualizzato questo errore: *workspace object revoScriptConnection not found*(oggetto revoScriptConnection dell'area di lavoro non trovato).

`revoScriptConnection` è un oggetto dell'area di lavoro R che contiene informazioni su una sessione R chiamata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se, tuttavia, il codice R include un comando per cancellare i dati dell'area di lavoro, ad esempio `rm(list=ls()))`, anche tutte le informazioni relative alla sessione e altri oggetti presenti nell'area di lavoro R vengono cancellati.

Come soluzione alternativa, evitare di cancellare in modo indiscriminato variabili e altri oggetti durante l'esecuzione di R in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Anche se è un'operazione comune nella console R, la cancellazione dei dati dell'area di lavoro può avere conseguenze impreviste.

+ Per eliminare variabili specifiche, usare la funzione `remove` di R, ad esempio `remove('name1', 'name2', ...)`
+ Se devono essere eliminate più variabili, salvare i nomi delle variabili temporanee in un elenco ed eseguire periodicamente operazioni di Garbage Collection.

### <a name="8-restrictions-on-data-that-can-be-provided-as-input-to-an-r-script"></a>8. Limitazioni sui dati che possono essere forniti come input per uno script R

Non è possibile usare in uno script R i tipi seguenti di risultati delle query:

- Dati da una query [!INCLUDE[tsql](../../includes/tsql-md.md)] che fa riferimento alle colonne AlwaysEncrypted.
  
- Dati da una query [!INCLUDE[tsql](../../includes/tsql-md.md)] che fa riferimento alle colonne mascherate.
  
     Se è necessario usare dati mascherati in uno script R, una possibile soluzione consiste nel creare una copia dei dati in una tabella temporanea e usare invece tali dati.

### <a name="9-use-of-strings-as-factors-can-lead-to-performance-degradation"></a>9. L'uso di stringhe come fattori può causare un calo delle prestazioni

L'uso di variabili di tipo stringa come fattori può aumentare significativamente la quantità di memoria usata per le operazioni R. Si tratta di un problema noto di R in generale e sono disponibili molti articoli sull'argomento. Vedere, ad esempio, [Factors are not first-class citizens in R, by John Mount, in R-bloggers](https://www.r-bloggers.com/factors-are-not-first-class-citizens-in-r/) (I fattori non sono cittadini di prima classe in R, di John Mount, in R-bloggers) o [stringsAsFactors: An unauthorized biography, by Roger Peng](https://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/) (stringsAsFactors: una biografia non autorizzata, di Roger Peng). 

Anche se il problema non è specifico di SQL Server, può influire significativamente sulle prestazioni del codice R eseguito in SQL Server. Le stringhe vengono in genere archiviate come varchar o nvarchar e se una colonna di dati di tipo stringa ha molti valori univoci, il processo di conversione interna eseguito da R di questi valori in numeri interi e di nuovo in stringhe può causare errori di allocazione della memoria.

Se non è assolutamente necessario un tipo di dati stringa per altre operazioni, il mapping dei valori stringa a un tipo di dati numerico (integer) come parte della preparazione dei dati può risultare vantaggioso dal punto di vista delle prestazioni e della scalabilità.

Per una descrizione di questo problema e per altri suggerimenti, vedere [Prestazioni per i servizi R - Ottimizzazione dei dati](../r/r-and-data-optimization-r-services.md).

### <a name="10-arguments-varstokeep-and-varstodrop-are-not-supported-for-sql-server-data-sources"></a>10. Gli argomenti *varsToKeep* e *varsToDrop* non sono supportati per le origini dati di SQL Server

Quando si usa la funzione rxDataStep per scrivere i risultati in una tabella, l'uso di *varsToKeep* e *varsToDrop* è un modo pratico per specificare le colonne da includere o escludere come parte dell'operazione. Questi argomenti non sono tuttavia supportati per le origini dati di SQL Server.

### <a name="11-limited-support-for-sql-data-types-in-sp_execute_external_script"></a>11. Supporto limitato per i tipi di dati SQL in sp\_execute\_external\_script

Non tutti i tipi di dati supportati in SQL possono essere usati in R. Come soluzione alternativa, valutare la possibilità di eseguire il cast del tipo di dati non supportato in un tipo di dati supportato prima di passare i dati a sp\_execute\_external\_script.

Per altre informazioni, vedere [Librerie R e tipi di dati](../r/r-libraries-and-data-types.md).

### <a name="12-possible-string-corruption-using-unicode-strings-in-varchar-columns"></a>12. Possibile danneggiamento delle stringhe con stringhe Unicode nelle colonne varchar

Il passaggio di dati Unicode nelle colonne varchar da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a R/Python può causare il danneggiamento delle stringhe. Ciò è dovuto al fatto che la codifica per queste stringhe Unicode nelle regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe non corrispondere alla codifica UTF-8 predefinita usata in R/Python. 

Per inviare dati di tipo stringa non ASCII da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a R/Python, usare la codifica UTF-8 (disponibile in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) o usare il tipo nvarchar.

### <a name="13-only-one-value-of-type-raw-can-be-returned-from-sp_execute_external_script"></a>13. È possibile restituire solo un valore di tipo `raw` da `sp_execute_external_script`

Quando un tipo di dati binario (il tipo di dati **raw** di R) viene restituito da R, il valore deve essere inviato nel frame dei dati di output.

Con tipi di dati diversi da **raw**, è possibile restituire i valori dei parametri insieme ai risultati della stored procedure aggiungendo la parola chiave OUTPUT. Per altre informazioni, vedere [Parametri](../../relational-databases/stored-procedures/parameters.md).

Se si vogliono usare più set di output che includono valori di tipo **raw**, una delle possibili soluzioni alternative consiste nell'eseguire più chiamate della stored procedure o nell'inviare di nuovo i set di risultati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite ODBC.

### <a name="14-loss-of-precision"></a>14. Perdita di precisione

Dato che [!INCLUDE[tsql](../../includes/tsql-md.md)] e R supportano vari tipi di dati, i tipi di dati numerici possono subire una perdita di precisione durante la conversione.

Per altre informazioni sulla conversione implicita dei tipi di dati, vedere [Librerie e tipi di dati R](../r/r-libraries-and-data-types.md).

### <a name="15-variable-scoping-error-when-you-use-the-transformfunc-parameter"></a>15. Errore di ambito delle variabili quando si usa il parametro transformFunc

Per trasformare i dati durante la modellazione, è possibile passare un argomento *transformFunc* in una funzione, ad esempio `rxLinmod` o `rxLogit`. Tuttavia, le chiamate di funzione annidate possono causare errori di ambito nel contesto di calcolo di SQL Server, anche se le chiamate funzionano correttamente nel contesto di calcolo locale.

> *Il set di dati di esempio per l'analisi non contiene variabili*

Ad esempio, si supponga di aver definito le due funzioni `f` e `g` nell'ambiente globale locale e che `g` chiami `f`. Nelle chiamate remote o distribuite che coinvolgono `g`, la chiamata a `g` potrebbe non riuscire con questo errore perché non è stato trovato `f`, anche se sono stati passati `f` e `g` alla chiamata remota.

Se si verifica questo problema, è possibile risolverlo incorporando la definizione di `f` all'interno della definizione di `g`, in qualsiasi punto prima che `g` chiamerebbe normalmente `f`.

Ad esempio:

```R
f <- function(x) { 2*x * 3 }
g <- function(y) {
              a <- 10 * y
               f(a)
}
```

Per evitare l'errore, riscrivere la definizione come segue:

```R
g <- function(y){
              f <- function(x) { 2*x +3}
              a <- 10 * y
              f(a)
}
```

### <a name="16-data-import-and-manipulation-using-revoscaler"></a>16. Importazione e modifica dei dati con RevoScaleR

Quando vengono lette colonne **varchar** da un database, lo spazio vuoto viene tagliato. Per evitare questo problema, racchiudere le stringhe tra caratteri che non siano spazi vuoti.

Quando si usano funzioni come `rxDataStep` per creare tabelle di database con colonne **varchar**, la larghezza della colonna viene stimata in base a un campione di dati. Se la larghezza può variare, potrebbe essere necessario riempire tutte le stringhe fino a una lunghezza comune.

L'uso di una trasformazione per modificare il tipo di dati di una variabile non è supportato quando si usano chiamate ripetute a `rxImport` o a `rxTextToXdf` per importare e aggiungere righe, combinando più file di input in un singolo file XDF.

### <a name="17-limited-support-for-rxexec"></a>17. Supporto limitato per rxExec

In SQL Server 2016, la funzione `rxExec` fornita dal pacchetto RevoScaleR può essere usata solo in modalità a thread singolo.

### <a name="18-increase-the-maximum-parameter-size-to-support-rxgetvarinfo"></a>18. Aumentare le dimensioni massime dei parametri per supportare rxGetVarInfo

Se si usano set di dati con un numero molto elevato di variabili (ad esempio, più di 40.000), impostare il flag `max-ppsize` quando si avvia R per usare funzioni come `rxGetVarInfo`. Il flag `max-ppsize` specifica le dimensioni massime dello stack di protezione dell'indicatore di misura.

Se si usa la console di R (ad esempio, RGui.exe o RTerm.exe), è possibile impostare il valore di _max-ppsize_ su 500000 digitando:

```R
R --max-ppsize=500000
```

### <a name="19-issues-with-the-rxdtree-function"></a>19. Problemi con la funzione rxDTree

La funzione `rxDTree` attualmente non supporta le trasformazioni nella formula. In particolare, l'uso della sintassi `F()` per la creazione di fattori in tempo reale non è supportata. Tuttavia, verrà eseguito automaticamente il binning dei dati numerici.

I fattori ordinati vengono trattati proprio come i fattori in tutte le funzioni di analisi RevoScaleR eccetto `rxDTree`.

### <a name="20-datatable-as-an-outputdataset-in-r"></a>20. Data.table come OutputDataSet in R

L'uso di `data.table` come `OutputDataSet` in R non è supportato in SQL Server 2017 aggiornamento cumulativo 13 (CU13) e versioni precedenti. Potrebbe essere visualizzato il messaggio seguente:

``` text
Msg 39004, Level 16, State 20, Line 2
A 'R' script error occurred during execution of 
'sp_execute_external_script' with HRESULT 0x80004004.
Msg 39019, Level 16, State 2, Line 2
An external script error occurred: 
Error in alloc.col(newx) : 
  Internal error: length of names (0) is not length of dt (11)
Calls: data.frame ... as.data.frame -> as.data.frame.data.table -> copy -> alloc.col

Error in execution.  Check the output for more information.
Error in eval(expr, envir, enclos) : 
  Error in execution.  Check the output for more information.
Calls: source -> withVisible -> eval -> eval -> .Call
Execution halted
```

L'uso di `data.table` come `OutputDataSet` in R è supportato in SQL Server 2017 aggiornamento cumulativo 14 (CU14) e versioni successive.

### <a name="21-running-a-long-script-fails-while-installing-a-library"></a>21. L'esecuzione di uno script lungo non riesce durante l'installazione di una libreria

Se si esegue una sessione di script esterno con esecuzione prolungata e il dbo tenta in parallelo di installare una libreria in un database diverso, lo script potrebbe essere terminato.

Ad esempio, si supponga di eseguire questo script esterno sul database master:

```sql
USE MASTER
DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(max) = N'Sys.sleep(100)'
DECLARE @input_data_1 nvarchar(max) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1 with result sets none
go
```

Mentre dbo installa in parallelo una libreria in LibraryManagementFunctional:

```sql
USE [LibraryManagementFunctional]
go

CREATE EXTERNAL LIBRARY [RODBC] FROM (CONTENT = N'/home/ani/var/opt/mssql/data/RODBC_1.3-16.tar.gz') WITH (LANGUAGE = 'R')
go

DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(14) = N'library(RODBC)'
DECLARE @input_data_1 nvarchar(8) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1
go
```

Lo script esterno con esecuzione prolungata precedente sul database master verrà terminato con il messaggio di errore seguente:

> *Si è verificato un errore di script 'R' durante l'esecuzione di 'sp_execute_external_script' con HRESULT 0x800704d4.*

**Soluzione alternativa**

Non eseguire l'installazione della libreria in parallelo alla query con esecuzione prolungata. Oppure eseguire nuovamente la query con esecuzione prolungata al termine dell'installazione.

**Si applica a:** SQL Server 2019 solo in Linux e in cluster Big Data.

### <a name="22-sql-server-stops-responding-when-executing-r-scripts-containing-parallel-execution"></a>22. SQL Server smette di rispondere quando si eseguono script R contenenti l'esecuzione parallela

SQL Server 2019 contiene una regressione che ha effetto sugli script R che usano l'esecuzione simultanea. Sono esempi l'uso di `rxExec` con il contesto di elaborazione `RxLocalPar` e gli script che usano il pacchetto parallelo. Questo problema è causato da errori rilevati dal pacchetto parallelo durante la scrittura sul dispositivo Null nel corso dell'esecuzione in SQL Server.

**Si applica a:** SQL Server 2019.

### <a name="23-precision-loss-for-moneynumericdecimalbigint-data-types"></a>23. Perdita di precisione per i tipi di dati money/numeric/decimal/bigint

L'esecuzione di uno script R con `sp_execute_external_script` consente di usare i tipi di dati money, numeric, decimal e bigint come dati di input. Tuttavia, poiché i dati vengono convertiti nel tipo numerico di R, si verifica una perdita di precisione per i valori molto alti o con posizioni decimali.

+ **money**: In alcuni casi i valori dei centesimi non sono precisi e viene generato un avviso: *Warning: unable to precisely represent cents values* (Avviso: Impossibile rappresentare con precisione i valori dei centesimi).  
+ **numeric/decimal**: `sp_execute_external_script` con uno script R non supporta l'intera gamma del tipo di dati e modifica le ultime cifre decimali, in particolare in presenza di frazioni.
+ **bigint**: R supporta solo valori integer fino a 53 bit, quindi inizia a verificarsi una perdita di precisione.

## <a name="python-script-execution-issues"></a>Problemi di esecuzione di script Python

Questa sezione contiene informazioni sui problemi noti specifici dell'esecuzione di Python in SQL Server, oltre ai problemi correlati ai pacchetti Python pubblicati da Microsoft, tra cui [revoscalepy](/r-server/python-reference/revoscalepy/revoscalepy-package) e [microsoftml](/r-server/python-reference/microsoftml/microsoftml-package).

### <a name="1-call-to-pretrained-model-fails-if-path-to-model-is-too-long"></a>1. La chiamata al modello con training preliminare non riesce se il percorso del modello è troppo lungo

Se sono stati installati i modelli con training preliminare in una versione preliminare di SQL Server 2017, il percorso completo del file di modello sottoposto a training potrebbe essere troppo lungo per la lettura da parte di Python. Questa limitazione è stata risolta in una versione successiva del servizio.

Esistono diverse possibili soluzioni alternative:

+ Quando si installano i modelli con training preliminare, scegliere un percorso personalizzato.
+ Se possibile, installare l'istanza di SQL Server in un percorso di installazione personalizzato con un percorso più breve, ad esempio C:\SQL\MSSQL14.MSSQLSERVER.
+ Usare l'utilità di Windows [Fsutil](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc788097(v=ws.11)) per creare un collegamento reale che esegue il mapping del file del modello a un percorso più breve.
+ Eseguire l'aggiornamento alla versione più recente del servizio.

### <a name="2-error-when-saving-serialized-model-to-sql-server"></a>2. Errore durante il salvataggio del modello serializzato in SQL Server

Quando si passa un modello a un'istanza di SQL Server remota e si tenta di leggere il modello binario usando la funzione `rx_unserialize` in [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), si potrebbe ricevere l'errore: 

> *NameError: il nome 'rx_unserialize_model' non è definito*

Questo errore viene generato se il modello è stato salvato usando una versione recente della funzione di serializzazione, ma l'istanza di SQL Server in cui si deserializza non riconosce l'API di serializzazione.

Per risolvere il problema, aggiornare l'istanza di SQL Server 2017 a CU3 o versione successiva.

### <a name="3-failure-to-initialize-a-varbinary-variable-causes-an-error-in-bxlserver"></a>3. L'impossibilità di inizializzare una variabile varbinary causa un errore in BxlServer

Se si esegue codice Python in SQL Server usando `sp_execute_external_script` e il codice include variabili di output di tipo varbinary(max), varchar(max) o tipi simili, la variabile deve essere inizializzata o impostata come parte dello script. In caso contrario, il componente di scambio di dati, BxlServer, genera un errore e smette di funzionare.

Questa limitazione verrà risolta in una prossima versione del servizio. Come soluzione alternativa, assicurarsi che la variabile sia inizializzata nello script Python. È possibile usare qualsiasi valore valido, come negli esempi seguenti:

```sql
declare @b varbinary(max);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N'b = 0x0'
  , @params = N'@b varbinary(max) OUTPUT'
  , @b = @b OUTPUT;
go
```

```sql
declare @b varchar(30);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N' b = ""  '
  , @params = N'@b varchar(30) OUTPUT'
  , @b = @b OUTPUT;
go
```

### <a name="4-telemetry-warning-on-successful-execution-of-python-code"></a>4. Avviso di telemetria al completamento dell'esecuzione del codice Python

A partire da SQL Server 2017 CU2, il messaggio seguente potrebbe essere visualizzato anche se il codice Python viene eseguito in modo corretto:

> *Messaggi STDERR dallo script esterno:* 
>  *~PYTHON_SERVICES\lib\site-packages\revoscalepy\utils\RxTelemetryLogger*
> *SyntaxWarning: telemetry_state usato prima della dichiarazione globale*

Questo problema è stato risolto in SQL Server 2017 aggiornamento cumulativo 3 (CU3). 

### <a name="5-numeric-decimal-and-money-data-types-not-supported"></a>5. Tipi di dati numerici, decimali e monetari non supportati

A partire da SQL Server 2017 aggiornamento cumulativo 12 (CU12), i tipi di dati numerici, decimali e money in WITH RESULT SETS non sono supportati quando si usa Python con `sp_execute_external_script`. Potrebbero essere visualizzati i messaggi seguenti:

> *[Codice: 39004, Stato SQL: S1000] Si è verificato un errore di script 'Python' durante l'esecuzione di 'sp_execute_external_script' con HRESULT 0x80004004.*
>
> *[Codice: 39019, Stato SQL: S1000] Si è verificato un errore dello script esterno:*
>
> *Errore SqlSatelliteCall: Tipo non supportato nello schema di output. Tipi supportati: bit, smallint, int, datetime, smallmoney, real e float. char, varchar sono parzialmente supportati.*

Questo problema è stato risolto in SQL Server 2017 aggiornamento cumulativo 14 (CU14).

### <a name="6-bad-interpreter-error-when-installing-python-packages-with-pip-on-linux"></a>6. Errore di interprete non valido durante l'installazione di pacchetti Python con pip in Linux 

In SQL Server 2019, se si tenta di usare **pip**. Ad esempio:

```bash
/opt/mssql/mlservices/runtime/python/bin/pip -h
```

Si riceverà questo errore:

> *bash: /opt/mssql/mlservices/runtime/python/bin/pip: /opt/microsoft/mlserver/9.4.7/bin/python/python: interprete non valido: Impossibile trovare il file o la directory*

**Soluzione alternativa**

Installare **pip** da [Python Package Authority (PyPA)](https://www.pypa.io):

```bash
wget 'https://bootstrap.pypa.io/get-pip.py' 
/opt/mssql/mlservices/bin/python/python ./get-pip.py 
```

**Consiglio**

Vedere [Installare pacchetti Python con sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md).

**Si applica a:** SQL Server 2019 in Linux

### <a name="7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows"></a>7. Non è possibile installare pacchetti Python con pip dopo l'installazione di SQL Server 2019 in Windows

Dopo l'installazione di SQL Server 2019 in Windows, il tentativo di installare un pacchetto Python tramite **pip** da una riga di comando DOS avrà esito negativo. Ad esempio:

```bash
pip install quantfolio
```

Verrà restituito l'errore seguente:

> *pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.* (pip è configurato con percorsi che richiedono TLS/SSL, ma il modulo SSL in Python non è disponibile)

Si tratta di un problema specifico del pacchetto Anaconda. Il problema verrà risolto in una prossima versione del servizio.

**Soluzione alternativa**

Copiare i file seguenti:

+ `libssl-1_1-x64.dll`
+ `libcrypto-1_1-x64.dll`

dalla cartella <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\Library\bin`

alla cartella <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\DLLs`

Aprire quindi un nuovo prompt della shell dei comandi DOS.

**Si applica a:** SQL Server 2019 in Windows

### <a name="8-error-when-using-sp_execute_external_script-without-libcaboso-on-linux"></a>8. Errore durante l'uso di `sp_execute_external_script` senza `libc++abo.so` in Linux

In un computer Linux pulito in cui non è installato `libc++abi.so`, l'esecuzione di una query (SPEES) `sp_execute_external_script` ha esito negativo con un errore "Impossibile trovare il file o la directory".

Ad esempio:

```text
EXEC sp_execute_external_script
    @language = N'Python'
    , @script = N'
OutputDataSet = InputDataSet'
    , @input_data_1 = N'select 1'
    , @input_data_1_name = N'InputDataSet'
    , @output_data_1_name = N'OutputDataSet'
    WITH RESULT SETS (([output] int not null));
Msg 39012, Level 16, State 14, Line 0
Unable to communicate with the runtime for 'Python' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Python' runtime.
STDERR message(s) from external script:

Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.

SqlSatelliteCall error: Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.
STDOUT message(s) from external script:
SqlSatelliteCall function failed. Please see the console output for more information.
Traceback (most recent call last):
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/computecontext/RxInSqlServer.py", line 605, in rx_sql_satellite_call
    rx_native_call("SqlSatelliteCall", params)
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/RxSerializable.py", line 375, in rx_native_call
    ret = px_call(functionname, params)
RuntimeError: revoscalepy function failed.
Total execution time: 00:01:00.387
```

**Soluzione alternativa**

Eseguire il comando seguente:

```bash
sudo cp /opt/mssql/lib/libc++abi.so.1 /opt/mssql-extensibility/lib/
```

**Si applica a:** SQL Server 2019 in Linux

### <a name="9-cannot-install-tensorflow-package-using-sqlmlutils"></a>9. Impossibile installare il pacchetto **tensorflow** usando **sqlmlutils**

Il [pacchetto sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md) viene usato per installare pacchetti Python in SQL Server 2019. È necessario scaricare, installare e aggiornare [Microsoft Visual C++ 2015-2019 Redistributable (x64)](https://visualstudio.microsoft.com/downloads/). Il pacchetto **tensorflow**, tuttavia, non può essere installato usando sqlmlutils. Il pacchetto tensorflow dipende da una versione più recente di numpy rispetto alla versione installata in SQL Server. numpy è tuttavia un pacchetto di sistema preinstallato che sqlmlutils non può aggiornare quando viene eseguito un tentativo di installazione di tensorflow.

**Soluzione alternativa**

Usando un prompt dei comandi in modalità amministratore, eseguire il comando seguente, sostituendo "MSSQLSERVER" con il nome dell'istanza di SQL:

   ```cmd
   "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES\python.exe" -m pip install --upgrade tensorflow
   ```

   Se viene visualizzato un errore "TLS/SSL", vedere [7. Non è possibile installare pacchetti Python con pip](#7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows) più indietro in questo articolo.

**Si applica a:** SQL Server 2019 in Windows

## <a name="revolution-r-enterprise-and-microsoft-r-open"></a>Revolution R Enterprise e Microsoft R Open

Questa sezione elenca i problemi specifici degli strumenti per le prestazioni, lo sviluppo e la connettività di R forniti da Revolution Analytics. Questi strumenti erano inclusi in precedenti versioni non definitive di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

In generale, è consigliabile disinstallare queste versioni e installare la versione più recente di SQL Server o Microsoft R Server.

### <a name="1-revolution-r-enterprise-is-not-supported"></a>1. Revolution R Enterprise non è supportato

L'installazione side-by-side di Revolution R Enterprise con qualsiasi altra versione di [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] non è supportata.

Se si ha una licenza esistente per Revolution R Enterprise, è necessario inserirla in un computer separato sia dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia da qualsiasi workstation che si vuole usare per connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Alcune versioni non definitive di [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] includono un ambiente di sviluppo R per Windows creato da Revolution Analytics. Questo strumento non è più disponibile e non è supportato.

Per compatibilità con [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], è consigliabile installare invece Microsoft R Client. Anche [R Tools per Visual Studio](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019) e [Visual Studio Code](https://code.visualstudio.com/) supportano le soluzioni Microsoft R.

### <a name="2-compatibility-issues-with-sqlite-odbc-driver-and-revoscaler"></a>2. Problemi di compatibilità con il driver ODBC di SQLite e RevoScaleR

La revisione 0.92 del driver ODBC SQLite è incompatibile con RevoScaleR. Le revisioni 0.88-0.91 e 0.93 e versioni successive sono note come compatibili.

## <a name="next-steps"></a>Passaggi successivi

[Raccogliere i dati per risolvere i problemi di SQL Server Machine Learning Services](data-collection-ml-troubleshooting-process.md)
