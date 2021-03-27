---
description: Informazioni sul tipo di dati sql_variant nel driver JDBC e su come può essere utilizzato per supportare i parametri con valori di tabella (TVP) e la copia bulk.
title: Uso del tipo di dati Sql_variant
ms.custom: ''
ms.date: 03/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dff69a149a8acf50aafa02653a6a083354dfa993
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633294"
---
# <a name="using-sql_variant-data-type"></a>Uso del tipo di dati Sql_variant

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

A partire dalla versione 6.3.0, il driver JDBC per supporta il tipo di dati sql_variant. Sql_variant è supportata anche quando si usano funzionalità come Table-Valued parametri e BulkCopy, con alcune limitazioni. Non tutti i tipi di dati possono essere archiviati nel tipo di dati sql_variant. Per un elenco dei tipi di dati supportati con sql_variant, vedere [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md).

## <a name="populating-and-retrieving-a-table"></a>Popolamento e recupero di una tabella

Si supponga di avere una tabella con una colonna sql_variant come riportato di seguito:

```sql
CREATE TABLE sampleTable (col1 sql_variant)
```

Script di esempio per inserire i valori usando un'istruzione:

```java
try (Statement stmt = connection.createStatement()){
    stmt.execute("insert into sampleTable values (1)");
}
```

Inserimento del valore usando un'istruzione preparata:

```java
try (PreparedStatement preparedStatement = con.prepareStatement("insert into sampleTable values (?)")) {
    preparedStatement.setObject(1, 1);
    preparedStatement.execute();
}
```

Se il tipo sottostante dei dati passati è noto, è possibile usare il rispettivo setter. È ad esempio possibile usare `preparedStatement.setInt()` quando si inserisce un valore intero.

```java
try (PreparedStatement preparedStatement = con.prepareStatement("insert into table values (?)")) {
    preparedStatement.setInt (1, 1);
    preparedStatement.execute();
}
```

Per leggere i valori della tabella, è possibile usare i rispettivi getter. È ad esempio possibile usare i metodi `getInt()` o `getString()` se i valori provenienti dal server sono noti:

```java
try (SQLServerResultSet resultSet = (SQLServerResultSet) stmt.executeQuery("select * from sampleTable ")) {
    resultSet.next();
    resultSet.getInt(1); //or rs.getString(1); or rs.getObject(1);
}
```

## <a name="using-stored-procedures-with-sql_variant"></a>Utilizzo di stored procedure con sql_variant

Si supponga di avere una stored procedure come riportato di seguito:

```java
String sql = "CREATE PROCEDURE " + inputProc + " @p0 sql_variant OUTPUT AS SELECT TOP 1 @p0=col1 FROM sampleTable ";
```

I parametri di output devono essere registrati:

```java
try (CallableStatement callableStatement = con.prepareCall(" {call " + inputProc + " (?) }")) {
    callableStatement.registerOutParameter(1, microsoft.sql.Types.SQL_VARIANT);
    callableStatement.execute();
}
```

## <a name="limitations-of-sql_variant"></a>Limitazioni di sql_variant

- Quando si usa TVP per popolare una tabella con un `datetime` / `smalldatetime` / `date` valore archiviato in una sql_variant, `getDateTime()` / `getSmallDateTime()` / `getDate()` la chiamata a su un ResultSet non funziona e genera l'eccezione seguente:

    `Java.lang.String cannot be cast to java.sql.Timestamp`

    Soluzione alternativa: usare invece `getString()` o `getObject()`.

- L'uso di TVP per popolare una tabella e inviare un valore null in un sql_variant non è supportato. Il tentativo di eseguire questa operazione comporta un'eccezione:

    `Inserting null value with column type sql_variant in TVP is not supported.`

## <a name="see-also"></a>Vedere anche

[Informazioni sui tipi di dati del driver JDBC](understanding-the-jdbc-driver-data-types.md)
