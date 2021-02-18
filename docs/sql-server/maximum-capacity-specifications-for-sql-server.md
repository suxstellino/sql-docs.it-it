---
title: Specifiche di capacità massima per SQL Server
description: Questo articolo mostra le dimensioni e i numeri massimi dei diversi oggetti definiti nei componenti di SQL Server, oltre a informazioni aggiuntive.
ms.date: 03/05/2020
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- objects [SQL Server]
- number capacity specifications [SQL Server]
- size [SQL Server], capacity specifications
- number of objects
- capacity specifications [SQL Server]
- maximum capacity specifications [SQL Server]
- size [SQL Server]
- replication capacity specifications [SQL Server]
- objects [SQL Server], capacity specifications
- Database Engine [SQL Server], capacity specifications
ms.assetid: 13e95046-0e76-4604-b561-d1a74dd824d7
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: c0d0da7b2f74eb9dacdef390ede2cdd34d0356fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336023"
---
# <a name="maximum-capacity-specifications-for-sql-server"></a>Specifiche di capacità massima per SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Questo articolo mostra le dimensioni e i numeri massimi dei diversi oggetti definiti nei componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

>[!NOTE]
>Oltre alle informazioni contenute in questo articolo, possono essere utili anche le informazioni nei collegamenti seguenti:
>
>* [Download di SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
>* [Requisiti hardware e software per l'installazione di SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
>* [Parametri di controllo di Controllo configurazione sistema](../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)
>

## <a name="ssde-objects"></a>Oggetti [!INCLUDE[ssDE](../includes/ssde-md.md)]

La tabella seguente indica le dimensioni e i numeri massimi dei diversi oggetti definiti nei database di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o a cui si fa riferimento nelle istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] .

|Oggetto [!INCLUDE[ssDE](../includes/ssde-md.md)] di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Quantità/dimensioni massime [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64 bit)|Informazioni aggiuntive|
|---------------------------------------------------------|------------------------------------------------------------------|----------------------------|
|Dimensioni dei batch|65.536 <sup>*</sup> (Dimensioni del pacchetto di rete)|Le dimensioni del pacchetto di rete indicano le dimensioni dei pacchetti TDS (Tabular Data Stream) usati per le comunicazioni tra le applicazioni e il [!INCLUDE[ssDE](../includes/ssde-md.md)] relazionale. La dimensione predefinita del pacchetto è 4 KB e viene controllata dall'opzione di configurazione delle dimensioni del pacchetto di rete.|
|Byte per ogni colonna di stringhe brevi|8\.000||
|Byte per `GROUP BY`, `ORDER BY`|8\.060||
|Byte per ogni chiave di indice|900 byte per un indice cluster. 1700 per un indice non cluster.|Il numero massimo di byte in una chiave di indice cluster non può essere maggiore di 900 in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per una chiave di indice non cluster, il valore massimo è 1700 byte.<br /><br /> È possibile definire una chiave usando colonne a lunghezza variabile le cui dimensioni massime superano il limite. Tuttavia, le dimensioni combinate dei dati di tali colonne non possono mai superare il limite.<br /><br /> In un indice non cluster, è possibile includere colonne non chiave aggiuntive che non vengono incluse nel conteggio per il limite di dimensioni della chiave. Le colonne non chiave potrebbero migliorare le prestazioni di alcune query.|
|Byte per ogni chiave di indice per tabelle ottimizzate per la memoria|2500 byte per un indice non cluster. Nessun limite per un indice hash purché tutte le chiavi di indice rientrino nella riga.|In una tabella ottimizzata per la memoria, un indice non cluster non può contenere colonne chiave le cui dimensioni massime dichiarate superano i 2500 byte. È irrilevante se i dati effettivi nelle colonne chiave sono minori delle dimensioni massime dichiarate.<br /><br /> Per una chiave di indice hash non esiste alcun limite fisico alle dimensioni.<br /><br /> Per gli indici delle tabelle ottimizzate per la memoria, non è disponibile alcun concetto di colonne incluse poiché tutti gli indici coprono implicitamente tutte le colonne.<br /><br /> Per una tabella ottimizzata per la memoria, anche se le dimensioni delle righe sono di 8060 byte, alcune colonne a lunghezza variabile possono essere fisicamente archiviate all'esterno di tali 8060 byte. Tuttavia, le dimensioni massime dichiarate di tutte le colonne chiave per tutti gli indici in una tabella, oltre a eventuali colonne a lunghezza fissa aggiuntive nella tabella, non devono superare 8060 byte.|
|Byte per ogni chiave esterna|900||
|Byte per ogni chiave primaria|900||
|Byte per ogni riga|8\.060|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta l'archiviazione dei dati di overflow della riga che consente di spostare colonne di lunghezza variabile all'esterno delle righe. Nel record principale viene archiviata solo una radice di 24 byte per le colonne di lunghezza variabile spostate all'esterno delle righe. Questa funzionalità consente un limite che è effettivamente superiore rispetto alle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Supporto per righe di grandi dimensioni](../relational-databases/pages-and-extents-architecture-guide.md#large-row-support).|
|Byte per ogni riga nelle tabelle ottimizzate per la memoria|8\.060|A partire da [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] le tabelle ottimizzate per la memoria supportano l'archiviazione all'esterno delle righe. Le colonne a lunghezza variabile vengono spostate all'esterno delle righe se le dimensioni massime di tutte le colonne nella tabella superano 8060 byte. Questa azione è una decisione in fase di compilazione. Per le colonne archiviate all'esterno delle righe viene archiviato un solo riferimento a 8 byte. Per altre informazioni, vedere [Dimensioni di tabelle e righe per le tabelle con ottimizzazione per la memoria](../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md).|
|Byte nel testo di origine di una stored procedure|Minore delle dimensioni batch o 250 MB||
|Byte per ogni colonna `varchar(max) `, `varbinary(max)`, `xml`, `text` o `image`|2^31-1||
|Caratteri per ogni colonna `ntext` o `nvarchar(max)`|2^30-1||
|Indici cluster per tabella|1||
|Colonne in `GROUP BY`, `ORDER BY`|Limitato solo dal numero di byte||
|Colonne o espressioni in un'istruzione `GROUP BY WITH CUBE` o `WITH ROLLUP`|10||
|Colonne per ogni chiave di indice|32|Se la tabella contiene uno o più indici XML, la chiave di clustering della tabella utente viene limitata a 31 colonne perché la colonna XML viene aggiunta alla chiave di clustering dell'indice XML primario. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è possibile includere colonne non chiave in un indice non cluster per evitare di raggiungere il limite massimo di 32 colonne chiave. Per altre informazioni, vedere [Creare indici con colonne incluse](../relational-databases/indexes/create-indexes-with-included-columns.md).|
|Colonne per ogni chiave esterna o primaria|32||
|Colonne per ogni istruzione `INSERT`|4\.096||
|Colonne per ogni istruzione `SELECT`|4\.096||
|Colonne per ogni tabella|1\.024|Le tabelle con set di colonne di tipo sparse includono fino a 30.000 colonne. Vedere le informazioni sui [set di colonne di tipo sparse](../relational-databases/tables/use-column-sets.md).|
|Colonne per ogni istruzione `UPDATE`|4\.096|Ai [set di colonne di tipo sparse](../relational-databases/tables/use-column-sets.md) vengono applicati limiti diversi.|
|Colonne per ogni vista|1\.024||
|Connessioni per ogni client|Valore massimo delle connessioni configurate||
|Dimensioni del database|524.272 terabytes||
|Database per ogni istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|32.767||
|Filegroup per ogni database|32.767||
|Filegroup per ogni database per dati ottimizzati per la memoria|1||
|File per ogni database|32.767||
|Dimensioni di file (dati)|16 terabyte||
|Dimensioni del file (log)|2 terabyte||
|File di dati per dati ottimizzati per la memoria per ogni database|4\.096 in [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)]. Le versioni successive di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non impongono un limite rigido di questo tipo.||
|File differenziale per ogni file di dati per dati ottimizzati per la memoria|1||
|Riferimenti alla tabella della chiave esterna per ogni tabella|In uscita = 253. In ingresso = 10.000.|Per informazioni sulle restrizioni, vedere [Create Foreign Key Relationships](../relational-databases/tables/create-foreign-key-relationships.md).|
|Lunghezza di identificatore (in caratteri)|128||
|Istanze per ogni computer|50 istanze in un server autonomo.<br /><br />25 istanze del cluster di failover quando si usano dischi di cluster condivisi come archiviazione.<br/><br/>50 istanze del cluster di failover con condivisioni file SMB come opzione di archiviazione.||
|Indici per ogni tabella ottimizzata per la memoria|999 avvio [!INCLUDE[ssSQL17](../includes/ssSQL17-md.md)] e in [!INCLUDE[ssSDSFull](../includes/ssSDSFull-md.md)]<br/>8 in [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)] e [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]||
|Lunghezza di una stringa contenente istruzioni SQL (dimensioni batch)|65.536 (dimensioni del pacchetto di rete)|Le dimensioni del pacchetto di rete indicano le dimensioni dei pacchetti TDS (Tabular Data Stream) usati per le comunicazioni tra le applicazioni e il [!INCLUDE[ssDE](../includes/ssde-md.md)] relazionale. La dimensione predefinita del pacchetto è 4 KB e viene controllata dall'opzione di configurazione delle dimensioni del pacchetto di rete.|
|Blocchi per ogni connessione|Numero massimo di blocchi per ogni server||
|Blocchi per ogni istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Limitato solo dalla memoria|Questo valore si riferisce all'allocazione di blocchi statici. I blocchi dinamici sono limitati solo dalla memoria.|
|Livelli di nidificazione delle stored procedure|32|Se una stored procedure accede a più di 64 database o a più di due database in interfoliazione, verrà generato un errore.|
|Sottoquery nidificate|32||
|Transazioni nidificate|4\.294.967.296|| 
|Livelli di nidificazione dei trigger|32||
|Indici non cluster per tabella|999||
|Numero di espressioni distinte nella clausola `GROUP BY` quando è presente una delle seguenti opzioni: `CUBE`, `ROLLUP`, `GROUPING SETS`, `WITH CUBE`, `WITH ROLLUP`|32||
|Numero di set di raggruppamento generati dagli operatori nella clausola `GROUP BY`|4\.096||
|Parametri per ogni stored procedure|2\.100||
|Parametri per ogni funzionalità definita dall'utente|2\.100||
|REFERENCES per ogni tabella|253||
|Righe per ogni tabella|Limitato dall'archiviazione disponibile||
|Tabelle per ogni database|Limitato dal numero totale di oggetti di un database|Gli oggetti includono tabelle, viste, stored procedure, funzioni definite dall'utente, trigger, regole, impostazioni predefinite e vincoli. La somma del numero di tutti gli oggetti in un database non può essere maggiore di 2.147.483.647.|
|Partizioni per ogni tabella o indice partizionato|15.000||
|Statistiche relative a colonne non indicizzate|30.000|| 
|Tabelle per ogni istruzione `SELECT`|Limitato solo dalle risorse disponibili||
|Trigger per ogni tabella|Limitato dal numero di oggetti di un database|Gli oggetti includono tabelle, viste, stored procedure, funzioni definite dall'utente, trigger, regole, impostazioni predefinite e vincoli. La somma del numero di tutti gli oggetti in un database non può essere maggiore di 2.147.483.647.|
|Connessioni utente|32.767||
|Indici XML|249||

## <a name="ssnoversion-utility-objects"></a>Oggetti Utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Dimensioni e numeri massimi dei vari oggetti testati nell'Utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Oggetto Utilità|Quantità/dimensioni massime [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64 bit)|
|----------------------------------------------|------------------------------------------------------------------|
|Computer (computer fisici o macchine virtuali) per Utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|100|
|Istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per ogni computer|5|
|Numero complessivo di istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per Utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|200<sup>*</sup>|
|Database utente per ogni istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], incluse applicazioni livello dati|50|
|Numero complessivo di database utente per Utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|1\.000|
|Filegroup per ogni database|1|
|File di dati per filegroup|1|
|File di log per ogni database|1|
|Volumi per ogni computer|3|

<sup>*</sup> Il numero massimo di istanze gestite di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supportate da Utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può variare in base alla configurazione hardware del server. Per informazioni introduttive, vedere [Attività e funzionalità di Utilità SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md). [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non è disponibile in tutte le edizione di [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vedere [funzionalità supportate dalle edizioni di SQL Server 2019](./editions-and-components-of-sql-server-version-15.md), [funzionalità supportate dalle edizioni di SQL Server 2017](./editions-and-components-of-sql-server-2017.md)o [funzionalità supportate dalle edizioni di SQL Server 2016](./editions-and-components-of-sql-server-2016.md).

## <a name="ssnoversion-data-tier-application-objects"></a>Oggetti applicazione livello dati [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Dimensioni e numeri massimi di oggetti diversi testati nelle applicazioni livello dati di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Oggetto applicazione livello dati|Quantità/dimensioni massime [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64 bit)|
|------------------------------------------|------------------------------------------------------------------|
|Database per ogni applicazione livello dati|1|
|Oggetti per ogni applicazione livello dati <sup>*</sup>|Limitati dal numero di oggetti in un database o dalla memoria disponibile.|

<sup>*</sup> I tipi di oggetti inclusi nel limite sono utenti, tabelle, viste, stored procedure, funzioni definite dall'utente, tipi di dati definiti dall'utente, ruoli di database, schemi e tipi di tabella definiti dall'utente.

## <a name="replication-objects"></a>Oggetti di replica

Dimensioni e numeri massimi dei vari oggetti definiti nella replica di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Oggetto replica|Dimensioni/numeri massimi per SQL Server (64 bit)|
|--------------------------------------------------|---------------------------------------------------|
|Articoli (pubblicazione di tipo merge)|2048|
|Articoli (pubblicazione snapshot o transazionale)|32.767|
|Colonne in una tabella<sup>*</sup> (pubblicazione di tipo merge)|246|
|Colonne in una tabella<sup>**</sup> (pubblicazione snapshot o transazionale di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)])|1\.000|
|Colonne in una tabella<sup>**</sup> (pubblicazione snapshot o transazionale di Oracle)|995|
|Byte per una colonna utilizzata in un filtro di riga (pubblicazione di tipo merge)|1\.024|
|Byte per una colonna utilizzata in un filtro di riga (pubblicazione snapshot o transazionale)|8\.000|

<sup>*</sup>Se si usa il rilevamento a livello di riga per il rilevamento dei conflitti (impostazione predefinita), la tabella di base può includere fino a 1.024 colonne, che devono tuttavia essere filtrate dall'articolo in modo da pubblicare un massimo di 246 colonne. Se viene utilizzato il rilevamento a livello di colonna, nella tabella di base possono essere incluse al massimo 246 colonne.

<sup>**</sup>La tabella di base può includere il numero massimo di colonne consentito nel database di pubblicazione (1.024 per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]), ma le colonne devono essere filtrate dall'articolo se superano il numero specificato per il tipo di pubblicazione.

## <a name="see-also"></a>Vedere anche

[Attività e funzionalità di Utilità SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md)