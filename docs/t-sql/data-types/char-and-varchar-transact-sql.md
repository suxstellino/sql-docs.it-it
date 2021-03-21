---
description: char and varchar (Transact-SQL)
title: char and varchar (Transact-SQL)
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- varchar
- varchar_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fixed-length data types [SQL Server]
- character data type
- char data type
- varchar(max) data type
- variable-length data types [SQL Server]
- varchar data type
- utf8
ms.assetid: 282cd982-f4fb-4b22-b2df-9e8478f13f6a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 20b23401d6f5d708dae34e20e3bfe1e202f7a7d9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735741"
---
# <a name="char-and-varchar-transact-sql"></a>char and varchar (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipi di dati character a dimensione fissa **char** o a dimensione variabile **varchar**. A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], quando si usano regole di confronto che supportano UTF-8, questi tipi di dati archiviano l'intera gamma dei dati di tipo carattere [Unicode](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) e usano la codifica dei caratteri [UTF-8 ](https://www.wikipedia.org/wiki/UTF-8). Se si specificano regole di confronto non UTF-8, questi tipi di dati archiviano solo un subset dei caratteri supportati dalla tabella codici corrispondente di tali regole di confronto.

## <a name="arguments"></a>Argomenti

**char** [ ( *n* ) ] Dati stringa a dimensione fissa. *n* definisce le dimensioni della stringa in byte e deve essere un valore compreso tra 1 e 8.000. Per i set di caratteri con codifica a byte singolo, ad esempio *Latin*, le dimensioni di archiviazione sono pari a *n* byte e anche il numero di caratteri che possono essere archiviati è *n*. Per i set di caratteri con codifica multibyte, le dimensioni di archiviazione sono di nuovo *n* byte, ma il numero di caratteri che possono essere archiviati può essere inferiore a *n*. Il sinonimo ISO per **char** è **character**. Per altre informazioni sui set di caratteri, vedere [Set di caratteri a byte singolo e multibyte](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets).

**varchar** [ ( *n* | **max** ) ] Dati stringa a dimensione variabile. Usare *n* per definire la dimensione della stringa in byte, che può essere un valore compreso tra 1 e 8000, oppure usare **max** per indicare la dimensione massima di archiviazione di un vincolo di colonna pari a 2^31-1 byte (2 GB). Per i set di caratteri con codifica a byte singolo, ad esempio *Latin*, le dimensioni di archiviazione sono pari a *n* byte + 2 byte e anche il numero di caratteri che possono essere archiviati è *n*. Per i set di caratteri con codifica multibyte, le dimensioni di archiviazione sono di nuovo *n* byte + 2 byte, ma il numero di caratteri che possono essere archiviati può essere inferiore a *n*. I sinonimi ISO per **varchar** sono **charvarying** o **charactervarying**. Per altre informazioni sui set di caratteri, vedere [Set di caratteri a byte singolo e multibyte](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets).

## <a name="remarks"></a>Osservazioni

Si pensa comunemente che in [CHAR(*n*) e VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), *n* definisca il numero di caratteri. Invece in [CHAR(*n*) e VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md)*n* definisce la lunghezza della stringa in **byte** (da 0 a 8.000). *n* non definisce mai il numero di caratteri che è possibile archiviare, analogamente alla definizione di [NCHAR(*n*) e NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md).
Si tende a pensare così perché quando si usa la codifica a byte singolo, le dimensioni di archiviazione di CHAR e VARCHAR sono espresse in *n* byte e anche il numero di caratteri è *n*. Tuttavia, per la codifica multibyte, ad esempio [UTF-8](https://www.wikipedia.org/wiki/UTF-8), gli intervalli Unicode più elevati (128-1.114.111) generano un carattere che usa due o più byte. Ad esempio in una colonna definita come CHAR(10) [!INCLUDE[ssde_md](../../includes/ssde_md.md)] può archiviare 10 caratteri che usano la codifica a byte singolo (intervallo Unicode 0-127), ma meno di 10 caratteri quando usano la codifica multibyte (intervallo Unicode 128-1.114.111). Per altre informazioni sull'archiviazione Unicode e sugli intervalli di caratteri, vedere [Differenze nell'archiviazione tra UTF-8 e UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).

Se non si specifica *n* in un'istruzione di definizione dei dati o di dichiarazione di variabili, la lunghezza predefinita è 1. Se non si specifica *n* quando si usano le funzioni CAST e CONVERT, la lunghezza predefinita è 30.

Agli oggetti che usano **char** o **varchar** vengono assegnate le regole di confronto predefinite del database, a meno che non vengano assegnate regole di confronto specifiche tramite la clausola COLLATE. Le regole di confronto controllano la tabella codici utilizzata per l'archiviazione dei dati di tipo carattere.

Le codifiche multibyte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] includono:

- Set di caratteri DBCS (Double Byte Character Set) per alcune lingue asiatiche orientali che usano le tabelle codici 936 e 950 (cinese), 932 (giapponese) o 949 (coreano).
- UTF-8 con tabella codici 65001. **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

In presenza di siti che supportano più lingue:

- A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], è consigliabile usare le regole di confronto abilitate per UTF-8 per il supporto di Unicode e per ridurre al minimo i problemi di conversione dei caratteri.
- Se si usa una versione precedente di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] valutare la possibilità di usare i tipi di dati Unicode **nchar** oppure **nvarchar** per ridurre al minimo i problemi di conversione dei caratteri.

Se si usano i tipi di dati **char** o **varchar**, è consigliabile:

- Usare **char** quando le dimensioni delle voci di dati delle colonne sono coerenti.
- Usare **varchar** quando le dimensioni delle voci di dati delle colonne presentano notevoli differenze.
- Usare **varchar(max)** quando le dimensioni delle voci di dati delle colonne variano in modo significativo e la lunghezza delle stringhe potrebbe essere superiore a 8.000 byte.

Se l'opzione SET ANSI_PADDING è impostata su OFF quando si esegue l'istruzione CREATE TABLE o ALTER TABLE, le colonne di tipo **char** definite come NULL vengono gestite come colonne di tipo **varchar**.

> [!WARNING]
> Ogni colonna non Null varchar(max) o nvarchar(max) richiede 24 byte di allocazione fissa aggiuntiva che concorre al raggiungimento del limite delle righe di 8.060 byte durante un'operazione di ordinamento. Ciò può creare un limite implicito per il numero di colonne non Null varchar(max) o nvarchar(max) che è possibile creare in una tabella.
Non vengono segnalati errori particolari (oltre il normale avviso che indica che le dimensioni massime per le righe superano il valore massimo consentito di 8.060 byte) durante la creazione della tabella o l'inserimento dei dati. Queste dimensioni elevate delle righe possono causare errori (ad esempio, l'errore 512) durante alcune operazioni normali, ad esempio un aggiornamento della chiave dell'indice cluster, o operazioni di ordinamento dell'intero set di colonne e si verificano solo nel corso dell'esecuzione di un'operazione.

## <a name="converting-character-data"></a><a name="_character"></a> Conversione dei dati di tipo carattere

Se un'espressione di caratteri viene convertita in un tipo di dati carattere di dimensioni diverse, i valori troppo lunghi per il nuovo tipo di dati vengono troncati. Ai fini della conversione da un'espressione di caratteri, il tipo **uniqueidentifier** viene considerato un tipo carattere ed è quindi soggetto alle regole di troncamento per la conversione in tipo carattere. Vedere la sezione Esempi riportata di seguito.

Se un'espressione di caratteri viene convertita in un'espressione di caratteri con tipo di dati o dimensioni diverse, ad esempio da **char(5)** a **varchar(5)** , o da **char(20)** a **char(15)** , al valore convertito vengono assegnate le regole di confronto del valore di input. Se un'espressione non di caratteri viene convertita in dati di tipo carattere, al valore convertito vengono assegnate le regole di confronto predefinite del database corrente. In entrambi i casi è possibile assegnare regole di confronto specifiche mediante la clausola [COLLATE](../../t-sql/statements/collations.md).

> [!NOTE]
> Le conversioni tra tabelle codici sono supportate per i tipi di dati **char** e **varchar**, ma non per il tipo di dati **text**. Come nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la perdita di dati durante le conversioni tra tabelle codici non viene segnalata.

Le espressioni di caratteri convertite in un tipo di dati **numeric** approssimato possono includere una notazione esponenziale facoltativa. La notazione è costituita da una e minuscola o una E maiuscola seguita da un segno più (+) o meno (-) facoltativo e quindi un numero.

Le espressioni di caratteri che vengono convertite in un tipo di dati **numeric** esatto devono includere cifre, il separatore decimale e un segno più (+) o meno (-) facoltativo. Gli spazi vuoti iniziali vengono ignorati. Nella stringa non è consentito l'uso della virgola come separatore, ad esempio come separatore decimale nel numero 123.456,00.

Le espressioni di caratteri che vengono convertite nel tipo di dati **money** o **smallmoney** possono includere anche un separatore decimale e il simbolo di dollaro ($) facoltativi. L'utilizzo della virgola come separatore decimale, ad esempio $ 123.456,00, è consentito.

Quando una stringa vuota viene convertita in un **int**, il relativo valore diventa ```0```. Quando una stringa vuota viene convertita in una data, il valore diventa il [valore predefinito per la data](date-transact-sql.md), ovvero ```1900-01-01```.

## <a name="examples"></a>Esempi

### <a name="a-showing-the-default-value-of-n-when-used-in-variable-declaration"></a>R. Visualizzazione del valore predefinito di n quando viene usato per la dichiarazione di variabili

L'esempio seguente indica che il valore predefinito di *n* è 1 per i tipi di dati `char` e `varchar` quando vengono usati per la dichiarazione di variabili.

```sql
DECLARE @myVariable AS VARCHAR = 'abc';
DECLARE @myNextVariable AS CHAR = 'abc';
--The following returns 1
SELECT DATALENGTH(@myVariable), DATALENGTH(@myNextVariable);
GO
```

### <a name="b-showing-the-default-value-of-n-when-varchar-is-used-with-cast-and-convert"></a>B. Visualizzazione del valore predefinito di n quando varchar viene usato con CAST e CONVERT

L'esempio seguente indica che il valore predefinito di *n* è 30 quando i tipi di dati `char` e `varchar` vengono usati con le funzioni `CAST` e `CONVERT`.

```sql
DECLARE @myVariable AS VARCHAR(40);
SET @myVariable = 'This string is longer than thirty characters';
SELECT CAST(@myVariable AS VARCHAR);
SELECT DATALENGTH(CAST(@myVariable AS VARCHAR)) AS 'VarcharDefaultLength';
SELECT CONVERT(CHAR, @myVariable);
SELECT DATALENGTH(CONVERT(CHAR, @myVariable)) AS 'VarcharDefaultLength';
```

### <a name="c-converting-data-for-display-purposes"></a>C. Conversione di dati per la visualizzazione

Nell'esempio seguente vengono convertite due colonne in dati di tipo carattere e viene utilizzato uno stile che applica un formato specifico ai dati visualizzati. Un tipo di dati **money** viene convertito in dati di tipo carattere e viene applicato lo stile 1. Tale stile consente di visualizzare i valori con le virgole ogni tre cifre a sinistra del separatore decimale e due cifre a destra del separatore decimale. Un tipo **datetime** viene convertito in dati di tipo carattere e viene applicato lo stile 3, che visualizza i dati nel formato gg/mm/aa. Nella clausola WHERE il cast del tipo **money** viene eseguito a un tipo carattere per eseguire un'operazione di confronto tra stringhe.

```sql
USE AdventureWorks2012;
GO
SELECT BusinessEntityID,
   SalesYTD,
   CONVERT (VARCHAR(12),SalesYTD,1) AS MoneyDisplayStyle1,
   GETDATE() AS CurrentDate,
   CONVERT(VARCHAR(12), GETDATE(), 3) AS DateDisplayStyle3
FROM Sales.SalesPerson
WHERE CAST(SalesYTD AS VARCHAR(20) ) LIKE '1%';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
BusinessEntityID SalesYTD              DisplayFormat CurrentDate             DisplayDateFormat
---------------- --------------------- ------------- ----------------------- -----------------
278              1453719.4653          1,453,719.47  2011-05-07 14:29:01.193 07/05/11
280              1352577.1325          1,352,577.13  2011-05-07 14:29:01.193 07/05/11
283              1573012.9383          1,573,012.94  2011-05-07 14:29:01.193 07/05/11
284              1576562.1966          1,576,562.20  2011-05-07 14:29:01.193 07/05/11
285              172524.4512           172,524.45    2011-05-07 14:29:01.193 07/05/11
286              1421810.9242          1,421,810.92  2011-05-07 14:29:01.193 07/05/11
288              1827066.7118          1,827,066.71  2011-05-07 14:29:01.193 07/05/11
```

### <a name="d-converting-uniqueidentifer-data"></a>D. Conversione del tipo di dati uniqueidentifier

Nell'esempio seguente un valore `uniqueidentifier` viene convertito in un tipo di dati `char`.

```sql
DECLARE @myid uniqueidentifier = NEWID();
SELECT CONVERT(CHAR(255), @myid) AS 'char';
```

Nell'esempio seguente viene illustrato il troncamento dei dati quando il valore è troppo lungo per il tipo di dati in cui avviene la conversione. Poiché la lunghezza del tipo **uniqueidentifier** è limitata a 36 caratteri, i caratteri eccedenti vengono troncati.

```sql
DECLARE @ID NVARCHAR(max) = N'0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong';
SELECT @ID, CONVERT(uniqueidentifier, @ID) AS TruncatedValue;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
String                                       TruncatedValue
-------------------------------------------- ------------------------------------
0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong    0E984725-C51C-4BF4-9960-E1C80E27ABA0

(1 row(s) affected)
```

## <a name="see-also"></a>Vedere anche

[nchar e nvarchar &#40;Transact-SQL&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)

[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)

[COLLATE &#40;Transact-SQL&#41;](../../t-sql/statements/collations.md)

[Conversione del tipo di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)

[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)

[Stima delle dimensioni di un database](../../relational-databases/databases/estimate-the-size-of-a-database.md)

[Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)

[Set di caratteri a byte singolo e multibyte](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)
