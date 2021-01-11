---
title: Esecuzione di un comando
description: Descrive l'oggetto `Command` del provider di dati Microsoft SqlClient per SQL Server e come usarlo per eseguire query e comandi su un'origine dati.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 40494916-c25a-4cb8-8f7c-fcb8d378464e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 772ecef470b69a34e4bd4b0eba2ca60255657eaf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771423"
---
# <a name="executing-a-command"></a>Esecuzione di un comando

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il provider di dati Microsoft SqlClient per SQL Server include l'oggetto <xref:Microsoft.Data.SqlClient.SqlCommand> che eredita da <xref:System.Data.Common.DbCommand>. Questo oggetto espone i metodi per eseguire i comandi in base al tipo di comando e al valore restituito desiderato, come descritto nella tabella seguente.

|Comando|Valore restituito|  
|-------------|------------------|  
|`ExecuteReader`|Restituisce un oggetto `DataReader`.|  
|`ExecuteScalar`|Restituisce un singolo valore scalare.|  
|`ExecuteNonQuery`|Esegue un comando che non restituisce righe.|  
|`ExecuteXMLReader`|Restituisce un valore <xref:System.Xml.XmlReader>. Disponibile solo per un oggetto `SqlCommand`.|

Ogni oggetto comando fortemente tipizzato supporta anche un'enumerazione <xref:System.Data.CommandType> che specifica la modalità di interpretazione di una stringa di comando, come descritto nella tabella seguente.

|CommandType|Descrizione|
|-----------------|-----------------|  
|`Text`|Comando SQL che definisce le istruzioni da eseguire nell'origine dati.|  
|`StoredProcedure`|Nome della stored procedure. È possibile usare la proprietà `Parameters` di un comando per accedere a parametri di input e output e a valori restituiti indipendentemente dal metodo `Execute` chiamato.|  
|`TableDirect`|Nome di una tabella.|

> [!IMPORTANT]
> Quando si usa `ExecuteReader`, non è possibile accedere ai valori restituiti e ai parametri di output fino a quando `DataReader` non viene chiuso.

## <a name="example"></a>Esempio

Nell'esempio di codice seguente viene illustrato come creare un oggetto <xref:Microsoft.Data.SqlClient.SqlCommand> per eseguire una stored procedure impostando le relative proprietà. Per specificare il parametro di input per la stored procedure, viene usato un oggetto <xref:Microsoft.Data.SqlClient.SqlParameter>. Il comando viene eseguito usando il metodo <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A> e l'output di <xref:Microsoft.Data.SqlClient.SqlDataReader> viene visualizzato nella finestra della console.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

### <a name="troubleshooting-commands"></a>Risoluzione dei problemi dei comandi

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Il provider di dati Microsoft SqlClient per SQL Server aggiunge **contatori delle prestazioni** per consentire di rilevare i problemi intermittenti correlati a esecuzioni di comandi non riuscite. Per altre informazioni, vedere [Contatori delle prestazioni in SqlClient](performance-counters.md).

## <a name="see-also"></a>Vedere anche

- [Comandi e parametri](commands-parameters.md)
- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
