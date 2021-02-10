---
title: Installare in Linux
titleSuffix: SQL Server Machine Learning Services
description: 'Informazioni su come installare SQL Server Machine Learning Services (Python e R) in Linux: Red Hat, Ubuntu e SUSE.'
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 11/24/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 462857bd0a9aac7b63659bad809dcdc9a577f3b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031513"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-linux"></a>Installare SQL Server Machine Learning Services (Python e R) in Linux

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

Questo articolo illustra l'installazione di [SQL Server Machine Learning Services](../machine-learning//sql-server-machine-learning-services.md) in Linux. Gli script Python e R possono essere eseguiti nel database usando Machine Learning Services.

È possibile installare Machine Learning Services in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) e Ubuntu. Per altre informazioni, vedere [la sezione delle piattaforme supportate nelle linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> Machine Learning Services viene installato per impostazione predefinita nei cluster Big Data di SQL Server. Per altre informazioni, vedere [Usare Machine Learning Services (Python e R) in cluster Big Data](../big-data-cluster/machine-learning-services.md)

<a name="mro"></a>

## <a name="pre-install-checklist"></a>Elenco di controllo preliminare all'installazione

* [Installare SQL Server in Linux](sql-server-linux-setup.md) e verificare l'installazione.

* Controllare i repository di SQL Server Linux per le estensioni Python e R. 
  Se i repository di origine per l'installazione del motore di database sono già stati configurati, è possibile eseguire i comandi di installazione del pacchetto **mssql-mlservices** usando la stessa registrazione dei repository.

* (Solo R) Microsoft R Open (MRO) fornisce la distribuzione R di base per la funzionalità R in SQL Server e costituisce un prerequisito per l'uso di RevoScaleR, MicrosoftML e di altri pacchetti R installati con Machine Learning Services.
    * La versione richiesta è MRO 3.5.2.
    * Per installare MRO, scegliere uno dei due approcci seguenti:
        * Scaricare il file tarball di MRO da MRAN, decomprimerlo ed eseguire lo script install.sh. Se si sceglie questo approccio, è possibile seguire le [istruzioni di installazione nella pagina di MRAN](https://mran.microsoft.com/releases/3.5.2).
        * Registrare il repository **packages.microsoft.com**, come descritto di seguito per installare la distribuzione di MRO: microsoft-r-open-mro e microsoft-r-open-mkl. 
    * Vedere le sezioni relative all'installazione di seguito per informazioni su come installare MRO.

* È necessario avere uno strumento per eseguire i comandi T-SQL. 

  * È possibile usare [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), uno strumento di database gratuito eseguito in Linux, Windows e macOS.

## <a name="package-list"></a>Elenco di pacchetti

In un dispositivo connesso a Internet i pacchetti vengono scaricati e installati in modo indipendente dal motore di database usando il programma di installazione del pacchetto per ogni sistema operativo. La tabella seguente descrive tutti i pacchetti disponibili, ma per R e Python si specificano pacchetti che forniscono l'installazione completa delle funzionalità o l'installazione minima delle funzionalità.

Pacchetti di installazione disponibili:

| Nome del pacchetto | Applicabile a | Descrizione |
|--------------|----------|-------------|
|mssql-server-extensibility  | Tutti | Framework di estendibilità usato per eseguire Python e R. |
| microsoft-openmpi  | Python, R | Interfaccia per la trasmissione di messaggi usata dalle librerie Rev* per la parallelizzazione in Linux. |
| mssql-mlservices-python | Python | Distribuzione open source di Anaconda e Python. |
|mssql-mlservices-mlm-py  | Python | *Installazione completa*. Fornisce revoscalepy, microsoftml, modelli con training preliminare per la definizione di un immagine e l'analisi del sentiment del testo.| 
|mssql-mlservices-packages-py  | Python | *Installazione minima*. Fornisce revoscalepy e microsoftml. <br/>Esclude i modelli con training preliminare. | 
| [microsoft-r-open*](#mro) | R | Distribuzione open source di R, costituita da tre pacchetti. |
|mssql-mlservices-mlm-r  | R | *Installazione completa*. Fornisce: RevoScaleR, MicrosoftML, sqlRUtils, olapR, modelli con training preliminare per la definizione di un immagine e l'analisi del sentiment del testo.| 
|mssql-mlservices-packages-r  | R | *Installazione minima*. Fornisce RevoScaleR, sqlRUtils, MicrosoftML, olapR. <br/>Esclude i modelli con training preliminare. |

<a name="RHEL"></a>

## <a name="install-on-rhel"></a>Eseguire l'installazione in RHEL

Seguire questa procedura per installare SQL Server Machine Learning Services in Red Hat Enterprise Linux (RHEL).

### <a name="install-mro-on-rhel"></a>Installare MRO in RHEL

I comandi seguenti registrano il repository che fornisce MRO. Dopo la registrazione, i comandi per l'installazione di altri pacchetti R, ad esempio mssql-mlservices-mml-r, includeranno automaticamente MRO come dipendenza del pacchetto.
```bash
# Import the Microsoft repository key

sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Set the location of the package repo at the "prod" directory
# The following command is for version 7.x
# For 6.x, replace 7 with 6 to get that version
rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Update packages on your system (optional)
yum update
```

Opzioni di installazione per Python e R:

*  Installare il supporto della lingua in base ai propri requisiti (una o più lingue).
*  L'*installazione completa* include tutte le funzionalità disponibili, inclusi i modelli di Machine Learning con training preliminare.
*  L'*installazione minima* esclude i modelli ma include comunque tutte le funzionalità.

> [!Tip]
> Se possibile, eseguire `yum clean all` per aggiornare i pacchetti nel sistema prima dell'installazione.

### <a name="full-installation"></a>Installazione completa

Include:
*  Python open source
*  R open source
*  Framework di estendibilità
*  Microsoft-openmpi
*  Estensioni (Python, R)
*  Librerie di Machine Learning
*  Modelli con training preliminare per Python e R

```bash
# Install as root or sudo
# Add everything (all R, Python)
# Be sure to include -9.4.7* in mlsservices package names
sudo yum install mssql-mlservices-mlm-py-9.4.7*
sudo yum install mssql-mlservices-mlm-r-9.4.7*
```

### <a name="minimum-installation"></a>Installazione minima

Include:
*  Python open source
*  R open source
*  Framework di estendibilità
*  Microsoft-openmpi
*  Librerie Revo* principali
*  Librerie di Machine Learning

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
# Be sure to include -9.4.6* in mlsservices package names
sudo yum install mssql-mlservices-packages-py-9.4.7*
sudo yum install mssql-mlservices-packages-r-9.4.7*
```
<a name="ubuntu"></a>

## <a name="install-on-ubuntu"></a>Installare in Ubuntu

Seguire questa procedura per installare SQL Server Machine Learning Services in Ubuntu.

### <a name="install-mro-on-ubuntu"></a>Installare MRO in Ubuntu

I comandi seguenti registrano il repository che fornisce MRO. Dopo la registrazione, i comandi per l'installazione di altri pacchetti R, ad esempio mssql-mlservices-mml-r, includeranno automaticamente MRO come dipendenza del pacchetto.

```bash
# Install as root
sudo su

# Optionally, if your system does not have the https apt transport option
apt-get install apt-transport-https

# Set the location of the package repo the "prod" directory containing the distribution.
# This example specifies 16.04. Replace with 14.04 if you want that version
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb

# Register the repo
dpkg -i packages-microsoft-prod.deb

# Update packages on your system (required), including MRO installation
sudo apt-get update
```

Opzioni di installazione per Python e R:

*  Installare il supporto della lingua in base ai propri requisiti (una o più lingue).
*  L'*installazione completa* include tutte le funzionalità disponibili, inclusi i modelli di Machine Learning con training preliminare.
*  L'*installazione minima* esclude i modelli ma include comunque tutte le funzionalità.

> [!Tip]
> Se possibile, eseguire `apt-get update` per aggiornare i pacchetti nel sistema prima dell'installazione. 

### <a name="full-installation"></a>Installazione completa 

Include:
*  Python open source
*  R open source
*  Framework di estendibilità
*  Microsoft-openmpi
*  Estensioni Python
*  Estensioni R
*  Librerie di Machine Learning
*  Modelli con training preliminare per Python e R

```bash
# Install as root or sudo
# Add everything (all R, Python)
# There is no asterisk in this full install
sudo apt-get install mssql-mlservices-mlm-py 
sudo apt-get install mssql-mlservices-mlm-r 
```

### <a name="minimum-installation"></a>Installazione minima 

Include:
*  Python open source
*  R open source
*  Framework di estendibilità
*  Microsoft-openmpi
*  Librerie Revo* principali
*  Librerie di Machine Learning

```bash
# Install as root or sudo
# Minimum install of R, Python
# No asterisk
sudo apt-get install mssql-mlservices-packages-py
sudo apt-get install mssql-mlservices-packages-r
```

<a name="SLES"></a>

## <a name="install-on-sles"></a>Eseguire l'installazione in SLES

Seguire questa procedura per installare SQL Server Machine Learning Services in SUSE Linux Enterprise Server (SLES).

### <a name="install-mro-on-sles"></a>Installare MRO in SLES

I comandi seguenti registrano il repository che fornisce MRO. Dopo la registrazione, i comandi per l'installazione di altri pacchetti R, ad esempio mssql-mlservices-mml-r, includeranno automaticamente MRO come dipendenza del pacchetto.

```bash
# Install as root
sudo su

# Set the location of the package repo at the "prod" directory containing the distribution
# This example is for SLES12, the only supported version of SLES in Machine Learning Server
zypper ar -f https://packages.microsoft.com/sles/12/prod packages-microsoft-com

# Update packages on your system (optional)
zypper update
```

Opzioni di installazione per Python e R:

*  Installare il supporto della lingua in base ai propri requisiti (una o più lingue).
*  L'*installazione completa* include tutte le funzionalità disponibili, inclusi i modelli di Machine Learning con training preliminare.
*  L'*installazione minima* esclude i modelli ma include comunque tutte le funzionalità.

### <a name="full-installation"></a>Installazione completa 

Include:
*  Python open source
*  R open source
*  Framework di estendibilità
*  Microsoft-openmpi
*  Estensioni per Python e R
*  Librerie di Machine Learning
*  Modelli con training preliminare per Python e R

```bash
# Install as root or sudo
# Add everything (all R, Python)
sudo zypper install mssql-mlservices-mlm-py
sudo zypper install mssql-mlservices-mlm-r
```

### <a name="minimum-installation"></a>Installazione minima 

Include:
*  Python open source
*  R open source
*  Framework di estendibilità
*  Microsoft-openmpi
*  Librerie Revo* principali
*  Librerie di Machine Learning 

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
sudo zypper install mssql-mlservices-packages-py
sudo zypper install mssql-mlservices-packages-r
```

## <a name="post-install-config-required"></a>Configurazione successiva all'installazione (obbligatoria)

La configurazione aggiuntiva viene eseguita principalmente tramite lo [strumento mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. Al termine dell'installazione del pacchetto, eseguire mssql-conf setup e seguire le istruzioni per impostare la password dell'amministratore di sistema e scegliere l'edizione. Eseguire questo passaggio solo se non è stato ancora configurato SQL Server in Linux. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

2. Accettare i contratti di licenza per le estensioni R e Python open source. Usare il comando seguente:

   ```bash
   # Run as SUDO or root
   # Use set + EULA 
   sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y
   ```
   Il programma di installazione rileva i pacchetti mssql-mlservices e richiede l'accettazione del contratto di licenza con l'utente finale (se non è stato precedentemente accettato) quando si esegue `mssql-conf setup`. Per altre informazioni sui parametri delle condizioni di licenza, vedere [Configurare SQL Server con lo strumento mssql conf](sql-server-linux-configure-mssql-conf.md#mlservices-eula).

3. Abilitare l'accesso alla rete in uscita. L'accesso alla rete in uscita è disabilitato per impostazione predefinita. Per abilitare le richieste in uscita, impostare la proprietà booleana "outboundnetworkaccess" usando lo strumento mssql-conf. Per altre informazioni, vedere [Configurare SQL Server in Linux con mssql conf](sql-server-linux-configure-mssql-conf.md#mlservices-outbound-access).

   ```bash
   # Run as SUDO or root
   # Enable outbound requests over the network
   sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1
   ```

4. Per integrare solo le funzionalità di R, impostare la variabile di ambiente **MKL_CBWR** in modo da [garantire un output coerente](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) dai calcoli di Intel Math Kernel Library (MKL).

   + Modificare o creare un file `.bash_profile` nella home directory dell'utente, aggiungendo la riga `export MKL_CBWR="AUTO"` al file.

   + Eseguire questo file digitando `source .bash_profile` al prompt dei comandi di Bash.

5. Riavviare il servizio Launchpad di SQL Server e l'istanza del motore di database per leggere i valori aggiornati dal file INI. Quando viene modificata un'impostazione relativa all'estendibilità, viene visualizzato un messaggio di notifica.  

   ```bash
   systemctl restart mssql-launchpadd

   systemctl restart mssql-server.service
   ```

6. Abilitare l'esecuzione di script esterni usando Azure Data Studio o un altro strumento come SQL Server Management Studio (solo Windows) che esegue Transact-SQL.

   ```sql
   EXEC sp_configure 'external scripts enabled', 1 
   RECONFIGURE WITH OVERRIDE 
   ```

7. Riavviare nuovamente il servizio Launchpad.

## <a name="verify-installation"></a>Verificare l'installazione

Le librerie R (MicrosoftML, RevoScaleR e altre) sono disponibili in `/opt/mssql/mlservices/libraries/RServer`.

Le librerie Python (microsoftml e revoscalepy) sono disponibili in `/opt/mssql/mlservices/libraries/PythonServer`.

Per convalidare l'installazione:

* Eseguire uno script T-SQL che esegue un stored procedure di sistema richiamando Python o R con uno strumento di query. 

* Eseguire il comando SQL seguente per testare l'esecuzione di R in SQL Server. Errori? Provare a riavviare il servizio, `sudo systemctl restart mssql-server.service`.
  ```sql
  EXEC sp_execute_external_script   
  @language =N'R', 
  @script=N' 
  OutputDataSet <- InputDataSet', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```
 
* Eseguire il comando SQL seguente per testare l'esecuzione di Python in SQL Server. 
 
  ```sql
  EXEC sp_execute_external_script  
  @language =N'Python', 
  @script=N' 
  OutputDataSet = InputDataSet; 
  ', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```

<a name="install-all"></a>

## <a name="unattended-installation"></a>Installazione automatica

Usando l'[installazione automatica](sql-server-linux-setup.md#unattended) per il motore di database, aggiungere i pacchetti per mssql-mlservices e le condizioni di licenza.

 Usare uno dei parametri EULA specifici di mlservices per le distribuzioni R e Python open source:

```bash
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml
```

Il contratto di licenza con l'utente finale completo è documentato in [Configurare SQL Server in Linux con lo strumento mssql conf](sql-server-linux-configure-mssql-conf.md#mlservices-eula).

## <a name="offline-installation"></a>Installazione offline

Per la procedura di installazione dei pacchetti, seguire le istruzioni contenute in [Installazione offline](sql-server-linux-setup.md#offline). Trovare il sito di download e quindi scaricare i pacchetti specifici usando l'elenco di pacchetti riportato di seguito.

> [!Tip]
> Molti degli strumenti di gestione dei pacchetti forniscono comandi che consentono di determinare le dipendenze dei pacchetti. Per yum, usare `sudo yum deplist [package]`. Per Ubuntu, usare `sudo apt-get install --reinstall --download-only [package name]` seguito da `dpkg -I [package name].deb`.

 
### <a name="download-site"></a>Sito di download

Scaricare i pacchetti da [https://packages.microsoft.com/](https://packages.microsoft.com/). Tutti i pacchetti mlservices per Python e R hanno un percorso condiviso con il pacchetto del motore di database. La versione di base per i pacchetti mlservices è la 9.4.6. Si ricordi che i pacchetti microsoft-r-open si trovano in un [repository diverso](#mro).

### <a name="rhel7-paths"></a>Percorsi RHEL/7

|Pacchetto|Percorso download|
|--|----|
| Pacchetti mssql/mlservices | [https://packages.microsoft.com/rhel/7/mssql-server-2019/](https://packages.microsoft.com/rhel/7/mssql-server-2019/) |
| Pacchetti microsoft-r-open | [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/) | 

### <a name="ubuntu1604-paths"></a>Percorsi Ubuntu/16.04

|Pacchetto|Percorso download|
|--|----|
| Pacchetti mssql/mlservices | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/) |
| Pacchetti microsoft-r-open | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/) | 

### <a name="sles12-paths"></a>Percorsi SLES/12

|Pacchetto|Percorso download|
|--|----|
| Pacchetti mssql/mlservices | [https://packages.microsoft.com/sles/12/mssql-server-2019/](https://packages.microsoft.com/sles/12/mssql-server-2019/) |
| Pacchetti microsoft-r-open | [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/) | 

Selezionare le estensioni che si vogliono usare, scaricare i pacchetti necessari per una linguaggio specifico. I nomi dei file includono le informazioni sulla piattaforma nel suffisso.

### <a name="package-list"></a>Elenco di pacchetti

A seconda delle estensioni che si vogliono usare, scaricare i pacchetti necessari per una linguaggio specifico. I nomi dei file esatti includono le informazioni sulla piattaforma nel suffisso, ma i nomi dei file riportati di seguito dovrebbero essere sufficienti per determinare quali file ottenere.

```
# Core packages 
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# R
microsoft-openmpi-3.0.0
microsoft-r-open-mkl-3.5.2
microsoft-r-open-mro-3.5.2
mssql-mlservices-packages-r-9.4.7.64
mssql-mlservices-mlm-r-9.4.7.64


# Python
microsoft-openmpi-3.0.0
mssql-mlservices-python-9.4.7.64
mssql-mlservices-packages-py-9.4.7.64
mssql-mlservices-mlm-py-9.4.7.64
```

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori Python possono apprendere come usare Python con SQL Server seguendo queste esercitazioni:

+ [Esercitazione su Python: Stimare il noleggio di sci con la regressione lineare in Machine Learning Services per SQL Server](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Esercitazione su Python: Categorizzazione dei clienti tramite clustering K-Means con Machine Learning Services per SQL Server](../machine-learning/tutorials/python-clustering-model.md)

Gli sviluppatori R possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di R con SQL Server. Per il passaggio successivo, vedere i collegamenti seguenti:

+ [Avvio rapido: Eseguire R in T-SQL](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [Esercitazione: Analisi nel database per sviluppatori R](../machine-learning/tutorials/r-taxi-classification-introduction.md)
