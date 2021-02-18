---
title: Convertire tipi di dati R e SQL
description: Esaminare le conversioni dei tipi di dati implicite ed esplicite tra R e SQL Server nelle soluzioni di data science e Machine Learning.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e024d65ed8a2dd9b55b7940fa19ed400a01af175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348123"
---
# <a name="data-type-mappings-between-r-and-sql-server"></a>Mapping dei tipi di dati tra R e SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo elenca i tipi di dati supportati e le conversioni dei tipi di dati eseguite quando si usa la funzionalità di integrazione di R in SQL Server Machine Learning Services.

## <a name="base-r-version"></a>Versione di R di base

R Services per SQL Server 2016 e Machine Learning Services per SQL Server con R sono allineati a versioni specifiche di Microsoft R Open. La versione più recente, Machine Learning Services per SQL Server 2019, ad esempio, si basa su Microsoft R Open 3.5.2.

Per visualizzare la versione di R associata a una determinata istanza di SQL Server, aprire **RGui** nell'istanza. Il percorso dell'istanza predefinita di SQL Server 2019 sarebbe ad esempio il seguente: `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`.

Lo strumento carica R di base e altre librerie. Le informazioni sulla versione dei pacchetti sono disponibili in una notifica per ogni pacchetto caricato all'avvio della sessione.

## <a name="r-and-sql-data-types"></a>Tipi di dati R e SQL

Mentre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta decine di tipi di dati diversi, R ha un numero limitato di tipi di dati scalari: numerico, intero, complesso, logico, carattere, data/ora e non elaborato. Di conseguenza, ogni volta che si usano dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'interno di script R, i dati possono essere convertiti in modo implicito in un tipo di dati compatibile. Spesso, tuttavia, non è possibile eseguire automaticamente una conversione esatta e viene restituito un errore che può indicare, ad esempio, che il tipo di dati SQL non è gestito.

Questa sezione elenca le conversioni implicite disponibili e i tipi di dati non supportati. È anche disponibile materiale sussidiario per il mapping dei tipi di dati tra R e SQL Server.

## <a name="implicit-data-type-conversions"></a>Conversioni implicite di tipi di dati

La tabella seguente mostra le modifiche del tipo di dati e dei valori quando i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono usati in uno script R e quindi restituiti a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

| Tipo SQL                         | Classe R     | Tipo di set di risultati    | Commenti            |
|----------------------------------|-------------|--------------------|---------------------|
| **bigint**                       | `numeric`   | **float**          | L'esecuzione di uno script R con `sp_execute_external_script` consente di usare il tipo di dati bigint come dati di input. Tuttavia, poiché i dati vengono convertiti nel tipo numerico di R, si verifica una perdita di precisione per i valori molto alti o con posizioni decimali. R supporta solo valori integer fino a 53 bit, quindi inizia a verificarsi una perdita di precisione.                                                                                         |
| **binary(n)**<br /> n <= 8000    | `raw`       | **varbinary(max)** | Consentito solo come parametro di input e di output |
| **bit**                          | `logical`   | **bit**            |                     |
| **char(n)**<br /> n <= 8000      | `character` | **ntext**   | I frame di dati di input (input_data_1) vengono creati senza impostare in modo esplicito il parametro *stringsAsFactors*, quindi il tipo di colonna dipende da *default.stringsAsFactors()* in R                                                                                                                                                                                                                                           |
| **datetime**                     | `POSIXct`   | **datetime**       | Rappresentato come GMT  |
| **date**                         | `POSIXct`   | **datetime**       | Rappresentato come GMT  |
| **decimal(p,s)**                 | `numeric`   | **float**          | L'esecuzione di uno script R con `sp_execute_external_script` consente di usare il tipo di dati decimal come dati di input. Tuttavia, poiché i dati vengono convertiti nel tipo numerico di R, si verifica una perdita di precisione per i valori molto alti o con posizioni decimali. `sp_execute_external_script` con uno script R non supporta l'intera gamma del tipo di dati e modifica le ultime cifre decimali, in particolare in presenza di frazioni. |
| **float**                        | `numeric`   | **float**          |                     |
| **int**                          | `integer`   | **int**            |                     |
| **money**                        | `numeric`   | **float**          | L'esecuzione di uno script R con `sp_execute_external_script` consente di usare il tipo di dati money come dati di input. Tuttavia, poiché i dati vengono convertiti nel tipo numerico di R, si verifica una perdita di precisione per i valori molto alti o con posizioni decimali. In alcuni casi i valori dei centesimi non sono precisi e viene generato un avviso: *Warning: unable to precisely represent cents values* (Avviso: Impossibile rappresentare con precisione i valori dei centesimi).                                               |
| **numeric(p,s)**                 | `numeric`   | **float**          | L'esecuzione di uno script R con `sp_execute_external_script` consente di usare il tipo di dati numeric come dati di input. Tuttavia, poiché i dati vengono convertiti nel tipo numerico di R, si verifica una perdita di precisione per i valori molto alti o con posizioni decimali. `sp_execute_external_script` con uno script R non supporta l'intera gamma del tipo di dati e modifica le ultime cifre decimali, in particolare in presenza di frazioni. |
| **real**                         | `numeric`   | **float**          |                     |
| **smalldatetime**                | `POSIXct`   | **datetime**       | Rappresentato come GMT  |
| **smallint**                     | `integer`   | **int**            |                     |
| **smallmoney**                   | `numeric`   | **float**          |                     |
| **tinyint**                      | `integer`   | **int**            |                     |
| **uniqueidentifier**             | `character` | **ntext**   |                     |
| **varbinary(n)**<br /> n <= 8000 | `raw`       | **varbinary(max)** | Consentito solo come parametro di input e di output |
| **varbinary(max)**               | `raw`       | **varbinary(max)** | Consentito solo come parametro di input e di output |
| **varchar(n)**<br /> n <= 8000   | `character` | **ntext**   | I frame di dati di input (input_data_1) vengono creati senza impostare in modo esplicito il parametro *stringsAsFactors*, quindi il tipo di colonna dipende da *default.stringsAsFactors()* in R                                                                                                                                                                                                                                           |

## <a name="data-types-not-supported-by-r"></a>Tipi di dati non supportati da R

Tra le categorie di tipi di dati supportati dal [sistema di tipi di SQL Server](../../t-sql/data-types/data-types-transact-sql.md), i tipi seguenti possono porre problemi se passati al codice R:

+ Tipi di dati elencati nella sezione **Altri tipi di dati** dell'articolo sul sistema di tipi di SQL Server: **cursor**, **timestamp**, **hierarchyid**, **uniqueidentifier**, **sql_variant**, **xml**, **table**
+ Tutti i tipi spaziali
+ **image**

## <a name="data-types-that-might-convert-poorly"></a>Tipi di dati che potrebbero essere convertiti in modo inadeguato

+ La maggior parte dei tipi datetime dovrebbe funzionare correttamente, ad eccezione di **datetimeoffset**.
+ È supportata la maggior parte dei tipi di dati numerici, ma per **money** e **smallmoney** le conversioni possono non riuscire.
+ Il tipo **varchar** è supportato, ma poiché SQL Server usa di norma Unicode, è consigliato l'uso di **nvarchar** e di altri tipi di dati di testo Unicode, se possibile.
+ Le funzioni della libreria RevoScaleR precedute da rx possono gestire i tipi di dati binari SQL (**binary** e **varbinary**), ma nella maggior parte degli scenari sarà necessaria una gestione speciale per questi tipi. La maggior parte del codice R non funziona con colonne binarie.

  
 Per altre informazioni sui tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)

## <a name="changes-in-data-types-between-sql-server-versions"></a>Differenze relative ai tipi di dati tra le versioni di SQL Server

Microsoft SQL Server 2016 e versioni successive includono alcuni miglioramenti per le conversioni dei tipi di dati e per diverse altre operazioni. La maggior parte di questi miglioramenti offre maggiore precisione quando si gestiscono tipi a virgola mobile, nonché alcune modifiche minori alle operazioni su tipi **datetime** classici.

Questi miglioramenti sono tutti disponibili per impostazione predefinita quando si usa il livello di compatibilità del database 130 o successivo. Tuttavia, se si usa un livello di compatibilità diverso o ci si connette a un database usando una versione meno recente, si potrebbero riscontrare differenze riguardo alla precisione dei numeri o di altri risultati. 

Per altre informazioni, vedere [Miglioramenti di SQL Server 2016 relativi alla gestione di alcuni tipi di dati e operazioni non comuni](https://support.microsoft.com/help/4010261/sql-server-2016-improvements-in-handling-some-data-types-and-uncommon-).
 

## <a name="verify-r-and-sql-data-schemas-in-advance"></a>Verificare in anticipo schemi di dati R e SQL

In genere, ogni volta che si hanno dubbi sull'uso di una particolare struttura di dati o di un tipo di dati specifico in R, usare la funzione  `str()` per ottenere la struttura interna e il tipo dell'oggetto R. Il risultato della funzione viene stampato nella console R ed è disponibile anche nei risultati della query, nella scheda **Messaggi** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. 

Quando si recuperano dati da un database per usarli all'interno di codice R, è sempre consigliabile eliminare le colonne che non possono essere usate in R, nonché quelle che non sono utili per l'analisi, come i GUID (identificatori univoci), i timestamp e le altre colonne usate per il controllo o le informazioni sulla derivazione create dai processi ETL (estrazione, trasformazione e caricamento). 

L'inclusione di colonne non necessarie può ridurre notevolmente le prestazioni del codice R, in particolare se vengono usate colonne con cardinalità elevata come fattori. Di conseguenza, è consigliabile usare le stored procedure e le viste di informazioni di sistema di SQL Server per ottenere in anticipo i tipi di dati per una tabella specifica ed eliminare o convertire le colonne non compatibili. Per altre informazioni, vedere [Viste degli schemi delle informazioni del sistema (Transact-SQL)](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)

Se un determinato tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è supportato da R, ma è necessario usare le colonne di dati nello script R, è consigliabile usare le funzioni [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md) per assicurarsi che le conversioni dei tipi di dati vengano eseguite come previsto prima di usare i dati nello script R.  

> [!WARNING]
> Se si usa **rxDataStep** per eliminare colonne non compatibili durante lo spostamento dei dati, tenere presente che gli argomenti _varsToKeep_ e _varsToDrop_ non sono supportati per il tipo di origine dati **RxSqlServerData**.


## <a name="examples"></a>Esempi

### <a name="example-1-implicit-conversion"></a>Esempio 1: Conversione implicita

L'esempio seguente mostra il modo in cui vengono trasformati i dati quando si esegue il round trip tra SQL Server e R.

La query ottiene una serie di valori da una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e usa la stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) per restituire i valori tramite il runtime R.

```sql
CREATE TABLE MyTable (    
 c1 int,    
 c2 varchar(10),    
 c3 uniqueidentifier    
);    
go    
INSERT MyTable VALUES(1, 'Hello', newid());    
INSERT MyTable VALUES(-11, 'world', newid());    
SELECT * FROM MyTable;    
  
EXECUTE sp_execute_external_script    
 @language = N'R'    
 , @script = N'    
inputDataSet["cR"] <- c(4, 2)    
str(inputDataSet)    
outputDataSet <- inputDataSet'    
 , @input_data_1 = N'SELECT c1, c2, c3 FROM MyTable'    
 , @input_data_1_name = N'inputDataSet'    
 , @output_data_1_name = N'outputDataSet'    
 WITH RESULT SETS((C1 int, C2 varchar(max), C3 varchar(max), C4 float));  
```

**Risultati**

|\# riga|C1|C2|C3|C4|
|-|-|-|-|-|
|1|1|Ciao|6e225611-4b58-4995-a0a5-554d19012ef1|4|
|2|-11|world|6732ea46-2d5d-430b-8ao1-86e7f3351c3e|2|

Si noti l'uso della funzione `str` in R per ottenere lo schema di dati di output. Questa funzione restituisce le informazioni seguenti:

```output
'data.frame':2 obs. of  4 variables:
 $ c1: int  1 -11
 $ c2: Factor w/ 2 levels "Hello","world": 1 2
 $ c3: Factor w/ 2 levels "6732EA46-2D5D-430B-8A01-86E7F3351C3E",..: 2 1
 $ cR: num  4 2
```

È possibile vedere che le conversioni del tipo di dati seguenti sono state eseguite in modo implicito come parte di questa query:

+ **Colonna C1**. La colonna è rappresentata come **int** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come `integer` in R e come **int** nel set di risultati di output.  
  
  Non è stata eseguita alcuna conversione del tipo di dati.  
  
+ **Colonna C2**. La colonna è rappresentata come **varchar(10)** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come `factor` in R e come **varchar(max)** nell'output.  
  
  Si noti il cambiamento dell'output. Qualsiasi stringa da R (un fattore o una stringa normale) sarà rappresentata come **varchar(max)** , indipendentemente dalla lunghezza delle stringhe.  
  
+ **Colonna C3**.  La colonna è rappresentata come **uniqueidentifier** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come `character` in R e come **varchar(max)** nell'output.
  
  Si noti la conversione del tipo di dati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta **uniqueidentifier** ma non R. Pertanto gli identificatori sono rappresentati come stringhe.
  
+ **Colonna C4**. La colonna contiene valori generati dallo script R e non presenti nei dati originali.


## <a name="example-2-dynamic-column-selection-using-r"></a>Esempio 2: Selezione dinamica di colonne tramite R

L'esempio seguente mostra come usare il codice R per verificare la presenza di tipi di colonna non validi. L'esempio ottiene lo schema di una tabella specificata usando le viste di sistema di SQL Server e rimuove qualsiasi colonna che ha un tipo non valido specificato.

```R
connStr <- "Server=.;Database=TestDB;Trusted_Connection=Yes"
data <- RxSqlServerData(connectionString = connStr, sqlQuery = "SELECT COLUMN_NAME FROM TestDB.INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = N'testdata' AND DATA_TYPE <> 'image';")
columns <- rxImport(data)
columnList <- do.call(paste, c(as.list(columns$COLUMN_NAME), sep = ","))
sqlQuery <- paste("SELECT", columnList, "FROM testdata")
```

## <a name="see-also"></a>Vedere anche

+ [Mapping dei tipi di dati tra Python e SQL Server](../python/python-libraries-and-data-types.md)
