---
title: Supporto dello streaming in SqlClient
description: Viene descritto come scrivere applicazioni che trasmettono i dati da SQL Server senza doverli caricare completamente in memoria.
ms.date: 12/04/2020
ms.assetid: c449365b-470b-4edb-9d61-8353149f5531
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c5ae05856f3f1e01d5831a6e80338f19e3994966
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744398"
---
# <a name="sqlclient-streaming-support"></a>Supporto dello streaming in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il supporto dello streaming tra SQL Server e un'altra applicazione supporta i dati non strutturati nel server (documenti, immagini e file multimediali). Un database SQL Server può archiviare oggetti binari di grandi dimensioni (BLOB), ma il recupero dei BLOB può impegnare una quantità consistente di memoria.

Il supporto del streaming da e verso SQL Server semplifica la creazione di applicazioni che trasmettono i dati, senza dover caricare completamente i dati in memoria, con conseguente riduzione del numero di eccezioni di overflow di memoria.

Il supporto dello streaming consentirà anche una migliore scalabilità delle applicazioni di livello intermedio, soprattutto in scenari in cui gli oggetti business si connettono ad Azure SQL per inviare, recuperare e gestire BLOB di grandi dimensioni.

> [!WARNING]
> I membri che supportano lo streaming vengono usati per recuperare i dati dalle query e per passare i parametri a query e stored procedure. La funzionalità di streaming è rivolta agli scenari di migrazione dei dati e OLTP di base ed è applicabile agli ambienti di migrazione dei dati locali e off-premise.

## <a name="streaming-support-from-sql-server"></a>Supporto dello streaming da SQL Server

Il supporto dello streaming da SQL Server introduce la nuova funzionalità nelle classi <xref:System.Data.Common.DbDataReader> e <xref:Microsoft.Data.SqlClient.SqlDataReader> per ottenere gli oggetti <xref:System.IO.Stream>, <xref:System.Xml.XmlReader> e <xref:System.IO.TextReader> e rispondere ad essi. Queste classi vengono usate per recuperare i dati dalle query. Di conseguenza, il supporto dello streaming da SQL Server è destinato agli scenari OLTP e si applica agli ambienti locali e off-premise.

I seguenti membri sono stati aggiunti a <xref:Microsoft.Data.SqlClient.SqlDataReader> per abilitare il supporto dello streaming da SQL Server:

- <xref:Microsoft.Data.SqlClient.SqlDataReader.IsDBNullAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValue%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValueAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetStream%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetTextReader%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetXmlReader%2A>

I seguenti membri sono stati aggiunti a <xref:System.Data.Common.DbDataReader> per abilitare il supporto dello streaming da SQL Server:

- <xref:System.Data.Common.DbDataReader.GetFieldValue%2A>

- <xref:System.Data.Common.DbDataReader.GetStream%2A>

- <xref:System.Data.Common.DbDataReader.GetTextReader%2A>

## <a name="streaming-support-to-sql-server"></a>Supporto dello streaming verso SQL Server

Il supporto del flusso verso SQL Server è disponibile nella classe <xref:Microsoft.Data.SqlClient.SqlParameter> cosicché può accettare e rispondere agli oggetti <xref:System.Xml.XmlReader>, <xref:System.IO.Stream> e <xref:System.IO.TextReader>. <xref:Microsoft.Data.SqlClient.SqlParameter> viene usato per passare i parametri a query e stored procedure.

> [!NOTE]
> L'eliminazione di un oggetto <xref:Microsoft.Data.SqlClient.SqlCommand> o la chiamata di <xref:Microsoft.Data.SqlClient.SqlCommand.Cancel%2A> deve annullare qualsiasi operazione di flusso. Se un'applicazione invia <xref:System.Threading.CancellationToken>, l'annullamento non è garantito.

I seguenti tipi <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> accetteranno <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> di <xref:System.IO.Stream>:

- **Binario**

- **VarBinary**

I seguenti tipi <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> accetteranno <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> di <xref:System.IO.TextReader>:

- **Char**

- **NChar**

- **NVarChar**

- **Xml**

Il tipo **Xml**<xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> accetterà <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> di <xref:System.Xml.XmlReader>.

<xref:Microsoft.Data.SqlClient.SqlParameter.SqlValue%2A> può accettare valori di tipo <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> e <xref:System.IO.Stream>.

Gli oggetti <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> e <xref:System.IO.Stream> verranno trasferiti fino al valore definito da <xref:Microsoft.Data.SqlClient.SqlParameter.Size%2A>.

## <a name="sample----streaming-from-sql-server"></a>Esempio: streaming da SQL Server

Usare il codice Transact-SQL seguente per creare il database di esempio:

```sql
CREATE DATABASE [Demo]
GO
USE [Demo]
GO
CREATE TABLE [Streams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX),
[bindata] VARBINARY(MAX),
[xmldata] XML)
GO
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'This is a test', 0x48656C6C6F, N'<test>value</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Hello, World!', 0x54657374696E67, N'<test>value2</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Another row', 0x666F6F626172, N'<fff>bbb</fff><fff>bbc</fff>')
GO
```

Nell'esempio vengono descritte le operazioni seguenti:

- Evitare di bloccare un thread di interfaccia utente fornendo una modalità asincrona per recuperare i file di grandi dimensioni.

- Trasferire un file di testo di grandi dimensioni da SQL Server in .NET.

- Trasferire un file con estensione XML di grandi dimensioni da SQL Server in .NET.

- Recuperare i dati da SQL Server.

- Trasferire file di grandi dimensioni (BLOB) da un database SQL Server a un altro senza esaurire la memoria.

[!code-csharp[SqlClient_Streaming_FromServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_FromServer.cs#1)]

## <a name="sample----streaming-to-sql-server"></a>Esempio: streaming verso SQL Server

Usare il codice Transact-SQL seguente per creare il database di esempio:

```sql
CREATE DATABASE [Demo2]
GO
USE [Demo2]
GO
CREATE TABLE [BinaryStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
CREATE TABLE [TextStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX))
GO
CREATE TABLE [BinaryStreamsCopy] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
```

Nell'esempio vengono descritte le operazioni seguenti:

- Trasferimento di un BLOB di grandi dimensioni verso SQL Server in .NET.

- Trasferimento di un file di testo di grandi dimensioni verso SQL Server in .NET.

- Utilizzo della nuova funzionalità asincrona per trasferire un BLOB di grandi dimensioni.

- Uso della nuova funzionalità asincrona e della parola chiave await per trasferire un BLOB di grandi dimensioni.

- Annullamento del trasferimento di un BLOB di grandi dimensioni.

- Streaming da un database SQL Server a un altro usando la nuova funzionalità asincrona.

[!code-csharp[SqlClient_Streaming_ToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ToServer.cs#1)]

## <a name="sample----streaming-from-one-sql-server-to-another-sql-server"></a>Esempio: streaming da un database SQL Server a un altro database SQL Server

Questo esempio illustra come trasmettere in modo asincrono un BLOB di grandi dimensioni da un database SQL Server a un altro, con il supporto per l'annullamento.

> [!NOTE]
> Prima di eseguire l'esempio seguente, assicurarsi di aver creato i database Demo e Demo2.

[!code-csharp[SqlClient_Streaming_ServerToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ServerToServer.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
