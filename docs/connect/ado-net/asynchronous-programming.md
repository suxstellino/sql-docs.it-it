---
title: Programmazione asincrona
description: Informazioni sulla programmazione asincrona nel provider di dati Microsoft SqlClient per SQL Server.
ms.date: 12/04/2020
ms.assetid: 85da7447-7125-426e-aa5f-438a290d1f77
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6eec681974499219a1ca649f9a47449a27ff4002
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804306"
---
# <a name="asynchronous-programming"></a>Programmazione asincrona

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In questo argomento viene trattato il supporto della programmazione asincrona nel provider di dati Microsoft SqlClient per SQL Server (SqlClient).

## <a name="legacy-asynchronous-programming"></a>Programmazione asincrona legacy

Il provider di dati Microsoft SqlClient per SQL Server include metodi di **System.Data.SqlClient** per mantenere la compatibilità con le versioni precedenti per le applicazioni che eseguono la migrazione a <xref:Microsoft.Data.SqlClient>. Non è consigliabile usare i metodi di programmazione asincrona legacy seguenti per il nuovo sviluppo:

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteReader%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A?displayProperty=nameWithType>

> [!TIP]
> Nel provider di dati Microsoft SqlClient per SQL Server, questi metodi legacy non richiedono più `Asynchronous Processing=true` nella stringa di connessione.

## <a name="asynchronous-programming-features"></a>Funzionalità della programmazione asincrona

Queste funzionalità di programmazione asincrona offrono una tecnica semplice per rendere il codice asincrono.

Per altre informazioni sulla programmazione asincrona in .NET, vedere:

- [Programmazione asincrona in C#](/dotnet/csharp/async)

- [Programmazione asincrona con Async e Await (Visual Basic)](/dotnet/visual-basic/programming-guide/concepts/async/index)

Quando l'interfaccia utente non risponde o il server non è scalabile, si potrebbe aver bisogno di un codice più asincrono. La scrittura di codice asincrono ha richiesto in genere l'installazione di un callback (denominato anche continuazione) per esprimere la logica che si verifica al termine dell'operazione asincrona. Ciò complica la struttura del codice asincrono rispetto al codice sincrono.

È possibile chiamare i metodi asincroni senza usare callback e senza suddividere il codice in più metodi o espressioni lambda.

Il modificatore `async` specifica che un metodo è asincrono. Quando si chiama un metodo `async`, viene restituita un'attività. Quando l'operatore `await` viene applicato a un'attività, il metodo corrente si chiude immediatamente. Al termine dell'attività, l'esecuzione riprende in corrispondenza dello stesso metodo.

> [!TIP]
> Nel provider di dati Microsoft SqlClient per SQL Server, le chiamate asincrone non sono necessarie per impostare la parola chiave della stringa di connessione `Context Connection`.

La chiamata di un metodo `async` non assegna thread aggiuntivi. È possibile usare brevemente il thread di completamento di I/O esistente alla fine.

I metodi seguenti nel provider di dati Microsoft SqlClient per SQL Server supportano la programmazione asincrona:

- <xref:System.Data.Common.DbConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteDbDataReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.GetFieldValueAsync%2A>

- <xref:System.Data.Common.DbDataReader.IsDBNullAsync%2A>

- <xref:System.Data.Common.DbDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteXmlReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>

Altri membri asincroni prevedono il [supporto dello streaming in SqlClient](sqlclient-streaming-support.md).

> [!TIP]
> I metodi asincroni non richiedono `Asynchronous Processing=true` nella stringa di connessione. E questa proprietà è obsoleta nel provider di dati Microsoft SqlClient per SQL Server.

### <a name="synchronous-to-asynchronous-connection-open"></a>Connessione da sincrona ad asincrona aperta

È possibile aggiornare un'applicazione esistente per l'utilizzo della funzionalità asincrona. Ad esempio, si presupponga che un'applicazione disponga di un algoritmo di connessione sincrono e che blocchi il thread UI ogni volta che si connette al database e che, una volta che connessa, l'applicazione chiami una stored procedure che segnala agli altri utenti l'utente che ha appena effettuato l'accesso.

[!code-csharp[SqlCommand_ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery.cs#1)]

Quando viene convertito per utilizzare la funzionalità asincrona, il programma è simile al seguente:

[!code-csharp[SqlCommand_ExecuteNonQueryAsync#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQueryAsync.cs#1)]

### <a name="add-the-asynchronous-feature-in-an-existing-application-mixing-old-and-new-patterns"></a>Aggiungere la funzionalità asincrona in un'applicazione esistente (combinando modelli vecchi e nuovi)

È possibile inoltre aggiungere la funzionalità asincrona (SqlConnection::OpenAsync) senza modificare la logica asincrona esistente. Ad esempio, se un'applicazione attualmente usa:

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#1](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#1)]

È possibile iniziare a usare il modello asincrono senza modificare sostanzialmente l'algoritmo esistente.

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#2](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#2)]

### <a name="use-the-base-provider-model-and-the-asynchronous-feature"></a>Usare il modello di provider di base e la funzionalità asincrona

Potrebbe essere necessario creare uno strumento in grado di connettersi al database e di eseguire query. È possibile usare il modello di provider di base e la funzionalità asincrona.

È necessario abilitare il servizio Microsoft Distributed Transaction Controller (MSDTC) sul server per usare transazioni distribuite. Per informazioni su come abilitare MSDTC, vedere la [pagina relativa all'abilitazione di MSDTC su un server Web](/previous-versions/commerce-server/dd327979(v=cs.90)).

[!code-csharp[SqlClient_AsyncProgramming_ProviderModel#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_ProviderModel.cs#1)]

### <a name="use-sql-transactions-and-the-new-asynchronous-feature"></a>Usare le transazioni SQL e la nuova funzionalità asincrona

[!code-csharp[SqlClient_AsyncProgramming_SqlTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlTransaction.cs#1)]

### <a name="use-distributed-transactions-and-the-new-asynchronous-feature"></a>Usare le transazioni distribuite e la nuova funzionalità asincrona

In un'applicazione aziendale potrebbe essere necessario aggiungere **transazioni distribuite** in alcuni scenari, per abilitare le transazioni tra più server di database. È possibile usare lo spazio dei nomi System.Transactions e inserire una transazione distribuita, come segue:

[!code-csharp[SqlClient_AsyncProgramming_DistributedTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_DistributedTransaction.cs#1)]

### <a name="cancel-an-asynchronous-operation"></a>Annullare un'operazione asincrona

È possibile annullare una richiesta asincrona tramite <xref:System.Threading.CancellationToken>.

[!code-csharp[SqlClient_AsyncProgramming_Cancellation#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_Cancellation.cs#1)]

### <a name="asynchronous-operations-with-sqlbulkcopy"></a>Operazioni asincrone con SqlBulkCopy

Le funzionalità asincrone sono presenti anche in <xref:Microsoft.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType> con <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>.

[!code-csharp[SqlClient_AsyncProgramming_SqlBulkCopy#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlBulkCopy.cs#1)]

## <a name="asynchronously-use-multiple-commands-with-mars"></a>Usare in modo asincrono più comandi con MARS

Nell'esempio viene aperta una singola connessione al database di **AdventureWorks**. Usando un oggetto <xref:Microsoft.Data.SqlClient.SqlCommand>, viene creata una <xref:Microsoft.Data.SqlClient.SqlDataReader>. Quando il lettore è in uso viene aperto un secondo <xref:Microsoft.Data.SqlClient.SqlDataReader>, usando i dati del primo <xref:Microsoft.Data.SqlClient.SqlDataReader> come input per la clausola WHERE per il secondo lettore.

> [!NOTE]
> Nell'esempio seguente viene usato il database di esempio [**AdventureWorks**](../../samples/adventureworks-install-configure.md). Per la stringa di connessione specificata nel codice di esempio si presuppone che il database sia installato e disponibile nel computer locale. Modificare la stringa di connessione in base alle esigenze dell'ambiente in uso.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommands#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommands.cs#1)]

## <a name="asynchronously-read-and-update-data-with-mars"></a>Leggere e aggiornare in modo asincrono i dati con MARS

MARS consente di usare una connessione sia per le operazioni di lettura che per operazioni DML (Data Manipulation Language) con più di un'operazione in sospeso. Questa funzionalità consente a un'applicazione di evitare la gestione degli errori dovuti a una connessione occupata. Inoltre, MARS può sostituire l'uso dei cursori sul lato server, che in genere consumano più risorse. Infine, poiché su una singola connessione possono essere eseguite più operazioni, queste possono condividere lo stesso contesto di transazione, eliminando la necessità di usare le stored procedure di sistema **sp_getbindtoken** e **sp_bindsession**.

Nell'applicazione console seguente viene illustrato come usare due oggetti <xref:Microsoft.Data.SqlClient.SqlDataReader> con tre oggetti <xref:Microsoft.Data.SqlClient.SqlCommand> e un singolo oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> con MARS abilitato. Il primo oggetto comando recupera un elenco di fornitori la cui posizione creditizia è 5. Il secondo oggetto comando usa l'ID fornitore specificato da <xref:Microsoft.Data.SqlClient.SqlDataReader> per caricare il secondo <xref:Microsoft.Data.SqlClient.SqlDataReader> con tutti i prodotti per il fornitore specifico. Ogni record di prodotto viene visitato dal secondo <xref:Microsoft.Data.SqlClient.SqlDataReader>. Viene calcolato il nuovo valore di **OnOrderQty**. Il terzo oggetto comando viene usato per aggiornare la tabella **ProductVendor** con il nuovo valore. L'intero processo viene eseguito nell'ambito di una singola transazione, di cui alla fine viene eseguito il rollback.

> [!NOTE]
> Nell'esempio seguente viene usato il database di esempio [**AdventureWorks**](../../samples/adventureworks-install-configure.md). Per la stringa di connessione specificata nel codice di esempio si presuppone che il database sia installato e disponibile nel computer locale. Modificare la stringa di connessione in base alle esigenze dell'ambiente in uso.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommandsEx#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommandsEx.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
