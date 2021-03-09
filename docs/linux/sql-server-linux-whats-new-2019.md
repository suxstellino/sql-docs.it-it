---
title: Novità di SQL Server 2019 in Linux
description: Questo articolo illustra le novità di SQL Server 2019 in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 8cc766248f4dd2776a0367b2c0ac6e12f385433e
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464654"
---
# <a name="whats-new-for-sql-server-2019-on-linux"></a>Novità di SQL Server 2019 in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo descrive le funzionalità e i servizi principali disponibili per SQL Server 2019 in esecuzione in Linux. Per il download dei pacchetti e per informazioni sui problemi noti, vedere le [Note sulla versione ](sql-server-linux-release-notes-2019.md).

## <a name="ubuntu-1804-supported"></a>Supporto per Ubuntu 18.04

A partire da SQL Server 2019 CU3, Ubuntu 18,04 è ora supportato. Vedere l'articolo di avvio rapido [Installazione di SQL Server e creazione di un database in Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="rhel-8-supported"></a>Supporto per RHEL 8

A partire da SQL Server 2019 CU1, RHEL 8 è ora supportato. Vedere l'articolo di avvio rapido [Installazione di SQL Server e creazione di un database in Red Hat](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="updates"></a>Aggiornamenti

Gli aggiornamenti sono stati eseguiti in SQL Server 2019 in Linux:

| Nuova funzionalità o aggiornamento | Dettagli |
|:-----|:-----|
|Supporto della replica |[Replica di SQL Server in Linux](sql-server-linux-replication.md)
|Supporto di Microsoft Distributed Transaction Coordinator (MSDTC) |[Come configurare MSDTC in Linux](sql-server-linux-configure-msdtc.md) |
|Supporto OpenLDAP per provider AD di terze parti |[Esercitazione: Usare l'autenticazione di Azure Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md) |
|Machine Learning in Linux |[Configurare Machine Learning in Linux](sql-server-linux-setup-machine-learning.md) |
|Miglioramenti di `tempdb` | Per impostazione predefinita, una nuova installazione di SQL Server in Linux crea più file di dati `tempdb` in base al numero di core logici (con un massimo di 8 file di dati). Questo non vale per gli aggiornamenti sul posto di una versione principale o secondaria. Ogni file `tempdb` è di 8 MB, con un aumento automatico di 64 MB. Questo comportamento è simile all'installazione predefinita di SQL Server in Windows. |
| PolyBase in Linux | [Installare PolyBase](../relational-databases/polybase/polybase-linux-setup.md) in Linux per i connettori non Hadoop.<br/><br/>[Mapping dei tipi di PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Supporto di Change Data Capture (CDC) | Change Data Capture (CDC) è ora supportato in Linux per SQL Server 2019. |
| Registro Container Microsoft | Il [Registro Container Microsoft](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) sostituisce ora Docker Hub per le nuove immagini di contenitori Microsoft ufficiali, incluso [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| Contenitori non radice | In [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] è stata introdotta la possibilità di creare contenitori più sicuri avviando, per impostazione predefinita, il processo [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] come utente non radice. Per altri dettagli, vedere [Compilare ed eseguire contenitori di SQL Server come utente non radice](./sql-server-linux-docker-container-security.md#buildnonrootcontainer). |

## <a name="next-steps"></a>Passaggi successivi

Per installare SQL Server in Linux, usare una delle esercitazioni seguenti:

- [Eseguire l'installazione in Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true)
- [Eseguire l'installazione in SUSE Linux Enterprise Server](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true)
- [Eseguire l'installazione in Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true)
- [Esecuzione in Docker](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true)
- [Eseguire il provisioning di una macchina virtuale SQL in Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](sql-server-linux-faq.yml). Per informazioni su altri miglioramenti introdotti in SQL Server 2019, vedere [Novità di SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]