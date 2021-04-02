---
title: Raccogliere i dati per la risoluzione dei problemi di Python e R
description: Informazioni su come raccogliere i dati necessari quando si tenta di risolvere i problemi in SQL Server Machine Learning Services autonomamente o con l'assistenza del supporto tecnico Microsoft.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/31/2021
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 8744bc74846760da36c242dd2bb37d0b78ff3e00
ms.sourcegitcommit: 2db7412d30722f198cbafcd683bd4da206b33996
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106099950"
---
# <a name="collect-data-to-troubleshoot-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Raccogliere i dati per risolvere i problemi degli script Python e R con SQL Server Machine Learning Services

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo descrive come raccogliere i dati necessari quando si sta provando a risolvere i problemi in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Questi dati possono essere utili sia se si cerca di risolvere i problemi in modo autonomo che se ci si rivolge al supporto tecnico Microsoft.

## <a name="sql-server-version-and-edition"></a>Versione ed edizione di SQL Server

R Services per SQL Server 2016 è la prima versione di SQL Server che include il supporto R integrato. SQL Server 2016 Service Pack 1 (SP1) include diversi miglioramenti importanti, tra cui la possibilità di eseguire script esterni. Se si usa SQL Server 2016, è necessario prendere in considerazione l'installazione di SP1 o versione successiva.

SQL Server 2017 e versioni successive dispongono dell'integrazione con il linguaggio Python. Non è possibile ottenere l'integrazione delle funzionalità Python nelle versioni precedenti.

Per altre informazioni su come ottenere l'edizione e le versioni richieste, vedere questo articolo in cui sono elencati i numeri di build per ciascuna delle [versioni di SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/783.sql-server-versions.aspx#Service_Pack_editions).

A seconda dell'edizione di SQL Server in uso, alcune funzionalità di Machine Learning potrebbero non essere disponibili o essere limitate. 

## <a name="r-language-and-tool-versions"></a>Versioni degli strumenti e del linguaggio R

In generale, la versione di Microsoft R installata quando si seleziona la funzionalità di R Services o la caratteristica di Machine Learning Services è determinata dal numero di build di SQL Server. Se si esegue l'aggiornamento o si applica una patch a SQL Server, è necessario aggiornare o applicare una patch anche ai relativi componenti R.

Per un elenco delle versioni e dei collegamenti ai download dei componenti R, vedere [Installare i componenti di Machine Learning senza accesso a Internet](../install/sql-ml-component-install-without-internet-access.md). Nei computer con accesso a Internet la versione richiesta di R viene identificata e installata automaticamente.

È possibile aggiornare i componenti di R Server separatamente dal motore di database di SQL Server, in un processo noto come binding. Pertanto, la versione di R usata quando si esegue il codice R in SQL Server potrebbe variare a seconda della versione installata di SQL Server e della migrazione del server alla versione di R più recente.

### <a name="determine-the-r-version"></a>Determinare la versione di R

Il modo più semplice per determinare la versione di R è ottenere le proprietà di runtime eseguendo un'istruzione come la seguente:

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
       , @script = N'
# Transform R version properties to data.frame
OutputDataSet <- data.frame(
  property_name = c("R.version", "Revo.version"),
  property_value = c(R.Version()$version.string, Revo.version$version.string),
  stringsAsFactors = FALSE)
# Retrieve properties like R.home, libPath & default packages
OutputDataSet <- rbind(OutputDataSet, data.frame(
  property_name = c("R.home", "libPaths", "defaultPackages"),
  property_value = c(R.home(), .libPaths(), paste(getOption("defaultPackages"), collapse=", ")),
  stringsAsFactors = FALSE)
)
'
WITH RESULT SETS ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));
```

> [!TIP]
> Se R Services non funziona, provare a eseguire solo la parte dello script R da RGui.

Come ultima risorsa, è possibile aprire i file nel server per determinare la versione installata. A tale scopo, individuare il file rlauncher.config per ottenere il percorso del runtime di R e della directory di lavoro corrente. Si consiglia di creare e aprire una copia del file in modo da non modificarne accidentalmente le proprietà.

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name\MSSQL\Binn\rlauncher.config`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Binn\rlauncher.config`

Per ottenere le versioni di R e RevoScaleR, aprire un prompt dei comandi di R o aprire l'RGui associato all'istanza.

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instancename>\R_SERVICES\bin\x64\RGui.exe`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES\bin\x64\RGui.exe`

La console R visualizza le informazioni sulla versione all'avvio. Ad esempio, la versione seguente rappresenta la configurazione predefinita per SQL Server 2017:

```console
*Microsoft R Open 3.3.3*

*The enhanced R distribution from Microsoft*

*Microsoft packages Copyright (C) 2017 Microsoft*

*Loading Microsoft R Server packages, version 9.1.0.*
```

## <a name="python-versions"></a>Versioni di Python

Per ottenere la versione di Python, è possibile procedere in diversi modi. Il modo più semplice consiste nell'eseguire questa istruzione da Management Studio o da qualsiasi altro strumento di query SQL:

```sql
-- Get Python runtime properties:
exec sp_execute_external_script
       @language = N'Python'
       , @script = N'
import sys
import pkg_resources
OutputDataSet = pandas.DataFrame(
                    {"property_name": ["Python.home", "Python.version", "Revo.version", "libpaths"],
                    "property_value": [sys.executable[:-10], sys.version, pkg_resources.get_distribution("revoscalepy").version, str(sys.path)]}
)
'
with WITH RESULT SETS (SQL keywords) ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));
```

Se Machine Learning Services non è in esecuzione, è possibile determinare la versione di Python installata esaminando il file pythonlauncher.config. Si consiglia di creare e aprire una copia del file in modo da non modificarne accidentalmente le proprietà.

1. Solo per SQL Server 2017: `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog\pythonlauncher.config`
2. Ottenere il valore per **PYTHONHOME**.
3. Ottenere il valore della directory di lavoro corrente.

> [!NOTE]
> Se sono stati installati sia Python che R in SQL Server 2017, la directory di lavoro e il pool di account di lavoro vengono condivisi per i linguaggi R e Python.

## <a name="are-multiple-instances-of-r-or-python-installed"></a>Sono installate più istanze di R o Python?

Verificare se nel computer è installata più di una copia delle librerie R. Questa duplicazione può verificarsi se:

* Durante l'installazione si selezionano sia R Services (In-Database) che R Server (Standalone).
* Si installa Microsoft R Client oltre a SQL Server.
* È stato installato un set diverso di librerie R usando R Tools per Visual Studio, R Studio, Microsoft R Client o un altro IDE R.
* Il computer ospita più istanze di SQL Server e più di un'istanza usa Machine Learning.

Le stesse condizioni si applicano a Python.

Se si ritiene che siano installati più runtime o librerie, assicurarsi di ottenere solo gli errori associati ai runtime di Python o R usati dall'istanza di SQL Server.

## <a name="origin-of-errors"></a>Origine degli errori

Gli errori che vengono visualizzati quando si tenta di eseguire il codice R possono provenire da una qualsiasi delle origini seguenti:

* Motore di database di SQL Server, inclusa la stored procedure sp_execute_external_script
* Launchpad attendibile di SQL Server
* Altri componenti del framework di estendibilità, inclusi i processi satellite e le utilità di avvio di R e Python
* Provider, quali Microsoft Open Database Connectivity (ODBC)
* Linguaggio R

Quando si usa il servizio per la prima volta, può essere difficile stabilire quali messaggi provengono da quali servizi. Si consiglia di acquisire non solo il testo esatto del messaggio, ma il contesto in cui è stato visualizzato il messaggio. Esaminare il software client usato per eseguire il codice di Machine Learning:

* Si sta usando Management Studio? Un'applicazione esterna?
* Si sta eseguendo il codice R in un client remoto o direttamente in una stored procedure?

## <a name="sql-server-log-files"></a>File di log di SQL Server

Ottenere il log ERRORLOG di SQL Server più recente. Il set completo di log degli errori è costituito dai file della directory di log predefinita seguente:

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\Log\ExtensibilityLog`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.SQL2016\MSSQL\Log\ExtensibilityLog`

> [!NOTE]
> Il nome esatto della cartella è diverso a seconda del nome dell'istanza.

## <a name="errors-returned-by-sp_execute_external_script"></a>Errori restituiti da sp_execute_external_script

Ottenere il testo completo degli eventuali errori restituiti, quando si esegue il comando sp_execute_external_script.

Per non prendere in considerazione i problemi relativi a R o a Python, è possibile eseguire questo script, che avvia il runtime di R o Python e passa i dati avanti e indietro.

**Per R**

```sql
exec sp_execute_external_script @language =N'R',  
@script=N'OutputDataSet<-InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

**Per Python**

```sql
exec sp_execute_external_script @language =N'Python',  
@script=N'OutputDataSet= InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

## <a name="errors-generated-by-the-extensibility-framework"></a>Errori generati dal framework di estendibilità

SQL Server genera log distinti per i runtime dei linguaggi di script esterni. Questi errori non vengono generati dal linguaggio Python o R, ma dai componenti di estendibilità in SQL Server, tra cui le utilità di avvio specifiche del linguaggio e i relativi processi satellite.

È possibile ottenere questi log dai percorsi predefiniti seguenti:

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\MSSQL\Log\ExtensibilityLog`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog`

> [!NOTE]
> Il nome esatto della cartella è diverso in base al nome dell'istanza. A seconda della configurazione, la cartella potrebbe trovarsi in un'unità diversa.

Ad esempio, i messaggi di log seguenti sono relativi al framework di estendibilità:

* *LogonUser non riuscito per l'utente MSSQLSERVER01*
  
  Questo messaggio potrebbe indicare che gli account di lavoro che eseguono script esterni non possono accedere all'istanza.

* *InitializePhysicalUsersPool non riuscito*
  
  Questo messaggio potrebbe indicare che le impostazioni di sicurezza impediscono al programma di installazione di creare il pool di account di lavoro necessari per eseguire gli script esterni.

* *L'inizializzazione di gestione contesto di protezione non è riuscita*

* *L'inizializzazione di gestione sessione satellite non è riuscita*

## <a name="system-events"></a>Eventi di sistema

1. Aprire il Visualizzatore eventi di Windows e cercare nel registro **Evento di sistema** i messaggi che includono la stringa *Launchpad*.
2. Aprire il file ExtLaunchErrorlog e cercare la stringa *ErrorCode*. Esaminare il messaggio associato a ErrorCode.

I messaggi seguenti, ad esempio, sono errori di sistema comuni correlati al framework di estendibilità di SQL Server:

* *Il servizio Launchpad di SQL Server (MSSQLSERVER) non è stato avviato per il seguente errore: <text>*

* *Il servizio non ha risposto in tempo utile alla richiesta di avvio o di controllo.*

* *Timeout (120000 millisecondi) durante l'attesa della connessione del servizio Launchpad di SQL Server (MSSQLSERVER).*

## <a name="dump-files"></a>File di dump

Se si ha una conoscenza approfondita delle procedure di debug, è possibile usare i file di dump per analizzare un errore in Launchpad.

1. Individuare la cartella contenente i log bootstrap del programma di installazione per SQL Server. Ad esempio, in SQL Server 2016 il percorso predefinito è C:\Programmi\Microsoft SQL Server\130\Setup Bootstrap\Log.
2. Aprire la sottocartella dei log bootstrap specifica per l'estendibilità.
3. Se è necessario inviare una richiesta di supporto, aggiungere l'intero contenuto di questa cartella in un file compresso. Ad esempio, C:\Programmi\Microsoft SQL Server\130\Setup Bootstrap\Log\LOG\ExtensibilityLog.
  
Il percorso esatto potrebbe variare a seconda del sistema in uso e potrebbe trovarsi in un'unità diversa dall'unità C. Assicurarsi di ottenere i log per l'istanza in cui è installato Machine Learning.

## <a name="configuration-settings"></a>Impostazioni di configurazione

Questa sezione elenca i provider o i componenti aggiuntivi che possono costituire una fonte di errore durante l'esecuzione di script R o Python.

### <a name="what-network-protocols-are-available"></a>Quali sono i protocolli di rete disponibili?

Machine Learning Services richiede i protocolli di rete seguenti per la comunicazione interna tra i componenti di estendibilità e per la comunicazione con i client R o Python esterni.

* Named Pipes
* TCP/IP

Aprire Gestione configurazione SQL Server per determinare se un protocollo è installato e, in caso affermativo, per determinare se è abilitato.

### <a name="security-configuration-and-permissions"></a>Configurazione della sicurezza e autorizzazioni

Per gli account di lavoro:

1. Nel Pannello di controllo aprire **Utenti e gruppi** e individuare il gruppo usato per eseguire i processi di script esterni. Per impostazione predefinita, il gruppo è **SQLRUserGroup**.
2. Verificare che il gruppo esista e che contenga almeno un account di lavoro.
3. In SQL Server Management Studio selezionare l'istanza in cui verranno eseguiti i processi di R o Python, selezionare **Sicurezza** e quindi determinare se è presente un account di accesso per SQLRUserGroup.
4. Verificare le autorizzazioni per il gruppo di utenti.

Per gli account utente singoli:

1. Determinare se l'istanza supporta l'autenticazione in modalità mista, solo gli account di accesso SQL o solo l'autenticazione di Windows. Questa impostazione ha effetto sui requisiti del codice R o Python.
2. Per ogni utente che deve eseguire codice R, determinare il livello di autorizzazioni richiesto per ogni database in cui gli oggetti verranno scritti da R, sarà possibile accedere ai dati o verranno creati oggetti.
3. Per abilitare l'esecuzione di script, creare ruoli o aggiungere utenti ai ruoli seguenti, se necessario:

   - Tutti ad eccezione di *db_owner*: richiedono EXECUTE ANY EXTERNAL SCRIPT.
   - *db_datawriter*: per scrivere risultati da R o Python.
   - *db_ddladmin*: per creare nuovi oggetti.
   - *db_datareader*: per leggere i dati usati dal codice R o Python.
4. Si noti se sono stati modificati gli account di avvio predefiniti quando è stato installato SQL Server 2016.
5. Se un utente deve installare nuovi pacchetti R o usare pacchetti R installati da altri utenti, potrebbe essere necessario abilitare la gestione dei pacchetti nell'istanza e quindi assegnare autorizzazioni aggiuntive. 

### <a name="what-folders-are-subject-to-locking-by-antivirus-software"></a>Quali cartelle sono soggette al blocco da parte del software antivirus?

Il software antivirus può bloccare le cartelle, impedendo la configurazione delle caratteristiche di Machine Learning e la corretta esecuzione degli script. Determinare se le cartelle nell'albero di SQL Server sono soggette alla scansione antivirus.

Tuttavia, quando si installano più servizi o caratteristiche in un'istanza, può essere difficile enumerare tutte le possibili cartelle usate dall'istanza. Ad esempio, quando vengono aggiunte nuove caratteristiche, è necessario identificare ed escludere le nuove cartelle.

Inoltre, alcune caratteristiche creano nuove cartelle in modo dinamico in fase di runtime. Ad esempio, le tabelle OLTP in memoria, le stored procedure e le funzioni creano tutte nuove directory in fase di runtime. Questi nomi di cartelle contengono spesso GUID e non possono essere previsti. Il Launchpad attendibile di SQL Server crea nuove directory di lavoro per i processi di script di R e Python.

Dal momento che potrebbe essere impossibile escludere tutte le cartelle necessarie per il processo di SQL Server e le relative funzionalità, è consigliabile escludere l'intero albero di directory dell'istanza di SQL Server.

### <a name="is-the-firewall-open-for-sql-server-does-the-instance-support-remote-connections"></a>Il firewall è aperto per SQL Server? L'istanza supporta le connessioni remote?

1. Per determinare se SQL Server supporta le connessioni remote, vedere [Configurare le connessioni al server remoto](../../database-engine/configure-windows/view-or-configure-remote-server-connection-options-sql-server.md).

2. Determinare se è stata creata una regola del firewall per SQL Server. Per motivi di sicurezza, in un'installazione predefinita il client R o Python remoto potrebbe non essere in grado di connettersi all'istanza. Per altre informazioni, vedere [Risoluzione dei problemi di connessione a SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).

## <a name="see-also"></a>Vedere anche

[Risolvere i problemi di Machine Learning in SQL Server](common-issues-external-script-execution.md)