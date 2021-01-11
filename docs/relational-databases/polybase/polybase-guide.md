---
title: Che cos'è PolyBase? | Microsoft Docs
description: PolyBase consente all'istanza di SQL Server di elaborare query Transact-SQL che leggono i dati da origini dati esterne, ad esempio Hadoop e Archiviazione BLOB di Azure.
ms.date: 12/14/2019
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
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: afcc848a169ec6a9c9dd02ecee50718d7e1508c1
ms.sourcegitcommit: 81f06a754fcaf4b72136859ccb18c82693f24780
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811536"
---
# <a name="what-is-polybase"></a>Che cos'è PolyBase?

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

PolyBase consente all'istanza di SQL Server di elaborare query Transact-SQL che leggono i dati da origini dati esterne. La stessa query può anche accedere a tabelle relazionali nell'istanza di SQL Server. PolyBase consente anche alla stessa query di creare un join dei dati da origini esterne e SQL Server.

Per usare PolyBase in un'istanza di SQL Server:

1. [Installare PolyBase in Windows](polybase-installation.md)
1. Creare un'[origine dati esterna](../../t-sql/statements/create-external-data-source-transact-sql.md)
1. Creare una [tabella esterna](../../t-sql/statements/create-external-table-transact-sql.md)

Insieme consentono di connettersi all'origine dati esterna.

SQL Server 2016 introduce PolyBase con supporto per le connessioni a Hadoop e all'Archiviazione BLOB di Azure.

SQL Server 2019 introduce connettori aggiuntivi, tra cui SQL Server, Oracle, Teradata e MongoDB.

![Logica di PolyBase](../../relational-databases/polybase/media/polybase-logical.png "Logica di PolyBase")

PolyBase esegue il push di alcuni calcoli sull'origine esterna per ottimizzare la query complessiva. L'accesso esterno di PolyBase non è limitato a Hadoop. Sono supportate anche altre tabelle non relazionali non strutturate, ad esempio i file di testo delimitato.

Esempi di connettori esterni sono i seguenti:

- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)

### <a name="supported-sql-products-and-services"></a>Prodotti e servizi SQL supportati

PolyBase offre queste stesse funzionalità per i prodotti SQL seguenti di Microsoft:

- SQL Server 2016 e versioni successive (solo Windows)
- Piattaforma di strumenti analitici (in precedenza Parallel Data Warehouse)
- Azure Synapse Analytics

### <a name="azure-integration"></a>Integrazione con Azure

Con il supporto sottostante di PolyBase, le query T-SQL possono anche importare ed esportare dati da Archiviazione BLOB di Azure. PolyBase consente inoltre ad Azure Synapse Analytics di importare ed esportare dati da Azure Data Lake Store e da Archiviazione BLOB di Azure.

## <a name="why-use-polybase"></a>Perché usare PolyBase

PolyBase consente di unire i dati di un'istanza di SQL Server con dati esterni. Prima di PolyBase, per aggiungere i dati alle origini dati esterne era possibile eseguire una delle operazioni seguenti:

- Trasferire la metà dei dati in modo che tutti i dati fossero in una posizione.
- Eseguire query su entrambe le origini dati, quindi scrivere logica di query personalizzata per creare i join e integrare i dati a livello di client.

PolyBase consente di usare semplicemente Transact-SQL per unire i dati.

PolyBase non richiede di installare software aggiuntivo nell'ambiente Hadoop. Si possono eseguire query sui dati esterni usando la stessa sintassi T-SQL usata per eseguire query su una tabella di database. Le azioni di supporto implementate da PolyBase vengono tutte eseguite in modo trasparente. L'autore della query non deve conoscere l'origine esterna.

### <a name="polybase-uses"></a>Usi di PolyBase

PolyBase rende possibili gli scenari seguenti in SQL Server:

- **Eseguire query sui dati archiviati in Hadoop da un'istanza di SQL Server o PDW.** Gli utenti scelgono di archiviare i dati in sistemi distribuiti e scalabili convenienti, come Hadoop. PolyBase semplifica la query dei dati con T-SQL.

- **Eseguire query sui dati archiviati in Archiviazione BLOB di Azure.** Nell'archivio BLOB di Azure è possibile salvare i dati da usare con i servizi di Azure.  PolyBase semplifica l'accesso ai dati con T-SQL.

- **Importare i dati da Hadoop, Archiviazione BLOB di Azure o Azure Data Lake Store.** Sfruttare la velocità della tecnologia columnstore e delle funzionalità di analisi di Microsoft SQL importando i dati da Hadoop, Archiviazione BLOB di Azure o Azure Data Lake Store in tabelle relazionali. Non è necessario uno strumento di importazione o ETL separato.

- **Esportare i dati in Hadoop, nell'archivio BLOB di Azure o in Azure Data Lake Store.** Archiviare i dati in Hadoop, nell'archivio BLOB di Azure o in Azure Data Lake Store per un'archiviazione conveniente e mantenerli online per accedervi facilmente.

- **Integrarsi con strumenti BI.** Usare PolyBase con la business intelligence e lo stack di analisi di Microsoft o usare strumenti di terze parti compatibili con SQL Server.

## <a name="performance"></a>Prestazioni

- **Eseguire il push del calcolo in Hadoop.** Query Optimizer prende una decisione basata sui costi per eseguire il push in Hadoop se in questo modo migliorano le prestazioni della query.  Per prendere la decisione basata sui costi, Query Optimizer usa le statistiche sulle tabelle esterne. Il push del calcolo crea processi MapReduce e sfrutta le risorse di calcolo distribuite di Hadoop.

- **Ridimensionare le risorse di calcolo.** Per migliorare le prestazioni delle query, è possibile usare i [gruppi con scalabilità orizzontale di PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md)per SQL Server. In questo modo viene abilitato il trasferimento dei dati paralleli tra le istanze di SQL Server e i nodi di Hadoop e vengono aggiunte le risorse di calcolo per operare sui dati esterni.

## <a name="next-steps"></a>Passaggi successivi

Prima di usare PolyBase, è necessario [installare la funzionalità PolyBase](polybase-installation.md). Vedere quindi le guide di configurazione seguenti a seconda dell'origine dati:

- [Hadoop](polybase-configure-hadoop.md)
- [Archiviazione BLOB di Azure](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Tipi generici ODBC](polybase-configure-odbc-generic.md)