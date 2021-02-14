---
title: Virtualizzare i dati CSV dal pool di archiviazione
subtitle: Cluster Big Data di SQL Server
description: Procedura che illustra in dettaglio la creazione di una tabella esterna per la virtualizzazione di un file CSV in un cluster Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/24/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 7b78e44ee36e9320688a8a6815fbb267f1663d3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038671"
---
# <a name="virtualize-csv-data-from-storage-pool-big-data-clusters"></a>Virtualizzare i dati CSV dal pool di archiviazione (cluster Big Data)

I cluster Big Data di SQL Server possono virtualizzare i dati da file CSV in HDFS. Questo processo consente di mantenere i dati nel percorso originale, ma di poter eseguire query da un'istanza di SQL Server come per qualsiasi altra tabella. Questa funzionalità usa connettori PolyBase e riduce al minimo la necessità di processi ETL. Per altre informazioni sulla virtualizzazione dei dati, vedere [Che cos'è PolyBase](../relational-databases/polybase/polybase-guide.md).

## <a name="prerequisites"></a>Prerequisiti

- [Un cluster Big Data distribuito](deployment-guidance.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)

## <a name="select-or-upload-a-csv-file-for-data-virtualization"></a>Selezionare o caricare un file CSV per la virtualizzazione dei dati 

In Azure Data Studio (ADS) [connettersi all'istanza master di SQL Server](connect-to-big-data-cluster.md#master) del cluster Big Data. Una volta connessi, espandere gli elementi HDFS in Esplora oggetti per individuare i file CSV per cui si vuole eseguire la virtualizzazione dei dati. 

Ai fini di questa esercitazione, creare una nuova directory denominata **Data**.

1. Fare clic con il pulsante destro del mouse sul menu di scelta rapida della directory radice HDFS.
2. Scegliere **Nuova directory**.
3. Assegnare alla nuova directory il nome *Data*.

Caricare dati di esempio. Per una semplice procedura dettagliata, è possibile usare un file di dati CSV di esempio. Questo articolo usa i dati relativi ai ritardi delle compagnie aeree provenienti dal [ministero dei trasporti statunitense](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp?pn=1). Scaricare i dati non elaborati ed estrarre i dati nel computer. Denominare il file *airline_delay_causes.csv*.

Per caricare il file di esempio dopo l'estrazione:

1. In Azure Data Studio *fare clic con il pulsante destro del mouse* sulla nuova directory creata. 
2. Scegliere **Carica file**.

![File CSV di esempio in HDFS](media/data-virtualization/100-csv-sample-file-hdfs.png)

Azure Data Studio carica i file in HDFS nel cluster Big Data.

## <a name="create-the-storage-pool-external-data-source-in-your-target-database"></a>Creare l'origine dati esterna del pool di archiviazione nel database di destinazione

L'origine dati esterna del pool di archiviazione non viene creata in un database per impostazione predefinita nel cluster Big Data. Prima di poter creare l'origine dati esterna, creare l'origine dati esterna **SqlStoragePool** predefinita nel database di destinazione con la query Transact-SQL seguente. Verificare di aver prima impostato il contesto della query sul database di destinazione.

```sql
-- Create the default storage pool source for SQL Big Data Cluster
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
    CREATE EXTERNAL DATA SOURCE SqlStoragePool
    WITH (LOCATION = 'sqlhdfs://controller-svc/default');
```

## <a name="create-the-external-table"></a>Creare la tabella esterna

In ADS fare clic con il pulsante destro del mouse sul file CSV e quindi scegliere **Create External Table From CSV File** (Crea tabella esterna da file CSV) dal menu di scelta rapida. È anche possibile creare tabelle esterne da file CSV di una directory in HDFS a patto che i file nella directory seguano lo stesso schema. Ciò consentirebbe la virtualizzazione dei dati a livello di directory senza la necessità di elaborare singoli file e ottenere un set di risultati unito in join per i dati combinati. Azure Data Studio consente di eseguire in modo guidato i passaggi per creare la tabella esterna.

Specificare il database, l'origine dati, il nome della tabella, lo schema e il nome per il formato di file esterno della tabella.

Fare clic su **Avanti**.

## <a name="preview-data"></a>Anteprima dei dati

Azure Data Studio fornisce un'anteprima dei dati importati.

![Screenshot che mostra la finestra per la creazione di una tabella esterna da CSV con un'anteprima dei dati importati.](media/data-virtualization/130-csv-preview-data.png)

Al termine della visualizzazione dell'anteprima, fare clic su **Avanti** per continuare

## <a name="modify-columns"></a>Modificare le colonne

Nella finestra successiva è possibile modificare le colonne della tabella esterna che si intende creare. È possibile modificare il nome della colonna, modificare il tipo di dati, nonché consentire righe che ammettono i valori Null. 

![Screenshot della finestra per la creazione di una tabella esterna da CSV che mostra il passaggio 3 per la modifica delle colonne.](media/data-virtualization/140-csv-modify-columns.png)

Dopo aver verificato le colonne di destinazione, fare clic su **Avanti**.

## <a name="summary"></a>Summary

Questo passaggio visualizza un riepilogo delle selezioni. Fornisce il nome dell'istanza di SQL Server, il nome del database, il nome della tabella, lo schema della tabella e le informazioni sulla tabella esterna. In questo passaggio è possibile generare uno script o creare una tabella. **Genera script** crea uno script in T-SQL per creare l'origine dati esterna. **Crea tabella** crea l'origine dati esterna.

![Schermata Riepilogo](media/data-virtualization/150-csv-virtualize-data-summary.png)

Se si fa clic **Crea tabella**, SQL Server crea la tabella esterna nel database di destinazione.

Se si fa clic su **Genera script**, Azure Data Studio crea la query T-SQL per la creazione della tabella esterna.

Una volta creata, è possibile eseguire query direttamente sulla tabella usando T-SQL dall'istanza di SQL Server.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data di SQL Server e gli scenari correlati, vedere [What is SQL Server Big Data Cluster?](big-data-cluster-overview.md) (Che cos'è un cluster Big Data di SQL Server?).
