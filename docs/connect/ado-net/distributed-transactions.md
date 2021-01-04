---
title: Transazioni distribuite
description: Descrizione di come eseguire transazioni distribuite in ADO.NET
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 77ed8486f8b059f12fe7178826d81a743a97bbc4
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559233"
---
# <a name="distributed-transactions"></a>Transazioni distribuite

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una transazione consiste in una set di attività correlate che costituiscono una singola unità, la quale può riuscire (ovvero viene eseguito il commit della transazione) o non riuscire (il processo viene interrotto). Una *transazione distribuita* è una transazione con effetti su varie risorse. Per eseguire il commit di una transazione distribuita è necessario che tutti i partecipanti garantiscano che le eventuali modifiche apportate ai dati siano permanenti. Le modifiche devono essere definitive anche in caso di arresti anomali del sistema o altri eventi imprevisti. Se anche un unico partecipante non è in grado di garantire tale requisito, l'intera transazione verrà annullata e verrà effettuato il rollback di tutte le modifiche apportate ai dati nell'ambito della transazione. 

> [!NOTE]
> Se si tenta di eseguire il commit o il rollback di una transazione in cui è stato avviato un `DataReader` mentre la transazione è attiva, verrà generata un'eccezione.

## <a name="working-with-systemtransactions"></a>Uso di System.Transactions

In .NET le transazioni distribuite vengono gestite tramite l'API nello spazio dei nomi <xref:System.Transactions>. L'API <xref:System.Transactions> delega quindi la gestione della transazione a un monitoraggio delle transazioni, quale Microsoft Distributed Transaction Coordinator (MS DTC), quando sono interessati più gestori di risorse persistenti. Per altre informazioni, vedere [Nozioni fondamentali sulle transazioni](/dotnet/framework/data/transactions/transaction-fundamentals).

In ADO.NET 2.0 è stato introdotto il supporto per l'inserimento in una transazione distribuita usando il metodo `EnlistTransaction`, che inserisce una connessione in un'istanza di <xref:System.Transactions.Transaction>. Nelle versioni precedenti di ADO.NET l'inserimento esplicito nelle transazioni distribuite veniva eseguito tramite il metodo `EnlistDistributedTransaction` di una connessione per inserire una connessione in un'istanza di <xref:System.EnterpriseServices.ITransaction>, che è supportata per la compatibilità con le versioni precedenti. Per altre informazioni sulle transazioni Enterprise Services, vedere [Interoperabilità con transazioni COM+ ed Enterprise Services](/dotnet/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions).

Quando si usa una transazione <xref:System.Transactions> con il provider di dati Microsoft SqlClient per SQL Server su un database di SQL Server, verrà usato automaticamente un tipo semplice <xref:System.Transactions.Transaction>. La transazione può quindi essere promossa a una piena transazione distribuita in base alle esigenze. Per altre informazioni, vedere [Integrazione di System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="automatically-enlisting-in-a-distributed-transaction"></a>Integrazione automatica in transazioni distribuite

L'inserimento automatico è il metodo predefinito e preferenziale per integrare connessioni ADO.NET in `System.Transactions`. Un oggetto connessione viene automaticamente inserito in una transazione distribuita esistente se viene rilevato che la transazione è attiva. In termini di `System.Transaction`, questo significa che `Transaction.Current` non è Null. L'inserimento automatico in una transazione viene eseguito quando si apre la connessione. Successivamente, l'inserimento non viene più eseguito, neanche nel caso in cui venga eseguito un comando nell'ambito di una transazione. È possibile disabilitare l'integrazione automatica nelle transazioni esistenti specificando `Enlist=false` come parametro della stringa di connessione per <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>.

## <a name="manually-enlisting-in-a-distributed-transaction"></a>Integrazione manuale in una transazione distribuita

Se l'integrazione automatica è disabilitata o è necessario integrare una transazione avviata dopo l'apertura della connessione, è possibile eseguire l'integrazione in una transazione distribuita esistente usando il metodo `EnlistTransaction` dell'oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> per il provider di dati Microsoft SqlClient per SQL Server. L'inserimento in una transazione distribuita esistente assicura che se viene eseguito il commit o il rollback della transazione, verrà eseguito il commit o, rispettivamente, il rollback anche delle modifiche apportate dal codice all'origine dati.

L'inserimento in transazioni distribuite è particolarmente indicato quando si effettua il pool di oggetti business. Se si effettua il pool di un oggetto business con una connessione aperta, l'inserimento automatico nella transazione verrà eseguito solo quando quella connessione è aperta. Se vengono condotte più transazioni usando l'oggetto business di cui si effettua il pool, la connessione aperta per tale oggetto non verrà automaticamente inserita nelle nuove transazioni. In questo caso è possibile disabilitare l'inserimento automatico nelle transazioni per la connessione e inserire tale connessione nelle transazioni usando il metodo `EnlistTransaction`.

`EnlistTransaction` accetta un solo argomento di tipo <xref:System.Transactions.Transaction>, ovvero un riferimento alla transazione esistente. Dopo la chiamata al metodo `EnlistTransaction` della connessione, nella transazione sono incluse tutte le modifiche apportate all'origine dati che usa la connessione. Tramite il passaggio di un valore null, la connessione viene rimossa dalla transazione distribuita corrente. Si noti che la connessione deve essere aperta prima di chiamare il metodo `EnlistTransaction`.

> [!NOTE]
> Una volta che è stata inserita esplicitamente in una transazione, una connessione non può essere rimossa né inserita in una transazione diversa fino a quando non è stata completata la prima transazione.

> [!CAUTION]
> Il metodo `EnlistTransaction` genera un'eccezione se la connessione ha già iniziato una transazione usando il metodo <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> della connessione. Tuttavia, nel caso di una transazione locale avviata nell'origine dati (ad esempio eseguendo in modo esplicito l'istruzione BEGIN TRANSACTION tramite <xref:Microsoft.Data.SqlClient.SqlCommand>), la chiamata a `EnlistTransaction` comporterà il rollback della transazione locale e l'inserimento nella transazione distribuita esistente, come richiesto. Non verrà notificato che è stato eseguito il rollback della transazione locale e l'utente dovrà gestire le transazioni locali non avviate tramite il metodo <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>. Se si usa il provider di dati Microsoft SqlClient per SQL Server con SQL Server, un tentativo di integrazione genererà un'eccezione. Tutti gli altri casi non verranno rilevati.  

## <a name="promotable-transactions-in-sql-server"></a>Transazioni promuovibili in SQL Server

SQL Server supporta transazioni promuovibili in cui una transazione semplice locale può essere automaticamente promossa a transazione distribuita soltanto nel caso in cui ciò sia richiesto. Una transazione promuovibile non richiama l'overhead aggiunto di una transazione distribuita, a meno che non sia necessario. Per altre informazioni e un esempio di codice, vedere [Integrazione di System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="configuring-distributed-transactions"></a>Configurazione di transazioni distribuite

 Può essere necessario abilitare MS DTC sulla rete per usare le transazioni distribuite. Se Windows Firewall è abilitato, è necessario consentire al servizio MS DTC di usare la rete o aprire la porta MS DTC.  
  
## <a name="see-also"></a>Vedere anche

- [Transazioni e concorrenza](transactions-and-concurrency.md)
- [Integrazione di System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
