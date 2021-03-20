---
description: Costruzione di istruzioni SQL per i cursori
title: Creazione di istruzioni SQL per i cursori | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], statement construction
- ODBC applications, cursors
- SQL Server Native Client ODBC driver, statements
- statements [ODBC], constructing
- ODBC applications, statements
- statements [ODBC], cursors
ms.assetid: 134003fd-9c93-4f5c-a988-045990933b80
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b294b77280449b275800ea75d12ca997ed1569b5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748001"
---
# <a name="constructing-sql-statements-for-cursors"></a>Costruzione di istruzioni SQL per i cursori
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client utilizza cursori server per implementare la funzionalità del cursore definita nella specifica ODBC. Un'applicazione ODBC controlla il comportamento del cursore utilizzando [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) per impostare attributi di istruzione differenti. Di seguito sono indicati gli attributi e le rispettive impostazioni predefinite.  
  
|Attributo|Predefinito|  
|---------------|-------------|  
|SQL_ATTR_CONCURRENCY|SQL_CONCUR_READ_ONLY|  
|SQL_ATTR_CURSOR_TYPE|SQL_CURSOR_FORWARD_ONLY|  
|SQL_ATTR_CURSOR_SCROLLABLE|SQL_NONSCROLLABLE|  
|SQL_ATTR_CURSOR_SENSITIVITY|SQL_UNSPECIFIED|  
|SQL_ATTR_ROW_ARRAY_SIZE|1|  
  
 Quando queste opzioni sono impostate sui valori predefiniti al momento dell'esecuzione di un'istruzione SQL, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client non utilizza un cursore server per implementare il set di risultati, bensì utilizza un set di risultati predefinito. Se una di queste opzioni viene modificata rispetto alle impostazioni predefinite nel momento in cui viene eseguita un'istruzione SQL, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client tenta di utilizzare un cursore server per implementare il set di risultati.  
  
 I set di risultati predefiniti supportano tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Non sono presenti restrizioni sui tipi di istruzioni SQL che è possibile eseguire quando si utilizza un set di risultati predefinito.  
  
 I cursori server non supportano tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e non supportano un'istruzione SQL che genera più set di risultati.  
  
 Di seguito sono indicati i tipi di istruzioni non supportati dai cursori server:  
  
-   Batch  
  
     Istruzioni SQL compilate da due o più istruzioni SQL SELECT singole, ad esempio:  
  
    ```  
    SELECT * FROM Authors; SELECT * FROM Titles  
    ```  
  
-   Stored procedure con più istruzioni SELECT  
  
     Istruzioni SQL che eseguono una stored procedure che contiene più di un'istruzione SELECT. Sono incluse le istruzioni SELECT che vengono inserite in parametri o variabili.  
  
-   Parole chiave  
  
     Istruzioni SQL contenenti le parole chiave FOR BROWSE o INTO.  
  
 Se in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un'istruzione SQL che corrisponde a una di queste condizioni viene eseguita con un cursore server, il cursore server viene convertito implicitamente in un set di risultati predefinito. Quando **SQLExecDirect** o **sqlexecute** restituisce SQL_SUCCESS_WITH_INFO, gli attributi del cursore verranno impostati nuovamente sulle impostazioni predefinite.  
  
 Le istruzioni SQL che non rientrano nelle categorie sopra riportate possono essere eseguite con qualsiasi impostazione degli attributi di istruzione. Il funzionamento è identico con un set di risultati predefinito o con un cursore server.  
  
## <a name="errors"></a>Errors  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 e versioni successive un tentativo di eseguire un'istruzione che produce più set di risultati genera SQL_SUCCESS_WITH INFO e il messaggio seguente:  
  
```  
SqlState: 01S02"  
pfNative: 0  
szErrorMsgString: "[Microsoft][SQL Server Native Client][SQL Server]  
               Cursor type changed."  
```  
  
 Le applicazioni ODBC che ricevono questo messaggio possono chiamare [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) per determinare le impostazioni correnti del cursore.  
  
 Un tentativo di eseguire una procedura con più istruzioni SELECT quando si utilizzano cursori server genera l'errore seguente:  
  
```  
SqlState: 42000  
pfNative: 16937  
szErrorMsgString: [Microsoft][SQL Server Native Client][SQL Server]  
               A server cursor is not allowed on a stored procedure  
               with more than one SELECT statement in it. Use a  
               default result set or client cursor.  
```  
  
 Un tentativo di eseguire un batch con più istruzioni SELECT quando l'utilizzo di cursori server genera l'errore seguente:  
  
```  
SqlState: 42000  
pfNative: 16938  
szErrorMsgString: [Microsoft][SQL Server Native Client][SQL Server]  
               sp_cursoropen. The statement parameter can only  
               be a single SELECT statement or a single stored   
               procedure.  
```  
  
 Nelle applicazioni ODBC che ricevono questi errori devono essere reimpostati tutti gli attributi di istruzione di cursore sui valori predefiniti prima di tentare di eseguire l'istruzione.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di query &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
