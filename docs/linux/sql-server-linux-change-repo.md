---
title: Configurare i repository Linux per SQL Server 2017 e 2019
description: Selezionare e configurare i repository di origine per SQL Server 2019 e SQL Server 2017 in Linux. Il repository di origine influisce sulla versione di SQL Server che viene applicata durante l'installazione e l'aggiornamento.
author: VanMSFT
ms.author: vanto
ms.date: 04/28/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
zone_pivot_groups: ld2-linux-distribution
ms.openlocfilehash: d284a049cb1266dc33c30657f3d86e8421825868
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340391"
---
# <a name="configure-repositories-for-installing-and-upgrading-sql-server-on-linux"></a>Configurare i repository per l'installazione e l'aggiornamento di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

::: zone pivot="ld2-rhel"
Questo articolo descrive come configurare il repository corretto per le installazioni e gli aggiornamenti di SQL Server 2017 e SQL Server 2019 in Linux. Nella parte superiore, la selezione corrente è **Red Hat (RHEL)** .
::: zone-end

::: zone pivot="ld2-sles"
Questo articolo descrive come configurare il repository corretto per le installazioni e gli aggiornamenti di SQL Server 2017 e SQL Server 2019 in Linux. Nella parte superiore, la selezione corrente è **SUSE (SLES)** .
::: zone-end

::: zone pivot="ld2-ubuntu"
Questo articolo descrive come configurare il repository corretto per le installazioni e gli aggiornamenti di SQL Server 2017 e SQL Server 2019 in Linux. Nella parte superiore, la selezione corrente è **Ubuntu**.
::: zone-end

> [!TIP]
> SQL Server 2019 è ora disponibile. Per provarlo, configurare il nuovo repository **mssql-server-2019** seguendo le indicazioni di questo articolo. Eseguire quindi l'installazione seguendo le istruzioni nella [guida all'installazione](sql-server-linux-setup.md).

## <a name="repositories"></a><a id="repositories"></a> Repository

Quando si installa SQL Server in Linux, è necessario configurare un repository Microsoft. Questo repository viene usato per acquisire il pacchetto del motore di database, **mssql-server**, e i pacchetti di SQL Server correlati. Esistono attualmente cinque repository principali:

| Archivio | Nome | Descrizione |
|---|---|---|
| **2019** | **mssql-server-2019** | Repository di SQL Server 2019 (aggiornamento cumulativo). |
| **2019 GDR** | **mssql-server-2019-gdr** | Repository di SQL Server 2019 GDR solo per aggiornamenti critici. |
| **2019 (anteprima)** | **mssql-server-preview** | Repository di SQL Server 2019 (anteprima) e RC. |
| **2017** | **mssql-server-2017** | Repository di SQL Server 2017 (aggiornamento cumulativo). |
| **2017 GDR** | **mssql-server-2017-gdr** | Repository di SQL Server 2017 GDR solo per aggiornamenti critici. |

## <a name="cumulative-update-versus-gdr"></a><a id="cuversusgdr"></a> Aggiornamento cumulativo e GDR

È importante sottolineare che esistono due tipi principali di repository per ogni distribuzione:

- **Aggiornamenti cumulativi (CU)** : il repository degli aggiornamenti cumulativi (CU) contiene pacchetti per la versione di base di SQL Server e le eventuali correzioni di bug o i miglioramenti apportati successivamente al rilascio. Gli aggiornamenti cumulativi sono specifici di una versione finale, ad esempio SQL Server 2019. Vengono rilasciati a cadenza regolare.

- **GDR**: il repository GDR contiene i pacchetti per la versione di base di SQL Server e solo le correzioni critiche e gli aggiornamenti della sicurezza a partire da tale versione. Questi aggiornamenti vengono aggiunti anche alla versione CU successiva.

Ogni versione CU e GDR contiene il pacchetto di SQL Server completo e tutti gli aggiornamenti precedenti per tale repository. L'aggiornamento da una versione GDR a una versione CU è supportato cambiando il repository configurato per SQL Server. È anche possibile [effettuare il downgrade](sql-server-linux-setup.md#rollback) a qualsiasi versione all'interno della versione principale (ad esempio: 2017).

> [!NOTE]
> È possibile eseguire l'aggiornamento da una versione GDR a una versione CU in qualsiasi momento cambiando repository. L'aggiornamento da una versione CU a una versione GDR non è supportato.

## <a name="configure-repositories"></a>Configurare i repository

::: zone pivot="ld2-rhel"
Usare le procedure descritte nelle sezioni seguenti per configurare i repository in Red Hat Enterprise Server (RHEL).
::: zone-end

::: zone pivot="ld2-sles"
Usare le procedure descritte nelle sezioni seguenti per configurare i repository in SUSE Linux Enterprise Server (SLES).
::: zone-end

::: zone pivot="ld2-ubuntu"
Usare le procedure descritte nelle sezioni seguenti per configurare i repository in Ubuntu.
::: zone-end

## <a name="check-for-previously-configured-repositories"></a>Verificare la presenza di repository configurati in precedenza

<!--RHEL-->
::: zone pivot="ld2-rhel"
Verificare prima di tutto se è già stato registrato un repository di SQL Server.

1. Visualizzare i file nella directory **/etc/yum.repos.d** con il comando seguente:

   ```bash
   sudo ls /etc/yum.repos.d
   ```

2. Cercare una file che configura la directory di SQL Server, ad esempio **mssql-server.repo**.

3. Visualizzare il contenuto del file.

   ```bash
   sudo cat /etc/yum.repos.d/mssql-server.repo
   ```

4. La proprietà **name** è il repository configurato. È possibile identificarla con la tabella nella sezione [Repository](#repositories) di questo articolo.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Verificare prima di tutto se è già stato registrato un repository di SQL Server.

1. Usare **zypper info** per ottenere informazioni su eventuali repository configurati in precedenza.

   ```bash
   sudo zypper info mssql-server
   ```

2. La proprietà **Repository** è il repository configurato. È possibile identificarla con la tabella nella sezione [Repository](#repositories) di questo articolo.

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Verificare prima di tutto se è già stato registrato un repository di SQL Server.

1. Visualizzare il contenuto del file **/etc/apt/sources.list**.

   ```bash
   sudo cat /etc/apt/sources.list
   ```

2. Esaminare l'URL del pacchetto per mssql-server. È possibile identificarla con la tabella nella sezione [Repository](#repositories) di questo articolo.

::: zone-end

## <a name="remove-old-repository"></a>Rimuovere il repository precedente

<!--RHEL-->
::: zone pivot="ld2-rhel"
Se necessario, rimuovere il repository precedente con il comando seguente.

```bash
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Questo comando presuppone che il file identificato nella sezione precedente sia denominato **mssql-server.repo**.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Se necessario, rimuovere il repository precedente. Usare uno dei comandi seguenti in base al tipo di repository configurato in precedenza.

| Archivio | Comando da rimuovere |
|---|---|
| **Anteprima (2019)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-preview'` |
| **2019 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019'` |
| **2019 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019-gdr'`|
| **2017 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017'` |
| **2017 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017-gdr'`|

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Se necessario, rimuovere il repository precedente. Usare uno dei comandi seguenti in base al tipo di repository configurato in precedenza.

> [!NOTE]
> A partire da SQL Server 2019 CU3 e SQL Server 2017 CU20 è supportato Ubuntu 18.04. Se si usa Ubuntu 16.04, modificare il percorso riportato di seguito impostando `/ubuntu/16.04` anziché `/ubuntu/18.04` e usare il [nome del codice di distribuzione](https://releases.ubuntu.com/) corretto.

| Archivio | Comando da rimuovere |
|---|---|
| **Anteprima (2019)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview xenial main'` |
| **2019 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019 bionic main'` | 
| **2019 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019-gdr bionic main'` |
| **2017 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017 xenial main'` | 
| **2017 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr xenial main'` |

::: zone-end

## <a name="configure-new-repository"></a>Configurare il nuovo repository

<!--RHEL-->
::: zone pivot="ld2-rhel"
Configurare il nuovo repository da usare per le installazioni e gli aggiornamenti di SQL Server. Usare uno dei comandi seguenti per configurare il repository scelto.

> [!NOTE]
> I comandi seguenti per SQL Server 2019 puntano al repository RHEL 8. RHEL 8 non viene preinstallato con python2, richiesto per SQL Server. Per altre informazioni, vedere il blog seguente sull'installazione di python2 e sulla relativa configurazione come interprete predefinito: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>
> A partire da SQL Server 2017 CU20 è supportato RHEL 8.
>
> Se si usa RHEL 7 o RHEL 8, assicurarsi che i percorsi corrispondano a `/rhel/7` o a `/rhel/8`. I pacchetti non tengono conto della versione secondaria di RHEL. Questo significa che se si usa RHEL 7.6, sarà necessario usare il percorso `/rhel/7` per configurare il repository.

| Archivio | Versione | Comando |
|---|---|---|
| **2019 CU** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017-gdr.repo` |

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Configurare il nuovo repository da usare per le installazioni e gli aggiornamenti di SQL Server. Usare uno dei comandi seguenti per configurare il repository scelto.

| Archivio | Versione | Comando |
|---|---|---|
| **2019 CU** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017-gdr.repo` |

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Configurare il nuovo repository da usare per le installazioni e gli aggiornamenti di SQL Server.

> [!NOTE]
> A partire da SQL Server 2019 CU3 e SQL Server 2017 CU20 è supportato Ubuntu 18.04. I comandi seguenti indicano il repository Ubuntu 18.04.
>
> Se si usa Ubuntu 16.04, modificare il percorso riportato di seguito sostituendo `/ubuntu/18.04` con `/ubuntu/16.04`.

1. Importare le chiavi GPG del repository pubblico.

   ```bash
   sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Usare uno dei comandi seguenti per configurare il repository scelto.

   | Archivio | Versione | Comando |
   |---|---|---|
   | **2019 CU** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"` |
   | **2019 GDR** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019-gdr.list)"` |
   | **2017 CU** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"` |
   | **2017 GDR** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017-gdr.list)"` |

3. Eseguire **apt-get update**.

   ```bash
   sudo apt-get update
   ```

::: zone-end

## <a name="next-steps"></a>Passaggi successivi

Dopo aver configurato il repository corretto, è possibile procedere con l'[installazione](sql-server-linux-setup.md#platforms) o l'[aggiornamento](sql-server-linux-setup.md#upgrade) di SQL Server e degli eventuali pacchetti correlati dal nuovo repository.

::: zone pivot="ld2-rhel"
> [!IMPORTANT]
> A questo punto, se si sceglie di usare la [guida di avvio rapido di RHEL](quickstart-install-connect-red-hat.md), tenere presente che il repository di destinazione è già stato configurato. Non ripetere la procedura nelle esercitazioni. Questo vale soprattutto se si configura il repository GDR, perché la guida di avvio rapido usa il repository CU.
::: zone-end

::: zone pivot="ld2-sles"
> [!IMPORTANT]
> A questo punto, se si sceglie di usare la [guida di avvio rapido di SLES](quickstart-install-connect-suse.md), tenere presente che il repository di destinazione è già stato configurato. Non ripetere la procedura nelle esercitazioni. Questo vale soprattutto se si configura il repository GDR, perché la guida di avvio rapido usa il repository CU.
::: zone-end

::: zone pivot="ld2-ubuntu"
> [!IMPORTANT]
> A questo punto, se si sceglie di usare la [guida di avvio rapido di Ubuntu](quickstart-install-connect-ubuntu.md), tenere presente che il repository di destinazione è già stato configurato. Non ripetere la procedura nelle esercitazioni. Questo vale soprattutto se si configura il repository GDR, perché la guida di avvio rapido usa il repository CU.
::: zone-end

Per altre informazioni su come installare SQL Server 2017 in Linux, vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md).
