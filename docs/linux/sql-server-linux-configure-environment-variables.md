---
title: Configurare le variabili di ambiente per SQL Server in Linux
description: Questo articolo descrive come usare variabili di ambiente per configurare impostazioni di SQL Server 2017 specifiche in Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: fb9455970c58daa65f8068792a4edc8ae7e20f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343171"
---
# <a name="configure-sql-server-settings-with-environment-variables-on-linux"></a>Configurare le impostazioni di SQL Server con variabili di ambiente in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Per configurare SQL Server 2017 in Linux è possibile usare diverse variabili di ambiente. Queste variabili vengono usate in due scenari:

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Per configurare SQL Server 2019 in Linux è possibile usare diverse variabili di ambiente. Queste variabili vengono usate in due scenari:

::: moniker-end

- Per definire la configurazione iniziale con il comando `mssql-conf setup`.
- Per configurare un nuovo [contenitore di SQL Server in Docker](quickstart-install-connect-docker.md).

> [!TIP]
> Se è necessario configurare scenari di SQL Server simili a questi scenari di installazione, vedere [Configurare SQL Server in Linux con lo strumento mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="environment-variables"></a>Variabili di ambiente

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Variabile di ambiente | Descrizione |
|-----|-----|
| **ACCEPT_EULA** | Impostare la variabile **ACCEPT_EULA** su qualsiasi valore per confermare l'accettazione delle [condizioni di licenza](https://go.microsoft.com/fwlink/?LinkId=746388). Impostazione obbligatoria per l'immagine di SQL Server. |
| **MSSQL_SA_PASSWORD** | Consente di configurare la password dell'utente SA. |
| **MSSQL_PID** | Consente di impostare l'edizione o il codice Product Key di SQL Server. I valori possibili sono: </br></br>**Versione di valutazione**</br>**Developer**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**Un codice Product Key**</br></br>Se si specifica un codice Product Key, questo deve essere nel formato #####-#####-#####-#####-#####, dove '#' è un numero o una lettera.|
| **MSSQL_LCID** | Imposta l'ID della lingua da usare per SQL Server. Ad esempio, 1036 corrisponde al francese. |
| **MSSQL_COLLATION** | Imposta le regole di confronto per SQL Server. In questo modo viene eseguito l'override del mapping predefinito dell'ID lingua (LCID) per le regole di confronto. |
| **MSSQL_MEMORY_LIMIT_MB** | Imposta la quantità massima di memoria (in MB) che SQL Server può usare. Per impostazione predefinita, corrisponde all'80% della memoria fisica totale. |
| **MSSQL_TCP_PORT** | Consente di configurare la porta TCP su cui è in ascolto SQL Server (impostazione predefinita: 1433). |
| **MSSQL_IP_ADDRESS** | Consente di impostare l'indirizzo IP. Attualmente l'indirizzo IP deve essere nello stile IPv4 (0.0.0.0). |
| **MSSQL_BACKUP_DIR** | Consente di usare il percorso della directory di backup predefinito. |
| **MSSQL_DATA_DIR** | Consente di cambiare la directory in cui vengono creati i file di dati (con estensione mdf) di un nuovo database di SQL Server. |
| **MSSQL_LOG_DIR** | Consente di cambiare la directory in cui vengono creati i file di log (con estensione ldf) di un nuovo database di SQL Server. |
| **MSSQL_DUMP_DIR** | Consente di cambiare la directory in cui SQL Server deposita i dump della memoria e altri file per la risoluzione dei problemi per impostazione predefinita. |
| **MSSQL_ENABLE_HADR** | Consente di abilitare un gruppo di disponibilità. Ad esempio, il valore '1' lo abilita e il valore '0' lo disabilita |
| **MSSQL_AGENT_ENABLED** | Abilitare SQL Server Agent. Ad esempio, il valore 'true' lo abilita e il valore 'false' lo disabilita. Per impostazione predefinita, l'agente è disabilitato.  |
| **MSSQL_MASTER_DATA_FILE** | Imposta il percorso del file di dati del database master. Il nome del file deve essere **master.mdf** fino alla prima esecuzione di SQL Server. |
| **MSSQL_MASTER_LOG_FILE** | Imposta il percorso del file di log del database master. Il nome del file deve essere **mastlog.mdf** fino alla prima esecuzione di SQL Server. |
| **MSSQL_ERROR_LOG_FILE** | Imposta il percorso dei file di log degli errori. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

| Variabile di ambiente | Descrizione |
|-----|-----|
| **ACCEPT_EULA** | Impostare la variabile **ACCEPT_EULA** su qualsiasi valore per confermare l'accettazione delle [condizioni di licenza](https://go.microsoft.com/fwlink/?LinkId=746388). Impostazione obbligatoria per l'immagine di SQL Server. |
| **MSSQL_SA_PASSWORD** | Consente di configurare la password dell'utente SA. |
| **MSSQL_PID** | Consente di impostare l'edizione o il codice Product Key di SQL Server. I valori possibili sono: </br></br>**Versione di valutazione**</br>**Developer**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**Un codice Product Key**</br></br>Se si specifica un codice Product Key, questo deve essere nel formato #####-#####-#####-#####-#####, dove '#' è un numero o una lettera.|
| **MSSQL_LCID** | Imposta l'ID della lingua da usare per SQL Server. Ad esempio, 1036 corrisponde al francese. |
| **MSSQL_COLLATION** | Imposta le regole di confronto per SQL Server. In questo modo viene eseguito l'override del mapping predefinito dell'ID lingua (LCID) per le regole di confronto. |
| **MSSQL_MEMORY_LIMIT_MB** | Imposta la quantità massima di memoria (in MB) che SQL Server può usare. Per impostazione predefinita, corrisponde all'80% della memoria fisica totale. |
| **MSSQL_TCP_PORT** | Consente di configurare la porta TCP su cui è in ascolto SQL Server (impostazione predefinita: 1433). |
| **MSSQL_IP_ADDRESS** | Consente di impostare l'indirizzo IP. Attualmente l'indirizzo IP deve essere nello stile IPv4 (0.0.0.0). |
| **MSSQL_BACKUP_DIR** | Consente di usare il percorso della directory di backup predefinito. |
| **MSSQL_DATA_DIR** | Consente di cambiare la directory in cui vengono creati i file di dati (con estensione mdf) di un nuovo database di SQL Server. |
| **MSSQL_LOG_DIR** | Consente di cambiare la directory in cui vengono creati i file di log (con estensione ldf) di un nuovo database di SQL Server. |
| **MSSQL_DUMP_DIR** | Consente di cambiare la directory in cui SQL Server deposita i dump della memoria e altri file per la risoluzione dei problemi per impostazione predefinita. |
| **MSSQL_ENABLE_HADR** | Consente di abilitare un gruppo di disponibilità. Ad esempio, il valore '1' lo abilita e il valore '0' lo disabilita |
| **MSSQL_AGENT_ENABLED** | Abilitare SQL Server Agent. Ad esempio, il valore 'true' lo abilita e il valore 'false' lo disabilita. Per impostazione predefinita, l'agente è disabilitato.  |
| **MSSQL_MASTER_DATA_FILE** | Imposta il percorso del file di dati del database master. Il nome del file deve essere **master.mdf** fino alla prima esecuzione di SQL Server. |
| **MSSQL_MASTER_LOG_FILE** | Imposta il percorso del file di log del database master. Il nome del file deve essere **mastlog.mdf** fino alla prima esecuzione di SQL Server. |
| **MSSQL_ERROR_LOG_FILE** | Imposta il percorso dei file di log degli errori. |

::: moniker-end

## <a name="use-with-initial-setup"></a>Usare con la configurazione iniziale

Questo esempio esegue `mssql-conf setup` con variabili di ambiente configurate. Vengono specificate le variabili di ambiente seguenti:

- **ACCEPT_EULA** consente di accettare il contratto di licenza con l'utente finale.
- **MSSQL_PID** specifica la Developer Edition di SQL Server con licenza gratuita per l'uso non in produzione.
- **MSSQL_SA_PASSWORD** consente di impostare una password complessa.
- **MSSQL_TCP_PORT** consente di configurare la porta TCP su cui è in ascolto SQL Server su 1234.

```bash
sudo ACCEPT_EULA='Y' MSSQL_PID='Developer' MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' MSSQL_TCP_PORT=1234 /opt/mssql/bin/mssql-conf setup
```

## <a name="use-with-docker"></a>Uso con Docker

Questo comando Docker di esempio usa le variabili di ambiente seguenti per creare un nuovo contenitore di SQL Server:

- **ACCEPT_EULA** consente di accettare il contratto di licenza con l'utente finale.
- **MSSQL_PID** specifica la Developer Edition di SQL Server con licenza gratuita per l'uso non in produzione.
- **MSSQL_SA_PASSWORD** consente di impostare una password complessa.
- **MSSQL_TCP_PORT** consente di configurare la porta TCP su cui è in ascolto SQL Server su 1234. Ciò significa che anziché eseguire il mapping della porta 1433 (impostazione predefinita) a una porta host, è necessario eseguire il mapping della porta TCP personalizzata con il comando `-p 1234:1234` riportato in questo esempio.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Se si esegue Docker in Linux o macOS, usare la sintassi seguente con virgolette singole:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

Se si esegue Docker in Windows, usare la sintassi seguente con virgolette doppie:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

> [!NOTE]
> Il processo di esecuzione delle edizioni di produzione nei contenitori è leggermente diverso. Per altre informazioni, vedere [Run production container images](./sql-server-linux-docker-container-deployment.md#production) (Eseguire immagini del contenitore di produzione).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Se si esegue Docker in Linux o macOS, usare la sintassi seguente con virgolette singole:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

Se si esegue Docker in Windows, usare la sintassi seguente con virgolette doppie:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

Per altre impostazioni di SQL Server non elencate, vedere [Configurare SQL Server in Linux con lo strumento mssql-conf](sql-server-linux-configure-mssql-conf.md).

Per altre informazioni su come installare ed eseguire SQL Server in Linux, vedere [Installare SQL Server in Linux](sql-server-linux-setup.md).