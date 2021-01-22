---
title: Note sulla versione di SQL Server 2017 in Linux
description: Questo articolo contiene le note sulla versione e le funzionalità supportate per SQL Server 2017 in esecuzione in Linux. Sono incluse le note sulla versione per la versione più recente e per diverse versioni precedenti.
author: VanMSFT
ms.author: vanto
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1314744f-fcaf-46db-800e-2918fa7e1b6c
ms.openlocfilehash: 369c52eeb66a3d8ca5042f54d71ea9001d30b194
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193038"
---
# <a name="release-notes-for-sql-server-2017-on-linux"></a>Note sulla versione di SQL Server 2017 in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Le note sulla versione seguenti si applicano a [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] in esecuzione in Linux. Questo articolo è suddiviso in sezioni corrispondenti a ogni versione. La versione disponibile a livello generale include informazioni dettagliate sul supporto e sui problemi noti elencati. Ogni aggiornamento cumulativo (CU, Cumulative Update) o aggiornamento pubblico (GDR, General Distribution Release) ha un collegamento a un articolo del supporto che descrive le modifiche dell'aggiornamento cumulativo, oltre ai collegamenti dei download dei pacchetti Linux.

> [!TIP]
> Queste note sulla versione sono specifiche delle versioni di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Per altre informazioni sul nuovo [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], vedere [Note sulla versione dell'anteprima di SQL Server 2019 in Linux](sql-server-linux-release-notes-2019.md?view=sql-server-ver15&preserve-view=true).

## <a name="supported-platforms"></a>Piattaforme supportate

| Piattaforma | File system | Guida all'installazione |
|-----|-----|-----|
| Red Hat Enterprise Linux 7.3 - 7.8 o 8.0 - 8.3 Server | XFS o EXT4 | [Guida all'installazione](quickstart-install-connect-red-hat.md) | 
| SUSE Enterprise Linux Server v12 SP2 - SP5 | XFS o EXT4 | [Guida all'installazione](quickstart-install-connect-suse.md) |
| Ubuntu 16.04 LTS, 18.04 LTS | XFS o EXT4 | [Guida all'installazione](quickstart-install-connect-ubuntu.md) | 
| Docker Engine 1.8+ in Windows, Mac o Linux | N/D | [Guida all'installazione](quickstart-install-connect-docker.md) | 

> [!TIP]
> Per altre informazioni, vedere i [requisiti di sistema](sql-server-linux-setup.md#system) per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux. Per i criteri di supporto tecnico più recenti per [!INCLUDE[ssSQL17](../includes/sssql17-md.md)], vedere [Criteri di supporto tecnico per Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

## <a name="tools"></a>Strumenti

La maggior parte degli strumenti client esistenti per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può essere facilmente usata per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in esecuzione in Linux. Per usare in modo efficiente alcuni strumenti in Linux, potrebbe essere richiesta una versione specifica. Per un elenco completo di strumenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vedere [Strumenti e utilità SQL per SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Cronologia delle versioni

Nella tabella seguente viene elencata la cronologia delle versioni per [!INCLUDE[ssSQL17](../includes/sssql17-md.md)].

| Versione               | Versione       | Data di rilascio |
|-----------------------|---------------|--------------|
| [CU22-GDR](#CU22)         | 14.0.3370.1  | 2021-01-12 |
| [CU22](#CU22)         | 14.0.3356.20  | 2020-09-10   |
| [CU21](#CU21)         | 14.0.3335.7   | 2020-07-01   |
| [CU20](#CU20)         | 14.0.3294.2   | 2020-04-10   |
| [CU19](#CU19)         | 14.0.3281.6   | 2020-02-05   |
| [CU18](#CU18)         | 14.0.3257.3   | 2019-12-09   |
| [CU17](#CU17)         | 14.0.3238.1   | 2019-10-08   |
| [CU16](#CU16)         | 14.0.3223.3   | 2019-08-01   |
| [CU15](#CU15)         | 14.0.3162.1   | 23/05/2019   |
| [CU14](#CU14)         | 14.0.3076.1   | 25/03/2019   |
| [CU13](#CU13)         | 14.0.3048.4   | 18/12/2018   |
| [CU12](#CU12)         | 14.0.3045.24  | 24/10/2018   |
| [CU11](#CU11)         | 14.0.3038.14  | 20/09/2018   |
| [CU10](#CU10)         | 14.0.3037.1   | 27/08/2018   |
| [CU9-GDR2](#CU9-GDR2) | 14.0.3035.2   | 18/08/2018   |
| [GDR2](#GDR2)         | 14.0.2002.14  | 18/08/2018   |
| [CU9](#CU9)           | 14.0.3030.27  | 18/07/2018   |
| [CU8](#CU8)           | 14.0.3029.16  | 21/06/2018   |
| [CU7](#CU7)           | 14.0.3026.27  | 24/05/2018   |
| [CU6](#CU6)           | 14.0.3025.34  | 19/04/2018   |
| [CU5](#CU5)           | 14.0.3023.8   | 20/03/2018   |
| [CU4](#CU4)           | 14.0.3022.28  | 20/02/2018   |
| [CU3](#CU3)           | 14.0.3015.40  | 03/01/2018   |
| [GDR1](#GDR1)         | 14.0.2000.63  | 03/01/2018   |
| [CU2](#CU2)           | 14.0.3008.27  | 28/11/2017   |
| [CU1](#CU1)           | 14.0.3006.16  | 24/10/2017   |
| [GA](#GA)             | 14.0.1000.169 | 02/10/2017   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Come installare gli aggiornamenti

Se è stato configurato il repository di aggiornamenti cumulativi (**mssql-server-2017**), quando si eseguono nuove installazioni, si otterrà l'aggiornamento cumulativo più recente dei pacchetti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il repository di aggiornamenti cumulativi è quello predefinito per tutti gli articoli sulle installazioni dei pacchetti per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux. Se è stato configurato il repository di aggiornamenti pubblici (**mssql-server-2017-gdr**), si otterranno solo gli aggiornamenti della sicurezza critici rilasciati dopo la disponibilità generale. Se sono necessari aggiornamenti cumulativi o aggiornamenti pubblici del contenitore Docker, vedere le immagini ufficiali per [Microsoft SQL Server in Linux per il motore Docker](https://hub.docker.com/r/microsoft/mssql-server). Per altre informazioni sulla configurazione del repository, vedere [Configurare i repository per SQL Server in Linux](sql-server-linux-change-repo.md).

Se si aggiornano pacchetti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] esistenti, eseguire il comando di aggiornamento appropriato per ogni pacchetto per ottenere l'aggiornamento cumulativo più recente. Per istruzioni dettagliate sull'aggiornamento per ogni pacchetto, vedere le guide all'installazione seguenti:

- [Installare il pacchetto di SQL Server](sql-server-linux-setup.md#upgrade)
- [Installare il pacchetto di ricerca full-text](sql-server-linux-setup-full-text-search.md)
- [Installare SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Abilitare SQL Server Agent](sql-server-linux-setup-sql-agent.md)

## <a name="cu22-gdr-january-2021"></a><a id="CU22-GDR"></a> CU22-GDR (gennaio 2021)

Questa è la versione Cumulative Update 22-GDR (CU22-GDR) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] corrispondente è 14.0.3370.1. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> **Ubuntu 18.04** e **RHEL 8** sono supportati in SQL Server 2017 a partire dalla versione CU20.
>
> I collegamenti per l'installazione di pacchetti offline per Ubuntu fanno riferimento a pacchetti Ubuntu 18.04, ad eccezione del pacchetto SSIS, che non è disponibile per Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, usare il percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> I collegamenti per l'installazione di pacchetti offline per Red Hat fanno riferimento a pacchetti RHEL 8, ad eccezione del pacchetto SSIS che non è disponibile per RHEL 8. Per scaricare i pacchetti RHEL 7 usare il percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3370.1-23-18 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) |
| Pacchetto SLES RPM | 14.0.3370.1-18 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm) | 
| Pacchetto Ubuntu 18.04 Debian | 14.0.3370.1-18 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3370.1-18_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3370.1-18_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3370.1-18_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-september-2020"></a><a id="CU22"></a> CU22 (settembre 2020)

Questa è la versione Cumulative Update 22 (CU22) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per questa versione è 14.0.3356.20. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> **Ubuntu 18.04** e **RHEL 8** sono supportati in SQL Server 2017 a partire dalla versione CU20.
>
> I collegamenti per l'installazione di pacchetti offline per Ubuntu fanno riferimento a pacchetti Ubuntu 18.04, ad eccezione del pacchetto SSIS, che non è disponibile per Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, usare il percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> I collegamenti per l'installazione di pacchetti offline per Red Hat fanno riferimento a pacchetti RHEL 8, ad eccezione del pacchetto SSIS che non è disponibile per RHEL 8. Per scaricare i pacchetti RHEL 7 usare il percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3356.20-23 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3356.20-23 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm) | 
| Pacchetto Ubuntu 18.04 Debian | 14.0.3356.20-23 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3356.20-23_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3356.20-23_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3356.20-23_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu21-july-2020"></a><a id="CU21"></a> CU21 (luglio 2020)

Questa è la versione Cumulative Update 21 (CU21) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per questa versione è 14.0.3335.7. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere <https://support.microsoft.com/help/4557397>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> **Ubuntu 18.04** e **RHEL 8** sono supportati in SQL Server 2017 a partire dalla versione CU20.
>
> I collegamenti per l'installazione di pacchetti offline per Ubuntu fanno riferimento a pacchetti Ubuntu 18.04, ad eccezione del pacchetto SSIS, che non è disponibile per Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, usare il percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> I collegamenti per l'installazione di pacchetti offline per Red Hat fanno riferimento a pacchetti RHEL 8, ad eccezione del pacchetto SSIS che non è disponibile per RHEL 8. Per scaricare i pacchetti RHEL 7 usare il percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3335.7-17 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3335.7-17 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm) | 
| Pacchetto Ubuntu 18.04 Debian | 14.0.3335.7-17 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3335.7-17_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3335.7-17_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3335.7-17_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu20-april-2020"></a><a id="CU20"></a> CU20 (aprile 2020)

Questa è la versione Cumulative Update 20 (CU20) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. La versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] per questa versione è 14.0.3294.2. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere <https://support.microsoft.com/help/4541283>.

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> **Ubuntu 18.04** e **RHEL 8** sono supportati in SQL Server 2017 a partire dalla versione CU20.
>
> I collegamenti per l'installazione di pacchetti offline per Ubuntu fanno riferimento a pacchetti Ubuntu 18.04, ad eccezione del pacchetto SSIS, che non è disponibile per Ubuntu 18.04. Per scaricare i pacchetti Ubuntu 16.04, usare il percorso di download <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> I collegamenti per l'installazione di pacchetti offline per Red Hat fanno riferimento a pacchetti RHEL 8, ad eccezione del pacchetto SSIS che non è disponibile per RHEL 8. Per scaricare i pacchetti RHEL 7 usare il percorso di download <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3294.2-27 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3294.2-27 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm) | 
| Pacchetto Ubuntu 18.04 Debian | 14.0.3294.2-27 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3294.2-27_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3294.2-27_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3294.2-27_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu19-february-2020"></a><a id="CU19"></a> CU19 (febbraio 2020)

Questo è l'aggiornamento cumulativo 19 (CU19) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3281.6. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4535007](https://support.microsoft.com/help/4535007).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3281.6-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3281.6-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3281.6-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3281.6-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3281.6-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3281.6-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu18-december-2019"></a><a id="CU18"></a> CU18 (dicembre 2019)

Questa è la versione Cumulative Update 18 (CU18) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3257.3. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4527377](https://support.microsoft.com/help/4527377).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3257.3-13 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3257.3-13 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3257.3-13 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3257.3-13_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3257.3-13_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3257.3-13_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

### <a name="added-support"></a>Supporto aggiuntivo

- A partire da CU18, Change Data Capture (CDC) è supportato con SQL Server 2017 in Linux.
- A partire da CU18, la replica transazionale è supportata con SQL Server 2017 in Linux.

### <a name="remarks"></a>Osservazioni

I contenitori di SQL Server 2017 ora hanno un nuovo modello di assegnazione di tag, come descritto di seguito con esempi.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-<update>-<Linux Distribution>-<Linux Distribution Version>`

  Verrà eseguito il pull dell'immagine del contenitore con la combinazione descritta nel tag.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-latest`

    Verrà eseguito il pull della versione più recente di SQL Server nella versione più recente di Ubuntu supportata.

**Esempi:**

`mcr.microsoft.com/mssql/server:2017-CU18-ubuntu-16.04`

Verrà eseguito il pull di SQL Server 2017 CU18 in base al contenitore Ubuntu 16.04.

`mcr.microsoft.com/mssql/server:2017-latest`

Verrà eseguito il pull della versione più recente di SQL Server 2017 (CU18 al momento della stesura di questo articolo) in base al contenitore Ubuntu 16.04.

> [!NOTE]
> In futuro non saranno più pubblicati contenitori con altri modelli di assegnazione di tag per i contenitori di SQL Server 2017.


## <a name="cu17-october-2019"></a><a id="CU17"></a> CU17 (ottobre 2019)

Questa è la versione Cumulative Update 17 (CU17) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3238.1. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4515579](https://support.microsoft.com/help/4515579).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3238.1-19 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3238.1-19 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3238.1-19 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3238.1-19_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3238.1-19_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3238.1-19_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu16-august-2019"></a><a id="CU16"></a> CU16 (agosto 2019)

Questa è la versione Cumulative Update 16 (CU16) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3223.3. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4508218](https://support.microsoft.com/help/4508218).

### <a name="whats-new"></a>Novità

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
| Supporto di MSDTC | Supporto per Microsoft Distributed Transaction Coordinator (MSDTC) per SQL Server 2017. Per altre informazioni, vedere [Come configurare Microsoft Distributed Transaction Coordinator (MSDTC) in Linux](sql-server-linux-configure-msdtc.md). |

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3223.3-15 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3223.3-15 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3223.3-15 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3223.3-15_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3223.3-15_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3223.3-15_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu15-may-2019"></a><a id="CU15"></a> CU15 (maggio 2019)

Questa è la versione Cumulative Update 15 (CU15) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3162.1. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4498951](https://support.microsoft.com/help/4498951).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3162.1-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3162.1-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3162.1-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3162.1-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3162.1-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3162.1-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu14-mar-2019"></a><a id="CU14"></a> CU14 (marzo 2019)

Questa è la versione Cumulative Update 14 (CU14) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3076.1. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4484710](https://support.microsoft.com/help/4484710).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3076.1-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3076.1-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3076.1-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3076.1-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3076.1-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3076.1-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu13-dec-2018"></a><a id="CU13"></a> CU13 (dicembre 2018)

Questa è la versione Cumulative Update 13 (CU13) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3048.4. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4466404](https://support.microsoft.com/help/4466404).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3048.4-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3048.4-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3048.4-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3048.4-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3048.4-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3048.4-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu12-oct-2018"></a><a id="CU12"></a> CU12 (ottobre 2018)

Questa è la versione Cumulative Update 12 (CU12) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3045.24. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4464082](https://support.microsoft.com/help/4464082).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3045.24-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3045.24-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3045.24-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3045.24-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3045.24-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3045.24-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu11-sept-2018"></a><a id="CU11"></a> CU11 (settembre 2018)

Questa è la versione Cumulative Update 11 (CU11) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3038.14. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4462262](https://support.microsoft.com/help/4462262).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3038.14-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3038.14-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3038.14-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3038.14-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3038.14-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3038.14-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu10-aug-2018"></a><a id="CU10"></a> CU10 (agosto 2018)

Questa è la versione Cumulative Update 10 (CU10) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3037.1. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4342123](https://support.microsoft.com/help/4342123).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3037.1-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3037.1-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3037.1-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3037.1-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3037.1-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3037.1-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu9-gdr2-aug-2018"></a><a id="CU9-GDR2"></a> CU9-GDR2 (agosto 2018)

Questo è un aggiornamento della sicurezza che include anche l'aggiornamento cumulativo rilasciato in precedenza (CU9) per [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3035.2. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4293805](https://support.microsoft.com/help/4293805).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3035.2-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm)| 
| Pacchetto SLES RPM | 14.0.3035.2-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3035.2-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3035.2-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3035.2-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3035.2-1_amd64.deb)<br/> |

## <a name="gdr2-aug-2018"></a><a id="GDR2"></a> GDR2 (agosto 2018)

Questo è un aggiornamento della sicurezza che include solo le correzioni per la sicurezza GDR2 (e GDR1) per [!INCLUDE[ssSQL17](../includes/sssql17-md.md)].  Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.2002.14. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4293803](https://support.microsoft.com/help/4293803).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.2002.14-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.2002.14-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.2002.14-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2002.14-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2002.14-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2002.14-1_amd64.deb) |

## <a name="cu9-jul-2018"></a><a id="CU9"></a> CU9 (luglio 2018)

Questa è la versione Cumulative Update 9 (CU9) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3030.27. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4341265](https://support.microsoft.com/help/4341265).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3030.27-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3030.27-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3030.27-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3030.27-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3030.27-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3030.27-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu8-jun-2018"></a><a id="CU8"></a> CU8 (giugno 2018)

Questa è la versione Cumulative Update 8 (CU8) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3029.16. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4338363](https://support.microsoft.com/help/4338363).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3029.16-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3029.16-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3029.16-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3029.16-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3029.16-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3029.16-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu7-may-2018"></a><a id="CU7"></a> CU7 (maggio 2018)

Questa è la versione Cumulative Update 7 (CU7) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3026.27. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4229789](https://support.microsoft.com/help/4229789).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3026.27-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3026.27-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3026.27-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3026.27-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3026.27-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3026.27-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu6-apr-2018"></a><a id="CU6"></a> CU6 (aprile 2018)

Questa è la versione Cumulative Update 6 (CU6) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3025.34. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4101464](https://support.microsoft.com/help/4101464).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3025.34-3 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3025.34-3 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3025.34-3 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu5-mar-2018"></a><a id="CU5"></a> CU5 (marzo 2018)

Questa è la versione Cumulative Update 5 (CU5) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3023.8. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4092643](https://support.microsoft.com/help/4092643).

### <a name="known-upgrade-issue"></a>Problema di aggiornamento noto

Quando si esegue l'aggiornamento da una versione precedente a CU5, è possibile che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non venga avviato con l'errore seguente:

```
Error: 4860, Severity: 16, State: 1.
Cannot bulk load. The file "C:\Install\SqlTraceCollect.dtsx" does not exist or you don't have file access rights.
Error: 912, Severity: 21, State: 2.
Script level upgrade for database 'master' failed because upgrade step 'msdb110_upgrade.sql' encountered error 200, state
```

Per risolvere questo errore, abilitare SQL Server Agent e riavviare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con i comandi seguenti:

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
sudo systemctl start mssql-server
```

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3023.8-5 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3023.8-5 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3023.8-5 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3023.8-5_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3023.8-5_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3023.8-5_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu4-feb-2018"></a><a id="CU4"></a> CU4 (febbraio 2018)

Questa è la versione Cumulative Update 4 (CU4) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3022.28. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4056498](https://support.microsoft.com/help/4056498).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

> [!NOTE]
> A partire dalla versione CU4, SQL Server Agent non viene più installato come pacchetto separato. Viene installato con il pacchetto del motore e deve essere abilitato per l'uso.

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3022.28-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3022.28-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3022.28-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3022.28-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3022.28-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3022.28-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="gdr1-jan-2018"></a><a id="GDR1"></a> GDR1 (gennaio 2018)

Questo è un aggiornamento della sicurezza che include solo le correzioni per la sicurezza GDR1 per [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.2000.63. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4057122](https://support.microsoft.com/help/4057122).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.2000.63-3 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.2000.63-3 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.2000.63-3 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2000.63-3_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2000.63-3_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2000.63-3_amd64.deb) |

## <a name="cu3-jan-2018"></a><a id="CU3"></a> CU3 (gennaio 2018)

Questa è la versione Cumulative Update 3 (CU3) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3015.40. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4052987](https://support.microsoft.com/help/4052987).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3015.40-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3015.40-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3015.40-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3015.40-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3015.40-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3015.40-1_amd64.deb)</br>[Pacchetto Debian di SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3015.40-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu2-nov-2017"></a><a id="CU2"></a> CU2 (novembre 2017)

Questa è la versione Cumulative Update 2 (CU2) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3008.27. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/4052574](https://support.microsoft.com/help/4052574).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3008.27-1 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3008.27-1 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3008.27-1 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3008.27-1_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3008.27-1_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3008.27-1_amd64.deb)</br>[Pacchetto Debian di SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3008.27-1_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu1-oct-2017"></a><a id="CU1"></a> CU1 (ottobre 2017)

Questa è la versione Cumulative Update 1 (CU1) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.3006.16. Per informazioni sulle correzioni e sui miglioramenti apportati in questa versione, vedere [https://support.microsoft.com/help/KB4053439](https://support.microsoft.com/help/4038634).

### <a name="package-details"></a>Dettagli del pacchetto

Per le installazioni dei pacchetti manuali o offline, è possibile scaricare i pacchetti RPM e Debian con le informazioni riportate nella tabella seguente:

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.3006.16-3 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.3006.16-3 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.3006.16-3 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3006.16-3_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3006.16-3_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3006.16-3_amd64.deb)</br>[Pacchetto Debian di SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3006.16-3_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="ga-oct-2017"></a><a id="GA"></a> GA (ottobre 2017)

Questa è la versione disponibile a livello generale (GA, General Availability) di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Il numero di questa versione di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] è 14.0.1000.169.

### <a name="package-details"></a>Dettagli del pacchetto

I dettagli del pacchetto e i percorsi di download per i pacchetti RPM e Debian sono elencati nella tabella seguente. Non è necessario scaricare questi pacchetti direttamente se si usano le procedure delle guide di installazione seguenti:

- [Installare il pacchetto di SQL Server](sql-server-linux-setup.md)
- [Installare il pacchetto di ricerca full-text](sql-server-linux-setup-full-text-search.md)
- [Installare il pacchetto di SQL Server Agent](sql-server-linux-setup-sql-agent.md)
- [Installare SQL Server Integration Services](sql-server-linux-setup-ssis.md)

| Pacchetto | Versione pacchetto | Download |
|-----|-----|-----|
| Pacchetto Red Hat RPM | 14.0.1000.169-2 | [Pacchetto RPM del motore](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Pacchetto SLES RPM | 14.0.1000.169-2 | [Pacchetto RPM del motore mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto RPM a disponibilità elevata](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto RPM di ricerca full-text](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Pacchetto RPM di SQL Server Agent](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm) | 
| Pacchetto Ubuntu 16.04 Debian | 14.0.1000.169-2 | [Pacchetto Debian del motore](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.1000.169-2_amd64.deb)</br>[Pacchetto Debian a disponibilità elevata](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.1000.169-2_amd64.deb)</br>[Pacchetto Debian di ricerca full-text](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.1000.169-2_amd64.deb)</br>[Pacchetto Debian di SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.1000.169-2_amd64.deb)<br/>[Pacchetto SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="known-issues"></a>Problemi noti

Le sezioni seguenti descrivono i problemi noti della versione disponibile a livello generale di [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] in Linux.

#### <a name="general"></a>Generale

- La lunghezza del nome host in cui [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è installato non deve superare i 15 caratteri. 

    - **Soluzione**: sostituire il nome in /etc/hostname con uno di 15 caratteri o meno.

- Mettendo manualmente indietro l'ora di sistema, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] smetterà di aggiornare l'ora del sistema interno di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Soluzione**: Riavviare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Sono supportate solo le installazioni a istanza singola.

    - **Soluzione**: Se si vuole avere più di un'istanza in un determinato host, è consigliabile usare le macchine virtuali o i contenitori Docker. 

- Gestione configurazione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non riesce a connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux.

- La lingua predefinita dell'account di accesso **sa** è l'inglese.

    - **Soluzione**: Cambiare la lingua dell'account di accesso **sa** con l'istruzione **ALTER LOGIN**.

#### <a name="databases"></a>Database

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

- I database di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] in Windows che usano OLTP in memoria non possono essere ripristinati in [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] in Linux. Per ripristinare un database di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] che usa OLTP in memoria, aggiornare i database a [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] o [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] in Windows prima di spostarli in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux tramite backup/restore o detach/attach.

- L'autorizzazione utente **AMMINISTRAZIONE OPERAZIONI BULK** per il momento non è supportata in Linux.

#### <a name="networking"></a>Rete

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

- Usare NFS versione **4.2 o successiva**. Le versioni precedenti di NFS non supportano le funzionalità necessarie, ad esempio fallocate e la creazione di file sparse, comuni ai file system moderni.
- Individuare solo le directory **/var/opt/mssql** nel montaggio NFS. Gli altri file, ad esempio i file binari di sistema di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], non sono supportati.
- Assicurarsi che i client NFS usino l'opzione "nolock" per il montaggio della condivisione remota.

#### <a name="localization"></a>Localizzazione

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

#### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

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

#### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

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
- [Esecuzione in Docker](quickstart-install-connect-docker.md)
- [Eseguire il provisioning di una macchina virtuale SQL in Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Esecuzione e connessione - Cloud](quickstart-install-connect-clouds.md)

Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](sql-server-linux-faq.md).
