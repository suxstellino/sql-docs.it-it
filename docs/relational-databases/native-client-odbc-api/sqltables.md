---
description: SQLTables
title: SQLTables | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLTables function
ms.assetid: 77b6c15c-9cf7-4019-b3f0-3d27d23ef656
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f5881dcbd92b136f50547655a02c749002c43acf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754791"
---
# <a name="sqltables"></a>SQLTables
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  SQLTables può essere eseguito su un cursore del server statico. Il tentativo di eseguire SQLTables su un cursore aggiornabile (dinamico o keyset) restituirà SQL_SUCCESS_WITH_INFO indicante che il tipo di cursore è stato modificato.  
  
 SQLTables riporta le tabelle di tutti i database quando il parametro *CatalogName* è SQL_ALL_CATALOGS e tutti gli altri parametri contengono valori predefiniti (puntatori null).  
  
 Per segnalare i cataloghi, gli schemi e i tipi di tabella disponibili, SQLTables utilizza in modo speciale le stringhe vuote (puntatori a byte di lunghezza zero). Le stringhe vuote non sono valori predefiniti (puntatori NULL).  
  
 The [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC driver supports reporting information for tables on linked servers by accepting a two-part name for the *CatalogName* parameter: *Linked_Server_Name.Catalog_Name*.  
  
 SQLTables restituisce informazioni su tutte le tabelle i cui nomi corrispondono a *TableName* e sono di proprietà dell'utente corrente.  
  
## <a name="sqltables-and-table-valued-parameters"></a>SQLTables e parametri con valori di tabella  
 Quando l'attributo dell'istruzione SQL_SOPT_SS_NAME_SCOPE ha il valore SQL_SS_NAME_SCOPE_TABLE_TYPE, anziché il valore predefinito di SQL_SS_NAME_SCOPE_TABLE, SQLTables restituisce informazioni sui tipi di tabella. Il valore TABLE_TYPE restituito per un tipo di tabella nella colonna 4 del set di risultati restituito da SQLTables è di tipo TABLE. Per ulteriori informazioni su SQL_SOPT_SS_NAME_SCOPE, vedere [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Tabelle, viste e sinonimi condividono uno spazio dei nomi comune che è distinto dallo spazio dei nomi utilizzato dai tipi di tabella. Anche se non è possibile avere una tabella e una vista con lo stesso nome, è possibile avere una tabella e un tipo di tabella con lo stesso nome nello stesso catalogo e schema.  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="example"></a>Esempio  
  
```  
// Get a list of all tables in the current database.  
SQLTables(hstmt, NULL, 0, NULL, 0, NULL, 0, NULL,0);  
  
// Get a list of all tables in all databases.  
SQLTables(hstmt, (SQLCHAR*) "%", SQL_NTS, NULL, 0, NULL, 0, NULL,0);  
  
// Get a list of databases on the current connection's server.  
SQLTables(hstmt, (SQLCHAR*) "%", SQL_NTS, (SQLCHAR*)"", 0, (SQLCHAR*)"",  
    0, NULL, 0);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [SQLTables (funzione)](../../odbc/reference/syntax/sqltables-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
