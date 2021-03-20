---
description: Gestione di colonne di tipo text e image - Usare colonne data-at-execution
title: Utilizzare le colonne data-at-execution (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data-at-execution
ms.assetid: 4eae58d1-03d4-40ca-8aa1-9b3ea10a38cf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fcbd0d9305e30f84ed369a63218119594bc62cee
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750181"
---
# <a name="managing-text-and-image-columns---use-data-at-execution-columns"></a>Gestione di colonne di tipo text e image - Usare colonne data-at-execution
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-use-data-at-execution-text-ntext-or-image-columns"></a>Per utilizzare le colonne data-at-execution di tipo text, ntext o image  
  
1.  Per ogni colonna data-at-execution, inserire valori speciali nei buffer precedentemente associati da [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md):  
  
    -   Per l'ultimo parametro, utilizzare SQL_LEN_DATA_AT_EXEC(length), dove length è la lunghezza totale, espressa in byte, dei dati della colonna di tipo text, ntext o image.  
  
    -   Per il quarto parametro, inserire un identificatore della colonna definito dal programma.  
  
2.  Se si chiama [SQLSetPos](../../odbc/reference/syntax/sqlsetpos-function.md), viene restituito SQL_NEED_DATA, che indica che le colonne data-at-execution sono pronte per l'elaborazione.  
  
3.  Per ogni colonna data-at-execution:  
  
    -   Chiamare [SQLParamData](../../odbc/reference/syntax/sqlparamdata-function.md) per ottenere il puntatore di una matrice di colonna. Verrà restituito SQL_NEED_DATA se è presente un'altra colonna data-at-execution.  
  
    -   Chiamare [SQLPutData](../../relational-databases/native-client-odbc-api/sqlputdata.md) una o più volte per inviare i dati della colonna, fino quando non viene inviata la lunghezza.  
  
4.  Chiamare [SQLParamData](../../odbc/reference/syntax/sqlparamdata-function.md) per indicare che tutti i dati relativi alla colonna data-at-execution finale devono essere inviati. Non verrà restituito SQL_NEED_DATA.  

## <a name="example"></a>Esempio  
 In questo esempio viene illustrato come leggere dati di tipo carattere variabili SQL_LONG mediante SQLGetData. Questo esempio non è supportato in IA64.  
  
 È necessaria un'origine dati ODBC denominata AdventureWorks, il cui database predefinito è il database di esempio AdventureWorks. È possibile scaricare il database di esempio AdventureWorks dal [Microsoft SQL Server esempi e progetti della Community](https://go.microsoft.com/fwlink/?LinkID=85384) Home page. Questa origine dati deve essere basata sul driver ODBC fornito dal sistema operativo (il nome del driver è "SQL Server"). Se questo esempio viene compilato ed eseguito come applicazione a 32 bit in un sistema operativo a 64 bit, è necessario creare l'origine dati ODBC con Amministratore ODBC in %windir%\SysWOW64\odbcad32.exe.  
  
 In questo esempio viene eseguita la connessione all'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer in uso. Per connettersi a un'istanza denominata, modificare la definizione dell'origine dati ODBC per specificare l'istanza in base al formato: server\istanzadenominata. Per impostazione predefinita, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] viene installato in un'istanza denominata.  
  
 Eseguire il primo [!INCLUDE[tsql](../../includes/tsql-md.md)] Listato di codice () per creare la tabella utilizzata dall'esempio.  
  
 Compilare il secondo listato di codice (C++) con odbc32.lib. Eseguire quindi il programma.  
  
 Eseguire il terzo [!INCLUDE[tsql](../../includes/tsql-md.md)] Listato di codice () per eliminare la tabella utilizzata dall'esempio.  
  
```  
use AdventureWorks  
CREATE TABLE emp3 (NAME char(30), AGE int, BIRTHDAY datetime, Memo1 text)  
INSERT INTO emp3 (NAME, AGE, Memo1) VALUES   ('Name1', '12', 'This is the first employee')  
INSERT INTO emp3 (NAME, AGE, Memo1) VALUES   ('Name2', '18', 'This is the second employee')  
```  
  
```  
// compile with: odbc32.lib  
#include <stdio.h>  
#include <string.h>  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define BUFFERSIZE  450  
  
SQLHENV henv = SQL_NULL_HENV;  
SQLHDBC hdbc1 = SQL_NULL_HDBC;  
SQLHSTMT hstmt1 = SQL_NULL_HSTMT;  
  
void Cleanup() {  
   if (hstmt1 != SQL_NULL_HSTMT)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
};  
  
int main() {  
   RETCODE retcode;  
   SWORD cntr;  
  
   // SQLGetData variables.  
   UCHAR Data[BUFFERSIZE];  
   SDWORD cbBatch = (SDWORD)sizeof(Data)-1;  
   SQLLEN cbTxtSize;  
  
   // Clear data array.  
   for (cntr = 0 ; cntr < BUFFERSIZE ; cntr++)  
      Data[cntr] = 0x00;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle and connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"AdventureWorks", SQL_NTS, (UCHAR*)"", SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Allocate statement handle; prepare, then execute command.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLAllocHandle(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   retcode = SQLExecDirect(hstmt1, (UCHAR*)"SELECT Memo1 FROM emp3", SQL_NTS);  
   if( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLExecDirect Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Get first row.  
   retcode = SQLFetch(hstmt1);  
   if( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLFetch(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Get the SQL_LONG column.  
   cntr = 1;  
   while ( (retcode = SQLGetData(hstmt1, 1, SQL_C_CHAR, Data, cbBatch, &cbTxtSize)) != SQL_NO_DATA) {  
      printf("GetData iteration %d, pcbValue = %d,\n", cntr++, cbTxtSize);  
      printf("Data = %s\n\n", Data);  
  
      if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
         printf("GetData(hstmt1) Failed\n\n");  
         Cleanup();  
         return(9);  
      }  
   }   
  
   // Clean up  
   //SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
```  
use AdventureWorks  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'emp3')  
     DROP TABLE emp3  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure per la gestione di colonne di testo e di immagini &#40;ODBC&#41;](./odbc-how-to-topics.md)  
  
