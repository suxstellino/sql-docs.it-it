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
ms.openlocfilehash: c3cf3766ffc6c8acf6025b58aa0adbaafafa2188
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038959"
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

[Transazioni e concorrenza](transactions-and-concurrency.md) Sono inclusi argomenti che descrivono come eseguire transazioni locali, transazioni distribuite e usare la concorrenza ottimistica.

[Recupero di informazioni dello schema del database](retrieving-database-schema-information.md) Descrizione di come ottenere i database o i cataloghi disponibili, le tabelle e le viste in un database, i vincoli esistenti per le tabelle e altre informazioni sullo schema da un'origine dati.

## <a name="see-also"></a>Vedere anche

- [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server e ADO.NET](./sql/index.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
