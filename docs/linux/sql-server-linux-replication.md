---
title: Replica di SQL Server in Linux
description: Informazioni sul modo in cui SQL Server 2017 (14.x) (CU18) e versioni successive supportano la replica di SQL Server per le istanze di SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: bb322f11b866a7593d22e0d722fad84765cb5139
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346455"
---
# <a name="sql-server-replication-on-linux"></a>Replica di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) e versioni successive supportano la replica di SQL Server per le istanze di SQL Server in Linux.

Configurare la replica in Linux con le [stored procedure di replica](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md) di SQL Server Management Studio (SSMS).

Un'istanza di SQL Server può far parte di qualsiasi ruolo di replica:

* Editore
* Database di distribuzione
* Sottoscrittore

Uno schema di replica può combinare diverse piattaforme di sistema operativo. Uno schema di replica può ad esempio includere un'istanza di SQL Server in Linux per il server di pubblicazione e il database di distribuzione e i sottoscrittori possono includere istanze di SQL Server in Windows e in Linux.

Le istanze di SQL Server in Linux possono far parte di qualsiasi tipo di replica.

* Transazionale
* Snapshot

Per informazioni dettagliate sulla replica, vedere la [documentazione della replica di SQL Server](../relational-databases/replication/sql-server-replication.md).

## <a name="supported-features"></a>Caratteristiche supportate

Sono supportate le funzionalità di replica seguenti:

* Replica snapshot
* Replica transazionale
* Replica con porte non predefinite <!--Add link to explanation-->
* Replica con autenticazione AD
* Configurazioni di replica in Windows e Linux
* Aggiornamenti immediati per la replica transazionale

## <a name="limitations"></a>Limitazioni

Le funzionalità seguenti non sono supportate:

* Replica di tipo merge
* Replica peer-to-peer
* Pubblicazione Oracle

## <a name="next-steps"></a>Passaggi successivi

[Configurare la replica di SQL Server in Linux](sql-server-linux-replication-tutorial-tsql.md)

[Esempio: configurare la replica di SQL Server in Linux](sql-server-linux-replication-configure.md)
