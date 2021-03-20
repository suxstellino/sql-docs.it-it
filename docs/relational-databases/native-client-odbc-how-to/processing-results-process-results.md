---
description: Elaborazione dei risultati - Elaborare i risultati
title: Risultati processo (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- processing results [ODBC]
ms.assetid: 4810fe3f-78ee-4f0d-8bcc-a4659fbcf46f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6bf12c65d5867b358b7493eadb300752a5f862aa
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750121"
---
# <a name="processing-results---process-results"></a>Elaborazione dei risultati - Elaborare i risultati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'elaborazione dei risultati in un'applicazione ODBC comporta innanzitutto la determinazione delle caratteristiche del set di risultati, quindi il recupero dei dati nelle variabili di programma utilizzando [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) o [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md).  
  
### <a name="to-process-results"></a>Per elaborare i risultati  
  
1.  Recuperare le informazioni sul set di risultati.  
  
2.  Se si usano colonne associate, per ogni colonna con cui si intende eseguire l'associazione, chiamare [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) per associare un buffer del programma alla colonna.  
  
3.  Per ogni riga del set di risultati, effettuare le operazioni seguenti:  
  
    -   Chiamare [SQLFetch](../../odbc/reference/syntax/sqlfetch-function.md) per ottenere la riga successiva.  
  
    -   Se si utilizzano colonne associate, utilizzare i dati disponibili nei relativi buffer.  
  
    -   Se si usano colonne non associate, chiamare [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) una o più volte per ottenere i dati relativi alle colonne non associate dopo l'ultima colonna associata. Le chiamate a **SQLGetData** devono essere effettuate nell'ordine di numero di colonna crescente.  
  
    -   Chiamare **SQLGetData** più volte per ottenere dati da una colonna di tipo text o image.  
  
4.  Quando [SQLFetch](../../odbc/reference/syntax/sqlfetch-function.md) segnala la fine del set di risultati restituendo SQL_NO_DATA, chiamare [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) per determinare se è disponibile un altro set di risultati.  
  
    -   Se viene restituito SQL_SUCCESS, è disponibile un altro set di risultati.  
  
    -   Se viene restituito SQL_NO_DATA, non è più disponibile alcun set di risultati.  
  
    -   Se viene restituito SQL_SUCCESS_WITH_INFO o SQL_ERROR, chiamare [SQLGetDiagRec](../../odbc/reference/syntax/sqlgetdiagrec-function.md) per determinare se è disponibile l'output da un'istruzione PRINT o RAISERROR.  
  
         Se si utilizzano parametri di istruzione associati per i parametri di output o il valore restituito di una stored procedure, utilizzare i dati disponibili nei buffer dei parametri associati. Inoltre, quando si utilizzano parametri associati, ogni chiamata a [SQLExecute](../../odbc/reference/syntax/sqlexecute-function.md) o [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) eseguirà l'istruzione SQL *S* volte, dove *S* è il numero di elementi nella matrice dei parametri associati. Ciò significa che saranno presenti *i* set di risultati da elaborare, in cui ogni set di risultati comprende tutti i set di risultati, i parametri di output e i codici restituiti restituiti in genere da una singola esecuzione dell'istruzione SQL.  
  
    > [!NOTE]  
    >  Quando un set di risultati contiene righe di calcolo, ogni riga di calcolo viene resa disponibile come set di risultati distinto. Tali set di risultati di calcolo vengono intercalati all'interno delle normali righe e le suddividono in più set di risultati.  
  
5.  Facoltativamente, è possibile chiamare [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) con SQL_UNBIND per rilasciare qualsiasi buffer delle colonne associate.  
  
6.  Se è disponibile un altro set di risultati, andare al passaggio 1.  

> [!NOTE]  
>  Per annullare l'elaborazione di un set di risultati prima che [SQLFetch](../../odbc/reference/syntax/sqlfetch-function.md) restituisca SQL_NO_DATA, chiamare [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md).  
  
## <a name="see-also"></a>Vedere anche  
[Recuperare informazioni sul set di risultati &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/processing-results-retrieve-result-set-information.md)   
  
