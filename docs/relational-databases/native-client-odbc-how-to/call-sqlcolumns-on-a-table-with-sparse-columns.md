---
description: Chiamare SQLColumns su un tabella con colonne di tipo sparse
title: Chiamare SQLColumns in una tabella con colonne di tipo sparse | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
yms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: afd35e13-2370-43c2-9cbc-f8da6248c39c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3573968629cddc206e59d5f8d769438e328a0dac
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748266"
---
# <a name="call-sqlcolumns-on-a-table-with-sparse-columns"></a>Chiamare SQLColumns su un tabella con colonne di tipo sparse
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo esempio viene illustrato come chiamare SQLColumns in una tabella con colonne di tipo sparse definite tramite ODBC in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 Non è possibile utilizzare questo esempio con le versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 Per ulteriori informazioni sulla funzionalità colonne di tipo sparse, vedere [supporto di colonne di tipo sparse in SQL Server Native Client](../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md).  
  
## <a name="example"></a>Esempio  
 Il primo listato di codice rappresenta il codice sorgente C++. Modificare "MyServer" impostando un nome di server valido. Verificare che nella variabile di ambiente INCLUDE sia presente la directory che contiene sqlncli.h. Se questo esempio viene compilato ed eseguito come applicazione a 32 bit in un sistema operativo a 64 bit, è necessario creare l'origine dati ODBC con Amministratore ODBC in %windir%\SysWOW64\odbcad32.exe.  
  
 In questo esempio viene eseguita la connessione all'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer in uso. Per connettersi a un'istanza denominata, modificare la definizione dell'origine dati ODBC per specificare l'istanza in base al formato: server\istanzadenominata. Per impostazione predefinita, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] viene installato in un'istanza denominata.  
  
 Eseguire la compilazione con /EHsc /D, "UNICODE" e odbc32.lib.  
  
 Il secondo [!INCLUDE[tsql](../../includes/tsql-md.md)] Listato di codice () consente di eliminare la tabella creata dall'esempio.  
  
```  
// compile with: /EHsc /D "UNICODE" odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
#include <sqlext.h>  
  
#include <sqlncli.h>  
  
#define SUCCESS(x) (!((x) & 0xFFFE))  
  
#define CHKRC(stmt) rc = (stmt); \  
   if (!SUCCESS(rc)) \  
   throw (RETCODE) rc;  
  
void PrintError(SQLSMALLINT HandleType, SQLHANDLE Handle) {  
   RETCODE rc = SQL_SUCCESS;  
   SQLTCHAR szSqlState[6], szMessage[1024];  
   SQLSMALLINT i = 1, msgLen = 0;  
   SQLINTEGER NativeError;  
  
   do {  
      i = 1;  
      while (SQL_NO_DATA != (rc = SQLGetDiagRec(HandleType, Handle, i, szSqlState, &NativeError,   
         szMessage, sizeof(szMessage)/sizeof(SQLTCHAR), &msgLen)) && SUCCESS(rc)) {  
            wprintf(L"SQLState=%s, NativeError=%ld, Message=%s\r\n", szSqlState, NativeError, szMessage);  
            i++;  
      }  
   }   
   while (SQL_NO_DATA != (rc = SQLMoreResults(Handle)) && SUCCESS(rc));  
}  
  
#define STR_LEN 128 + 1  
#define REM_LEN 254 + 1  
  
void ProcessSQLColumnsResult(SQLHSTMT hstmt) {  
   SQLCHAR szSchema[STR_LEN];  
   SQLCHAR szCatalog[STR_LEN];  
   SQLCHAR szColumnName[STR_LEN];  
   SQLCHAR szTableName[STR_LEN];  
   SQLCHAR szTypeName[STR_LEN];  
   SQLCHAR szRemarks[REM_LEN];  
   SQLCHAR szColumnDefault[STR_LEN];  
   SQLCHAR szIsNullable[STR_LEN];  
  
   SQLINTEGER ColumnSize;  
   SQLINTEGER BufferLength;  
   SQLINTEGER CharOctetLength;  
   SQLINTEGER OrdinalPosition;  
  
   SQLSMALLINT DataType;  
   SQLSMALLINT DecimalDigits;  
   SQLSMALLINT NumPrecRadix;  
   SQLSMALLINT Nullable;  
   SQLSMALLINT SQLDataType;  
   SQLSMALLINT DatetimeSubtypeCode;  
   SQLLEN cbCatalog;  
   SQLLEN cbSchema;  
   SQLLEN cbTableName;  
   SQLLEN cbColumnName;  
   SQLLEN cbDataType;  
   SQLLEN cbTypeName;  
   SQLLEN cbColumnSize;  
   SQLLEN cbBufferLength;  
   SQLLEN cbDecimalDigits;  
   SQLLEN cbNumPrecRadix;  
   SQLLEN cbNullable;  
   SQLLEN cbRemarks;  
   SQLLEN cbColumnDefault;  
   SQLLEN cbSQLDataType;  
   SQLLEN cbDatetimeSubtypeCode;  
   SQLLEN cbCharOctetLength;  
   SQLLEN cbOrdinalPosition;  
   SQLLEN cbIsNullable;  
  
   SQLRETURN rc = SQL_SUCCESS;  
  
   CHKRC(SQLColumns(hstmt, L"tempdb", SQL_NTS, L"dbo", SQL_NTS, L"tbl_sparse_test", SQL_NTS, NULL, 0 ));  
  
   // Bind columns in result set to buffers  
   SQLBindCol(hstmt, 1,  SQL_C_CHAR,   szCatalog,              STR_LEN,    &cbCatalog);  
   SQLBindCol(hstmt, 2,  SQL_C_CHAR,   szSchema,               STR_LEN,    &cbSchema);  
   SQLBindCol(hstmt, 3,  SQL_C_CHAR,   szTableName,            STR_LEN,    &cbTableName);  
   SQLBindCol(hstmt, 4,  SQL_C_CHAR,   szColumnName,           STR_LEN,    &cbColumnName);  
   SQLBindCol(hstmt, 5,  SQL_C_SSHORT, &DataType,              0,          &cbDataType);  
   SQLBindCol(hstmt, 6,  SQL_C_CHAR,   szTypeName,             STR_LEN,    &cbTypeName);  
   SQLBindCol(hstmt, 7,  SQL_C_SLONG,  &ColumnSize,            0,          &cbColumnSize);  
   SQLBindCol(hstmt, 8,  SQL_C_SLONG,  &BufferLength,          0,          &cbBufferLength);  
   SQLBindCol(hstmt, 9,  SQL_C_SSHORT, &DecimalDigits,         0,          &cbDecimalDigits);  
   SQLBindCol(hstmt, 10, SQL_C_SSHORT, &NumPrecRadix,          0,          &cbNumPrecRadix);  
   SQLBindCol(hstmt, 11, SQL_C_SSHORT, &Nullable,              0,          &cbNullable);  
   SQLBindCol(hstmt, 12, SQL_C_CHAR,   szRemarks,              REM_LEN,    &cbRemarks);  
   SQLBindCol(hstmt, 13, SQL_C_CHAR,   szColumnDefault,        STR_LEN,    &cbColumnDefault);  
   SQLBindCol(hstmt, 14, SQL_C_SSHORT, &SQLDataType,           0,          &cbSQLDataType);  
   SQLBindCol(hstmt, 15, SQL_C_SSHORT, &DatetimeSubtypeCode,   0,          &cbDatetimeSubtypeCode);  
   SQLBindCol(hstmt, 16, SQL_C_SLONG,  &CharOctetLength,       0,          &cbCharOctetLength);  
   SQLBindCol(hstmt, 17, SQL_C_SLONG,  &OrdinalPosition,       0,          &cbOrdinalPosition);  
   SQLBindCol(hstmt, 18, SQL_C_CHAR,   szIsNullable,           STR_LEN,    &cbIsNullable);  
  
   try {  
      while (SQL_SUCCESS == rc) {  
         CHKRC(SQLFetch(hstmt));  
         wprintf(L"Column name: %hs\tIsNullable: %hs\tType: %hs\n", szColumnName, szIsNullable, szTypeName);  
      }  
   }  
   catch (RETCODE retcode) {  
      if (SQL_NO_DATA != retcode)  
         throw retcode;  
   }  
   SQLFreeStmt(hstmt, SQL_CLOSE);  
}  
  
int main() {  
   RETCODE rc = SQL_SUCCESS;  
   HENV henv = SQL_NULL_HENV;  
   HDBC hdbc = SQL_NULL_HDBC;  
   SQLHSTMT hstmt = SQL_NULL_HSTMT;  
   SQLTCHAR * pszConnection = L"DRIVER={SQL Server Native Client 10.0}; Server=MyServer; Trusted_Connection=Yes;";  
  
   try {  
      CHKRC(SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HENV, &henv));  
      CHKRC(SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0));  
      CHKRC(SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc));  
      CHKRC(SQLDriverConnect( hdbc, NULL, pszConnection, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT));  
      CHKRC(SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt));  
      CHKRC(SQLExecDirect(hstmt,  
         L"if object_id('tempdb.dbo.tbl_sparse_test','U') is not null drop table tempdb.dbo.tbl_sparse_test",  
         SQL_NTS));  
  
      // Create a new table  
      CHKRC(SQLExecDirect(hstmt,  
         L"create table tempdb.dbo.tbl_sparse_test (col1 int SPARSE, col2 int, col3 XML column_set for all_sparse_columns)",  
         SQL_NTS));  
  
      // Insert a row into the table  
      CHKRC(SQLExecDirect(hstmt,  
         L"insert tempdb.dbo.tbl_sparse_test (col1, col2) values (1,2)",  
         SQL_NTS));  
  
      wprintf(L"Checking default SQLColumns behavior.\nYou should not see the first sparse column.\n");  
  
      ProcessSQLColumnsResult(hstmt);  
  
      wprintf(L"\nChecking SQLColumns with the statement attribute SQL_SS_NAME_SCOPE_EXTENDED.\nYou should see all the columns\n");  
      CHKRC(SQLSetStmtAttr(hstmt, SQL_SOPT_SS_NAME_SCOPE, (SQLPOINTER)SQL_SS_NAME_SCOPE_EXTENDED, SQL_IS_SMALLINT));  
  
      ProcessSQLColumnsResult(hstmt);  
  
      wprintf(L"\nChecking SQLColumns with the statement attribute SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET.\nYou should see only the sparse columns\n");  
      CHKRC(SQLSetStmtAttr(hstmt, SQL_SOPT_SS_NAME_SCOPE, (SQLPOINTER)SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET, SQL_IS_SMALLINT));  
  
      ProcessSQLColumnsResult(hstmt);  
  
   }  
   catch (RETCODE retcode) {  
      rc = retcode;  
   }  
  
   if (!SUCCESS(rc)) {  
      if (hstmt)  
         PrintError(SQL_HANDLE_STMT, hstmt);  
      else if (hdbc)  
         PrintError(SQL_HANDLE_DBC, hdbc);  
      else if(henv)  
         PrintError(SQL_HANDLE_ENV, henv);  
   }  
  
   if (hstmt)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
   if (hdbc) {  
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
   }  
   if (henv)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
```  
use tempdb  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'tbl_sparse_test')  
     DROP TABLE tbl_sparse_test  
GO  
```  
  
  
