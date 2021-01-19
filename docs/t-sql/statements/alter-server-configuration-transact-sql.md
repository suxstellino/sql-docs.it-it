---
description: ALTER SERVER CONFIGURATION (Transact-SQL)
title: ALTER SERVER CONFIGURATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SERVER CONFIGURATION
- ALTER_SERVER_CONFIGURATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER CONFIGURATION, ALTER
- process affinity, setting
- ALTER SERVER CONFIGURATION statement
- setting process affinity
ms.assetid: f3059e42-5f6f-4a64-903c-86dca212a4b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bab0801b0193d9f675ef69e566eef375f0930e5b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170863"
---
# <a name="alter-server-configuration-transact-sql"></a>ALTER SERVER CONFIGURATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Vengono modificate le impostazioni di configurazione globali per il server corrente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  

```syntaxsql
ALTER SERVER CONFIGURATION  
SET <optionspec>   
[;]  
  
<optionspec> ::=  
{  
     <process_affinity>  
   | <diagnostic_log>  
   | <failover_cluster_property>  
   | <hadr_cluster_context>  
   | <buffer_pool_extension>  
   | <soft_numa>  
   | <memory_optimized>
}  
  
<process_affinity> ::=   
   PROCESS AFFINITY   
   {  
     CPU = { AUTO | <CPU_range_spec> }   
   | NUMANODE = <NUMA_node_range_spec>   
   }  
   <CPU_range_spec> ::=   
      { CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]   
  
   <NUMA_node_range_spec> ::=   
      { NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
  
<diagnostic_log> ::=   
   DIAGNOSTICS LOG   
   {   
     ON    
   | OFF    
   | PATH = { 'os_file_path' | DEFAULT }    
   | MAX_SIZE = { 'log_max_size' MB | DEFAULT }    
   | MAX_FILES = { 'max_file_count' | DEFAULT }    
   }  
  
<failover_cluster_property> ::=   
   FAILOVER CLUSTER PROPERTY <resource_property>  
   <resource_property> ::=  
      {  
        VerboseLogging = { 'logging_detail' | DEFAULT }    
      | SqlDumperDumpFlags = { 'dump_file_type' | DEFAULT }  
      | SqlDumperDumpPath = { 'os_file_path'| DEFAULT }  
      | SqlDumperDumpTimeOut = { 'dump_time-out' | DEFAULT }  
      | FailureConditionLevel = { 'failure_condition_level' | DEFAULT }  
      | HealthCheckTimeout = { 'health_check_time-out' | DEFAULT }  
      }  
  
<hadr_cluster_context> ::=  
   HADR CLUSTER CONTEXT = { 'remote_windows_cluster' | LOCAL }  
  
<buffer_pool_extension>::=  
    BUFFER POOL EXTENSION   
    { ON ( FILENAME = 'os_file_path_and_name' , SIZE = <size_spec> )   
    | OFF }  
  
    <size_spec> ::=  
        { size [ KB | MB | GB ] }  
  
<soft_numa> ::=  
    SOFTNUMA  
    { ON | OFF }  

<memory-optimized> ::=   
   MEMORY_OPTIMIZED   
   {   
     ON 
   | OFF
   | [ TEMPDB_METADATA = { ON [(RESOURCE_POOL='resource_pool_name')] | OFF }
   | [ HYBRID_BUFFER_POOL = { ON | OFF }
   }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
**\<process_affinity> ::=**  
  
PROCESS AFFINITY  
Consente di associare i thread di hardware alle CPU.  
  
CPU = { AUTO | \<CPU_range_spec> }  
Consente di distribuire thread di lavoro di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a ogni CPU all'interno dell'intervallo specificato. Alle CPU non incluse nell'intervallo specificato non saranno assegnati thread.  
  
AUTO  
Specifica che a nessun thread viene assegnata una CPU. Il sistema operativo può spostare liberamente i thread tra le CPU in base al carico di lavoro del server. Questa impostazione è quella predefinita e consigliata.  
  
\<CPU_range_spec> ::=  
Specifica la CPU o l'intervallo di CPU a cui assegnare thread.  
  
{ CPU_ID | CPU_ID TO CPU_ID } [ ,...n ]  
Elenco di una o più CPU. Gli ID CPU iniziano da 0 e sono valori **interi**.  
  
NUMANODE = \<NUMA_node_range_spec>  
Consente di assegnare thread a tutte le CPU che appartengono al nodo NUMA o all'intervallo di nodi NUMA specificato.  
  
\<NUMA_node_range_spec> ::=  
Specifica lo stato del nodo NUMA o dell'intervallo di nodi NUMA.  
  
{ NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
Elenco di uno o più nodi NUMA. Gli ID del nodo NUMA iniziano da 0 e sono valori **interi**.  
  
**\<diagnostic_log> ::=**  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  

  
DIAGNOSTICS LOG  
Avvia o arresta la registrazione dei dati di diagnostica acquisiti dalla stored procedure sp_server_diagnostics. Questo argomento consente anche di impostare i parametri di configurazione del log SQLDIAG, ad esempio il conteggio del rollover dei file, le dimensioni del file di log e il percorso del file. Per altre informazioni, vedere [Visualizzazione e lettura del log di diagnostica dell'istanza del cluster di failover](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
ON  
Consente di avviare la registrazione dei dati di diagnostica di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] nel percorso specificato dall'opzione file PATH. Questo argomento è l'impostazione predefinita.  
  
OFF  
Consente di arrestare la registrazione dei dati di diagnostica.  
  
PATH = { 'os_file_path' | DEFAULT }  
Percorso che indica la posizione dei log di diagnostica. Il percorso predefinito è \<\MSSQL\Log> all'interno della cartella di installazione dell'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
MAX_SIZE = { 'log_max_size' MB | DEFAULT }  
Dimensione massima in megabyte che può raggiungere ogni log di diagnostica. Il valore predefinito è 100 MB.  
  
MAX_FILES = { 'max_file_count' | DEFAULT }  
Numero massimo di file di log di diagnostica che è possibile archiviare nel computer prima che vengano riciclati per nuovi log di diagnostica.  
  
**\<failover_cluster_property> ::=**  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
FAILOVER CLUSTER PROPERTY  
Consente di modificare le proprietà del cluster di failover privato delle risorse di SQL Server.  
  
VERBOSE LOGGING = { 'logging_detail' | DEFAULT }  
Consente di impostare il livello di registrazione per il clustering di failover di SQL Server. Può essere abilitata per fornire dettagli aggiuntivi nei log degli errori per la risoluzione dei problemi.  
  
-   0: la registrazione è disabilitata (impostazione predefinita)  
  
-   1: solo errori  
  
-   2: errori e avvisi  
  
Negli scenari di failover delle risorse, la DLL della risorsa SQL Server può ottenere un file dump prima che si verifichi un failover. Questo vale sia per le tecnologie FCI che per quelle del gruppo di disponibilità. Quando la DLL della risorsa SQL Server determina che una risorsa SQL Server ha avuto esito negativo, la DLL della risorsa SQL Server usa l'utilità Sqldumper.exe per ottenere un file dump del processo di SQL Server. Per assicurarsi che l'utilità Sqldumper.exe generi correttamente il file dump al momento del failover delle risorse, è necessario impostare le tre proprietà seguenti come prerequisiti: SqlDumperDumpTimeOut, SqlDumperDumpPath, SqlDumperDumpFlags.

SQLDUMPEREDUMPFLAGS  
Determina il tipo di file di dump generati dall'utilità SQLDumper di SQL Server. L'impostazione predefinita è 0. Per questa impostazione vengono usati valori decimali, anziché esadecimali. Per il minidump usare 288, per il minidump con memoria indiretta usare 296, per il dump filtrato usare 33024. Per altre informazioni, vedere l'[articolo sulla knowledge base dell'utilità SQL Server Dumper](https://go.microsoft.com/fwlink/?LinkId=206173).  
  
SQLDUMPERDUMPPATH = { 'os_file_path' | DEFAULT }  
Percorso in cui l'utilità SQLDumper archivia i file di dump. Per altre informazioni, vedere l'[articolo sulla knowledge base dell'utilità SQL Server Dumper](https://go.microsoft.com/fwlink/?LinkId=206173).  
  
SQLDUMPERDUMPTIMEOUT = { 'dump_time-out' | DEFAULT }  
Valore di timeout in millisecondi prima che l'utilità SQLDumper generi un dump in caso di errore di SQL Server. Il valore predefinito è 0, che indica che non vi sono limiti di tempo per completare il dump. Per altre informazioni, vedere l'[articolo sulla knowledge base dell'utilità SQL Server Dumper](https://go.microsoft.com/fwlink/?LinkId=206173).  
  
 FAILURECONDITIONLEVEL = { 'failure_condition_level' | DEFAULT }  
 Condizioni in cui si verifica il failover o il riavvio dell'istanza del cluster di failover di SQL Server. Il valore predefinito è 3, che indica che si verificherà il failover o il riavvio della risorsa di SQL Server in caso di errori critici del server. Per altre informazioni su questo aspetto e su altri livelli delle condizioni di errore, vedere [Configurare le impostazioni della proprietà FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
HEALTHCHECKTIMEOUT = { 'health_check_time-out' | DEFAULT }  
Valore di timeout che consente di definire il tempo di attesa da parte della DLL risorse del motore di database di SQL Server relativo alla restituzione delle informazioni sull'integrità del server prima che venga stabilita la mancata risposta dell'istanza di SQL Server. Il valore di timeout è espresso in millisecondi. L'impostazione predefinita è 60.000 millisecondi (60 secondi).  
  
**\<hadr_cluster_context> ::=**  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
HADR CLUSTER CONTEXT **=** { **'** _remote\_windows\_cluster_ **'** | LOCAL }  
Passa il contesto del cluster HADR dell'istanza del server al cluster WSFC (Windows Server Failover Cluster) specificato. Il *contesto del cluster HADR* determina il cluster WSFC che gestisce i metadati per le repliche di disponibilità ospitate dall'istanza del server. Usare l'opzione SET HADR CLUSTER CONTEXT solo durante una migrazione tra cluster di [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] a un'istanza di [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] o versione successiva in un nuovo cluster WSFC.  
  
È possibile passare il contesto del cluster HADR solo dal cluster WSFC locale a un cluster WSFC remoto. Quindi, è possibile scegliere di passare nuovamente dal cluster WSFC remoto al cluster WSFC locale. È possibile cambiare il contesto del cluster HADR in un cluster remoto solo se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non ospita alcuna replica di disponibilità.  
  
Il contesto di un cluster HADR remoto può essere nuovamente cambiato nel cluster locale in qualsiasi momento, a meno che l'istanza del server non ospiti repliche di disponibilità. 
  
Per identificare il cluster di destinazione, specificare uno dei valori seguenti:  
  
*windows_cluster*  
Nome rete di un cluster WSFC. È possibile specificare il nome breve o il nome di dominio completo. Per individuare l'indirizzo IP di destinazione di un nome breve, ALTER SERVER CONFIGURATION utilizza la risoluzione DNS. In alcuni casi, un nome breve potrebbe generare confusione e DNS potrebbe restituire l'indirizzo IP errato. È consigliabile specificare il nome di dominio completo.  
  
> [!NOTE] 
> Una migrazione tra cluster che usa questa impostazione non è più supportata. Per eseguire una migrazione tra cluster, usare un gruppo di disponibilità distribuito o un altro metodo, ad esempio il log shipping. 
  
LOCAL  
Cluster WSFC locale.  
  
Per altre informazioni, vedere [Modificare il contesto del cluster HADR dell'istanza del server &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md).  
  
**\<buffer_pool_extension>::=**  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
ON  
Abilita l'opzione di estensione del pool di buffer. Questa opzione estende le dimensioni del pool di buffer tramite memoria non volatile. Con la memoria non volatile, come le unità SSD, vengono salvate in modo permanente pagine di dati pulite nel pool. Per altre informazioni su questa funzionalità, vedere [Estensione pool di buffer](../../database-engine/configure-windows/buffer-pool-extension.md). L'estensione del pool di buffer non è disponibile in tutte le edizioni di SQL Server. Per altre informazioni, vedere [Edizioni e funzionalità supportate di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
FILENAME = 'os_file_path_and_name'  
Definisce il percorso e il nome della directory del file di cache dell'estensione del pool di buffer. L'estensione del file deve essere specificata come .BPE. Disattivare BUFFER POOL EXTENSION prima di modificare FILENAME.  
  
SIZE = *size* [ **KB** | MB | GB ]  
Definisce le dimensioni della cache. La specifica predefinita delle dimensioni è KB. La dimensione minima è la dimensione della memoria massima del server. Il limite massimo è 32 volte la dimensione della memoria massima del server. Per altre informazioni sulla memoria massima del server, vedere [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Disattivare BUFFER POOL EXTENSION prima di modificare le dimensioni del file. Per specificare dimensioni inferiori a quelle correnti, è necessario riavviare l'istanza di SQL Server per recuperare memoria. In caso contrario, le dimensioni specificate devono essere uguali o maggiori delle dimensioni correnti.  
  
OFF  
Disabilita l'opzione di estensione del pool di buffer. Disabilitare l'opzione di estensione del pool di buffer prima di modificare i parametri associati, ad esempio le dimensioni o il nome del file. Quando questa opzione è disabilitata, tutte le informazioni di configurazione correlate vengono rimosse dal Registro di sistema.  
  
> [!WARNING]  
>  La disabilitazione dell'estensione del pool di buffer potrebbe influire negativamente sulle prestazioni del server perché la dimensione del pool di buffer si riduce in modo significativo.  
  
**\<soft_numa>**  

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]).  
  
ON  
Consente il partizionamento automatico in modo da dividere i nodi hardware NUMA di grandi dimensioni in nodi NUMA di dimensioni ridotte. Per modificare il valore corrente è necessario riavviare il motore di database.  
  
OFF  
Disabilita il partizionamento automatico dei nodi hardware NUMA di grandi dimensioni in nodi NUMA di dimensioni ridotte. Per modificare il valore corrente è necessario riavviare il motore di database.  

> [!WARNING]
> Sono noti problemi di comportamento dell'istruzione ALTER SERVER CONFIGURATION se usata insieme all'opzione SOFT-NUMA e a SQL Server Agent.  È consigliabile eseguire le operazioni seguendo la sequenza riportata di seguito:  
> 1) Arrestare l'istanza di SQL Server Agent.  
> 2) Eseguire l'opzione ALTER SERVER CONFIGURATION SOFT NUMA.  
> 3) Riavviare l'istanza di SQL Server.  
> 4) Avviare l'istanza di SQL Server Agent.  
  
**Altre informazioni:** se si esegue l'istruzione ALTER SERVER CONFIGURATION con il comando SET SOFTNUMA prima del riavvio del servizio SQL Server, all'arresto del servizio SQL Server Agent viene eseguito un comando T-SQL RECONFIGURE che ripristina le impostazioni SOFTNUMA sui valori antecedenti l'esecuzione di ALTER SERVER CONFIGURATION. 

**\<memory_optimized> ::=**

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).

ON <br>
Abilita tutte le funzionalità a livello di istanza che fanno parte della famiglia di funzionalità del [database in memoria](../../relational-databases/in-memory-database.md). Attualmente include i [metadati tempdb ottimizzati per la memoria](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) e il [pool di buffer ibrido](../../database-engine/configure-windows/hybrid-buffer-pool.md). È necessario un riavvio per rendere effettiva l'impostazione.

OFF <br>
Disabilita tutte le funzionalità a livello di istanza che fanno parte della famiglia di funzionalità del database in memoria. È necessario un riavvio per rendere effettiva l'impostazione.

TEMPDB_METADATA = ON | OFF <br>
Abilita o disabilita solo i metadati tempdb ottimizzati per la memoria. È necessario un riavvio per rendere effettiva l'impostazione. 

RESOURCE_POOL='resource_pool_name' <br>
Se combinato con TEMPDB_METADATA = ON, specifica il pool di risorse definito dall'utente da usare per tempdb. Se non specificato, tempdb userà il pool predefinito. Il pool deve essere già esistente. Se il pool non è disponibile quando il servizio viene riavviato, tempdb userà il pool predefinito.


HYBRID_BUFFER_POOL = ON | OFF <br>
Abilita o disabilita il pool di buffer ibrido a livello di istanza. È necessario un riavvio per rendere effettiva l'impostazione.


## <a name="general-remarks"></a>Osservazioni generali  
Questa istruzione non richiede il riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se non specificato diversamente. Nel caso di un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], non è necessario un riavvio della risorsa cluster di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
Questa istruzione non supporta i trigger DDL.  
  
## <a name="permissions"></a>Autorizzazioni  
Richiede:
- Le autorizzazioni `ALTER SETTINGS` per l'opzione di affinità del processo.
- Le autorizzazioni `ALTER SETTINGS` e `VIEW SERVER STATE` per le opzioni delle proprietà del log di diagnostica e del cluster di failover.
- L'autorizzazione `CONTROL SERVER` per l'opzione di contesto del cluster HADR.  
- L'autorizzazione `ALTER SERVER STATE` per l'opzione di estensione del pool di buffer.  
  
La DLL della risorsa del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] viene eseguita con l'account di sistema locale. L'account di sistema locale, pertanto, deve avere accesso in lettura e in scrittura al percorso specificato nell'opzione relativa al log di diagnostica.  
  
## <a name="examples"></a>Esempi  
  
|Category|Elementi di sintassi inclusi|  
|--------------|------------------------------|  
|[Impostazione dell'affinità del processo](#Affinity)|CPU • NUMANODE • AUTO|  
|[Impostazioni delle opzioni del log di diagnostica](#Diagnostic)|ON • OFF • PATH • MAX_SIZE|  
|[Impostazione delle proprietà del cluster di failover](#Failover)|HealthCheckTimeout|  
|[Modifica del contesto del cluster di una replica di disponibilità](#ChangeClusterContextExample)|**'** *windows_cluster* **'**|  
|[Impostazione dell'estensione del pool di buffer](#BufferPoolExtension)|BUFFER POOL EXTENSION| 
|[Impostazione delle opzioni del database in memoria](#MemoryOptimized)|MEMORY_OPTIMIZED|

  
###  <a name="setting-process-affinity"></a><a name="Affinity"></a> Impostazione dell'affinità del processo  
Negli esempi inclusi in questa sezione viene illustrato come impostare l'affinità del processo in CPU e nodi NUMA. Negli esempi si presuppone che il server contenga 256 CPU disposte ciascuna in quattro gruppi di 16 nodi NUMA. I thread non sono assegnati ad alcun nodo NUMA o CPU.  
  
-   Gruppo 0: nodi NUMA da 0 a 3, CPU da 0 a 63  
-   Gruppo 1: nodi NUMA da 4 a 7, CPU da 64 a 127  
-   Gruppo 2: nodi NUMA da 8 a 12, CPU da 128 a 191  
-   Gruppo 3: nodi NUMA da 13 a 16, CPU da 192 a 255  
  
#### <a name="a-setting-affinity-to-all-cpus-in-groups-0-and-2"></a>R. Impostazione dell'affinità su tutte le CPU nei gruppi 0 e 2  
Nell'esempio seguente viene impostata l'affinità su tutte le CPU nei gruppi 0 e 2.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 63, 128 TO 191;  
```  
  
#### <a name="b-setting-affinity-to-all-cpus-in-numa-nodes-0-and-7"></a>B. Impostazione dell'affinità su tutte le CPU nei nodi NUMA 0 e 7  
Nell'esempio seguente l'affinità delle CPU viene impostata sui nodi `0` e `7`.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY NUMANODE=0, 7;  
```  
  
#### <a name="c-setting-affinity-to-cpus-60-through-200"></a>C. Impostazione dell'affinità sulle CPU da 60 a 200  
Nell'esempio seguente viene impostata l'affinità sulle CPU da 60 a 200.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=60 TO 200;  
```  
  
#### <a name="d-setting-affinity-to-cpu-0-on-a-system-that-has-two-cpus"></a>D. Impostazione dell'affinità sulla CPU 0 in un sistema che dispone di due CPU  
Nell'esempio seguente viene impostata l'affinità su `CPU=0` in un computer che dispone di due CPU. Prima dell'esecuzione dell'istruzione seguente, la maschera di bit di affinità interna è 00.  
  
```sql  
ALTER SERVER CONFIGURATION SET PROCESS AFFINITY CPU=0;  
```  
  
#### <a name="e-setting-affinity-to-auto"></a>E. Impostazione dell'affinità su AUTO  
Nell'esempio seguente l'affinità viene impostata su `AUTO`.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET PROCESS AFFINITY CPU=AUTO;  
```  
  
###  <a name="setting-diagnostic-log-options"></a><a name="Diagnostic"></a> Setting diagnostic log options  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
Negli esempi inclusi in questa sezione viene illustrato come impostare i valori per l'opzione del log di diagnostica.  
  
#### <a name="a-starting-diagnostic-logging"></a>R. Avvio della registrazione dei dati di diagnostica  
Nell'esempio seguente viene avviata la registrazione dei dati di diagnostica.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
#### <a name="b-stopping-diagnostic-logging"></a>B. Arresto della registrazione dei dati di diagnostica  
Nell'esempio seguente viene arrestata la registrazione dei dati di diagnostica.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
#### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. Definizione della posizione dei log di diagnostica  
Nell'esempio seguente viene impostata la posizione dei log di diagnostica sul percorso di file specificato.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
#### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D. Definizione della dimensione massima di ogni log di diagnostica  
Nell'esempio seguente viene impostata su 10 megabyte la dimensione massima di ogni log di diagnostica.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
###  <a name="setting-failover-cluster-properties"></a><a name="Failover"></a> Impostazione delle proprietà del cluster di failover  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
Nell'esempio seguente viene illustrata l'impostazione dei valori delle proprietà della risorsa cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="a-specifying-the-value-for-the-healthchecktimeout-property"></a>R. Impostazione del valore per la proprietà HealthCheckTimeout  
Nell'esempio seguente viene impostata l'opzione `HealthCheckTimeout` su 15.000 millisecondi (15 secondi).  
  
```sql  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
###  <a name="b-changing-the-cluster-context-of-an-availability-replica"></a><a name="ChangeClusterContextExample"></a> B. Modifica del contesto del cluster di una replica di disponibilità  
Nell'esempio seguente viene cambiato il contesto del cluster HADR dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per specificare il cluster WSFC di destinazione, `clus01`, nell'esempio viene specificato il nome completo dell'oggetto cluster, `clus01.xyz.com`.  
  
```sql  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
### <a name="setting-buffer-pool-extension-options"></a>Impostazione delle opzioni di estensione del pool di buffer  
  
####  <a name="a-setting-the-buffer-pool-extension-option"></a><a name="BufferPoolExtension"></a> A. Impostazione dell'opzione di estensione del pool di buffer  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
Nell'esempio seguente viene abilitata l'opzione di estensione del pool di buffer e vengono specificati un nome file e una dimensione.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 50 GB);  
```  
  
#### <a name="b-modifying-buffer-pool-extension-parameters"></a>B. Modifica dei parametri dell'estensione del pool di buffer  
Nell'esempio seguente vengono modificate le dimensioni di un file di estensione del pool di buffer. L'opzione di estensione del pool di buffer deve essere disabilitata prima di poter modificare uno qualsiasi dei parametri.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION OFF;  
GO  
EXEC sp_configure 'max server memory (MB)', 12000;  
GO  
RECONFIGURE;  
GO  
ALTER SERVER CONFIGURATION  
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 60 GB);  
GO   
```  

### <a name="setting-in-memory-database-options"></a><a name="MemoryOptimized"></a> Impostazione delle opzioni del database in memoria

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).

#### <a name="a-enable-all-in-memory-database-features-with-default-options"></a>R. Abilitare tutte le funzionalità del database in memoria con le opzioni predefinite

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED ON;
GO
```

#### <a name="b-enable-memory-optimized-tempdb-metadata-using-the-default-resource-pool"></a>B. Abilitare i metadati tempdb ottimizzati per la memoria usando il pool di risorse predefinito

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
GO
```

#### <a name="c-enable-memory-optimized-tempdb-metadata-with-a-user-defined-resource-pool"></a>C. Abilitare i metadati tempdb ottimizzati per la memoria usando un pool di risorse definito dall'utente

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
GO
```

#### <a name="d-enable-hybrid-buffer-pool"></a>D. Abilitare il pool di buffer ibrido

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
GO
```

## <a name="see-also"></a>Vedere anche  
[Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)   
[Modificare il contesto del cluster HADR dell'istanza del server &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)   
[sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)   
[sys.dm_os_memory_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md)   
[sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)   
[Estensione pool di buffer](../../database-engine/configure-windows/buffer-pool-extension.md)  
  
