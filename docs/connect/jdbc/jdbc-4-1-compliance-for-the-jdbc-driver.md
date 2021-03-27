---
description: Informazioni sul modo in cui il driver JDBC per SQL Server è conforme alla specifica JDBC 4,1.
title: Conformità con JDBC 4.1 per il driver JDBC
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: f087fd40-8451-478e-b465-43112c711515
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60fef7e2a6340e8d450732b814035ff8e92714c8
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633193"
---
# <a name="jdbc-41-compliance-for-the-jdbc-driver"></a>Conformità con JDBC 4.1 per il driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

> [!NOTE]
> Le versioni precedenti a Microsoft JDBC Driver 4.2 per SQL Server sono compatibili con le specifiche Java Database Connectivity API 4.0. Questa sezione non è applicabile per le versioni precedenti alla 4.2.

La specifica Java Database Connectivity API 4.1 è supportata da Microsoft JDBC Driver 4.2 per SQL Server, con i metodi API seguenti.

## <a name="sqlserverconnection-class"></a>SQLServerConnection, classe

|Nuovo metodo|Descrizione|Implementazione del driver JDBC|
|----------------|-----------------|--------------------------------|
|void abort(Executor executor)|Termina una connessione aperta a SQL Server.|Implementato come descritto nell'interfaccia java.sql.Connection. Per ulteriori informazioni, vedere [java. SQL. Connection](https://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|
|void setSchema(String schema)|Imposta lo schema per la connessione corrente.|SQL Server non supporta l'impostazione dello schema per la sessione corrente. Se questo metodo viene chiamato, il driver registra un messaggio di avviso in modo invisibile all'utente. Per ulteriori informazioni, vedere [java. SQL. Connection](https://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|
|String getSchema()|Restituisce il nome dello schema per la connessione corrente.|Poiché SQL Server non supporta l'impostazione dello schema per la connessione corrente, il driver restituisce invece lo schema predefinito dell'utente. Per ulteriori informazioni, vedere [java. SQL. Connection](https://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|

## <a name="sqlserverdatabasemetadata-class"></a>SQLServerDatabaseMetaData, classe

|Nuovo metodo|Descrizione|Implementazione del driver JDBC|
|----------------|-----------------|--------------------------------|
|boolean generatedKeyAlwaysReturned()|Restituisce true, dal momento che il driver supporta il recupero delle chiavi generate|Implementato come descritto in java.sql. Interfaccia DatabaseMetaData. Per ulteriori informazioni, vedere [java. SQL. DatabaseMetaData](https://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html).|
|ResultSet getPseudoColumns (String Catalog, String schemaPattern, String tableNamePattern, String columnNamePattern)|Recupera una descrizione delle pseudocolonne o delle colonne nascoste|Restituisce un set di risultati vuoto come SQL Server non dispone di una nozione formale di pseudo-colonne. Per ulteriori informazioni, vedere [java. SQL. DatabaseMetaData](https://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html).|

## <a name="sqlserverstatement-class"></a>SQLServerStatement, classe

|Nuovo metodo|Descrizione|Implementazione del driver JDBC|
|----------------|-----------------|--------------------------------|
|void closeOnCompletion()|Specifica che l'istruzione verrà chiusa quando vengono chiusi tutti i relativi set di risultati dipendenti.|Implementato come descritto nell'interfaccia java.sql.Statement. Per ulteriori informazioni, vedere [java. SQL. Statement](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html).|
|boolean isCloseOnCompletion()|Restituisce un valore che indica se l'istruzione verrà chiusa quando vengono chiusi tutti i relativi set di risultati dipendenti.|Implementato come descritto nell'interfaccia java.sql.Statement. Per ulteriori informazioni, vedere [java. SQL. Statement](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html).|

 La specifica Java Database Connectivity API 4.1 è supportata da Microsoft JDBC Driver 4.2 per SQL Server, con le funzionalità seguenti.

|Nuova funzionalità|Descrizione|
|-----------------|-----------------|
|Nuova funzione di escape<br /><br /> Escape di righe restituite limitate|Parzialmente supportato<br /><br /> Sintassi di escape: LIMIT \<rows> [OFFSET <row_offset>](using-sql-escape-sequences.md).|

La specifica Java Database Connectivity API 4.1 è supportata da Microsoft JDBC Driver 4.2 per SQL Server, con i mapping dei tipi di dati seguenti.

|Mapping di tipi di dati|Descrizione|
|------------------------|-----------------|
|Ora sono supportati nuovi mapping dei tipi di dati nei metodi PreparedStatement.setObject() e PreparedStatement.setNull().|1. Nuovo mapping dei tipi da Java a JDBC<br /><br /> (a) Da java.math.BigInteger a BIGINT JDBC<br /><br /> (b) Da java.util.Date e java.util.Calendar a TIMESTAMP JDBC<br /><br /> 2. Nuove conversioni dei tipi di dati:<br /><br /> (a) Java. Math. BigInteger per CHAR, VARCHAR, LONGVARCHAR e BIGINT<br /><br /> (b) Java. util. date e Java. util. Calendar to CHAR, VARCHAR, LONGVARCHAR, DATE, TIME e TIMESTAMP<br /><br /> Per ulteriori informazioni, vedere la specifica JDBC 4,1.|
