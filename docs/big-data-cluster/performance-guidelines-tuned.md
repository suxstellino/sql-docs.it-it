---
title: Procedure consigliate per le prestazioni dei cluster Big Data di SQL Server
description: Questo articolo illustra le procedure consigliate per le prestazioni e le linee guida per l'esecuzione di cluster Big Data di SQL Server in Kubernetes
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7b430e09a06523bc3ee369434911738f83ab5ee4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044161"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-big-data-clusters"></a>Procedure consigliate per le prestazioni e linee guida per la configurazione di cluster Big Data di SQL Server

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo illustra le procedure consigliate e offre suggerimenti per ottimizzare le prestazioni delle applicazioni che hanno come destinazione servizi in esecuzione all'interno di un cluster Big Data.

Le linee guida seguenti sono incentrate sulle raccomandazioni per la configurazione del sistema operativo Linux che ospita i nodi di lavoro Kubernetes in cui verrà distribuito il cluster BDC. Come procedura consigliata, configurare il profilo di ottimizzazione prima di distribuire il cluster Big Data. Le impostazioni incluse nel profilo di ottimizzazione proposto sono state convalidate durante il case study condotto da Microsoft e Intel. I risultati dello studio sono pubblicati e disponibili per il download in questo [white paper](https://aka.ms/sql-bdc-spark-perf/).

> [!TIP]
> Per l'ottimizzazione delle configurazioni specifiche di SQL Server in Linux, vedere [Procedure consigliate per le prestazioni e linee guida per la configurazione per SQL Server in Linux](../linux/sql-server-linux-performance-best-practices.md). Sono inoltre valide altre procedure consigliate, ad esempio la progettazione di indici per database di SQL Server.

## <a name="proposed-linux-settings-using-a-tuned-mssql-bdc-profile"></a>Impostazioni di Linux proposte con un profilo `mssql-bdc` ottimizzato

Creare un profilo di configurazione **tuned.conf** con il contenuto seguente.

```bash
[main]
summary=Optimize for Microsoft SQL Server Big Data Clusters TPC-DS performance
include=throughput-performance
 
[sysctl]
#network tunings
net.ipv4.conf.default.rp_filter=1
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_sack = 1
net.core.netdev_max_backlog = 25000
net.core.rmem_max = 2147483647
net.core.wmem_max = 2147483647
net.core.rmem_default = 33554431
net.core.wmem_default = 33554432
net.core.optmem_max = 33554432
net.ipv4.tcp_rmem =8192 33554432 2147483647
net.ipv4.tcp_wmem =8192 33554432 2147483647
net.ipv4.tcp_low_latency=1
net.ipv4.tcp_adv_win_scale=1
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv4.conf.all.arp_filter=1
net.ipv4.tcp_retries2=5
net.ipv6.conf.lo.disable_ipv6 = 1
net.core.somaxconn = 65535
 
#memory cache settings
vm.swappiness=1
vm.overcommit_memory=0
vm.dirty_background_ratio=1
 
#kernel NUMA
kernel.numa_balancing=0

#filesystem
fs.aio-max-nr=1048576
 
[vm]
#should be revisited for SQL large pages use in master/data/compute pods
transparent_hugepages=never
```

## <a name="install-tuned-utility-on-all-the-kubernetes-worker-nodes"></a>Installare l'utilità **tuned** in tutti i nodi di lavoro Kubernetes

Per installare **tuned**, eseguire il comando seguente:

```bash
apt-get -y install tuned
```

## <a name="apply-tuning-settings-to-all-kubernetes-worker-nodes"></a>Applicare le impostazioni di ottimizzazione a tutti i nodi di lavoro Kubernetes

In ognuno dei nodi di lavoro di destinazione copiare il file **tuned.conf** creato in precedenza:

```bash
cd /usr/lib/tuned
scp -r <sourcePath> ./mssql-bdc
```

Per abilitare questo profilo ottimizzato **mssql-bdc**, salvare le definizioni in un file **tuned.conf** in una cartella `/usr/lib/tuned/mssql-bdc` su tutti i nodi di lavoro Kubernetes e abilitare il profilo usando quanto segue:

```bash
chmod +x /usr/lib/tuned/mssql-bdc/tuned.conf
tuned-adm profile mssql-bdc
```

Verificare che il profilo sia abilitato usando questo comando:

```bash
tuned-adm active
```

o

```bash
tuned-adm list
```

Se il profilo viene modificato in modo dinamico, per rendere effettive le nuove modifiche è necessario riavviare **tuned** in tutti i nodi di lavoro interessati:

```bash
systemctl restart tuned
```
 
I log relativi al servizio **tuned** sono reperibili in */var/log/tuned/tuned.log*.

Facoltativamente, è possibile configurare il profilo di ottimizzazione in un solo nodo del cluster Kubernetes e usare lo script seguente per copiarlo e configurarlo nei nodi rimanenti.

```bash
#!/bin/bash -e
# This script takes a list of servers (IPs like `cat ~administrator/workerhosts)) as input
# and update these servers with the local version of mssql-bdc tuned.conf.
 
is_root() {
    local is_root_set=0
    if [ "$EUID" -ne 0 ]; then
        echo "Please run as root"
    else
        is_root_set=1
    fi
    return "${is_root_set}"
}
 
# function main
if is_root -eq 0; then
    exit 0
fi
 
while [ $# -gt 0 ]
do
    # sometimes, people add non-breaking space characters to their *host* files.
    WORKER_IP=$(echo "$1" | sed -e 's/\xC2\xA0//g')
    echo -e "updating mssql-bdc tuned on \e[42m${WORKER_IP}\e[0m"
    # the following commands assume tuned was not set up and active...
    # TODO: add the tuned install and status checks
    ssh "${WORKER_IP}" mkdir -p /usr/lib/tuned/mssql-bdc
    scp ~administrator/tuned.conf "${WORKER_IP}":/usr/lib/tuned/mssql-bdc/tuned.conf
    ssh "${WORKER_IP}" tuned-adm profile mssql-bdc
    ssh "${WORKER_IP}" systemctl restart tuned
    ssh "${WORKER_IP}" tuned-adm active
    shift
done

```

## <a name="next-steps"></a>Passaggi successivi

Per altre risorse, incluse le architetture di riferimento per i cluster Big Data di SQL Server, vedere la documentazione seguente:

* [Case Study: Carichi di lavoro SQL in esecuzione su Apache Spark nel cluster Big Data di MS SQL Server 2019](https://aka.ms/sql-bdc-spark-perf/)

* [Architettura di riferimento HPE per una visione approfondita di tutti i dati con cluster Big Data di Microsoft SQL Server 2019](https://h20195.www2.hpe.com/V2/GetDocument.aspx?docname=a50001963enw)

* [Dell EMC PowerStore: cluster Big Data di Microsoft SQL Server 2019](https://www.dellemc.com/resources/en-us/asset/white-papers/products/storage/h18231-dell-emc-powerstore-sql-server-big-data-clusters.pdf)

* [Cluster Big Data di Microsoft SQL Server 2019: una soluzione per Big Data con l'infrastruttura Dell EMC](https://infohub.delltechnologies.com/t/microsoft-sql-server-2019-big-data-clusters-a-big-data-solution-using-dell-emc-infrastructure/)

* [Cluster Big Data di Microsoft SQL Server 2019 su architettura di riferimento Cisco UCS](https://www.cisco.com/c/en/us/solutions/collateral/data-center-virtualization/unified-computing/sql-server-on-big-data-cluster-on-ucs.html)