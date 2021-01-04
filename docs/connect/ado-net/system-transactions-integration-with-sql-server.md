---
title: Integrazione di System.Transactions con SQL Server
description: Descrizione dell'integrazione di System.Transactions con SQL Server per l'utilizzo di transazioni distribuite.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: af0ba2865719a5388314a4ca695e09191cb56173
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051316"
---
# <a name="systemtransactions-integration-with-sql-server"></a>Integrazione di System.Transactions con SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET include un nuovo framework per le transazioni, accessibile tramite lo spazio dei nomi <xref:System.Transactions>. Questo framework espone le transazioni in modo completamente integrato in .NET, incluso ADO.NET.  
  
Oltre ai miglioramenti apportati alla programmabilità, <xref:System.Transactions> e ADO.NET possono interagire insieme per coordinare le ottimizzazioni quando si utilizzano le transazioni. Una transazione promuovibile è una transazione di tipo semplice (locale) che può essere promossa in modo automatico a una transazione completamente distribuita in base alle esigenze.

Il provider di dati Microsoft SqlClient per SQL Server supporta le transazioni promuovibili quando si lavora con SQL Server. Una transazione promuovibile non richiama l'overhead aggiunto di una transazione distribuita, a meno che non sia necessario. Le transazioni promuovibili sono automatiche e non richiedono alcun intervento da parte dello sviluppatore.

## <a name="creating-promotable-transactions"></a>Creazione di transazioni promuovibili

Il provider di dati Microsoft SqlClient per SQL Server supporta le transazioni promuovibili, gestite tramite le classi dello spazio dei nomi <xref:System.Transactions>. Le transazioni promuovibili consentono di ottimizzare le transazioni distribuite rinviandone la creazione finché non è richiesta. Se è necessario un solo gestore di risorse, non si verificherà alcuna transazione distribuita.

> [!NOTE]
> In un scenario ad attendibilità parziale è richiesto <xref:System.Transactions.DistributedTransactionPermission> quando una transazione viene promossa a transazione distribuita.

## <a name="promotable-transaction-scenarios"></a>Scenari di transazioni promuovibili

In genere, le transazioni distribuite richiedono un notevole utilizzo delle risorse, in quanto sono gestite da Microsoft Distributed Transaction Coordinator (MS DTC) che integra tutti i gestori delle risorse a cui si accede nella transazione. Una transazione promuovibile rappresenta una forma speciale di transazione <xref:System.Transactions> che delega in modo efficiente il lavoro a una semplice transazione di SQL Server. <xref:System.Transactions>, <xref:Microsoft.Data.SqlClient> e SQL Server coordinano il lavoro di gestione della transazione, promuovendola a transazione completamente distribuita, se necessario.

Il vantaggio derivante dall'utilizzo delle transazioni promuovibili consiste nel fatto che quando si apre una connessione tramite un transazione <xref:System.Transactions.TransactionScope> attiva e non sono presenti altre transazioni aperte, viene eseguito il commit della transazione come tipo semplice, anziché provocare l'overhead aggiuntivo di una piena transazione distribuita.

### <a name="connection-string-keywords"></a>Parole chiave per le stringhe di connessione

La proprietà <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> supporta la parola chiave `Enlist`, che indica se <xref:Microsoft.Data.SqlClient> rileverà i contesti transazionali ed elenca automaticamente la connessione in una transazione distribuita. Se `Enlist=true`, la connessione verrà automaticamente elencata nel contesto di transazione corrente del thread di apertura. Se `Enlist=false`, la connessione `SqlClient` non interagirà con una transazione distribuita. Il valore predefinito per `Enlist` è true. Se `Enlist` non è specificato nella stringa di connessione e se viene rilevata una transazione distribuita all'apertura della connessione, quest'ultima verrà automaticamente elencata nella transazione distribuita.

Le parole chiave `Transaction Binding` di una stringa di connessione <xref:Microsoft.Data.SqlClient.SqlConnection> controllano l'associazione della connessione a una transazione `System.Transactions` elencata. È anche disponibile tramite le proprietà <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> e <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

Nella tabella seguente sono descritti i valori possibili.
  
|Parola chiave|Descrizione|  
|-------------|-----------------|  
|Implicit Unbind|Valore predefinito. La connessione viene scollegata dalla transazione una volta completata e torna alla modalità di commit automatico.|
|Explicit Unbind|La connessione rimane collegata alla transazione fino alla chiusura di quest'ultima. La connessione non verrà eseguita se la transazione associata non è attiva o non corrisponde a <xref:System.Transactions.Transaction.Current%2A>.|

## <a name="using-transactionscope"></a>Uso di TransactionScope

La classe <xref:System.Transactions.TransactionScope> rende transazionale un blocco di codice attraverso un'integrazione implicita delle connessioni in una transazione distribuita. È necessario chiamare il metodo <xref:System.Transactions.TransactionScope.Complete%2A> alla fine del blocco <xref:System.Transactions.TransactionScope> prima di lasciarlo. Quando si lascia il blocco viene richiamato il metodo <xref:System.Transactions.TransactionScope.Dispose%2A> . Se è stata generata un'eccezione che ha provocato l'uscita del codice dall'ambito, la transazione sarà considerata interrotta.

Si consiglia di usare un blocco `using` per assicurare che venga chiamato il metodo <xref:System.Transactions.TransactionScope.Dispose%2A> sull'oggetto <xref:System.Transactions.TransactionScope> quando si esce dal blocco using. Se non viene eseguito il commit o il rollback delle transazioni in sospeso, è possibile che le prestazioni vengano notevolmente compromesse, in quanto il timeout predefinito per <xref:System.Transactions.TransactionScope> è di un minuto. Se non si utilizza un'istruzione `using`, è necessario eseguire tutto il lavoro in un blocco `Try` e chiamare in modo esplicito il metodo <xref:System.Transactions.TransactionScope.Dispose%2A> all'interno del blocco `Finally`.

Se viene generata un'eccezione all'interno di <xref:System.Transactions.TransactionScope>, la transazione viene contrassegnata come incoerente e viene interrotta. Il rollback verrà eseguito quando viene eliminato il tipo <xref:System.Transactions.TransactionScope> . Se non si verifica alcuna eccezione, viene eseguito il commit delle transazioni partecipanti.

> [!NOTE]
> Per impostazione predefinita, la classe `TransactionScope` crea una transazione con un <xref:System.Transactions.Transaction.IsolationLevel%2A> di `Serializable`. A seconda dell'applicazione, è possibile abbassare il livello di isolamento per evitare che si verifichi un numero elevato di contese.

> [!NOTE]
> Si consiglia di eseguire solo operazioni di aggiornamento, inserimento ed eliminazione all'interno delle transazioni distribuite, poiché comportano un utilizzo notevole delle risorse di database. Le istruzioni SELECT possono bloccare le risorse di database inutilmente e, in alcuni scenari, può essere necessario usare le transazioni per questo tipo di istruzioni. È necessario eseguire le operazioni non di database al di fuori dell'ambito della transazione, a meno che non richiedano altri gestori di risorse transazionali.
Sebbene un'eccezione nell'ambito della transazione impedisca l'esecuzione del commit, la classe <xref:System.Transactions.TransactionScope> non è in grado di eseguire il rollback delle modifiche apportate al codice al di fuori dell'ambito della transazione stessa. Per eseguire operazioni durante il rollback della transazione, è necessario scrivere la propria implementazione dell'interfaccia <xref:System.Transactions.IEnlistmentNotification> ed elencarla in modo esplicito nella transazione.

## <a name="example"></a>Esempio

Per usare <xref:System.Transactions> , è necessario un riferimento a System.Transactions.dll.

Nella funzione seguente viene illustrato come creare una transazione promuovibile in due istanze diverse di SQL Server, rappresentate da due oggetti <xref:Microsoft.Data.SqlClient.SqlConnection> diversi, incapsulati in un blocco <xref:System.Transactions.TransactionScope> .

Il codice seguente crea il blocco <xref:System.Transactions.TransactionScope> con un'istruzione `using` e apre la prima connessione, che ne comporta l'integrazione automatica in <xref:System.Transactions.TransactionScope>.

La transazione viene integrata inizialmente come transazione lightweight, non come transazione completamente distribuita. La seconda connessione viene inserita in <xref:System.Transactions.TransactionScope> solo se il comando della prima connessione non genera un'eccezione. Quando viene aperta la seconda connessione, la transazione viene automaticamente promossa diventando una piena transazione distribuita.

Successivamente, viene richiamato il metodo <xref:System.Transactions.TransactionScope.Complete%2A>, che esegue il commit della transazione solo se non sono state generate eccezioni. Se è stata generata un'eccezione in qualsiasi punto del blocco <xref:System.Transactions.TransactionScope> , il metodo `Complete` non verrà chiamato e verrà eseguito il rollback della transazione distribuita quando <xref:System.Transactions.TransactionScope> viene eliminato al termine del relativo blocco `using` .

[!code-csharp[SqlTransactionScope#1](~/../sqlclient/doc/samples/SqlTransactionScope.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Transazioni e concorrenza](transactions-and-concurrency.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
