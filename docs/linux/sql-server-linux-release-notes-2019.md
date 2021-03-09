---
title: Note sulla versione di SQL Server 2019 in Linux
description: Questo articolo contiene le note sulla versione e le funzionalità supportate per SQL Server 2019 in esecuzione in Linux. Sono incluse le note sulla versione per la versione più recente e per diverse versioni precedenti.
author: VanMSFT
ms.author: vanto
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: c81ca4cb8d531626c7f5ae51f8b4e264d907d2ce
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464699"
---
# <a name="release-notes-for-sql-server-2019-on-linux"></a>Note sulla versione di SQL Server 2019 in Linux

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md)]

Le note sulla versione seguenti si applicano a SQL Server 2019 in esecuzione in Linux. Questo articolo è suddiviso in sezioni corrispondenti a ogni versione. Ogni versione ha un collegamento a un articolo del supporto che descrive le modifiche CU, oltre ai collegamenti ai download dei pacchetti Linux.

> [!TIP]
> Per informazioni sulle nuove funzionalità di Linux in SQL Server 2019, vedere [Novità di SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true#sql-server-on-linux).

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

## <a name="tools"></a>Strumenti

La maggior parte degli strumenti client esistenti per SQL Server può essere facilmente usata per SQL Server in esecuzione in Linux. Per usare in modo efficiente alcuni strumenti in Linux, potrebbe essere richiesta una versione specifica. Per un elenco completo di strumenti di SQL Server, vedere [Strumenti e utilità SQL per SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Cronologia delle versioni

La tabella seguente elenca la cronologia delle versioni di SQL Server 2019.

| Versione                   | Versione       | Data di rilascio |
|---------------------------|---------------|--------------|
| [CU9](#cu9)               | 15.0.4102.2   | 2021-02-10   |
| [CU8-GDR](#cu8-GDR)               | 15.0.4083.2  | 2021-01-12   |
| [CU8](#cu8)               | 15.0.4073.23  | 2020-10-07   |
| [CU7 (rimossa)](https://support.microsoft.com/help/4570012)     | 15.0.4063.15  | 2020-09-02   |
| [CU6](#cu6)               | 15.0.4053.23  | 2020-08-04   |
| [CU5](#cu5)               | 15.0.4043.16  | 2020-06-22   |
| [CU4](#cu4)               | 15.0.4033.1   | 2020-03-31   |
| [CU3](#cu3)               | 15.0.4023.6   | 2020-03-12   |
| [CU2](#cu2)               | 15.0.4013.40  | 2020-02-13   |
| [CU1](#cu1)               | 15.0.4003.23  | 07 gennaio 2020   |
| [GA](#ga)                 | 15.0.2000.5   | 4 novembre 2019   |
| [Release Candidate](#rc)  | 15.0.1900.25  | 21 agosto 2019   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Come installare gli aggiornamenti

Se è stato configurato il repository di aggiornamenti cumulativi (mssql-server-2019), quando si eseguono nuove installazioni si otterrà l'aggiornamento cumulativo più recente dei pacchetti di SQL Server. Se sono necessarie immagini del contenitore Docker, vedere le immagini ufficiali per [Microsoft SQL Server in Linux per il motore Docker](https://hub.docker.com/r/microsoft/mssql-server/). Per altre informazioni sulla configurazione del repository, vedere [Configurare i repository per SQL Server in Linux](sql-server-linux-change-repo.md).

Se si aggiornano pacchetti di SQL Server esistenti, eseguire il comando di aggiornamento appropriato per ogni pacchetto per ottenere l'aggiornamento cumulativo più recente. Per istruzioni dettagliate sull'aggiornamento per ogni pacchetto, vedere le guide all'installazione seguenti:

- [Installare il pacchetto di SQL Server](sql-server-linux-setup.md#upgrade)
- [Installare il pacchetto di ricerca full-text](sql-server-linux-setup-full-text-search.md)
- [Installare SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Installare il supporto per R e Python per Machine Learning Services di SQL Server 2019 in Linux](sql-server-linux-setup-machine-learning.md)
- [Installare il pacchetto PolyBase](../relational-databases/polybase/polybase-linux-setup.md)
- [Abilitare SQL Server Agent](sql-server-linux-setup-sql-agent.md)

## <a name="cu9-february-2021"></a><a id="cu9"></a> CU9 (febbraio 2021)

Si tratta della versione dell'aggiornamento cumulativo 9 (CU9) di SQL Server 2019 (15. x). La versione SQL Server motore di database per questa versione è 15.0.4102.2. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/5000642>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4102.2-4 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4102.2-4.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4102.2-4 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4102.2-4.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4102.2-4.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4102.2-4 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4102.2-4_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4102.2-4_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4102.2-4_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4102.2-4_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4102.2-4_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4102.2-4_amd64.deb)|

## <a name="cu8-gdr-january-2021"></a><a id="cu8-GDR"></a> CU8-GDR (gennaio 2021)

Questo è la versione Cumulative Update 8-GDR (CU8-GDR) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server corrispondente è 15.0.4083.2. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4083.2-15 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4083.2-15 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4083.2-15 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4083.2-15_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4083.2-15_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4083.2-15_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4083.2-15_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4083.2-15_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4083.2-15_amd64.deb)|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (settembre 2020)

Questo è l'aggiornamento cumulativo 8 (CU8) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4073.23. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4073.23-4 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4073.23-4 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4073.23-4 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4073.23-4_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4073.23-4_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4073.23-4_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4073.23-4_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4073.23-4_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4073.23-4_amd64.deb)|

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (luglio 2020)

Questo è l'aggiornamento cumulativo 6 (CU6) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4053.23. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4563110>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4053.23-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4053.23-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4053.23-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4053.23-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4053.23-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4053.23-2_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4053.23-2_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4053.23-2_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4053.23-2_amd64.deb)|

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (giugno 2020)

Questo è l'aggiornamento cumulativo 5 (CU5) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4043.16. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4552255>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4043.16-4 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4043.16-4 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4043.16-4 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4043.16-4_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4043.16-4_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4043.16-4_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4043.16-4_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4043.16-4_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4043.16-4_amd64.deb)|

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (aprile 2020)

Questo è l'aggiornamento cumulativo 4 (CU4) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4033.1. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4548597>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4033.1-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4033.1-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4033.1-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4033.1-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4033.1-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4033.1-2_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4033.1-2_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4033.1-2_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4033.1-2_amd64.deb)|

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (marzo 2020)

Questo è l'aggiornamento cumulativo 3 (CU3) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4023.6. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4538853>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>
>
> **Ubuntu 18.04** è ora supportato in SQL Server 2019 a partire da CU3. I collegamenti per l'installazione di pacchetti offline per Ubuntu indirizzano a pacchetti Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, fare riferimento al percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4023.6-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4023.6-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Pacchetto Ubuntu 18.04 Debian | 15.0.4023.6-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4023.6-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4023.6-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4023.6-2_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4023.6-2_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4023.6-2_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4023.6-2_amd64.deb)|

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2 (febbraio 2020)

Questa è la versione Cumulative Update 2 (CU2) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4013.40. Per informazioni sulle correzioni e sui miglioramenti, vedere <https://support.microsoft.com/help/4536075>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4013.40-8 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4013.40-8 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Pacchetto Ubuntu 16.04 Debian | 15.0.4013.40-8 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4013.40-8_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4013.40-8_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4013.40-8_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4013.40-8_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4013.40-8_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4013.40-8_amd64.deb)|

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (gennaio 2020)

Questa è la versione Cumulative Update 1 (CU1) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.4003.23. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere <https://support.microsoft.com/en-us/help/4527376>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire da CU1, i collegamenti per l'installazione offline dei pacchetti per Red Hat indirizzeranno ai pacchetti RHEL 8. Per scaricare i pacchetti RHEL 7, fare riferimento al percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2019/>

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.4003.23-3 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.4003.23-3 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Pacchetto Ubuntu 16.04 Debian | 15.0.4003.23-3 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4003.23-3_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4003.23-3_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4003.23-3_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4003.23-3_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4003.23-3_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4003.23-3_amd64.deb)|

## <a name="ga-november-2019"></a><a id="ga"></a> GA (novembre 2019)

Questa è la versione disponibile a livello generale (GA, General Availability) di SQL Server 2019 (15.x). La versione del motore di database di SQL Server per questa versione è 15.0.2000.5.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.2000.5-5 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.2000.5-5 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Pacchetto Ubuntu 16.04 Debian | 15.0.2000.5-5 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.2000.5-5_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.2000.5-5_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.2000.5-5_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.2000.5-5_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.2000.5-5_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.2000.5-5_amd64.deb)|

## <a name="release-candidate-august-2019"></a><a id="rc"></a> Release Candidate (agosto 2019)

Le sezioni seguenti elencano i percorsi dei pacchetti e i problemi noti per la versione finale candidata. Per altre informazioni sulle nuove funzionalità per Linux in SQL Server 2019, vedere [Novità di SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 15.0.1900.25-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Pacchetto SLES RPM | 15.0.1900.25-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM di estendibilità](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM di estendibilità Java](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Pacchetto Ubuntu 16.04 Debian | 15.0.1900.25-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server/mssql-server_15.0.1900.25-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.1900.25-1_amd64.deb)</br>[Pacchetto Debian di estendibilità](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.1900.25-1_amd64.deb)</br>[Pacchetto Debian di estendibilità Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.1900.25-1_amd64.deb)</br>[Pacchetto RPM PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.1900.25-1_amd64.deb)|

## <a name="known-issues"></a>Problemi noti

Le sezioni seguenti descrivono i problemi noti della versione GA (General Availability) di SQL Server 2019 (15.x) in Linux.

### <a name="general"></a>Generale

- La lunghezza del nome host in cui [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è installato non deve superare i 15 caratteri. 

    - **Soluzione**: sostituire il nome in /etc/hostname con uno di 15 caratteri o meno.

- Mettendo manualmente indietro l'ora di sistema, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] smetterà di aggiornare l'ora del sistema interno di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Soluzione**: Riavviare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Sono supportate solo le installazioni a istanza singola.

    - **Soluzione**: Se si vuole avere più di un'istanza in un determinato host, è consigliabile usare le macchine virtuali o i contenitori Docker. 

- Gestione configurazione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non riesce a connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux.

- La lingua predefinita dell'account di accesso **sa** è l'inglese.

    - **Soluzione**: Cambiare la lingua dell'account di accesso **sa** con l'istruzione **ALTER LOGIN**.

- Il provider OLEDB registra l'avviso seguente: `Failed to verify the Authenticode signature of 'C:\binn\msoledbsql.dll'. Signature verification of SQL Server DLLs will be skipped. Genuine copies of SQL Server are signed. Failure to verify the Authenticode signature might indicate that this is not an authentic release of SQL Server. Install a genuine copy of SQL Server or contact customer support.`

   - **Soluzione**: Non è richiesta alcuna azione. Il provider OLEDB viene firmato con SHA256. Il motore di database di SQL Server non convalida correttamente il file .dll firmato.

### <a name="databases"></a>Database

- Non è possibile spostare il database master con l'utilità mssql-conf. È possibile spostare altri database di sistema con mssql-conf.

- Quando si ripristina un database di cui è stato eseguito un backup in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Windows, è necessario usare la clausola **WITH MOVE** nell'istruzione Transact-SQL.

- Alcuni algoritmi (suite di crittografia) per Transport Layer Security (TLS) non funzionano correttamente con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux. Ne derivano errori di connessione quando si prova a connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], oltre a problemi a stabilire le connessioni tra repliche nei gruppi di disponibilità elevata.

   - **Soluzione**: modificare lo script di configurazione **mssql.conf** per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux per disabilitare le suite di crittografia problematiche, seguendo questa procedura:

      1. Aggiungere il codice seguente a /var/opt/mssql/mssql.conf.

          ```
          [network]
          tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
          ```

         > [!NOTE]
         > Nel codice precedente `!` nega l'espressione. Questo indica a OpenSSL di non usare il pacchetto di crittografia seguente.  

      1. Riavviare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con il comando seguente.

          ```bash
          sudo systemctl restart mssql-server
          ```

- I database di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] in Windows che usano OLTP in memoria non possono essere ripristinati in SQL Server 2019 (15.x) in Linux. Per ripristinare un database di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] che usa OLTP in memoria, aggiornare i database a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], SQL Server 2017 o SQL Server 2019 in Windows prima di spostarli in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux tramite backup/ripristino o scollegamento/collegamento.

- L'autorizzazione utente **AMMINISTRAZIONE OPERAZIONI BULK** per il momento non è supportata in Linux.

### <a name="networking"></a>Rete

Le funzionalità che coinvolgono le connessioni TCP in uscita dal processo sqlservr, ad esempio i server collegati o i gruppi di disponibilità, potrebbero non funzionare se vengono soddisfatte entrambe le condizioni seguenti:

1. Il server di destinazione è specificato come nome host e non come indirizzo IP.

1. L'istanza di origine ha IPv6 disabilitato nel kernel. Per verificare se il sistema ha IPv6 abilitato nel kernel, è necessario che vengano superati tutti i test seguenti:

   - `cat /proc/cmdline` visualizzerà la riga di comando di avvio del kernel corrente. L'output non deve contenere `ipv6.disable=1`.
   - La directory /proc/sys/net/ipv6/ deve esistere.
   - Un programma C che chiama `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)` deve avere esito positivo. Syscall deve restituire fd != -1 e non deve verificarsi l'errore EAFNOSUPPORT.

L'errore esatto dipende dalla funzionalità. Per i server collegati, viene presentato come errore di timeout di accesso. Per i gruppi di disponibilità, il DDL `ALTER AVAILABILITY GROUP JOIN` nel database secondario avrà esito negativo dopo 5 minuti con un errore di timeout della configurazione del download.

Per ovviare al problema, effettuare una delle seguenti operazioni:

1. Usare gli IP invece dei nomi host per specificare la destinazione della connessione TCP.

1. Abilitare IPv6 nel kernel rimuovendo `ipv6.disable=1` dalla riga di comando di avvio. La modalità di esecuzione di questa operazione dipende dalla distribuzione di Linux e dal bootloader, ad esempio grub. Se si vuole disabilitare IPv6, è comunque possibile farlo impostando `net.ipv6.conf.all.disable_ipv6 = 1` nella configurazione `sysctl` (ad esempio, `/etc/sysctl.conf`). Questa operazione impedirà comunque alla scheda di rete del sistema di ottenere un indirizzo IPv6, ma consentirà di usare le funzionalità di sqlservr.

#### <a name="network-file-system-nfs"></a>File system di rete (NFS)
Se si usano condivisioni di rete **NFS (Network File System, file system di rete)** nell'ambiente di produzione, tenere presenti i requisiti di supporto seguenti:

- Usare NFS versione **4.2 o successiva**. Le versioni precedenti di NFS non supportano le funzionalità necessarie, ad esempio `fallocate` e la creazione di file sparse, comuni ai file system moderni.
- Individuare solo le directory **/var/opt/mssql** nel montaggio NFS. Gli altri file, ad esempio i file binari di sistema di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], non sono supportati.
- Assicurarsi che i client NFS usino l'opzione `nolock` per il montaggio della condivisione remota.

### <a name="localization"></a>Localizzazione

- Se le impostazioni locali non sono in inglese (en_us) durante l'installazione, è necessario usare la codifica UTF-8 nella sessione/terminale Bash. Se si usa la codifica ASCII, potrebbe essere visualizzato un errore simile al seguente:

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Se non è possibile usare la codifica UTF-8, eseguire il programma di installazione usando la variabile di ambiente MSSQL_LCID per specificare la lingua scelta.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- Quando si eseguono la configurazione di mssql-conf e un'installazione non in inglese di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vengono visualizzati caratteri estesi non corretti dopo il testo "Configurazione di SQL Server..." localizzato. Invece, per le installazioni non basate sull'alfabeto latino, la frase potrebbe mancare completamente. La frase mancante dovrebbe corrispondere alla stringa localizzata seguente: "Il PID di licenze è stato elaborato. La nuova edizione è [edizione \<Name\>]". La stringa viene visualizzata solo a scopo informativo. Il problema verrà risolto nel successivo aggiornamento cumulativo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per tutte le lingue. Il problema non influisce in alcun modo sull'installazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

#### <a name="full-text-search"></a>Ricerca full-text

- Non tutti i filtri sono disponibili con questa versione, inclusi i filtri per i documenti di Office. Per un elenco di filtri supportati, vedere [Installare la ricerca full-text di SQL Server in Linux](sql-server-linux-setup-full-text-search.md#filters).

### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

- In questa versione il pacchetto **mssql-server-is** non è supportato in SUSE. È attualmente supportato in Ubuntu e in Red Hat Enterprise Linux (RHEL).

- Con [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] in Linux CTP 2.1 Refresh e versioni successive, i pacchetti di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] possono usare le connessioni ODBC in Linux. Questa funzionalità è stata testata con i driver ODBC di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e MySQL, ma è previsto che funzioni anche con i driver ODBC Unicode che osservano la specifica ODBC. In fase di progettazione è possibile specificare un DSN o una stringa di connessione per la connessione ai dati ODBC. È anche possibile usare l'autenticazione di Windows. Per altre informazioni, vedere il [post di blog che annuncia il supporto ODBC in Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

- Le funzionalità seguenti non sono supportate in questa versione quando si eseguono pacchetti SSIS in Linux:
  - Database del catalogo di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]
  - Esecuzione pianificata dei pacchetti tramite SQL Agent
  - Autenticazione di Windows
  - Componenti di terze parti
  - Change Data Capture (CDC)
  - Aumentare il numero di istanze di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]
  - Feature Pack di Azure per SSIS
  - Supporto di Hadoop e HDFS
  - Microsoft Connector for SAP BW

Per un elenco di componenti SSIS predefiniti non attualmente supportati oppure supportati con limitazioni, vedere [Limitazioni e problemi noti per SSIS in Linux](sql-server-linux-ssis-known-issues.md#components).

Per altre informazioni su SSIS in Linux, vedere gli articoli seguenti:
-   [Post di blog che annuncia il supporto SSIS per Linux](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/).
-   [Installare SQL Server Integration Services (SSIS) in Linux](sql-server-linux-setup-ssis.md)
-   [Estrarre, trasformare e caricare i dati in Linux con SSIS](sql-server-linux-migrate-ssis.md)

### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

Le limitazioni seguenti si applicano a [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] in Windows connesso a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux.

- I piani di manutenzione non sono supportati.

- Il data warehouse di gestione e l'agente di raccolta dati in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] non sono supportati. 

- I componenti dell'interfaccia utente di [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] con le opzioni Autenticazione di Windows o Registro eventi di Windows non funzionano con Linux. È comunque possibile usare queste funzionalità con altre opzioni, ad esempio gli account di accesso SQL. 

- Non è possibile modificare il numero di file di log da conservare.

## <a name="next-steps"></a>Passaggi successivi

Per iniziare, vedere gli argomenti di avvio rapido seguenti:

- [Eseguire l'installazione in Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Eseguire l'installazione in SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Eseguire l'installazione in Ubuntu](quickstart-install-connect-ubuntu.md)
- [Esecuzione in Docker](quickstart-install-connect-ubuntu.md)
- [Eseguire il provisioning di una macchina virtuale SQL in Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Esecuzione e connessione - Cloud](quickstart-install-connect-clouds.md)

Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](sql-server-linux-faq.yml).
