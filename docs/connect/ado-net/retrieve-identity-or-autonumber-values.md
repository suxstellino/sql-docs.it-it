---
title: Recuperare i valori Identity o di numerazione automatica
description: Informazioni su come recuperare i valori Identity e di numerazione automatica delle chiavi primarie in SQL Server e come unire i nuovi valori Identity in ADO.NET.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: d6b7f9cb-81be-44e1-bb94-56137954876d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4ca2199686359235ff1d2c834b0b19a88296030d
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744391"
---
# <a name="retrieve-identity-or-autonumber-values"></a>Recuperare i valori Identity o di numerazione automatica

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Per chiave primaria di un database relazionale si intende una colonna o una combinazione di colonne che contengono sempre valori univoci. Se si conosce il valore della chiave primaria, è possibile individuare la riga che lo contiene. Nei motori dei database relazionali, ad esempio SQL Server, Oracle e Microsoft Access/Jet, è supportata la creazione di colonne a incremento automatico che è possibile impostare come chiavi primarie. Tali valori vengono generati dal server quando si aggiungono righe a una tabella. In SQL Server viene impostata la proprietà Identity di una colonna, in Oracle viene creata una sequenza, mentre in Microsoft Access viene creata una colonna Contatore.

È inoltre possibile usare un oggetto <xref:System.Data.DataColumn> per generare valori a incremento automatico impostando la proprietà <xref:System.Data.DataColumn.AutoIncrement%2A> su true. Se, tuttavia, più applicazioni client generano valori a incremento automatico in modo indipendente l'una dall'altra, è possibile che in istanze distinte di un oggetto <xref:System.Data.DataTable> siano presenti valori duplicati. Se invece i valori a incremento automatico vengono generati dal server, è possibile eliminare possibili conflitti in quanto si consente a ogni utente di recuperare il valore generato per ciascuna riga inserita.

Durante una chiamata al metodo `Update` di un `DataAdapter`, il database può restituire dati all'applicazione ADO.NET sotto forma di parametri di output o di primo record restituito del set di risultati di un'istruzione SELECT eseguita nello stesso batch dell'istruzione INSERT. Il provider di dati Microsoft SqlClient per SQL Server può recuperare questi valori e aggiornare le colonne corrispondenti nell'oggetto <xref:System.Data.DataRow> da aggiornare.

> [!NOTE]
> In alternativa a un valore a incremento automatico, è possibile usare il metodo <xref:System.Guid.NewGuid%2A> di un oggetto <xref:System.Guid> per generare nel computer client un GUID che può essere copiato nel server a ogni nuova riga inserita. Il metodo `NewGuid` genera un valore binario a 16 byte per la cui creazione viene usato un algoritmo che consente con buona probabilità di evitare valori duplicati. In un database SQL Server un GUID viene archiviato in una colonna `uniqueidentifier` che SQL Server è in grado di generare automaticamente usando la funzione Transact-SQL `NEWID()`. L'utilizzo di un GUID come chiave primaria può influire negativamente sulle prestazioni. SQL Server include il supporto per la funzione `NEWSEQUENTIALID()` che genera un GUID sequenziale di cui non viene garantita l'univocità a livello globale ma che può essere indicizzato in modo più efficiente.

## <a name="retrieve-sql-server-identity-column-values"></a>Recuperare i valori della colonna Identity di SQL Server

Quando si usa Microsoft SQL Server, è possibile creare una stored procedure con un parametro di output in modo che venga restituito il valore della colonna Identity per una riga inserita. Nella tabella seguente sono descritte le tre funzioni Transact-SQL disponibili in SQL Server che è possibile usare per recuperare i valori delle colonne Identity.

|Funzione|Descrizione|
|--------------|-----------------|
|SCOPE_IDENTITY|Restituisce l'ultimo valore della colonna Identity compreso nell'ambito di esecuzione corrente. SCOPE_IDENTITY è consigliato per la maggior parte degli scenari.|
|@@IDENTITY|Contiene l'ultimo valore di colonna Identity generato in qualsiasi tabella nella sessione corrente. @@IDENTITY può essere influenzato dai trigger e può non restituire il valore di colonna Identity che ci si aspetta.|
|IDENT_CURRENT|Restituisce l'ultimo valore di colonna Identity generato per una tabella specifica in qualsiasi sessione e in qualsiasi ambito.|

Nella stored procedure seguente viene illustrato come inserire una riga nella tabella **Categories** e usare un parametro di output per restituire il nuovo valore Identity generato dalla funzione SCOPE_IDENTITY() di Transact-SQL.

```sql
CREATE PROCEDURE dbo.InsertCategory
  @CategoryName nvarchar(15),
  @Identity int OUT
AS
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)
SET @Identity = SCOPE_IDENTITY()
```

È quindi possibile specificare la stored procedure come origine di <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> in un oggetto <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. La proprietà <xref:Microsoft.Data.SqlClient.SqlCommand.CommandType%2A> di <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> deve essere impostata su <xref:System.Data.CommandType.StoredProcedure>. Per recuperare l'output relativo all'identità, viene creato un oggetto <xref:Microsoft.Data.SqlClient.SqlParameter> il cui <xref:System.Data.ParameterDirection> è <xref:System.Data.ParameterDirection.Output>. Quando viene eseguito `InsertCommand`, il valore Identity a incremento automatico viene restituito e inserito nella colonna **CategoryID** della riga corrente se si imposta la proprietà <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> del comando di inserimento su `UpdateRowSource.OutputParameters` o su `UpdateRowSource.Both`.

Se il comando di inserimento esegue un batch in cui sono incluse sia un'istruzione INSERT che un'istruzione SELECT che restituisce il nuovo valore Identity, è possibile recuperare il nuovo valore impostando la proprietà `UpdatedRowSource` del comando di inserimento su `UpdateRowSource.FirstReturnedRecord`.

[!code-csharp[DataWorks SqlClient.RetrieveIdentityStoredProcedure#1](~/../sqlclient/doc/samples/SqlDataAdapter_RetrieveIdentityStoredProcedure.cs#1)]

## <a name="merge-new-identity-values"></a>Unire nuovi valori Identity

In uno scenario comune viene chiamato il metodo `GetChanges` di un oggetto `DataTable` per creare una copia che contiene solo le righe modificate e viene usata la nuova copia durante la chiamata al metodo `Update` di un oggetto `DataAdapter`. Tale scenario risulta particolarmente utile quando è necessario effettuare il marshalling delle righe modificate in un componente diverso che esegue l'aggiornamento. In seguito all'aggiornamento, la copia può contenere nuovi valori Identity che devono essere reinseriti nell'oggetto `DataTable` originale. È probabile che i nuovi valori Identity siano diversi dai valori originali di `DataTable`. Per eseguire il merge, è necessario mantenere i valori originali delle colonne **AutoIncrement** nella copia per poter individuare e aggiornare le righe esistenti nell'oggetto `DataTable` originale, anziché accodare altre righe che contengono i nuovi valori Identity. Per impostazione predefinita, i valori originali vengono tuttavia persi dopo una chiamata al metodo `Update` di un oggetto `DataAdapter`, perché viene chiamato implicitamente `AcceptChanges` per ogni `DataRow` aggiornato.

Sono disponibili sono due tecniche per mantenere i valori originali di un oggetto `DataColumn` in un `DataRow` durante un aggiornamento di `DataAdapter`:

- La prima tecnica per mantenere i valori originali consiste nell'impostare la proprietà `AcceptChangesDuringUpdate` di `DataAdapter` su `false`. Questa configurazione ha effetto su ogni `DataRow` nel `DataTable` da aggiornare. Per altre informazioni ed esempi di codice, vedere <xref:System.Data.Common.DataAdapter.AcceptChangesDuringUpdate%2A>.

- La seconda tecnica consiste invece nello scrivere codice nel gestore eventi `RowUpdated` di `DataAdapter` per impostare <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> su <xref:System.Data.UpdateStatus.SkipCurrentRow>. `DataRow` viene aggiornato ma il valore originale di ogni `DataColumn` viene mantenuto. Questa tecnica consente di mantenere i valori originali per alcune righe ma non per altre. Ad esempio, il codice può mantenere i valori originali per le righe aggiunte e non per quelle modificate o eliminate controllando dapprima <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> e quindi impostando <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> su <xref:System.Data.UpdateStatus.SkipCurrentRow> solo per le righe in cui il valore di `StatementType` è `Insert`.

Quando uno di questi metodi viene usato per mantenere i valori originali in un `DataRow` durante un aggiornamento di `DataAdapter`, l'adattatore dati Microsoft SqlClient per SQL Server esegue una serie di azioni per impostare i valori correnti del `DataRow` sui nuovi valori restituiti dai parametri di output o dalla prima riga restituita di un set di risultati, mantenendo comunque il valore originale in ogni `DataColumn`. Viene innanzitutto chiamato il metodo `AcceptChanges` di `DataRow` per mantenere i valori correnti come valori originali, quindi vengono assegnati i nuovi valori. In seguito a queste azioni, è possibile che inaspettatamente la proprietà `DataRows` degli oggetti <xref:System.Data.DataRow.RowState%2A> la cui proprietà <xref:System.Data.DataRowState.Added> è stata impostata su `RowState` verrà impostata su <xref:System.Data.DataRowState.Modified>.

La modalità di applicazione dei risultati del comando a ogni <xref:System.Data.DataRow> da aggiornare dipende dalla proprietà <xref:System.Data.Common.DbCommand.UpdatedRowSource%2A> di ogni <xref:System.Data.Common.DbCommand>. Questa proprietà è impostata su un valore restituito dall'enumerazione `UpdateRowSource`.

Nella tabella seguente viene descritto il modo in cui i diversi valori dell'enumerazione `UpdateRowSource` influiscono sulla proprietà <xref:System.Data.DataRow.RowState%2A> delle righe aggiornate.

|Nome del membro|Descrizione|
|-----------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|Viene chiamato `AcceptChanges` e i valori dei parametri di output e/o i valori nella prima riga di qualsiasi set di risultati restituito vengono inseriti nell'oggetto `DataRow` da aggiornare. Se non sono disponibili valori da applicare, il valore di `RowState` sarà <xref:System.Data.DataRowState.Unchanged>.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Se è stata restituita una riga, viene chiamato `AcceptChanges` e viene eseguito il mapping della riga alla riga modificata in `DataTable`, impostando `RowState` su `Modified`. Se non viene restituita nessuna riga, `AcceptChanges` non viene chiamato e `RowState` rimane impostato su `Added`.|
|<xref:System.Data.UpdateRowSource.None>|Eventuali parametri o righe restituite vengono ignorati. Non viene eseguita nessuna chiamata a `AcceptChanges` e `RowState` rimane impostato su `Added`.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Viene chiamato `AcceptChanges` e viene eseguito il mapping degli eventuali parametri di output alla riga modificata in `DataTable`, impostando `RowState` su `Modified`. Se non sono disponibili parametri di output, il valore di `RowState` sarà `Unchanged`.|

### <a name="example"></a>Esempio

In questo esempio vengono illustrati l'estrazione di righe modificate da un oggetto `DataTable` e l'uso di <xref:Microsoft.Data.SqlClient.SqlDataAdapter> per aggiornare l'origine dati e recuperare un nuovo valore per la colonna Identity. <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> esegue due istruzioni Transact-SQL: la prima è l'istruzione INSERT, mentre la seconda è un'istruzione SELECT che usa la funzione SCOPE_IDENTITY per recuperare il valore Identity.

```sql
INSERT INTO dbo.Shippers (CompanyName)
VALUES (@CompanyName);
SELECT ShipperID, CompanyName FROM dbo.Shippers
WHERE ShipperID = SCOPE_IDENTITY();
```

La proprietà `UpdatedRowSource` del comando di inserimento viene impostata su `UpdateRowSource.FirstReturnedRow`, mentre la proprietà <xref:System.Data.MissingSchemaAction> di `DataAdapter` viene impostata su `MissingSchemaAction.AddWithKey`. `DataTable` viene riempito e il codice aggiunge una nuova riga a `DataTable`. Le righe modificate vengono quindi estratte in un nuovo oggetto `DataTable`, che viene passato a `DataAdapter` e usato per l'aggiornamento del server.

[!code-csharp[DataWorks SqlClient.MergeIdentity#1](~/../sqlclient/doc/samples/SqlDataAdapter_MergeIdentity.cs#1)]

Il gestore eventi `OnRowUpdated` verifica la proprietà <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> di <xref:Microsoft.Data.SqlClient.SqlRowUpdatedEventArgs> per determinare se la riga è un inserimento. In caso affermativo, la proprietà <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> viene impostata su <xref:System.Data.UpdateStatus.SkipCurrentRow>. La riga viene aggiornata, tuttavia i valori originali della riga vengono mantenuti. Nel corpo principale della procedura viene chiamato il metodo <xref:System.Data.DataSet.Merge%2A> per unire il nuovo valore Identity nell'oggetto `DataTable` originale e infine viene chiamato `AcceptChanges`.

[!code-csharp[DataWorks SqlClient.MergeIdentity#2](~/../sqlclient/doc/samples/SqlDataAdapter_MergeIdentity.cs#2)]

### <a name="retrieve-identity-values"></a>Recuperare i valori Identity

Spesso la colonna viene impostata come Identity quando i valori nella colonna devono essere univoci. E a volte è necessario il valore Identity di nuovi dati. Questo esempio illustra come recuperare i valori Identity:

- Crea una stored procedure per inserire dati e restituire un valore Identity.

- Esegue un comando per inserire i nuovi dati e visualizzare il risultato.

- Usa <xref:Microsoft.Data.SqlClient.SqlDataAdapter> per inserire i nuovi dati e visualizzare il risultato.

Prima di compilare ed eseguire l'esempio, è necessario creare il database di esempio, usando lo script seguente:

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
CREATE procedure [dbo].[CourseExtInfo] @CourseId int
as
select c.CourseID,c.Title,c.Credits,d.Name as DepartmentName
from Course as c left outer join Department as d on c.DepartmentID=d.DepartmentID
where c.CourseID=@CourseId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create procedure [dbo].[DepartmentInfo] @DepartmentId int,@CourseCount int output
as
select @CourseCount=Count(c.CourseID)
from course as c
where c.DepartmentID=@DepartmentId

select d.DepartmentID,d.Name,d.Budget,d.StartDate,d.Administrator
from Department as d
where d.DepartmentID=@DepartmentId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
Create PROCEDURE [dbo].[GetDepartmentsOfSpecifiedYear]
@Year int,@BudgetSum money output
AS
BEGIN
        SELECT @BudgetSum=SUM([Budget])
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

SELECT [DepartmentID]
      ,[Name]
      ,[Budget]
      ,[StartDate]
      ,[Administrator]
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[GradeOfStudent]
-- Add the parameters for the stored procedure here
@CourseTitle nvarchar(100),@FirstName nvarchar(50),
@LastName nvarchar(50),@Grade decimal(3,2) output
AS
BEGIN
select @Grade=Max(Grade)
from [dbo].[StudentGrade] as s join [dbo].[Course] as c on
s.CourseID=c.CourseID join [dbo].[Person] as p on s.StudentID=p.PersonID
where c.Title=@CourseTitle and p.FirstName=@FirstName
and p.LastName= @LastName
END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[InsertPerson]
-- Add the parameters for the stored procedure here
@FirstName nvarchar(50),@LastName nvarchar(50),
@PersonID int output
AS
BEGIN
    insert [dbo].[Person](LastName,FirstName) Values(@LastName,@FirstName)

    set @PersonID=SCOPE_IDENTITY()
END
Go

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

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[Person]([PersonID] [int] IDENTITY(1,1) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[HireDate] [datetime] NULL,
[EnrollmentDate] [datetime] NULL,
[Picture] [varbinary](max) NULL,
 CONSTRAINT [PK_School.Student] PRIMARY KEY CLUSTERED
(
[PersonID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[StudentGrade]([EnrollmentID] [int] IDENTITY(1,1) NOT NULL,
[CourseID] [nvarchar](10) NOT NULL,
[StudentID] [int] NOT NULL,
[Grade] [decimal](3, 2) NOT NULL,
 CONSTRAINT [PK_StudentGrade] PRIMARY KEY CLUSTERED
(
[EnrollmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create view [dbo].[EnglishCourse]
as
select c.CourseID,c.Title,c.Credits,c.DepartmentID
from Course as c join Department as d on c.DepartmentID=d.DepartmentID
where d.Name=N'English'

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
SET IDENTITY_INSERT [dbo].[Person] ON

INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (1, N'Hu', N'Nan', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (2, N'Norman', N'Laura', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (3, N'Olivotto', N'Nino', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (4, N'Anand', N'Arturo', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (5, N'Jai', N'Damien', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (6, N'Holt', N'Roger', CAST(0x000097F100000000 AS DateTime), NULL)
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (7, N'Martin', N'Randall', CAST(0x00008B1A00000000 AS DateTime), NULL)
SET IDENTITY_INSERT [dbo].[Person] OFF
SET IDENTITY_INSERT [dbo].[StudentGrade] ON

INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (1, N'C1045', 1, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (2, N'C1045', 2, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (3, N'C1045', 3, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (4, N'C1045', 4, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (5, N'C1045', 5, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (6, N'C1061', 1, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (7, N'C1061', 3, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (8, N'C1061', 4, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (9, N'C1061', 5, CAST(1.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (10, N'C2021', 1, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (11, N'C2021', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (12, N'C2021', 4, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (13, N'C2021', 5, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (14, N'C2042', 1, CAST(2.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (15, N'C2042', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (16, N'C2042', 3, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (17, N'C2042', 5, CAST(3.00 AS Decimal(3, 2)))
SET IDENTITY_INSERT [dbo].[StudentGrade] OFF
ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
ALTER TABLE [dbo].[StudentGrade]  WITH CHECK ADD  CONSTRAINT [FK_StudentGrade_Student] FOREIGN KEY([StudentID])
REFERENCES [dbo].[Person] ([PersonID])
GO
ALTER TABLE [dbo].[StudentGrade] CHECK CONSTRAINT [FK_StudentGrade_Student]
GO
```

Di seguito è riportato il listato di codice:

[!code-csharp[SqlClient_RetrieveIdentity#1](~/../sqlclient/doc/samples/SqlClient_RetrieveIdentity.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](retrieving-modifying-data.md)
- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Aggiornare origini dati con DataAdapter](update-data-sources-with-dataadapters.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
