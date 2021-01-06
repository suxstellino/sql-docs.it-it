---
title: Opzioni di configurazione della memoria del server | Microsoft Docs
ms.custom: contperf-fy20q4
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: Informazioni su come configurare la quantità di memoria allocata dal gestore della memoria di SQL Server ai processi di SQL Server. Visualizzare approcci ed esempi per la gestione della memoria.
helpviewer_keywords:
- Virtual Memory Manager
- max server memory option
- virtual memory [SQL Server]
- VMM
- server memory options [SQL Server]
- servers [SQL Server], memory
- buffer pools [SQL Server]
- min server memory option
- manual memory options [SQL Server]
- memory [SQL Server], servers
ms.assetid: 29ce373e-18f8-46ff-aea6-15bbb10fb9c2
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 71cab9c11aaa4f3dc569761fdfef407601162b1c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878910"
---
# <a name="server-memory-configuration-options"></a>Opzioni di configurazione della memoria del server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Riconfigurare la quantità di memoria (in megabyte) per un processo di SQL Server usato da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Sono disponibili due opzioni per la memoria del server, **min server memory** e **max server memory**. Queste opzioni modificano la quantità di memoria che il gestore della memoria di SQL Server può allocare a un processo di SQL Server.
  
Le impostazioni predefinite e i valori minimi consentiti per queste opzioni sono:

|Opzione  |  Predefinito | Minimo consentito  |
|---------|---------|---------|
|**min server memory**     |    0     |    0     |
|**max server memory**     |     2\.147.483.647 megabyte (MB)     |  128 MB       |

Per impostazione predefinita, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i requisiti di memoria possono variare dinamicamente in base alle risorse di sistema disponibili. Per altre informazioni, vedere [Gestione della memoria dinamica](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management).

> [!IMPORTANT]  
> Se si imposta **max server memory** su un valore troppo alto, una singola istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe risultare in competizione per la memoria con altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ospitate nello stesso host. Tuttavia, un valore troppo basso per questa impostazione potrebbe causare un utilizzo molto elevato di memoria e problemi di prestazioni. L'impostazione di **max server memory** sul valore minimo può persino impedire l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non è possibile avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dopo la modifica di questa opzione, usare l'opzione di avvio **_-f_** e reimpostare **max server memory** sul valore precedente. Per altre informazioni, vedere [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare la memoria in modo dinamico. È possibile, tuttavia, impostare manualmente le opzioni per la memoria e limitare la quantità di memoria a cui può accedere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Prima di impostare la quantità di memoria per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], determinare l'impostazione appropriata per la memoria sottraendo dalla memoria fisica totale la memoria necessaria per il sistema operativo, per le allocazioni di memoria non controllate dall'impostazione max server memory e per qualsiasi altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nonché per eventuali altri utilizzi del sistema se il computer non è completamente dedicato a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La differenza così ottenuta rappresenta la quantità di memoria massima assegnabile all'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 
## <a name="set-options-manually"></a><a name="manually"></a> Impostare manualmente le opzioni  
Le opzioni per la memoria **min server memory** e **max server memory** possono essere impostate come valori limite di un intervallo di valori di memoria. Questo metodo è utile per gli amministratori di sistema o di database che desiderano configurare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] insieme ai requisiti di memoria di altre applicazioni o altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguite nello stesso host.

> [!NOTE]
> **min server memory** e **max server memory** sono opzioni avanzate. Se si utilizza la stored procedure di sistema **sp_configure** per modificare queste impostazioni, è possibile modificarle solo se il valore di **show advanced options** è impostato su 1. Queste impostazioni diventano effettive immediatamente e non richiedono il riavvio del server.  
  
<a name="min_server_memory"></a> Usare **min server memory** per garantire una quantità minima di memoria disponibile per lo strumento di gestione della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non alloca immediatamente la quantità di memoria specificata in **min server memory** all'avvio. Se, tuttavia, l'utilizzo della memoria raggiunge tale valore a causa del carico di lavoro del client, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può liberare memoria solo se si riduce il valore di **min server memory** . Ad esempio, quando più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono esistere contemporaneamente nello stesso host, impostare il parametro min server memory anziché max server memory allo scopo di riservare memoria per un'istanza. In un ambiente virtualizzato, inoltre, è essenziale impostare un valore min server memory per assicurarsi che un utilizzo elevato di memoria dall'host sottostante non tenti di deallocare memoria dal pool di buffer in una macchina virtuale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] guest, oltre i livelli necessari per garantire prestazioni accettabili.

>[!NOTE]
>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non esegue necessariamente l'allocazione della quantità di memoria specificata in **min server memory**. Se il carico sul server non richiede mai l'allocazione della quantità di memoria specificata in **min server memory**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà eseguito con una quantità di memoria inferiore.

<a name="max_server_memory"></a> Usare **max server memory** per garantire che il sistema operativo non subisca un pregiudizievole utilizzo elevato di memoria. Per impostare la configurazione di max server memory, monitorare il consumo complessivo del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per determinare i requisiti di memoria. Per ottenere calcoli più precisi per una singola istanza:
- Dalla memoria totale del sistema operativo riservare 1-4 GB al sistema operativo stesso.
- Sottrarre quindi l'equivalente delle potenziali allocazioni di memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal controllo **max server memory** costituito da **stack size <sup>1</sup> \* calculated max worker threads <sup>2</sup>** . Il valore restante dovrebbe corrispondere all'impostazione di max server memory per la configurazione di una singola istanza.

<sup>1</sup> Fare riferimento alla [Guida sull'architettura di gestione della memoria](../../relational-databases/memory-management-architecture-guide.md#stacksizes) per informazioni sulle dimensioni degli stack di thread per ogni architettura.

<sup>2</sup> Fare riferimento alla pagina della documentazione [Configurare l'opzione di configurazione del server max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) per informazioni sui thread di lavoro predefiniti calcolati per un determinato numero di CPU per cui è stata impostata l'affinità nell'host corrente.

## <a name="use-ssmanstudiofull"></a>Utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
Usare le due opzioni per la memoria del server **min server memory** e **max server memory** per riconfigurare la quantità di memoria, in megabyte, gestita dallo strumento di gestione della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per impostazione predefinita, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i requisiti di memoria possono variare dinamicamente in base alle risorse di sistema disponibili.  
  
### <a name="procedure-for-configuring-a-fixed-amount-of-memory-not-recommended"></a>Procedura per la configurazione di una quantità di memoria fissa (non consigliata)  
Per impostare una quantità di memoria fissa:  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Memoria** .  
  
3.  In **Opzioni per la memoria del server** immettere la quantità da usare per **Memoria minima per il server** e **Memoria massima per il server**.  
  
     Usare le impostazioni predefinite per consentire a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di modificare i requisiti di memoria in base alle risorse di sistema disponibili. È consigliabile impostare il valore **max server memory** come [descritto in dettaglio in precedenza](#max_server_memory). 

Lo screenshot seguente illustra tutti e tre i passaggi: 

:::image type="content" source="media/server-memory-server-configuration-options/configure-memory-in-ssms.png" alt-text="Configurare la memoria in SSMS":::

  
## <a name="lock-pages-in-memory-lpim"></a>Blocco di pagine in memoria 
Questi criteri di Windows determinano gli account autorizzati a usare un processo per mantenere i dati nella memoria fisica, impedendo al sistema di eseguire il paging dei dati nella memoria virtuale su disco. Il blocco delle pagine in memoria può garantire il corretto funzionamento del server quando si verifica il paging della memoria su disco. L'opzione **Blocco di pagine in memoria** viene impostata su ON per istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition ed edizioni superiori quando all'account con i privilegi per l'esecuzione di sqlservr.exe è stato concesso il diritto utente di Windows *Blocco di pagine in memoria*.  
  
Per disabilitare l'opzione **Blocco di pagine in memoria** per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], rimuovere il diritto utente *Blocco di pagine in memoria* per l'account con i privilegi per l'esecuzione di sqlservr.exe (l'account di avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
 
Questa opzione non influisce sulla [gestione dinamica della memoria](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management) di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consentendone l'espansione o la riduzione su richiesta di altri clerk di memoria. Quando si usa il diritto utente *Blocco di pagine in memoria* è consigliabile impostare un limite superiore per **max server memory** come [descritto in dettaglio in precedenza](#max_server_memory).

> [!IMPORTANT]
> L'impostazione di questa opzione deve essere usata solo quando necessario, ovvero in presenza di segnali di page out del processo sqlservr. In questo caso nel log degli errori viene segnalato l'errore 17890, simile a quello riportato nell'esempio seguente: `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`
> A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] il [flag di traccia 845](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) non è necessario per l'uso dell'opzione Blocco di pagine nell'edizione Standard. 
  
### <a name="to-enable-lock-pages-in-memory"></a>Per abilitare l'opzione Blocco di pagine in memoria  
Per abilitare l'opzione Blocco di pagine in memoria:  
  
1.  Fare clic sul menu **Start** e scegliere **Esegui**. Nella casella **Apri** digitare **gpedit.msc**.  
  
     Viene visualizzata la finestra di dialogo **Criteri gruppo** .  
  
2.  Nella console **Criteri di gruppo** espandere **Configurazione computer** e quindi espandere **Impostazioni di Windows**.  
  
3.  Espandere **Impostazioni sicurezza** e quindi espandere **Criteri locali**.  
  
4.  Selezionare la cartella **Assegnazione diritti utente** .  
  
     I criteri verranno visualizzati nel riquadro dei dettagli.  
  
5.  Nel riquadro fare doppio clic su **Blocco di pagine in memoria**.  
  
6.  Nella finestra di dialogo **Impostazioni criteri di sicurezza locali** aggiungere l'account con i privilegi per l'esecuzione di sqlservr.exe (account di avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
## <a name="multiple-instances-of-ssnoversion"></a>Più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Quando si eseguono più istanze di [!INCLUDE[ssDE](../../includes/ssde-md.md)], è possibile gestire la memoria in tre modi:  
  
-   Usare **max server memory** per controllare l'utilizzo della memoria, come [descritto in dettaglio in precedenza](#max_server_memory). Stabilire le impostazioni massime per ogni istanza, accertandosi che il totale non sia superiore alla memoria fisica disponibile sul computer. È possibile rendere la memoria di ogni istanza proporzionale al relativo carico di lavoro previsto o alle dimensioni del database. Questo approccio presenta il vantaggio di rendere la memoria libera immediatamente disponibile ad ogni nuovo processo o istanza. Lo svantaggio è che se non vengono eseguite tutte le istanze, parte della memoria resterà inusata.  
  
-   Usare **min server memory** per controllare l'utilizzo della memoria, come [descritto in dettaglio in precedenza](#min_server_memory). Stabilire le impostazioni minime per ogni istanza, in modo che la somma di tali minimi sia 1-2 GB inferiore alla memoria fisica totale del computer. Anche questi minimi possono essere resi proporzionali al carico previsto dell'istanza. Con questo approccio, quando non vengono eseguite tutte le istanze contemporaneamente, quelle in esecuzione potranno usare la memoria libera rimanente. Questo approccio consente inoltre di riservare a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] una quantità ragionevole di memoria quando sullo stesso computer vengono eseguiti anche altri processi particolarmente onerosi. Lo svantaggio è che quando vengono avviati una nuova istanza o altri processi, le istanze eseguite rilasceranno la memoria con un certo ritardo, in particolare quando a tale scopo dovranno riscrivere le pagine modificate nei rispettivi database.  
  
-   Non intervenire in alcun modo (non consigliato). Le prime istanze sottoposte a carico di lavoro tenderanno ad allocare tutta la memoria. Alle istanze inattive o a quelle avviate in un secondo momento verrà destinata solo una minima quantità di memoria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene ripartita in alcun modo la memoria tra le diverse istanze. Tutte le istanze, tuttavia, risponderanno ai segnali di Windows Memory Notification correggendo di conseguenza le dimensioni dei rispettivi footprint di memoria. In Windows la memoria non viene bilanciata tra le applicazioni tramite l'API di Windows Memory Notification. Offre invece un semplice feedback globale sulla disponibilità di memoria nel sistema.  
  
 Poiché è possibile modificare queste impostazioni senza riavviare le istanze, sarà possibile provare agevolmente valori diversi fino a individuare quelli più adatti alle esigenze.  
  
## <a name="provide-the-maximum-amount-of-memory"></a>Assegnare la quantità massima di memoria

La memoria può essere configurata fino al limite dello spazio degli indirizzi virtuali del processo in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Memory Limits for Windows and Windows Server Releases](/windows/desktop/Memory/memory-limits-for-windows-releases#physical-memory-limits-windows-server-2016) (Limiti di memoria per le diverse versioni di Windows e Windows Server).

## <a name="examples"></a>Esempi

### <a name="example-a-set-the-max-server-memory-option-to-4-gb"></a>Esempio A. Impostare l'opzione di memoria massima del server su 4 GB.
 Nell'esempio seguente l'opzione `max server memory` viene impostata su 4 GB.  Si noti che anche se `sp_configure` specifica il nome dell'opzione come `max server memory (MB)`, l'esempio illustra l'omissione di `(MB)`.

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'max server memory', 4096;
GO
RECONFIGURE;
GO
```
Verrà restituita un'istruzione simile a:

> L'impostazione 2147483647 dell'opzione di configurazione 'max server memory (MB)' è stata sostituita con 4096. Per eseguire l'installazione, utilizzare RECONFIGURE.

### <a name="example-b-determining-current-memory-allocation"></a>Esempio B: Determinazione dell'allocazione di memoria corrente  
 La query seguente restituisce le informazioni sulla memoria attualmente allocata.  

```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
   large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
   locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
   virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
   virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
   virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
   page_fault_count AS sql_page_fault_count,
   memory_utilization_percentage AS sql_memory_utilization_percentage, 
   process_physical_memory_low AS sql_process_physical_memory_low, 
   process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  

### <a name="example-c-determining-value-for-max-server-memory-mb"></a>Esempio C. Determinazione del valore per 'max server memory (MB)'
La query seguente restituisce informazioni sul valore attualmente configurato e il valore in uso in SQL Server.  Questa query restituirà risultati indipendentemente dal fatto che 'show advanced options' sia impostato su True.

```sql
SELECT c.value, c.value_in_use
FROM sys.configurations c WHERE c.[name] = 'max server memory (MB)'
```
  
## <a name="next-steps"></a>Passaggi successivi
 [Guida sull'architettura di gestione della memoria](../../relational-databases/memory-management-architecture-guide.md)   
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md)   
 [Edizioni e le funzionalità supportate di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md#Cross-BoxScaleLimits)   
 [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md#Cross-BoxScaleLimits)   
 [Edizioni e funzionalità supportate di SQL Server 2017 in Linux](../../linux/sql-server-linux-editions-and-components-2017.md#Cross-BoxScaleLimits)   
 [Memory Limits for Windows and Windows Server Releases](/windows/desktop/Memory/memory-limits-for-windows-releases) (Limiti di memoria per le diverse versioni di Windows e Windows Server)
