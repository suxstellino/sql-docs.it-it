---
description: MSSQL_REPL027183
title: MSSQL_REPL027183 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL027183 error
ms.assetid: 52c271ac-1a0e-43d5-85d4-35886d1efd32
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 32112470b6740eaac429895a394a3ae5cbdec110
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206148"
---
# <a name="mssql_repl027183"></a>MSSQL_REPL027183
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|27183|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Impossibile enumerare le modifiche negli articoli con filtri di riga con parametri. Se il problema persiste, aumentare il timeout per le query per questo processo, ridurre il periodo di memorizzazione per la pubblicazione e migliorare gli indici delle tabelle pubblicate.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore viene generato se si verifica un timeout dell'agente di merge durante l'elaborazione delle modifiche in una pubblicazione filtrata. Il timeout potrebbe essere provocato da uno dei problemi seguenti:  
  
-   Mancato uso dell'ottimizzazione delle partizioni pre-calcolate.  
  
-   Frammentazione dell'indice nelle colonne utilizzate per il filtraggio.  
  
-   Tabelle di metadati di tipo merge di grandi dimensioni, come **MSmerge_tombstone**, **MSmerge_contents** e **MSmerge_genhistory**.  
  
-   Tabelle filtrate non unite in join in una chiave univoca e filtri join che coinvolgono un gran numero di tabelle.  
  
## <a name="user-action"></a>Azione dell'utente  
 Per risolvere il problema:  
  
-   Aumentare il valore del parametro **-QueryTimeOut** per l'agente di merge, in modo da continuare l'elaborazione mentre si risolve il problema sottostante che causa l'errore. I parametri degli agenti possono essere specificati nei profili agente e dalla riga di comando. Per altre informazioni, vedere:  
  
    -   [Usare i profili agenti di replica](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Visualizzare e modificare i parametri del prompt dei comandi dell'agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
-   Se possibile, utilizzare l'ottimizzazione delle partizioni pre-calcolate. Questa ottimizzazione viene utilizzata per impostazione predefinita se vengono soddisfatti alcuni requisiti di pubblicazione. Per altre informazioni su questi requisiti, vedere [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Se la pubblicazione non soddisfa questi requisiti, è necessario prendere in considerazione l'eventualità di riprogettare la pubblicazione.  
  
-   Specificare la minima impostazione possibile per il periodo di memorizzazione della pubblicazione, poiché la replica non è in grado di eliminare i metadati dei database di pubblicazione e sottoscrizione prima della scadenza del periodo di memorizzazione. Per altre informazioni, vedere [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
-   Come parte della manutenzione per la replica di tipo merge, controllare occasionalmente l'aumento delle dimensioni delle tabelle di sistema associate alla replica di tipo merge: **MSmerge_contents**, **MSmerge_genhistory** e **MSmerge_tombstone**, **MSmerge_current_partition_mappings** e **MSmerge_past_partition_mappings**. Reindicizzare periodicamente queste tabelle. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
-   Verificare che le colonne utilizzate per il filtraggio siano indicizzate correttamente e, se necessario, ricompilare tali indici. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
-   Impostare la proprietà **join_unique_key** per i filtri di join basati su colonne univoche. Per altre informazioni, vedere [Join Filters](../../relational-databases/replication/merge/join-filters.md).  
  
-   Limitare il numero massimo di tabelle nella gerarchia dei filtri di join. Se si generano filtri di join di cinque o più tabelle, considerare altre soluzioni: non filtrare tabelle piccole, non soggette a modifica o tabelle che fungono principalmente da tabelle di ricerca. Utilizzare i filtri di join solo tra tabelle che devono essere partizionate tra diverse sottoscrizioni.  
  
-   Apportare un numero minore di modifiche sulle tabelle filtrate tra le sincronizzazioni, oppure eseguire l'agente di merge con maggiore frequenza. Per ulteriori informazioni sull'impostazione delle pianificazioni della sincronizzazione, vedere [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
