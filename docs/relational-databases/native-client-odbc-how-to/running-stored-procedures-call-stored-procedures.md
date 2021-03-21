---
title: Stored procedure call (ODBC) | Microsoft Docs
description: Informazioni su come aggiungere un'origine dati tramite amministratore ODBC, a livello di codice o tramite un file, prima di utilizzare le applicazioni ODBC con SQL Server 2005 o versione successiva.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], calling
ms.assetid: 31176be8-d40e-4f93-8d44-a46e804a3e2d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 152ecb6304a4049418e82dee42d9dd236fc38f4b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751841"
---
# <a name="running-stored-procedures---call-stored-procedures"></a>Esecuzione delle stored procedure - Chiamare le stored procedure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta l'esecuzione di stored procedure come stored procedure remote. L'esecuzione di una stored procedure come stored procedure remota consente al driver e al server di ottimizzare le prestazioni di esecuzione della procedura.  
  
  Quando un'istruzione SQL chiama una stored procedure utilizzando la clausola di escape ODBC CALL, tramite driver di Microsoft® SQL Server™ la stored procedure viene inviata a SQL Server utilizzando il meccanismo di chiamata RPC (Remote Procedure Call). Le richieste RPC ignorano la maggior parte dell'analisi delle istruzioni e dell'elaborazione dei parametri in SQL Server e sono più veloci rispetto a un'istruzione Transact-SQL EXECUTE.  
  
 Per un'applicazione di esempio in cui viene illustrata questa funzionalità, vedere [elaborare i codici restituiti e i parametri di Output &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
### <a name="to-run-a-procedure-as-an-rpc"></a>Per eseguire una procedura come RPC  
  
1.  Costruire un'istruzione SQL che utilizzi la sequenza di escape ODBC CALL. Nell'istruzione vengono utilizzati marcatori di parametro per ogni parametro di input, input/output e di output e per il valore restituito della procedura, se disponibile:  
  
    ```  
    {? = CALL procname (?,?)}  
    ```  
  
2.  Chiamare [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) per ogni parametro di input, di input/output e di output e per il valore restituito della procedura, se disponibile.  
  
3.  Eseguire l'istruzione con [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md).  
  
> [!NOTE]  
>  Se un'applicazione invia una procedura utilizzando la sintassi Transact-SQL EXECUTE, invece della sequenza di escape ODBC CALL, il driver ODBC di SQL Server passa la chiamata di procedura a SQL Server come istruzione SQL anziché come chiamata RPC. Se viene utilizzata l'istruzione Transact-SQL EXECUTE, inoltre, i parametri di output non vengono restituiti.  
  
## <a name="see-also"></a>Vedere anche  
  [Invio in batch di chiamate a stored procedure](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)   
 [Esecuzione di stored procedure](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Chiamata di una stored procedure](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)   
 [Procedure](../../relational-databases/native-client-odbc-queries/executing-statements/procedures.md)  
  
