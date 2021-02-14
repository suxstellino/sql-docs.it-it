---
description: Tecnologie e sistemi di database in memoria
title: Funzionalità e tecnologie dei sistemi di database in memoria
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- in-memory systems
- in-memory technologies
- in-memory features
- database, in-memory database
- system, in-memory system
- features, in-memory features
- in-memory
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: briancarrig
ms.author: brcarrig
ms.openlocfilehash: 96f175f4eb6f913c7870f573e65201e1124dffad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351279"
---
# <a name="in-memory-database-systems-and-technologies"></a>Tecnologie e sistemi di database in memoria

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Questa pagina è stata progettata come documento di riferimento per le tecnologie e le funzionalità in memoria disponibili in SQL Server. Il concetto di sistema di database in memoria si riferisce a un sistema di database progettato per sfruttare le maggiori capacità di memoria disponibili nei sistemi di database moderni. Un database in memoria può essere di natura relazionale o non relazionale.

Si presuppone spesso che i vantaggi delle prestazioni di un sistema di database in memoria dipendono dalla maggiore velocità di accesso ai dati residenti in memoria rispetto ai dati presenti anche nei più veloci sottosistemi disco disponibili (in base a diversi ordini di grandezza). Molti carichi di lavoro di SQL Server, tuttavia, possono adattarsi all'intero working set nella memoria disponibile. Molti sistemi di database in memoria possono salvare in modo permanente i dati su disco e potrebbero non essere sempre in grado di adattare l'intero set di dati alla memoria disponibile.

Una cache volatile veloce di fronte a un supporto notevolmente più lento ma durevole ha un'importanza predominante per i carichi di lavoro dei database relazionali. Questa cache richiede particolari approcci alla gestione del carico di lavoro. Le opportunità offerte da velocità di trasferimento della memoria superiori, maggiore capacità o persino memoria persistente facilitano lo sviluppo di nuove funzionalità e tecnologie che possono stimolare l'adozione di nuovi approcci alla gestione del carico di lavoro del database relazionale.

## <a name="hybrid-buffer-pool"></a>Pool di buffer ibrido

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Il [pool di buffer ibrido](../database-engine/configure-windows/hybrid-buffer-pool.md) espande il pool di buffer per i file di database che risiedono in dispositivi di archiviazione con memoria persistente indirizzabili a byte per entrambe le piattaforme Windows e Linux con [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

## <a name="memory-optimized-tempdb-metadata"></a>Metadati `tempdb` ottimizzati per la memoria

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce una nuova funzionalità, ovvero i [metadati tempdb ottimizzati per la memoria](./databases/tempdb-database.md#memory-optimized-tempdb-metadata) che rimuove in modo efficace i colli di bottiglia dovuti alla contesa e sblocca un nuovo livello di scalabilità per i carichi di lavoro tempdb elevati.

## <a name="in-memory-oltp"></a>OLTP in memoria

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[OLTP in memoria](./in-memory-oltp/in-memory-oltp-in-memory-optimization.md) è una tecnologia di database disponibile in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../includes/sssds-md.md)] per ottimizzare le prestazioni di elaborazione delle transazioni, l'inserimento di dati, il caricamento di dati e gli scenari di dati temporanei.

## <a name="configuring-persistent-memory-support-for-linux"></a>Configurazione del supporto della memoria persistente per Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] descrive come configurare la memoria persistente (PMEM) usando la [memoria persistente](../linux/sql-server-linux-configure-pmem.md) dell'utilità `ndctl`.

## <a name="persisted-log-buffer"></a>Buffer di log persistente

Il Service Pack 1 di [!INCLUDE[ssSQL16](../includes/sssql16-md.md)] ha introdotto un'ottimizzazione delle prestazioni per i carichi di lavoro a elevato utilizzo di scrittura vincolati dalle attese di WRITELOG. Per archiviare il buffer di log viene usata la memoria persistente. Questo buffer, di dimensioni ridotte (20 MB per database utente), deve essere scaricato su disco per garantire la finalizzazione delle transazioni scritte nel log. Per i carichi di lavoro OLTP a elevato utilizzo di scrittura, questo meccanismo di scaricamento può diventare un collo di bottiglia. Con il buffer di log nella memoria persistente, il numero di operazioni necessarie per finalizzare il log viene ridotto, migliorando i tempi complessivi delle transazioni e aumentando le prestazioni del carico di lavoro. Questo processo è stato inizialmente definito [memorizzazione nella cache della coda del log]( https://blogs.msdn.microsoft.com/bobsql/2016/11/08/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm/). Questo nome, tuttavia, appariva in conflitto con il [backup della coda del log](./backup-restore/tail-log-backups-sql-server.md) e la nozione classica in base alla quale la coda del log è la parte finalizzata, ma non ancora sottoposta a backup, del log delle transazioni. È stato quindi usato il nome ufficiale di questa funzionalità, ovvero "buffer di log persistente".

Vedere [Aggiungere un buffer di log persistente a un database](./databases/add-persisted-log-buffer.md).
