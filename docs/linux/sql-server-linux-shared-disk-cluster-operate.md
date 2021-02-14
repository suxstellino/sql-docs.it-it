---
title: Failover manuale di un'istanza del cluster di failover - SQL Server in Linux
description: Informazioni su come consentire manualmente l'errore di un'istanza di cluster di failover in SQL Server in Linux, in particolare per Red Hat Linux Enterprise, Ubuntu e Suse Linux Enterprise Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 93bff6d536a6c1c5ad4eb8d8805fefb706377594
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346289"
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>Gestione di un'istanza del cluster di failover - SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come gestire un'istanza del cluster di failover di SQL Server in Linux. Se non è stata creata un'istanza del cluster di failover di SQL Server in Linux, vedere [Configurare un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure.md). 

## <a name="failover"></a>Failover

Il failover per le istanze del cluster di failover è simile a quello di un cluster di failover di Windows Server (WSFC, Windows Server Failover Cluster). Se nel nodo del cluster che ospita l'istanza del cluster di failover si verifica un errore, l'istanza del cluster di failover esegue automaticamente il failover in un altro nodo. A differenza di un cluster WSFC, non esiste alcun modo per impostare proprietari preferiti. Il nodo che costituirà il nuovo host per l'istanza del cluster di failover viene quindi scelto da Pacemaker.

In alcuni casi può essere necessario eseguire il failover di un'istanza del cluster di failover in un altro nodo. Il processo non è uguale a quello delle istanze del cluster di failover in un cluster WSFC. In un cluster WSFC il failover delle risorse viene eseguito a livello di ruolo. In Pacemaker si sceglie la risorsa da spostare e, se tutti i vincoli sono corretti, anche tutti gli altri elementi vengono spostati. 

La modalità di failover dipende dalla distribuzione di Linux. Seguire le istruzioni per la distribuzione di Linux in uso.

- [RHEL o Ubuntu](#manual-failover-rhel-or-ubuntu)
- [SLES](#manual-failover-sles)

## <a name="manual-failover-rhel-or-ubuntu"></a>Failover manuale (RHEL o Ubuntu)

Per eseguire un failover manuale, nei server Red Hat Enterprise Linux (RHEL) o Ubuntu eseguire la procedura seguente.
1.  Immettere il comando seguente: 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName> è il nome della risorsa di Pacemaker per l'istanza del cluster di failover di SQL Server.

   \<NewHostNode> è il nome del nodo del cluster in cui si vuole ospitare l'istanza. 

   Non si riceverà alcuna conferma.

2.  Durante un failover manuale, Pacemaker crea un vincolo di percorso per la risorsa scelta per lo spostamento manuale. Per visualizzare questo vincolo, eseguire `sudo pcs constraint`.

3.  Al termine del failover, rimuovere il vincolo con il comando `sudo pcs resource clear <FCIResourceName>`. 

\<FCIResourceName> è il nome della risorsa di Pacemaker per l'istanza del cluster di failover. 

## <a name="manual-failover-sles"></a>Failover manuale (SLES)


In SUSE Linux Enterprise Server (SLES) usare il comando `migrate` per eseguire manualmente il failover di un'istanza del cluster di failover di SQL Server. Ad esempio:

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName> è il nome della risorsa per l'istanza del cluster di failover. 

\<NewHostNode> è il nome del nuovo host di destinazione. 


<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

--->

## <a name="next-steps"></a>Passaggi successivi

- [Configurare un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
