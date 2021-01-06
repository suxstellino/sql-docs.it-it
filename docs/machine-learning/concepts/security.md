---
title: Architettura di sicurezza per l'estendibilità
description: Questo articolo descrive l'architettura di sicurezza per il framework di estendibilità in Machine Learning Services per SQL Server. Include sicurezza per gli account di accesso e gli account utente, servizio Launchpad di SQL Server, account di lavoro, esecuzione di più script e autorizzazioni file.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.custom: contperf-fy21q1, seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 63138dbd9ef2acd45dfc508bac20bd6694d49d90
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878887"
---
# <a name="security-architecture-for-the-extensibility-framework-in-sql-server-machine-learning-services"></a>Architettura di sicurezza per il framework di estendibilità in Machine Learning Services per SQL Server

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo descrive l'architettura di sicurezza usata per integrare il motore di database di SQL Server e i componenti correlati con il framework di estendibilità in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). L'articolo esamina anche gli oggetti a protezione diretta, i servizi, l'identità dei processi e le autorizzazioni. I concetti chiave presentati in questo articolo includono le finalità di launchpad, SQLRUserGroup e gli account di lavoro, l'isolamento dei processi di script esterni e il mapping delle identità utente ad account di lavoro.

Per altre informazioni sui concetti e i componenti principali dell'estendibilità in SQL Server, vedere [Architettura di estendibilità in Machine Learning Services per SQL Server](extensibility-framework.md).

## <a name="securables-for-external-script"></a>Oggetti a protezione diretta per lo script esterno

Uno script esterno viene inviato come parametro di input a una [stored procedure di sistema](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) creata a questo scopo oppure viene incluso tramite wrapping in una stored procedure definita dall'utente. Lo script può essere scritto in R, Python o in linguaggi esterni, ad esempio Java o .NET. In alternativa, possono essere presenti modelli già sottoposti a training e archiviati in un formato binario in una tabella di database, che possono essere chiamati in una funzione T-SQL [PREDICT](../../t-sql/queries/predict-transact-sql.md).

Poiché lo script viene fornito tramite oggetti dello schema del database esistenti, stored procedure e tabelle, non esistono nuovi [oggetti a protezione diretta](../../relational-databases/security/securables.md) per Machine Learning Services per SQL Server.

Indipendente dal modo in cui gli script vengono usati o da cosa sono costituiti, vengono creati e probabilmente salvati oggetti di database, ma non viene introdotto alcun nuovo tipo di oggetto per l'archiviazione dello script. Di conseguenza, la capacità di utilizzare, creare e salvare oggetti di database dipende in buona parte dalle autorizzazioni del database già definite per gli utenti.

<a name="permissions"></a>

## <a name="permissions"></a>Autorizzazioni

Il modello di sicurezza dei dati di SQL Server degli account di accesso e dei ruoli del database si estende allo script esterno. È necessario un account di accesso SQL Server o un account utente Windows per eseguire script esterni che usano dati di SQL Server o che vengono eseguiti con SQL Server come contesto di calcolo. Gli utenti di database che hanno le autorizzazioni necessarie per eseguire una query possono accedere agli stessi dati dallo script esterno.

L'account di accesso o l'account utente identifica l'*entità di sicurezza*, che può necessitare di più livelli di accesso, a seconda dei requisiti dello script esterno:

+ Autorizzazione di accesso al database in cui sono abilitati script esterni.
+ Autorizzazioni di lettura dei dati da oggetti protetti, ad esempio tabelle.
+ Capacità di scrivere nuovi dati in una tabella, ad esempio un modello, o i risultati di assegnazione dei punteggi.
+ Capacità di creare nuovi oggetti, ad esempio tabelle, stored procedure che usano lo script esterno o funzioni personalizzate che usano il processo dello script esterno.
+ Diritto di installare nuovi pacchetti nel computer SQL Server o usare i pacchetti forniti a un gruppo di utenti.

Ogni persona che esegue uno script esterno tramite SQL Server come contesto di esecuzione deve essere mappata a un utente nel database. Invece di impostare singolarmente autorizzazioni utente del database, è possibile creare ruoli per gestire i set di autorizzazioni e assegnare gli utenti ai ruoli.

Per altre informazioni, vedere [Concedere agli utenti l'autorizzazione per Machine Learning Services per SQL Server](../../machine-learning/security/user-permission.md).

## <a name="permissions-when-using-an-external-client-tool"></a>Autorizzazioni per l'uso di uno strumento client esterno

Per gli utenti che usano lo script in uno strumento client esterno, l'account utente o di accesso deve essere mappato a un utente nel database se questi utenti devono usare uno script esterno nel database o accedere a oggetti e dati del database. Sono necessarie le stesse autorizzazioni indipendentemente dal fatto che lo script esterno venga inviato da un client di data science remoto o eseguito tramite una stored procedure T-SQL.

Ad esempio, si supponga di aver creato uno script esterno eseguito nel computer locale e di volerlo eseguire in SQL Server. È necessario assicurarsi che siano soddisfatte le condizioni seguenti:

+ Il database consente le connessioni remote.
+ L'account di accesso SQL o l'account Windows usato per l'accesso al database è stato aggiunto a SQL Server a livello di istanza.
+ L'account di accesso SQL o l'utente Windows deve avere l'autorizzazione necessaria per eseguire script esterni. In genere, questa autorizzazione può essere aggiunta solo da un amministratore del database.
+ L'account di accesso SQL o l'utente Windows deve essere aggiunto come utente, con le autorizzazioni appropriate, in ogni database in cui lo script esterno esegue una qualsiasi di queste operazioni:
  + Recupero di dati.
  + Scrittura o aggiornamento di dati.
  + Creazione di nuovi oggetti, ad esempio tabelle o stored procedure.

Dopo avere effettuato il provisioning dell'account di accesso o dell'account utente Windows e avere concesso le autorizzazioni necessarie, è possibile eseguire uno script esterno in SQL Server usando un oggetto origine dati in R o la libreria **revoscalepy** in Python oppure chiamando una stored procedure contenente lo script esterno.

Ogni volta che viene avviato uno script esterno da SQL Server, la sicurezza del motore di database ottiene il contesto di protezione dell'utente che ha avviato il processo e gestisce i mapping dell'utente o dell'account di accesso a oggetti a protezione diretta.

Di conseguenza, tutti gli script esterni avviati da un client remoto devono specificare le informazioni sull'account di accesso o sull'utente all'interno della stringa di connessione.

<a name="launchpad"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="services-used-in-external-processing-launchpad"></a>Servizi usati nell'elaborazione esterna (launchpad)

Il framework di estendibilità aggiunge un nuovo servizio NT all'[elenco dei servizi](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details) in un'installazione di SQL Server: [**Launchpad di SQL Server (MSSSQLSERVER)**](extensibility-framework.md#launchpad).

Il motore di database usa il servizio **Launchpad** di SQL Server per creare un'istanza di una sessione di script esterno come processo separato. 
Il processo viene eseguito con un account con privilegi limitati. Questo account è diverso da SQL Server, dal launchpad stesso e dall'identità utente con cui è stata eseguita la stored procedure o la query host. L'esecuzione dello script in un processo separato, con un account con privilegi limitati, è alla base del modello di sicurezza e di isolamento per gli script esterni in SQL Server.

SQL Server gestisce anche un mapping dell'identità dell'utente chiamante all'account di lavoro con privilegi limitati usato per avviare il processo satellite. In alcuni scenari, in cui uno script o il codice richiama SQL Server per dati e operazioni, SQL Server può gestire il trasferimento di identità in modo uniforme. Uno script che contiene istruzioni SELECT o che chiama funzioni e altri oggetti di programmazione riesce in genere se l'utente chiamante ha autorizzazioni sufficienti.

> [!NOTE]
> Per impostazione predefinita, [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] è configurato per l'esecuzione in **NT Service\MSSQLLaunchpad**, di cui viene effettuato il provisioning con tutte le autorizzazioni necessarie per l'esecuzione di script esterni. Per altre informazioni sulle opzioni configurabili, vedere [Configurazione del servizio Launchpad di SQL Server](../security/sql-server-launchpad-service-account.md).

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="services-used-in-external-processing-launchpad"></a>Servizi usati nell'elaborazione esterna (launchpad)

Il framework di estendibilità aggiunge un nuovo servizio NT all'[elenco dei servizi](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details) in un'installazione di SQL Server: [**Launchpad di SQL Server (MSSSQLSERVER)**](extensibility-framework.md#launchpad).

Il motore di database usa il servizio **Launchpad** di SQL Server per creare un'istanza di una sessione di script esterno come processo separato. 
Il processo viene eseguito con l'identità utente del launchpad ma con la restrizione aggiuntiva di essere contenuto all'interno di un AppContainer. L'esecuzione dello script in un processo separato, con AppContainer, è alla base del modello di sicurezza e di isolamento per gli script esterni in SQL Server.

SQL Server gestisce anche un mapping dell'identità dell'utente chiamante all'account di lavoro con privilegi limitati usato per avviare il processo satellite. In alcuni scenari, in cui uno script o il codice richiama SQL Server per dati e operazioni, SQL Server può gestire il trasferimento di identità in modo uniforme. Uno script che contiene istruzioni SELECT o che chiama funzioni e altri oggetti di programmazione riesce in genere se l'utente chiamante ha autorizzazioni sufficienti.

> [!NOTE]
> Per impostazione predefinita, [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] è configurato per l'esecuzione in **NT Service\MSSQLLaunchpad**, di cui viene effettuato il provisioning con tutte le autorizzazioni necessarie per l'esecuzione di script esterni. Per altre informazioni sulle opzioni configurabili, vedere [Configurazione del servizio Launchpad di SQL Server](../security/sql-server-launchpad-service-account.md).

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="services-used-in-external-processing"></a>Servizi usati nell'elaborazione esterna

Il framework di estendibilità aggiunge un nuovo daemon in un'installazione di SQL Server: [mssql-launchpadd](extensibility-framework.md#launchpad). mssql-launchpadd viene eseguito con l'account con privilegi limitati mssql_launchpadd creato quando viene installato il pacchetto mssql-server-extensibility.

È supportata una sola istanza del motore di database e all'istanza è associato un solo servizio launchpad. Quando viene eseguito uno script, il servizio launchpad avvia un processo launchpad separato con l'account utente mssql_satellite che dispone di privilegi limitati con nuovo PID, IPC, montaggio e spazio dei nomi di rete corrispondenti. Ogni processo satellite eredita l'account utente mssql_satellite di launchpad e usa tale account per la durata dell'esecuzione dello script.

Per altre informazioni, vedere [Architettura di estendibilità in SQL Server Machine Learning Services](extensibility-framework.md).

::: moniker-end

<a name="sqlrusergroup"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="identities-used-in-processing-sqlrusergroup"></a>Identità usate nell'elaborazione (SQLRUserGroup)

**SQLRUserGroup** (gruppo di utenti SQL limitato) viene creato dal programma di installazione di SQL Server e contiene un pool di account utente Windows locali con privilegi limitati. Quando è necessario un processo esterno, launchpad usa un account di lavoro disponibile per l'esecuzione del processo. Più in particolare, launchpad attiva un account di lavoro disponibile, ne esegue il mapping all'identità dell'utente chiamante ed esegue lo script con l'account di lavoro.

+ **SQLRUserGroup** è collegato a un'istanza specifica. Per ogni istanza in cui è stato abilitato l'apprendimento automatico è necessario un pool di account di lavoro separato. Gli account non possono essere condivisi tra istanze.

+ La dimensione del pool di account utente è statica e il valore predefinito è 20, ovvero il pool supporta 20 sessioni simultaneamente. Il numero delle sessioni di runtime esterne che possono essere avviate simultaneamente è limitato dalla dimensione di questo pool di account utente. 

+ I nomi degli account di lavoro nel pool usano il formato NomeIstanzaSQL *nn*. Ad esempio, in un'istanza predefinita **SQLRUserGroup** contiene gli account denominati MSSQLSERVER01, MSSQLSERVER02 e così via fino a MSSQLSERVER20.

Le attività parallelizzate non usano account aggiuntivi. Ad esempio, se un utente esegue un'attività di assegnazione di punteggi che usa l'elaborazione parallela, viene riutilizzato lo stesso account di lavoro per tutti i thread. Se si intende fare un uso intenso dell'apprendimento automatico, è possibile aumentare il numero di account necessari per l'esecuzione di script esterni. Per altre informazioni, vedere [Ridimensionare l'esecuzione simultanea di script esterni in SQL Server Machine Learning Services](../../machine-learning/administration/scale-concurrent-execution-external-scripts.md).

### <a name="permissions-granted-to-sqlrusergroup"></a>Autorizzazioni concesse a SQLRUserGroup

Per impostazione predefinita, i membri di **SQLRUserGroup** hanno autorizzazioni di lettura ed esecuzione per i file nelle directory di SQL Server **Binn**, **R_SERVICES** e **PYTHON_SERVICES**. Questo include l'accesso a file eseguibili, librerie e set di dati predefiniti nelle distribuzioni R e Python installate con SQL Server. 

Per proteggere risorse sensibili in SQL Server, è facoltativamente possibile definire un elenco di controllo di accesso che nega l'accesso a **SQLRUserGroup**. Al contrario, è possibile concedere autorizzazioni per le risorse dati locali presenti nel computer host, oltre che SQL Server stesso. 

Per impostazione predefinita, **SQLRUserGroup** non ha un account di accesso al database o autorizzazioni per i dati. In alcuni casi, può essere necessario creare un account di accesso per consentire connessioni loopback, in particolare quando l'utente chiamante è un'identità Windows attendibile. Questo scenario è denominato [*autenticazione implicita*](#implied-authentication). Per altre informazioni, vedere [Aggiungere SQLRUserGroup come utente del database](../../machine-learning/security/create-a-login-for-sqlrusergroup.md).

## <a name="identity-mapping"></a>Mapping di identità

All'avvio di una sessione, launchpad esegue il mapping dell'identità dell'utente chiamante a un account di lavoro. Il mapping di un utente Windows esterno o di un account di accesso SQL valido a un account di lavoro è consentito solo per la durata della stored procedure SQL che esegue lo script esterno. Le query parallele dallo stesso account di accesso sono mappate allo stesso account utente di lavoro.

Durante l'esecuzione, launchpad crea cartelle temporanee per l'archiviazione dei dati di sessione, eliminandole al termine della sessione. Le directory sono ad accesso limitato. Per R, questa attività viene eseguita da RLauncher. Per Python, questa attività viene eseguita da PythonLauncher. Ogni singolo account di lavoro è limitato alla propria cartella e non può accedere a file in cartelle di livello superiore. Tuttavia, l'account di lavoro può leggere, scrivere o eliminare figli all'interno della cartella di lavoro della sessione creata. Un amministratore del computer può visualizzare le directory create per ogni processo. Ogni directory è identificata dal relativo GUID di sessione.

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="appcontainer-isolation"></a>Isolamento tramite AppContainer

L'isolamento viene ottenuto tramite [AppContainer](/windows/desktop/secauthz/appcontainer-isolation). In fase di esecuzione, quando viene rilevato uno script esterno in una stored procedure o una query, SQL Server chiama launchpad con una richiesta di un'utilità di avvio specifica dell'estensione. Launchpad richiama l'ambiente di runtime appropriato in un processo con la propria identità e crea un'istanza di un ambiente AppContainer per contenerlo. Questa modifica è utile perché la gestione degli account locali e delle password non è più necessaria. Inoltre, nelle installazioni in cui non sono consentiti account utente locali, l'eliminazione della dipendenza dall'account utente locale significa che è ora possibile usare questa funzionalità.

In base all'implementazione in SQL Server, gli ambienti AppContainer sono meccanismi interni. Anche se non si può avere una prova fisica degli ambienti AppContainer nel monitoraggio dei processi, questi sono presenti nelle regole del firewall in uscita create dal programma di installazione per impedire ai processi di eseguire chiamate di rete. [Configurazione del firewall per Machine Learning Services per SQL Server](../../machine-learning/security/firewall-configuration.md).

## <a name="identity-mapping"></a>Mapping di identità

All'avvio di una sessione, launchpad esegue il mapping dell'identità dell'utente chiamante a un **AppContainer**.

> [!Note]
> In SQL Server 2019 e versioni successive **SQLRUserGroup** include un solo membro, che è ora il singolo account del servizio Launchpad di SQL Server, anziché più account di lavoro.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="identity-mapping"></a>Mapping di identità

Il daemon **launchpadd** (due 'D' - [mssql-launchpadd](extensibility-framework.md#launchpad)) esegue il mapping dell'identità dell'utente chiamante a un processo **launchpad** separato (una sola 'D') con una cartella "launchpad GUID" e un certificato satellite. Queste cartelle GUID del launchpad vengono create in `/var/opt/mssql-extensibility/data/`. Il processo launchpad usa questo certificato per eseguire l'autenticazione in SQL e quindi crea le cartelle temporanee per ogni GUID di sessione nella cartella GUID del launchpad. Il processo satellite (R, Python o ExtHost) può accedere alla cartella GUID del launchpad, al certificato in essa incluso e alla relativa cartella GUID della sessione.

Lo script SQL seguente stampa il contenuto delle cartelle del launchpad.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    ,@script = N'
print("Contents of /var/opt/mssql-extensibility/data :");
print(system("ls -al /var/opt/mssql-extensibility/data"));
print("Contents of Launchpad GUID folder:");
print(system("ls -al /var/opt/mssql-extensibility/data/*"));
print(system("ls -al /var/opt/mssql-extensibility/data/*/*"))
'
    ,@input_data_1 = N'SELECT 1 AS hello'
```

::: moniker-end

<a name="implied-authentication"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="implied-authentication-loopback-requests"></a>Autenticazione implicita (richieste loopback)

L'*autenticazione implicita* descrive il comportamento delle richieste di connessione in cui i processi esterni in esecuzione come account di lavoro con privilegi limitati vengono presentati come identità utente attendibile a SQL Server in richieste loopback di dati o operazioni. Concettualmente l'autenticazione implicita è univoca per l'autenticazione di Windows, nelle stringhe di connessione di SQL Server che specificano una connessione trusted e in richieste provenienti da processi esterni come uno script R o Python. A volte è anche nota come *loopback*.

Le connessioni trusted possono essere usate da script esterni, ma solo con una configurazione aggiuntiva. Nell'architettura di estendibilità i processi esterni vengono eseguiti con account di lavoro ed ereditano le autorizzazioni dal gruppo **SQLRUserGroup** padre. Quando una stringa di connessione specifica `Trusted_Connection=True`, l'identità dell'account di lavoro viene presentata nella richiesta di connessione, che per impostazione predefinita è sconosciuta a SQL Server.

Perché le connessioni trusted riescano, è necessario creare un account di accesso al database per **SQLRUserGroup**. Una volta creato, tutte le connessioni trusted da qualsiasi membro di **SQLRUserGroup** avranno diritti di accesso a SQL Server. Per istruzioni dettagliate, vedere [Aggiungere SQLRUserGroup a un account di accesso al database](../../machine-learning/security/create-a-login-for-sqlrusergroup.md).

Le connessioni trusted sono la formulazione più diffusa di una richiesta di connessione. Quando lo script esterno specifica una connessione, può essere più comune usare un account di accesso SQL o un nome utente specificato completamente e una password se la connessione viene eseguita a un'origine dati ODBC.

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Funzionamento dell'autenticazione implicita per sessioni di script esterni

Il diagramma seguente mostra l'interazione dei componenti di SQL Server con il runtime del linguaggio e descrive come viene eseguita l'autenticazione implicita per Windows.

![Autenticazione implicita in Windows](../security/media/implied-auth-windows-2017.png)

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="implied-authentication-loopback-requests"></a>Autenticazione implicita (richieste loopback)

L'*autenticazione implicita* descrive il comportamento delle richieste di connessione in cui i processi esterni in esecuzione in AppContainer vengono presentati come identità utente attendibile a SQL Server in richieste loopback di dati o operazioni. Concettualmente l'autenticazione implicita non è più univoca per l'autenticazione di Windows, nelle stringhe di connessione di SQL Server che specificano una connessione trusted e in richieste provenienti da processi esterni come uno script R o Python. A volte è anche nota come *loopback*.

Grazie alla gestione di identità e credenziali, AppContainer impedisce l'uso di credenziali utente per ottenere l'accesso alle risorse o ad altri ambienti. L'ambiente AppContainer crea un identificatore che usa le identità combinate dell'utente e dell'applicazione, in modo che le credenziali siano univoche per ogni coppia utente/applicazione e che l'applicazione non possa rappresentare l'utente. Per altre informazioni, vedere [Isolamento tramite AppContainer](/windows/win32/secauthz/appcontainer-isolation).

Per altri dettagli sulle connessioni loopback, vedere [Connessione loopback a SQL Server da uno script Python o R](../connect/loopback-connection.md).

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Funzionamento dell'autenticazione implicita per sessioni di script esterni

Il diagramma seguente mostra l'interazione dei componenti di SQL Server con il runtime del linguaggio e descrive come viene eseguita l'autenticazione implicita per Windows.

![Autenticazione implicita in Windows](../security/media/implied-auth-windows.png)

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="implied-authentication-loopback-requests"></a>Autenticazione implicita (richieste loopback)

L'*autenticazione implicita* descrive il comportamento delle richieste di connessione in cui i processi esterni in esecuzione come utenti mssql_satellite con privilegi limitati nei rispettivi spazi dei nomi vengono presentati come identità utente attendibile a SQL Server in richieste loopback di dati o operazioni. A volte è anche nota come loopback.

Una connessione loopback viene stabilita usando il certificato satellite dalla cartella GUID del launchpad per eseguire l'autenticazione in SQL Server dal processo satellite. Viene eseguito il mapping dell'identità dell'utente chiamante a questo certificato e, di conseguenza, il processo satellite che si connette a SQL Server usando il certificato può essere a sua volta mappato all'utente chiamante.

Per altre informazioni, vedere, [Connessione loopback a SQL Server da uno script Python o R](../connect/loopback-connection.md).

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Funzionamento dell'autenticazione implicita per sessioni di script esterni

Il diagramma seguente mostra l'interazione dei componenti di SQL Server con il runtime del linguaggio e descrive come viene eseguita l'autenticazione implicita in Linux.

![Autenticazione implicita in Linux](../security/media/implied-auth-linux.png)

::: moniker-end

## <a name="no-support-for-transparent-data-encryption-at-rest"></a>Nessun supporto per Transparent Data Encryption con dati inattivi

La tecnologia [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) non è supportata per i dati inviati o ricevuti dal runtime dello script esterno. Il motivo è che il processo esterno viene eseguito esternamente al processo di SQL Server. Di conseguenza, i dati usati dal runtime esterno non vengono protetti dalla funzionalità di crittografia del motore di database. Questo comportamento non è diverso da quello di qualsiasi altro client in esecuzione nel computer SQL Server che legge dati dal database ed esegue una copia.

Di conseguenza, la tecnologia TDE **non** viene applicata ai dati usati in script esterni, né ai dati salvati su disco o ai risultati intermedi permanenti. Tuttavia, altri tipi di crittografia, ad esempio la crittografia unità BitLocker di Windows o la crittografia di terze parti applicata a livello di file o cartella, saranno comunque validi.

Nel caso di [Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md), i runtime esterni non hanno accesso alle chiavi di crittografia. Di conseguenza, i dati non possono essere inviati agli script.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo sono stati presentati i componenti e il modello di interazione dell'architettura di sicurezza integrata nel [framework di estendibilità](../../machine-learning/concepts/extensibility-framework.md). I concetti chiave presentati in questo articolo includono le finalità di launchpad, SQLRUserGroup e gli account di lavoro, l'isolamento dei processi di script esterni e il mapping delle identità utente ad account di lavoro. 

Come passaggio successivo, leggere le istruzioni per la [concessione delle autorizzazioni](../../machine-learning/security/user-permission.md). Per i server che usano l'autenticazione di Windows, vedere [Aggiungere SQLRUserGroup a un account di accesso al database](../../machine-learning/security/create-a-login-for-sqlrusergroup.md) per informazioni sui casi in cui sono necessarie attività di configurazione aggiuntive.