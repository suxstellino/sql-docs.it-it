---
title: Uso di tipi di dati di base JDBC
description: Microsoft JDBC driver per SQL Server usa i tipi di dati JDBC di base per convertire i tipi di dati SQL Server in un formato che può essere riconosciuto da Java.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: d7044936-5b8c-4def-858c-28a11ef70a97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1c6f764fffca3408380da9c96227eb9d7bb4457c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195268"
---
# <a name="using-basic-data-types"></a>Uso di tipi di dati di base

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] usa i tipi di dati JDBC di base per convertire i tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un formato comprensibile nel linguaggio di programmazione Java e viceversa. Il driver JDBC offre supporto per l'API di JDBC 4.0, che include il tipo di dati **SQLXML** e i tipi di dati National (Unicode) come **NCHAR**, **NVARCHAR**, **LONGNVARCHAR** e **NCLOB**.  
  
## <a name="data-type-mappings"></a>Mapping di tipi di dati

Nella tabella seguente sono elencati i mapping predefiniti tra i tipi di dati di base di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], JDBC e del linguaggio di programmazione Java:  
  
| Tipi di SQL Server   | Tipi JDBC (java.sql.Types)                        | Tipi del linguaggio Java          |
| ------------------ | -------------------------------------------------- | ---------------------------- |
| bigint             | bigint                                             | long                         |
| BINARY             | BINARY                                             | byte[]                       |
| bit                | BIT                                                | boolean                      |
| char               | CHAR                                               | string                       |
| Data               | DATE                                               | java.sql.Date                |
| datetime<sup>3</sup>          | timestamp                               | java.sql.Timestamp           |
| datetime2          | timestamp                                          | java.sql.Timestamp           |
| datetimeoffset<sup>2</sup> | microsoft.sql.Types.DATETIMEOFFSET         | microsoft.sql.DateTimeOffset |
| decimal            | DECIMAL                                            | java.math.BigDecimal         |
| float              | DOUBLE                                             | double                       |
| image              | LONGVARBINARY                                      | byte[]                       |
| INT                | INTEGER                                            | INT                          |
| money              | DECIMAL                                            | java.math.BigDecimal         |
| NCHAR              | CHAR<br /><br /> NCHAR (Java SE 6.0)               | string                       |
| ntext              | LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0) | string                       |
| NUMERIC            | NUMERIC                                            | java.math.BigDecimal         |
| NVARCHAR           | VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)         | string                       |
| nvarchar(max)      | VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)         | string                       |
| real               | real                                               | float                        |
| smalldatetime      | timestamp                                          | java.sql.Timestamp           |
| SMALLINT           | SMALLINT                                           | short                        |
| SMALLMONEY         | DECIMAL                                            | java.math.BigDecimal         |
| text               | LONGVARCHAR                                        | string                       |
| time               | TIME<sup>1</sup>                                   | java.sql.Time<sup>1</sup>            |
| timestamp          | BINARY                                             | byte[]                       |
| TINYINT            | TINYINT                                            | short                        |
| udt                | VARBINARY                                          | byte[]                       |
| UNIQUEIDENTIFIER   | CHAR                                               | string                       |
| varbinary          | VARBINARY                                          | byte[]                       |
| varbinary(max)     | VARBINARY                                          | byte[]                       |
| varchar            | VARCHAR                                            | string                       |
| ntext       | VARCHAR                                            | string                       |
| Xml                | LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0) | string<br /><br /> SQLXML    |
| sqlvariant         | microsoft.sql.Types.SQL_VARIANT                    | Oggetto                       |
| geometry           | VARBINARY                                          | byte[]                       |
| geography          | VARBINARY                                          | byte[]                       |
  
<sup>1</sup> Per usare java.sql.Time con il tipo time di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario impostare la proprietà di connessione **sendTimeAsDatetime** su false.  
  
<sup>2</sup> È possibile accedere a livello di codice ai valori di **datetimeoffset** con la [classe DateTimeOffset](reference/datetimeoffset-class.md).  
  
<sup>3</sup> Si noti che a partire da SQL Server 2016 non è più possibile usare i valori java.sql.Timestamp per confrontare i valori di una colonna datetime. Questa limitazione è dovuta a una modifica sul lato server che converte datetime in datetime2 in modo diverso, ottenendo valori non equi. La soluzione alternativa a questo problema consiste nel modificare le colonne di tipo datetime in datetime2(3), usare una stringa anziché java.sql.Timestamp o modificare il livello di compatibilità del database a 120 o inferiore.
  
Nelle sezioni seguenti vengono forniti esempi di come sia possibile utilizzare il driver JDBC e i tipi di dati di base. Per un esempio più dettagliato dell'utilizzo dei tipi di dati di base in un'applicazione Java, vedere [Esempio di tipi di dati di base](basic-data-types-sample.md).  
  
## <a name="retrieving-data-as-a-string"></a>Recupero di dati in forma di stringa

Se è necessario recuperare dati da un'origine dati con mapping a uno qualsiasi dei tipi di dati JDBC di base, per visualizzarli come stringa, o se non sono richiesti dati fortemente tipizzati, è possibile usare il metodo [getString](reference/getstring-method-sqlserverresultset.md) della classe [SQLServerResultSet](reference/sqlserverresultset-class.md), come nell'esempio seguente:  
  
[!code[JDBC#UsingBasicDataTypes1](codesnippet/Java/using-basic-data-types_1.java)]  
  
## <a name="retrieving-data-by-data-type"></a>Recupero di dati per tipo di dati

Se è necessario recuperare dati di cui si conosce il tipo da un'origine dati, usare uno dei metodi get\<Type> della classe SQLServerResultSet, noti anche come *metodi getter*. Con i metodi get\<Type> è possibile usare un nome o un indice di colonna, come nell'esempio seguente:  
  
[!code[JDBC#UsingBasicDataTypes2](codesnippet/Java/using-basic-data-types_2.java)]  
  
> [!NOTE]  
> I metodi con scala getUnicodeStream e getBigDecimal sono deprecati e non sono supportati dal driver JDBC.

## <a name="updating-data-by-data-type"></a>Aggiornamento di dati per tipo di dati

Se è necessario aggiornare il valore di un campo in un'origine dati, usare uno dei metodi update\<Type> della classe SQLServerResultSet. Nell'esempio seguente il metodo [updateInt](reference/updateint-method-sqlserverresultset.md) viene usato insieme al metodo [updateRow](reference/updaterow-method-sqlserverresultset.md) per aggiornare i dati nell'origine dati:  
  
[!code[JDBC#UsingBasicDataTypes3](codesnippet/Java/using-basic-data-types_3.java)]  
  
> [!NOTE]  
> Il driver JDBC non è in grado di aggiornare una colonna SQL Server il cui nome contiene più di 127 caratteri. Se si tenta di aggiornare una colonna il cui nome contiene più di 127 caratteri, verrà generata un'eccezione.  
  
## <a name="updating-data-by-parameterized-query"></a>Aggiornamento di dati mediante query con parametri

Se è necessario aggiornare dati in un'origine dati usando una query con parametri, è possibile impostare il tipo di dati dei parametri usando uno dei metodi set\<Type> della classe [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md), noti anche come *metodi setter*. Nell'esempio seguente viene usato il metodo [prepareStatement](reference/preparestatement-method-sqlserverconnection.md) per precompilare la query con parametri, quindi viene usato il metodo [setString](reference/setstring-method-sqlserverpreparedstatement.md) per impostare il valore stringa del parametro prima di chiamare il metodo [executeUpdate](reference/executeupdate-method.md).  
  
[!code[JDBC#UsingBasicDataTypes4](codesnippet/Java/using-basic-data-types_4.java)]  
  
Per altre informazioni sulle query con parametri, vedere [Uso di istruzioni SQL con parametri](using-an-sql-statement-with-parameters.md).  

## <a name="passing-parameters-to-a-stored-procedure"></a>Passaggio di parametri a una stored procedure

Se è necessario passare parametri tipizzati in una stored procedure, è possibile impostare i parametri in base all'indice o al nome usando uno dei metodi set\<Type> della classe [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md). Nell'esempio seguente viene usato il metodo [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) per impostare la chiamata alla stored procedure, quindi viene usato il metodo [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) per impostare il parametro per la chiamata prima di chiamare il metodo [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md).  
  
[!code[JDBC#UsingBasicDataTypes5](codesnippet/Java/using-basic-data-types_5.java)]  
  
> [!NOTE]  
> In questo esempio viene restituito un set di risultati con i risultati dell'esecuzione della stored procedure.

Per altre informazioni sull'uso del driver JDBC con stored procedure e parametri di input, vedere [Uso di una stored procedure con parametri di input](using-a-stored-procedure-with-input-parameters.md).  

## <a name="retrieving-parameters-from-a-stored-procedure"></a>Recupero di parametri da una stored procedure

Se è necessario recuperare parametri da una stored procedure, occorre innanzitutto registrare un parametro OUT in base al nome o all'indice, usando il metodo [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) della classe SQLServerCallableStatement, e quindi assegnare il parametro OUT restituito a una variabile appropriata, dopo aver eseguito la chiamata alla stored procedure. Nell'esempio seguente viene usato il metodo prepareCall per impostare la chiamata alla stored procedure, quindi viene usato il metodo registerOutParameter per impostare il parametro OUT e infine viene usato il metodo [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) per impostare il parametro per la chiamata prima di chiamare il metodo executeQuery. Il valore restituito dal parametro OUT della stored procedure viene recuperato usando il metodo [getShort](../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md).  
  
[!code[JDBC#UsingBasicDataTypes6](../../connect/jdbc/codesnippet/Java/using-basic-data-types_6.java)]  
  
> [!NOTE]  
> Oltre al parametro out, può essere restituito anche un set di risultati con i risultati dell'esecuzione della stored procedure.  
  
Per altre informazioni sull'uso del driver JDBC con stored procedure e parametri di output, vedere [Uso di una stored procedure con parametri di output](../../connect/jdbc/using-a-stored-procedure-with-output-parameters.md).  

## <a name="see-also"></a>Vedere anche

[Informazioni sui tipi di dati del driver JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
