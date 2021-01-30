---
description: Transizioni di istruzione
title: Transizioni di istruzioni | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], statement
- state transitions [ODBC], statement
- statement transitions [ODBC]
ms.assetid: 3d70e0e3-fe83-4b4d-beac-42c82495a05b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7fb43bb8aa7ad9bd71906036a081dac4ec91c794
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202545"
---
# <a name="statement-transitions"></a>Transizioni di istruzione
Le istruzioni ODBC hanno gli Stati seguenti.  
  
|State|Descrizione|  
|-----------|-----------------|  
|S0|Istruzione unallocata. (Lo stato della connessione deve essere C4, C5 o C6. Per ulteriori informazioni, vedere [transizioni di connessione](../../../odbc/reference/appendixes/connection-transitions.md).|  
|S1|Istruzione allocata.|  
|S2|Istruzione preparata. Non verrà creato alcun set di risultati.|  
|S3|Istruzione preparata. Verrà creato un set di risultati (possibilmente vuoto).|  
|S4|L'istruzione è stata eseguita e non è stato creato alcun set di risultati.|  
|S5|L'istruzione è stata eseguita ed è stato creato un set di risultati (possibilmente vuoto). Il cursore viene aperto e posizionato prima della prima riga del set di risultati.|  
|S6|Cursore posizionato con **SQLFetch** o **SQLFetchScroll**.|  
|S7|Cursore posizionato con **SQLExtendedFetch**.|  
|S8|La funzione necessita di dati. **SQLParamData** non è stato chiamato.|  
|S9|La funzione necessita di dati. **SQLPutData** non è stato chiamato.|  
|S10|La funzione necessita di dati. **SQLPutData** è stato chiamato.|  
|S11|Ancora in esecuzione. Un'istruzione viene lasciata in questo stato dopo che una funzione eseguita in modo asincrono restituisce SQL_STILL_EXECUTING. Un'istruzione è temporaneamente in questo stato mentre è in esecuzione qualsiasi funzione che accetta un handle di istruzione. La residenza temporanea nello stato S11 non viene visualizzata nelle tabelle di stato ad eccezione della tabella di stato per **SQLCancel**. Mentre un'istruzione è temporaneamente nello stato S11, la funzione può essere annullata chiamando **SQLCancel** da un altro thread.|  
|S12|Esecuzione asincrona annullata. In S12, un'applicazione deve chiamare la funzione annullata finché non restituisce un valore diverso da SQL_STILL_EXECUTING. La funzione è stata annullata correttamente solo se la funzione restituisce SQL_ERROR e SQLSTATE HY008 (operazione annullata). Se restituisce qualsiasi altro valore, ad esempio SQL_SUCCESS, l'operazione di annullamento non è riuscita e la funzione è stata eseguita normalmente.|  
  
 Gli stati S2 e S3 sono noti come stati preparati, Stati da S5 a S7 come Stati del cursore, Stati da S8 a S10 come stati dei dati necessari e Stati S11 e S12 come stati asincroni. In ognuno di questi gruppi, le transizioni vengono visualizzate separatamente solo quando sono diverse per ogni stato del gruppo. nella maggior parte dei casi, le transizioni per ogni stato in ogni gruppo sono le stesse.  
  
 Nelle tabelle seguenti viene illustrato il modo in cui ogni funzione ODBC influiscono sullo stato dell'istruzione.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1], [5], [6]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[2], [5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|S1 [3]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[4], [5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DBC.  
  
 [3] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [4] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC.  
  
 [5] la chiamata a **SQLAllocHandle** con *OutputHandlePtr* che punta a un handle valido sovrascrive tale handle senza considerare il contenuto precedente a tale handle e potrebbe causare problemi per i driver ODBC. La programmazione di applicazioni ODBC non è corretta per chiamare **SQLAllocHandle** due volte con la stessa variabile dell'applicazione definita per *\* OutputHandlePtr* senza chiamare **SQLFreeHandle** per liberare l'handle prima della riallocazione. La sovrascrittura degli handle ODBC in questo modo può causare un comportamento incoerente o errori della parte dei driver ODBC.  
  
## <a name="sqlbindcol"></a>SQLBindCol  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbindparameter"></a>SQLBindParameter  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbrowseconnect-sqlconnect-and-sqldriverconnect"></a>SQLBrowseConnect, SQLConnect e SQLDriverConnect  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|08002|08002|08002|08002|08002|08002|08002|  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|Vedi tabella successiva|HY010|NS [c] HY010 o|  
  
## <a name="sqlbulkoperations-cursor-states"></a>SQLBulkOperations (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[s] S8 [d] S11 [x]|--[s] S8 [d] S11 [x]|HY010|  
  
## <a name="sqlcancel"></a>SQLCancel  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|S1 [1] S2 [Nr] e [2] S3 [r] e [2] S5 [3] e [5] S6 ([3] o [4]) e [6] S7 [4] e [7]|Vedi tabella successiva|  
  
 [1]   **SQLExecDirect** ha restituito SQL_NEED_DATA.  
  
 [2]   **SQLExecute** ha restituito SQL_NEED_DATA.  
  
 [3]   **SQLBulkOperations** restituito SQL_NEED_DATA.  
  
 [4]   **SQLSetPos** restituito SQL_NEED_DATA.  
  
 [5]   **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch** non è stato chiamato.  
  
 [6]   **SQLFetch** o **SQLFetchScroll** è stato chiamato.  
  
 [7]   **SQLExtendedFetch** è stato chiamato.  
  
## <a name="sqlcancel-asynchronous-states"></a>SQLCancel (Stati asincroni)  
  
|S11<br /><br /> Ancora in esecuzione|S12<br /><br /> Asynch annullato|  
|-----------------------------|-----------------------------|  
|NS [1] S12 [2]|S12|  
  
 [1] l'istruzione era temporaneamente nello stato S11 durante l'esecuzione di una funzione. **SQLCancel** è stato chiamato da un thread diverso.  
  
 [2] l'istruzione era nello stato S11 perché una funzione chiamata restituiva in modo asincrono SQL_STILL_EXECUTING.  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|24000|24000|24000|S1 [NP] S3 [p]|HY010|HY010|  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|Vedi tabella successiva|24000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqlcolattribute-prepared-states"></a>SQLColAttribute (stati preparati)  
  
|S2<br /><br /> Nessun risultato|S3<br /><br /> Risultati|  
|-----------------------|--------------------|  
|--[1] 07005 [2]|--[s] S11 x|  
  
 [1]   *FieldIdentifier* è stato SQL_DESC_COUNT.  
  
 [2]   *FieldIdentifier* non è stato SQL_DESC_COUNT.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges e SQLTables  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S5 [s] S11 [x]|S1 [e] S5 [s] S11 [x]|S1 [e] e [1] S5 [s] e [1] S11 [x] e [1] 24000 [2]|Vedi tabella successiva|HY010|NS [c] HY010 o|  
  
 [1] il risultato corrente è l'ultimo o unico risultato oppure non sono presenti risultati correnti. Per ulteriori informazioni su più risultati, vedere [più risultati](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [2] il risultato corrente non è l'ultimo risultato.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables-cursor-states"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges e SQLTables (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000 [1]|24000|  
  
 [1] questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH [1]|--|--|--|--|HY010|NS [c] e [3] HY010 [o] o [4]|  
|IH [2]|HY010|Vedi tabella successiva|24000|--[s] S11 x|HY010|NS [c] e [3] HY010 [o] o [4]|  
  
 [1] Questa riga Mostra le transizioni quando l'argomento *SourceDescHandle* è ARD, APD o dip.  
  
 [2] Questa riga Mostra le transizioni quando l'argomento *SourceDescHandle* è un IRD.  
  
 [3] gli argomenti *SourceDescHandle* e *TargetDescHandle* sono uguali a quelli della funzione **SQLCopyDesc** eseguita in modo asincrono.  
  
 [4] l'argomento *SourceDescHandle* o l'argomento *TargetDescHandle* (o entrambi) sono diversi da quelli della funzione **SQLCopyDesc** eseguita in modo asincrono.  
  
## <a name="sqlcopydesc-prepared-states"></a>SQLCopyDesc (stati preparati)  
  
|S2<br /><br /> Nessun risultato|S3<br /><br /> Risultati|  
|-----------------------|--------------------|  
|24000 [1]|--[s] S11 [x]|  
  
 1 Questa riga Mostra le transizioni quando l'argomento *SourceDescHandle* è un IRD.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources e SQLDrivers  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqldescribecol"></a>SQLDescribeCol  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|Vedi tabella successiva|24000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqldescribecol-prepared-states"></a>SQLDescribeCol (stati preparati)  
  
|S2<br /><br /> Nessun risultato|S3<br /><br /> Risultati|  
|-----------------------|--------------------|  
|07005|--[s] S11 [x]|  
  
## <a name="sqldescribeparam"></a>SQLDescribeParam  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|HY010|HY010|HY010|NS [c] HY010 [o]|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|S0 [1]|S0 [1]|S0 [1]|S0 [1]|HY010|HY010|  
  
 [1] la chiamata a **SQLConnect** consente di liberare tutte le istruzioni associate alla connessione. Inoltre, viene restituito lo stato di connessione a C2; lo stato di connessione deve essere C4 prima che lo stato dell'istruzione sia S0.  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--[2] o [3] S1 [1]|--[3] S1 [NP] e ([1] o [2]) S1 [p] e [1] S2 [p] e [2]|--[3] S1 [NP] e ([1] o [2]) S1 [p] e [1] S3 [p] e [2]|HY010|HY010|  
  
 [1] l'argomento *CompletionType* è SQL_COMMIT e **SQLGetInfo** restituisce SQL_CB_DELETE per il tipo di informazioni SQL_CURSOR_COMMIT_BEHAVIOR oppure l'argomento *CompletionType* è SQL_ROLLBACK e **SQLGetInfo** restituisce SQL_CB_DELETE per il tipo di informazioni SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
 [2] l'argomento *CompletionType* è SQL_COMMIT e **SQLGetInfo** restituisce SQL_CB_CLOSE per il tipo di informazioni SQL_CURSOR_COMMIT_BEHAVIOR oppure l'argomento *CompletionType* è SQL_ROLLBACK e **SQLGetInfo** restituisce SQL_CB_CLOSE per il tipo di informazioni SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
 [3] l'argomento *CompletionType* è SQL_COMMIT e **SQLGetInfo** restituisce SQL_CB_PRESERVE per il tipo di informazioni SQL_CURSOR_COMMIT_BEHAVIOR oppure l'argomento *CompletionType* è SQL_ROLLBACK e **SQLGetInfo** restituisce SQL_CB_PRESERVE per il tipo di informazioni SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
## <a name="sqlexecdirect"></a>SQLExecDirect  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S4 [s] e [Nr] S5 [s] e [r] S8 [d] S11 [x]|--[e] e [1] S1 [e] e [2] S4 [s] e [Nr] S5 [s] e [r] S8 [d] S11 [x]|--[e], [1] e [3] S1 [e], [2] e [3] S4 [s], [Nr] e [3] S5 [s], [r] e [3] S8 [d] e [3] S11 [x] e [3] 24000 [4]|Vedi tabella successiva|HY010|NS [c] HY010 [o]|  
  
 [1] l'errore è stato restituito da Gestione driver.  
  
 [2] l'errore non è stato restituito da Gestione driver.  
  
 [3] il risultato corrente è l'ultimo o unico risultato oppure non sono presenti risultati correnti. Per ulteriori informazioni su più risultati, vedere [più risultati](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [4] il risultato corrente non è l'ultimo risultato.  
  
## <a name="sqlexecdirect-cursor-states"></a>SQLExecDirect (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000 [1]|24000|  
  
 [1] questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.  
  
## <a name="sqlexecute"></a>SQLExecute  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|Vedi tabella successiva|S2 [e], p, e [1] S4 [s], [p], [Nr] e [1] S5 [s], [p], [r] e [1] S8 [d], [p] e [1] S11 [x], [p] e [1] 24000 [p] e [2] HY010 [NP]|Vedere la tabella degli Stati del cursore|HY010|NS [c] HY010 [o]|  
  
 [1] il risultato corrente è l'ultimo o unico risultato oppure non sono presenti risultati correnti. Per ulteriori informazioni su più risultati, vedere [più risultati](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [2] il risultato corrente non è l'ultimo risultato.  
  
## <a name="sqlexecute-prepared-states"></a>SQLExecute (stati preparati)  
  
|S2<br /><br /> Nessun risultato|S3<br /><br /> Risultati|  
|-----------------------|--------------------|  
|S4 [s] S8 [d] S11 [x]|S5 [s] S8 [d] S11 [x]|  
  
## <a name="sqlexecute-cursor-states"></a>SQLExecute (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000 [p] HY010 [NP]|24000 [p], [1] HY010 [NP]|24000 [p] HY010 [NP]|  
  
 [1] questo errore viene restituito da Gestione driver se **SQLFetch** o **SQLFetchScroll** non ha restituito SQL_NO_DATA e viene restituito dal driver se **SQLFetch** o **SQLFetchScroll** ha restituito SQL_NO_DATA.  
  
## <a name="sqlextendedfetch"></a>SQLExtendedFetch  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S1010|S1010|24000|Vedi tabella successiva|S1010|NS [c] S1010 [o]|  
  
## <a name="sqlextendedfetch-cursor-states"></a>SQLExtendedFetch (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S7 [s] o [nf] S11 [x]|S1010|--[s] o [nf] S11 [x]|  
  
## <a name="sqlfetch-and-sqlfetchscroll"></a>SQLFetch e SQLFetchScroll  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|Vedi tabella successiva|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlfetch-and-sqlfetchscroll-cursor-states"></a>SQLFetch e SQLFetchScroll (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S6 [s] o [nf] S11 [x]|--[s] o [nf] S11 [x]|HY010|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|HY010|HY010|HY010|HY010|HY010|HY010|  
|IH [2]|S0|S0|S0|S0|HY010|HY010|  
|--[3]|--|--|--|--|--|--|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV o SQL_HANDLE_DBC.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [3] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_DESC e il descrittore è stato allocato in modo esplicito.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH [1]|--|--|S1 [NP] S2 [p]|S1 [NP] S3 [p]|HY010|HY010|  
|IH [2]|--|--|--|--|HY010|HY010|  
  
 [1] Questa riga Mostra le transizioni quando l' *opzione* è stata SQL_CLOSE.  
  
 [2] Questa riga Mostra le transizioni quando l' *opzione* è stata SQL_UNBIND o SQL_RESET_PARAMS. Se l'argomento *Option* è stato SQL_DROP e il driver sottostante è un driver ODBC 3 *. x* , gestione driver esegue il mapping di questo oggetto a una chiamata a **SQLFreeHandle** con *HandleType* impostato su SQL_HANDLE_STMT. Per ulteriori informazioni, vedere la tabella di transizione per [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetcursorname"></a>SQLGetCursorName  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlgetdata"></a>SQLGetData  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|Vedi tabella successiva|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlgetdata-cursor-states"></a>SQLGetData (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|--[s] o [nf] S11 [x] 24000 [b] HY109 [i]|--[s] o [nf] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField e SQLGetDescRec  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1] o [2] HY010 [3]|Vedi tabella successiva|--[1] o [2] 24000 [3]|--[1], [2] o [3] S11 [3] e [x]|HY010|NS [c] o [4] HY010 [o] e [5]|  
  
 [1] l'argomento *DescriptorHandle* è un APD o ARD.  
  
 [2] l'argomento *DescriptorHandle* è un valore dpi.  
  
 [3] l'argomento *DescriptorHandle* è un IRD.  
  
 [4] l'argomento *DescriptorHandle* è uguale all'argomento *DescriptorHandle* nella funzione **SQLGetDescField** o **SQLGetDescRec** in esecuzione in modo asincrono.  
  
 [5] l'argomento *DescriptorHandle* è diverso dall'argomento *DescriptorHandle* nella funzione **SQLGetDescField** o **SQLGetDescRec** in esecuzione in modo asincrono.  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec-prepared-states"></a>SQLGetDescField e SQLGetDescRec (stati preparati)  
  
|S2<br /><br /> Nessun risultato|S3<br /><br /> Risultati|  
|-----------------------|--------------------|  
|--[1], [2] o [3] S11 [2] e [x]|--[1], [2] o [3] S11 [x]|  
  
 [1] l'argomento *DescriptorHandle* è un APD o ARD.  
  
 [2] l'argomento *DescriptorHandle* è un valore dpi.  
  
 [3] l'argomento *DescriptorHandle* è un IRD. Si noti che queste funzioni restituiscono sempre SQL_NO_DATA nello stato S2 quando *DescriptorHandle* è un IRD.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField e SQLGetDiagRec  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--|--|--|  
|IH [2]|--[3]|--[3]|--|--|--[3]|--[3]|  
  
 [1] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_ENV, SQL_HANDLE_DBC o SQL_HANDLE_DESC.  
  
 [2] Questa riga Mostra le transizioni quando *HandleType* è stato SQL_HANDLE_STMT.  
  
 [3]   **SQLGetDiagField** restituisce sempre un errore in questo stato quando *DiagIdentifier* è SQL_DIAG_ROW_COUNT.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1] 24000 [2]|--[1] 24000 [2]|--[1] 24000 [2]|Vedi tabella successiva|HY010|HY010|  
  
 [1] l'attributo dell'istruzione non è stato SQL_ATTR_ROW_NUMBER.  
  
 [2] l'attributo dell'istruzione è stato SQL_ATTR_ROW_NUMBER.  
  
## <a name="sqlgetstmtattr-cursor-states"></a>SQLGetStmtAttr (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[1] 24000 [2]|--[1] o ([v] e [2]) 24000 [b] e [2] HY109 [i] e [2]|--[i] o ([v] e [2]) 24000 [b] e [2] HY109 [1] e [2]|  
  
 [1] l'argomento dell' *attributo* non è stato SQL_ATTR_ROW_NUMBER.  
  
 [2] l'argomento dell' *attributo* è stato SQL_ATTR_ROW_NUMBER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1]|--[1]|--[s] e [2] S1 [nf], [NP] e [4] S2 [nf], [p] e [4] S5 [s] e [3] S11 [x]|S1 [nf], [NP] e [4] S3 [nf], [p] e [4] S4 [s] e [2] S5 [s] e [3] S11 [x]|HY010|NS [c] HY010 [o]|  
  
 [1] la funzione restituisce sempre SQL_NO_DATA in questo stato.  
  
 [2] il risultato successivo è un conteggio delle righe.  
  
 [3] il risultato successivo è un set di risultati.  
  
 [4] il risultato corrente è l'ultimo risultato.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlnumparams"></a>SQLNumParams  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlnumresultcols"></a>SQLNumResultCols  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata"></a>SQLParamData  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|HY010|HY010|Vedi tabella successiva|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata-need-data-states"></a>SQLParamData (sono necessari stati dati)  
  
|S8<br /><br /> Dati necessari|S9<br /><br /> Deve inserire|S10<br /><br /> Può inserire|  
|----------------------|---------------------|---------------------|  
|S1 [e] e [1] S2 [e], [Nr] e [2] S3 [e], [r] e [2] S5 [e] e [4] S6 [e] e [5] S7 [e] e [3] S9 [d] S11 [x]|HY010|S1 [e] e [1] S2 [e], [Nr] e [2] S3 [e], [r] e [2] S4 [s], [Nr] e ([1] o [2]) S5 [s], [r] e ([1] o [2]) S5 ([s] o [e]) e [4] S6 ([s] o [e]) e [5] S7 ([s] o [e]) e [3] S9 [d] S11 [x]|  
  
 [1]   **SQLExecDirect** ha restituito SQL_NEED_DATA.  
  
 [2]   **SQLExecute** ha restituito SQL_NEED_DATA.  
  
 [3]   **SQLSetPos** è stato chiamato dallo stato S7 e ha restituito SQL_NEED_DATA.  
  
 [4]   **SQLBulkOperations** è stato chiamato dallo stato S5 e restituito SQL_NEED_DATA.  
  
 [5]   **SQLSetPos** o **SQLBulkOperations** è stato chiamato da stato S6 e restituito SQL_NEED_DATA.  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S2 [s] e [Nr] S3 [s] e [r] S11 [x]|--[s] o ([e] e [1]) S1 [e] e [2] S11 [x]|S1 [e] e [3] S2 [s], [Nr] e [3] S3 [s], [r] e [3] S11 [x] e [3] 24000 [4]|Vedi tabella successiva|HY010|NS [c] HY010 [o]|  
  
 [1] la preparazione ha esito negativo per un motivo diverso dalla convalida dell'istruzione (SQLSTATE was HY009 [valore di argomento non valido] o HY090 [stringa non valida o lunghezza del buffer]).  
  
 [2] la preparazione ha esito negativo durante la convalida dell'istruzione (SQLSTATE non è HY009 [valore argomento non valido] o HY090 [lunghezza di stringa o di buffer non valida]).  
  
 [3] il risultato corrente è l'ultimo o unico risultato oppure non sono presenti risultati correnti. Per ulteriori informazioni su più risultati, vedere [più risultati](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [4] il risultato corrente non è l'ultimo risultato.  
  
## <a name="sqlprepare-cursor-states"></a>SQLPrepare (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000|24000|  
  
## <a name="sqlputdata"></a>SQLPutData  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|HY010|HY010|Vedi tabella successiva|NS [c] HY010 [o]|  
  
## <a name="sqlputdata-need-data-states"></a>SQLPutData (sono necessari stati dati)  
  
|S8<br /><br /> Dati necessari|S9<br /><br /> Deve inserire|S10<br /><br /> Può inserire|  
|----------------------|---------------------|---------------------|  
|HY010|S1 [e] e [1] S2 [e], [Nr] e [2] S3 [e], [r] e [2] S5 [e] e [4] S6 [e] e [5] S7 [e] e [3] S10 [s] S11 [x]|--[s] S1 [e] e [1] S2 [e], [Nr] e [2] S3 [e], [r] e [2] S5 [e] e [4] S6 [e] e [5] S7 [e] e [3] S11 [x] HY011 [6]|  
  
 [1]   **SQLExecDirect** ha restituito SQL_NEED_DATA.  
  
 [2]   **SQLExecute** ha restituito SQL_NEED_DATA.  
  
 [3]   **SQLSetPos** è stato chiamato dallo stato S7 e ha restituito SQL_NEED_DATA.  
  
 [4]   **SQLBulkOperations** è stato chiamato dallo stato S5 e restituito SQL_NEED_DATA.  
  
 [5]   **SQLSetPos** o **SQLBulkOperations** è stato chiamato da stato S6 e restituito SQL_NEED_DATA.  
  
 [6] una o più chiamate a **SQLPutData** per un singolo parametro hanno restituito SQL_SUCCESS, quindi è stata effettuata una chiamata a **SQLPutData** per lo stesso parametro con *StrLen_Or_Ind* impostato su SQL_NULL_DATA.  
  
## <a name="sqlrowcount"></a>SQLRowCount  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|--|--|HY010|HY010|  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--[2] 24000 [3]|HY010|HY010|  
  
 [1] Questa riga Mostra le transizioni quando l' *attributo* è un attributo di connessione. Per le transizioni quando *attribute* è un attributo Statement, vedere la tabella di transizione dell'istruzione per **SQLSetStmtAttr**.  
  
 [2] l'argomento dell' *attributo* non è stato SQL_ATTR_CURRENT_CATALOG.  
  
 [3] l'argomento dell' *attributo* è stato SQL_ATTR_CURRENT_CATALOG.  
  
## <a name="sqlsetcursorname"></a>SQLSetCursorName  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|24000|24000|HY010|HY010|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField e SQLSetDescRec  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH [1]|--|--|--|--|HY010|HY010|  
  
 [1] Questa riga Mostra le transizioni in cui l'argomento *DescriptorHandle* è ARD, APD, dpi o (per **SQLSetDescField**) IRD quando l'argomento *FieldIdentifier* è SQL_DESC_ARRAY_STATUS_PTR o SQL_DESC_ROWS_PROCESSED_PTR. È un errore chiamare **SQLSetDescField** per un IRD quando *FieldIdentifier* è qualsiasi altro valore.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|HY011|HY011|HY011|HY011|Y011|HY01|HY011|  
  
## <a name="sqlsetpos"></a>SQLSetPos  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|Vedi tabella successiva|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlsetpos-cursor-states"></a>SQLSetPos (Stati del cursore)  
  
|S5<br /><br /> Aperto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
  
|S0<br /><br /> Non allocato|S1<br /><br /> Allocato|S2-S3<br /><br /> Prepared|S4<br /><br /> Eseguito|S5-S7<br /><br /> Cursore|S8-S10<br /><br /> Dati necessari|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--[1] HY011 [2]|--[1] 24000 [2]|--[1] 24000 [2]|HY010 [NP] o [1] HY011 [p] e [2]|HY010 [NP] o [1] HY011 [p] e [2]|  
  
 [1] l'argomento dell' *attributo* non è SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE o SQL_ATTR_CURSOR_SENSITIVITY.  
  
 [2] l'argomento dell' *attributo* è SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE o SQL_ATTR_CURSOR_SENSITIVITY.
