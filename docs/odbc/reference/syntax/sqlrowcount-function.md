---
description: SQLRowCount Function
title: Funzione SQLRowCount | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRowCount
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLRowCount
helpviewer_keywords:
- SQLRowCount function [ODBC]
ms.assetid: 61e00a8a-9b3b-45b9-b397-7fe818822416
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1fea56b7bf7b54bf4d1aa02efa2c4cb9ea1978e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192464"
---
# <a name="sqlrowcount-function"></a>SQLRowCount Function
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLRowCount** restituisce il numero di righe interessate da un'istruzione **Update**, **Insert** o **Delete** . operazione di SQL_ADD, SQL_UPDATE_BY_BOOKMARK o SQL_DELETE_BY_BOOKMARK in **SQLBulkOperations**; oppure un'operazione di SQL_UPDATE o SQL_DELETE in **SQLSetPos**.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLRowCount(  
      SQLHSTMT   StatementHandle,  
      SQLLEN *   RowCountPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *RowCountPtr*  
 Output Punta a un buffer in cui restituire un conteggio di righe. Per le istruzioni **Update**, **Insert** e **Delete** , per le operazioni di SQL_ADD, SQL_UPDATE_BY_BOOKMARK e SQL_DELETE_BY_BOOKMARK in **SQLBulkOperations** e per le operazioni SQL_UPDATE o SQL_DELETE in **SQLSetPos**, il valore restituito in **RowCountPtr* è il numero di righe interessate dalla richiesta o-1 se il numero di righe interessate non è disponibile.  
  
 Quando viene chiamato **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, **SQLSetPos o SQLMoreResults** , il campo SQL_DIAG_ROW_COUNT della struttura dei dati di diagnostica viene impostato sul numero di righe e il conteggio delle righe viene memorizzato nella cache in modo dipendente dall'implementazione. **SQLRowCount** restituisce il valore del conteggio delle righe memorizzato nella cache. Il valore del conteggio delle righe memorizzato nella cache è valido fino a quando l'handle di istruzione non viene impostato di nuovo sullo stato preparato o allocato, l'istruzione viene rieseguita o viene chiamato **SQLCloseCursor** . Si noti che se una funzione è stata chiamata dopo che è stato impostato il campo SQL_DIAG_ROW_COUNT, il valore restituito da **SQLRowCount** potrebbe essere diverso dal valore nel campo SQL_DIAG_ROW_COUNT perché il campo SQL_DIAG_ROW_COUNT viene reimpostato su 0 da qualsiasi chiamata di funzione.  
  
 Per altre istruzioni e funzioni, il driver può definire il valore restituito in \* *RowCountPtr*. Alcune origini dati, ad esempio, possono essere in grado di restituire il numero di righe restituite da un'istruzione **Select** o una funzione di catalogo prima di recuperare le righe.  
  
> [!NOTE]  
>  Molte origini dati non possono restituire il numero di righe in un set di risultati prima di recuperarle; per garantire la massima interoperabilità, le applicazioni non devono basarsi su questo comportamento.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLRowCount** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti comunemente da **SQLRowCount** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLRowCount** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) la funzione è stata chiamata prima di chiamare **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** per *statementHandle*.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Se l'ultima istruzione SQL eseguita nell'handle di istruzione non è un'istruzione **Update**, **Insert** o **Delete** oppure se l'argomento *Operation* nella chiamata precedente a **SQLBulkOperations** non è SQL_ADD, SQL_UPDATE_BY_BOOKMARK o SQL_DELETE_BY_BOOKMARK oppure se l'argomento *operation* nella precedente chiamata a **SQLSetPos** non è stato SQL_UPDATE o SQL_DELETE, il valore di **RowCountPtr* è definito dal driver. Per ulteriori informazioni, vedere [determinazione del numero di righe interessate](../../../odbc/reference/develop-app/determining-the-number-of-affected-rows.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
