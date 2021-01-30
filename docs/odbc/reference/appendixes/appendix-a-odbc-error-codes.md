---
description: 'Appendice A: codici di errore ODBC'
title: 'Appendice A: codici di errore ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- error codes [ODBC]
- SQLSTATE [ODBC]
- error codes [ODBC], SQLSTATE
ms.assetid: c06902e4-721d-42e2-b818-05f0e18e4ce0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b37c846230cd60e067cf7ff2c8e7a72c74d1e540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212577"
---
# <a name="appendix-a-odbc-error-codes"></a>Appendice A: codici di errore ODBC
In questo argomento vengono illustrati i valori SQLSTATE per ODBC 3. *x*. Per ulteriori informazioni su ODBC 3. i valori SQLSTATE *x* , vedere la pagina relativa ai [mapping SQLSTATE](../../../odbc/reference/develop-app/sqlstate-mappings.md).  
  
 **SQLGetDiagRec** o **SQLGetDiagField** restituisce i valori SQLSTATE come definito da Open Group *Gestione dati: Structured Query Language (SQL), versione 2* (marzo 1995). I valori SQLSTATE sono stringhe che contengono cinque caratteri. Nella tabella seguente sono elencati i valori SQLSTATE che un driver può restituire per **SQLGetDiagRec**.  
  
 Il valore della stringa di caratteri restituito per un valore SQLSTATE è costituito da un valore di classe a due caratteri seguito da un valore di sottoclasse di tre caratteri. Il valore di una classe "01" indica un avviso ed è accompagnato da un codice restituito di SQL_SUCCESS_WITH_INFO. I valori della classe diversi da "01", ad eccezione della classe "IM", indicano un errore e sono accompagnati da un valore restituito di SQL_ERROR. La classe "IM" è specifica di avvisi ed errori che derivano dall'implementazione di ODBC. Il valore di sottoclasse "000" in qualsiasi classe indica che non è presente alcuna sottoclasse per tale SQLSTATE. L'assegnazione dei valori della classe e della sottoclasse è definita da SQL-92.  
  
> [!NOTE]  
>  Anche se l'esecuzione corretta di una funzione è in genere indicata da un valore restituito di SQL_SUCCESS, il valore SQLSTATE 00000 indica anche l'esito positivo.  
  
|SQLSTATE|Errore|Può essere restituito da|  
|--------------|-----------|--------------------------|  
|01000|Avviso generale|Tutte le funzioni ODBC eccetto:<br /><br /> **SQLError**<br /><br /> **SQLGetDiagField**<br /><br /> **SQLGetDiagRec**|  
|01001|Conflitto operazione cursore|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLSetPos**|  
|01002|Errore di disconnessione|**SQLDisconnect**|  
|01003|Valore NULL eliminato nella funzione set|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**|  
|01004|Dati stringa, troncati a destra|**SQLBrowseConnect**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColAttribute**<br /><br /> **SQLDataSources**<br /><br /> **SQLDescribeCol**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLDrivers**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetCursorName**<br /><br /> **SQLGetData**<br /><br /> **SQLGetDescField**<br /><br /> **SQLGetDescRec**<br /><br /> **SQLGetEnvAttr**<br /><br /> **SQLGetInfo**<br /><br /> **SQLGetStmtAttr**<br /><br /> **Sqlnative**<br /><br /> **SQLParamData SQL**<br /><br /> **SQLPutData**<br /><br /> **SQLSetCursorName**|  
|01006|Privilegio non revocato|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**|  
|01007|Privilegio non concesso|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**|  
|01S00|Attributo della stringa di connessione non valido|**SQLBrowseConnect**<br /><br /> **SQLDriverConnec**|  
|01S01|Errore nella riga|**SQLBulkOperations**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLSetPos**|  
|01S02|Valore di opzione modificato|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetStmtAttr**|  
|01S06|Tentativo di recupero prima che il set di risultati restituisse il primo set di righe|**SQLExtendedFetch**<br /><br /> **SQLFetchScroll**|  
|01S07|Troncamento frazionario|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLParamData**<br /><br /> **SQLSetPos**|  
|01S08|Errore durante il salvataggio del DSN del file|**SQLDriverConnect**|  
|01S09|Parola chiave non valida|**SQLDriverConnect**|  
|07001|Numero di parametri errato|**SQLExecDirect**<br /><br /> **SQLExecute**|  
|07002|Campo COUNT non corretto|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**|  
|07005|L'istruzione preparata non è una *specifica del cursore*|**SQLColAttribute**<br /><br /> **SQLDescribeCol**|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|**SQLBindCol**<br /><br /> **SQLBindParameter**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**<br /><br /> **SQLSetPos**|  
|07009|Indice del descrittore non valido|**SQLBindCol**<br /><br /> **SQLBindParameter**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColAttribute**<br /><br /> **SQLDescribeCol**<br /><br /> **SQLDescribeParam**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLGetDescField**<br /><br /> **SQLGetDescRec**<br /><br /> **SQLParamData**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetDescRecSQLSetPos**|  
|07S01|Uso non valido del parametro predefinito|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**|  
|08001|Il client non è in grado di stabilire la connessione|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|08002|Nome della connessione in uso|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLSetConnectAttr**|  
|08003|Connessione non aperta|**SQLAllocHandle**<br /><br /> **SQLDisconnect**<br /><br /> **SQLEndTran**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetInfo**<br /><br /> **SQLNativeSql**<br /><br /> **SQLSetConnectAttr**|  
|08004|Il server ha rifiutato la connessione|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|08007|Errore di connessione durante la transazione|**SQLEndTran**|  
|08S01|Errore collegamento comunicazione|**SQLBrowseConnect**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLConnect**<br /><br /> **SQLCopyDesc**<br /><br /> **SQLDescribeCol**<br /><br /> **SQLDescribeParam**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetData**<br /><br /> **SQLGetDescField**<br /><br /> **SQLGetDescRec**<br /><br /> **SQLGetFunctions**<br /><br /> **SQLGetInfo**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLMoreResults**<br /><br /> **SQLNativeSql**<br /><br /> **SQLNumParams**<br /><br /> **SQLNumResultCols**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLPutData**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetDescRec**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetStmtAttr**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|21S01|L'elenco di valori di inserimento non corrisponde all'elenco di colonne|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|21S02|Il grado della tabella derivata non corrisponde all'elenco di colonne|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLSetPos**|  
|22001|Dati stringa, troncati a destra|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetPos**|  
|22002|Variabile indicatore obbligatoria ma non fornita|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLParamData**|  
|22003|Valore numerico non compreso nell'intervallo|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLGetInfo**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**<br /><br /> **SQLSetPos**|  
|22007|Formato DateTime non valido|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**<br /><br /> **SQLSetPos**|  
|22008|Overflow del campo DateTime|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **QLParamData**<br /><br /> **SQLPutData**|  
|22012|Divisione per zero|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLPutData**|  
|22015|Overflow del campo Interval|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**<br /><br /> **SQLSetPos**|  
|22018|Valore di carattere non valido per la specifica del cast|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**<br /><br /> **SQLParamData**<br /><br /> **SQLPutData**<br /><br /> **SQLSetPos**|  
|22019|Carattere di escape non valido|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLPrepare**|  
|22025|Sequenza di escape non valida|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLPrepare**|  
|22026|Lunghezza dei dati non corrispondente.|**SQLParamData**|  
|23000|Violazione del vincolo di integrità|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLSetPos**|  
|24000|Stato del cursore non valido|**SQLBulkOperations**<br /><br /> **SQLCloseCursor**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetData**<br /><br /> **SQLGetStmtAttr**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLNativeSql**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetCursorName**<br /><br /> **SQLSetPos**<br /><br /> **SQLSetStmtAttr**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|25000|Stato della transazione non valido|**SQLDisconnect**|  
|25S01|Stato della transazione|**SQLEndTran**|  
|25S02|La transazione è ancora attiva|**SQLEndTran**|  
|25S03|Viene eseguito il rollback della transazione|**SQLEndTran**|  
|28000|Specifica di autorizzazione non valida|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|34000|Nome di cursore non valido|**SQLExecDirect**<br /><br /> **SQLPrepare**<br /><br /> **SQLSetCursorName**|  
|3C000|Nome cursore duplicato|**SQLSetCursorName**|  
|3D000|Nome catalogo non valido|**SQLExecDirect**<br /><br /> **SQLPrepare**<br /><br /> **SQLSetConnectAttr**|  
|3F000|Nome schema non valido|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|40001|Errore di serializzazione|**SQLBulkOperations**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLEndTran**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLMoreResults**<br /><br /> **SQLParamData**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLSetPos**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|40002|Violazione del vincolo di integrità|**SQLEndTran**|  
|40003|Completamento istruzione sconosciuto|**SQLBulkOperations**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLMoreResults**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLParamData**<br /><br /> **SQLSetPos**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|42000|Errore di sintassi o violazione di accesso|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLSetPos**|  
|42S01|La tabella o la vista di base esiste già|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|42S02|La tabella o la vista di base non è stata trovata|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|42S11|Indice già esistente|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|42S12|Indice non trovato|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|42S21|La colonna esiste già|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|42S22|Colonna non trovata|**SQLExecDirect**<br /><br /> **SQLPrepare**|  
|44000|Violazione della clausola WITH CHECK OPTION|**SQLBulkOperations**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLSetPos**|  
|HY000|Errore generale:|Tutte le funzioni ODBC eccetto:<br /><br /> **SQLError**<br /><br /> **SQLGetDiagField**<br /><br /> **SQLGetDiagRec**|  
|HY001|Errore di allocazione della memoria|Tutte le funzioni ODBC eccetto:<br /><br /> **SQLError**<br /><br /> **SQLGetDiagField**<br /><br /> **SQLGetDiagRec**|  
|HY003|Tipo di buffer dell'applicazione non valido|**SQLBindCol**<br /><br /> **SQLBindParameter**<br /><br /> **SQLGetData**|  
|HY004|Tipo di dati SQL non valido|**SQLBindParameter**<br /><br /> **SQLGetTypeInfo**|  
|HY007|L'istruzione associata non è preparata|**SQLCopyDesc**<br /><br /> **SQLGetDescField**<br /><br /> **SQLGetDescRec**|  
|HY008|Operation canceled|Tutte le funzioni ODBC che possono essere elaborate in modo asincrono:<br /><br /> **SQLBrowseConnect**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColAttribute**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLConnect**<br /><br /> **SQLDescribeCol**<br /><br /> **SQLDescribeParam**<br /><br /> **SQLDisconnect**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLEndTran**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetData**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLMoreResults**<br /><br /> **SQLNumParams**<br /><br /> **SQLNumResultCols**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLPutData**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetPos**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|HY009|Uso non valido del puntatore null|**SQLAllocHandle**<br /><br /> **SQLBindParameter**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLExecDirect**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetCursorName**<br /><br /> **SQLGetData**<br /><br /> **SQLGetFunctions**<br /><br /> **SQLNativeSql**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLPutData**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetCursorName**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetStmtAttr**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|HY010|Errore sequenza funzione|**SQLAllocHandle**<br /><br /> **SQLBindCol**<br /><br /> **SQLBindParameter**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLCloseCursor**<br /><br /> **SQLColAttribute**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLCopyDesc**<br /><br /> **SQLDescribeCol**<br /><br /> **SQLDescribeParam**<br /><br /> **SQLDisconnect**<br /><br /> **SQLEndTran**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLFreeHandle**<br /><br /> **SQLFreeStmt**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetCursorName**<br /><br /> **SQLGetData**<br /><br /> **SQLGetDescField**<br /><br /> **SQLGetDescRec**<br /><br /> **SQLGetFunctions**<br /><br /> **SQLGetStmtAttr**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLMoreResults**<br /><br /> **SQLNumParams**<br /><br /> **SQLNumResultCols**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLPutData**<br /><br /> **SQLRowCount**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetCursorName**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetDescRec**<br /><br /> **SQLSetPos**<br /><br /> **SQLSetStmtAttr**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|HY011|Non è possibile impostare l'attributo adesso|**SQLBulkOperations**<br /><br /> **SQLParamData**<br /><br /> **QLSetPos**<br /><br /> **SQLSetStmtAttr**|  
|HY012|Codice operazione transazione non valido|**SQLEndTran**|  
|HY013|Errore di gestione della memoria|Tutte le funzioni ODBC eccetto:<br /><br /> **SQLGetDiagField**<br /><br /> **SQLGetDiagRec**|  
|HY014|È stato superato il limite per il numero di handle|**SQLAllocHandle**|  
|HY015|Nessun nome di cursore disponibile|**SQLGetCursorName**|  
|HY016|Impossibile modificare il descrittore di una riga di implementazione|**SQLCopyDesc**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetDescRec**|  
|HY017|Utilizzo non valido di un handle descrittore allocato automaticamente|**SQLFreeHandle**<br /><br /> **SQLSetStmtAttr**|  
|HY018|Il server ha rifiutato la richiesta di annullamento|**SQLCancel**|  
|HY019|Dati non di tipo carattere e non binario inviati in parti|**SQLPutData**|  
|HY020|Tentativo di concatenare un valore null|**SQLPutData**|  
|HY021|Informazioni descrittore incoerenti|**SQLBindParameter**<br /><br /> **SQLCopyDesc**<br /><br /> **SQLGetDescField**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetDescRec**|  
|HY024|Valore di attributo non valido|**SQLSetConnectAttr**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetStmtAttr**|  
|HY090|Lunghezza della stringa o del buffer non valida|**SQLBindCol**<br /><br /> **SQLBindParameter**<br /><br /> **SQLBrowseConnect**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColAttribute**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLConnect**<br /><br /> **SQLDataSources**<br /><br /> **SQLDescribeCol**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLDrivers**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetCursorName**<br /><br /> **SQLGetData**<br /><br /> **SQLGetDescField**<br /><br /> **SQLGetInfo**<br /><br /> **SQLGetStmtAttr**<br /><br /> **SQLNativeSql**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLPutData**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetCursorName**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetDescRec**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetStmtAttr**<br /><br /> **SQLSetPos**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|HY091|Identificatore del campo del descrittore non valido|**SQLColAttribute**<br /><br /> **SQLGetDescField**<br /><br /> **SQLSetDescField**|  
|HY092|Identificatore di attributo/opzione non valido|**SQLAllocHandle**<br /><br /> **QLBulkOperations**<br /><br /> **SQLCopyDesc**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLEndTran**<br /><br /> **SQLFreeStmt**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetEnvAttr**<br /><br /> **QLParamData**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetDescField**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetPos**<br /><br /> **SQLSetStmtAttr**|  
|HY095|Tipo di funzione non compreso nell'intervallo|**SQLGetFunctions**|  
|HY096|Tipo di informazioni non valido|**SQLGetInfo**|  
|HY097|Tipo di colonna non compreso nell'intervallo|**SQLSpecialColumns**|  
|HY098|Tipo di ambito non compreso nell'intervallo|**SQLSpecialColumns**|  
|HY099|Tipo nullable non compreso nell'intervallo|**SQLSpecialColumns**|  
|HY100|Tipo di opzione di univocità non compreso nell'intervallo|**SQLStatistics**|  
|HY101|Tipo di opzione di accuratezza non compreso nell'intervallo|**SQLStatistics**|  
|HY103|Codice di recupero non valido|**SQLDataSources**<br /><br /> **SQLDrivers**|  
|HY104|Valore di precisione o di scala non valido|**SQLBindParameter**|  
|HY105|Tipo di parametro non valido|**SQLBindParameter**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLParamData**<br /><br /> **SQLSetDescField**|  
|HY106|Tipo di recupero non compreso nell'intervallo|**SQLExtendedFetch**<br /><br /> **SQLFetchScroll**|  
|HY107|Valore di riga non compreso nell'intervallo|**SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLSetPos**|  
|HY109|Posizione del cursore non valida|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLGetData**<br /><br /> **SQLGetStmtAttr**<br /><br /> **SQLNativeSql**<br /><br /> **SQLParamData**<br /><br /> **SQLSetPos**|  
|HY110|Completamento driver non valido|**SQLDriverConnect**|  
|HY111|Valore segnalibro non valido|**SQLExtendedFetch**<br /><br /> **SQLFetchScroll**|  
|HYC00|Funzionalità facoltativa non implementata|**SQLBindCol**<br /><br /> **SQLBindParameter**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColAttribute**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLEndTran**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetConnectAttr**<br /><br /> **SQLGetData**<br /><br /> **SQLGetEnvAttr**<br /><br /> **SQLGetInfo**<br /><br /> **SQLGetStmtAttr**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLSetConnectAttr**<br /><br /> **SQLSetEnvAttr**<br /><br /> **SQLSetPos**<br /><br /> **SQLSetStmtAttr**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|HYT00|Timeout|**SQLBrowseConnect**<br /><br /> **SQLBulkOperations**<br /><br /> **SQLColumnPrivileges**<br /><br /> **SQLColumns**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLExtendedFetch**<br /><br /> **SQLForeignKeys**<br /><br /> **SQLGetTypeInfo**<br /><br /> **SQLParamData**<br /><br /> **SQLPrepare**<br /><br /> **SQLPrimaryKeys**<br /><br /> **SQLProcedureColumns**<br /><br /> **SQLProcedures**<br /><br /> **SQLSetPos**<br /><br /> **SQLSpecialColumns**<br /><br /> **SQLStatistics**<br /><br /> **SQLTablePrivileges**<br /><br /> **SQLTables**|  
|HYT01|Timeout connessione scaduto|Tutte le funzioni ODBC eccetto:<br /><br /> **SQLDrivers**<br /><br /> **SQLDataSources**<br /><br /> **SQLGetEnvAttr**<br /><br /> **SQLSetEnvAttr**|  
|IM001|Il driver non supporta questa funzione|Tutte le funzioni ODBC eccetto:<br /><br /> **SQLAllocHandle**<br /><br /> **SQLDataSources**<br /><br /> **SQLDrivers**<br /><br /> **SQLFreeHandle**<br /><br /> **SQLGetFunctions**|  
|IM002|Il nome dell'origine dati non è stato trovato e non è stato specificato alcun driver predefinito|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|IM003|Non è stato possibile caricare il driver specificato|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|IM004|**SQLAllocHandle** del Driver su SQL_HANDLE_ENV non riuscito|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|IM005|**SQLAllocHandle** del Driver su SQL_HANDLE_DBC non riuscito|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|IM006|Errore di **SQLSetConnectAttr** del driver|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|IM007|Non è stata specificata alcuna origine dati o driver; dialogo non consentito|**SQLDriverConnect**|  
|IM008|Finestra di dialogo non riuscita|**SQLDriverConnect**|  
|IM009|Impossibile caricare la DLL di traduzione|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**<br /><br /> **SQLSetConnectAttr**|  
|IM010|Nome origine dati troppo lungo|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|IM011|Nome driver troppo lungo|**SQLBrowseConnect**<br /><br /> **SQLDriverConnect**|  
|IM012|Errore di sintassi della parola chiave DRIVER|**SQLBrowseConnect**<br /><br /> **SQLDriverConnect**|  
|IM013|Errore file di traccia|Tutte le funzioni ODBC.|  
|IM014|Nome del DSN del file non valido|**SQLDriverConnect**|  
|IM015|Origine dati file danneggiati|**SQLDriverConnect**|
