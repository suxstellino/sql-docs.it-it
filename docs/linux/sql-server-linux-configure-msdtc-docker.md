---
title: Transazioni distribuite (MSDTC) con SQL Server in Docker
description: Informazioni su come usare Microsoft Distributed Transaction Coordinator (MSDTC) per le transazioni distribuite in un contenitore di SQL Server in Docker.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: f6cc1ad3c66ae7f4ec1817af3bcaa3934a1858ef
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622897"
---
# <a name="how-to-use-distributed-transactions-with-sql-server-on-docker"></a>Come usare le transazioni distribuite con SQL Server in Docker

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come configurare contenitori di SQL Server in Linux in Docker per le transazioni distribuite.

Le immagini del contenitore di SQL Server possono usare Microsoft Distributed Transaction Coordinator (MSDTC), necessario per le transazioni distribuite. Per informazioni sui requisiti di comunicazione per MSDTC, vedere [Come configurare Microsoft Distributed Transaction Coordinator (MSDTC) in Linux](sql-server-linux-configure-msdtc.md). Questo articolo illustra i requisiti e gli scenari speciali correlati ai contenitori Docker di SQL Server.

## <a name="configuration"></a>Configurazione

Per abilitare le transazioni MSDTC nei contenitori per Docker, è necessario impostare due nuove variabili di ambiente:

- **MSSQL_RPC_PORT**: porta TCP a cui viene associato il servizio del mapper di endpoint RPC e su cui rimane in ascolto il servizio.  
- **MSSQL_DTC_TCP_PORT**: porta su cui il servizio MSDTC è configurato per l'ascolto.

### <a name="pull-and-run"></a>Pull ed esecuzione

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

L'esempio seguente illustra come usare queste variabili di ambiente per il pull e l'esecuzione di un singolo contenitore di SQL Server 2017 configurato per MSDTC. Ciò consente le comunicazioni con qualsiasi applicazione su qualsiasi host.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2017-latest
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2017-latest
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

L'esempio seguente illustra come usare queste variabili di ambiente per il pull e l'esecuzione di un singolo contenitore di SQL Server 2019 configurato per MSDTC. Ciò consente le comunicazioni con qualsiasi applicazione su qualsiasi host.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

> [!IMPORTANT]
> Il comando precedente funziona solo per Docker in esecuzione in Linux. Per Docker in Windows, l'host Windows è già in ascolto sulla porta 135. È possibile rimuovere il parametro `-p 135:135` per Docker in Windows, ma esistono alcune limitazioni. Il contenitore risultante non può poi essere usato per le transazioni distribuite che coinvolgono l'host. Può partecipare solo alle transazioni distribuite tra i contenitori Docker nell'host.

In questo comando il servizio **mapper di endpoint RPC** è stato associato alla porta 135 e il servizio **MSDTC** è stato associato alla porta 51000 nella rete virtuale Docker. Le comunicazioni TDS di SQL Server avvengono sulla porta 1433 all'interno della rete virtuale Docker. Queste porte sono state esposte esternamente all'host come porta TDS 51433, porta del mapper di endpoint RPC 135 e porta MSDTC 51000.

> [!NOTE]
> La porta del mapper di endpoint RPC e la porta MSDTC non devono essere uguali nell'host e nel contenitore. Quindi, anche se viene configurata la porta 135 per il mapper di endpoint RPC nel contenitore, si potrebbe potenzialmente eseguirne il mapping alla porta 13501 o a qualsiasi altra porta disponibile nel server host.

## <a name="configure-the-firewall"></a>Configurare il firewall

Per comunicare con e tramite l'host, è anche necessario configurare il firewall nel server host per i contenitori. Aprire il firewall per tutte le porte esposte dal contenitore Docker per le comunicazioni esterne. Nell'esempio precedente si tratta delle porte 135, 51433 e 51000. Queste sono le porte nell'host stesso e non le porte a cui sono mappate nel contenitore. Pertanto, se è stato eseguito il mapping della porta del mapper di endpoint RPC 51000 del contenitore alla porta host 51001, per le comunicazioni con l'host deve essere aperta la porta 51001 (non 51000) nel firewall.  

L'esempio seguente mostra come creare queste regole in Ubuntu.

```bash
sudo ufw allow from any to any port 51433 proto tcp
sudo ufw allow from any to any port 51000 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

L'esempio seguente mostra come eseguire questa operazione in Red Hat Enterprise Linux (RHEL):

```bash
sudo firewall-cmd --zone=public --add-port=51433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=51000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

## <a name="configure-port-routing-on-the-host"></a>Configurare il routing delle porte nell'host

Nell'esempio precedente, dato che un singolo contenitore Docker esegue il mapping della porta RPC 135 alla porta 135 nell'host, le transazioni distribuite con l'host dovrebbero ora funzionare senza ulteriori configurazioni. Si noti che è possibile usare direttamente la porta 135 in contenitori in esecuzione come radice, perché in tali contenitori SQL Server viene eseguito con privilegi elevati. Per SQL Server all'esterno di un contenitore o in contenitori non radice, è necessario usare una porta temporanea diversa, ad esempio la porta 13500, nel contenitore e il traffico destinato alla porta 135 deve quindi essere indirizzato alla porta temporanea. È anche necessario configurare regole di routing delle porte all'interno del contenitore dalla porta 135 alla porta temporanea.

Se si decide di eseguire il mapping della porta 135 del contenitore a una porta diversa nell'host, ad esempio alla porta 13500, è anche necessario configurare il routing delle porte nell'host. Ciò consente al contenitore Docker di partecipare alle transazioni distribuite con l'host e con altri server esterni.

Per altre informazioni sulle porte di routing, vedere [Configurare il routing delle porte](sql-server-linux-configure-msdtc.md#configure-port-routing).

> [!NOTE]
> SQL Server 2017 viene eseguito in contenitori radice per impostazione predefinita, mentre i contenitori di SQL Server 2019 vengono eseguiti come utenti non root.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su MSDTC in Linux, vedere [Come configurare Microsoft Distributed Transaction Coordinator (MSDTC) in Linux](sql-server-linux-configure-msdtc.md).
