---
title: Recupero e modifica di dati
description: In .NET, il provider di dati Microsoft SqlClient per SQL Server funge da ponte tra un'applicazione e un'origine dati per la lettura e l'aggiornamento dei dati.
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4275b7de0f31d03aa36ef31d8801fcdc0e9ec853
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771546"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>Recupero e modifica di dati in ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La connessione a un'origine dati e il recupero dei dati in essa contenuti sono funzioni fondamentali nelle applicazioni di database. Il provider di dati SqlClient funge da ponte tra un'applicazione e un'origine dati, consentendo di eseguire comandi e di recuperare dati tramite un **DataReader** o un **DataAdapter**. Una funzione chiave di qualsiasi applicazione di database è la capacità di aggiornare i dati archiviati nel database. Nel provider di dati Microsoft SqlClient per SQL Server l'aggiornamento dei dati prevede l'uso di oggetti **DataAdapter**, <xref:System.Data.DataSet> e **Command**. Può anche prevedere l'uso di transazioni.

## <a name="in-this-section"></a>Contenuto della sezione

[Connessione a un'origine dati](connecting-to-data-source.md)  
Viene descritto come stabilire una connessione a un'origine dati e come usare gli eventi di connessione.

[Stringhe di connessione](connection-strings.md)  
Sono inclusi argomenti in cui vengono descritti diversi aspetti relativi all'utilizzo delle stringhe di connessione, quali le parole chiave, le informazioni di sicurezza e l'archiviazione e il recupero delle stringhe di connessione.

[Pool di connessioni](connection-pooling.md)  
Descrive i pool di connessioni per il provider di dati Microsoft SqlClient per SQL Server.

[Comandi e parametri](commands-parameters.md)  
Sono inclusi argomenti in cui viene descritto come creare comandi e compilatori di comandi, come configurare parametri e come eseguire comandi per recuperare e modificare dati.

[DataAdapter e DataReader](dataadapters-datareaders.md)  
Sono inclusi argomenti in cui vengono descritti DataReaders, DataAdapters, i parametri, la gestione di eventi DataAdapter e l'esecuzione di operazioni batch.

[Transazioni e concorrenza](transactions-and-concurrency.md)  
Sono inclusi argomenti in cui viene descritto come eseguire transazioni locali e transazioni distribuite e come usare concorrenza ottimistica.

[Recupero di informazioni dello schema del database](retrieving-database-schema-information.md)  
Viene descritto come ottenere da un'origine dati database o cataloghi disponibili, tabelle e visualizzazioni in un database, vincoli esistenti per tabelle e altre informazioni relative allo schema.

[Oggetti DbProviderFactory](dbproviderfactories.md)  
Viene descritto il modello a livello di factory del provider e viene illustrato come usare le classi base nello spazio dei nomi `System.Data.Common`.  

[Recuperare i valori Identity o di numerazione automatica](retrieve-identity-or-autonumber-values.md)  
Vengono forniti esempi di mapping dei valori generati per una colonna **identity** di una tabella di SQL Server a una colonna di una riga inserita in una tabella. Viene descritta l'unione di valori Identity in un oggetto `DataTable`.  
  
[Recuperare i dati binari](retrieve-binary-data.md)  
Viene descritto come recuperare dati binari o strutture di dati di grandi dimensioni usando `CommandBehavior`.`SequentialAccess` per modificare il comportamento predefinito di un oggetto `DataReader`.  
  
[Modificare i dati con stored procedure](modify-data-with-stored-procedures.md)  
Viene descritto come usare i parametri di input e di output della stored procedure per inserire una riga in un database, restituendo un nuovo valore Identity.  

[Traccia dei dati in SqlClient](data-tracing.md)  
Viene descritto il modo in cui il provider di dati Microsoft SqlClient per SQL Server offre funzionalità di traccia dei dati incorporate.  
  
[Contatori delle prestazioni in SqlClient](performance-counters.md)  
Vengono descritti i contatori delle prestazioni disponibili per il provider di dati Microsoft SqlClient per SQL Server.  
  
[Programmazione asincrona](asynchronous-programming.md)  
Viene descritto il supporto del provider di dati Microsoft SqlClient per SQL Server per la programmazione asincrona.  
  
[Supporto dello streaming in SqlClient](sqlclient-streaming-support.md)  
Viene descritto come scrivere applicazioni che trasmettono i dati da SQL Server senza doverli caricare completamente in memoria.  

## <a name="see-also"></a>Vedere anche

- [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server e ADO.NET](./sql/index.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
