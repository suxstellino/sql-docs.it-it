---
title: Aggiornare origini dati con DataAdapter
description: Informazioni su come il metodo Update di DataAdapter risolve le modifiche da DataSet all'origine dati nelle applicazioni ADO.NET.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6f2feb876d0f232f4d7951de8ee1cc84587e6486
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771361"
---
# <a name="update-data-sources-with-dataadapters"></a>Aggiornare origini dati con DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il metodo `Update` di <xref:System.Data.Common.DataAdapter> viene chiamato per applicare le modifiche apportate a un oggetto <xref:System.Data.DataSet> nell'origine dati. Il metodo `Update`, analogamente al metodo `Fill`, accetta come argomenti un'istanza di un oggetto `DataSet` e un oggetto <xref:System.Data.DataTable> o nome di `DataTable` facoltativi. L'istanza di `DataSet` rappresenta l'oggetto `DataSet` contenente le modifiche che sono state apportate e l'oggetto `DataTable` identifica la tabella da cui recuperare le modifiche. Se non viene specificato nessun oggetto `DataTable`, verrà usato il primo oggetto `DataTable` di `DataSet`.

Quando si chiama il metodo `Update`, `DataAdapter` analizza le modifiche apportate ed esegue il comando appropriato (INSERT, UPDATE o DELETE). Quando rileva una modifica a un oggetto `DataAdapter`, <xref:System.Data.DataRow> elabora la modifica usando <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> o <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A>.

Queste proprietà consentono di ottimizzare le prestazioni dell'applicazione ADO.NET specificando la sintassi del comando in fase di progettazione e, se possibile, tramite l'uso di stored procedure. È necessario impostare in modo esplicito i comandi prima di chiamare `Update`. Se `Update` viene chiamato e non esiste il comando appropriato per un determinato aggiornamento, ad esempio nessun `DeleteCommand` per le righe cancellate, viene generata un'eccezione.

> [!IMPORTANT]
> Se si usano le stored procedure di SQL Server per modificare o eliminare i dati usando una classe `DataAdapter`, assicurarsi di non usare `SET NOCOUNT ON` nella definizione della stored procedure. Con tale comando il totale restituito delle righe interessate è pari a zero e tale situazione viene interpretata da `DataAdapter` come un conflitto di concorrenza. In questo caso verrà generata un'eccezione <xref:System.Data.DBConcurrencyException>.

È possibile usare i parametri Command per specificare i valori di input e di output di un'istruzione SQL o una stored procedure per ogni riga modificata in un `DataSet`. Per altre informazioni, vedere [Parametri per DataAdapter](dataadapter-parameters.md).

> [!NOTE]
> È importante capire la differenza tra eliminazione di una riga in un oggetto <xref:System.Data.DataTable> e rimozione della riga. Quando si chiama il metodo `Remove` o `RemoveAt`, la riga viene rimossa immediatamente. Eventuali righe corrispondenti nell'origine dati back-end **non saranno interessate** se si passa `DataTable` o `DataSet` a `DataAdapter` e si chiama `Update`. Quando si usa il metodo `Delete`, la riga rimane in `DataTable` e viene contrassegnato per l'eliminazione. Se quindi si passa `DataTable` o `DataSet` a `DataAdapter` e si chiama `Update`, la riga corrispondente nell'origine dati back-end **viene eliminata**.

Se `DataTable` esegue il mapping o viene generato da una singola tabella di database, è possibile usare l'oggetto <xref:System.Data.Common.DbCommandBuilder> per generare automaticamente gli oggetti `DeleteCommand`, `InsertCommand` e `UpdateCommand` di `DataAdapter`. Per altre informazioni, vedere [Generazione dei comandi con CommandBuilders](generate-commands-with-commandbuilders.md).

## <a name="use-updatedrowsource-to-map-values-to-a-dataset"></a>Usare UpdatedRowSource per eseguire il mapping dei valori a DataSet

È possibile controllare come viene eseguito il mapping dei valori restituiti dall'origine dati a `DataTable` in seguito a una chiamata al metodo <xref:System.Data.Common.DbDataAdapter.Update%2A> di `DataAdapter`, usando la proprietà <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> di un oggetto <xref:Microsoft.Data.SqlClient.SqlCommand>. Impostando la proprietà `UpdatedRowSource` su uno dei valori di enumerazione <xref:System.Data.UpdateRowSource>, è possibile controllare se i parametri restituiti dai comandi di `DataAdapter` vengono ignorati o applicati alla riga modificata in `DataSet`. È possibile inoltre specificare se la prima riga restituita (se esiste) viene applicata alla riga modificata in `DataTable`.

La tabella seguente descrive i diversi valori dell'enumerazione `UpdateRowSource` e viene illustrato il modo in cui influenzano il comportamento di un comando usato con un `DataAdapter`.

|Enumerazione UpdatedRowSource|Descrizione|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|È possibile eseguire il mapping dei parametri di output e della prima riga di un set di risultati restituiti sulla riga modificata nel `DataSet`.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|È possibile eseguire il mapping sulla riga modificata nel `DataSet` solo dei dati nella prima riga di un set di risultati restituiti.|
|<xref:System.Data.UpdateRowSource.None>|Tutti i parametri di output, o righe, di un set di risultati restituiti vengono ignorati.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|È possibile eseguire il mapping sulla riga modificata nel `DataSet` solo dei parametri di output.|

Il metodo `Update` applica le modifiche nell'origine dati, ma è possibile che altri client abbiano modificato i dati nell'origine dati dall'ultima volta che è stato compilato il `DataSet`. Per aggiornare `DataSet` con i dati correnti, usare i metodi `DataAdapter` e `Fill`. Le nuove righe verranno aggiunte alla tabella e le informazioni aggiornate verranno inserite nelle righe esistenti.

Quando si esegue il metodo `Fill`, verrà determinato se aggiungere una nuova riga o aggiornare una riga esistente esaminando i valori delle chiavi primarie delle righe del `DataSet` e delle righe restituite da `SelectCommand`. Se il metodo `Fill` rileva un valore di chiave primaria di una riga del `DataSet` che corrisponde al valore di chiave primaria di una riga presente nei risultati restituiti da `SelectCommand`, la riga esistente verrà aggiornata con le informazioni presenti nella riga restituita da `SelectCommand` e la proprietà <xref:System.Data.DataRow.RowState%2A> della riga esistente verrà impostata su `Unchanged`. Se una riga restituita da `SelectCommand` presenta un valore di chiave primaria che non corrisponde ad alcuno dei valori di chiave primaria delle righe del `DataSet`, il metodo `Fill` aggiungerà una nuova riga con il valore relativo a `RowState` impostato su `Unchanged`.

> [!NOTE]
> Se `SelectCommand` restituisce i risultati di **OUTER JOIN**, `DataAdapter` non imposterà un valore `PrimaryKey` per la classe `DataTable` risultante. Per assicurarsi che le righe duplicate vengano risolte correttamente, sarà necessario definire `PrimaryKey` in modo autonomo.

Per gestire le eccezioni che possono verificarsi quando si chiama il metodo `Update`, è possibile usare l'evento `RowUpdated` per rispondere agli errori di aggiornamento delle righe nel momento in cui si verificano (vedere [Gestire eventi DataAdapter](handle-dataadapter-events.md)) oppure impostare <xref:System.Data.Common.DataAdapter.ContinueUpdateOnError%2A> su `true` prima di chiamare il metodo `Update` e rispondere alle informazioni sugli errori archiviate nella proprietà `RowError` di una determinata riga al termine dell'aggiornamento.

> [!NOTE]
> Se si chiama `AcceptChanges` sull'oggetto `DataSet`, `DataTable` o `DataRow`, tutti i valori `Original` relativi a un oggetto `DataRow` verranno sovrascritti con i valori `Current` relativi a `DataRow`. Se i valori di campo che identificano la riga come univoca sono stati modificati, dopo la chiamata a `AcceptChanges` i valori `Original` non corrisponderanno più ai valori dell'origine dati. `AcceptChanges` viene chiamato automaticamente per ogni riga durante una chiamata al metodo `Update` di `DataAdapter`. Per mantenere i valori originali durante una chiamata al metodo Update, impostare prima la proprietà `AcceptChangesDuringUpdate` di `DataAdapter` su false oppure creare un gestore eventi per l'evento `RowUpdated` e impostare <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> su <xref:System.Data.UpdateStatus.SkipCurrentRow>. Per altre informazioni, vedere [Gestire eventi DataAdapter](handle-dataadapter-events.md).

Gli esempi seguenti illustrano come aggiornare le righe modificate impostando in modo esplicito `UpdateCommand` di `DataAdapter` e chiamando il relativo metodo `Update`.

> [!NOTE]
> Il parametro specificato in `WHERE clause` di `UPDATE statement` è impostato in modo da usare il valore `Original` di `SourceColumn`. Questo è importante in quanto è possibile che il valore `Current` sia stato modificato e che non corrisponda più al valore nell'origine dati. Il valore `Original` è il valore che è stato usato per compilare `DataTable` dall'origine dati.

[!code-csharp[SqlDataAdapter_Update#1](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#1)]

## <a name="autoincrement-columns"></a>colonne AutoIncrement

Se nelle tabelle dell'origine dati sono presenti colonne con incremento automatico, è possibile riempire le colonne nel `DataSet` restituendo il valore dell'incremento automatico come un parametro di output di una stored procedure ed eseguendone il mapping su una colonna della tabella, restituendo il valore dell'incremento automatico nella prima riga di un set di risultati restituito da una stored procedure o un'istruzione SQL oppure usando l'evento `RowUpdated` di `DataAdapter` per eseguire un'ulteriore istruzione SELECT. Per altre informazioni e un esempio, vedere [Recuperare i valori Identity o di numerazione automatica](retrieve-identity-or-autonumber-values.md).

## <a name="ordering-of-inserts-updates-and-deletes"></a>Ordinamento di inserimenti, aggiornamenti ed eliminazioni

In molti casi l'ordine in cui le modifiche apportate mediante `DataSet` vengono inviate all'origine dati è importante. Se ad esempio il valore di una chiave primaria relativo a una riga esistente viene aggiornato ed è stata aggiunta una nuova riga con il nuovo valore della chiave primaria come chiave esterna, è importante elaborare l'aggiornamento prima di effettuare l'inserimento.

È possibile usare il metodo `Select` di `DataTable` per restituire una matrice `DataRow` che faccia riferimento solo a righe con un particolare `RowState`. È quindi possibile passare la matrice `DataRow` restituita al metodo `Update` di `DataAdapter` per elaborare le righe modificate. Se si specifica un subset di righe da aggiornare, è possibile controllare l'ordine in cui vengono elaborati gli inserimenti, gli aggiornamenti e le eliminazioni.

## <a name="example"></a>Esempio

Il codice seguente, ad esempio, assicura che vengano elaborate prima le righe cancellate della tabella, quindi le righe aggiornate e infine le righe inserite.

[!code-csharp[SqlDataAdapter_Update#2](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#2)]

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>Usare DataAdapter per recuperare e aggiornare i dati

È possibile usare un oggetto DataAdapter per recuperare e aggiornare i dati.

- L'esempio usa `DataAdapter.AcceptChangesDuringFill` per clonare i dati nel database. Se la proprietà è impostata su **false**, **AcceptChanges** non viene chiamato quando si compila la tabella e le righe appena aggiunte vengono considerate come righe inserite. Di conseguenza, nell'esempio queste righe vengono usate per inserire nuove righe nel database.

- Gli esempi usano `DataAdapter.TableMappings` per definire il mapping tra la tabella di origine e DataTable.

- L'esempio usa `DataAdapter.FillLoadOption` per determinare come l'adapter riempie **DataTable** da **DbDataReader**. Quando si crea un oggetto DataTable, è possibile scrivere i dati solo dal database alla versione corrente o alla versione originale impostando la proprietà su **LoadOption.Upsert** o **LoadOption.PreserveChanges**.

- L'esempio aggiornerà anche la tabella usando `DbDataAdapter.UpdateBatchSize` per eseguire operazioni batch.

Prima di compilare ed eseguire l'esempio, è necessario creare il database di esempio:

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

[!code-csharp[SqlDataAdapter_Properties#1](~/../sqlclient/doc/samples/SqlDataAdapter_Properties.cs#1)]

## <a name="see-also"></a>Vedere anche

- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Recuperare i valori Identity o di numerazione automatica](retrieve-identity-or-autonumber-values.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
