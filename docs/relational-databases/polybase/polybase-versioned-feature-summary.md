---
description: Funzionalità e limitazioni di PolyBase
title: Funzionalità e limitazioni di PolyBase
descriptions: This article summarizes PolyBase features available for SQL Server products and services. It lists T-SQL operators supported for pushdown and known limitations.
ms.date: 04/19/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7ad508f4d745468a194ca95769439845887f2d1f
ms.sourcegitcommit: 708b3131db2e542b1d461d17dec30d673fd5f0fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107729123"
---
# <a name="polybase-features-and-limitations"></a>Funzionalità e limitazioni di PolyBase

[!INCLUDE[appliesto-ss2016-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

Questo articolo è un riepilogo delle funzionalità di PolyBase disponibili per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prodotti e servizi.  
  
## <a name="feature-summary-for-product-releases"></a>Riepilogo delle funzionalità per le versioni dei prodotti

Questa tabella elenca le funzionalità principali per PolyBase e i prodotti in cui sono disponibili.  

|**Funzionalità** |**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** (A partire dal 2016) |**Database SQL di Azure** |**[!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]** |**Parallel Data Warehouse** |
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

<sup>*</sup>Introdotto in [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , vedere Esempi di accesso bulk ai dati [nell'archiviazione BLOB di Azure.](../import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md)



## <a name="known-limitations"></a>Limitazioni note

PolyBase include le limitazioni seguenti:

- Per usare PolyBase, sono necessarie le autorizzazioni di livello CONTROL SERVER o sysadmin per il database.

- Prima di , la dimensione massima possibile delle righe, che include l'intera lunghezza delle colonne a lunghezza variabile, non può superare [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] i 32 KB in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o 1 MB in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] . A partire [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] da , questa limitazione viene revocata. Il limite rimane di 1 MB per le origini dati Hadoop, ma è limitato solo dal limite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] massimo per altre origini dati.

- Quando i dati vengono esportati in un formato di file ORC da o , è possibile che le colonne con un numero molto grande [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] di testo siano limitate. Possono essere limitate a 50 colonne a causa di messaggi di errore di memoria insufficiente di Java. Per risolvere questo problema, esportare solo un subset delle colonne.

- PolyBase non può connettersi a un'istanza di Hadoop se Knox è abilitato.

- Se si usano tabelle Hive con transactional=true, PolyBase non può accedere ai dati nella directory della tabella Hive.

- I servizi PolyBase richiedono SQL Server servizio per il corretto funzionamento del protocollo di rete TCP/IP. Inoltre, se l'impostazione di  configurazione Protocollo TCP/IP Ascolto tutto è impostata su **No,** è comunque necessario avere una voce per la porta del listener corretta in Porte **dinamiche TCP** o Porte **TCP** in **IPAll** in Proprietà TCP/IP. Questa operazione è necessaria a causa del modo in cui i servizi PolyBase risolvono la porta del listener del SQL Server motore.

<!--SQL Server 2016-->
::: moniker range="= sql-server-2016 "

- [PolyBase non viene installato quando si aggiunge un nodo a un [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] cluster di failover](https://support.microsoft.com/help/3173087/fix-polybase-feature-doesn-t-install-when-you-add-a-node-to-a-sql-server-2016-failover-cluster).

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Introduzione alla virtualizzazione dei dati con PolyBase.](polybase-guide.md)
