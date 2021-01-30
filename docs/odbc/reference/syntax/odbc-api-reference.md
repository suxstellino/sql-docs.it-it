---
description: Informazioni di riferimento sull'API ODBC
title: Informazioni di riferimento sulle API ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: dllExport
ms.assetid: b7a49774-f458-44ce-9a04-a0457501405b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60dab6c50856d9f4132b9ec3d066d15c99194814
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174662"
---
# <a name="odbc-api-reference"></a>Informazioni di riferimento sull'API ODBC
Negli argomenti di questa sezione viene descritta ogni funzione ODBC in ordine alfabetico. Ogni funzione è definita come funzione del linguaggio di programmazione C. Sono incluse le descrizioni seguenti:  
  
-   Scopo  
  
-   Versione ODBC  
  
-   Livello di conformità dell'interfaccia della riga di comando standard  
  
-   Sintassi  
  
-   Argomenti  
  
-   Valori restituiti  
  
-   Diagnostica  
  
-   Commenti sull'utilizzo e sull'implementazione  
  
-   Esempio di codice  
  
-   Riferimenti a funzioni correlate  
  
 Il livello di conformità standard dell'interfaccia della riga di comando può essere uno dei seguenti: ISO 92, gruppo aperto, ODBC o deprecato. Una funzione contrassegnata come ISO 92 è presente anche in Open Group versione 1, perché Open Group è un superset puro di ISO 92. Una funzione contrassegnata come conforme a gruppi aperti viene visualizzata anche in ODBC 3. *x*, perché ODBC 3. *x* è un superset puro della versione 1 del gruppo aperto. Una funzione contrassegnata come conforme a ODBC non viene visualizzata in nessuno standard. Una funzione contrassegnata come deprecata è stata deprecata in ODBC 3. *x*.  
  
 La gestione delle informazioni di diagnostica è descritta nella descrizione della funzione [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) . Il testo associato ai valori SQLSTATE è incluso per fornire una descrizione della condizione, ma non è destinato a prescrivere testo specifico.  
  
> [!NOTE]  
>  Per informazioni specifiche del driver sulle funzioni ODBC, vedere la sezione relativa al driver.  
  
 Questa sezione contiene argomenti per le funzioni seguenti:  
  
-   [Funzione SQLAllocConnect](../../../odbc/reference/syntax/sqlallocconnect-function.md)  
  
-   [Funzione SQLAllocEnv](../../../odbc/reference/syntax/sqlallocenv-function.md)  
  
-   [Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)  
  
-   [Funzione SQLAllocStmt](../../../odbc/reference/syntax/sqlallocstmt-function.md)  
  
-   [Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)  
  
-   [Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)  
  
-   [Funzione SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)  
  
-   [Funzione SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)  
  
-   [Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)  
  
-   [Funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)  
  
-   [Funzione SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)  
  
-   [Funzione SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)  
  
-   [Funzione SQLColAttributes](../../../odbc/reference/syntax/sqlcolattributes-function.md)  
  
-   [SQLColumnPrivileges Function](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)  
  
-   [Funzione SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)  
  
-   [Funzione SQLCompleteAsync](../../../odbc/reference/syntax/sqlcompleteasync-function.md)  
  
-   [Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)  
  
-   [Funzione SQLCopyDesc](../../../odbc/reference/syntax/sqlcopydesc-function.md)  
  
-   [Funzione SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)  
  
-   [Funzione SQLDescribeCol ](../../../odbc/reference/syntax/sqldescribecol-function.md)  
  
-   [Funzione SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)  
  
-   [Funzione SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)  
  
-   [Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)  
  
-   [Funzione SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)  
  
-   [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md)  
  
-   [Funzione SQLError](../../../odbc/reference/syntax/sqlerror-function.md)  
  
-   [Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)  
  
-   [Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)  
  
-   [Funzione SQLExtendedFetch](../../../odbc/reference/syntax/sqlextendedfetch-function.md)  
  
-   [Funzione SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)  
  
-   [Funzione SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)  
  
-   [Funzione SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)  
  
-   [Funzione SQLFreeConnect](../../../odbc/reference/syntax/sqlfreeconnect-function.md)  
  
-   [Funzione SQLFreeEnv](../../../odbc/reference/syntax/sqlfreeenv-function.md)  
  
-   [SQLFreeHandle Function](../../../odbc/reference/syntax/sqlfreehandle-function.md)  
  
-   [SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md)  
  
-   [Funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)  
  
-   [Funzione SQLGetConnectOption](../../../odbc/reference/syntax/sqlgetconnectoption-function.md)  
  
-   [Funzione SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)  
  
-   [Funzione SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)  
  
-   [Funzione SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)  
  
-   [Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)  
  
-   [Funzione SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)  
  
-   [Funzione SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)  
  
-   [Funzione SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)  
  
-   [SQLGetFunctions Function](../../../odbc/reference/syntax/sqlgetfunctions-function.md)  
  
-   [Funzione SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)  
  
-   [Funzione SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)  
  
-   [Funzione SQLGetStmtOption](../../../odbc/reference/syntax/sqlgetstmtoption-function.md)  
  
-   [Funzione SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)  
  
-   [Funzione SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)  
  
-   [Funzione SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)  
  
-   [Funzione SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)  
  
-   [Funzione SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)  
  
-   [Funzione SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)  
  
-   [Funzione SQLParamOptions](../../../odbc/reference/syntax/sqlparamoptions-function.md)  
  
-   [Pagina relativa alla funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)  
  
-   [SQLPrimaryKeys Function](../../../odbc/reference/syntax/sqlprimarykeys-function.md)  
  
-   [Funzione SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)  
  
-   [Funzione SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)  
  
-   [SQLPutData Function](../../../odbc/reference/syntax/sqlputdata-function.md)  
  
-   [SQLRowCount Function](../../../odbc/reference/syntax/sqlrowcount-function.md)  
  
-   [Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)  
  
-   [Funzione SQLSetConnectOption](../../../odbc/reference/syntax/sqlsetconnectoption-function.md)  
  
-   [Funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)  
  
-   [Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)  
  
-   [Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)  
  
-   [SQLSetEnvAttr Function](../../../odbc/reference/syntax/sqlsetenvattr-function.md)  
  
-   [Funzione SQLSetParam](../../../odbc/reference/syntax/sqlsetparam-function.md)  
  
-   [Funzione SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)  
  
-   [Funzione SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)  
  
-   [Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)  
  
-   [Funzione SQLSetStmtOption](../../../odbc/reference/syntax/sqlsetstmtoption-function.md)  
  
-   [Funzione SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)  
  
-   [Funzione SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)  
  
-   [Funzione SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)  
  
-   [Funzione SQLTables](../../../odbc/reference/syntax/sqltables-function.md)  
  
-   [Funzione SQLTransact](../../../odbc/reference/syntax/sqltransact-function.md)
