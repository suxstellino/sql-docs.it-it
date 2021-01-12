---
title: BULK INSERT (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per l'istruzione BULK INSERT.
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BULK_TSQL
- BULK_INSERT
- BULK_INSERT_TSQL
- BULK INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9e0d729d951de184aa16717e784f14ae244639d8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098499"
---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Importa un file di dati in una tabella o una vista di database in un formato specificato dall'utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
BULK INSERT
   { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }
      FROM 'data_file'
     [ WITH
    (
   [ [ , ] BATCHSIZE = batch_size ]
   [ [ , ] CHECK_CONSTRAINTS ]
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ [ , ] DATAFILETYPE =
      { 'char' | 'native'| 'widechar' | 'widenative' } ]
   [ [ , ] DATA_SOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] FIRSTROW = first_row ]
   [ [ , ] FIRE_TRIGGERS ]
   [ [ , ] FORMATFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]
   [ [ , ] KEEPNULLS ]
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]
   [ [ , ] LASTROW = last_row ]
   [ [ , ] MAXERRORS = max_errors ]
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
   [ [ , ] TABLOCK ]

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
    )]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*database_name* è il nome del database contenente la tabella o la vista specificata. Se viene omesso, il valore predefinito è il database corrente.

*schema_name* è il nome dello schema della tabella o della vista. *schema_name* è facoltativo se lo schema predefinito per l'utente che esegue l'operazione di importazione bulk corrisponde allo schema della tabella o della vista specificata. Se *schema* non viene specificato e lo schema predefinito dell'utente che esegue l'operazione di importazione bulk è diverso dalla tabella o dalla vista specificata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore e l'operazione di importazione bulk viene annullata.

*table_name* è il nome della tabella o della vista in cui eseguire l'importazione bulk dei dati. È possibile utilizzare solo le viste in cui tutte le colonne fanno riferimento alla stessa tabella di base. Per altre informazioni sulle restrizioni per il caricamento dei dati nelle viste, vedere [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).

**'** _data_file_ **'** è il percorso completo del file contenente i dati da importare nella tabella o nella vista specificata. L'istruzione BULK INSERT consente di importare dati da un disco o da Archiviazione BLOB di Azure, ad esempio unità di rete, dischi floppy, dischi rigidi e così via.

*data_file* deve specificare un percorso valido dal server in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se *data_file* corrisponde a un file remoto, è necessario specificare il nome UNC (Universal Naming Convention). Il formato del nome UNC è \\\\*NomeSistema*\\*NomeCondivisione*\\*Percorso*\\*NomeFile*. Ad esempio:

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.dat';
```

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 e database SQL di Azure.
A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, data_file può essere presente nell'archiviazione BLOB di Azure. In tal caso, è necessario specificare l'opzione **data_source_name**. Per un esempio, vedere [Importazione di dati da un file nell'archiviazione BLOB di Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

**'** _data_source_name_ **'** 
**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 e database SQL di Azure.
Origine dati esterna denominata che punta alla posizione dell'archiviazione BLOB di Azure del file che verrà importato. L'origine dati esterna deve essere creata tramite l'opzione `TYPE = BLOB_STORAGE` aggiunta in [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md). Per un esempio, vedere [Importazione di dati da un file nell'archiviazione BLOB di Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

BATCHSIZE **=** _batch_size_ specifica il numero di righe in un batch. Ogni batch viene copiato nel server come singola transazione. In caso di errori, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue il commit o il rollback della transazione per ogni batch. Per impostazione predefinita, tutti i dati inclusi nel file di dati specificato costituiscono un batch. Per alcune considerazioni relative alle prestazioni, vedere la sezione "Osservazioni" di seguito in questo argomento.

CHECK_CONSTRAINTS specifica che tutti i vincoli sulla tabella o sulla vista di destinazione devono essere controllati durante l'operazione di importazione bulk. Se non si specifica l'opzione CHECK_CONSTRAINTS, i vincoli CHECK e FOREIGN KEY vengono ignorati e al termine dell'operazione il vincolo sulla tabella viene contrassegnato come non attendibile.

> [!NOTE]
> I vincoli UNIQUE e PRIMARY KEY vengono sempre applicati. Durante l'importazione in una colonna di tipo carattere definita con un vincolo NOT NULL, tramite BULK INSERT viene inserita una stringa vuota se non è presente alcun valore nel file di testo.

A un certo punto, sarà necessario esaminare i vincoli nell'intera tabella. Se prima dell'operazione di importazione bulk la tabella non era vuota, il costo per la riconvalida dei vincoli può essere superiore a quello correlato all'applicazione dei vincoli CHECK ai dati incrementali.

Una situazione in cui può essere necessario disabilitare i vincoli (comportamento predefinito) è rappresentata dal caso in cui i dati di input contengono righe che violano i vincoli. Se si disabilitano i vincoli CHECK, è possibile importare i dati e quindi utilizzare istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per rimuovere i dati non validi.

> [!NOTE]
> L'opzione MAXERRORS non viene applicata al controllo dei vincoli.

CODEPAGE **=** { **'** ACP **'** \| **'** OEM **'** \| **'** RAW **'** \| **'** _code_page_ **'** } specifica la tabella codici dei dati contenuti nel file di dati. CODEPAGE è pertinente solo se i dati contengono colonne di tipo **char**, **varchar** o **text** con valori carattere maggiori di **127** o minori di **32**. Per un esempio, vedere [Definizione di una tabella codici](#d-specifying-a-code-page).

> [!IMPORTANT]
> CODEPAGE non è un'opzione supportata in Linux per [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]. Per [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)], è consentita solo l'opzione **'RAW'** per CODEPAGE.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di specificare un nome di regole di confronto per ogni colonna in un [file di formato](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

|Valore CODEPAGE|Descrizione|
|--------------------|-----------------|
|ACP|Le colonne con tipo di dati **char**, **varchar** o **text** vengono convertire dalla tabella codici [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) a quella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (predefinito)|Le colonne con tipo di dati **char**, **varchar** o **text** vengono convertire dalla tabella codici OEM di sistema a quella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|Non viene eseguita alcuna conversione da una tabella codici a un'altra. Si tratta dell'opzione più rapida.|
|*code_page*|Numero specifico della tabella codici, ad esempio 850.<br /><br /> **&#42;&#42; Importante &#42;&#42;** Le versioni precedenti a [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] non supportano la tabella codici 65001 (codifica UTF-8).|
| &nbsp; | &nbsp; |

DATAFILETYPE **=** { **'char'** \| **'native'** \| **'widechar'** \| **'widenative'** } specifica che l'operazione di importazione viene eseguita tramite BULK INSERT usando il valore specificato per il tipo di file di dati.

&nbsp;

|Valore DATAFILETYPE|Rappresentazione di tutti i dati|
|------------------------|------------------------------|
|**char** (impostazione predefinita)|Formato carattere.<br /><br /> Per altre informazioni, vedere [Usare il formato carattere per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).|
|**native**|Tipi di dati nativi (database). Creare il file di dati nativo tramite l'importazione bulk dei dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'utilità **bcp**.<br /><br /> Il valore native rappresenta un'alternativa con prestazioni superiori rispetto al valore char. L'uso del formato nativo è consigliabile per il trasferimento bulk dei dati tra più istanze di SQL Server tramite un file di dati che non contiene caratteri estesi o DBCS (Double Byte Character Set).<br /><br /> Per altre informazioni, vedere [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).|
|**widechar**|Caratteri Unicode.<br /><br /> Per altre informazioni, vedere [Usare il formato carattere Unicode per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).|
|**widenative**|Tipi di dati nativi (database), ad eccezione delle colonne di tipo **char**, **varchar** e **text**, in cui i dati vengono archiviati in formato Unicode. Creare il file di dati **widenative** tramite l'importazione bulk dei dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'utilità **bcp**.<br /><br /> Il valore **widenative** rappresenta un'alternativa con prestazioni migliori rispetto a **widechar**. Se il file di dati contiene caratteri estesi [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)], specificare **widenative**.<br /><br /> Per altre informazioni, vedere [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).|
| &nbsp; | &nbsp; |

ERRORFILE **='** _file_name_ **'** specifica il file usato per raccogliere le righe che contengono errori di formattazione e non possono essere convertite in un set di righe OLE DB. Tali righe vengono copiate nel file degli errori dal file di dati così come sono.

Il file degli errori viene creato durante l'esecuzione del comando. Se il file esiste già, viene generato un errore. Viene inoltre creato un file di controllo con estensione ERROR.txt. Questo file contiene un riferimento a ogni riga nel file degli errori e offre informazioni di diagnostica. Una volta corretti gli errori, è possibile caricare i dati.
**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], `error_file_path` può essere presente nell'archiviazione BLOB di Azure.

'errorfile_data_source_name' **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Origine dati esterna denominata che punta alla posizione di archiviazione BLOB di Azure del file degli errori rilevati durante l'importazione. L'origine dati esterna deve essere creata tramite l'opzione `TYPE = BLOB_STORAGE` aggiunta in [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FIRSTROW **=** _first_row_ specifica il numero della prima riga da caricare. Il valore predefinito è la prima riga nel file di dati specificato. FIRSTROW è in base 1.

> [!NOTE]
> L'attributo FIRSTROW non è progettato per ignorare le intestazioni di colonna. Nell'istruzione BULK INSERT non è possibile ignorare le intestazioni. Quando vengono ignorate righe specifiche, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] esegue la ricerca solo nei caratteri di terminazione dei campi e non convalida i dati nei campi delle righe ignorate.

FIRE_TRIGGERS specifica che gli eventuali trigger di inserimento definiti nella tabella di destinazione vengono eseguiti durante l'operazione di importazione bulk. Se sono presenti trigger definiti per operazioni INSERT nella tabella di destinazione, questi trigger vengono attivati per ogni batch completato.

Se non si specifica FIRE_TRIGGERS, non viene eseguito alcun trigger di inserimento.

FORMATFILE_DATA_SOURCE **=** 'data_source_name' **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 1.1.
Origine dati esterna denominata che punta alla posizione dell'archiviazione BLOB di Azure del file di formato che definisce lo schema dei dati importati. L'origine dati esterna deve essere creata tramite l'opzione `TYPE = BLOB_STORAGE` aggiunta in [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

KEEPIDENTITY specifica che il valore o i valori Identity presenti nel file di dati importato devono essere usati per la colonna Identity. Se KEEPIDENTITY viene omesso, i valori Identity per la colonna vengono verificati ma non importati e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna automaticamente valori univoci in base ai valori di inizializzazione e di incremento specificati in fase di creazione della tabella. Se il file di dati non contiene valori per la colonna Identity nella tabella o nella vista, utilizzare un file di formato per specificare che durante l'importazione dei dati la colonna Identity nella tabella o nella vista deve essere ignorata. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna automaticamente valori univoci alla colonna. Per altre informazioni, vedere [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).

Per altre informazioni sul mantenimento dei valori Identity, vedere [Mantenere i valori Identity durante l'importazione bulk dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).

KEEPNULLS specifica che durante l'operazione di importazione bulk le colonne vuote devono mantenere un valore Null. Ciò significa che durante l'operazione non verranno inseriti eventuali valori predefiniti per le colonne vuote. Per altre informazioni, vedere [Mantenere i valori Null o usare i valori predefiniti durante l'importazione in blocco &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).

KILOBYTES_PER_BATCH **=** _kilobytes_per_batch_ specifica il numero approssimativo di kilobyte (KB) di dati per ogni batch come *kilobytes_per_batch*. Per impostazione predefinita, il valore KILOBYTES_PER_BATCH è sconosciuto. Per alcune considerazioni relative alle prestazioni, vedere la sezione "Osservazioni" di seguito in questo argomento.

LASTROW **=** _last_row_ specifica il numero dell'ultima riga da caricare. Il valore predefinito è 0, che indica l'ultima riga nel file di dati specificato.

MAXERRORS **=** _max_errors_ specifica il numero massimo di errori di sintassi consentiti nei dati prima dell'annullamento dell'operazione di importazione bulk. Ogni riga che non è possibile importare tramite l'operazione di importazione bulk viene ignorata e considerata come errore. Se non si specifica *max_errors*, il valore predefinito è 10.

> [!NOTE]
> L'opzione MAX_ERRORS non è applicabile alle verifiche dei vincoli o alla conversione di tipi di dati **money** e **bigint**.

ORDER ( { *column* [ ASC | DESC ] } [ **,** ... *n* ] ) specifica la modalità di ordinamento dei dati nel file. Le prestazioni dell'importazione bulk sono migliori se i dati da importare vengono ordinati in base all'indice cluster della tabella, se disponibile. Se il file di dati viene ordinato in modo diverso rispetto all'ordine di una chiave di indice cluster o se la tabella non include un indice cluster, la clausola ORDER viene ignorata. I nomi di colonna specificati devono corrispondere a nomi di colonna validi nella tabella di destinazione. Per impostazione predefinita, per l'operazione di inserimento bulk si presume che il file di dati non sia ordinato. Per garantire l'ottimizzazione dell'importazione bulk, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene inoltre verificato che i dati importati siano ordinati.

*n* è un segnaposto che indica la possibilità di specificare più colonne.

ROWS_PER_BATCH **=** _rows_per_batch_ indica il numero approssimativo di righe di dati nel file.

Per impostazione predefinita, tutti i dati nel file di dati vengono inviati al server come singola transazione e il numero di righe nel batch non è noto per Query Optimizer. Se si specifica ROWS_PER_BATCH (con valore > 0), il server usa questo valore per ottimizzare l'operazione di importazione bulk. Il valore specificato per ROWS_PER_BATCH deve essere più o meno uguale al numero effettivo di righe. Per alcune considerazioni relative alle prestazioni, vedere la sezione "Osservazioni" di seguito in questo argomento.

TABLOCK imposta l'acquisizione di un blocco a livello di tabella per l'intera durata dell'operazione di importazione bulk. Una tabella può essere caricata simultaneamente da più client se non include indici e si specifica TABLOCK. Per impostazione predefinita, la modalità di blocco è determinata dall'opzione **table lock on bulk load** della tabella. Quando un blocco viene mantenuto attivo solo per la durata dell'operazione di importazione bulk, la contesa tra blocchi nella tabella viene ridotta, producendo in alcuni casi un miglioramento significativo delle prestazioni. Per alcune considerazioni relative alle prestazioni, vedere la sezione "Osservazioni" di seguito in questo argomento.

Per l'indice columnstore, il comportamento di blocco è diverso perché è internamente suddiviso in più set di righe. Ogni thread carica dati esclusivamente in ogni set di righe tramite l'applicazione di un blocco X al set di righe, consentendo il caricamento parallelo dei dati con sessioni di caricamento dati simultanee. L'uso dell'opzione TABLOCK causa l'applicazione da parte del thread di un blocco X sulla tabella (a differenza del blocco aggiornamenti bulk per i set di righe tradizionali), che impedisce ad altri thread simultanei di caricare dati contemporaneamente.

### <a name="input-file-format-options"></a>Opzioni di formato del file di input

FORMAT **=** 'CSV' **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Specifica un file di valori separati da virgole conforme allo standard [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.csv'
WITH ( FORMAT='CSV');
```

FIELDQUOTE **=** 'field_quote' **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Specifica il carattere da usare come carattere virgolette nel file CSV. Se non viene specificato alcun carattere, viene usato il carattere virgolette (") in base alla definizione dello standard [RFC 4180](https://tools.ietf.org/html/rfc4180).

FORMATFILE **=** '_format_file_path_' specifica il percorso completo di un file di formato. Un file di formato descrive il file di dati contenente le risposte archiviate create tramite l'utilità **bcp** nella stessa tabella o vista. È consigliabile utilizzare il file di formato nei casi seguenti:

- Il file di dati contiene un numero di colonne maggiore o minore rispetto alla tabella o alla vista.
- Le colonne sono in un ordine diverso.
- I delimitatori di colonna variano.
- Sono presenti altre modifiche di formato dei dati. I file di formato vengono in genere creati tramite l'utilità **bcp** e modificati in un editor di testo in base alle esigenze. Per altre informazioni, vedere [Utilità bcp](../../tools/bcp-utility.md) e [Creare un file di formato](../../relational-databases/import-export/create-a-format-file-sql-server.md).

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 e database SQL di Azure.
A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, format_file_path può essere presente nell'archiviazione BLOB di Azure.

FIELDTERMINATOR **='** _field_terminator_ **'** specifica il carattere di terminazione del campo da usare per i file di dati di tipo **char** e **widechar**. Il carattere di terminazione del campo predefinito è \t (carattere di tabulazione). Per altre informazioni, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

ROWTERMINATOR **='** _row_terminator_ **'** specifica il carattere di terminazione della riga da usare per i file di dati di tipo **char** e **widechar**. Il carattere di terminazione della riga predefinito è **\r\n** (carattere di nuova riga). Per altre informazioni, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

## <a name="compatibility"></a>Compatibilità

L'istruzione BULK INSERT consente di applicare meccanismi restrittivi di convalida e controllo dei dati letti da un file che potrebbero causare errori negli script esistenti eseguiti in dati non validi. L'istruzione BULK INSERT, ad esempio, prevede le verifiche seguenti:

- Validità delle rappresentazioni native dei tipi di dati **float** o **real**.
- Lunghezza in byte pari dei dati Unicode.

## <a name="data-types"></a>Tipi di dati

### <a name="string-to-decimal-data-type-conversions"></a>Conversione del tipo di dati da string a decimal

Le conversioni del tipo di dati da string a decimal usate in BULK INSERT seguono le stesse regole della funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md), che rifiuta le stringhe che rappresentano valori numerici con notazione scientifica. Tali stringhe vengono quindi considerate da BULK INSERT come valori non validi e vengono segnalati errori di conversione.

Per risolvere il problema, usare un file di formato per eseguire l'importazione bulk dei dati **float** in notazione scientifica in una colonna decimale. Nel file di formato, descrivere in modo esplicito la colonna come dati **real** o **float**. Per altre informazioni su questi tipi di dati, vedere [float e real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).

> [!NOTE]
> I file di formato rappresentano i dati **real** con il tipo di dati **SQLFLT4** e i dati **float** con il tipo di dati **SQLFLT8**. Per informazioni sui file di formato diverso da XML, vedere [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).

#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>Esempio di importazione di un valore numerico con notazione scientifica

Nell'esempio viene utilizzata la tabella seguente:

```sql
CREATE TABLE t_float(c1 FLOAT, c2 DECIMAL (5,4));
```

 Si desidera eseguire l'importazione bulk di dati nella tabella `t_float`. Il file di dati, C:\t_float-c.dat, contiene dati **float** in notazione scientifica, ad esempio:

```input
8.0000000000000002E-28.0000000000000002E-2
```

Non è tuttavia possibile importare i dati direttamente nella tabella `t_float` tramite BULK INSERT, in quanto la seconda colonna della tabella, `c2`, utilizza il tipo di dati `decimal`. È pertanto necessario un file di formato. Il file di formato deve eseguire il mapping dei dati **float** in notazione scientifica al formato decimale della colonna `c2`.

Nel file di formato seguente viene utilizzato il tipo di dati `SQLFLT8` per eseguire il mapping del secondo campo dati alla seconda colonna:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>
<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/> </RECORD> <ROW>
<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>
<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/> </ROW> </BCPFORMAT>
```

Per importare i dati di prova nella tabella di prova utilizzando questo file di formato con il nome di file `C:\t_floatformat-c-xml.xml`, eseguire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:

```sql
BULK INSERT bulktest..t_float
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');
```

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>Tipi di dati per l'esportazione o l'importazione bulk di documenti SQLXML

Per eseguire l'esportazione o l'importazione bulk di dati SQLXML, utilizzare uno dei tipi di dati seguenti nel file di formato:

|Tipo di dati|Effetto|
|---------------|------------|
|SQLCHAR o SQLVARCHAR|I dati vengono inviati nella tabella codici del client o nella tabella codici implicita delle regole di confronto. L'effetto è lo stesso ottenuto specificando DATAFILETYPE **= 'char'** senza indicare un file di formato.|
|SQLNCHAR o SQLNVARCHAR|I dati vengono inviati in formato Unicode. L'effetto è lo stesso ottenuto specificando DATAFILETYPE **= 'widechar'** senza indicare un file di formato.|
|SQLBINARY o SQLVARBIN|I dati vengono inviati senza conversione.|
| &nbsp; | &nbsp; |

## <a name="general-remarks"></a>Osservazioni generali

Per un confronto tra l'istruzione BULK INSERT, l'istruzione INSERT ... SELECT \* FROM OPENROWSET(BULK...) e il comando **bcp**, vedere [Importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).

Per informazioni sulla preparazione dei dati per le operazioni di importazione bulk, vedere [Preparazione dei dati per l'importazione o l'esportazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

L'istruzione BULK INSERT può essere eseguita all'interno di una transazione definita dall'utente per importare dati in una tabella o una vista. Facoltativamente, per utilizzare più corrispondenze per l'importazione bulk dei dati, una transazione può consentire di specificare la clausola BATCHSIZE nell'istruzione BULK INSERT. Se una transazione con più batch viene sottoposta a rollback, tale operazione viene effettuata anche per ogni batch che è stato inviato a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dalla transazione.

## <a name="interoperability"></a>Interoperabilità

### <a name="importing-data-from-a-csv-file"></a>Importazione di dati da un file CSV

A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, BULK INSERT supporta il formato CSV, come il database SQL di Azure.
Prima di [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, i file con valori delimitati da virgole (CSV) non sono supportati nelle operazioni di importazione bulk di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In alcuni casi, tuttavia, è possibile usare un file CSV come file di dati per un'importazione bulk di dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per informazioni sui requisiti per l'importazione di dati da un file di dati CSV, vedere [Preparare i dati per l'importazione o l'esportazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

## <a name="logging-behavior"></a>Comportamento di registrazione

 Per informazioni sui casi in cui le operazioni di inserimento di righe eseguite durante l'importazione in blocco in SQL Server vengono registrate nel log delle transazioni, vedere [Prerequisiti per la registrazione minima nell'importazione in blocco](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md). La registrazione minima non è supportata nel database SQL di Azure.

## <a name="restrictions"></a><a name="Limitations"></a> Restrizioni

Quando si utilizza un file di formato con BULK INSERT, è possibile specificare al massimo 1024 campi. Si tratta dello stesso numero massimo di colonne consentite in una tabella. Se si usa un file di formato con BULK INSERT insieme a un file di dati contenente più di 1024 campi, verrà generato l'errore 4822. L'[utilità bcp](../../tools/bcp-utility.md) non ha questa limitazione. Pertanto, per i file di dati contenenti più di 1024 campi, usare BULK INSERT senza un file di formato oppure usare il comando **bcp**.

## <a name="performance-considerations"></a>Considerazioni sulle prestazioni

Se il numero di pagine da scaricare in un singolo batch supera una soglia interna, è possibile che venga eseguita un'analisi completa del pool di buffer per identificare le pagine da scaricare durante il commit del batch. L'analisi completa può influire sulle prestazioni dell'importazione bulk. Un probabile caso di superamento della soglia interna si verifica quando un pool di buffer di grandi dimensioni è combinato a un sottosistema di I/O lento. Per evitare gli overflow del buffer in sistemi di grandi dimensioni, non utilizzare l'hint TABLOCK, che elimina l'ottimizzazione delle operazioni bulk, o utilizzare un batch di dimensioni minori, che mantiene l'ottimizzazione delle operazioni bulk.

È consigliabile testare diverse dimensioni del batch con il caricamento dei dati per individuare la soluzione più adatta per il computer in uso.

Se si usa il database SQL di Azure e si esegue l'importazione di una quantità elevata di dati, valutare l'opportunità di aumentare temporaneamente il livello delle prestazioni del database o dell'istanza prima dell'importazione.

## <a name="security"></a>Sicurezza

### <a name="security-account-delegation-impersonation"></a>Delega degli account di sicurezza (rappresentazione)

Se un utente utilizza un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , viene utilizzato il profilo di sicurezza dell'account del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un account di accesso basato sull'autenticazione di SQL Server non può essere autenticato all'esterno del motore di database. Pertanto, quando un comando BULK INSERT viene avviato da un account di accesso che utilizza l'autenticazione di SQL, la connessione ai dati viene effettuata utilizzando il contesto di sicurezza dell'account del processo di SQL Server (l'account utilizzato dal servizio Motore di database di SQL Server). Per una lettura corretta dei dati di origine è necessario concedere all'account utilizzato dal motore di database di SQL Server l'accesso ai dati di origine. Al contrario, un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che esegue l'accesso utilizzando l'autenticazione di Windows potrà leggere solo i file a cui l'account utente può accedere, indipendentemente dal profilo di sicurezza del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Quando si esegue l'istruzione BULK INSERT con **sqlcmd** o **osql** da un computer, si inseriscono dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un secondo computer e si specifica un parametro *data_file* in un terzo computer usando un percorso UNC, è possibile che venga restituito l'errore 4861.

Per risolvere l'errore, usare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e specificare un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che usa il profilo di sicurezza dell'account del processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure configurare Windows per consentire la delega degli account di sicurezza. Per informazioni sull'abilitazione di un account utente in modo che venga considerato attendibile per la delega, vedere la Guida di Windows.

Per altre considerazioni sulla sicurezza, vedere [Importare dati per operazioni bulk con BULK INSERT o OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Quando si esegue l'importazione da Archiviazione BLOB di Azure e i dati non sono pubblici (accesso anonimo), creare [credenziali con ambito database](../../t-sql/statements/create-database-scoped-credential-transact-sql.md) basate su una chiave di firma di accesso condiviso crittografata con una [chiave master](create-master-key-transact-sql.md) e quindi creare un'[origine del database esterna](../../t-sql/statements/create-external-data-source-transact-sql.md) da usare nel comando BULK INSERT. Per un esempio, vedere [Importazione di dati da un file nell'archiviazione BLOB di Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

### <a name="permissions"></a>Autorizzazioni

Sono necessarie le autorizzazioni INSERT e ADMINISTER BULK OPERATIONS. Nel database SQL di Azure, sono necessarie le autorizzazioni INSERT e ADMINISTER DATABASE BULK OPERATIONS. Le autorizzazioni ADMINISTER BULK OPERATIONS o il ruolo bulkadmin non sono supportati per SQL Server in Linux. Solo il ruolo `sysadmin` può eseguire inserimenti bulk per SQL Server in Linux. 

È inoltre richiesta l'autorizzazione ALTER TABLE se si verificano una o più delle condizioni seguenti:

- Sono presenti vincoli e l'opzione CHECK_CONSTRAINTS non è specificata.

   > [!NOTE]
   > Per impostazione predefinita, i vincoli sono disabilitati. Per verificare i vincoli in modo esplicito, utilizzare l'opzione CHECK_CONSTRAINTS.

- Sono presenti trigger e l'opzione FIRE_TRIGGER non è specificata.

   > [!NOTE]
   > Per impostazione predefinita, i trigger non sono attivati. Per attivare i trigger in modo esplicito, utilizzare l'opzione FIRE_TRIGGERS.

- Si utilizza l'opzione KEEPIDENTITY per importare valori Identity dal file di dati.

## <a name="examples"></a>Esempi

### <a name="a-using-pipes-to-import-data-from-a-file"></a>R. Utilizzo di barre verticali per l'importazione di dati da un file

Nell'esempio seguente vengono importate informazioni dettagliate sugli ordini nella tabella `AdventureWorks2012.Sales.SalesOrderDetail` dal file di dati specificato, utilizzando una barra verticale (`|`) come carattere di terminazione del campo e la combinazione `|\n` come carattere di terminazione della riga.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
      (  
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR =' |\n'
      );
```

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="b-using-the-fire_triggers-argument"></a>B. Utilizzo dell'argomento FIRE_TRIGGERS

Nell'esempio seguente viene specificato l'argomento `FIRE_TRIGGERS`.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
     (
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR = ':\n'
         , FIRE_TRIGGERS
      );
```

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="c-using-line-feed-as-a-row-terminator"></a>C. Utilizzo del carattere di avanzamento riga come carattere di terminazione della riga

 Nell'esempio seguente viene importato un file che utilizza il carattere di avanzamento riga come carattere di terminazione della riga, come nel caso di output UNIX:

```sql
DECLARE @bulk_cmd VARCHAR(1000);
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
FROM ''<drive>:\<path>\<filename>''
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';
EXEC(@bulk_cmd);
```

> [!NOTE]
> A causa della modalità di gestione dei file di testo in Microsoft Windows, **(\n** viene automaticamente sostituito con **\r\n)** .

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="d-specifying-a-code-page"></a>D. Definizione di una tabella codici

L'esempio seguente illustra come specificare una tabella codici.

```sql
BULK INSERT MyTable
FROM 'D:\data.csv'
WITH
( CODEPAGE = '65001'
   , DATAFILETYPE = 'char'
   , FIELDTERMINATOR = ','
);
```

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="e-importing-data-from-a-csv-file"></a>E. Importazione di dati da un file CSV

L'esempio seguente illustra come specificare un file CSV, ignorando l'intestazione (prima riga), usando `;` come carattere di terminazione del campo e `0x0a` come carattere di terminazione della riga:

```sql
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'
      , FIRSTROW=2
      , FIELDQUOTE = '\'
      , FIELDTERMINATOR = ';'
      , ROWTERMINATOR = '0x0a');
```

> [!IMPORTANT]
> Database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>F. Importazione di dati da un file nell'archiviazione BLOB di Azure

L'esempio seguente illustra come caricare dati da un file con estensione csv in una posizione di Archiviazione BLOB di Azure in cui è stata creata una chiave di firma di accesso condiviso. La posizione di Archiviazione BLOB di Azure è configurata come origine dati esterna. Questa operazione richiede credenziali con ambito database che usano una firma di accesso condiviso crittografata con una chiave master nel database utente.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/invoices'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

> [!IMPORTANT]
> SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="g-importing-data-from-a-file-in-azure-blob-storage-and-specifying-an-error-file"></a>G. Importazione di dati da un file nell'archiviazione BLOB di Azure e specifica di un file degli errori

L'esempio seguente illustra come caricare dati da un file csv in una posizione di archiviazione BLOB di Azure configurata come origine dati esterna e come specificare un file degli errori. Ciò richiede credenziali con ambito database tramite una firma di accesso condiviso. Si noti che, se in esecuzione nel database SQL di Azure, l'opzione ERRORFILE deve essere accompagnata da ERRORFILE_DATA_SOURCE. In caso contrario l'importazione potrebbe non riuscire con un messaggio di errore relativo alle autorizzazioni. Il file specificato in ERRORFILE non deve esistere nel contenitore.

```sql
BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (
         DATA_SOURCE = 'MyAzureInvoices'
         , FORMAT = 'CSV'
         , ERRORFILE = 'MyErrorFile'
         , ERRORFILE_DATA_SOURCE = 'MyAzureInvoices');
```

Per esempi di `BULK INSERT` completi che includono la configurazione di credenziali e di un'origine dati esterna, vedere [Esempi di accesso bulk ai dati nell'archiviazione BLOB di Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="additional-examples"></a>Esempi aggiuntivi

 Altri esempi relativi a `BULK INSERT` sono disponibili negli argomenti seguenti:

- [Esempi di importazione ed esportazione bulk di documenti XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Mantenere i valori Identity durante l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Mantenimento dei valori Null o uso dei valori predefiniti durante un'importazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)
- [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)
- [Utilizzo del formato carattere Unicode per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
- [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)
- [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Vedere anche

- [Informazioni sull'importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [Utilità bcp](../../tools/bcp-utility.md)
- [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)
- [Preparare i dati per l'importazione o l'esportazione in blocco &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)