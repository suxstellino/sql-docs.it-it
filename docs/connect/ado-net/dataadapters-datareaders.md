---
title: DataAdapter e DataReader
description: Informazioni sull'oggetto DataReader, che recupera i dati da un database, e sull'oggetto DataAdapter, che recupera i dati da un'origine dati e popola un oggetto DataSet, del provider di dati Microsoft SqlClient per SQL Server.
ms.date: 11/30/2020
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3a252ca44683c062e531081f0b625a5e2359d236
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771390"
---
# <a name="dataadapters-and-datareaders"></a>DataAdapter e DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

È possibile usare l'oggetto **DataReader** del provider di dati Microsoft SqlClient per SQL Server per recuperare un flusso di dati di sola lettura forward-only da un database. I risultati vengono restituiti durante l'esecuzione della query e vengono archiviati nel buffer di rete del client finché non vengono richiesti usando il metodo **Read** di **DataReader**. L'uso di **DataReader** consente di migliorare le prestazioni dell'applicazione recuperando i dati non appena sono disponibili e, per impostazione predefinita, archiviando solo una riga alla volta per evitare di sovraccaricare il sistema.

Un oggetto <xref:System.Data.Common.DataAdapter> viene usato per recuperare i dati da un'origine dati e compilare le tabelle all'interno di un oggetto <xref:System.Data.DataSet>. Il `DataAdapter` risolve inoltre le modifiche apportate al `DataSet` nell'origine dati. `DataAdapter` usa l'oggetto `Connection` del provider di dati Microsoft SqlClient per SQL Server per connettersi a un'origine dati e usa gli oggetti `Command` per recuperare i dati dall'origine dati e risolvere le modifiche apportatevi.

.NET ha un oggetto <xref:System.Data.Common.DbDataReader> e un oggetto <xref:System.Data.Common.DbDataAdapter>: il provider di dati Microsoft SqlClient per SQL Server include un oggetto <xref:Microsoft.Data.SqlClient.SqlDataReader> e un oggetto <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="in-this-section"></a>Contenuto della sezione

[Recuperare dati tramite un DataReader](retrieve-data-by-datareader.md)  
Descrive l'oggetto **DataReader** di ADO.NET e illustra come usarlo per restituire un flusso di risultati da un'origine dati.

[Popolare un DataSet da un DataAdapter](populate-dataset-from-dataadapter.md)  
Viene descritto come compilare un `DataSet` con tabelle, colonne e righe usando un `DataAdapter`.

[Parametri DataAdapter](dataadapter-parameters.md)  
Viene descritto come usare i parametri con le proprietà dei comandi di un `DataAdapter` e vengono fornite informazioni su come eseguire il mapping del contenuto di una colonna in un `DataSet` sul parametro di un comando.

[Aggiungere vincoli esistenti a un DataSet](add-existing-constraints-to-dataset.md)  
Viene descritto come aggiungere i vincoli esistenti a un `DataSet`.

[Mapping di DataAdapter, DataTable e DataColumn](dataadapter-datatable-datacolumn-mappings.md)  
Viene descritto come impostare `DataTableMappings` e `ColumnMappings` per un `DataAdapter`.

[Paging di un risultato di query](paging-through-query-result.md)  
Viene fornito un esempio di visualizzazione dei risultati di una query sotto forma di pagine di dati.

[Aggiornare origini dati con DataAdapter](update-data-sources-with-dataadapters.md)  
Viene descritto come usare un `DataAdapter` per applicare le modifiche apportate a un `DataSet` fino a risalire al database.

[Gestire eventi DataAdapter](handle-dataadapter-events.md)  
Vengono descritti gli eventi del `DataAdapter` e il relativo uso.

[Operazioni batch tramite DataAdapter](batch-operations-using-dataadapters.md)  
Viene descritto il miglioramento delle prestazioni delle applicazioni mediante la riduzione del numero dei round trip a SQL Server quando si applicano gli aggiornamenti dal `DataSet`.

## <a name="see-also"></a>Vedere anche

- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Comandi e parametri](commands-parameters.md)
- [Transazioni e concorrenza](transactions-and-concurrency.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
