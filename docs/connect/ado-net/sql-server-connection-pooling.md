---
title: Pool di connessioni di SQL Server
description: Informazioni sul modo in cui il provider di dati Microsoft SqlClient per SQL Server riduce al minimo il costo dell'apertura di connessioni tramite il pool di connessioni di SQL Server, che riduce il sovraccarico per le nuove connessioni.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 7e51d44e-7c4e-4040-9332-f0190fe36f07
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a878d8250a3e402cd1043dc289eb1712af45f385
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771534"
---
# <a name="sql-server-connection-pooling-adonet"></a>Pool di connessioni di SQL Server (ADO.NET)

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Generalmente, la connessione a un server database comporta passaggi che richiedono molto tempo. È necessario, infatti, stabilire un canale fisico, ad esempio un socket oppure una named pipe. Deve verificarsi l'handshake iniziale con il server, deve essere analizzata l'informazione sulla stringa di connessione, la connessione deve essere autenticata dal server, sono necessarie verifiche per l'inserimento in un elenco nella transazione corrente e così via.

In pratica, la maggior parte delle applicazioni usa solo una o alcune configurazioni di connessione. Per questo motivo, durante l'esecuzione dell'applicazione, verranno aperte e chiuse più volte connessioni identiche. Per ridurre al minimo il costo dell'apertura delle connessioni, nel provider di dati Microsoft SqlClient per SQL Server viene usata una tecnica di ottimizzazione denominata *pool di connessioni*.

Il pool di connessioni riduce il numero di volte in cui è necessario aprire nuove connessioni. Il *pool* conserva la proprietà della connessione fisica. Gestisce le connessioni mantenendo attivo un set di connessioni attive per ogni configurazione di connessione. Quando un utente chiama `Open` su una connessione, il pool verifica la presenza di una connessione disponibile. Se è disponibile una connessione, il pool la restituisce al chiamante invece di aprirne una nuova. Quando l'applicazione chiama `Close`, il pool restituisce la connessione al set di connessioni attive invece di chiuderla realmente. Una volta restituita al pool, la connessione può essere usata nuovamente nella successiva chiamata `Open`.

È possibile raggruppare in pool solo le connessioni che presentano la stessa configurazione. Il provider di dati Microsoft SqlClient for SQL Server mantiene diversi pool contemporaneamente, uno per ogni configurazione. Le connessioni vengono divise in pool in base alla stringa di connessione e, quando si usa la sicurezza integrata, in base all'identità Windows. Le connessioni vengono inserite nei pool anche in base a dove sono inserite in una transazione. Quando si usa un <xref:Microsoft.Data.SqlClient.SqlConnection.ChangePassword%2A>, le istanze di <xref:Microsoft.Data.SqlClient.SqlCredential> influiscono sul pool di connessioni. Diverse istanze di <xref:Microsoft.Data.SqlClient.SqlCredential> useranno pool di connessioni diversi, anche se l'ID utente e la password sono uguali.

Il pool di connessioni consente di migliorare notevolmente le prestazioni e di aumentare la scalabilità dell'applicazione. Per impostazione predefinita, il pool di connessioni è abilitato nel provider di dati Microsoft SqlClient per SQL Server. A meno che non venga disabilitato in modo esplicito, il pool ottimizza le connessioni che vengono aperte e chiuse nell'applicazione. È possibile, inoltre, fornire più modificatori delle stringhe di connessione per controllare il comportamento del pool di connessioni. Per ulteriori informazioni, vedere "**Controllo del pool di connessioni con parole chiave delle stringhe di connessione**" più avanti in questo argomento.

> [!IMPORTANT]
> Quando il pool di connessioni è abilitato e si verifica un errore di timeout o un altro errore di accesso, viene generata un'eccezione e i tentativi di connessione successivi hanno esito negativo per i successivi **5** secondi, ovvero il "`blocking period`". Se l'applicazione tenta di connettersi durante il periodo di blocco, viene generata di nuovo la prima eccezione. Gli errori successivi al termine di un periodo di blocco comporteranno un nuovo periodo di blocco che è il doppio della durata del periodo di blocco precedente, fino a un *massimo di **1** minuto*.

> [!NOTE]
> Per impostazione predefinita, il meccanismo "`blocking period`" non si applica ad Azure SQL Server. Questo comportamento può essere modificato cambiando la proprietà <xref:Microsoft.Data.SqlClient.PoolBlockingPeriod> in <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString>, ad eccezione di *.NET Standard*.

## <a name="pool-creation-and-assignment"></a>Creazione e assegnazione di pool

Quando una connessione viene aperta per la prima volta, il pool di connessioni viene creato in base a un algoritmo esattamente corrispondente che associa il pool alla stringa di connessione. Ogni pool di connessioni è associato a una stringa di connessione distinta. Quando viene aperta una nuova connessione, se la stringa di connessione non corrisponde esattamente a un pool esistente, viene creato un nuovo pool.

> [!NOTE]
> Le connessioni vengono raggruppate in base al _processo_, al _dominio dell'applicazione_, alla _stringa di connessione_ e, allorché viene utilizzata la sicurezza integrata, all'_identità Windows_. Le stringhe di connessione devono anche essere una corrispondenza esatta. Le parole chiave fornite in un ordine diverso per la stessa connessione verranno inserite in pool separatamente.

> [!NOTE]
> Se `MinPoolSize` non è specificato nella stringa di connessione oppure è specificato come zero, le connessioni nel pool verranno chiuse dopo un periodo di inattività. Tuttavia, se il valore `MinPoolSize` specificato è maggiore di zero, il pool di connessioni verrà eliminato solo dopo lo scaricamento di `AppDomain` e la fine del processo. La manutenzione di pool inattivi o vuoti implica un overhead minimo del sistema.

> [!NOTE]
> Il pool viene cancellato automaticamente quando si verifica un errore irreversibile, quale un failover.

Nell'esempio C# seguente vengono creati tre nuovi oggetti <xref:Microsoft.Data.SqlClient.SqlConnection>, ma per gestirli bastano solo due pool di connessioni. Si noti che la differenza tra la prima e la seconda stringa di connessione consiste nel valore assegnato a `Initial Catalog`.  


[!code-csharp[SqlConnection_Pooling#1](~/../sqlclient/doc/samples/SqlConnection_Pooling.cs#1)]

## <a name="add-connections"></a>Aggiungere connessioni

Per ogni stringa di connessione univoca viene creato un pool di connessioni. Quando si crea un pool, vengono creati e aggiunti al pool più oggetti connessione in modo da soddisfare il requisito relativo alle dimensioni minime del pool. Le connessioni vengono aggiunte al pool in base alle esigenze, fino alla dimensione massima del pool specificata (**100 è il valore predefinito**). Una volta chiuse o eliminate, le connessioni vengono rilasciate nel pool.

Quando viene richiesto un oggetto <xref:Microsoft.Data.SqlClient.SqlConnection>, esso viene ottenuto dal pool se è disponibile una connessione usabile. Per essere utilizzabile, una connessione non deve essere in uso, deve disporre di un contesto di transazione corrispondente oppure non deve essere associata ad alcun contesto di transazione e, infine, deve disporre di un collegamento valido al server.

Il pool di connessioni soddisfa le richieste di connessione grazie alla riallocazione delle connessioni rilasciate nel pool. Se le dimensioni massime del pool sono state raggiunte e non è disponibile alcuna connessione usabile, la richiesta viene accodata. Il pool tenta quindi di recuperare tutte le connessioni fino a quando non viene raggiunto il timeout (**il valore predefinito è 15 secondi**). Se il pool non è in grado di soddisfare la richiesta prima che scada la connessione, viene generata un'eccezione.

> [!CAUTION]
> Al termine dell'uso, si consiglia di chiudere sempre la connessione in modo che possa essere restituita al pool. È possibile eseguire questa operazione usando il metodo `Close` o `Dispose` dell'oggetto `Connection` oppure aprendo tutte le connessioni in un'istruzione `using` in C# o in un'istruzione `Using` in Visual Basic. Le connessioni che non vengono chiuse in modo esplicito potrebbero non essere aggiunte o restituite al pool. Per altre informazioni, vedere [Istruzione using](/dotnet/csharp/language-reference/keywords/using-statement) o [Procedura: eliminare una risorsa di sistema](/dotnet/visual-basic/programming-guide/language-features/control-flow/how-to-dispose-of-a-system-resource) per Visual Basic.

> [!NOTE]
> Non chiamare `Close` o `Dispose` su un oggetto `Connection`, `DataReader` o su qualsiasi altro oggetto gestito nel metodo `Finalize` della classe. Nei finalizzatori rilasciare solo le risorse non gestite che la classe controlla direttamente. Se nella classe non sono presenti risorse non gestite, non includere un metodo `Finalize` nella relativa definizione della classe. Per altre informazioni, vedere [Garbage Collection](/dotnet/standard/garbage-collection/index).

Per altre informazioni sugli eventi associati all'apertura e alla chiusura delle connessioni, vedere [Audit Login - classe di evento](/sql/relational-databases/event-classes/audit-login-event-class) e [Audit Logout - classe di evento](/sql/relational-databases/event-classes/audit-logout-event-class) nella documentazione di SQL Server.

## <a name="remove-connections"></a>Rimuovere connessioni

Il pool di connessioni rimuove una connessione dopo un periodo di inattività di circa **4-8** minuti, oppure se rileva che la connessione al server è stata interrotta.

> [!NOTE]
> Una connessione danneggiata può essere rilevata solo dopo aver tentato di comunicare con il server. Se viene individuata una connessione al server che non è più attiva, la connessione viene contrassegnata come non valida. Le connessioni non valide vengono rimosse dal pool di connessioni solo quando vengono chiuse o recuperate.

Se esiste una connessione a un server non più disponibile, la connessione può essere recuperata dal pool anche se nessuna connessione è stata rilevata come interrotta e pertanto non è stata contrassegnata come non valida. In caso contrario, l'overhead per la verifica della validità della connessione annullerebbe i vantaggi derivanti dal pool provocando un'ulteriore sequenza di andata e ritorno al server. In questo caso, al primo tentativo di uso della connessione verrà rilevato che la connessione è stata interrotta e verrà generata un'eccezione.

## <a name="clear-the-pool"></a>Cancellare il pool

Nel provider di dati Microsoft SqlClient per SQL Server sono stati introdotti due nuovi metodi per cancellare il pool: <xref:Microsoft.Data.SqlClient.SqlConnection.ClearAllPools%2A> e <xref:Microsoft.Data.SqlClient.SqlConnection.ClearPool%2A>. `ClearAllPools` cancella i pool di connessioni per un determinato provider, mentre `ClearPool` cancella il pool di connessioni associato a una connessione specifica.

> [!NOTE]
> Le connessioni in uso al momento della chiamata vengono contrassegnate in modo appropriato. Una volta chiuse, anziché essere restituite al pool, queste connessioni vengono eliminate.

## <a name="transaction-support"></a>Supporto delle transazioni

Le connessioni vengono recuperate dal pool e assegnate in base al contesto della transazione. Se nella stringa di connessione non è specificato `Enlist=false`, il pool di connessioni assicura che la connessione venga inserita in un elenco del contesto <xref:System.Transactions.Transaction.Current%2A>. Quando una connessione viene chiusa e restituita al pool con una transazione `System.Transactions` in elenco, questa connessione viene conservata in modo tale che, quando si richiederà nuovamente il pool di connessioni con la stessa transazione `System.Transactions`, verrà restituita la stessa connessione, se è disponibile. Se viene eseguita una richiesta di questo tipo e non sono disponibili connessioni in pool, viene prelevata e inserita in elenco una connessione della parte non transazionale del pool. Se non sono disponibili connessioni in alcuna area del pool, viene creata e inserita in elenco una nuova connessione.

Una volta chiusa, la connessione viene rilasciata nel pool e nella suddivisione appropriata in base al relativo contesto di transazione. La connessione può quindi essere chiusa senza generare un errore anche se è ancora in sospeso una transazione distribuita. In questo modo è possibile eseguire il commit o interrompere la transazione distribuita in un secondo momento.

## <a name="control-connection-pooling-with-connection-string-keywords"></a>Controllare il pool di connessioni con le parole chiave della stringa di connessione

La proprietà `ConnectionString` dell'oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> supporta le coppie chiave/valore della stringa di connessione che possono essere usare per regolare il comportamento della logica del pool di connessioni. Per altre informazioni, vedere <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="pool-fragmentation"></a>Frammentazione del pool

La frammentazione di pool rappresenta un problema comune di varie applicazioni Web in cui l'applicazione può creare numerosi pool che vengono rilasciati solo al termine del processo. In questo modo, molte connessioni vengono lasciate aperte, occupando memoria e riducendo le prestazioni.

### <a name="pool-fragmentation-due-to-integrated-security"></a>Frammentazione del pool dovuta alla sicurezza integrata

Le connessioni vengono eseguite in pool in base alla stringa di connessione e all'identità utente. Di conseguenza, se nel sito Web si usa l'autenticazione di base o l'autenticazione Windows, nonché un accesso di sicurezza, si otterrà un pool per ogni utente. Anche se in questo modo migliorano le prestazioni delle successive richieste di database per il singolo utente, l'utente non può usare le connessioni eseguite da altri. Viene generata, inoltre, almeno una connessione per utente al server database. Questo è un effetto collaterale di una particolare architettura di applicazione Web che gli sviluppatori devono tenere in considerazione per non compromettere i requisiti di sicurezza e controllo.

### <a name="pool-fragmentation-due-to-many-databases"></a>Frammentazione del pool dovuta a numerosi database

Molti provider di servizi Internet ospitano numerosi siti Web su un unico server. Possono usare un unico database per confermare un accesso basato su form e aprire una connessione a un database specifico di un utente o di un gruppo di utenti. La connessione al database di autenticazione viene eseguita in pool e può essere usata da tutti. Tuttavia è presente un pool di connessioni separato per ogni database, aumentando il numero di connessioni al server.

Anche questo è un effetto collaterale della progettazione dell'applicazione. È relativamente semplice evitare questo effetto collaterale senza compromettere la sicurezza quando ci si connette a SQL Server. Invece di connettersi a un solo database per ogni utente o gruppo, è sufficiente connettersi allo stesso database sul server e successivamente eseguire l'istruzione USE Transact-SQL per passare al database desiderato.
 
Nel frammento di codice seguente viene illustrata la creazione di una connessione iniziale al database `master` e il passaggio al database specificato nella variabile di tipo stringa `databaseName`.

[!code-csharp[SqlConnection_Pooling_Use_Statement#1](~/../sqlclient/doc/samples/SqlConnection_Pooling_Use_Statement.cs#1)]

## <a name="application-roles-and-connection-pooling"></a>Ruoli applicazione e pool di connessioni

Dopo che un ruolo dell'applicazione SQL Server è stato attivato chiamando la stored procedure di sistema `sp_setapprole`, non è possibile impostare nuovamente il contesto di sicurezza di tale connessione. Se è attivata la creazione di pool, la connessione verrà restituita al pool e si verificherà un errore quando la connessione in pool verrà riutilizzata.

### <a name="application-role-alternatives"></a>Alternative del ruolo applicazione

Si consiglia di trarre vantaggio dai meccanismi di sicurezza che è possibile usare al posto dei ruoli applicazione.

## <a name="see-also"></a>Vedere anche

- [Pool di connessioni](connection-pooling.md)
- [SQL Server e ADO.NET](./sql/index.md)
- [Contatori delle prestazioni in SqlClient](performance-counters.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
