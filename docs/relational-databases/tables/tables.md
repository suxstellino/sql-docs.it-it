---
description: Tabelle
title: Tabelle | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- tables [SQL Server]
- table components [SQL Server]
ms.assetid: 82d7819c-b801-4309-a849-baa63083e83f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db500738dbd328647d25f5c79ca533da081fc9b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181512"
---
# <a name="tables"></a>Tabelle
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

Le tabelle sono oggetti di database che contengono tutti i dati presenti in un database. Nelle tabelle, i dati sono organizzati in modo logico in righe e colonne in un formato simile a quello di un foglio di calcolo. Ogni riga rappresenta un record univoco e ogni colonna rappresenta un campo all'interno del record. Ad esempio, una tabella che include i dati dei dipendenti di un'azienda può contenere una riga per ogni dipendente e colonne che rappresentano i dettagli dei dipendenti quali l'ID, il nome, l'indirizzo, la posizione e il numero di telefono dell'abitazione. 

- Il numero di tabelle presenti in un database è limitato solo dal numero di oggetti disponibili in un database (2,147,483,647). Una tabella standard definita dall'utente può avere fino a 1.024 colonne. Il numero di righe nella tabella è limitato solo dalla capacità di archiviazione del server. 

- È possibile assegnare proprietà alla tabella e a ogni colonna nella tabella per controllare i dati consentiti e le altre proprietà. Ad esempio, è possibile creare vincoli in una colonna per impedire valori null o fornire un valore predefinito se non è specificato un valore oppure è possibile assegnare un vincolo principale sulla tabella che applica l'univocità o definisce una relazione tra tabelle. 

- I dati nella tabella possono essere compressi per riga o per pagina. La compressione dei dati può consentire l'archiviazione di più righe su una pagina. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md). 

## <a name="types-of-tables"></a>Tipi di tabelle
 Oltre al ruolo standard delle tabelle di base definite dall'utente, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili i tipi di tabelle seguenti per scopi specifici in un database: 

### <a name="partitioned-tables"></a>Tabelle partizionate

Nelle tabelle partizionate, i dati vengono suddivisi orizzontalmente in unità che possono essere distribuite in più filegroup di un database. Il partizionamento semplifica la gestione di tabelle o indici di grandi dimensioni in quanto consente di gestire o accedere a subset di dati in modo rapido ed efficace mantenendo l'integrità della raccolta. Per impostazione predefinita, [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supporta fino a 15.000 partizioni. Per ulteriori informazioni, vedere [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

### <a name="temporary-tables"></a>Tabelle temporanee

Le tabelle temporanee vengono archiviate in **tempdb**. Esistono due tipi di tabelle temporanee, ovvero le tabelle locali e le tabelle globali. I due tipi differiscono per i nomi, la visibilità e la disponibilità. Le tabelle temporanee locali contengono un solo simbolo di cancelletto (#) come primo carattere del nome, sono visibili solo durante la connessione utente corrente e vengono eliminate quando l'utente chiude la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le tabelle temporanee globali contengono due simboli di cancelletto (##) come primi caratteri del nome, una volta create sono visibili per tutti gli utenti e vengono eliminate quando tutti gli utenti che fanno riferimento alla tabella chiudono la connessione all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 


#### <a name="reduced-recompilations-for-workloads-using-temporary-tables-across-multiple-scopes"></a><a name="ctp23"></a> Riduzione delle ricompilazioni per carichi di lavoro che usano tabelle temporanee in più ambiti

In [!INCLUDE[ss2019](../../includes/sssqlv15-md.md)] con tutti i livelli di compatibilità del database vengono ridotte le ricompilazioni per carichi di lavoro che usano tabelle temporanee in più ambiti. Questa funzionalità è abilitata anche nel database SQL di Azure con il livello di compatibilità del database 150 per tutti i modelli di distribuzione.  Prima di questa funzionalità, quando si faceva riferimento a una tabella temporanea con un'istruzione DML (`SELECT`, `INSERT`, `UPDATE`, `DELETE`), se la tabella temporanea era stata creata da un batch di ambito esterno, l'istruzione DML veniva ricompilata ogni volta che veniva eseguita. Grazie a questo miglioramento, SQL Server esegue semplici controlli aggiuntivi per evitare ricompilazioni non necessarie:

- Controlla se il modulo di ambito esterno usato per creare la tabella temporanea in fase di compilazione è uguale a quello usato per le esecuzioni consecutive. 
- Tiene traccia di eventuali modifiche di Data Definition Language (DDL) apportate alla compilazione iniziale e le confronta con le operazioni DDL per le esecuzioni consecutive.

Il risultato finale è la riduzione di ricompilazioni estranee e di overhead della CPU.

### <a name="system-tables"></a>Tabelle di sistema

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] archivia i dati che definiscono la configurazione del server e di tutte le relative tabelle in un set di tabelle speciale noto come tabelle di sistema. Gli utenti non possono eseguire una query direttamente o aggiornare le tabelle di sistema. Le informazioni delle tabelle di sistema vengono rese disponibili tramite le viste di sistema. Per altre informazioni, vedere [Viste di sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md). 
 
### <a name="wide-tables"></a>Tabelle estese in larghezza

Le tabelle estese in larghezza usano le [colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md) per aumentare a 30.000 il numero totale di colonne che una tabella può contenere. Le colonne di tipo sparse sono colonne comuni che dispongono di archiviazione ottimizzata per i valori Null. Tali colonne consentono di ridurre i requisiti di spazio per i valori Null aumentando tuttavia l'overhead per il recupero dei valori non Null. Una colonna estesa ha definito un [set di colonne](../../relational-databases/tables/use-column-sets.md), ovvero una rappresentazione XML non tipizzata che combina tutte le colonne di tipo sparse di una tabella in un output strutturato. Anche il numero degli indici e delle statistiche viene portato rispettivamente a 1.000 e 30.000. Le dimensioni di una riga di tabella estesa in larghezza non possono superare 8.019 byte. Pertanto, la maggior parte dei dati di qualsiasi riga deve essere NULL. Il numero massimo di colonne di tipo nonsparse più le colonne calcolate di una tabella estesa in larghezza rimane 1.024. 

Le tabelle estese in larghezza hanno le seguenti implicazioni sulle prestazioni.

- Le tabelle estese in larghezza possono aumentare i costi di gestione degli indici nella tabella. È consigliabile limitare il numero di indici di una tabella estesa in larghezza agli indici richiesti dalla logica di business. All'aumento del numero di indici corrisponde infatti un aumento dei requisiti di memoria e dei tempi di compilazione DML. Gli indici non cluster devono essere indici filtrati applicati a subset di dati. Per altre informazioni, vedere [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md). 

- Tramite applicazioni è possibile aggiungere e rimuovere in modo dinamico le colonne dalle tabelle estese in larghezza. Quando si aggiungono o rimuovono colonne, i piani di query compilati perdono validità. È pertanto consigliabile progettare un'applicazione in base al carico di lavoro previsto in modo che le modifiche allo schema siano minime. 

- L'aggiunta e la rimozione di dati da una tabella estesa in larghezza possono incidere negativamente sulle prestazioni. È necessario progettare le applicazioni per il carico di lavoro previsto in modo che le modifiche ai dati della tabella siano minime. 

- Limitare l'esecuzione di istruzioni DML in una tabella estesa in larghezza che comportino l'aggiornamento di più righe di una chiave di clustering. La compilazione e l'esecuzione di queste istruzioni possono richiedere una notevole quantità di memoria. 

- Il passaggio tra partizioni nelle tabelle estese in larghezza può essere lento e richiedere grandi quantità di memoria di elaborazione. Le prestazioni e i requisiti di memoria sono proporzionali al numero totale di colonne sia nella partizione di origine sia in quella di destinazione. 

- I cursori di aggiornamento di colonne specifiche di una tabella estesa in larghezza dovrebbero elencare le colonne in modo esplicito per la clausola FOR UPDATE. In questo modo, è possibile ottimizzare le prestazioni quando si utilizzano i cursori. 

## <a name="common-table-tasks"></a>Attività comuni della tabella
 Nella tabella riportata di seguito vengono forniti i collegamenti a attività comuni associate alla creazione o alla modifica di una tabella. 

|Attività tabella|Argomento|
|-----------------|-----------|
|Viene illustrato come creare una tabella.|[Creare tabelle &#40;motore di database&#41;](../../relational-databases/tables/create-tables-database-engine.md)|
|Viene illustrato come eliminare una tabella.|[Eliminare tabelle &#40;motore di database&#41;](../../relational-databases/tables/delete-tables-database-engine.md)|
|Viene illustrato come creare una nuova tabella che contiene alcune o tutte le colonne in una tabella esistente.|[Duplicare le tabelle](../../relational-databases/tables/duplicate-tables.md)|
|Viene illustrato come ridenominare una tabella.|[Rinominare tabelle &#40;motore di database&#41;](../../relational-databases/tables/rename-tables-database-engine.md)|
|Viene illustrata la procedura per visualizzare le proprietà della tabella.|[Visualizzare la definizione di una tabella](../../relational-databases/tables/view-the-table-definition.md)|
|Viene illustrato come determinare se altri oggetti quali una vista o una stored procedure dipende da una tabella.|[Visualizzare le dipendenze di una tabella](../../relational-databases/tables/view-the-dependencies-of-a-table.md)|

 Nella tabella riportata di seguito vengono forniti i collegamenti a attività comuni associate alla creazione o alla modifica di colonne in una tabella. 

|Attività colonne|Argomento|
|------------------|-----------|
|Viene illustrato come aggiungere colonne a una tabella esistente.|[Aggiungere colonne a una tabella &#40;motore di database&#41;](../../relational-databases/tables/add-columns-to-a-table-database-engine.md)|
|Viene illustrato come eliminare colonne da una tabella.|[Eliminare le colonne da una tabella](../../relational-databases/tables/delete-columns-from-a-table.md)|
|Viene illustrato come modificare il nome di una colonna.|[Rinominare colonne &#40;motore di database&#41;](../../relational-databases/tables/rename-columns-database-engine.md)|
|Viene illustrato come copiare colonne da una tabella a un'altra copiando solo la definizione di colonna oppure la definizione e i dati.|[Copiare colonne da una tabella a un'altra &#40;motore di database&#41;](../../relational-databases/tables/copy-columns-from-one-table-to-another-database-engine.md)|
|Viene illustrato come modificare una definizione di colonna, modificando il tipo di dati o altre proprietà.|[Modificare colonne &#40;Motore di database&#41;](../../relational-databases/tables/modify-columns-database-engine.md)|
|Viene illustrato come modificare l'ordine di visualizzazione delle colonne.|[Modificare l'ordine delle colonne in una tabella](../../relational-databases/tables/change-column-order-in-a-table.md)|
|Viene illustrato come creare una colonna calcolata in una tabella.|[Specificare le colonne calcolate in una tabella](../../relational-databases/tables/specify-computed-columns-in-a-table.md)|
|Viene illustrato come specificare un valore predefinito per una colonna. Questo valore viene utilizzato nel caso non venga fornito alcun altro valore.|[Specificare valori predefiniti per le colonne](../../relational-databases/tables/specify-default-values-for-columns.md)|

## <a name="see-also"></a>Vedere anche
 [Vincoli di chiavi primarie ed esterne](../../relational-databases/tables/primary-and-foreign-key-constraints.md) [Vincoli UNIQUE e CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md)