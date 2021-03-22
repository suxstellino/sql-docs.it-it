---
description: Informazioni sui risultati dell'elaborazione completa, inclusi più set di risultati, da un'esecuzione di query nel driver JDBC.
title: Analisi dei risultati
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: 1605fd65414a81acc0912d774a24ff4ae10a1c9c
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793027"
---
# <a name="parsing-the-results"></a>Analisi dei risultati

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Questo articolo descrive come SQL Server preveda l'elaborazione completa dei risultati restituiti da qualsiasi query da parte degli utenti.

## <a name="update-counts-and-result-sets"></a>Conteggi aggiornamenti e set di risultati

Questa sezione illustra i due risultati più comuni restituiti da SQL Server: il conteggio aggiornamenti e il set di risultati. In generale, qualsiasi query eseguita da un utente provocherà la restituzione di uno di questi risultati. Si prevede che gli utenti gestiscano entrambi durante l'elaborazione dei risultati.

Il codice seguente è un esempio di come un utente può eseguire l'iterazione di tutti i risultati restituiti dal server:

```java
try (Connection connection = DriverManager.getConnection(URL); Statement statement = connection.createStatement()) {
    boolean resultsAvailable = statement.execute(USER_SQL);
    int updateCount = -2;
    while (true) {
        updateCount = statement.getUpdateCount();
        if (!resultsAvailable && updateCount == -1)
            break;
        if (resultsAvailable) {
            // handle ResultSet
        } else {
            // handle Update Count
        }
        resultsAvailable = statement.getMoreResults();
    }
}
```

## <a name="exceptions"></a>Eccezioni

Quando si esegue un'istruzione che restituisce un errore o un messaggio informativo, SQL Server possibile rispondere in modo diverso. Se, ad esempio, non è possibile generare un piano di esecuzione, il messaggio di errore verrà immediatamente generato `execute()` . Se un errore è correlato all'elaborazione di istruzioni, l'applicazione deve analizzare il set di risultati per vedere l'eccezione.

Quando SQL Server non è in grado di generare un piano di esecuzione, l'eccezione viene generata immediatamente.

```java
String SQL = "SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    statement.execute(SQL);
} catch (SQLException e) {
    e.printStackTrace();
}
```

Quando SQL Server restituisce un messaggio di errore in un set di risultati, quest'ultimo deve essere elaborato per recuperare l'eccezione.

```java
String SQL = "SELECT 1/0;";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    if (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            // Exception is thrown on next().
            while (rs.next()) {}
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Se l'esecuzione dell'istruzione genera più set di risultati, è necessario elaborare ogni set di risultati fino a raggiungere quello con l'eccezione.

```java
String SQL = "SELECT 1; SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    // Does not throw an exception on execute().
    boolean hasResult = statement.execute(SQL);
    while (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            while (rs.next()) {
                System.out.println(rs.getString(1));
            }
        }
        // Moves the next result set that generates the exception.
        hasResult = statement.getMoreResults();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

Ad esempio, `String SQL = "SELECT * FROM nonexistentTable; SELECT 1;";` , genera immediatamente un'eccezione `execute()` e `SELECT 1` non viene eseguita.

Se l'errore restituito da SQL Server ha una gravità compresa tra `0` e `9`, viene considerato come un messaggio informativo e restituito come `SQLWarning`.

```java
String SQL = "RAISERROR ('WarningLevel5', 5, 2);";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    SQLWarning warning = statement.getWarnings();
    System.out.println(warning);
}
```

## <a name="see-also"></a>Vedere anche

[Panoramica del driver JDBC](overview-of-the-jdbc-driver.md)
