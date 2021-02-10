---
title: Hardware per OLTP in memoria di SQL | Microsoft Docs
description: Informazioni sulle considerazioni sull'hardware per le prestazioni di OLTP in memoria in SQL Server. OLTP in memoria usa la memoria e il disco in modo diverso rispetto alle tabelle basate su disco.
ms.custom: ''
ms.date: 03/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ba34a1b550d9726c3de479c56f92e40adf404214
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063707"
---
# <a name="hardware-considerations-for-in-memory-oltp-in-sql-server"></a>Considerazioni sull'hardware per OLTP in memoria in SQL Server

OLTP in memoria usa la memoria e il disco in modo diverso rispetto alle comuni tabelle basate su disco. Il miglioramento delle prestazioni registrato con OLTP in memoria dipende dall'hardware in uso. Questo post di blog include varie considerazioni generali sull'hardware e include informazioni e linee guida generiche per l'hardware da usare con OLTP in memoria.

> [!NOTE]
> Questo articolo è stato pubblicato in un blog il 1 agosto 2013 dal team di Microsoft SQL Server 2014. La pagina Web del blog verrà ritirata.
>
> [OLTP in memoria di SQL Server](./in-memory-oltp-in-memory-optimization.md)

<!--
    Here was the link to the blog. This blog was captured into this new article on 2018/11/30, by GeneMi (MightyPen).
    https://cloudblogs.microsoft.com/sqlserver/2013/08/01/hardware-considerations-for-in-memory-oltp-in-sql-server-2014/
    At least one pre-existing article that contained the obsolete blog link was:
        relational-databases\in-memory-oltp\sample-database-for-in-memory-oltp.md
-->

## <a name="cpu"></a>CPU

OLTP in memoria non richiede un server di fascia alta per il supporto di un carico di lavoro OLTP con velocità effettiva elevata. È consigliabile usare un server di livello intermedio con 2 socket per CPU. A causa dell'aumento della velocità effettiva causato da OLTP in memoria, 2 socket sono probabilmente sufficienti per le esigenze operative.

È consigliabile di abilitare la tecnologia hyper-threading con OLTP in memoria. Alcuni carichi di lavoro OLTP hanno fatto registrare un miglioramento delle prestazioni fino al 40% con l'hyper-threading.

## <a name="memory"></a>Memoria

Tutte le tabelle con ottimizzazione per la memoria risiedono completamente in memoria. Pertanto è necessario avere memoria fisica sufficiente per le tabelle stesse e per il carico di lavoro di interazione con il database. La quantità di memoria reale necessaria dipende dal carico di lavoro, ma probabilmente sarà necessaria una quantità corrispondente al doppio delle dimensioni dei dati. Sarà necessario avere anche memoria sufficiente per il pool di buffer, se il carico di lavoro opera anche su tabelle tradizionali basate su disco.

Per determinare la quantità di memoria usata da una determinata tabella con ottimizzazione per la memoria, eseguire la query seguente:

```sql
select object_name(object_id), * from sys.dm_db_xtp_table_memory_stats;
```

I risultati mostrano la memoria usata per le tabelle ottimizzate per la memoria e i relativi indici. I dati della tabella includono i dati dell'utente e anche tutte le versioni di riga precedenti ancora necessarie per l'esecuzione di transazioni o che non sono state ancora eliminate dal sistema. La memoria usata dagli indici hash è costante e non dipende dal numero di righe nella tabella.

Quando si usa OLTP in memoria è importante tenere presente che non è necessario che l'intero database sia ospitato in memoria. È possibile avere un database di vari terabyte e trarre comunque vantaggio da OLTP in memoria, a condizione che le dimensioni dei dati attivi (ovvero delle tabelle ottimizzate per la memoria) non superino i 256 GB. Il numero massimo di file di dati checkpoint gestibile da SQL Server per un singolo database è 4000 e ogni file può avere dimensioni massime pari a 128 MB. Il massimo teorico risultante sarebbe di 512 GB, ma per garantire che SQL Server sia in grado di unire i file di checkpoint e non raggiungere il limite di 4000 file, il massimo supportato è di 256 GB. Si noti che questo limite è valido solo per le tabelle ottimizzate per la memoria. Non esiste alcun limite di dimensioni per le tabelle basate su disco tradizionali nello stesso database SQL Server.

Le tabelle non persistenti (NDT) ottimizzate per la memoria, ad esempio le tabelle ottimizzate per la memoria con DURABILITY=SCHEMA_ONLY, non sono persistenti sul disco. Anche se le NDT non sono limitate dal numero di file di checkpoint, è supportato un massimo di 256 GB. Le considerazioni per le unità di registro e dati nella parte restante di questo post non si applicano alle tabelle non persistenti, perché i dati esistono solo in memoria.

## <a name="log-drive"></a>Unità di log

I record di registro relativi a tabelle ottimizzate per la memoria vengono scritti nel log delle transazioni del database, insieme agli altri record di log di SQL Server.

È sempre importante inserire il file di log in un'unità con bassa latenza, in modo che le transazioni non attendano troppo a lungo e per evitare contese per l'I/O del log. Il sistema esegue alla velocità del componente più lento (legge di Amdahl). È necessario garantire che durante l'esecuzione di OLTP in memoria il dispositivo di I/O log non diventi un collo di bottiglia. È consigliabile usare un dispositivo di archiviazione con bassa latenza, almeno un'unità SSD.

Le tabelle ottimizzate per la memoria usano una larghezza di banda del log inferiore rispetto alle tabelle basate su disco, in quanto non registrano le operazioni di indicizzazione né i record UNDO. Ciò consente di ridurre le contese I/O nel log.

## <a name="data-drive"></a>Unità dati

La persistenza delle tabelle ottimizzate per la memoria che usano file del checkpoint usa l'I/O in streaming. Di conseguenza, questi file non richiedono un disco con latenza bassa o I/O casuale rapido. Il fattore principale per queste unità è invece la velocità di I/O sequenziale e la larghezza di banda della scheda bus host (HBA). Di conseguenza non sono necessarie unità SSD per i file checkpoint, che possono essere posizionati in spindle ad alte prestazioni (ad esempio SAS) a condizione che la loro velocità di I/O sequenziale soddisfi i requisiti.

Il fattore principale che determina il requisito di velocità è l'obiettivo RTO (obiettivo tempo di ripristino) al riavvio del server. Durante il recupero del database tutti i dati nelle tabelle ottimizzate per la memoria devono essere letti dal disco alla memoria. Il ripristino del database si verifica alla velocità di lettura sequenziale del sistema di I/O. Il collo di bottiglia è rappresentato dal disco.

Per soddisfare requisiti RTO rigorosi è consigliabile distribuire i file del checkpoint su più dischi, aggiungendo più contenitori al filegroup MEMORY_OPTIMIZED_DATA. SQL Server supporta il caricamento parallelo di file del checkpoint da più unità. Il ripristino si verifica alla velocità complessiva delle unità.

In termini di capacità del disco è consigliabile avere una capacità pari a 2-3 volte le dimensioni delle tabelle ottimizzate per la memoria.

## <a name="see-also"></a>Vedere anche

[Database di esempio per OLTP in memoria](sample-database-for-in-memory-oltp.md)