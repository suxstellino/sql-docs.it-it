---
description: Funzionalità e limitazioni di PolyBase
title: Funzionalità e limitazioni di PolyBase
descriptions: This article summarizes PolyBase features available for SQL Server products and services. It lists T-SQL operators supported for pushdown and known limitations.
ms.date: 03/23/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0ce732ab5e7f920f19a47537419242b637cda91c
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103758"
---
# <a name="polybase-features-and-limitations"></a>Funzionalità e limitazioni di PolyBase

[!INCLUDE[appliesto-ss2016-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

Questo articolo è un riepilogo delle funzionalità di polibase disponibili per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prodotti e i servizi.  
  
## <a name="feature-summary-for-product-releases"></a>Riepilogo delle funzionalità per le versioni dei prodotti

Questa tabella elenca le funzionalità principali per PolyBase e i prodotti in cui sono disponibili.  

|**Funzionalità** |**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** (A partire da 2016) |**Database SQL di Azure** |**[!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]** |**Parallel Data Warehouse** |
|---------|---------|---------|---------|---------|
|Eseguire query sui dati di Hadoop con [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sì|No|No|Sì|
|Importare dati da Hadoop|Sì|No|No|Sì|
|Esportare dati in Hadoop  |Sì|No|No| Sì|
|Eseguire query, importare da Azure HDInsight |No|No|No|No
|Eseguire il push down dei calcoli delle query in Hadoop|Sì|No|No|Sì|  
|Importare dati dall'archivio BLOB di Azure|Sì|Sì<sup>*</sup>|Sì|Sì|
|Esportare dati nell'archivio BLOB di Azure|Sì|No|Sì|Sì|  
|Importare dati da Azure Data Lake Store|No|No|Sì|No|
|Esportare dati in Azure Data Lake Store|No|No|Sì|No|
|Eseguire query PolyBase da strumenti BI di Microsoft|Sì|No|Sì|Sì|

<sup>*</sup> Introdotta in [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , vedere [esempi di accesso bulk ai dati nell'archivio BLOB di Azure](../import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).


## <a name="syntax-that-prevents-pushdown"></a>Sintassi che impedisce distribuzione

La sintassi o le funzioni T-SQL seguenti impediranno il calcolo distribuzione:

- `AT TIME ZONE`
- `CONCAT_WS`
- `TRANSLATE`
- `RAND`
- `CHECKSUM`
- `BINARY_CHECKSUM`
- `ISJSON`
- `JSON_VALUE`
- `JSON_QUERY`
- `JSON_MODIFY`
- `NEWID`
- `STRING_ESCAPE`
- `COMPRESS`
- `DECOMPRESS`
- `GREATEST`
- `LEAST`
- `PARSE`

Il supporto di distribuzione per la `FORMAT` `TRIM` sintassi e è stato introdotto in [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] CU10 dalla.

Per altre informazioni, vedere [distribuzione Computing in polibase](polybase-pushdown-computation.md).

## <a name="pushdown-computation-supported-by-t-sql-operators"></a>Calcolo della distribuzione dinamica supportata da operatori T-SQL

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e APS non tutti gli operatori T-SQL possono essere spostati nel cluster Hadoop. Nella tabella seguente sono elencati tutti gli operatori supportati e un subset degli operatori non supportati.

|**Tipo di operatore** |**Distribuibile su hadoop** |**Distribuibile nell'archiviazione BLOB** | 
|---------|---------|---------|
|Proiezioni di colonna|Sì|No|
|Predicati|Sì|No|
|Aggregazioni|Parziale\*|No|
|Crea un join tra le tabelle esterne|No|No|
|Crea un join tra le tabelle esterne e locali|No|No|
|Ordina|No|No|

\* L'aggregazione parziale significa che un'aggregazione finale deve verificarsi dopo che i dati raggiungono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tuttavia, una parte dell'aggregazione viene eseguita in Hadoop. Si tratta di un metodo comune di calcolo delle aggregazioni nei sistemi con la funzionalità di elaborazione parallela massiva.  

#### <a name="hadoop-pushdown-support"></a>Supporto di Hadoop distribuzione

I provider Hadoop supportano quanto segue:

| **Aggregazioni**                  | **Filtri (confronto binario)** | 
|-----------------------------------|---------------------------------| 
| Count_Big                         | NotEqual                        | 
| Sum                               | LessThan                        | 
| Media                               | LessOrEqual                     | 
| Max                               | GreaterOrEqual                  | 
| Min                               | GreaterThan                     | 
| Approx_Count_Distinct             | È                              | 
|                                   | IsNot                           | 
|                                   |                                 | 

## <a name="known-limitations"></a>Limitazioni note

PolyBase include le limitazioni seguenti:

- Per usare PolyBase, sono necessarie le autorizzazioni di livello CONTROL SERVER o sysadmin per il database.

- Prima di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] , la dimensione massima possibile per la riga, che include la lunghezza totale delle colonne a lunghezza variabile, non può superare 32 KB in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o 1 MB in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] . A partire da [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] , questa limitazione viene revocata. Il limite rimane 1 MB per le origini dati Hadoop, ma è limitato solo dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] limite massimo per le altre origini dati.

- Quando i dati vengono esportati in un formato di file ORC da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o, le colonne con utilizzo [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] intensivo del testo potrebbero essere limitate. Possono essere limitate a 50 colonne a causa di messaggi di errore di memoria insufficiente di Java. Per risolvere questo problema, esportare solo un subset delle colonne.

- La funzionalità polibase non può connettersi a un'istanza di Hadoop se Knox è abilitato.

- Se si usano tabelle Hive con transactional=true, PolyBase non può accedere ai dati nella directory della tabella Hive.

<!--SQL Server 2016-->
::: moniker range="= sql-server-2016 "

- [La polibase non viene installata quando si aggiunge un nodo a un [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] cluster di failover](https://support.microsoft.com/help/3173087/fix-polybase-feature-doesn-t-install-when-you-add-a-node-to-a-sql-server-2016-failover-cluster).

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su polibase, vedere [che cos'è la polibase?](polybase-guide.md)
