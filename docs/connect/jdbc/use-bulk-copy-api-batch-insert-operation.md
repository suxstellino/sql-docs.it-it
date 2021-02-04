---
title: API di copia bulk per l'inserimento batch in JDBC
description: Microsoft JDBC driver per SQL Server supporta l'utilizzo della copia bulk per gli inserimenti batch per il caricamento più rapido dei dati nel database.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a769d73f799b8ca0b4b806a3e656517377e23ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161570"
---
# <a name="using-bulk-copy-api-for-batch-insert-operation"></a>Uso dell'API di copia bulk per un'operazione di inserimento batch

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft JDBC driver per SQL Server versione 9,2 e successive supporta l'utilizzo dell'API per la copia bulk per le operazioni di inserimento batch. Questa funzionalità consente agli utenti di consentire al driver di eseguire operazioni di copia bulk sottostanti durante l'esecuzione di operazioni di inserimento in batch. Il driver cerca di migliorare le prestazioni inserendo gli stessi dati che si avrebbero con una normale operazione di inserimento batch. Il driver analizza la query SQL dell'utente, usando l'API per la copia bulk invece della consueta operazione di inserimento batch. Di seguito sono riportati diversi modi per abilitare l'API per la copia bulk per la funzionalità di inserimento in batch ed elencarne le limitazioni. Questa pagina contiene un piccolo codice di esempio che illustra un utilizzo e anche l'aumento delle prestazioni.

Questa funzionalità è applicabile solo alle API `executeBatch()` & `executeLargeBatch()` di PreparedStatement e CallableStatement.

## <a name="prerequisites"></a>Prerequisiti

Prerequisito per l'abilitazione dell'API per la copia bulk per l'inserimento batch.

* La query deve essere una query di inserimento (la query può contenere commenti, ma deve iniziare con la parola chiave INSERT perché questa funzionalità venga applicata).

## <a name="enabling-bulk-copy-api-for-batch-insert"></a>Abilitazione dell'API di copia bulk per l'inserimento batch

Sono disponibili tre modi per abilitare l'API di copia bulk per l'inserimento batch.

### <a name="1-enabling-with-connection-property"></a>1. Abilitazione con proprietà di connessione

L'aggiunta di **useBulkCopyForBatchInsert=true;** alla stringa di connessione consente di abilitare questa funzionalità.

```java
Connection connection = DriverManager.getConnection("jdbc:sqlserver://<server>:<port>;userName=<user>;password=<password>;database=<database>;useBulkCopyForBatchInsert=true;");
```

### <a name="2-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverconnection-object"></a>2. Abilitazione con il metodo setUseBulkCopyForBatchInsert() dall'oggetto SQLServerConnection

La chiamata di **SQLServerConnection. setUseBulkCopyForBatchInsert (true)** consente di abilitare questa funzionalità.

**SQLServerConnection.getUseBulkCopyForBatchInsert()** consente di recuperare il valore corrente per la proprietà di connessione **useBulkCopyForBatchInsert**.

Il valore per **useBulkCopyForBatchInsert** rimane costante per ogni PreparedStatement al momento della sua inizializzazione. Qualsiasi chiamata successiva a **SQLServerConnection. setUseBulkCopyForBatchInsert ()** non influirà sul valore di PreparedStatement già creato.

### <a name="3-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverdatasource-object"></a>3. Abilitazione con il metodo setUseBulkCopyForBatchInsert() dall'oggetto SQLServerDataSource

Simile a quanto descritto sopra, ma con l'uso di SQLServerDataSource per creare un oggetto SQLServerConnection. Entrambi i metodi ottengono lo stesso risultato.

## <a name="known-limitations"></a>Limitazioni note

Al momento, questa funzionalità è soggetta alle limitazioni seguenti.

* Le query INSERT che contengono valori senza parametri (ad esempio, `INSERT INTO TABLE VALUES (?, 2` )) non sono supportate. I caratteri jolly (?) sono gli unici parametri supportati per questa funzione.
* Le query INSERT che contengono espressioni INSERT-SELECT (ad esempio, `INSERT INTO TABLE SELECT * FROM TABLE2` ) non sono supportate.
* Le query INSERT che contengono più espressioni di valore (ad esempio, `INSERT INTO TABLE VALUES (1, 2) (3, 4)` ) non sono supportate.
* Le query di inserimento seguite dalla clausola OPTION, unite a più tabelle o seguite da un'altra query, non sono supportate.
* A causa delle limitazioni dell'API per la copia bulk, i tipi di dati,,,, `MONEY` `SMALLMONEY` `DATE` `DATETIME` `DATETIMEOFFSET` , `SMALLDATETIME` , `TIME` , `GEOMETRY` e `GEOGRAPHY` non sono attualmente supportati per questa funzionalità.

Se la query ha esito negativo a causa di errori non correlati a "SQL Server", il driver registrerà il messaggio di errore e il fallback alla logica originale per l'inserimento batch.

## <a name="example"></a>Esempio

Questo è un esempio in cui viene illustrato il caso di utilizzo per un'operazione di inserimento batch di migliaia di righe, sia per gli scenari di API di copia di confronto normali

```java
    public static void main(String[] args) throws Exception
    {
        String tableName = "batchTest";
        String tableNameBulkCopyAPI = "batchTestBulk";

        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>";

        try (Connection con = DriverManager.getConnection(connectionUrl);
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableName + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableName + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableName + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableName + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using regular batch insert operation.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }

        try (Connection con = DriverManager.getConnection(connectionUrl + ";useBulkCopyForBatchInsert=true");
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableNameBulkCopyAPI + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableNameBulkCopyAPI + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableNameBulkCopyAPI + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableNameBulkCopyAPI + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using Bulk Copy API.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }
    }
```

Risultato:

```bash
Starting batch operation using regular batch insert operation.
Finished. Time taken : 104132 milliseconds.
Starting batch operation using Bulk Copy API.
Finished. Time taken : 1058 milliseconds.
```

## <a name="see-also"></a>Vedere anche

[Uso del driver JDBC per il miglioramento di prestazioni e affidabilità](improving-performance-and-reliability-with-the-jdbc-driver.md)
