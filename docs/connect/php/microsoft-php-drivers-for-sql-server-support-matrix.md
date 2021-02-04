---
title: Matrice di supporto dei driver Microsoft per PHP
description: Questa pagina contiene i criteri relativi al ciclo di vita e alla matrice del supporto di Microsoft PHP Driver per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca05a57c88f93a890cf546b3b1c60382f672d3be
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076433"
---
# <a name="microsoft-php-drivers-for-sql-server-support-matrix"></a>Driver PHP Microsoft per la matrice di supporto SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Questa pagina contiene i criteri relativi al ciclo di vita e alla matrice del supporto di Microsoft PHP Driver per SQL Server.

## <a name="microsoft-php-drivers-support-lifecycle-matrix-and-policy"></a>Criteri e matrice del ciclo di vita del supporto dei driver PHP Microsoft

I criteri relativi al ciclo di vita del supporto Microsoft (MSL) forniscono informazioni trasparenti e prevedibili riguardanti il ciclo di vita del supporto dei prodotti Microsoft. Le versioni dei driver PHP 3.x, 4.x e 5.x offrono fino a cinque anni di supporto Mainstream dalla data di rilascio del driver. Il supporto "Mainstream" viene definito nel [sito Web del ciclo di vita del supporto Microsoft](https://support.microsoft.com/lifecycle).

Le opzioni di supporto esteso e personalizzato non sono disponibili per Microsoft PHP Driver.

I seguenti Microsoft PHP Driver sono supportati fino alla data di fine del supporto indicata.

|Nome del driver|Versione del pacchetto driver|Fine del supporto Mainstream|
|-|:-:|-|
|Driver Microsoft PHP 5,9 per SQL Server|5.9|29 gennaio 2026|
|Driver PHP Microsoft 5.8 per SQL Server|5.8|31 gennaio 2025|
|Driver Microsoft PHP 5.6 per SQL Server|5.6|21 febbraio 2024|
|Driver Microsoft PHP 5.3 per SQL Server|5.3|20 luglio 2023|
|Driver Microsoft PHP 5.2 per SQL Server|5,2|9 febbraio 2023|
|Driver Microsoft PHP 4.3 per SQL Server|4.3|6 luglio 2022|
|Driver Microsoft PHP 4.0 per SQL Server|4.0|11 luglio 2021|
| &nbsp; | &nbsp; | &nbsp; |

I Microsoft PHP Driver non sono più supportati.

|Nome del driver|Versione del pacchetto driver|Fine del supporto Mainstream|
|-|:-:|-|
|Driver Microsoft PHP 3.2 per SQL Server|3.2|9 marzo 2020|
|Driver Microsoft PHP 3.1 per SQL Server|3.1|12 dicembre 2019|
|Driver Microsoft PHP 3.0 per SQL Server|3.0|6 marzo 2017|
|Driver Microsoft PHP 2.0 per SQL Server|2.0|10 agosto 2015|
|Driver Microsoft PHP 1.0 per SQL Server|1.0|28 aprile 2014|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="sql-server-version-certified-compatibility"></a>Compatibilità certificata delle versioni di SQL Server
 La matrice seguente elenca le versioni del database testate e certificate come compatibili con la versione del driver corrispondente. Microsoft si impegna a mantenere la compatibilità con le versioni precedenti del driver, ma solo il driver supportato più recente viene testato e certificato con le nuove versioni di SQL Server quando viene rilasciato SQL Server.

|Versione driver&nbsp;&#8594;<br />&#8595; Versione database|5.9|5.8|5.6|5.3|5,2|4.3|4.0|3.2|
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|database SQL di Azure        |Sì|Sì|sì|Sì|Sì|Sì|   |   |
|Istanza gestita di SQL di Azure|sì|Sì|Sì|Sì|Sì|Sì|   |   |
|Azure Synapse Analytics   |Sì|Sì|Sì|Sì|Sì|Sì|   |   |
|SQL Server 2019           |Sì|Sì|   |   |   |   |   |   |
|SQL Server 2017           |Sì|Sì|Sì|Sì|Sì|Sì|   |   |
|SQL Server 2016           |Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |
|SQL Server 2014           |Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|SQL Server 2012           |Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|SQL Server 2008 R2        |   |   |Sì|Sì|Sì|Sì|Sì|Sì|
|SQL Server 2008           |   |   |   |   |   |   |Sì|Sì|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

Per informazioni sull'uso di PHP con il database SQL di Azure, vedere [Connessione al database SQL di Microsoft Azure](connecting-to-microsoft-azure-sql-database.md).

## <a name="php-version-support"></a>Supporto versione PHP

Le versioni seguenti di PHP sono supportate con la versione indicata dei driver PHP Microsoft:

|Versione driver&nbsp;&#8594;<br />&#8595; Versione PHP|5.9|5.8|5.6|5.3|5,2|4.3|4.0|3.2|
|:---:|---|---|---|---|---|---|---|---|
|8.0|8.0.0 + |                |                |                |                |       |        |        |
|7.4|7.4.0+ |7.4.0+          |                |                |                |       |        |        |
|7.3|7.3.0+ |7.3.0+          |7.3.0+          |                |                |       |        |        |
|7.2|       |7.2+<sup>1</sup>|7.2+<sup>1</sup>|7.2+<sup>1</sup>|7.2+<sup>1</sup>|       |        |        |
|7.1|       |                |7.1.0+          |7.1.0+          |7.1.0+          |7.1.0+ |        |        |
|7.0|       |                |                |7.0.0+          |7.0.0+          |7.0.0+ |7.0.0+  |        |
|5.6|       |                |                |                |                |       |        |5.6.4+  |
|5.5|       |                |                |                |                |       |        |5.5.16+ |
|5.4|       |                |                |                |                |       |        |5.4.32  |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> le versioni 7.2.1 e versioni successive sono supportate in Windows, mentre le versioni 7.2.0 e successive sono supportate in Linux e MacOS.

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Le versioni seguenti del sistema operativo Windows sono supportate con la versione indicata dei driver PHP Microsoft:

|Versione driver&nbsp;&#8594;<br />&#8595; Sistema operativo|5.9|5.8|5.6|5.3|5,2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Windows Server 2019                 |Sì|Sì|Sì|   |   |   |   |   |
|Windows Server 2016                 |Sì|Sì|Sì|Sì|Sì|Sì|   |   |
|Windows Server 2012 R2              |Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|Windows Server 2012                 |Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|Windows Server 2008 R2 SP1          |   |   |   |   |   |   |Sì|Sì|
|Windows Server 2008 R2              |   |   |   |   |   |   |   |   |
|Windows Server 2008 SP2             |   |   |   |   |   |   |Sì|Sì|
|Windows 10                          |Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |
|Windows 8.1                         |Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|Windows 8                           |   |   |   |   |   |Sì|Sì|Sì|
|Windows 7 SP1                       |   |   |   |   |   |   |Sì|Sì|
|Windows Vista SP2                   |   |   |   |   |   |   |Sì|Sì|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

Le versioni seguenti dei sistemi operativi Linux e macOS (solo 64 bit) sono supportate con la versione indicata dei driver PHP Microsoft:

|Versione driver&nbsp;&#8594;<br />&#8595; Sistema operativo|5.9|5.8|5.6|5.3|5,2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Ubuntu 20,10 (64 bit)               |Sì|   |   |   |   |   |   |   |
|Ubuntu 20.04 (a 64 bit)               |Sì|Sì|   |   |   |   |   |   |
|Ubuntu 19.10 (a 64 bit)               |   |Sì|   |   |   |   |   |   |
|Ubuntu 18.10 (a 64 bit)               |   |   |Sì|   |   |   |   |   |
|Ubuntu 18.04 (a 64 bit)               |Sì|Sì|Sì|Sì|   |   |   |   |
|Ubuntu 17.10 (a 64 bit)               |   |   |   |Sì|Sì|   |   |   |
|Ubuntu 16.04 (a 64 bit)               |Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |
|Ubuntu 15.10 (a 64 bit)               |   |   |   |   |   |Sì|   |   |
|Ubuntu 15.04 (a 64 bit)               |   |   |   |   |   |   |Sì|   |
|Debian 10 (a 64 bit)                  |Sì|Sì|   |   |   |   |   |   |
|Debian 9 (a 64 bit)                   |Sì|Sì|Sì|Sì|Sì|   |   |   |
|Debian 8 (a 64 bit)                   |   |Sì|Sì|Sì|Sì|Sì|   |   |
|Red Hat Enterprise Linux 8 (64 bit) |Sì|Sì|   |   |   |   |   |   |
|Red Hat Enterprise Linux 7 (64 bit) |Sì|Sì|Sì|Sì|Sì|Sì|Sì|   |
|Suse Enterprise Linux 15 (64 bit)   |Sì|Sì|Sì|   |   |   |   |   |
|Suse Enterprise Linux 12 (64 bit)   |Sì|Sì|Sì|Sì|Sì|   |   |   |
|Alpine Linux 3,12 (64 bit)          |Sì|   |   |   |   |   |   |   |
|Alpine Linux 3,11 (64 bit)          |Sì|Sì<sup>1</sup>|   |   |   |   |   |   |
|macOS Big Sur (64 bit)              |Sì|   |   |   |   |   |   |   |
|macOS Catalina (64 bit)             |Sì|Sì|   |   |   |   |   |   |
|macOS Mojave (64 bit)               |Sì|Sì|Sì|   |   |   |   |   |
|macOS High Sierra (64 bit)          |   |Sì|Sì|Sì|   |   |   |   |
|macOS Sierra (a 64 bit)               |   |   |Sì|Sì|Sì|Sì|   |   |
|macOS El Capitan (a 64 bit)           |   |   |   |Sì|Sì|Sì|   |   |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Il supporto di Alpine Linux è sperimentale per la versione 5.8.0. La versione 5.8.1 introduce il supporto per la produzione.

## <a name="see-also"></a>Vedere anche

[Note sulla versione](release-notes-php-sql-driver.md)

[Risorse di supporto](support-resources-for-the-php-sql-driver.md)

[System Requirements](system-requirements-for-the-php-sql-driver.md)
