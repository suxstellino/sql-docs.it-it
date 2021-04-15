---
description: Linee guida per l'utilizzo dei metodi con tipo di dati xml
title: Linee guida per l'utilizzo dei metodi con tipo di dati xml
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: 1a483aa1-42de-4c88-a4b8-c518def3d496
author: rothja
ms.author: jroth
ms.openlocfilehash: 5873546a4c0212cc97a0365d0ba1e3706b1835da
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491311"
---
# <a name="guidelines-for-using-xml-data-type-methods"></a>Linee guida per l'utilizzo dei metodi con tipo di dati xml

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento vengono descritte le linee guida per l'uso dei metodi con tipo di dati **xml**.

## <a name="the-print-statement"></a>Istruzione PRINT

Non è possibile usare i metodi con tipo di dati **xml** nell'istruzione PRINT, come illustrato nell'esempio seguente. I metodi **xml** sono considerati come sottoquery, che non sono consentite nell'istruzione PRINT. L'esempio seguente restituisce pertanto un errore:

```sql
DECLARE @x XML
SET @x = '<root>Hello</root>'
PRINT @x.value('/root[1]', 'varchar(20)') -- will not work because this is treated as a subquery (select top 1 col from table)
```

Una soluzione consiste nell'assegnare il risultato del metodo **value()** a una variabile di tipo **xml** e quindi specificare la variabile nella query.

```sql
DECLARE @x XML
DECLARE @c VARCHAR(max)
SET @x = '<root>Hello</root>'
SET @c = @x.value('/root[1]', 'VARCHAR(11)')
PRINT @c
```

## <a name="the-group-by-clause"></a>Clausola GROUP BY

I metodi con tipo di dati **xml** vengono considerati internamente come sottoquery. La clausola GROUP BY richiede una funzione scalare e non consente aggregazioni e sottoquery, pertanto non è possibile specificare i metodi con tipo di dati **xml** in questa clausola. Una soluzione è rappresentata da una chiamata a una funzione definita dall'utente all'interno della quale vengono utilizzati metodi XML.

## <a name="reporting-errors"></a>Segnalazione degli errori

Per la segnalazione degli errori, i metodi con tipo di dati **xml** generano un singolo errore nel formato seguente:

```
Msg errorNumber, Level levelNumber, State stateNumber:
XQuery [database.table.method]: description_of_error
```

Ad esempio:

```
Msg 2396, Level 16, State 1:
XQuery [xmldb_test.xmlcol.query()]: Attribute may not appear outside of an element
```

## <a name="singleton-checks"></a>Verifiche dei singleton

Se il compilatore non è in grado di determinare se un determinato valore singleton è garantito in fase di esecuzione, i passi, i parametri delle funzioni e gli operatori che richiedono un singleton restituiranno un errore. Questo problema si verifica di frequente con i dati non tipizzati. Per la ricerca di un attributo, ad esempio, è necessario un elemento padre singleton. È sufficiente un numero ordinale che seleziona un nodo padre singolo. Per valutare una combinazione **node()** -**value()** per l'estrazione dei valori degli attributi potrebbe non essere necessario specificare il numero ordinale, come illustrato nell'esempio seguente.

### <a name="example-known-singleton"></a>Esempio: singleton noto

In questo esempio il metodo **nodes()** genera una riga distinta per ogni elemento `<book>`. Il metodo **value()** , valutato su un nodo `<book>`, estrae il valore di `@genre` e, essendo un attributo, è un singleton.

```sql
SELECT nref.value('@genre', 'VARCHAR(max)') LastName
FROM T CROSS APPLY xCol.nodes('//book') AS R(nref)
```

Per la verifica dei dati XML tipizzati viene utilizzato un XML Schema. Se un determinato nodo è specificato come singleton nell'XML Schema, il compilatore utilizza tale informazione e non viene generato alcun errore. In caso contrario, è necessario specificare un numero ordinale che seleziona un nodo singolo. In particolare, l'uso dell'asse descendant-or-self (//), come ad esempio in `/book//title`, impedisce l'inferenza della cardinalità singleton per l'elemento `<title>`, anche se nell'XML Schema è specificato come tale, ed è pertanto necessario riscriverlo come `(/book//title)[1]`.

La differenza tra `//first-name[1]` e `(//first-name)[1]` è molto importante per la verifica dei tipi. Nel primo caso viene restituita una sequenza di nodi `<first-name>`, in cui ogni nodo è il nodo `<first-name>` più a sinistra tra gli elementi di pari livello. Nel secondo caso viene restituito il primo nodo `<first-name>` singleton nell'ordine del documento nell'istanza XML.

### <a name="example-using-value"></a>Esempio: uso di value()

Per la query seguente su una colonna XML non tipizzata viene restituito un errore statico di compilazione, perché il primo argomento del metodo **value()** deve essere un nodo singleton e il compilatore non è in grado di determinare se in fase di esecuzione verrà rilevato un solo nodo `<last-name>`:

```sql
SELECT xCol.value('//author/last-name', 'NVARCHAR(50)') LastName
FROM T
```

Una possibile soluzione è la seguente:

```sql
SELECT xCol.value('//author/last-name[1]', 'NVARCHAR(50)') LastName
FROM T
```

Tale soluzione non risolve tuttavia il problema, perché in ogni istanza XML possono essere presenti più nodi `<author>`. Per risolvere il problema è necessario riscrivere il codice come segue:

```sql
SELECT xCol.value('(//author/last-name/text())[1]', 'NVARCHAR(50)') LastName
FROM T
```

Questa query restituisce il valore del primo elemento `<last-name>` in ogni istanza XML.

## <a name="see-also"></a>Vedere anche

- [metodi con tipo di dati xml](../../t-sql/xml/xml-data-type-methods.md)
