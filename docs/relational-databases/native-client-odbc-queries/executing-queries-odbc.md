---
title: Esecuzione di query (ODBC) | Microsoft Docs
description: Un'applicazione ODBC può eseguire istruzioni su un'istanza di SQL Server inizializzando un handle di connessione e connettendosi a un'origine dati.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, executing queries
- SQL Server Native Client ODBC driver, statements
- ODBC applications, statements
- SQL Server Native Client ODBC driver, queries
- queries [ODBC]
ms.assetid: d935bcba-8ce6-4159-8395-6c86431602ad
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b686da10b222ed26c5a234c81f6f2314d9e9a2f1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751291"
---
# <a name="executing-queries-odbc"></a>Esecuzione di query (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Dopo che un'applicazione ODBC inizializza un handle di connessione e si connette a un'origine dati, alloca uno o più handle di istruzione nell'handle di connessione. L'applicazione può quindi eseguire [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istruzioni nell'handle di istruzione. Di seguito viene indicata la sequenza generale degli eventi nell'esecuzione di un'istruzione SQL:  
  
1.  Impostare tutti gli attributi di istruzione necessari.  
  
2.  Creare l'istruzione.  
  
3.  Eseguire l'istruzione.  
  
4.  Recuperare tutti i set di risultati.  
  
 Dopo che un'applicazione recupera tutte le righe in tutti i set di risultati restituiti dall'istruzione SQL, può eseguire un'altra query sullo stesso handle di istruzione. Se un'applicazione determina che non è necessario recuperare tutte le righe in un determinato set di risultati, può annullare il resto del set di risultati chiamando [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) o [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md).  
  
 Se in un'applicazione ODBC è necessario eseguire la stessa istruzione SQL più volte con dati diversi, utilizzare un marcatore di parametro rappresentato da un punto interrogativo (?) nella creazione di un'istruzione SQL:  
  
```  
INSERT INTO MyTable VALUES (?, ?, ?)  
```  
  
 Ogni marcatore di parametro può quindi essere associato a una variabile di programma chiamando [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md).  
  
 Al termine dell'esecuzione di tutte le istruzioni SQL e dell'elaborazione dei relativi set di risultati, l'applicazione rilascia l'handle di istruzione.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client supporta più handle di istruzione per ogni handle di connessione. Le transazioni vengono gestite a livello di connessione, in modo che tutte le operazioni eseguite su tutti gli handle di gestione in un singolo handle di connessione vengano gestite come parte della stessa transazione.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Allocazione di un handle di istruzione](../../relational-databases/native-client-odbc-queries/allocating-a-statement-handle.md)  
  
-   [Creazione di un'istruzione SQL &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/constructing-an-sql-statement-odbc.md)  
  
-   [Costruzione di istruzioni SQL per i cursori](../../relational-databases/native-client-odbc-queries/constructing-sql-statements-for-cursors.md)  
  
-   [Utilizzo dei parametri delle istruzioni](../../relational-databases/native-client-odbc-queries/using-statement-parameters.md)  
  
-   [Esecuzione di istruzioni &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
-   [Rilascio di un handle di istruzione](../../relational-databases/native-client-odbc-queries/freeing-a-statement-handle.md)  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  
