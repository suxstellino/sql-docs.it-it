---
description: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
title: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: conceptual
f1_keywords:
- CREATE EXTERNAL TABLE AS SELECT
- CREATE_EXTERNAL_TABLE_AS_SELECT
- CREATE EXTERNAL TABLE AS SELECT_TSQL
helpviewer_keywords:
- External, table
- PolyBase, external table
- External, table create as select
- PolyBase, create table as select
ms.assetid: 32dfe254-6df7-4437-bfd6-ca7d37557b0a
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 14a7dce61b50ced328c103ec88973e9bf7b18c7d
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345245"
---
# <a name="create-external-table-as-select-transact-sql"></a>CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crea una tabella esterna e quindi esporta, in parallelo, i risultati di un'istruzione SELECT di [!INCLUDE[tsql](../../includes/tsql-md.md)] in Hadoop o in Archiviazione BLOB di Azure.

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql 
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name   
    WITH (   
        LOCATION = 'hdfs_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
    AS <select_statement>  
[;]  
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage  
    | REJECT_VALUE = reject_value  
    | REJECT_SAMPLE_VALUE = reject_sample_value  
}  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```

## <a name="arguments"></a>Argomenti
 **[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name*** è il nome della tabella da creare nel database, composto da una, due o tre parti. Per una tabella esterna, solo i metadati della tabella vengono archiviati nel database relazionale. 

 **LOCATION =  '*hdfs_folder*'** specifica la posizione in cui scrivere i risultati dell'istruzione SELECT nell'origine dati esterna. La posizione è un nome di cartella e, se necessario, può includere un percorso relativo alla cartella radice del cluster Hadoop o di Archiviazione BLOB. PolyBase creerà il percorso e la cartella se non esistono già.

I file esterni vengono scritti in *hdfs_folder* e denominati *QueryID_date_time_ID.format*, dove *ID* è un identificatore incrementale e *format* è il formato dei dati esportati. Un esempio è QID776_20160130_182739_0.orc.

 **DATA_SOURCE = *external_data_source_name*** specifica il nome dell'oggetto origine dati esterna che contiene il percorso in cui sono o verranno archiviati i dati esterni. Il percorso è un cluster Hadoop o un'istanza di Archiviazione BLOB di Azure. Per creare un'origine dati esterna, usare [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).

 **FILE_FORMAT = *external_file_format_name*** specifica il nome dell'oggetto formato di file esterno che contiene il formato per il file di dati esterno. Per creare un formato di file esterno, usare [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).

 Le **opzioni di rifiuto** non si applicano al momento dell'esecuzione dell'istruzione CREATE EXTERNAL TABLE AS SELECT. Vengono invece specificate qui in modo che il database sia in grado di usarle in un secondo tempo quando importa i dati dalla tabella esterna. Successivamente, quando l'istruzione CREATE TABLE AS SELECT seleziona dati dalla tabella esterna, il database usa le opzioni di rifiuto per determinare il numero o la percentuale di righe che può non essere possibile importare prima di interrompere l'importazione.

   - **REJECT_VALUE = *reject_value*** specifica il valore o la percentuale di righe con potenziali errori di importazione prima che il database interrompa l'importazione.

   - **REJECT_TYPE = **value** | percentage** indica se l'opzione REJECT_VALUE è specificata come valore letterale o percentuale.

      - Il tipo **Value** viene usato se REJECT_VALUE è un valore letterale, non una percentuale. Il database interrompe l'importazione di righe dal file di dati esterno quando il numero di righe con esito negativo supera *reject_value*.

        Ad esempio, se REJECT_VALUE = 5 e REJECT_TYPE = value, il database interrompe l'importazione di righe dopo cinque righe che non possono essere importate.

      - Il tipo **Percentage** viene usato se REJECT_VALUE è una percentuale e non un valore letterale. Il database interrompe l'importazione di righe dal file di dati esterno quando la *percentuale* di righe con esito negativo supera *reject_value*. La percentuale di righe con esito negativo viene calcolata a intervalli.

   - **REJECT_SAMPLE_VALUE = *reject_sample_value*** è obbligatoria quando REJECT_TYPE = percentage. Specifica il numero di righe che è possibile provare a importare prima che il database ricalcoli la percentuale di righe con esito negativo.

      Ad esempio, se REJECT_SAMPLE_VALUE = 1000, il database calcola la percentuale di righe con esito negativo dopo che ha tentato di importare 1000 righe dal file di dati esterno. Se la percentuale di righe con esito negativo è inferiore al valore *reject_value*, il database tenterà di recuperare altre 1000 righe. Il database continua a ricalcolare la percentuale di righe con esito negativo dopo aver tentato di importare ognuna delle 1000 righe aggiuntive.

     > [!NOTE]
     >  Poiché il database calcola la percentuale di righe con esito negativo a intervalli, la percentuale effettiva di tali righe può superare *reject_value*.

     **Esempio:**

     Questo esempio illustra come le tre opzioni REJECT interagiscono tra loro. Ad esempio, se REJECT_TYPE = percentage, REJECT_VALUE = 30 e REJECT_SAMPLE_VALUE = 100, potrebbe verificarsi il seguente scenario:

     - Il database tenta di caricare le prime 100 righe, di cui 25 hanno esisto negativo e 75 esito positivo.
     - La percentuale di righe con esito negativo viene calcolata al 25%, che è minore del valore di rifiuto pari al 30%. Non è quindi necessario interrompere il carico.
     - Il database tenta di caricare le 100 righe successive. Questa volta 25 hanno esito positivo e 75 esito negativo.
     - La percentuale di righe con esito negativo viene ricalcolata al 50%. La percentuale di righe con esito negativo ha superato il valore di rifiuto del 30%.
     - Il caricamento non riesce con il 50% delle righe con esito negativo dopo aver tentato di caricare 200 righe, un valore superiore al limite del 30% specificato.

 **WITH *common_table_expression*** indica un set di risultati denominato temporaneo, noto come espressione di tabella comune (CTE). Per altre informazioni, vedere [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md) 

 **SELECT \<select_criteria>** popola la nuova tabella con i risultati di un'istruzione SELECT. *select_criteria* è il corpo dell'istruzione SELECT che determina i dati da copiare nella nuova tabella. Per informazioni sulle istruzioni SELECT, vedere [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).

## <a name="permissions"></a>Autorizzazioni

 Per eseguire questo comando, l'*utente del database* deve avere tutte le autorizzazioni o appartenenze seguenti:

- Autorizzazione **ALTER SCHEMA** per lo schema locale che conterrà la nuova tabella o appartenenza al ruolo predefinito del database **db_ddladmin**.
- Autorizzazione **CREATE TABLE** o appartenenza al ruolo predefinito del database **db_ddladmin**.
- Autorizzazione **SELECT** per tutti gli oggetti a cui si fa riferimento in *select_criteria*.

 L'account di accesso deve avere tutte le autorizzazioni che seguono:

- **ADMINISTER BULK OPERATIONS**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- Autorizzazione di **scrittura** per leggere e scrivere nella cartella esterna del cluster Hadoop o in Archiviazione BLOB.

 > [!IMPORTANT]
 >  L'autorizzazione ALTER ANY EXTERNAL DATA SOURCE concede a qualsiasi entità di sicurezza la possibilità di creare e modificare qualsiasi oggetto origine dati esterna e anche la possibilità di accedere a tutte le credenziali con ambito database nel database. Questa autorizzazione deve essere considerata con privilegi elevati e deve essere concessa solo a entità attendibili nel sistema.

## <a name="error-handling"></a>Gestione degli errori
 Quando CREATE EXTERNAL TABLE AS SELECT esporta dati in un file di testo delimitato, non esiste alcun file di rifiuto per le righe che non è possibile esportare.

 Quando si crea la tabella esterna, il database tenta di connettersi al cluster Hadoop esterno o ad Archiviazione BLOB. Se la connessione non riesce, il comando ha esito negativo e la tabella esterna non verrà creata. La conferma dell'esito negativo del comando può richiedere un minuto o più poiché il database ritenta la connessione almeno tre volte.

 Se CREATE EXTERNAL TABLE AS SELECT viene annullata o ha esito negativo, il database farà un solo tentativo di rimuovere eventuali nuovi file e cartelle già creati nell'origine dati esterna.

 Il database segnalerà eventuali errori Java che si verificano nell'origine dati esterna durante l'esportazione dei dati.

##  <a name="general-remarks"></a><a name="GeneralRemarks"></a> Osservazioni generali
 Eseguita l'istruzione CREATE EXTERNAL TABLE AS SELECT, è possibile eseguire query [!INCLUDE[tsql](../../includes/tsql-md.md)] sulla tabella esterna. Queste operazioni consentono di importare i dati nel database per la durata della query a meno che non si esegua l'importazione usando l'istruzione CREATE TABLE AS SELECT.

 Il nome e la definizione della tabella esterna vengono archiviati nei metadati del database. I dati vengono archiviati nell'origine dati esterna.

 I file esterni sono denominati *QueryID_date_time_ID.format*, dove *ID* è un identificatore incrementale e *format* è il formato dei dati esportati. Un esempio è QID776_20160130_182739_0.orc.

 L'istruzione CREATE EXTERNAL TABLE AS SELECT crea sempre una tabella non partizionata, anche se la tabella di origine è partizionata.

 Per i piani di query creati con EXPLAIN, il database usa queste operazioni del piano di query per le tabelle esterne:

- Spostamento esterno dei dati di riproduzione casuale
- Spostamento esterno dei dati trasmessi
- Spostamento esterno delle partizioni

 **Si applica a:** Parallel Data Warehouse

Come prerequisito per la creazione di una tabella esterna, l'amministratore dell'appliance deve configurare la connettività di Hadoop. Per altre informazioni, vedere l'argomento relativo alla configurazione delle connessioni ai dati esterni nella documentazione della piattaforma di strumenti analitici che può essere scaricata dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=48241).

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni
 Poiché i dati della tabella esterna si trovano all'esterno del database, le operazioni di backup e ripristino funzioneranno solo per i dati archiviati nel database. Ciò significa che il backup e il ripristino verranno eseguiti solo per i metadati.

 Il database non verifica la connessione all'origine dati esterna quando si ripristina un backup del database che contiene una tabella esterna. Se la risorsa originale non è accessibile, il ripristino dei metadati della tabella esterna avrà comunque esito positivo, a differenza delle operazioni SELECT eseguite sulla tabella esterna.

 Il database non garantisce la coerenza dei dati tra il database e i dati esterni. È responsabilità esclusiva del cliente mantenere la coerenza tra i dati esterni e il database.

 Le operazioni DML (Data Manipulation Language) non sono supportate per le tabelle esterne. Ad esempio, non è possibile usare le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE, INSERT o DELETE per modificare i dati esterni.

 CREATE TABLE, DROP TABLE, CREATE STATISTICS, DROP STATISTICS, CREATE VIEW e DROP VIEW sono le uniche operazioni DDL (Data Definition Language) consentite per le tabelle esterne.

 Non è possibile creare tabelle esterne per il pool SQL senza server in una posizione in cui sono presenti dati. Per riutilizzare un percorso utilizzato per archiviare i dati, il percorso deve essere eliminato manualmente in ADLS.

 PolyBase può utilizzare al massimo 33.000 file per cartella durante l'esecuzione di 32 query PolyBase simultanee. Questo numero massimo include i file e le sottocartelle presenti in ogni cartella HDFS. Se il livello di concorrenza è inferiore a 32, un utente può eseguire le query PolyBase sulle cartelle in HDFS che contengono più di 33.000 file. Per gli utenti di Hadoop e PolyBase, è consigliabile usare percorsi brevi per i file e non più di 30.000 file per ogni cartella HDFS. Quando si fa riferimento a troppi file, si verifica un'eccezione di memoria insufficiente in JVM.

 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) non ha alcun effetto su CREATE EXTERNAL TABLE AS SELECT. Per ottenere un comportamento simile, usare [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).

 Quando CREATE EXTERNAL TABLE AS SELECT esegue una selezione da un file RCFILE, i valori delle colonne del file RCFILE non devono contenere il carattere pipe "|".

CREATE EXTERNAL TABLE AS SELECT in file Parquet oppure ORC provocherà errori che possono includere record rifiutati quando i caratteri seguenti sono presenti nei dati:

- |
- “ (virgolette)
- /r/n
- /r
- /n

Per usare CREATE EXTERNAL TABLE AS SELECT con questi caratteri, è prima di tutto necessario eseguire l'istruzione CREATE EXTERNAL TABLE AS SELECT per esportare i dati in file di testo delimitati, che è poi possibile convertire nel formato Parquet oppure ORC con uno strumento esterno.

## <a name="locking"></a>Blocco
 Accetta un blocco condiviso per l'oggetto SCHEMARESOLUTION.

##  <a name="examples"></a><a name="Examples"></a> Esempi

### <a name="a-create-a-hadoop-table-by-using-create-external-table-as-select"></a>R. Creare una tabella di Hadoop usando CREATE EXTERNAL TABLE AS SELECT

 L'esempio seguente crea una nuova tabella esterna denominata `hdfsCustomer` che usa le definizioni e i dati delle colonne della tabella di origine `dimCustomer`.

 La definizione della tabella è archiviata nel database e i risultati dell'istruzione SELECT vengono esportati nel file "/pdwdata/customer.tbl" nell'origine dati esterna Hadoop *customer_ds*. Il file viene formattato in base al formato di file esterno *customer_ff*.

 Il nome del file è generato dal database e contiene l'ID di query per facilitare l'allineamento del file con la query che lo ha generato.

 Il percorso `hdfs://xxx.xxx.xxx.xxx:5000/files/` che precede la directory Customer deve essere già presente. Se la directory Customer non esiste, verrà creata dal database.

> [!NOTE]
>  In questo esempio viene specificato 5000. Se la porta non è specificata, il database usa 8020 come porta predefinita.

 Il percorso e nome file Hadoop risultanti saranno `hdfs:// xxx.xxx.xxx.xxx:5000/files/Customer/ QueryID_YearMonthDay_HourMinutesSeconds_FileIndex.txt.`.

```sql  
-- Example is based on AdventureWorks   
CREATE EXTERNAL TABLE hdfsCustomer  
WITH (  
        LOCATION='/pdwdata/customer.tbl',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
) AS SELECT * FROM dimCustomer;  
```

### <a name="b-use-a-query-hint-with-create-external-table-as-select"></a>B. Usare un hint per la query con CREATE EXTERNAL TABLE AS SELECT

 Questa query illustra la sintassi di base per l'uso di un hint di join per la query con l'istruzione CREATE EXTERNAL TABLE AS SELECT. Dopo l'inoltro della query il database usa la strategia hash join per generare il piano di query. Per altre informazioni sugli hint di join e su come usare la clausola OPTION, vedere [Clausola OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).

> [!NOTE]
>  In questo esempio viene specificato 5000. Se la porta non è specificata, il database usa 8020 come porta predefinita.

```sql  
-- Example is based on AdventureWorks  
CREATE EXTERNAL TABLE dbo.FactInternetSalesNew  
WITH   
    (   
        LOCATION = '/files/Customer',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
    )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```

## <a name="see-also"></a>Vedi anche
 - [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)
 - [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)
 - [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)
 - [CREATE TABLE &#40;Azure Synapse Analytics, Parallel Data Warehouse&#41;](~/t-sql/statements/create-table-azure-sql-data-warehouse.md)
 - [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)
 - [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)
 - [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)



