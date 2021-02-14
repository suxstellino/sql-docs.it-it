---
title: Eseguire la migrazione di database a SQL Server in Linux
description: Questo articolo descrive le diverse opzioni disponibili per la migrazione di database e dati a SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1619489d-377a-4f32-8930-d4f536539689
ms.openlocfilehash: d267bc2861d94e46cdb10fa825d204c8110a9273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345580"
---
# <a name="migrate-databases-and-structured-data-to-sql-server-on-linux"></a>Eseguire la migrazione di database e dati strutturati a SQL Server in Linux 

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

È possibile eseguire la migrazione di database e dati a SQL Server in esecuzione in Linux. Il metodo che si sceglie di usare dipende dai dati di origine e dallo scenario specifico. Le sezioni seguenti presentano le procedure consigliate per diversi scenari di migrazione.

## <a name="migrate-from-sql-server-on-windows"></a>Eseguire la migrazione da SQL Server in Windows
Se si vuole eseguire la migrazione di database di SQL Server in Windows a SQL Server in Linux, la tecnica consigliata consiste nell'uso di Backup e ripristino di SQL Server.

1. Creare un backup del database nel computer Windows.
2. Trasferire il file di backup nel computer SQL Server Linux di destinazione.
3. Ripristinare il backup nel computer Linux. 

Per un'esercitazione sulla migrazione di un database con Backup e ripristino, vedere l'argomento seguente:

- [Ripristinare un database di SQL Server da Windows a Linux](sql-server-linux-migrate-restore-database.md).

È anche possibile esportare il database in un file BACPAC, un file compresso che contiene lo schema e i dati del database. Se si ha un file BACPAC, è possibile trasferirlo nel computer Linux e quindi importarlo in SQL Server. Per altre informazioni, vedere gli argomenti seguenti:

- [Esportare e importare un database con SSMS o SqlPackage.exe](sql-server-linux-migrate-ssms.md)

## <a name="migrate-from-other-database-servers"></a>Eseguire la migrazione da altri server di database
È possibile eseguire la migrazione a SQL Server in Linux di database in altri sistemi di database, ad esempio Microsoft Access, DB2, MySQL, Oracle e Sybase. In questo scenario, usare SQL Server Management Assistant (SSMA) per automatizzare la migrazione a SQL Server in Linux. Per altre informazioni, vedere [Usare SSMA per la migrazione di database a SQL Server in Linux](sql-server-linux-migrate-ssma.md).  

## <a name="migrate-structured-data"></a>Eseguire la migrazione di dati strutturati
Sono disponibili anche tecniche per l'importazione di dati non elaborati. È possibile che siano disponibili file di dati strutturati esportati da altri database o da altre origini dati. In questo caso, è possibile usare lo strumento bcp per eseguire l'inserimento bulk dei dati. In alternativa, è possibile eseguire SQL Server Integration Services in Windows per importare i dati in un database di SQL Server in Linux. SQL Server Integration Services consente di eseguire trasformazioni più complesse sui dati durante l'importazione. 

Per altre informazioni su queste attività, vedere gli argomenti seguenti:

- [Eseguire la copia bulk di dati con bcp](sql-server-linux-migrate-bcp.md)
- [Estrarre, trasformare e caricare i dati per SQL Server in Linux con SSIS](sql-server-linux-migrate-ssis.md) 
