---
title: Transazioni locali
description: Descrizione di come eseguire transazioni su un database con il provider di dati Microsoft SqlClient per SQL Server.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a310ab409c2300eb31d1e3c0e58b7ebe32096bfd
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051304"
---
# <a name="local-transactions"></a>Transazioni locali

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le transazioni in ADO.NET vengono usate quando si vogliono associare più attività in modo che vengano eseguite come una singola unità di lavoro. Ad esempio, si supponga che un'applicazione esegua due attività. Ovvero che prima aggiorni una tabella con le informazioni sull'ordine e che, successivamente, aggiorni una tabella contenente le informazioni d'inventario addebitando gli articoli ordinati. Se una delle attività non viene eseguita correttamente, verrà eseguito il rollback di entrambi gli aggiornamenti.  

## <a name="determining-the-transaction-type"></a>Determinazione del tipo di transazione

Una transazione è considerata locale quando è composta da una sola fase e viene gestita direttamente dal database. Una transazione è considerata distribuita quando viene coordinata da un monitoraggio delle transazioni e usa meccanismi fail-safe (quale il commit in due fasi) per la risoluzione.

Il provider di dati Microsoft SqlClient per SQL Server include un proprio oggetto <xref:Microsoft.Data.SqlClient.SqlTransaction> per l'esecuzione di transazioni locali nei database di SQL Server. Anche altri provider di dati .NET forniscono oggetti `Transaction` specifici. È anche disponibile una classe <xref:System.Data.Common.DbTransaction> che consente di scrivere codice indipendente dal provider che richiede transazioni.

> [!NOTE]
> Le transazioni sono più efficienti quando vengono eseguite nel server. Se si usa un database SQL Server in cui sono ampiamente usate le transazioni esplicite, è consigliabile scrivere queste transazioni come stored procedure usando l'istruzione BEGIN TRANSACTION Transact-SQL.

## <a name="performing-a-transaction-using-a-single-connection"></a>Esecuzione di una transazione usando una singola connessione 

In ADO.NET è possibile controllare le transazioni con l'oggetto `Connection`. È possibile avviare una transazione locale con il metodo `BeginTransaction`. Una volta iniziata una transazione, è possibile inserire un comando nell'elenco della transazione usando la proprietà `Transaction` di un oggetto `Command`. In seguito è possibile eseguire il commit o il rollback delle modifiche apportate nell'origine dati in base all'esito corretto o errato dei componenti della transazione.

> [!NOTE]
> Il metodo `EnlistDistributedTransaction` non deve essere usato per una transazione locale.

L'ambito della transazione si limita alla connessione. Nell'esempio seguente viene eseguita una transazione esplicita composta da due comandi separati nel blocco `try`. I comandi eseguono le istruzioni INSERT sulla tabella `Production.ScrapReason` nel database di esempio AdventureWorks di SQL Server e, se non vengono generate eccezioni, viene eseguito il commit. Il codice nel blocco `catch` esegue il rollback della transazione se viene generata un'eccezione. Allo stesso modo, se la transazione viene interrotta oppure la connessione viene chiusa prima del completamento della transazione, viene eseguito automaticamente il rollback della transazione.

## <a name="example"></a>Esempio  

 Per eseguire una transazione, usare la procedura seguente:

1. Chiamare il metodo <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> per contrassegnare l'inizio della transazione. Il metodo <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> restituisce un riferimento alla transazione. Questo riferimento viene assegnato agli oggetti <xref:Microsoft.Data.SqlClient.SqlCommand> contenuti nell'elenco della transazione.

2. Assegnare l'oggetto `Transaction` alla proprietà <xref:Microsoft.Data.SqlClient.SqlCommand.Transaction%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlCommand> da eseguire. Se un comando viene eseguito su una connessione con una transazione attiva e l'oggetto `Transaction` non è stato assegnato alla proprietà `Transaction` dell'oggetto `Command`, viene generata un'eccezione.

3. Eseguire i comandi richiesti.

4. Chiamare il metodo <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlTransaction> per completare la transazione oppure il metodo <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A> per interrompere la transazione. Se la connessione viene chiusa o eliminata prima che venga eseguito il metodo <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> o <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A>, viene eseguito il rollback della transazione.

Nell'esempio di codice riportato di seguito viene illustrata la logica transazionale usando il provider di dati Microsoft SqlClient per SQL Server.  

[!code-csharp[SqlTransactionLocal#1](~/../sqlclient/doc/samples/SqlTransactionLocal.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Transazioni e concorrenza](transactions-and-concurrency.md)
- [Transazioni distribuite](distributed-transactions.md)
- [Integrazione di System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
