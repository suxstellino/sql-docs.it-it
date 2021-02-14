---
title: Istanze del cluster di failover - SQL Server in Linux
description: I concetti relativi alle istanze del cluster di failover di SQL Server in Linux includono il livello di clustering, il numero di istanze, l'indirizzo IP, il nome e l'archiviazione condivisa.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 1839442f20e2a48ed612e42e3c654d9e3255a477
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346404"
---
# <a name="failover-cluster-instances---sql-server-on-linux"></a>Istanze del cluster di failover - SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra i concetti relativi alle istanze del cluster di failover di SQL Server in Linux. 

Per creare un'istanza del cluster di failover di SQL Server in Linux, vedere [Configurare un'istanza del cluster di failover di SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure.md)

## <a name="the-clustering-layer"></a>Livello di clustering

* In RHEL il livello di clustering si basa sul [componente aggiuntivo a disponibilità elevata](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) di Red Hat Enterprise Linux (RHEL). 

    > [!NOTE] 
    > Per accedere al componente aggiuntivo a disponibilità elevata e alla documentazione di Red Hat, è richiesta una sottoscrizione. 

* In SLES il livello di clustering si basa su SUSE Linux Enterprise [High Availability Extension (HAE)](https://www.suse.com/products/highavailability).

    Per altre informazioni su configurazione dei cluster, opzioni degli agenti delle risorse, gestione, procedure consigliate e suggerimenti, vedere [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

Sia il componente aggiuntivo a disponibilità elevata RHEL che SUSE HAE sono basati su [Pacemaker](https://clusterlabs.org/).

Come illustrato nel diagramma seguente, la risorsa di archiviazione viene presentata a due server. I componenti di clustering, Corosync e Pacemaker, coordinano le comunicazioni e la gestione delle risorse. Uno dei server ha la connessione attiva alle risorse di archiviazione e a SQL Server. Quando Pacemaker rileva un errore, i componenti di clustering gestiscono il trasferimento delle risorse nell'altro nodo.  

![Cluster SQL del disco condiviso Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 


> [!NOTE]
> A questo punto, l'integrazione di SQL Server con Pacemaker in Linux non è associata come con il cluster WSFC in Windows. Da SQL non è possibile avere informazioni sulla presenza del cluster, tutte le orchestrazioni si trovano all'esterno e il servizio viene controllato come istanza autonoma da Pacemaker. Inoltre, il nome della rete virtuale è specifico di WSFC e non esiste un equivalente in Pacemaker. Si prevede che @@servername e sys.servers restituiscano il nome del nodo, mentre dmvs sys.dm_os_cluster_nodes e sys.dm_os_cluster_properties del cluster non includeranno record. Per usare una stringa di connessione che punta a un nome del server in formato stringa e non usare l'indirizzo IP, dovranno registrare nel server DNS l'IP usato per creare la risorsa IP virtuale (come illustrato nelle sezioni seguenti) con il nome del server scelto.

## <a name="number-of-instances-and-nodes"></a>Numero di istanze e nodi

Una differenza fondamentale con SQL Server in Linux è data dalla presenza di una sola installazione di SQL Server per ogni server Linux. Tale installazione viene chiamata istanza. Ciò significa che, a differenza di Windows Server che supporta fino a 25 istanze del cluster di failover per ogni Windows Server Failover Cluster (WSFC), un'istanza del cluster di failover basata su Linux avrà una sola istanza. Questa istanza è anche un'istanza predefinita. In Linux non esiste il concetto di istanza denominata. 

Un cluster Pacemaker può avere solo fino a 16 nodi quando è coinvolto Corosync, quindi una singola istanza del cluster di failover può estendersi su un massimo di 16 server. Un'istanza del cluster di failover implementata con l'edizione Standard di SQL Server supporta fino a due nodi di un cluster, anche se il cluster Pacemaker consente un massimo di 16 nodi.

In un'istanza del cluster di failover di SQL Server l'istanza di SQL Server è attiva in un nodo o nell'altro.

## <a name="ip-address-and-name"></a>Indirizzo IP e nome
In un cluster Pacemaker di Linux ogni istanza del cluster di failover di SQL Server necessita di un proprio nome e indirizzo IP univoci. Se la configurazione dell'istanza del cluster di failover si estende su più subnet, sarà necessario un indirizzo IP per ogni subnet. Il nome e gli indirizzi IP univoci vengono usati per accedere all'istanza del cluster di failover in modo che non sia necessario che le applicazioni e gli utenti finali conoscano il server sottostante del cluster Pacemaker.

Il nome dell'istanza del cluster di failover in DNS deve corrispondere al nome della risorsa istanza del cluster di failover che viene creata nel cluster Pacemaker.
Sia il nome che l'indirizzo IP devono essere registrati in DNS.

## <a name="shared-storage"></a>Archiviazione condivisa
Tutte le istanze del cluster di failover, che si trovino in Linux o in Windows Server, richiedono qualche tipo di archiviazione condivisa. Questa risorsa di archiviazione viene presentata a tutti i server che possono ospitare l'istanza del cluster di failover, ma, in un determinato momento, solo un server può usare lo spazio di archiviazione per l'istanza del cluster di failover. Le opzioni disponibili per l'archiviazione condivisa in Linux sono:

- iSCSI
- File system di rete (NFS)
- SMB (Server Message Block) in Windows Server, che prevede opzioni leggermente diverse. Un'opzione attualmente non supportata per le istanze del cluster di failover basate su Linux è la possibilità di usare un disco locale per il nodo di TempDB, che è l'area di lavoro temporanea di SQL Server.

In una configurazione che si estende su più posizioni, ciò che viene archiviato in un data center deve essere sincronizzato con l'altro. In caso di failover, l'istanza del cluster di failover riuscirà a tornare online e lo spazio di archiviazione risulterà identico. A questo scopo, sarà necessario un metodo esterno per la replica di archiviazione, indipendentemente dal fatto che venga eseguita tramite l'hardware di archiviazione sottostante o un'utilità basata su software. 

>[!NOTE]
>Per SQL Server, le distribuzioni basate su Linux che usano i dischi presentati direttamente a un server devono essere formattate con XFS o EXT4. Gli altri file system attualmente non sono supportati. Eventuali modifiche verranno indicate qui.

Il processo di presentazione della risorsa di archiviazione condivisa è lo stesso per i diversi metodi supportati:

- Configurare la risorsa di archiviazione condivisa
- Montare la risorsa di archiviazione come cartella nei server che fungeranno da nodi del cluster Pacemaker per l'istanza del cluster di failover
- Se necessario, spostare i database di sistema SQL Server nella risorsa di archiviazione condivisa
- Verificare che SQL Server funzioni da ogni server connesso alla risorsa di archiviazione condivisa

Una delle principali differenze con SQL Server in Linux è che, nonostante sia possibile configurare il percorso predefinito dei file di log e dei dati dell'utente, i database di sistema devono sempre essere presenti in `/var/opt/mssql/data`. In Windows Server è possibile spostare i database di sistema, incluso TempDB. Ciò influisce sul modo in cui viene configurata la risorsa di archiviazione condivisa per un'istanza del cluster di failover.

I percorsi predefiniti per i database non di sistema possono essere modificati usando l'utilità `mssql-conf`. Per informazioni su come modificare le impostazione predefinite, vedere [Modificare il percorso predefinito della directory dei dati o dei log](sql-server-linux-configure-mssql-conf.md#datadir). È anche possibile archiviare la transazione e i dati di SQL Server in altre posizioni, purché la sicurezza sia appropriata anche se non si tratta di un percorso predefinito. Il percorso deve essere dichiarato.

Gli argomenti seguenti illustrano come configurare i tipi di archiviazione supportati per un'istanza del cluster di failover di SQL Server basata su Linux:

- [Configurare un'istanza del cluster di failover - iSCSI - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configurare un'istanza del cluster di failover - NFS - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configurare un'istanza del cluster di failover - SMB - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure-smb.md)
