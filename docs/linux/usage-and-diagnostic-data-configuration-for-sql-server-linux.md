---
title: Configurare la raccolta di dati di diagnostica e utilizzo per SQL Server in Linux
description: Descrive come i dati di diagnostica e utilizzo del cliente di SQL Server vengono raccolti e configurati in Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 31f391360636bb27bf346ee76614c409738347e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347319"
---
# <a name="configure-usage--diagnostic-data-collection-for-sql-server-on-linux"></a>Configurare la raccolta di dati di diagnostica e utilizzo per SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Per impostazione predefinita, Microsoft SQL Server raccoglie informazioni su come i clienti usano l'applicazione. In particolare, SQL Server raccoglie informazioni sull'esperienza di installazione, l'utilizzo e le prestazioni. Queste informazioni consentono a Microsoft di migliorare il prodotto per meglio soddisfare le esigenze dei clienti. Ad esempio, Microsoft raccoglie informazioni sui tipi di codici di errore riscontrati dai clienti in modo da poter correggere i bug correlati, migliorare la documentazione su come usare SQL Server e determinare se occorre aggiungere funzionalità al prodotto per offrire un'esperienza migliore ai clienti.

Questo documento fornisce dettagli sui tipi di informazioni raccolte e su come configurare Microsoft SQL Server in Linux per inviare le informazioni raccolte a Microsoft. SQL Server 2017 include un'informativa sulla privacy che spiega quali informazioni degli utenti vengono raccolte e quali non vengono raccolte. Per altre informazioni, vedere l'[informativa sulla privacy](../sql-server/sql-server-privacy.md).

In particolare, Microsoft non invia alcuna informazione dei tipi seguenti tramite questo meccanismo:

- Qualsiasi valore all'interno delle tabelle utente
- Credenziali di accesso o altre informazioni di autenticazione
- Informazioni personali

SQL Server 2017 raccoglie e invia sempre informazioni sull'esperienza di installazione dal processo di installazione, in modo che sia possibile individuare e correggere eventuali problemi di installazione riscontrati dai clienti. SQL Server 2017 può essere configurato (per ogni singola istanza) per non inviare informazioni a Microsoft tramite **mssql-conf**. mssql-conf è uno script di configurazione che viene installato con SQL Server 2017 per Red Hat Enterprise Linux, SUSE Linux Enterprise Server e Ubuntu.

> [!NOTE]
> È possibile disabilitare l'invio di informazioni a Microsoft solo nelle versioni a pagamento di SQL Server.

## <a name="disable-usage-and-diagnostic-data-collection"></a>Disabilitare la raccolta di dati di diagnostica e utilizzo

Questa opzione consente di modificare l'impostazione che determina se SQL Server deve inviare la raccolta di dati di utilizzo e diagnostica a Microsoft o meno. Per impostazione predefinita, questo valore è impostato su true. Per modificare il valore, eseguire i comandi seguenti:

> [!IMPORTANT]
> Non è possibile disattivare la raccolta di dati di utilizzo e diagnostica per le edizioni gratuite di SQL Server, Express e Developer.

### <a name="on-red-hat-suse-and-ubuntu"></a>In Red Hat, SUSE e Ubuntu

1. Eseguire lo script mssql-conf come radice con il comando **set** per **telemetry.customerfeedback**. L'esempio seguente disattiva la raccolta di dati di utilizzo e diagnostica specificando **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>In Docker
Per disabilitare la raccolta di dati di utilizzo e diagnostica in Docker, è necessario che Docker [salvi in modo permanente i dati](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Aggiungere un file `mssql.conf` con le righe `[telemetry]` e `customerfeedback = false` nella directory host:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Eseguire l'immagine del contenitore

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Aggiungere un file `mssql.conf` con le righe `[telemetry]` e `customerfeedback = false` nella directory host:

   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Eseguire l'immagine del contenitore

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="local-audit-for-sql-server-on-linux-usage-and-diagnostic-data-collection"></a>Controllo locale per la raccolta di dati di diagnostica e utilizzo di SQL Server in Linux

In Microsoft SQL Server 2017 sono disponibili funzionalità abilitate per Internet che riescono a raccogliere e inviare a Microsoft dati relativi al computer o al dispositivo ("dati standard sul computer"). Il componente per il controllo locale della raccolta di dati di diagnostica e utilizzo di SQL Server può scrivere i dati raccolti dal servizio in una specifica cartella, che rappresenta i dati (log) da inviare a Microsoft. Lo scopo del controllo locale è quello di consentire ai clienti di visualizzare tutti i dati che Microsoft raccoglie con questa funzionalità, per motivi di conformità alle normative o rispetto della privacy.

In SQL Server in Linux il controllo locale è configurabile a livello di istanza per il motore di database di SQL Server. Gli altri componenti di SQL Server e strumenti di SQL Server non hanno funzionalità di controllo locale per la raccolta di dati di utilizzo e diagnostica.

### <a name="enable-local-audit"></a>Abilitare il controllo locale

Questa opzione abilita il controllo locale e consente di impostare la directory in cui vengono creati i log di controllo locale.

1. Creare una directory di destinazione per i nuovi log di controllo locale. L'esempio seguente crea una nuova directory **/tmp/audit**:

   ```bash
   sudo mkdir /tmp/audit
   ```

2. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

3. Eseguire lo script mssql-conf come radice con il comando **set** per **telemetry.userrequestedlocalauditdirectory**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

4. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>In Docker
Per abilitare il controllo locale in Docker, è necessario che Docker [salvi in modo permanente i dati](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. La directory di destinazione per i nuovi log di controllo locale sarà nel contenitore. Creare una directory di destinazione per i nuovi log di controllo locale nella directory host del computer. L'esempio seguente crea una nuova directory **/audit**:

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Aggiungere un file `mssql.conf` con le righe `[telemetry]` e `userrequestedlocalauditdirectory = <host directory>/audit` nella directory host:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Eseguire l'immagine del contenitore

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. La directory di destinazione per i nuovi log di controllo locale sarà nel contenitore. Creare una directory di destinazione per i nuovi log di controllo locale nella directory host del computer. L'esempio seguente crea una nuova directory **/audit**:

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Aggiungere un file `mssql.conf` con le righe `[telemetry]` e `userrequestedlocalauditdirectory = <host directory>/audit` nella directory host:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Eseguire l'immagine del contenitore

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su SQL Server in Linux, vedere la [panoramica di SQL Server in Linux](sql-server-linux-overview.md).