---
title: Linee guida per l'installazione di SQL Server in Linux
titleSuffix: SQL Server
description: Installare, aggiornare e disinstallare SQL Server in Linux. Questo articolo illustra gli scenari online, offline e automatici.
author: VanMSFT
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: 01417fa9f7f790229e073964133396f87f144d45
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465163"
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Linee guida per l'installazione di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo offre linee guida per l'installazione, l'aggiornamento e la disinstallazione di SQL Server 2017 e SQL Server 2019 in Linux.

Per altri scenari di distribuzione, vedere:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Contenitori Docker](./sql-server-linux-docker-container-deployment.md)
- [Kubernetes - Cluster Big Data](../big-data-cluster/deploy-get-started.md)

> [!TIP]
> Questa guida tratta diversi scenari di distribuzione. Se sono necessarie solo le istruzioni dettagliate per l'installazione, passare a uno degli argomenti di avvio rapido:
> - [Avvio rapido in RHEL](quickstart-install-connect-red-hat.md)
> - [Avvio rapido in SLES](quickstart-install-connect-suse.md)
> - [Avvio rapido in Ubuntu](quickstart-install-connect-ubuntu.md)
> - [Avvio rapido in Docker](quickstart-install-connect-docker.md)

Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](../linux/sql-server-linux-faq.yml).

## <a name="supported-platforms"></a><a id="supportedplatforms"></a> Piattaforme supportate

SQL Server è supportato in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) e Ubuntu. È anche supportato come immagine Docker, che può essere eseguita in Docker Engine in Linux o in Docker per Windows/Mac.

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Piattaforma | Versioni supportate | Recupero
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3, 7.4, 7.5, 7.6, 8  | [Scaricare RHEL 7.6](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 SP2 | [Scaricare SLES v12 SP2](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [Scaricare Ubuntu 16.04](http://releases.ubuntu.com/xenial/)
| **Docker Engine** | 1.8+ | [Scaricare Docker](https://www.docker.com/get-started)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

::: moniker-end

Microsoft supporta anche la distribuzione e la gestione di contenitori di SQL Server tramite OpenShift e Kubernetes.

> [!NOTE]
> SQL Server è testato e supportato in Linux per le distribuzioni elencate in precedenza. Se si sceglie di installare SQL Server in un sistema operativo non supportato, vedere la sezione **Criteri di supporto** di [Criteri di supporto tecnico per Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) per comprendere le implicazioni per il supporto.

## <a name="system-requirements"></a><a id="system"></a> Requisiti di sistema

SQL Server ha i requisiti di sistema seguenti per Linux:

|||
|-----|-----|
| **Memoria** | 2 GB |
| **File system** | **XFS** o **EXT4** (sono supportati altri file system, ad esempio **BTRFS**) |
| **Spazio su disco** | 6 GB |
| **Velocità del processore** | 2 GHz |
| **Core del processore** | 2 core |
| **Tipo di processore** | Solo compatibile con x64 |

Se si usano condivisioni di rete **NFS (Network File System, file system di rete)** nell'ambiente di produzione, tenere presenti i requisiti di supporto seguenti:

- Usare NFS versione **4.2 o successiva**. Le versioni precedenti di NFS non supportano le funzionalità necessarie, ad esempio fallocate e la creazione di file sparse, comuni ai file system moderni.
- Individuare solo le directory **/var/opt/mssql** nel montaggio NFS. Gli altri file, ad esempio i file binari di sistema di SQL Server, non sono supportati.
- Assicurarsi che i client NFS usino l'opzione "nolock" per il montaggio della condivisione remota.

## <a name="configure-source-repositories"></a><a id="repositories"></a> Configurare i repository di origine

Quando si installa o si aggiorna SQL Server, si ottiene la versione più recente di SQL Server dal repository Microsoft configurato. Le guide di avvio rapido usano il repository **CU** degli aggiornamenti cumulativi per SQL Server, ma è anche possibile configurare un repository **GDR**. Per altre informazioni sui repository e su come configurarli, vedere [Configurare i repository per SQL Server in Linux](sql-server-linux-change-repo.md).

## <a name="install-sql-server"></a><a id="platforms"></a> Installare SQL Server

È possibile installare SQL Server 2017 o SQL Server 2019 in Linux dalla riga di comando. Per istruzioni dettagliate, vedere gli argomenti di avvio rapido seguenti:

| Piattaforma | Argomenti di avvio rapido per l'installazione |
|---|---|
| Red Hat Enterprise Linux (RHEL) | [2017](quickstart-install-connect-red-hat.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true) |
| SUSE Linux Enterprise Server (SLES) | [2017](quickstart-install-connect-suse.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true) |
| Ubuntu | [2017](quickstart-install-connect-ubuntu.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true) |
| Docker | [2017](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) |

È anche possibile eseguire SQL Server in Linux in una macchina virtuale di Azure. Per altre informazioni, vedere [Provisioning di una macchina virtuale SQL in Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json).

Dopo l'installazione, provare ad apportare ulteriori modifiche di configurazione per ottenere prestazioni ottimali. Per altre informazioni, vedere [Procedure consigliate per le prestazioni e linee guida per la configurazione per SQL Server in Linux](sql-server-linux-performance-best-practices.md).

## <a name="update-or-upgrade-sql-server"></a><a id="upgrade"></a> Aggiornamento o upgrade di SQL Server

Per aggiornare il pacchetto **mssql-server** alla versione più recente, usare uno dei comandi seguenti in base alla piattaforma:

| Piattaforma | Comandi di aggiornamento del pacchetto |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

Questi comandi scaricano il pacchetto più recente e sostituiscono i file binari presenti in `/opt/mssql/`. I database e i database di sistema generati dall'utente non sono interessati da questa operazione.

Per eseguire l'upgrade di SQL Server, è prima necessario [passare il repository configurato](sql-server-linux-change-repo.md) alla versione di SQL Server desiderata. Usare quindi lo stesso comando **update** per eseguire l'upgrade della versione di SQL Server. Questa operazione è possibile solo se il percorso di upgrade tra i due repository è supportato.

## <a name="rollback-sql-server"></a><a id="rollback"></a> Eseguire il rollback di SQL Server

Per eseguire il rollback o effettuare il downgrade di SQL Server a una versione precedente, seguire questa procedura:

1. Identificare il numero di versione per il pacchetto di SQL Server a cui si vuole effettuare il downgrade. Per un elenco di numeri di pacchetto, vedere le [Note sulla versione](../linux/sql-server-linux-release-notes.md).

1. Effettuare il downgrade a una versione precedente di SQL Server. Nei comandi seguenti sostituire `<version_number>` con il numero di versione di SQL Server identificato nel primo passaggio.

   | Piattaforma | Comandi di aggiornamento del pacchetto |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> È supportato solo il downgrade a una versione all'interno della stessa versione principale, ad esempio SQL Server 2019.

## <a name="check-installed-sql-server-version"></a><a id="versioncheck"></a> Controllare la versione di SQL Server installata

Per verificare la versione corrente e l'edizione di SQL Server in Linux, seguire questa procedura:

1. Installare gli [strumenti da riga di comando di SQL Server](sql-server-linux-setup-tools.md), se non sono già installati.

1. Usare **sqlcmd** per eseguire un comando Transact-SQL che visualizza la versione e l'edizione di SQL Server.

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a name="uninstall-sql-server"></a><a id="uninstall"></a> Disinstallare SQL Server

Per rimuovere il pacchetto **mssql-server** in Linux, usare uno dei comandi seguenti in base alla piattaforma:

| Piattaforma | Comandi di rimozione dei pacchetti |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

La rimozione del pacchetto non comporta l'eliminazione dei file di database generati. Per eliminare i file di database, usare il comando seguente:

```bash
sudo rm -rf /var/opt/mssql/
```

## <a name="unattended-install"></a><a id="unattended"></a> Installazione automatica

È possibile eseguire un'installazione automatica nel modo seguente:

- Seguire i passaggi iniziali negli [argomenti di avvio rapido](#platforms) per registrare i repositori e installare SQL Server.
- Quando si esegue `mssql-conf setup`, impostare le [variabili di ambiente](sql-server-linux-configure-environment-variables.md) e usare l'opzione `-n` (nessun prompt).

L'esempio seguente configura l'edizione Developer di SQL Server con la variabile di ambiente **MSSQL_PID**. Accetta anche l'EULA (**ACCEPT_EULA**) e imposta la password dell'utente amministratore di sistema (**MSSQL_SA_PASSWORD**). Il parametro `-n` esegue un'installazione non richiesta in cui viene eseguito il pull dei valori di configurazione dalle variabili di ambiente.

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

È anche possibile creare uno script per eseguire altre azioni. È ad esempio possibile installare altri pacchetti di SQL Server.

Per uno script di esempio più dettagliato, vedere gli esempi seguenti:

- [Script di installazione automatica per Red Hat](sample-unattended-install-redhat.md)
- [Script di installazione automatica per SUSE](sample-unattended-install-suse.md)
- [Script di installazione automatica per Ubuntu](sample-unattended-install-ubuntu.md)

## <a name="offline-install"></a><a id="offline"></a> Installazione offline

Se il computer Linux non ha accesso ai repository online usati negli [argomenti di avvio rapido](#platforms), è possibile scaricare direttamente i file del pacchetto. Questi pacchetti si trovano nel repository di Microsoft, [https://packages.microsoft.com](https://packages.microsoft.com).

> [!TIP]
> Se l'installazione è stata eseguita correttamente con i passaggi indicati negli argomenti di avvio rapido, non è necessario scaricare o installare manualmente i pacchetti di SQL Server. Questa sezione riguarda solo lo scenario offline.

1. **Scaricare il pacchetto del motore di database per la propria piattaforma**. I collegamenti ai download dei pacchetti sono disponibili nella sezione dei dettagli del pacchetto delle [Note sulla versione](../linux/sql-server-linux-release-notes.md).

1. **Spostare il pacchetto scaricato nel computer Linux**. Se è stato usato un computer diverso per scaricare i pacchetti, per spostarli nel computer Linux, è possibile usare il comando **scp**.

1. **Installare il pacchetto del motore di database**. Usare uno dei comandi seguenti in base alla piattaforma. Sostituire il nome del file del pacchetto in questo esempio con il nome esatto di quello scaricato.

   | Piattaforma | Comando di installazione del pacchetto |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > È anche possibile installare i pacchetti RPM (RHEL e SLES) con il comando `rpm -ivh`, ma i comandi della tabella precedente installano automaticamente le dipendenze, se disponibili, dai repository approvati.

1. **Risolvere le dipendenze mancanti**: a questo punto potrebbero mancare alcune dipendenze. In caso contrario, è possibile ignorare questo passaggio. In Ubuntu, se si ha accesso a repository approvati contenenti tali dipendenze, la soluzione più semplice è usare il comando `apt-get -f install`. Questo comando completa anche l'installazione di SQL Server. Per controllare manualmente le dipendenze, usare i comandi seguenti:

   | Piattaforma | Comando per elencare le dipendenze |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   Dopo aver risolto le dipendenze mancanti, provare a installare di nuovo il pacchetto mssql-server.

1. **Completare l'installazione di SQL Server**. Usare **mssql-conf** per completare l'installazione di SQL Server:

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="licensing-and-pricing"></a>Licenze e prezzi

SQL Server ha una licenza identica per Linux e Windows. Per altre informazioni sulle licenze e sui prezzi di SQL Server, vedere [Come ottenere una licenza per SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

## <a name="optional-sql-server-features"></a>Funzionalità facoltative di SQL Server

Dopo l'installazione, è anche possibile installare o abilitare le funzionalità facoltative di SQL Server.

- [Strumenti da riga di comando di SQL Server](sql-server-linux-setup-tools.md)
- [SQL Server Agent](sql-server-linux-setup-sql-agent.md)
- [Ricerca full-text di SQL Server](sql-server-linux-setup-full-text-search.md)
- [Machine Learning Services (R, Python)](sql-server-linux-setup-machine-learning.md)
- [SQL Server Integration Services](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](sql-server-linux-faq.yml).
