---
title: COPY INTO (Transact-SQL)
titleSuffix: (Azure Synapse Analytics) - SQL Server
description: Usare l'istruzione COPY in Azure Synapse Analytics per il caricamento da account di archiviazione esterni.
ms.date: 09/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COPY_TSQL
- COPY INTO
- COPY
- LOAD
dev_langs:
- TSQL
author: kevinvngo
ms.author: kevin
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fb52e9e869c0210e8e320c11dfec21aa8f7d585e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076809"
---
# <a name="copy-transact-sql"></a>COPY (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Questo articolo illustra come usare l'istruzione COPY in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] per il caricamento da account di archiviazione esterni. L'istruzione COPY offre la massima flessibilità per l'inserimento di dati con velocità effettiva elevata in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. Usare COPY per le funzionalità seguenti:

- Usare utenti con privilegi inferiori per caricare senza richiedere autorizzazioni CONTROL rigorose per il data warehouse
- Eseguire una sola istruzione T-SQL senza dover creare oggetti di database aggiuntivi
- Analizzare e caricare correttamente i file CSV in cui i **delimitatori** (stringa, campo, riga) **sono** **preceduti da un carattere di escape all'interno di colonne delimitate da stringhe**
- Specificare un modello di autorizzazione più preciso senza esporre le chiavi dell'account di archiviazione usando le firme di accesso condiviso (SAS)
- Usare un account di archiviazione diverso per il percorso di ERRORFILE (REJECTED_ROW_LOCATION)
- Personalizzare i valori predefiniti per ogni colonna di destinazione e specificare i campi dei dati di origine da caricare in colonne di destinazione specifiche
- Specificare un carattere di terminazione riga personalizzato per i file CSV
- Usare i formati di data di SQL Server per i file CSV
- Specificare caratteri jolly e più file nel percorso della posizione di archiviazione

Per esempi completi e argomenti di avvio rapido relativi all'istruzione COPY, vedere la documentazione seguente:

- [Avvio rapido: Caricamento bulk di dati con l'istruzione COPY](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
- [Avvio rapido: Esempi di utilizzo dell'istruzione COPY e dei metodi di autenticazione supportati](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples)
- [Avvio rapido: Creazione dell'istruzione COPY con l'avanzata interfaccia utente di Synapse Studio](/azure/synapse-analytics/quickstart-load-studio-sql-pool)

## <a name="syntax"></a>Sintassi  

```syntaxsql
COPY INTO [schema.]table_name
[(Column_list)] 
FROM '<external_location>' [,...n]
WITH  
 ( 
 [FILE_TYPE = {'CSV' | 'PARQUET' | 'ORC'} ]
 [,FILE_FORMAT = EXTERNAL FILE FORMAT OBJECT ]  
 [,CREDENTIAL = (AZURE CREDENTIAL) ]
 [,ERRORFILE = '[http(s)://storageaccount/container]/errorfile_directory[/]]' 
 [,ERRORFILE_CREDENTIAL = (AZURE CREDENTIAL) ]
 [,MAXERRORS = max_errors ] 
 [,COMPRESSION = { 'Gzip' | 'DefaultCodec'| 'Snappy'}] 
 [,FIELDQUOTE = 'string_delimiter'] 
 [,FIELDTERMINATOR =  'field_terminator']  
 [,ROWTERMINATOR = 'row_terminator']
 [,FIRSTROW = first_row]
 [,DATEFORMAT = 'date_format'] 
 [,ENCODING = {'UTF8'|'UTF16'}] 
 [,IDENTITY_INSERT = {'ON' | 'OFF'}]
)
```

## <a name="arguments"></a>Argomenti  

*schema_name*  
Argomento facoltativo se lo schema predefinito per l'utente che esegue l'operazione corrisponde allo schema della tabella specificata. Se non si specifica *schema* e lo schema predefinito dell'utente che esegue l'operazione COPY è diverso dalla tabella specificata, l'operazione COPY viene annullata e viene restituito un messaggio di errore.  

*table_name*  
Nome della tabella in cui copiare i dati con l'istruzione COPY. La tabella di destinazione può essere temporanea o permanente e deve già esistere nel database. 

*(column_list)*  
Elenco facoltativo di una o più colonne usate per eseguire il mapping dei campi dati di origine alle colonne della tabella di destinazione per il caricamento dei dati. Il valore di *column_list* deve essere racchiuso tra parentesi e delimitato da virgole. L'elenco di colonne ha il formato seguente:

[(Column_name [Default_value] [Field_number] [,...n])]

- *Column_name* - Nome della colonna nella tabella di destinazione.
- *Default_value* - Valore predefinito che sostituisce eventuali valori NULL nel file di input. Il valore predefinito si applica a tutti i formati di file. L'istruzione COPY tenterà di caricare NULL dal file di input quando una colonna viene omessa dall'elenco di colonne o quando è presente un campo vuoto nel file di input.
- *Field_number* - Numero del campo nel file di input che verrà mappato al nome della colonna di destinazione.
- L'indicizzazione del campo inizia da 1.

Se non si specifica un elenco di colonne, l'istruzione COPY esegue il mapping delle colonne in base all'ordine degli elementi di origine e di destinazione: il campo di input 1 viene mappato alla colonna di destinazione 1, il campo 2 alla colonna 2 e così via.

*Percorsi esterni*</br>
Posizione di gestione temporanea dei file contenenti i dati. Attualmente sono supportati Azure Data Lake Storage (ADLS) Gen2 e Archiviazione BLOB di Azure:

- *Percorso esterno* per Archiviazione BLOB: https://<account>.blob.core.windows.net/<container>/<path>
- *Percorso esterno* per ADLS Gen2: https://<account>. dfs.core.windows.net/<container>/<path>

> [!NOTE]  
> L'endpoint .blob è disponibile anche per ADLS Gen2 e attualmente garantisce le migliori prestazioni. Usare l'endpoint .blob quando non è richiesto .dfs per il metodo di autenticazione.

- *Account* - Nome dell'account di archiviazione

- *Contenitore* - Nome del contenitore BLOB

- *Percorso* - Percorso di file o cartella per i dati. Il percorso inizia dal contenitore. Se viene specificata una cartella, COPY recupera tutti i file dalla cartella e da tutte le relative sottocartelle. COPY ignora le cartelle nascoste e non restituisce i file che iniziano con un carattere di sottolineatura (_) o un punto (.), a meno che non venga specificato in modo esplicito nel percorso. Questo comportamento è identico anche quando si specifica un percorso con un carattere jolly.

È possibile includere caratteri jolly nel percorso, con le regole seguenti:

- Per la corrispondenza del nome del percorso con un carattere jolly viene fatta distinzione tra maiuscole e minuscole
- È possibile applicare una sequenza di escape al carattere jolly usando la barra rovesciata (\\)
- L'espansione del carattere jolly viene applicata in modo ricorsivo. Ad esempio, con l'istruzione seguente vengono caricati tutti i file CSV in Customer1, incluse le sottodirectory di Customer1: ‘Account/Container/Customer1/*.csv’

> [!NOTE]  
> Per ottenere prestazioni ottimali, evitare di specificare caratteri jolly che prevedono l'espansione in un numero elevato di file. Se possibile, elencare più percorsi di file invece di specificare caratteri jolly.

È possibile specificare più percorsi di file inclusi nello stesso account di archiviazione e contenitore usando un elenco delimitato da virgole, ad esempio:

- ‘ https://<account>.blob.core.windows.net/<container>/<path>’, ‘ https://<account>.blob.core.windows.net/<container>/<path>’…

*FILE_TYPE = { ‘CSV’ | ‘PARQUET’ | ‘ORC’ }*</br>
*FILE_TYPE* specifica il formato dei dati esterni.

- CSV: specifica un file di valori delimitati da virgole conforme allo standard [RFC 4180](https://tools.ietf.org/html/rfc4180).
- PARQUET: specifica un formato Parquet.
- ORC: Specifica un formato ORC (Optimized Row Columnar).

>[!NOTE]  
>Il tipo di file "Delimited Text" in Polybase viene sostituito dal formato di file "CSV", in cui è possibile configurare il delimitatore virgola predefinito tramite il parametro FIELDTERMINATOR. 

*FILE_FORMAT = external_file_format_name*</br>
*FILE_FORMAT* si applica solo ai file Parquet e ORC e specifica il nome dell'oggetto formato di file esterno che archivia il tipo di file e il metodo di compressione per i dati esterni. Per creare un formato di file esterno, usare [CREATE EXTERNAL FILE FORMAT](create-external-file-format-transact-sql.md).

*CREDENTIAL (IDENTITY = ‘’, SECRET = ‘’)*</br>
*CREDENTIAL* specifica il meccanismo di autenticazione per accedere all'account di archiviazione esterno. I metodi di autenticazione sono i seguenti:

|                          |                CSV                |                      Parquet                       |                        ORC                         |
| :----------------------: | :-------------------------------: | :------------------------------------------------: | :------------------------------------------------: |
|  **Archiviazione BLOB di Azure**  | FIRMA DI ACCESSO CONDIVISO/IDENTITÀ DEL SERVIZIO GESTITA/ENTITÀ SERVIZIO/CHIAVE/AAD |                      FIRMA DI ACCESSO CONDIVISO/CHIAVE                       |                      FIRMA DI ACCESSO CONDIVISO/CHIAVE                       |
| **Azure Data Lake Gen2** | FIRMA DI ACCESSO CONDIVISO/IDENTITÀ DEL SERVIZIO GESTITA/ENTITÀ SERVIZIO/CHIAVE/AAD | FIRMA DI ACCESSO CONDIVISO (blob<sup>1</sup>)/IDENTITÀ DEL SERVIZIO GESTITA (dfs<sup>2</sup>)/ENTITÀ SERVIZIO/CHIAVE/AAD | FIRMA DI ACCESSO CONDIVISO (blob<sup>1</sup>)/IDENTITÀ DEL SERVIZIO GESTITA (dfs<sup>2</sup>)/ENTITÀ SERVIZIO/CHIAVE/AAD |

1: per questo metodo di autenticazione è necessario l'endpoint .blob ( **.blob**.core.windows.net) nel percorso esterno.

2: per questo metodo di autenticazione è necessario l'endpoint .dfs ( **.dfs**.core.windows.net) nel percorso esterno.


> [!NOTE]  
>
> - Quando si esegue l'autenticazione con AAD o in un account di archiviazione pubblico, non è necessario specificare CREDENTIAL. 
> - Se l'account di archiviazione è associato a una VNet, è necessario eseguire l'autenticazione con MSI (identità gestita).

- Autenticazione con firme di accesso condiviso (SAS)
  
  - *IDENTITY: costante con valore ‘Shared Access Signature’*
  - *SECRET: La* [*firma di accesso condiviso*](/azure/storage/common/storage-sas-overview) *fornisce accesso delegato controllato alle risorse dell'account di archiviazione*.
  -  Autorizzazioni minime richieste: READ e LIST
  
- Autenticazione con [*entità servizio*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)

  - *IDENTITY: <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET: chiave dell'entità servizio dell'applicazione AAD*
  -  Ruoli Controllo degli accessi in base al ruolo minimi richiesti: Collaboratore ai dati dei BLOB di archiviazione, Proprietario dei dati dei BLOB di archiviazione o Lettore dei dati dei BLOB di archiviazione

- Autenticazione con chiave dell'account di archiviazione
  
  - *IDENTITY: costante con valore ‘Storage Account Key’*
  - *SECRET: chiave dell'account di archiviazione*
  
- Autenticazione con [identità gestita](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (endpoint servizio di rete virtuale)
  
  - *IDENTITY: costante con valore ‘Managed Identity’*
  - Ruoli Controllo degli accessi in base al ruolo minimi richiesti: Collaboratore ai dati dei BLOB di archiviazione o Proprietario dei dati dei BLOB di archiviazione per il server di database SQL registrato con AAD
  
- Autenticazione con utente di AAD
  
  - *Non è necessario specificare CREDENTIAL*
  - Ruoli Controllo degli accessi in base al ruolo minimi richiesti: Collaboratore ai dati dei BLOB di archiviazione o Proprietario dei dati dei BLOB di archiviazione per l'utente di AAD

*ERRORFILE = Percorso directory*</br>
*ERRORFILE* si applica solo al formato CSV. Specifica la directory all'interno dell'istruzione COPY in cui scrivere le righe rifiutate e il file di errori corrispondente. È possibile specificare il percorso completo dall'account di archiviazione oppure il percorso relativo al contenitore. Se il percorso specificato non esiste, viene creato automaticamente. Viene creata una directory figlio con nome "_rejectedrows". Il carattere "_ " assicura che la directory venga ignorata da altre attività di elaborazione dati, salvo se indicata in modo esplicito nel parametro del percorso. 

Questa directory include una cartella creata in base all'ora di inoltro del carico, con il formato AnnoMeseGiorno - OraMinutoSecondo (ad esempio 20180330-173205). In questa cartella vengono scritti due tipi di file, il file relativo al motivo (errore) e il file di dati (riga), anteponendo a ognuno i valori di queryID, distributionID e un GUID di file. Poiché i dati e il motivo si trovano in file distinti, i file corrispondenti hanno un prefisso corrispondente.

Se per ERRORFILE viene definito il percorso completo dell'account di archiviazione, la connessione a tale account viene stabilita usando ERRORFILE_CREDENTIAL. In caso contrario, viene usato il valore indicato per CREDENTIAL.

*ERRORFILE_CREDENTIAL = (IDENTITY= ‘’, SECRET = ‘’)*</br>
*ERRORFILE_CREDENTIAL* si applica solo ai file CSV. Le origini dati e i metodi di autenticazione supportati sono i seguenti:

- Archiviazione BLOB di Azure - FIRMA DI ACCESSO CONDIVISO/ENTITÀ SERVIZIO/CHIAVE/AAD
- Azure Data Lake Gen2 - FIRMA DI ACCESSO CONDIVISO/IDENTITÀ DEL SERVIZIO GESTITA/ENTITÀ SERVIZIO/CHIAVE/AAD
  
- Autenticazione con firme di accesso condiviso (SAS)
  - *IDENTITY: costante con valore ‘Shared Access Signature’*
  - *SECRET: La* [*firma di accesso condiviso*](/azure/storage/common/storage-sas-overview) *fornisce accesso delegato controllato alle risorse dell'account di archiviazione*.
  - Autorizzazioni minime richieste: READ, LIST, WRITE, CREATE, DELETE
  
- Autenticazione con [*entità servizio*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)
  - *IDENTITY: <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET: chiave dell'entità servizio dell'applicazione AAD*
  - Ruoli Controllo degli accessi in base al ruolo minimi richiesti: Collaboratore ai dati dei BLOB di archiviazione o Proprietario dei dati dei BLOB di archiviazione
  
> [!NOTE]  
> Usare l'endpoint di token OAuth 2.0 **V1**

- Autenticazione con chiave dell'account di archiviazione
  - *IDENTITY: costante con valore ‘Storage Account Key’*
  - *SECRET: chiave dell'account di archiviazione*
  
- Autenticazione con [identità gestita](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (endpoint servizio di rete virtuale)
  - *IDENTITY: costante con valore ‘Managed Identity’*
  - Ruoli Controllo degli accessi in base al ruolo minimi richiesti: Collaboratore ai dati dei BLOB di archiviazione o Proprietario dei dati dei BLOB di archiviazione per il server di database SQL registrato con AAD
  
- Autenticazione con utente di AAD
  - *Non è necessario specificare CREDENTIAL*
  - Ruoli Controllo degli accessi in base al ruolo minimi richiesti: Collaboratore ai dati dei BLOB di archiviazione o Proprietario dei dati dei BLOB di archiviazione per l'utente di AAD

> [!NOTE]  
> Se si usa lo stesso account di archiviazione per ERRORFILE e si specifica il percorso di ERRORFILE relativo alla radice del contenitore, non è necessario specificare ERROR_CREDENTIAL.

*MAXERRORS = max_errors*</br>
*MAXERRORS* specifica il numero massimo consentito di righe rifiutate nel caricamento prima che l'operazione COPY venga annullata. Ogni riga che non è possibile importare tramite l'operazione COPY viene ignorata e conteggiata come errore. Se il valore di max_errors non viene specificato, il valore predefinito è 0.

*COMPRESSION = { 'DefaultCodec '\| ’Snappy’ \| ‘GZIP’ \| ‘NONE’}*</br>
*COMPRESSION* è un argomento facoltativo e specifica il metodo di compressione dei dati per i dati esterni.

- Il formato CSV supporta GZIP
- Il formato Parquet supporta GZIP e Snappy
- Il formato ORC supporta DefaultCodec e Snappy
- Zlib è il tipo di compressione predefinito per ORC

Se questo parametro non è specificato, il comando COPY rileva automaticamente il tipo di compressione in base all'estensione del file:

- Estensione gz - **GZIP**
- Estensione snappy - **Snappy**
- .deflate - **DefaultCodec** (solo Parquet e ORC)

 *FIELDQUOTE = 'field_quote'*</br>
*FIELDQUOTE* si applica al formato CSV e specifica un singolo carattere che verrà usato come carattere virgolette (delimitatore di stringa) nel file CSV. Se non viene specificato alcun valore, viene usato il carattere virgolette (") come definito nello standard RFC 4180. I caratteri ASCII e multibyte estesi non sono supportati con UTF-8 per FIELDQUOTE.

> [!NOTE]  
> Ai caratteri FIELDQUOTE viene applicata una sequenza di escape nelle colonne stringa in cui è presente un oggetto FIELDQUOTE (delimitatore) doppio. 

*FIELDTERMINATOR = 'field_terminator’*</br>
*FIELDTERMINATOR* si applica solo al formato CSV. Specifica il carattere di terminazione del campo che verrà usato nel file CSV. Il carattere di terminazione del campo può essere specificato usando la notazione esadecimale. Il carattere di terminazione del campo può essere costituito da più caratteri. Il carattere di terminazione del campo predefinito è una virgola (,). I caratteri ASCII e multibyte estesi non sono supportati con UTF-8 per FIELDTERMINATOR.

ROW TERMINATOR = 'row_terminator'</br>
*ROW TERMINATOR* si applica solo al formato CSV. Specifica il carattere di terminazione della riga che verrà usato nel file CSV. Il carattere di terminazione della riga può essere specificato usando la notazione esadecimale. Il carattere di terminazione della riga può essere costituito da più caratteri. Per impostazione predefinita, il carattere di terminazione della riga è \r\n. 

Il comando COPY aggiunge come prefisso il carattere \r quando si specifica \n (nuova riga), quindi il risultato è \r\n. Per specificare solo il carattere \n, usare la notazione esadecimale (0x0A). Quando si specificano caratteri di terminazione della riga costituiti da più caratteri in formato esadecimale, non specificare 0x tra i singoli caratteri.

I caratteri ASCII e multibyte estesi non sono supportati con UTF-8 per ROW TERMINATOR.

*FIRSTROW  = First_row_int*</br>
*FIRSTROW* si applica al formato CSV e specifica il numero della riga che viene letta per prima in tutti i file con il comando COPY. I valori iniziano da 1, che è il valore predefinito. Se il valore è impostato su due, la prima riga di ogni file (riga di intestazione) viene ignorata quando i dati vengono caricati. Le righe vengono ignorate in base alla presenza di caratteri di terminazione della riga.

*DATEFORMAT = { ‘mdy’ \| ‘dmy’ \| ‘ymd’ \| ‘ydm’ \| ‘myd’ \| ‘dym’ }*</br>
DATEFORMAT si applica solo al formato CSV e specifica il formato di data per il mapping della data ai formati di data di SQL Server. Per una panoramica di tutte le funzioni e i tipi di dati di data e ora Transact-SQL, vedere [Funzioni e tipi di dati di data e ora (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md). DATEFORMAT all'interno del comando COPY ha la precedenza su [DATEFORMAT configurato a livello di sessione](set-dateformat-transact-sql.md).

*ENCODING = ‘UTF8’ | ‘UTF16’*</br>
*ENCODING* si applica solo al formato CSV. L'impostazione predefinita è UTF8. Specifica lo standard di codifica dei dati per i file caricati tramite il comando COPY. 

*IDENTITY_INSERT = ‘ON’ | ‘OFF’*</br>
IDENTITY_INSERT specifica se il valore o i valori Identity presenti nel file di dati importato devono essere usati per la colonna Identity. Se il valore di IDENTITY_INSERT è OFF (impostazione predefinita), i valori Identity per questa colonna vengono verificati ma non importati. Azure Synapse Analytics assegna automaticamente valori univoci in base ai valori di inizializzazione e incremento specificati durante la creazione della tabella. Si noti il comportamento seguente con il comando COPY:

- Se IDENTITY_INSERT è OFF e la tabella contiene una colonna Identity
  - È necessario specificare un elenco di colonne che non esegue il mapping di un campo di input alla colonna Identity.
- Se IDENTITY_INSERT è ON e la tabella contiene una colonna Identity
  - Se viene passato un elenco di colonne, deve eseguire il mapping di un campo di input alla colonna Identity.
- Il valore predefinito non è supportato per la colonna IDENTITY nell'elenco di colonne.
- È possibile impostare IDENTITY_INSERT solo per una tabella alla volta.

### <a name="permissions"></a>Autorizzazioni  

L'utente che esegue il comando Copy deve avere le autorizzazioni seguenti: 

- [ADMINISTER DATABASE BULK OPERATIONS](grant-database-permissions-transact-sql.md#remarks)
- [INSERT ](grant-database-permissions-transact-sql.md#remarks)

Sono necessarie le autorizzazioni INSERT e ADMINISTER BULK OPERATIONS. In [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], sono necessarie le autorizzazioni INSERT e ADMINISTER DATABASE BULK OPERATIONS.

## <a name="examples"></a>Esempi  

### <a name="a-load-from-a-public-storage-account"></a>R. Eseguire il caricamento da un account di archiviazione pubblico

L'esempio seguente mostra la forma più semplice del comando COPY, che carica i dati da un account di archiviazione pubblico. Per questo esempio, le impostazioni predefinite dell'istruzione COPY corrispondono al formato del file CSV di lineitem.

```sql
COPY INTO dbo.[lineitem] FROM 'https://unsecureaccount.blob.core.windows.net/customerdatasets/folder1/lineitem.csv'
```

I valori predefiniti del comando COPY sono i seguenti:

- DATEFORMAT = Valore DATEFORMAT della sessione 

- MAXERRORS = 0

- COMPRESSION - Per impostazione predefinita non viene applicata la compressione

- FIELDQUOTE = “” 

- FIELDTERMINATOR = “,” 

- ROWTERMINATOR = ‘\n'

> [!IMPORTANT]
> COPY tratta ‘\n’ come ‘\r\n’ internamente. Per altre informazioni, vedere la sezione ROWTERMINATOR.

- FIRSTROW = 1

- ENCODING = ‘UTF8’

- FILE_TYPE = ‘CSV’

- IDENTITY_INSERT = ‘OFF’

### <a name="b-load-authenticating-via-share-access-signature-sas"></a>B. Eseguire il caricamento con l'autenticazione tramite firma di accesso condiviso

L'esempio seguente carica file che usano il carattere di avanzamento riga come carattere di terminazione della riga, come nel caso di un output UNIX. Questo esempio usa anche una chiave di firma di accesso condiviso per l'autenticazione nell'Archiviazione BLOB di Azure.

```sql
COPY INTO test_1
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSHFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=';',
    ROWTERMINATOR='0X0A',
    ENCODING = 'UTF8',
    DATEFORMAT = 'ymd',
    MAXERRORS = 10,
    ERRORFILE = '/errorsfolder',--path starting from the storage container
    IDENTITY_INSERT = 'ON'
)
```

### <a name="c-load-with-a-column-list-with-default-values-authenticating-via-storage-account-key"></a>C. Eseguire il caricamento con un elenco di colonne con valori predefiniti e con l'autenticazione tramite chiave dell'account di archiviazione

Questo esempio carica i file specificando un elenco di colonne con valori predefiniti. 

```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO test_1 (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0PQW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=',',
    ROWTERMINATOR='0x0A',
    ENCODING = 'UTF8',
    FIRSTROW = 2
)
```

### <a name="d-load-parquet-or-orc-using-existing-file-format-object"></a>D. Eseguire il caricamento di Parquet o ORC usando un oggetto formato di file esistente

 Questo esempio usa un carattere jolly per caricare tutti i file Parquet in una cartella. 

```sql
COPY INTO test_parquet
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.parquet'
WITH (
    FILE_FORMAT = myFileFormat,
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
)
```

### <a name="e-load-specifying-wild-cards-and-multiple-files"></a>E. Eseguire il caricamento specificando caratteri jolly e più file

```sql
COPY INTO t1
FROM 
'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt', 
    'https://myaccount.blob.core.windows.net/myblobcontainer/folder1'
WITH ( 
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= '<client_id>@<OAuth_2.0_Token_EndPoint>',SECRET='<key>'),
    FIELDTERMINATOR = '|'
)
```

### <a name="f-load-using-msi-credentials"></a>F. Eseguire il caricamento con credenziali MSI

```sql
COPY INTO dbo.myCOPYDemoTable
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL = (IDENTITY = 'Managed Identity'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=','
)
```

## <a name="faq"></a>Domande frequenti

### <a name="what-is-the-performance-of-the-copy-command-compared-to-polybase"></a>Quali sono le prestazioni del comando COPY rispetto a PolyBase?
Il comando COPY avrà prestazioni migliori a seconda del carico di lavoro. Per ottimizzare le prestazioni, è consigliabile dividere l'input in più file durante il caricamento del file CSV. Questo materiale sussidiario si adatta anche ai file compressi gzip.

### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-csv-files"></a>Quali sono le linee guida per la divisione in file per il comando COPY che carica i file CSV?
Le linee guida per il numero di file sono descritte nella tabella seguente. Una volta raggiunto il numero consigliato di file, si otterranno prestazioni migliori per i file più grandi. Per una semplice esperienza di suddivisione dei file, vedere la [documentazione](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/how-to-maximize-copy-load-throughput-with-file-splits/ba-p/1314474) seguente. 

| **DWU** | **#Files** |
| :-----: | :--------: |
|   100   |     60     |
|   200   |     60     |
|   300   |     60     |
|   400   |     60     |
|   500   |     60     |
|  1\.000  |    120     |
|  1\.500  |    180     |
|  2\.000  |    240     |
|  2\.500  |    300     |
|  3,000  |    360     |
|  5\.000  |    600     |
|  6.000  |    720     |
|  7\.500  |    900     |
| 10,000  |    1200    |
| 15.000  |    1800    |
| 30.000  |    3600    |


### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-parquet-or-orc-files"></a>Quali sono le linee guida per la divisione in file per il comando COPY che carica file Parquet o ORC?
Non è necessario dividere i file Parquet e ORC perché il comando COPY dividerà automaticamente i file. Per ottenere prestazioni ottimali, i file Parquet e ORC nell'account di archiviazione di Azure devono avere dimensioni minime pari a 256 MB. 

### <a name="are-there-any-limitations-on-the-number-or-size-of-files"></a>Sono previste limitazioni per il numero o le dimensioni dei file?
Non sono previste limitazioni per il numero o le dimensioni dei file. Per assicurare prestazioni ottimali, tuttavia, è consigliabile usare file di almeno 4 MB.

### <a name="are-there-any-known-issues-with-the-copy-statement"></a>Esistono problemi noti con l'istruzione COPY?
Se si dispone di un'area di lavoro Synapse creata prima del 12/07/2020, è possibile che venga visualizzato un messaggio di errore simile quando si esegue l'autenticazione tramite identità gestita:

*com.microsoft.sqlserver.jdbc.SQLServerException: L'identità del servizio gestita non è stata abilitata in questo server. Abilitare l'identità del servizio gestita e riprovare.*

Seguire questa procedura per risolvere il problema registrando di nuovo l'identità gestita dell'area di lavoro:

1. Passare all'area di lavoro di Synapse nel portale di Azure
2. Passare al riquadro Identità gestite 
3. Se l'opzione "Consenti pipeline" è già selezionata, è necessario deselezionare questa impostazione e salvare
4. Selezionare l'opzione "Consenti pipeline" e salvare


## <a name="see-also"></a>Vedere anche  

 [Panoramica sul caricamento con [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/design-elt-data-loading)
