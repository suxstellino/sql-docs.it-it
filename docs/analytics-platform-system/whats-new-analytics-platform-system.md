---
title: Novità
description: Vedere Novità di piattaforma di strumenti analitici Microsoft, un'appliance locale con scalabilità orizzontale che ospita MPP SQL Server data warehouse parallele.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 004550d95ca69bae45c518b8054886e635e829df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091784"
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>Novità del sistema di piattaforma di analisi, un MPP con scalabilità orizzontale data warehouse
Vedere Novità degli aggiornamenti più recenti degli appliance per piattaforma di strumenti analitici Microsoft (APS). APS è un'appliance locale con scalabilità orizzontale che ospita MPP SQL Server data warehouse parallele. 

::: moniker range=">= aps-pdw-2016-au7 "
<a name="h2-aps-cu7.7"></a>
## <a name="aps-cu77"></a>APS CU7.7
Data di rilascio-2020 novembre

### <a name="scvmm2016"></a>SCVMM2016
APS CU 7,7 software aggiorna la macchina virtuale VMM a Windows Server 2016 e installa SCVMM2016. SCVMM 2012 R2 attualmente in uso ha una data di fine vita del 2022 luglio. Il SCVMM più recente è necessario per supportare la creazione di un aggiornamento obbligatorio di CU 7,7. I clienti sono invitati a eseguire l'aggiornamento a CU 7,7 il prima possibile.

### <a name="ssis-destination-adapter-for-sql-server-2019-as-target"></a>Adapter di destinazione SSIS per SQL Server 2019 come destinazione
È possibile [scaricare dalla pagina di download](https://www.microsoft.com/download/details.aspx?id=57472)un nuovo adattatore di destinazione SSIS APS che supporta SQL Server 2019 come destinazione di distribuzione.

<a name="h2-aps-cu7.6"></a>
## <a name="aps-cu76"></a>APS CU7.6
Data di rilascio-aprile 2020

### <a name="rename-column"></a>Rename Column (Rinomina colonna)
Dopo l'aggiornamento a CU 7.6, i clienti potranno rinominare una colonna di una tabella creata dall'utente. Vedere [Rename (Transact-SQL)](../t-sql/statements/rename-transact-sql.md) per sintassi, esempi, limitazioni e altre informazioni.

### <a name="alter-view"></a>Alter View
I clienti saranno ora in grado di modificare le visualizzazioni. Per ulteriori informazioni, vedere [ALTER View (Transact-SQL)](../t-sql/statements/alter-view-transact-sql.md) .

<a name="h2-aps-cu7.5"></a>
## <a name="aps-cu75"></a>APS CU7.5
Data di rilascio-2019 settembre

### <a name="alter-external-data-source"></a>Alter External Data Source
I clienti potranno modificare la definizione dell'origine dati esterna con l'aggiornamento CU 7.5. I clienti con disponibilità elevata del nodo nome Hadoop possono ora modificare l'origine dati per modificare gli argomenti quando si verifica un failover. Per gli APS è possibile modificare solo il percorso, RESOURCE_MANAGER_LOCATION e le credenziali. Per ulteriori informazioni, vedere [ALTER External Data Source](../t-sql/statements/alter-external-data-source-transact-sql.md?view=sql-server-2017&preserve-view=true) .

### <a name="cdh-515-and-516-support-with-polybase"></a>Supporto di CDH 5,15 e 5,16 con polibase
La polibase su APS con aggiornamento CU 7.5 supporta ora le versioni CDH 5,15 e 5,16 della distribuzione Hadoop da Cloudera. Usare l'opzione 6 per le versioni CDH 5. x. 

### <a name="try_convert-and-try_cast-support"></a>Supporto di Try_Convert e Try_Cast
CU 7.5 APS supporta ora [try_cast](../t-sql/functions/try-cast-transact-sql.md?view=sql-server-2017&preserve-view=true) e [TRY_CONVERT](../t-sql/functions/try-convert-transact-sql.md?view=sql-server-2017&preserve-view=true) le funzioni TSQL. Entrambe le funzioni restituiscono un valore convertito nel tipo di dati specificato se la conversione ha esito positivo; in caso contrario, restituisce null.

<a name="h2-aps-cu7.4"></a>
## <a name="aps-cu74"></a>APS CU7.4
Data di rilascio: maggio 2019

### <a name="loading-large-rows-with-dwloader"></a>Caricamento di righe di grandi dimensioni con dwloader
A partire da APS CU 7.4, i clienti saranno in grado di usare un nuovo dwloader per caricare le righe in tabelle di dimensioni superiori a 32 KB (32.768 byte). Il nuovo dwloader supporta l'opzione-l che accetta un valore intero compreso tra 32768 e 33554432 (in byte) per caricare righe maggiori di 32 KB. Usare questa opzione solo quando si caricano righe di grandi dimensioni (superiori a 32 KB) perché questa opzione consente di allocare più memoria nel client e nel server e potrebbe rallentare i carichi. È possibile scaricare il nuovo dwloader dal [sito di download](https://www.microsoft.com/download/details.aspx?id=57472).  

### <a name="hdp-30-and-31-support-with-polybase"></a>Supporto di HDP 3,0 e 3,1 con polibase
La polibase su punti di APS supporta ora HDP 3,0 e 3,1 con questo aggiornamento. Usare l'opzione 7 per le versioni HDP 3. x. Per ulteriori informazioni, vedere la pagina relativa alla [connettività di base](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) .

### <a name="utf16-file-support-with-polybase"></a>Supporto file UTF16 con polibase
La codebase ora supporta la lettura di file di testo delimitati presenti nella codifica UTF16 (LE). Per informazioni dettagliate sull'installazione, vedere [creare un formato di file esterno](../t-sql/statements/create-external-file-format-transact-sql.md) . 

<a name="h2-aps-cu7.3"></a>
## <a name="aps-cu73"></a>APS CU7.3
Data di rilascio-2018 dicembre

### <a name="common-subexpression-elimination"></a>Eliminazione di sottoespressioni comuni
APS CU 7.3 migliora le prestazioni delle query con l'eliminazione della sottoespressione comune in SQL Query Optimizer. Il miglioramento migliora le query in due modi. Il primo vantaggio è la possibilità di identificare ed eliminare tali espressioni per ridurre il tempo di compilazione SQL. Il secondo e più importante vantaggio è che le operazioni di spostamento dei dati per queste sottoespressioni ridondanti vengono eliminate, quindi il tempo di esecuzione per le query diventa più veloce. Una spiegazione dettagliata di questa funzionalità è disponibile [qui](common-sub-expression-elimination.md).

### <a name="aps-informatica-connector-for-informatica-1020-published"></a>Connettore APS informatica per informatica 10.2.0 pubblicato
È stata rilasciata una nuova versione dei connettori informatica per APS che funziona con informatica versione 10.2.0 e 10.2.0 hotfix 1. I nuovi connettori possono essere scaricati dal [sito di download](https://www.microsoft.com/download/details.aspx?id=57472).
> [!NOTE]
> APS informatica Connector per informatica 10.2.0 o 10.2.0 hotfix 1 non funziona su Strict TLS 1.2 e richiede che TLS 1.0 e 1,1 siano completamente funzionanti.

#### <a name="supported-versions"></a>Versioni supportate

| Versione di APS | Informatica PowerCenter | Driver |
|:---|:---|:---|
| APS 2016 | 9.6.1 | SQL Server Native Client 11. x |
| APS 2016 e versioni successive | 10.2.0, hotfix 10.2.0 1 | SQL Server Native Client 11. x |

<a name="h2-aps-cu7.2"></a>
## <a name="aps-cu72"></a>APS CU7.2
Data di rilascio-2018 ottobre

### <a name="support-for-tls-12"></a>Supporto per TLS 1.2
APS CU 7.2 supporta TLS 1,2. È ora possibile impostare la comunicazione tra il computer client e la comunicazione tra nodi APS e APS per comunicare solo tramite TLS 1.2. Strumenti come SSDT, SSIS e Dwloader installati nei computer client impostati per la comunicazione solo tramite TLS 1,2 possono ora connettersi ad APS usando TLS 1,2. Per impostazione predefinita, gli APS supporteranno tutte le versioni di TLS (1,0, 1,1 e 1,2) per la compatibilità con le versioni precedenti. Se si vuole impostare il dispositivo APS in modo da usare esclusivamente TLS 1,2, è possibile modificare le impostazioni del registro di sistema. 

Per altre informazioni, vedere [Configuring TLS 1.2 on APS](configure-tls12-aps.md).

### <a name="hadoop-encryption-zone-support-for-polybase"></a>Supporto per la zona di crittografia Hadoop per polibase
La codebase ora può comunicare con le zone di crittografia Hadoop. Vedere modifiche di configurazione di APS necessarie in [configurare la sicurezza di Hadoop](polybase-configure-hadoop-security.md#encryptionzone).

### <a name="insert-select-maxdop-options"></a>Opzioni di Insert-Select MAXDOP
È stata aggiunta un' [opzione di funzionalità](appliance-feature-switch.md) che consente di selezionare le impostazioni MAXDOP maggiori di 1 per le operazioni di inserimento e selezione. È ora possibile impostare l'impostazione MAXDOP su 0, 1, 2 o 4. Il valore predefinito è 1.

> [!IMPORTANT]  
> L'aumento di MAXDOP può a volte causare operazioni più lente o errori di deadlock. In tal caso, modificare di nuovo l'impostazione in MAXDOP 1, quindi ripetere l'operazione.

### <a name="columnstore-index-health-dmv"></a>DMV sull'integrità dell'indice columnstore
È possibile visualizzare le informazioni sull'integrità degli indici columnstore utilizzando **dm_pdw_nodes_db_column_store_row_group_physical_stats** DMV. Usare la vista seguente per determinare la frammentazione e decidere quando ricompilare o riorganizzare un indice columnstore.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[pdw_node_id]    = nt.[pdw_node_id]                                          
)
select *
from cte;
```

### <a name="polybase-date-range-increase-for-orc-and-parquet-files"></a>Incremento dell'intervallo di date di base per i file ORC e parquet
La lettura, l'importazione e l'esportazione di tipi di dati di data tramite la polibase supporta ora le date precedenti al 1970-01-01 e dopo 2038-01-20 per i tipi di file ORC e parquet.

### <a name="ssis-destination-adapter-for-sql-server-2017-as-target"></a>Adapter di destinazione SSIS per SQL Server 2017 come destinazione
È possibile [scaricare dalla pagina di download](https://www.microsoft.com/download/details.aspx?id=57472)un nuovo adattatore di destinazione SSIS APS che supporta SQL Server 2017 come destinazione di distribuzione.

<a name="h2-aps-cu7.1"></a>
## <a name="aps-cu71"></a>APS CU7.1
Data di rilascio-2018 luglio

### <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>I comandi DBCC non utilizzano gli slot di concorrenza (modifica del comportamento)
APS supporta un subset dei [comandi DBCC](../t-sql/database-console-commands/dbcc-transact-sql.md) T-SQL, ad esempio [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md). In precedenza, questi comandi avrebbero utilizzato uno [slot di concorrenza](./workload-management.md?view=aps-pdw-2016-au7&preserve-view=true&#concurrency-slots) riducendo il numero di carichi/query utente che poteva essere eseguito. I `DBCC` comandi vengono ora eseguiti in una coda locale che non utilizzano uno slot di concorrenza utente per migliorare le prestazioni complessive dell'esecuzione delle query.

### <a name="replaces-some-metadata-calls-with-catalog-objects"></a>Sostituisce alcune chiamate di metadati con oggetti catalogo
L'utilizzo di oggetti del catalogo per le chiamate ai metadati anziché l'utilizzo di SMO ha mostrato un miglioramento delle prestazioni in APS. A partire da CU 7.1, alcune di queste chiamate di metadati utilizzano ora gli oggetti catalogo per impostazione predefinita. Questo comportamento può essere disattivato dal [commutatore di funzionalità](appliance-feature-switch.md) se i clienti che usano query di metadati si verificano in caso di problemi.

### <a name="bug-fixes"></a>Correzioni di bug
È stato eseguito l'aggiornamento a SQL Server 2016 SP2 CU2 con APS CU 7.1. L'aggiornamento corregge alcuni problemi descritti di seguito.

| Titolo | Descrizione |
|:---|:---|
| **Potenziale deadlock del motore di Tuple** |L'aggiornamento corregge una possibilità di deadlock a lungo termine in una transazione distribuita e in un thread in background del motore di Tuple. Dopo aver installato CU 7.1, i clienti che hanno usato TF634 per arrestare il motore di tupla come SQL Server parametro di avvio o un flag di traccia globale possono rimuoverlo in modo sicuro. | 
| **Una determinata query di ritardo/lead ha esito negativo** |Alcune query sulle tabelle CCI con funzioni lag/lead annidate che potrebbero avere un errore sono ora corrette con questo aggiornamento. | 


<a name="h2-aps-au7"></a>
## <a name="aps-au7"></a>APS AU7
Data di rilascio: maggio 2018

APS 2016 è un prerequisito per l'aggiornamento a AU7. Di seguito sono riportate le nuove funzionalità di APS AU7:

### <a name="auto-create-and-auto-update-statistics"></a>Creazione automatica e aggiornamento automatico delle statistiche
APS AU7 crea e aggiorna automaticamente le statistiche, per impostazione predefinita. Per aggiornare le impostazioni delle statistiche, gli amministratori possono utilizzare una nuova voce di menu opzioni funzionalità nel [Configuration Manager](appliance-configuration.md#CMTasks). L' [opzione della funzionalità](appliance-feature-switch.md) controlla il comportamento di creazione automatica, aggiornamento automatico e aggiornamento asincrono delle statistiche. È inoltre possibile aggiornare le impostazioni delle statistiche con l'istruzione [ALTER database (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) .

### <a name="t-sql"></a>T-SQL
Select @var è ora supportato. Per altre informazioni, vedere [selezionare una variabile locale](../t-sql/language-elements/select-local-variable-transact-sql.md) 

Sono ora supportati il gruppo ORDER e HASH degli hint per la query. Per altre informazioni, vedere [hint (Transact-SQL)-query](../t-sql/queries/hints-transact-sql-query.md)

### <a name="feature-switch"></a>Opzione funzionalità
APS AU7 introduce un'opzione di funzionalità in [Configuration Manager](launch-the-configuration-manager.md). AutoStatsEnabled e DmsProcessStopMessageTimeoutInSeconds sono ora opzioni configurabili che possono essere modificate dagli amministratori.

### <a name="known-issues"></a>Problemi noti
Con il software APS AU7, viene fornito un aggiornamento del BIOS Intel che corregge un problema descritto come *attacchi speculativi per il canale laterale di esecuzione*. Gli attacchi mirano a sfruttare le cosiddette *vulnerabilità di Spectre e Meltdown*. Sebbene sia incluso in un pacchetto con APS, l'aggiornamento del BIOS viene installato manualmente e non come parte dell'installazione del software APS AU7.

Microsoft consiglia a tutti i clienti di installare il BIOS aggiornato. Microsoft ha misurato l'effetto dello shadowing degli indirizzi virtuali (KVAS) kernel, della tabella delle pagine kernel (KPTI) e del IBP (Indirect Branch Prediction Mitigation) su vari carichi di lavoro SQL in diversi ambienti. Le misurazioni hanno rilevato un calo significativo per alcuni carichi di lavoro. In base ai risultati, si consiglia di testare l'effetto sulle prestazioni dell'abilitazione dell'aggiornamento del BIOS prima di distribuirli in un ambiente di produzione. Vedere le istruzioni SQL Server [qui](https://support.microsoft.com/help/4073225/guidance-protect-sql-server-against-spectre-meltdown).

::: moniker-end
::: moniker range=">= aps-pdw-2016 "
<a name="h2-aps-au6"></a>
## <a name="aps-2016"></a>APS 2016
Questa sezione descrive le nuove funzionalità per APS 2016-AU6.

### <a name="sql-server-2016"></a>SQL Server 2016

APS AU6 viene eseguito nella versione più recente di SQL Server 2016 e usa il livello di compatibilità del database predefinito 130. SQL Server 2016 Abilita il supporto per le nuove funzionalità, ad esempio:

- Indici secondari per gli indici columnstore cluster.
- Kerberos per la polibase.

### <a name="t-sql"></a>T-SQL
APS AU6 supporta i miglioramenti per la compatibilità con T-SQL.  Questi elementi del linguaggio aggiuntivi semplificano la migrazione da SQL Server e da altre origini dati. 

- Sono ora supportate le [regole di confronto SQL a livello di colonna][] , oltre alle regole di confronto di Windows.
- Gli [indici non cluster sugli indici columnstore cluster][] migliorano le prestazioni delle query che cercano valori specifici nell'indice columnstore cluster. 
- [SELECT...INTO][] 
- [sp_spaceused ()][] consente di visualizzare lo spazio su disco utilizzato o riservato in una tabella o un database.
- Il supporto per le [tabelle estese][] è uguale a quello SQL Server 2016. Il limite precedente di 32 K per le dimensioni della riga non esiste più. 

**Tipi di dati**

- [Varchar (max)][], [nvarchar (max)][] e [varbinary (max)][]. Questi tipi di dati LOB hanno una dimensione massima di 2 GB. Per caricare questi oggetti, utilizzare l' [utilità bcp][]. I tipi di dati di polibase e dwloader non sono attualmente supportati. 
- [SYSNAME][]
- [UNIQUEIDENTIFIER][]
- Tipi di dati [numerici][] e decimali.

**Funzioni finestra**

- [Righe o intervalli][] nella clausola over dell'istruzione SELECT.
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**Funzioni di sicurezza**

- [Checksum ()][] e [BINARY_CHECKSUM ()][]
- [HAS_PERMS_BY_NAME ()][]

**Funzioni aggiuntive**

- [NEWID ()][]
- [RAND ()][]

### <a name="polybasehadoop-enhancements"></a>Miglioramenti di polibase/Hadoop

- Compatibilità con Hortonworks HDP 2,4 e HDP 2,5
- Supporto di Kerberos tramite credenziali con ambito database
- Supporto delle credenziali con i BLOB di archiviazione di Azure

### <a name="install-and-upgrade-enhancements"></a>Miglioramenti per l'installazione e l'aggiornamento

**Aggiornamenti dell'architettura aziendale** L'aggiornamento dell'appliance esistente ad APS AU6 installa gli aggiornamenti del firmware e del driver più recenti, che includono correzioni per la sicurezza. 

Un nuovo appliance di HPE o DELL include tutti gli aggiornamenti più recenti, oltre a:

- Supporto del processore di generazione più recente (Broadwell)
- Aggiornamento ai moduli DIMM DDR4
- Velocità effettiva DIMM migliorata

**Integrazione**

- Il supporto per il nome di dominio completo (FQDN) rende possibile la configurazione di un trust di dominio per l'appliance. 
- Per usare il nome di dominio completo, è necessario eseguire un aggiornamento completo e acconsentire esplicitamente durante l'aggiornamento. 

**Tempi di inattività ridotti** L'installazione o l'aggiornamento ad APS AU6 è più veloce e richiede meno tempo di inattività rispetto alle versioni precedenti. Per ridurre i tempi di inattività, l'installazione o l'aggiornamento: 

 - Semplifica l'applicazione degli aggiornamenti WSUS usando un'immagine che contiene tutti gli aggiornamenti fino al 2016 giugno
 - Applica gli aggiornamenti della sicurezza con gli aggiornamenti del driver e del firmware
 - Inserisce gli hotfix più recenti e l'utilità di verifica dell'appliance (PAV) nell'appliance in modo che siano pronti per l'installazione senza che sia necessario scaricarli.

::: moniker-end

<!--
Link references to other articles in this same GitHub repo.

The link format that starts with '/sql/what-ever/my-artlcle' is not appropriate for common links within the same repo (as most of these link are).  The first couple links have been edited to show the proper syntax, but all other links in this article need to be similarly edited.
The proper formats have at least two big advantages.  One big advantage is that the proper formats enable the OPS Build system to detect broken links at Pull Request build time, instead of only later during run time.
-->
[database compatibility level 130]: ../t-sql/statements/alter-database-transact-sql-compatibility-level.md
[Regole di confronto SQL a livello di colonna]: ~/relational-databases/collations/collation-and-unicode-support.md

[Indici non cluster sugli indici columnstore cluster]:/sql/t-sql/statements/create-index-transact-sql
[VARCHAR (MAX)]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[NVARCHAR (MAX)]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[VARBINARY (MAX)]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[SYSNAME]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[SELECT...INTO]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused ()]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[Tabelle estese]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[Utilità bcp]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[ROWS o RANGE]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[CHECKSUM ()]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM ()]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME ()]:/sql/t-sql/functions/has-perms-by-name-transact-sql
[NEWID ()]:/sql/t-sql/functions/newid-transact-sql
[RAND ()]:/sql/t-sql/functions/rand-transact-sql


  

