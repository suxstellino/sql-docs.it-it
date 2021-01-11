---
title: Configurazione dei parametri
description: Gli oggetti comando usano i parametri per passare valori a istruzioni o stored procedure SQL, offrendo la verifica e la convalida dei tipi in ADO.NET.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 14442892bc59fc733c6a67153832b95c3f368b95
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771458"
---
# <a name="configuring-parameters"></a>Configurazione dei parametri

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Gli oggetti comando usano i parametri per passare valori a istruzioni o stored procedure SQL, fornendo la verifica e la convalida dei tipi. A differenza del testo dei comandi, l'input dei parametri viene trattato come valore letterale, non come codice eseguibile. In questo modo è possibile difendersi da attacchi SQL injection, in cui l'autore di un attacco inserisce un comando che compromette la sicurezza del server in un'istruzione SQL.

I comandi con parametri possono anche migliorare le prestazioni di esecuzione delle query in quanto aiutano il server database a ottenere una corrispondenza accurata tra il comando in arrivo e un piano di query memorizzato nella cache appropriato. Per altre informazioni, vedere [Memorizzazione nella cache e riutilizzo del piano di esecuzione](/sql/relational-databases/query-processing-architecture-guide#execution-plan-caching-and-reuse) e [Parametri e riutilizzo del piano di esecuzione](/sql/relational-databases/query-processing-architecture-guide#PlanReuse). Oltre ai vantaggi in termini di sicurezza e prestazioni, i comandi con parametri offrono un metodo pratico per organizzare i valori passati a un'origine dati.

Per creare un oggetto <xref:System.Data.Common.DbParameter> , è possibile usare il relativo costruttore o aggiungerlo all'oggetto <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> chiamando il metodo `Add` della raccolta <xref:System.Data.Common.DbParameterCollection> . Il metodo `Add` accetta come input argomenti del costruttore o un oggetto parametro esistente, a seconda del provider di dati.

## <a name="supply-the-parameterdirection-property"></a>Specificare la proprietà ParameterDirection

Quando si aggiungono parametri, è necessario fornire una proprietà <xref:System.Data.ParameterDirection> per i parametri diversi da quelli di input. La tabella seguente illustra i valori `ParameterDirection` che è possibile usare con l'enumerazione <xref:System.Data.ParameterDirection> .

|Nome del membro|Descrizione|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|Il parametro è un parametro di input. Questo è il valore predefinito.|
|<xref:System.Data.ParameterDirection.InputOutput>|Il parametro può essere sia di input che di output.|
|<xref:System.Data.ParameterDirection.Output>|Il parametro è un parametro di output.|
|<xref:System.Data.ParameterDirection.ReturnValue>|Il parametro rappresenta un valore restituito da un'operazione quale una stored procedure, una funzione predefinita o una funzione definita dall'utente.|

## <a name="work-with-parameter-placeholders"></a>Usare i segnaposto dei parametri

La sintassi per i segnaposto dei parametri varia in base all'origine dati. Il provider di dati Microsoft SqlClient per SQL Server gestisce la denominazione e la specifica dei parametri e dei segnaposto dei parametri in modo diverso. Il provider di dati usa parametri denominati nel formato `@`*parametername*.

## <a name="specify-parameter-data-types"></a>Specificare i tipi di dati per i parametri

Il tipo di dati di un parametro è specifico per il provider di dati Microsoft SqlClient per SQL Server. Se si specifica il tipo, il valore di `Parameter` viene convertito nel tipo per il provider di dati Microsoft SqlClient per SQL Server prima che sia passato all'origine dati. È inoltre possibile specificare il tipo di un oggetto `Parameter` in modo generico impostando la proprietà `DbType` dell'oggetto `Parameter` su un determinato oggetto <xref:System.Data.DbType>.

Il tipo per il provider di dati Microsoft SqlClient per SQL Server di un oggetto `Value` viene dedotto dal tipo .NET Framework di `Parameter` dell'oggetto `Parameter` oppure da `DbType` dell'oggetto `Parameter`. La tabella seguente illustra il tipo `Parameter` dedotto in base all'oggetto passato come valore di `Parameter` o all'oggetto `DbType`specificato.

|Tipo .NET|DbType|SqlDbType|
|-------------------------|------------|---------------|
|<xref:System.Boolean>|Boolean|bit|
|<xref:System.Byte>|Byte|TinyInt|
|byte[]|Binary|VarBinary. La conversione implicita non riesce se le dimensioni della matrice di byte sono maggiori delle dimensioni di VarBinary, che è di 8000 byte. Per le matrici di byte maggiori di 8000 byte, impostare in modo esplicito <xref:System.Data.SqlDbType>.|
|<xref:System.Char>| |L'inferenza di un oggetto <xref:System.Data.SqlDbType> da char non è supportata.|
|<xref:System.DateTime>|Datetime|Datetime|
|<xref:System.DateTimeOffset>|DateTimeOffset|DateTimeOffset in SQL Server 2008. L'inferenza di un oggetto <xref:System.Data.SqlDbType> da DateTimeOffset non è supportata nelle versioni di SQL Server precedenti a SQL Server 2008.|
|<xref:System.Decimal>|Decimal|Decimal|
|<xref:System.Double>|Double|Float|
|<xref:System.Single>|Single|Real|
|<xref:System.Guid>|Guid|UniqueIdentifier|
|<xref:System.Int16>|Int16|SmallInt|
|<xref:System.Int32>|Int32|Int|
|<xref:System.Int64>|Int64|BigInt|
|<xref:System.Object>|Oggetto|Variant|
|<xref:System.String>|string|NVarChar. La conversione implicita non riesce se la stringa ha una dimensione superiore a quella massima di NVarChar, che è di 4000 caratteri. Per le stringhe maggiori di 4000 caratteri, impostare in modo esplicito <xref:System.Data.SqlDbType>.|
|<xref:System.TimeSpan>|Tempo|Time in SQL Server 2008. L'inferenza di un oggetto <xref:System.Data.SqlDbType> da TimeSpan non è supportata nelle versioni di SQL Server precedenti a SQL Server 2008.|
|<xref:System.UInt16>|UInt16|L'inferenza di un oggetto <xref:System.Data.SqlDbType> da UInt16 non è supportata.|
|<xref:System.UInt32>|UInt32|L'inferenza di un oggetto <xref:System.Data.SqlDbType> da UInt32 non è supportata.|
|<xref:System.UInt64>|UInt64|L'inferenza di un oggetto <xref:System.Data.SqlDbType> da UInt64 non è supportata.|
||AnsiString|VarChar|
||AnsiStringFixedLength|Char|
||Valuta|Money|
||Data|Date in SQL Server 2008. L'inferenza di un oggetto <xref:System.Data.SqlDbType> da Date non è supportata nelle versioni di SQL Server precedenti a SQL Server 2008.|
||SByte|L'inferenza di un oggetto <xref:System.Data.SqlDbType> da SByte non è supportata.|
||StringFixedLength|NChar|
||Tempo|Time in SQL Server 2008. L'inferenza di un oggetto <xref:System.Data.SqlDbType> da Time non è supportata nelle versioni di SQL Server precedenti a SQL Server 2008.|
||VarNumeric|L'inferenza di un oggetto <xref:System.Data.SqlDbType> da VarNumeric non è supportata.|
|Tipo di oggetto definito dall'utente (oggetto con <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>|SqlClient restituisce sempre un oggetto|`SqlDbType.Udt` se <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute> è presente. In caso contrario, `Variant`|

> [!NOTE]
> Le conversioni da Decimal in altri tipi sono conversioni di restrizione che arrotondano il valore Decimal al valore integer più vicino che tende allo zero. Se non è possibile rappresentare il risultato della conversione nel tipo di destinazione, verrà generata un'eccezione <xref:System.OverflowException> .

> [!NOTE]
> Quando si invia un valore di parametro Null al server, è necessario specificare <xref:System.DBNull> anziché `null` (`Nothing` in Visual Basic). Il valore null nel sistema è un oggetto vuoto senza un valore. <xref:System.DBNull> viene usato per rappresentare i valori null.

## <a name="derive-parameter-information"></a>Derivare le informazioni sui parametri

I parametri possono anche essere derivati da una stored procedure usando la classe `DbCommandBuilder` . La classe `SqlCommandBuilder` offre un metodo statico, `DeriveParameters`, che popola automaticamente la raccolta di parametri di un oggetto comando che usa le informazioni sui parametri di una stored procedure. Si noti che `DeriveParameters` sovrascrive qualsiasi informazione esistente sui parametri per il comando.

> [!NOTE]
> La derivazione di informazioni sui parametri implica una riduzione delle prestazioni, in quando richiede un round trip aggiuntivo con l'origine dati per recuperare le informazioni. Se le informazioni sui parametri sono note in fase di progettazione, è possibile migliorare le prestazioni dell'applicazione impostando i parametri in modo esplicito.

Per altre informazioni, vedere [Generazione dei comandi con CommandBuilders](generate-commands-with-commandbuilders.md).

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>Uso di parametri con SqlCommand e di una stored procedure

Le stored procedure offrono numerosi vantaggi nelle applicazioni guidate dai dati. Usando le stored procedure, le operazioni nel database possono essere incapsulate in un unico comando, ottimizzate per migliorare le prestazioni e rese più sicure con funzioni di sicurezza aggiuntive. Anche se è possibile chiamare una stored procedure passandone il nome seguito dagli argomenti dei parametri come istruzione SQL, l'uso della raccolta <xref:System.Data.Common.DbCommand.Parameters%2A> dell'oggetto <xref:System.Data.Common.DbCommand> di ADO.NET consente di definire in modo più esplicito i parametri delle stored procedure e di accedere ai parametri di output e ai valori restituiti.

> [!NOTE]
> Le istruzioni con parametri vengono eseguite sul server tramite `sp_executesql,` , che consente il riutilizzo del piano di query. I cursori o le variabili locali del batch `sp_executesql` non sono visibili per il batch che chiama `sp_executesql`. Le modifiche apportate al contesto del database durano solo fino al termine dell'esecuzione dell'istruzione `sp_executesql` . Per altre informazioni, vedere [sp_executesql (Transact-SQL)](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql).

Quando si usano parametri con <xref:Microsoft.Data.SqlClient.SqlCommand> per eseguire una stored procedure di SQL Server, i nomi dei parametri aggiunti alla raccolta <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> devono corrispondere ai nomi dei marcatori dei parametri nella stored procedure. Il provider di dati Microsoft SqlClient per SQL Server non supporta il segnaposto punto interrogativo (?) per il passaggio dei parametri a un'istruzione SQL o a una stored procedure. I parametri nella stored procedure vengono trattati come parametri denominati e vengono ricercati marcatori di parametri corrispondenti. Ad esempio, la stored procedure `CustOrderHist` è definita con un parametro denominato `@CustomerID`. Quando il codice esegue la stored procedure deve usare anche un parametro denominato `@CustomerID`.

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>Esempio

In questo esempio viene illustrato come chiamare una stored procedure di SQL Server nel database di esempio `Northwind` . Il nome della stored procedure è `dbo.SalesByCategory` e accetta un parametro di input denominato `@CategoryName` con un tipo di dati `nvarchar(15)`. Nel codice viene creato un nuovo oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> all'interno di un blocco using, in modo che la connessione venga eliminata al termine della procedura. Vengono creati gli oggetti <xref:Microsoft.Data.SqlClient.SqlCommand> e <xref:Microsoft.Data.SqlClient.SqlParameter> e vengono impostate le relative proprietà. Un oggetto <xref:Microsoft.Data.SqlClient.SqlDataReader> esegue `SqlCommand` e restituisce il set di risultati dalla stored procedure, visualizzando l'output nella finestra della console.

> [!NOTE]
> Anziché creare oggetti `SqlCommand` e `SqlParameter` e impostare quindi le proprietà in istruzioni distinte, è possibile scegliere di usare uno dei costruttori di overload per impostare più proprietà in una singola istruzione.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Comandi e parametri](commands-parameters.md)
- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
