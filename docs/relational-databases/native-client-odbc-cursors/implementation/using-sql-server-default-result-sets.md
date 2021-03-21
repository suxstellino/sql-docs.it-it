---
description: Utilizzo dei set di risultati predefiniti di SQL Server
title: Utilizzo di SQL Server set di risultati predefiniti | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function
- ODBC cursors, default result sets
- cursors [ODBC], default result sets
- default result sets
- result sets [ODBC], default
- ODBC applications, cursors
ms.assetid: ee1db3e5-60eb-4425-8a6b-977eeced3f98
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a810c08596567e2bed2e8d4484b9b6676f4af44c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756241"
---
# <a name="using-sql-server-default-result-sets"></a>Utilizzo dei set di risultati predefiniti di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gli attributi predefiniti del cursore ODBC sono:  
  
```  
SQLSetStmtAttr(hstmt, SQL_ATTR_CURSOR_TYPE, SQL_CURSOR_FORWARD_ONLY, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_CONCURRENCY, SQL_CONCUR_READ_ONLY, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, 1, SQL_IS_INTEGER);  
```  
  
 Ogni volta che questi attributi sono impostati sui valori predefiniti, il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client utilizza un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] set di risultati predefinito. I set di risultati predefiniti possono essere utilizzati per qualsiasi istruzione SQL supportata da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e rappresentano il metodo più efficiente per trasferire un intero set di risultati al client.  
  
 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] è stato introdotto il supporto per MARS (Multiple Active Result Sets); le applicazioni possono ora disporre di più set di risultati predefiniti attivi per connessione. Per impostazione predefinita, la funzionalità MARS non è abilitata.  
  
 Prima di [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], i set di risultati predefiniti non supportavano più istruzioni attive nella stessa connessione. Dopo l'esecuzione di un'istruzione SQL in una connessione, il server non accetta comandi dal client in tale connessione finché non sono state elaborate tutte le righe del set di risultati, ad eccezione di una richiesta per annullare il resto del set di risultati. Per annullare il resto di un set di risultati parzialmente elaborato, chiamare [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md) o [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md) con il parametro *fOption* impostato su SQL_CLOSE. Per completare un set di risultati parzialmente elaborato e verificare la presenza di un altro set di risultati, chiamare [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md). Se un'applicazione ODBC tenta di eseguire un comando su un handle di connessione prima che un set di risultati predefinito venga elaborato completamente, la chiamata genera SQL_ERROR e una chiamata a **SQLGetDiagRec** restituisce:  
  
```  
szSqlState: "HY000", pfNativeError: 0  
szErrorMsg: "[Microsoft][SQL Server Native Client]  
                Connection is busy with results for another hstmt."  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Modalità di implementazione dei cursori](../../../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
  
