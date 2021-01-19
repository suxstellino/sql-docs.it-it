---
title: Contatori delle prestazioni XTP (OLTP in memoria)
description: SQL Server fornisce oggetti e contatori utilizzabili da Monitor prestazioni per il monitoraggio dell'attività OLTP in memoria.
ms.custom: seo-dt-2019
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: fe3cbaf4-65f4-44c5-acc6-7b735cda0c5d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 38d44cc0c916c7e40e26b89196b21012f7b72499
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170713"
---
# <a name="sql-server-xtp-in-memory-oltp-performance-counters"></a>Contatori delle prestazioni XTP di SQL Server (OLTP in memoria)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce oggetti e contatori utilizzabili da Performance Monitor per il monitoraggio dell'attività OLTP in memoria. Gli oggetti e i contatori sono condivisi tra tutte le istanze di una determinata versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del computer, a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 In passato i nomi degli oggetti e dei contatori iniziavano con *XTP*, come in **XTP Cursors**. Ora, a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], i nomi adottano il modello seguente:  
  
-   **XTP Cursors** *\<version>* **di SQL Server**  
  
 Il valore per *\<version>* è qualcosa come 2016.  
  
##  <a name="sql-server-xtp-performance-objects"></a><a name="SQLServerPOs"></a> Oggetti prestazione XTP di SQL Server  
 Nella tabella seguente sono descritti gli oggetti prestazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Oggetto prestazione|Descrizione|  
|------------------------|-----------------|  
|[XTP Cursors di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-cursors.md)|L'oggetto prestazione SQL Server XTP Cursors contiene contatori correlati ai cursori interni del motore OLTP in memoria. I cursori sono i blocchi predefiniti di basso livello usati dal motore OLTP in memoria per elaborare query [!INCLUDE[tsql](../../includes/tsql-md.md)] . Di conseguenza, in genere non si ha controllo diretto su di essi.|  
|[XTP Databases di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-databases.md)|L'oggetto prestazione SQL Server XTP Databases fornisce contatori specifici del database OLTP in memoria.|  
|[XTP Garbage Collection di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-garbage-collection.md)|L'oggetto prestazione SQL Server XTP Garbage Collection contiene contatori correlati al Garbage Collector del motore OLTP in memoria.|  
|[XTP IO Governor di SQL Server 2016](../../relational-databases/performance-monitor/sql-server-xtp-io-governor.md)|L'oggetto prestazione XTP IO Governor di SQL Server include i contatori correlati a IO Rate Governor di OLTP in memoria.|
|[XTP Phantom Processor di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-phantom-processor.md)|L'oggetto prestazione SQL Server XTP Phantom Processor contiene contatori correlati al sottosistema di elaborazione fantasma del motore OLTP in memoria. Con questo componente è possibile rilevare le righe fantasma nelle transazioni in esecuzione a livello di isolamento SERIALIZABLE.|  
|[XTP Storage di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-storage.md)|L'oggetto prestazione SQL Server XTP Storage include i contatori correlati all'archiviazione OLTP in memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[XTP Transaction Log di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transaction-log.md)|L'oggetto prestazione SQL Server XTP Transaction Log include i contatori correlati alla registrazione delle transazioni OLTP in memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[XTP Transactions di SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transactions.md)|L'oggetto prestazione SQL Server XTP Transactions contiene i contatori correlati alle transazioni del motore OLTP in memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
  
