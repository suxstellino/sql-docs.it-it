---
description: OPENJSON (Transact-SQL)
title: OPENJSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENJSON
- OPENJSON_TSQL
helpviewer_keywords:
- OPENJSON rowset function
- JSON, importing
- JSON, converting from
ms.assetid: 233d0877-046b-4dcc-b5da-adeb22f78531
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 7cf19093bb9e98dc0806bae16cbf8c9e0428db10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210699"
---
# <a name="openjson-transact-sql"></a>OPENJSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** è una funzione con valori di tabella che analizza il testo JSON e restituisce gli oggetti e le proprietà dell'input JSON come righe e colonne. In altre parole, **OPENJSON** offre una visualizzazione di set di righe in un documento JSON. È possibile specificare in modo esplicito le colonne nel set di righe e i percorsi delle proprietà JSON usate per popolare le colonne. Poiché **OPENJSON** restituisce un set di righe, è possibile usare **OPENJSON** nella clausola `FROM` di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] nello stesso modo in cui è possibile usare qualsiasi altra tabella, visualizzazione o funzione con valori di tabella.  
  
Usare **OPENJSON** per importare i dati JSON in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o convertire dati JSON in formato relazionale per un'app o un servizio che non può usare direttamente JSON.  
  
> [!NOTE]  
>  La funzione **OPENJSON** è disponibile solo nel livello di compatibilità 130 o superiore. Se il livello di compatibilità del database è inferiore a 130, SQL Server non riesce a trovare e a eseguire la funzione **OPENJSON**. Altre funzioni JSON sono disponibili in tutti i livelli di compatibilità.
> 
> È possibile controllare il livello di compatibilità nella vista `sys.databases` o nelle proprietà del database. È possibile modificare il livello di compatibilità di un database con il comando seguente:  
> 
> `ALTER DATABASE DatabaseName SET COMPATIBILITY_LEVEL = 130`
>
> Il livello di compatibilità 120 può essere predefinito anche in un nuovo database SQL di Microsoft Azure.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento")[Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
OPENJSON( jsonExpression [ , path ] )  [ <with_clause> ]

<with_clause> ::= WITH ( { colName type [ column_path ] [ AS JSON ] } [ ,...n ] )
```  

La funzione con valori di tabella **OPENJSON** analizza *jsonExpression* specificato come primo argomento e restituisce una o più righe contenenti i dati degli oggetti JSON nell'espressione. *jsonExpression* può contenere oggetti secondari nidificati. Se si vuole analizzare un oggetto secondario dall'interno di *jsonExpression*, è possibile specificare un parametro **path** per l'oggetto secondario JSON.

### <a name="openjson"></a>openjson

![Sintassi per OPENJSON TVF](../../relational-databases/json/media/openjson-syntax.png "Sintassi di OPENJSON")  

Per impostazione predefinita, la funzione con valori di tabella **OPENJSON** restituisce tre colonne che contengono il nome della chiave, il valore e il tipo di ogni coppia {chiave:valore} trovato in *jsonExpression*. In alternativa, è possibile specificare in modo esplicito lo schema del set di risultati che **OPENJSON** restituisce fornendo *with_clause*.
  
### <a name="with_clause"></a>with_clause
  
![Sintassi per la clausola WITH in OPENJSON TVF](../../relational-databases/json/media/openjson-shema-syntax.png "Sintassi di OPENJSON WITH")

*with_clause* contiene un elenco delle colonne con i relativi tipi che devono essere restituite da **OPENJSON**. Per impostazione predefinita, **OPENJSON** ricerca la corrispondenza delle chiavi in *jsonExpression* con i nomi di colonna in *with_clause* (in questo caso, la ricerca della corrispondenza delle chiavi implica che viene fatta distinzione tra maiuscole e minuscole). Se un nome di colonna non corrisponde al nome di una chiave, è possibile specificare un valore *column_path* facoltativo che è costituito da un'[espressione di percorso JSON](../../relational-databases/json/json-path-expressions-sql-server.md) che fa riferimento a una chiave all'interno di *jsonExpression*. 

## <a name="arguments"></a>Argomenti

### <a name="jsonexpression"></a>*jsonExpression*

Espressione di caratteri Unicode contenente testo JSON.  
  
OPENJSON esegue l'iterazione sugli elementi della matrice o sulle proprietà dell'oggetto nell'espressione JSON e restituisce una riga per ogni elemento o proprietà. L'esempio seguente restituisce ogni proprietà dell'oggetto specificato come *jsonExpression*:  

```sql
DECLARE @json NVARCHAR(2048) = N'{
   "String_value": "John",
   "DoublePrecisionFloatingPoint_value": 45,
   "DoublePrecisionFloatingPoint_value": 2.3456,
   "BooleanTrue_value": true,
   "BooleanFalse_value": false,
   "Null_value": null,
   "Array_value": ["a","r","r","a","y"],
   "Object_value": {"obj":"ect"}
}';

SELECT * FROM OpenJson(@json);
```

**Risultati:**

| Key                                | Valore                 | type |
| :--                                | :----                 | :--- |
| String_value                       | John                  | 1 |
| DoublePrecisionFloatingPoint_value | 45                    | 2 |
| DoublePrecisionFloatingPoint_value | 2,3456                | 2 |
| BooleanTrue_value                  | true                  | 3 |
| BooleanFalse_value                 | false                 | 3 |
| Null_value                         | NULL                  | 0 |
| Array_value                        | ["a","r","r","a","y"] | 4 |
| Object_value                       | {"obj":"ect"}         | 5 |
| &nbsp; | &nbsp; | &nbsp; |

- DoublePrecisionFloatingPoint_value è conforme a IEEE-754.

### <a name="path"></a>*path*

Espressione di percorso JSON facoltativa che fa riferimento a un oggetto o a una matrice all'interno di *jsonExpression*. **OPENJSON** esegue la ricerca nel testo JSON nella posizione specificata e analizza solo il frammento cui viene fatto riferimento. Per altre informazioni, vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).

In [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] e nel [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] è possibile specificare una variabile come valore di *path*.
  
L'esempio seguente restituisce un oggetto nidificato tramite la specifica di *path*:  

```sql  
DECLARE @json NVARCHAR(4000) = N'{  
      "path": {  
            "to":{  
                 "sub-object":["en-GB", "en-UK","de-AT","es-AR","sr-Cyrl"]  
                 }  
              }  
 }';

SELECT [key], value
FROM OPENJSON(@json,'$.path.to."sub-object"')
```  
  
 **Risultati**  
  
|Chiave|valore|  
|---------|-----------|  
|0|en-GB|  
|1|en-UK|  
|2|de-AT|  
|3|es-AR|  
|4|sr-Cyrl|  

Quando **OPENJSON** analizza una matrice JSON, la funzione restituisce gli indici degli elementi nel testo JSON come chiavi.

Il confronto usato per la ricerca delle corrispondenze dei passaggi di percorso con le proprietà dell'espressione JSON fa distinzione tra maiuscole e minuscole e non considera le regole di confronto (ovvero è un confronto BIN2). 

### <a name="with_clause"></a>*with_clause*

Definisce in modo esplicito lo schema di output che deve essere restituito dalla funzione **OPENJSON**. *with_clause* può contenere gli elementi seguenti:

*colName* Nome della colonna di output.  
  
Per impostazione predefinita, **OPENJSON** usa il nome della colonna per cercare una corrispondenza con una proprietà nel testo JSON. Ad esempio, se si specifica la colonna *name* nello schema, OPENJSON tenta di popolare la colonna con la proprietà "name" nel testo JSON. È possibile eseguire l'override di questo mapping predefinito usando l'argomento *column_path*.  
  
*type*  
Tipo di dati della colonna di output.  

> [!NOTE]
> Se viene usata anche l'opzione **AS JSON**, il valore *type* della colonna deve essere `NVARCHAR(MAX)`.
  
*column_path*  
Percorso JSON che specifica la proprietà da restituire nella colonna specificata. Per altre informazioni, vedere la descrizione del parametro *path* in precedenza in questo argomento.  
  
Usare *column_path* per eseguire l'override delle regole di mapping predefinite quando il nome di una colonna di output non corrisponde al nome della proprietà.  
  
Il confronto usato per la ricerca delle corrispondenze dei passaggi di percorso con le proprietà dell'espressione JSON fa distinzione tra maiuscole e minuscole e non considera le regole di confronto (ovvero è un confronto BIN2).  
  
Per altre informazioni sui percorsi, vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
*AS JSON*  
Usare l'opzione **AS JSON** nella definizione di una colonna per specificare che la proprietà cui viene fatto riferimento contiene un oggetto JSON interno o una matrice. Se si specifica l'opzione **AS JSON**, il tipo della colonna deve essere NVARCHAR(MAX).

- Se non si specifica **AS JSON** per una colonna, la funzione restituisce un valore scalare (ad esempio, int, string, true, false) della proprietà JSON specificata nel percorso specificato. Se il percorso rappresenta un oggetto o una matrice e la proprietà non viene trovata nel percorso specificato, la funzione restituisce Null in modalità lax oppure restituisce un errore in modalità strict. Questo comportamento è simile al comportamento della funzione **JSON_VALUE**.  
  
- Se si specifica **AS JSON** per una colonna, la funzione restituisce un frammento JSON della proprietà JSON specificata nel percorso specificato. Se il percorso rappresenta un valore scalare e la proprietà non viene trovata nel percorso specificato, la funzione restituisce Null in modalità lax oppure restituisce un errore in modalità strict. Questo comportamento è simile al comportamento della funzione **JSON_QUERY**.  
  
> [!NOTE]  
> Se si vuole restituire un frammento JSON nidificato di una proprietà JSON, è necessario specificare il flag **AS JSON**. Senza questa opzione, se la proprietà non viene trovata, OPENJSON restituisce un valore NULL anziché l'oggetto JSON o una matrice cui viene fatto riferimento oppure restituisce un errore di run-time in modalità strict.  
  
Ad esempio, la query seguente restituisce e formatta gli elementi di una matrice:  
  
```sql  
DECLARE @json NVARCHAR(MAX) = N'[  
  {  
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]'  
   
SELECT *
FROM OPENJSON ( @json )  
WITH (   
              Number   VARCHAR(200)   '$.Order.Number',  
              Date     DATETIME       '$.Order.Date',  
              Customer VARCHAR(200)   '$.AccountNumber',  
              Quantity INT            '$.Item.Quantity',  
              [Order]  NVARCHAR(MAX)  AS JSON  
 )
```  
  
**Risultati**
  
|Number|Data|Customer|Quantità|JSON|  
|------------|----------|--------------|--------------|-----------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|{"Number":"SO43659","Date":"2011-05-31T00:00:00"}|  
|SO43661|2011-06-01T00:00:00|AW73565|3|{"Number":"SO43661","Date":"2011-06-01T00:00:00"}|  
  
## <a name="return-value"></a>Valore restituito
Le colonne restituite dalla funzione OPENJSON dipendono dall'opzione WITH.  
  
1. Quando si chiama OPENJSON con lo schema predefinito, ovvero non si specifica uno schema esplicito nella clausola WITH, la funzione restituisce una tabella con le colonne seguenti:  
    1.  **Key**. Valore nvarchar(4000) che contiene il nome della proprietà specificata o l'indice dell'elemento nella matrice specificata. La colonna Key ha regole di confronto BIN2.  
    2.  **Valore**. Valore nvarchar(max) che contiene il valore della proprietà. La colonna Value eredita le regole di confronto da *jsonExpression*.
    3.  **Type**. Valore int che contiene il tipo del valore. La colonna **Type** viene restituita solo quando OPENJSON viene usata con lo schema predefinito. La colonna Type include uno dei valori seguenti:  
  
        |Valore della colonna Type|Tipo di dati JSON|  
        |------------------------------|--------------------|  
        |0|Null|  
        |1|string|  
        |2|d'acquisto|  
        |3|true/false|  
        |4|array|  
        |5|object|  
  
     Vengono restituite solo le proprietà di primo livello. L'istruzione ha esito negativo se il testo JSON non è formattato correttamente.  

2. Quando si chiama OPENJSON e si specifica uno schema esplicito nella clausola WITH, la funzione restituisce una tabella con lo schema definito nella clausola WITH.

> [!NOTE]  
> Le colonne **Key**, **Value** e **Type** vengono restituite solo quando si usa OPENJSON con lo schema predefinito e non sono disponibili con uno schema esplicito.

## <a name="remarks"></a>Osservazioni  

*json_path* usato nel secondo argomento di **OPENJSON** o in *with_clause* può iniziare con la parola chiave **lax** o **strict**.

- In modalità **lax**, **OPENJSON** non genera un errore se non viene trovato l'oggetto o il valore nel percorso specificato. Se il percorso non viene trovato, **OPENJSON** restituisce un set di risultati vuoto o un valore NULL.
- In modalità **strict**, **OPENJSON** restituisce un errore se il percorso non viene trovato.

Alcuni esempi in questa pagina specificano in modo esplicito la modalità di percorso, ovvero lax o strict. La modalità di percorso è facoltativa. Se non si specifica in modo esplicito una modalità di percorso, la modalità lax è la modalità predefinita. Per altre informazioni sulla modalità di percorso e le espressioni di percorso, vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).    

Viene cercata la corrispondenza dei nomi di colonna in *with_clause* con le chiavi nel testo JSON. Se si specifica il nome di colonna `[Address.Country]`, viene cercata la corrispondenza con la chiave `Address.Country`. Se si vuole fare riferimento a una chiave nidificata `Country` all'interno dell'oggetto `Address`, è necessario specificare il percorso `$.Address.Country` nel percorso di colonna.

*json_path* può contenere chiavi con caratteri alfanumerici. Se sono presenti caratteri speciali nelle chiavi, specificare il nome della chiave in *json_path* con virgolette doppie. Ad esempio, `$."my key $1".regularKey."key with . dot"` corrisponde al valore 1 nel testo JSON seguente:

```json
{
  "my key $1": {
    "regularKey":{
      "key with . dot": 1
    }
  }
}
```  

## <a name="examples"></a>Esempi  
  
### <a name="example-1---convert-a-json-array-to-a-temporary-table"></a>Esempio 1: convertire una matrice JSON in una tabella temporanea

L'esempio seguente visualizza un elenco di identificatori come matrice JSON di numeri. La query converte la matrice JSON in una tabella di identificatori e filtra tutti i prodotti con gli ID specificati.  
  
```sql  
DECLARE @pSearchOptions NVARCHAR(4000) = N'[1,2,3,4]'

SELECT *
FROM products
INNER JOIN OPENJSON(@pSearchOptions) AS productTypes
 ON product.productTypeID = productTypes.value
```  
  
La query corrisponde all'esempio seguente. Tuttavia, nell'esempio seguente, è necessario incorporare i numeri nella query anziché passarli come parametri.  
  
```sql  
SELECT *
FROM products
WHERE product.productTypeID IN (1,2,3,4)
```  
  
### <a name="example-2---merge-properties-from-two-json-objects"></a>Esempio 2: unire le proprietà di due oggetti JSON

L'esempio seguente seleziona un'unione di tutte le proprietà di due oggetti JSON. I due oggetti hanno una proprietà *name* duplicata. L'esempio usa il valore di chiave per escludere la riga duplicata dai risultati.  
  
```sql  
DECLARE @json1 NVARCHAR(MAX),@json2 NVARCHAR(MAX)

SET @json1=N'{"name": "John", "surname":"Doe"}'

SET @json2=N'{"name": "John", "age":45}'

SELECT *
FROM OPENJSON(@json1)
UNION ALL
SELECT *
FROM OPENJSON(@json2)
WHERE [key] NOT IN (SELECT [key] FROM OPENJSON(@json1))
```  
  
### <a name="example-3---join-rows-with-json-data-stored-in-table-cells-using-cross-apply"></a>Esempio 3: unire in join le righe con i dati JSON memorizzati nelle celle di tabella usando CROSS APPLY

Nell'esempio seguente la tabella `SalesOrderHeader` ha una colonna di testo `SalesReason` che contiene una matrice di `SalesOrderReasons` in formato JSON. Gli oggetti `SalesOrderReasons` contengono proprietà come *Quality* e *Manufacturer*. L'esempio crea un report che unisce ogni riga dell'ordine di vendita ai motivi di vendita correlati. L'operatore OPENJSON espande la matrice JSON dei motivi di vendita come se fossero archiviati in una tabella figlio separata. L'operatore CROSS APPLY unisce ogni riga dell'ordine di vendita alle righe restituite dalla funzione con valori di tabella OPENJSON.  
  
```sql  
SELECT SalesOrderID,OrderDate,value AS Reason
FROM Sales.SalesOrderHeader
CROSS APPLY OPENJSON(SalesReasons)
```  
  
> [!TIP] 
> Quando è necessario espandere le matrici JSON memorizzate in singoli campi e unirle alle relative righe padre, viene in genere usato l'operatore CROSS APPLY [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per altre informazioni su CROSS APPLY, vedere [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
La stessa query può essere riscritta usando `OPENJSON` con uno schema definito in modo esplicito delle righe da restituire:  
  
```sql  
SELECT SalesOrderID, OrderDate, value AS Reason  
FROM Sales.SalesOrderHeader  
     CROSS APPLY OPENJSON (SalesReasons) WITH (value NVARCHAR(100) '$')
```  
  
In questo esempio il percorso `$` fa riferimento a ogni elemento nella matrice. Se si vuole eseguire in modo esplicito il cast del valore restituito, è possibile usare questo tipo di query.  
  
### <a name="example-4---combine-relational-rows-and-json-elements-with-cross-apply"></a>Esempio 4: combinare le righe relazionali e gli elementi JSON con CROSS APPLY

La query seguente combina le righe relazionali e gli elementi JSON nei risultati riportati nella tabella seguente.  
  
```sql  
SELECT store.title, location.street, location.lat, location.long  
FROM store  
CROSS APPLY OPENJSON(store.jsonCol, 'lax $.location')   
     WITH (street VARCHAR(500) ,  postcode VARCHAR(500) '$.postcode' ,  
     lon int '$.geo.longitude', lat int '$.geo.latitude')  
     AS location
```  
  
**Risultati**
  
|title|street|postcode|lon|lat|  
|-----------|------------|--------------|---------|---------|  
|Whole Food Markets|17991 Redmond Way|WA  98052|47.666124|-122.10155|  
|Sears|148th Ave NE|WA  98052|47.63024|-122.141246,17|  
  
### <a name="example-5---import-json-data-into-sql-server"></a>Esempio 5: importare dati JSON in SQL Server

Nell'esempio seguente viene caricato un intero oggetto JSON in una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```sql  
DECLARE @json NVARCHAR(max)  = N'{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id INT,  
        firstName NVARCHAR(50), lastName NVARCHAR(50),   
        isAlive BIT, age INT,  
        dateOfBirth DATETIME, spouse NVARCHAR(50))
```  

### <a name="example-6---simple-example-with-json-content"></a>Esempio 6: esempio semplice con contenuto JSON

```sql
--simple cross apply example
DECLARE @JSON NVARCHAR(MAX) = N'[
{
"OrderNumber":"SO43659",
"OrderDate":"2011-05-31T00:00:00",
"AccountNumber":"AW29825",
"ItemPrice":2024.9940,
"ItemQuantity":1
},
{
"OrderNumber":"SO43661",
"OrderDate":"2011-06-01T00:00:00",
"AccountNumber":"AW73565",
"ItemPrice":2024.9940,
"ItemQuantity":3
}
]'

SELECT root.[key] AS [Order],TheValues.[key], TheValues.[value]
FROM OPENJSON ( @JSON ) AS root
CROSS APPLY OPENJSON ( root.value) AS TheValues
```

## <a name="see-also"></a>Vedere anche

- [JSON Path Expressions (Espressioni di percorso JSON) &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Convertire dati JSON in righe e colonne con la funzione OPENJSON &#40;SQL Server&#41;](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)   
- [Usare OPENJSON con lo schema predefinito &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)   
- [Usare OPENJSON con uno schema esplicito &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)  
  
