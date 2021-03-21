---
description: SQLFreeStmt
title: SQLFreeStmt | Microsoft Docs
ms.custom: ''
ms.date: 11/23/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLFreeStmt function
ms.assetid: d9666d0b-3446-480e-bf1a-10b01213e411
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92cf1996fe7a4079307e448a80cb4d0f22f5227e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753291"
---
# <a name="sqlfreestmt"></a>SQLFreeStmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Generalmente   
      **SQLFreeStmt** non è consigliato in ODBC 3,0 e versioni successive. Tuttavia, se l'applicazione deve riutilizzare l'istruzione è comunque necessario utilizzare **SQLFreeStmt** con le opzioni **SQL_RESET_PARAMS** e **SQL_UNBIND** ). È anche possibile usare [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md), [SQLSetDescField](../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)e [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) per sostituire o duplicare la funzione di **SQLFreeStmt** e usarla in alternativa.  
  
 In generale, è più efficiente riutilizzare le istruzioni anziché eliminarle e allocarne di nuove. Tuttavia, in alcune situazioni, come il riutilizzo delle istruzioni, è ancora necessario usare SQLFreeStmt.  
  
## <a name="see-also"></a>Vedere anche  
 [SQLFreeStmt (funzione)](../../odbc/reference/syntax/sqlfreestmt-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
