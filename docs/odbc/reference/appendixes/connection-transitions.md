---
description: Transizioni di connessione
title: Transizioni di connessione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60a26505af6d53265111bd24b08990e61b992902
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165421"
---
# <a name="connection-transitions"></a>Transizioni di connessione
Le connessioni ODBC hanno gli Stati seguenti.  
  
|State|Descrizione|  
|-----------|-----------------|  
|C0|Ambiente non allocato, connessione non allocata|  
|C1|Ambiente allocato, connessione non allocata|  
|C2|Ambiente allocato, connessione allocata|  
|C3|La funzione di connessione necessita di dati|  
|C4|Connessione connessa|  
|C5|Connessione connessa, istruzione allocata|  
|C6|Connessione connessa, transazione in corso. È possibile che una connessione si trovi nello stato C6 senza alcuna istruzione allocata per la connessione. Si supponga, ad esempio, che la connessione sia in modalità di commit manuale e si trovi nello stato C4. Se un'istruzione viene allocata, eseguita (avviando una transazione) e quindi liberata, la transazione rimane attiva ma non sono presenti istruzioni per la connessione.|  
  
 Nelle tabelle seguenti viene illustrato il modo in cui ogni funzione ODBC influiscono sullo stato della connessione.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> Nessun ENV.|C1 non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|IH 2|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
|IH 3|IH|(08003)|(08003)|C5|--[5]|--[5]|  
|IH 4|IH|(08003)|(08003)|--[5]|--[5]|--[5]|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DBC.  
  
 [3] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [4] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC.  
  
 [5] la chiamata a **SQLAllocHandle** con *OutputHandlePtr* che punta a un handle valido sovrascrive tale handle senza considerare il contenuto precedente diquesto handle e potrebbe causare problemi per i driver ODBC. La programmazione di applicazioni ODBC non è corretta per chiamare **SQLAllocHandle** due volte con la stessa variabile dell'applicazione definita per *\* OutputHandlePtr* senza chiamare **SQLFreeHandle** per liberare l'handle prima della riallocazione. La sovrascrittura degli handle ODBC in questo modo può causare un comportamento incoerente o errori della parte dei driver ODBC.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|C3 [d] C4 [s]|--[d] C2 [e] C4 [s]|(08002)|(08002)|(08002)|  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--|--[1] C5 [2]|  
  
 [1] la connessione era in modalità di commit manuale.  
  
 [2] la connessione era in modalità autocommit.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges e SQLTables  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2]|--|  
  
 [1] la connessione era in modalità autocommit oppure l'origine dati non ha iniziato una transazione.  
  
 [2] la connessione era in modalità di commit manuale e l'origine dati ha avviato una transazione.  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|C4|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc, SQLGetDescField, SQLGetDescRec, SQLSetDescField e SQLSetDescRec  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|--[1]|--|--|  
  
 [1] in questo stato, gli unici descrittori disponibili per l'applicazione sono descrittori allocati in modo esplicito.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources e SQLDrivers  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|(08003)|C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|C4 s--n [f]|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1|--[3]|--[3]|--[3]|--|--|--[4] o ([5], [6] e [8]) C4 [5] e [7] C5 [5], [6] e [9]|  
|IH 2|IH|(08003)|(08003)|--|--|C5|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DBC.  
  
 [3] perché la connessione non si trova in uno stato connesso, non è interessata dalla transazione.  
  
 [4] non è stato possibile eseguire il commit o il rollback sulla connessione. In questo caso la funzione restituisce SQL_ERROR.  
  
 [5] il commit o il rollback è riuscito sulla connessione. La funzione restituisce SQL_ERROR se il commit o il rollback non è riuscito in un'altra connessione oppure la funzione restituisce SQL_SUCCESS se il commit o il rollback è riuscito in tutte le connessioni.  
  
 [6] è stata allocata almeno un'istruzione per la connessione.  
  
 [7] non sono state allocate istruzioni per la connessione.  
  
 [8] la connessione include almeno un'istruzione per la quale è presente un cursore aperto e l'origine dati conserva i cursori quando viene eseguito il commit o il rollback delle transazioni, a seconda del fatto che *CompletionType* sia stato SQL_COMMIT o SQL_ROLLBACK). Per ulteriori informazioni, vedere gli attributi SQL_CURSOR_COMMIT_BEHAVIOR e SQL_CURSOR_ROLLBACK_BEHAVIOR in [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 [9] se per la connessione erano presenti istruzioni per le quali erano presenti cursori aperti, i cursori non venivano conservati quando è stato eseguito il commit o il rollback della transazione.  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect e SQLExecute  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2] C6 [3]|--|  
  
 [1] la connessione era in modalità autocommit e l'istruzione eseguita non era una *specifica* del *cursore* , ad esempio un'istruzione SELECT. in alternativa, la connessione era in modalità di commit manuale e l'istruzione eseguita non ha iniziato una transazione.  
  
 [2] la connessione era in modalità autocommit e l'istruzione eseguita era una *specifica* del *cursore* , ad esempio un'istruzione SELECT.  
  
 [3] la connessione era in modalità di commit manuale e l'origine dati ha avviato una transazione.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1|C0|HY010|HY010|HY010|HY010|HY010|  
|IH 2|IH|C1|HY010|HY010|HY010|HY010|  
|IH 3|IH|IH|IH|IH|C4 [5]--[6]|--[7] C4 [5] e [8] C5 [6] e [8]|  
|IH 4|IH|IH|IH|--|--|--|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DBC.  
  
 [3] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [4] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC.  
  
 [5] è stata allocata una sola istruzione alla connessione.  
  
 [6] sono state allocate più istruzioni per la connessione.  
  
 [7] la connessione era in modalità di commit manuale.  
  
 [8] la connessione era in modalità autocommit.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1|IH|IH|IH|IH|--|C5 [3]--[4]|  
|IH 2|IH|IH|IH|IH|--|--|  
  
 [1] Questa riga Mostra le transazioni quando l'argomento *Option* è SQL_CLOSE.  
  
 [2] Questa riga Mostra le transazioni quando l'argomento *Option* è SQL_UNBIND o SQL_RESET_PARAMS.  
  
 [3] la connessione è in modalità autocommit e nessun cursore è aperto in alcuna istruzione ad eccezione di questa.  
  
 [4] la connessione era in modalità di commit manuale o era in modalità autocommit e un cursore era aperto in almeno un'altra istruzione.  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|HY010|--|--|--|  
  
 [1] l'argomento dell' *attributo* è SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE o SQL_ATTR_TRACEFILE oppure è stato impostato un valore per l'attributo Connection.  
  
 [2] l'argomento dell' *attributo* non è SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE o SQL_ATTR_TRACEFILE e non è stato impostato un valore per l'attributo Connection.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField e SQLGetDiagRec  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1|--|--|--|--|--|--|  
|IH 2|IH|--|--|--|--|--|  
|IH 3|IH|IH|IH|IH|--|--|  
|IH 4|IH|IH|IH|--|--|--|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DBC.  
  
 [3] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [4] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|08003|--|--|--|  
  
 [1] l'argomento *InfoType* è stato SQL_ODBC_VER.  
  
 [2] l'argomento *InfoType* non è stato SQL_ODBC_VER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2]|--[3] C5 [1]|  
  
 [1] la connessione era in modalità autocommit e la chiamata a **SQLMoreResults** non ha inizializzato l'elaborazione di un set di risultati di una specifica del cursore.  
  
 [2] la connessione era in modalità autocommit e la chiamata a **SQLMoreResults** ha inizializzato l'elaborazione di un set di risultati di una specifica del cursore.  
  
 [3] la connessione era in modalità di commit manuale.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|(08003)|(08003)|--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2]|--|  
  
 [1] la connessione era in modalità autocommit oppure l'origine dati non ha iniziato una transazione.  
  
 [2] la connessione era in modalità di commit manuale e l'origine dati ha avviato una transazione.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|HY010|--[3] 08002 [4] HY011 [5]|--[3] 08002 [4] HY011 [5]|--[3] e [6] C5 [8] 08002 [4] HY011 [5] o [7]|  
  
 [1] l'argomento dell' *attributo* non è SQL_ATTR_TRANSLATE_LIB o SQL_ATTR_TRANSLATE_OPTION.  
  
 [2] l'argomento dell' *attributo* è stato SQL_ATTR_TRANSLATE_LIB o SQL_ATTR_TRANSLATE_OPTION.  
  
 [3] l'argomento dell' *attributo* non è SQL_ATTR_ODBC_CURSORS o SQL_ATTR_PACKET_SIZE.  
  
 [4] l'argomento dell' *attributo* è stato SQL_ATTR_ODBC_CURSORS.  
  
 [5] l'argomento dell' *attributo* è stato SQL_ATTR_PACKET_SIZE.  
  
 [6] l'argomento dell' *attributo* non è stato SQL_ATTR_AUTOCOMMIT oppure l'argomento dell' *attributo* è stato SQL_ATTR_AUTOCOMMIT e l'impostazione di questo attributo non ha eseguito il commit della transazione.  
  
 [7] l'argomento dell' *attributo* è stato SQL_ATTR_TXN_ISOLATION.  
  
 [8] l'argomento dell' *attributo* è stato SQL_ATTR_AUTOCOMMIT e l'impostazione di questo attributo ha eseguito il commit della transazione.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|HY010|--|--|--|  
  
## <a name="all-other-odbc-functions"></a>Tutte le altre funzioni ODBC  
  
|C0<br /><br /> Nessun ENV.|C1<br /><br /> Non allocato|C2<br /><br /> Allocato|C3<br /><br /> Dati necessari|C4<br /><br /> Connesso|C5<br /><br /> Istruzione|C6<br /><br /> Transazione|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--|--|
