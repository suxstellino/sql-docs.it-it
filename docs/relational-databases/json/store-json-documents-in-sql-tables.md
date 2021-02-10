---
title: Archiviare documenti JSON in SQL Server o nel database SQL
description: Questo articolo descrive perché e come archiviare e indicizzare i documenti JSON in SQL Server o nel database SQL e come ottimizzare le query sui documenti JSON.
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 30d51162b36d787e8976b2c6aa48e533fd476f8e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015061"
---
# <a name="store-json-documents-in-sql-server-or-sql-database"></a>Archiviare documenti JSON in SQL Server o nel database SQL
SQL Server e il database SQL di Azure includono funzioni JSON native che consentono di analizzare i documenti JSON usando il linguaggio SQL standard. È possibile archiviare i documenti JSON in SQL Server o nel database SQL ed eseguire query sui dati JSON come in un database NoSQL. In questo articolo vengono descritte le opzioni per archiviare i documenti JSON in SQL Server o nel database SQL.

## <a name="json-storage-format"></a>Formato di archiviazione JSON

La prima decisione per la progettazione dell'archiviazione è la modalità di archiviazione dei documenti JSON nelle tabelle. Sono disponibili due opzioni:
- **Archiviazione LOB**: i documenti JSON possono essere archiviati così come sono in colonne `NVARCHAR`. Questo è il modo migliore per eseguire rapidamente il caricamento e l'inserimento dei dati, perché la velocità di caricamento corrisponde al caricamento delle colonne stringa. Questo approccio può introdurre un'ulteriore riduzione delle prestazioni per il tempo di query/analisi se non viene eseguita l'indicizzazione sui valori JSON, poiché devono essere analizzati i documenti JSON non elaborati durante l'esecuzione delle query. 
- **Archiviazione relazionale**: i documenti JSON possono essere analizzati durante l'inserimento nella tabella usando le funzioni `OPENJSON`, `JSON_VALUE` o `JSON_QUERY`. I frammenti dei documenti JSON di input possono essere archiviati nelle colonne con tipo di dati SQL o in colonne NVARCHAR contenenti elementi secondari JSON. Questo approccio aumenta il tempo di caricamento perché l'analisi JSON viene eseguita durante il caricamento, tuttavia le prestazioni delle query corrispondono a quelle delle query classiche sui dati relazionali.

## <a name="classic-tables"></a>Tabelle classiche

Il modo più semplice per archiviare i documenti JSON in SQL Server o nel database SQL consiste nel creare una tabella a due colonne contenente l'ID e il contenuto del documento. Ad esempio:

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max)
);
```

Questa struttura è equivalente alle raccolte che è possibile trovare nei database di documenti classici. La chiave primaria `_id` è un valore a incremento automatico che assicura un identificatore univoco per ogni documento e consente ricerche veloci. Questa struttura è una buona scelta in scenari NoSQL classici in cui si vuole recuperare un documento in base all'ID o aggiornarne uno archiviato in base all'ID.

Il tipo di dati nvarchar(max) consente di archiviare documenti JSON con una dimensione massima di 2 GB. Per motivi di prestazioni, se si è certi che i documenti JSON non superino le dimensioni di 8 KB, è tuttavia consigliabile usare NVARCHAR(4000) anziché NVARCHAR(max).

Nella tabella di esempio creata nell'esempio precedente si presuppone che nella colonna `log` vengano archiviati documenti JSON validi. Per assicurarsi che nella colonna `log` venga salvato contenuto JSON valido è possibile aggiungere un vincolo CHECK nella colonna. Ad esempio:

```sql
ALTER TABLE WebSite.Logs
    ADD CONSTRAINT [Log record should be formatted as JSON]
                   CHECK (ISJSON(log)=1)
```

Ogni volta che un utente inserisce o aggiorna un documento nella tabella, questo vincolo verifica che il documento JSON sia formattato correttamente. Senza il vincolo, la tabella è ottimizzata per gli inserimenti. I documenti JSON vengono infatti aggiunti direttamente alla colonna senza alcuna elaborazione.

Con i documenti JSON archiviati nella tabella, sarà possibile di usare il linguaggio Transact-SQL standard per eseguire query sui documenti. Ad esempio:

```sql
SELECT TOP 100 JSON_VALUE(log, '$.severity'), AVG( CAST( JSON_VALUE(log,'$.duration') as float))
 FROM WebSite.Logs
 WHERE CAST( JSON_VALUE(log,'$.date') as datetime) > @datetime
 GROUP BY JSON_VALUE(log, '$.severity')
 HAVING AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) > 100
 ORDER BY AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) DESC
```

La possibilità di usare *qualsiasi* funzione e clausola di query T-SQL per eseguire query sui documenti JSON è un enorme vantaggio. SQL Server e il database SQL non introducono alcun vincolo nelle query che è possibile usare per analizzare i documenti JSON. È possibile estrarre i valori da un documento JSON con la funzione `JSON_VALUE` e usarla nella query come qualsiasi altro valore.

Questa capacità di usare una sintassi di query T-SQL avanzata è la differenza principale tra SQL Server e il database SQL e i database NoSQL classici. Transact-SQL include probabilmente tutte le funzioni necessarie per elaborare i dati JSON.

## <a name="indexes"></a>Indici

Se ci si rende conto che la ricerca nei documenti viene spesso eseguita in base a una proprietà (ad esempio, una proprietà `severity` in un documento JSON) è possibile aggiungere un indice NON CLUSTER classico nella proprietà per velocizzare le query.

È possibile creare una colonna calcolata che espone i valori JSON delle colonne JSON nel percorso specificato, ovvero nel percorso `$.severity`, e creare un indice standard in questa colonna calcolata. Ad esempio:

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max),

    severity AS JSON_VALUE(log, '$.severity'),
    index ix_severity (severity)
);
```

La colonna calcolata usata in questo esempio è una colonna non persistente o virtuale che non aggiunge altro spazio alla tabella. Viene usata dall'indice `ix_severity` per migliorare le prestazioni delle query come nell'esempio seguente:

```sql
SELECT log
FROM Website.Logs
WHERE JSON_VALUE(log, '$.severity') = 'P4'
```

Una caratteristica importante di questo indice è che riconosce le regole di confronto. Se la colonna NVARCHAR originale include una proprietà COLLATION (ad esempio, distinzione tra maiuscole e minuscole o lingua giapponese), l'indice verrà organizzato in base alle regole della lingua o della distinzione tra maiuscole e minuscole associate alla colonna NVARCHAR. Questa conoscenza delle regole di confronto può rivelarsi importante se si sviluppano applicazioni per mercati globali che devono usare regole della lingua personalizzate durante l'elaborazione di documenti JSON.

## <a name="large-tables--columnstore-format"></a>Tabelle di grandi dimensioni e formato columnstore

Se si prevede che la propria raccolta includerà un numero elevato di documenti JSON, è consigliabile aggiungere un indice CLUSTERED COLUMNSTORE nella raccolta, come illustrato nell'esempio seguente:

```sql
create sequence WebSite.LogID as bigint;
go
create table WebSite.Logs (
    _id bigint default(next value for WebSite.LogID),
    log nvarchar(max),

    INDEX cci CLUSTERED COLUMNSTORE
);
```

Un indice CLUSTERED COLUMNSTORE consente un'elevata compressione dei dati (fino a 25 volte) che può ridurre in modo significativo i requisiti di spazio di archiviazione, diminuire i costi di archiviazione e migliorare le prestazioni di I/O del carico di lavoro. Gli indici CLUSTERED COLUMNSTORE sono inoltre ottimizzati per le scansioni di tabella e l'analisi nei documenti JSON, pertanto questo tipo di indice potrebbe essere l'opzione migliore per Log Analytics.

Nell'esempio precedente è stato usato un oggetto sequenza per assegnare i valori alla colonna `_id`. Sequenze e identità sono entrambe opzioni valide per la colonna ID.

## <a name="frequently-changing-documents--memory-optimized-tables"></a>Documenti modificati di frequente e tabelle ottimizzate per la memoria

Se nelle raccolte si prevedono numerose operazioni di aggiornamento, inserimento ed eliminazione è possibile archiviare i documenti JSON in tabelle ottimizzate per la memoria. Le raccolte JSON ottimizzate per la memoria mantengono sempre i dati in memoria, evitando il sovraccarico di I/O dell'archiviazione. Le raccolte JSON ottimizzate per la memoria non prevedono inoltre alcun tipo di blocco. Le azioni sui documenti non bloccano infatti le altre operazioni.

Per convertire una raccolta classica in una raccolta ottimizzata per la memoria è sufficiente specificare l'opzione **with (memory_optimized=on)** dopo la definizione della tabella, come illustrato nell'esempio seguente. Si otterrà quindi una versione ottimizzata per la memoria della raccolta JSON.

```sql
create table WebSite.Logs (
  _id bigint identity primary key nonclustered,
  log nvarchar(4000)
) with (memory_optimized=on)
```

Una tabella ottimizzata per la memoria è l'opzione migliore per i documenti modificati di frequente. Quando si valuta l'uso di tabelle ottimizzate per la memoria è consigliabile tenere presente anche le prestazioni. Se possibile, usare NVARCHAR(4000) anziché NVARCHAR(max) per i documenti JSON presenti nelle raccolte ottimizzate per la memoria poiché le prestazioni potrebbero migliorare in modo significativo.

In modo analogo alle tabelle classiche, è possibile aggiungere indici sui campi esposti nelle tabelle ottimizzate per la memoria usando le colonne calcolate. Ad esempio:

```sql
create table WebSite.Logs (

  _id bigint identity primary key nonclustered,
  log nvarchar(4000),

  severity AS cast(JSON_VALUE(log, '$.severity') as tinyint) persisted,
  index ix_severity (severity)

) with (memory_optimized=on)
```

Per ottimizzare le prestazioni, eseguire il cast del valore JSON nel tipo più piccolo possibile che si può usare per contenere il valore della proprietà. Nell'esempio precedente viene usato **tinyint**.

Per ottenere il vantaggio della compilazione nativa è anche possibile inserire le query SQL che aggiornano i documenti JSON in stored procedure. Ad esempio:

```sql
CREATE PROCEDURE WebSite.UpdateData(@Id int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION

AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE WebSite.Logs
    SET log = JSON_MODIFY(log, @Property, @Value)
    WHERE _id = @Id;

END
```

Questa procedura compilata in modo nativo accetta la query e crea il codice DLL che la esegue. Una procedura compilata in modo nativo è l'approccio più rapido per l'esecuzione di query e l'aggiornamento dei dati.

## <a name="conclusion"></a>Conclusioni

Le funzioni JSON native in SQL Server e nel database SQL consentono di elaborare i documenti JSON come nei database NoSQL. Per tutti i database, relazionali o NoSQL, esistono pro e contro in termini di elaborazione dei dati JSON. Il principale vantaggio dell'archiviazione di documenti JSON in SQL Server o nel database SQL è il supporto completo del linguaggio SQL. È possibile usare il linguaggio avanzato Transact-SQL per elaborare i dati e configurare una varietà di opzioni di archiviazione, dagli indici columnstore per la compressione elevata e l'analisi rapida alle tabelle ottimizzate per la memoria per l'elaborazione senza blocchi. Allo stesso tempo, si ottengono i vantaggi di una sicurezza avanzata e delle funzionalità di internazionalizzazione che sarà possibile riusare facilmente nello scenario NoSQL. I motivi descritti in questo articolo sono tutti ottimi motivi per prendere in considerazione l'archiviazione di documenti JSON in SQL Server o nel database SQL.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Altre informazioni su JSON in SQL Server e nel database SQL di Azure  
  
### <a name="microsoft-videos"></a>Video Microsoft

Per un'introduzione visiva al supporto JSON predefinito in SQL Server e nel database SQL di Azure, vedere i video seguenti:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 e supporto JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso di JSON in SQL Server 2016 e nel database SQL di Azure)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON come ponte tra NoSQL e gli ambienti relazionali)
