---
title: Requisiti di sistema (ODBC Driver for SQL Server)
description: In questo articolo sono indicati i requisiti di sistema per ODBC Driver for SQL Server nei sistemi operativi Linux e macOS.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- prerequisites
- system requirements
- requirements
ms.assetid: f03b7fdd-0e9d-4e74-958d-e8c87e027348
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f78fdfcb8fe19856483403c4675f973c4f5c97b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198379"
---
# <a name="system-requirements-linux-and-macos"></a>Requisiti di sistema (Linux e macOS)

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Questo argomento elenca i requisiti per l'uso di [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux e macOS.

## <a name="sql-version-compatibility"></a>Compatibilità tra versioni SQL

La compatibilità tra versioni SQL per i driver Linux e macOS e uguale alla [compatibilità tra versioni SQL dei driver Windows](../windows/system-requirements-installation-and-driver-files.md#sql-version-compatibility).

## <a name="operating-system-support"></a>Supporto dei sistemi operativi

Le versioni 17, 13.1 e 13 dei driver per Linux e macOS sono supportate solo sull'architettura x64 dei sistemi operativi seguenti:

|Versione driver&nbsp;&#8594;<br />&#8595; Sistema operativo     |17.7|17.6|17.5|17.4|17.3|17.2|17.1|17.0|13.1|13|
|-------------------------------|----|----|----|----|----|----|----|----|----|---|
|Apple OS X 10.11 (El Capitan)  |    |    |    |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Apple macOS 10.12 (Sierra)     |    |    |    |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Apple macOS 10.13 (High Sierra)|    |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Apple macOS 10.14 (Mojave)     |Sì |Sì |Sì |Sì |Sì |    |    |    |    |   |
|Apple macOS 10.15 (Catalina)   |Sì |Sì |Sì |    |    |    |    |    |    |   |
|Apple macOS 11,0 (Big Sur)     |Sì |    |    |    |    |    |    |    |    |   |
|Alpine Linux 3.11              |Sì |Sì |Sì |    |    |    |    |    |    |   |
|Alpine Linux 3,12              |Sì |    |    |    |    |    |    |    |    |   |
|Debian Linux 8                 |    |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Debian Linux 9                 |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Debian Linux 10                |Sì |Sì |Sì |Sì |    |    |    |    |    |   |
|Oracle Linux 7                 |Sì |    |    |    |    |    |    |    |    |   |
|Oracle Linux 8                 |Sì |Sì |Sì |    |    |    |    |    |    |   |
|RedHat Enterprise Linux 6      |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|RedHat Enterprise Linux 7      |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|RedHat Enterprise Linux 8      |Sì |Sì |Sì |Sì |    |    |    |    |    |   |
|SUSE Linux Enterprise Server 11<sup>1</sup>|Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|SUSE Linux Enterprise Server 12|Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|SUSE Linux Enterprise Server 15|Sì |Sì |Sì |Sì |Sì |    |    |    |    |   |
|Ubuntu Linux 14.04             |    |    |    |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Ubuntu Linux 16.04             |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì |Sì|
|Ubuntu Linux 18.04             |Sì |Sì |Sì |Sì |Sì |Sì |    |    |    |   |
|Ubuntu Linux 20.04             |Sì |Sì |    |    |    |    |    |    |    |   |
|Ubuntu Linux 20,10             |Sì |    |    |    |    |    |    |    |    |   |

<sup>1</sup> ODBC Driver 17 supporta solo SUSE Linux Enterprise Server 11 SP4

I pacchetti di installazione di [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13, 13.1 e 17 per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux e macOS risolvono automaticamente le dipendenze del driver se installati tramite il sistema di gestione pacchetti della distribuzione, come descritto in [Installare il driver ODBC (Linux)](installing-the-microsoft-odbc-driver-for-sql-server.md) e [Installare il driver ODBC (macOS)](install-microsoft-odbc-driver-sql-server-macos.md).

## <a name="microsoft-odbc-driver-11-for-sql-server"></a>Microsoft ODBC Driver 11 per SQL Server  
  
* Gestione driver UnixODBC 2.3.0 a 64 bit per SQLLEN/SQLULEN a 64 bit. Le versioni successive di Gestione driver UnixODBC a 64 bit non sono supportate con il driver ODBC in Linux. Per ulteriori informazioni, vedere [Installing the Driver Manager](../../../connect/odbc/linux-mac/installing-the-driver-manager.md) .  
  
* Il driver ODBC per **Red Hat Enterprise Linux 5 (64 bit)** richiede i pacchetti seguenti e può essere scaricato da qui: [Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
  * `glibc`  
  * `libgcc`  
  * `libstdc++`  
  * `e2fsprogs-libs`  
  * `krb5-libs`  
  * `openssl`  
  
* Il driver ODBC per **Red Hat Enterprise Linux 6 (64 bit)** richiede i pacchetti seguenti e può essere scaricato da qui: [Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
  * `glibc`  
  * `libgcc`  
  * `libstdc++`  
  * `libuuid`  
  * `krb5-libs`  
  * `openssl`  
  
* Il driver ODBC per **SUSE Linux Enterprise 11 Service Pack 2 (64 bit)** richiede i pacchetti seguenti e può essere scaricato da qui: [Microsoft ODBC Driver 11 Preview for SQL Server - SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)  
  * `glibc`  
  * `libstdc++46`  
  * `libgcc46`  
  * `libuuid1`  
  * `krb5`  
  * `libopenssl0_9_8`  
  
## <a name="see-also"></a>Vedere anche

[Installazione di Gestione driver](../../../connect/odbc/linux-mac/installing-the-driver-manager.md)  
[Problemi noti in questa versione del driver](../../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)  
[Note sulla versione](../../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)  
