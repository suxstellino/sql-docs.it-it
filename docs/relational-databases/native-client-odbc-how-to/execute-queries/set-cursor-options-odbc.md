---
description: Impostare le opzioni del cursore (ODBC)
title: Impostare le opzioni del cursore (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], options
ms.assetid: 0e72b48a-fc5a-4656-8cf5-39f57d8c1565
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f15fe2c6ada467fa03033f0d2dd32c5631fa8e59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747841"
---
# <a name="set-cursor-options-odbc"></a>Impostare le opzioni del cursore (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per impostare le opzioni del cursore, chiamare [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) per impostare o [SQLGetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) per ottenere le opzioni di istruzione che controllano il comportamento del cursore.  
  
|*Attributo*|Specifica|  
|-----------------|---------------|  
|SQL_ATTR_CURSOR_TYPE|Tipo di cursore: forward only, statico, dinamico o gestito da keyset|  
|SQL_ATTR_CONCURRENCY|Opzione di controllo della concorrenza: di sola lettura, di blocco, ottimistica che utilizza timestamp o ottimistica che utilizza valori|  
|SQL_ATTR_ROW_ARRAY_SIZE|Numero di righe recuperate in ogni operazione di recupero|  
|SQL_ATTR_CURSOR_SENSITIVITY|Cursore che mostra o non mostra aggiornamenti a righe di cursore create da altre connessioni|  
|SQL_ATTR_CURSOR_SCROLLABLE|Cursore che è possibile scorrere in avanti e indietro|  
  
 I valori predefiniti per questi attributi (forward only, di sola lettura, dimensione 1 del set di righe) non determinano l'utilizzo dei cursori server. Per utilizzare i cursori server, è necessario che almeno uno di questi attributi sia impostato su un valore diverso dall'impostazione predefinita e che l'istruzione eseguita sia un'istruzione SELECT singola o una stored procedure che contiene un'istruzione SELECT singola. In caso di utilizzo di cursori server, le istruzioni SELECT non possono utilizzare le clausole non supportate dai cursori server: COMPUTE, COMPUTE BY, FOR BROWSE e INTO.  
  
 È possibile controllare il tipo di cursore utilizzato impostando SQL_ATTR_CURSOR_TYPE e SQL_ATTR_CONCURRENCY oppure impostando SQL_ATTR_CURSOR_SENSITIVITY e SQL_ATTR_CURSOR_SCROLLABLE. È consigliabile non combinare i due metodi di definizione del comportamento del cursore.  
  
## <a name="examples"></a>Esempi  

### <a name="a-set-a-dynamic-cursor"></a>R. Imposta un cursore dinamico

 Nell'esempio seguente viene allocato un handle di istruzione, viene impostato un tipo di cursore dinamico con concorrenza ottimistica del controllo delle versioni delle righe, quindi viene eseguita un'istruzione SELECT.  
  
```  
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER)SQL_CURSOR_DYNAMIC, SQL_IS_INTEGER);  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CONCURRENCY, SQLPOINTER)SQL_CONCUR_ROWVER, SQL_IS_INTEGER);  
retcode = SQLExecDirect(hstmt1, SELECT au_lname FROM authors", SQL_NTS);  
```  
  
### <a name="b-set-a-scrollable-sensitive-cursor"></a>B. Imposta un cursore sensibile scorrevole
 Nell'esempio seguente viene allocato un handle di istruzione, viene impostato un cursore scorrevole di tipo sensitive, quindi viene eseguita un'istruzione SELECT  
  
```  
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
  
// Set the cursor options and execute the statement.  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CURSOR_SCROLLABLE, SQLPOINTER)SQL_SCROLLABLE, SQL_IS_INTEGER);  
retcode = SQLSetStmtAttr(hstmt1, SQL_ATTR_CURSOR_SENSITIVITY, SQLPOINTER)SQL_INSENSITIVE, SQL_IS_INTEGER);  
retcode = SQLExecDirect(hstmt1, select au_lname from authors", SQL_NTS);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure relative all'esecuzione di query &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
  
