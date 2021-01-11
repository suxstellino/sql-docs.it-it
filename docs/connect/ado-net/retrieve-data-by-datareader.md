---
title: Recuperare dati tramite un DataReader
description: Informazioni su come recuperare i dati usando DataReader in ADO.NET con questo codice di esempio. DataReader fornisce un flusso di dati non archiviato nel buffer.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: dfbe41fe29a3dadc69a2a428f1bf8b606a2a7050
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771351"
---
# <a name="retrieve-data-by-a-datareader"></a>Recuperare dati tramite un DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Per recuperare dati usando **DataReader**, creare un'istanza dell'oggetto **Command** e quindi creare **DataReader** chiamando **Command.ExecuteReader** per recuperare righe da un'origine dati. **DataReader** fornisce un flusso di dati non archiviati nel buffer che consente alla logica procedurale di elaborare sequenzialmente i risultati provenienti da un'origine dati.

> [!NOTE]
> L'uso di **DataReader** è consigliabile quando si recuperano grandi quantità di dati perché i dati non sono memorizzati nella cache.

L'esempio seguente illustra l'uso di **DataReader** in cui `reader` rappresenta un oggetto DataReader valido e `command` rappresenta un oggetto Command valido.  

```csharp
reader = command.ExecuteReader();  
```

Usare il metodo **DataReader.Read** per ottenere una riga dai risultati della query. È possibile accedere a ogni colonna della riga restituita passando il nome o il numero ordinale della colonna a **DataReader**. Per migliorare le prestazioni, tuttavia, **DataReader** fornisce una serie di metodi che consentono di accedere ai valori delle colonne nei tipi di dati nativi, quali **GetDateTime**, **GetDouble**, **GetGuid**, **GetInt32** e così via. Per un elenco dei metodi delle funzioni di accesso tipizzate per **DataReader** specifici del provider di dati, vedere <xref:Microsoft.Data.SqlClient.SqlDataReader>. L'uso di metodi di funzioni di accesso tipizzate quando si conosce il tipo di dati sottostante riduce il numero di conversioni dei tipi necessarie al momento del recupero del valore della colonna.  

L'esempio seguente scorre un oggetto **DataReader** e restituisce due colonne da ogni riga.  

[!code-csharp[DataWorks SqlClient.HasRows#1](~/../sqlclient/doc/samples/SqlDataReader_HasRows.cs#1)]

## <a name="close-the-datareader"></a>Chiudere l’oggetto DataReader  

Chiamare sempre il metodo `Close()` una volta terminato l'uso dell'oggetto `DataReader`.

> [!NOTE]
> I parametri di output o i valori restituiti presenti nell'oggetto **Command** non sono disponibili fino a quando l'oggetto **DataReader** non viene chiuso.  

> [!IMPORTANT]
> Mentre un oggetto **DataReader** è aperto, l'oggetto **Connection** viene usato esclusivamente da tale oggetto **DataReader**. Non sarà possibile eseguire alcun comando per l'oggetto **Connection** né creare un altro oggetto **DataReader** fino a quando l'oggetto **DataReader** originale non viene chiuso.  

> [!NOTE]
> Non chiamare **Close** o **Dispose** su un oggetto **Connection**, un oggetto **DataReader** o su qualsiasi altro oggetto gestito nel metodo **Finalize** della classe. Nei finalizzatori rilasciare solo le risorse non gestite che la classe controlla direttamente. Se nella classe non sono presenti risorse non gestite, non includere un metodo **Finalize** nella definizione della classe. Per altre informazioni, vedere [Garbage Collection](/dotnet/standard/garbage-collection/index).
 
## <a name="retrieve-multiple-result-sets-using-nextresult"></a>Recuperare più set di risultati usando NextResult

Se l'oggetto **DataReader** restituisce più set di risultati, chiamare il metodo **NextResult** per scorrere in modo sequenziale i set di risultati. Nell'esempio seguente viene illustrata l'elaborazione dei risultati di due dichiarazioni SELECT da parte del tipo <xref:Microsoft.Data.SqlClient.SqlDataReader> usando il metodo <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A>.  

[!code-csharp[DataWorks SqlClient.NextResult#1](~/../sqlclient/doc/samples/SqlDataReader_NextResult.cs#1)]

## <a name="get-schema-information-from-the-datareader"></a>Ottenere informazioni sullo schema dall'oggetto DataReader  

Mentre un oggetto **DataReader** è aperto, è possibile recuperare le informazioni sullo schema relative al set di risultati corrente usando il metodo **GetSchemaTable**. **GetSchemaTable** restituisce un oggetto <xref:System.Data.DataTable> popolato con righe e colonne contenenti le informazioni sullo schema del set di risultati corrente. L'oggetto **DataTable** contiene una riga per ogni colonna del set di risultati. Ogni colonna della tabella dello schema è associata a una proprietà delle colonne restituite nelle righe del set di risultati, in cui **ColumnName** è il nome della proprietà e il valore della colonna è il valore della proprietà. L'esempio seguente rilascia le informazioni sullo schema per **DataReader**.  

[!code-csharp[DataWorks SqlClient.GetSchemaTable#1](~/../sqlclient/doc/samples/SqlDataReader_GetSchemaTable.cs#1)]

## <a name="see-also"></a>Vedere anche

- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Comandi e parametri](commands-parameters.md)
- [Recupero di informazioni dello schema del database](retrieving-database-schema-information.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
