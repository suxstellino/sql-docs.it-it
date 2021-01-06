---
title: Mapping di tipi di dati
description: Descrive i tipi di dati usati dal provider di dati Microsoft SqlClient per SQL Server.
ms.date: 11/13/2020
ms.assetid: d4afab94-ada6-4c77-a73c-41f17bae6b5a
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 808acdf89350f539f03f6fdc75f2f7a5ed5b7707
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771613"
---
# <a name="data-type-mappings-in-adonet"></a>Mapping dei tipi di dati in ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

ADO.NET si basa su Common Type System, che definisce la modalità di dichiarazione, uso e gestione dei tipi in runtime. È costituito sia da tipi di valore che da tipi di riferimento, che derivano tutti dal tipo di base <xref:System.Object>. Quando si usa un'origine dati, il tipo di dati viene dedotto dal provider di dati, se non è specificato in modo esplicito. Un oggetto <xref:System.Data.DataSet> è ad esempio indipendente da qualsiasi origine dati specifica. I dati in un oggetto `DataSet` vengono recuperati da un'origine dati e le modifiche vengono applicate nell'origine dati usando un oggetto `DataAdapter`. Questo flusso di programma significa che quando un `DataAdapter` compila un <xref:System.Data.DataTable> in un `DataSet` con i valori di un'origine dati, i tipi di dati risultanti delle colonne nella`DataTable` sono tipi .NET Framework, anziché tipi specifici del provider di dati Microsoft SqlClient per SQL Server usati per la connessione all'origine dati.

Analogamente, quando un `DataReader` restituisce un valore da un'origine dati, il valore risultante viene archiviato in una variabile locale con un tipo .NET Framework. Per entrambe le operazioni `Fill` dei metodi `DataAdapter` e `Get` del `DataReader`, il tipo .NET Framework viene dedotto dal valore restituito dal provider di dati Microsoft SqlClient per SQL Server.

Anziché basarsi sul tipo di dati dedotto, quando si conosce il tipo specifico del valore che viene restituito è consigliabile usare i metodi delle funzioni di accesso tipizzate di `DataReader` . I metodi delle funzioni di accesso tipizzate offrono prestazioni migliori restituendo un valore come tipo .NET Framework specifico, eliminando la necessità di una conversione di tipi aggiuntiva.

> [!NOTE]
> I valori Null per il provider di dati Microsoft SqlClient per SQL Server sono rappresentati da `DBNull.Value`.

## <a name="in-this-section"></a>Contenuto della sezione

[Mapping dei tipi di dati SQL Server](sql-server-data-type-mappings.md) elenca i mapping dei tipi di dati dedotti e i metodi delle funzioni di accesso ai dati per <xref:Microsoft.Data.SqlClient>.

[Numeri a virgola mobile](floating-point-numbers.md) descrive i problemi riscontrati frequentemente dagli sviluppatori quando usano numeri a virgola mobile.

## <a name="see-also"></a>Vedere anche

- [Tipi di dati SQL Server e ADO.NET](./sql/sql-server-data-types.md)
- [Configurazione dei parametri](configure-parameters.md)
- [Recupero di informazioni dello schema del database](retrieving-database-schema-information.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
