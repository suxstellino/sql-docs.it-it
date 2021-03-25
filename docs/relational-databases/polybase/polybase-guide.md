---
title: Introduzione alla virtualizzazione dei dati con polibase
description: La polibase consente all'istanza di SQL Server di elaborare query Transact-SQL che leggono dati da origini dati esterne, ad esempio Hadoop e l'archiviazione BLOB di Azure.
ms.date: 03/23/2021
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
ms.custom: contperf-fy21q2
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: debeb0916d8ecb14a5b0f52726bed90f04429fca
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103298"
---
# <a name="introducing-data-virtualization-with-polybase"></a>Introduzione alla virtualizzazione dei dati con polibase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

Polibase è una funzionalità di virtualizzazione dei dati per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 

## <a name="what-is-polybase"></a>Che cos'è PolyBase?

La polibase consente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'istanza di eseguire query sui dati con T-SQL direttamente dai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cluster, Oracle, Teradata, MongoDB, Hadoop, Cosmos DB senza installare separatamente il software di connessione client. È anche possibile usare il connettore ODBC generico per connettersi a provider aggiuntivi tramite driver ODBC di terze parti. La polibase consente alle query T-SQL di unire i dati da origini esterne a tabelle relazionali in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

Un caso d'uso chiave per la virtualizzazione dei dati con la funzionalità di base è quello di consentire ai dati di rimanere nella posizione e nel formato originali. È possibile virtualizzare i dati esterni tramite l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza di, in modo che sia possibile eseguire query sul posto come qualsiasi altra tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questo processo riduce al minimo la necessità di processi ETL per lo spostamento dei dati. Questo scenario di virtualizzazione dei dati è possibile con l'uso di connettori di base.

> [!NOTE]
> Alcune funzionalità della funzionalità di base sono in anteprima privata per le **istanze gestite di SQL di Azure**, inclusa la possibilità di eseguire query su dati esterni (file parquet) in Azure Data Lake Storage (ADLS) Gen2. L'anteprima privata include l'accesso alle librerie client e alla documentazione a scopo di test non ancora disponibili pubblicamente. Se si è interessati e si è pronti a investire del tempo per provare le funzionalità e condividere commenti e suggerimenti, consultare la Guida all' [Anteprima privata di Azure SQL istanza gestita](https://sqlmipg.blob.core.windows.net/azsqlpolybaseshare/Azure_SQL_Managed_Instance_Polybase_Private_Preview_Onboarding_Guide.pdf).

### <a name="supported-sql-products-and-services"></a>Prodotti e servizi SQL supportati

PolyBase offre queste stesse funzionalità per i prodotti SQL seguenti di Microsoft:

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive (solo Windows)
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] e versioni successive (Linux)
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[pdw](../../includes/sspdw-md.md)](PDW), ospitato nel sistema di piattaforma di analisi (APS) 
- [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]

### <a name="polybase-connectors"></a>Connettori di polibase

 La funzionalità di polibase fornisce connettività alle origini dati esterne seguenti:

| Origini dati esterne     | [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con polibase | PDW APS    | [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] |
|---------------------------|--------------------------|------------|---------------|
| Oracle, MongoDB, Teradata | Lettura                     | **No**     | **No**        |  
| ODBC generico              | Lettura (solo Windows)      | **No**     | **No**        |  
| Archiviazione di Azure             | Lettura/Scrittura               | Lettura/Scrittura | Lettura/Scrittura    |
| Hadoop                    | Lettura/Scrittura               | Lettura/Scrittura | **No**        |  
| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | Lettura                     | **No**     | **No**        |  
|                           |                          |            |               |


* [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] introduzione di polibase con supporto per le connessioni a Hadoop e all'archiviazione BLOB di Azure.
* [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] sono stati introdotti connettori aggiuntivi, tra cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Oracle, Teradata e MongoDB.

 Esempi di connettori esterni sono i seguenti:

- [[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Hadoop](polybase-configure-hadoop.md)*

\* Polibase supporta due provider Hadoop, Hortonworks Data Platform (HDP) e Cloudera Distributed Hadoop (CDH).

 Per utilizzare la polibase in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

1. [Installare la polibase in Windows](polybase-installation.md) o [installare la polibase in Linux](polybase-linux-setup.md).
1. A partire da [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] , [abilitare la polibase in sp_configure](polybase-installation.md#enable), se necessario. 
1. Creare un' [origine dati esterna](../../t-sql/statements/create-external-data-source-transact-sql.md).
1. Creare una [tabella esterna](../../t-sql/statements/create-external-table-transact-sql.md).



### <a name="azure-integration"></a>Integrazione con Azure

Con la guida sottostante di polibase, le query T-SQL possono anche importare ed esportare dati dall'archiviazione BLOB di Azure. Polibase consente inoltre [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] di importare ed esportare dati da Azure Data Lake Store e dall'archivio BLOB di Azure.

## <a name="why-use-polybase"></a>Perché usare PolyBase

La polibase consente di unire dati da un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza di a dati esterni. Prima di PolyBase, per aggiungere i dati alle origini dati esterne era possibile eseguire una delle operazioni seguenti:

- Trasferire la metà dei dati in modo che tutti i dati fossero in una posizione.
- Eseguire query su entrambe le origini dati, quindi scrivere logica di query personalizzata per creare i join e integrare i dati a livello di client.

PolyBase consente di usare semplicemente Transact-SQL per unire i dati.

PolyBase non richiede di installare software aggiuntivo nell'ambiente Hadoop. Si possono eseguire query sui dati esterni usando la stessa sintassi T-SQL usata per eseguire query su una tabella di database. Le azioni di supporto implementate da PolyBase vengono tutte eseguite in modo trasparente. L'autore della query non deve conoscere l'origine esterna.

### <a name="polybase-uses"></a>Usi di PolyBase

La polibase Abilita gli scenari seguenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

- **Eseguire query sui dati archiviati in Hadoop da un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza di o PDW.** Gli utenti scelgono di archiviare i dati in sistemi distribuiti e scalabili convenienti, come Hadoop. PolyBase semplifica la query dei dati con T-SQL.

- **Eseguire query sui dati archiviati nell'archivio BLOB di Azure.** Nell'archivio BLOB di Azure è possibile salvare i dati da usare con i servizi di Azure. PolyBase semplifica l'accesso ai dati con T-SQL.

- **Importa i dati da Hadoop, dall'archiviazione BLOB di Azure o da Azure Data Lake Store.** Sfrutta la velocità della tecnologia columnstore e delle funzionalità di analisi di Microsoft SQL importando i dati da Hadoop, dall'archiviazione BLOB di Azure o Azure Data Lake Store nelle tabelle relazionali. Non è necessario uno strumento di importazione o ETL separato.

- **Esportare i dati in Hadoop, nell'archivio BLOB di Azure o in Azure Data Lake Store.** Archivia i dati in Hadoop, nell'archivio BLOB di Azure o in Azure Data Lake Store per ottenere un'archiviazione conveniente e mantenerla online per semplificare l'accesso.

- **Integrarsi con strumenti BI.** Usa la polibase con la business intelligence e lo stack di analisi di Microsoft oppure usa qualsiasi strumento di terze parti compatibile con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="performance"></a>Prestazioni

- **Eseguire il push del calcolo in Hadoop.** PolyBase esegue il push di alcuni calcoli sull'origine esterna per ottimizzare la query complessiva. Query Optimizer prende una decisione basata sui costi per eseguire il push in Hadoop se in questo modo migliorano le prestazioni della query.  Per prendere la decisione basata sui costi, Query Optimizer usa le statistiche sulle tabelle esterne. Il push del calcolo crea processi MapReduce e sfrutta le risorse di calcolo distribuite di Hadoop. Per altre informazioni, vedere [distribuzione Computing in polibase](polybase-pushdown-computation.md). 

- **Ridimensionare le risorse di calcolo.** Per migliorare le prestazioni delle query, è possibile usare i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [gruppi con scalabilità orizzontale di base](../../relational-databases/polybase/polybase-scale-out-groups.md). Questo consente il trasferimento dei dati paralleli tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le istanze e i nodi Hadoop e aggiunge risorse di calcolo per operare sui dati esterni.

## <a name="next-steps"></a>Passaggi successivi

Prima di usare la polibase, è necessario [installare la Polibase in Windows](polybase-installation.md) o [installare la polibase in Linux](polybase-linux-setup.md)e abilitare la [polibase in sp_configure](polybase-installation.md#enable) , se necessario. Vedere quindi le guide di configurazione seguenti a seconda dell'origine dati:

- [Hadoop](polybase-configure-hadoop.md)
- [Archiviazione BLOB di Azure](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Tipi generici ODBC](polybase-configure-odbc-generic.md)
