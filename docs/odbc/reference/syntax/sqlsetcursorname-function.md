---
description: Funzione SQLSetCursorName
title: Funzione SQLSetCursorName | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetCursorName
helpviewer_keywords:
- SQLSetCursorName function [ODBC]
ms.assetid: 4e055946-12d4-4589-9891-41617a50f34e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2141a6ee259e51080d018af32bd0c433a1725e1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208093"
---
# <a name="sqlsetcursorname-function"></a>Funzione SQLSetCursorName
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLSetCursorName** associa un nome di cursore a un'istruzione attiva. Se un'applicazione non chiama **SQLSetCursorName**, il driver genera i nomi di cursore necessari per l'elaborazione dell'istruzione SQL.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLSetCursorName(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CursorName,  
     SQLSMALLINT   NameLength);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *Nomecursore*  
 Input Nome del cursore. Per un'elaborazione efficiente, il nome del cursore non deve includere spazi iniziali o finali nel nome del cursore e se il nome del cursore include un identificatore delimitato, il delimitatore deve essere posizionato come primo carattere nel nome del cursore.  
  
 *NameLength*  
 Input Lunghezza in caratteri di **CursorName*.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetCursorName** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLSetCursorName** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il nome del cursore supera il limite massimo, pertanto è stato utilizzato solo il numero massimo consentito di caratteri.|  
|24000|Stato del cursore non valido|L'istruzione corrispondente a *statementHandle* era già in uno stato eseguito o posizionato in un cursore.|  
|34000|Nome di cursore non valido|Il nome del cursore specificato in **CursorName* non è valido perché supera la lunghezza massima definita dal driver oppure è stato avviato con "SQLCUR" o "SQL_CUR".|  
|3C000|Nome cursore duplicato|Il nome del cursore specificato in **CursorName* esiste già.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY009|Uso non valido del puntatore null|(DM) l'argomento *CursorName* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione Aynchronous era ancora in esecuzione quando è stata chiamata la funzione **SQLSetCursorName** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) l'argomento *NameLength* è minore di 0 ma non uguale a SQL_NTS.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 I nomi dei cursori vengono utilizzati solo nelle istruzioni Update e Delete posizionate, ad esempio **Update** _Table-Name_ ... **Dove Current di** _Cursor-Name_). Per altre informazioni, vedere [istruzioni Update e Delete posizionate](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Se l'applicazione non chiama **SQLSetCursorName** per definire un nome di cursore, durante l'esecuzione di un'istruzione di query il driver genera un nome che inizia con le lettere SQL_CUR e non supera i 18 caratteri di lunghezza.  
  
 Tutti i nomi di cursore all'interno della connessione devono essere univoci. La lunghezza massima del nome di un cursore è definita dal driver. Per garantire la massima interoperabilità, è consigliabile che le applicazioni limitino i nomi di cursore a non più di 18 caratteri. In ODBC 3 *. x*, se il nome di un cursore è un identificatore tra virgolette, viene considerato con distinzione tra maiuscole e minuscole e può contenere caratteri non consentiti dalla sintassi di SQL, ad esempio spazi vuoti o parole chiave riservate. Se il nome di un cursore deve essere considerato con distinzione tra maiuscole e minuscole, deve essere passato come identificatore tra virgolette.  
  
 Il nome di un cursore impostato in modo esplicito o implicito rimane impostato fino a quando non viene eliminata l'istruzione a cui è associato, utilizzando **SQLFreeHandle**. È possibile chiamare **SQLSetCursorName** per rinominare un cursore su un'istruzione purché il cursore sia in uno stato allocato o preparato.  
  
## <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente, un'applicazione utilizza **SQLSetCursorName** per impostare un nome di cursore per un'istruzione. Viene quindi utilizzata l'istruzione per recuperare i risultati dalla tabella CUSTOMers. Infine, viene eseguito un aggiornamento posizionato per modificare il numero di telefono di John Smith. Si noti che l'applicazione utilizza diversi handle di istruzione per le istruzioni **Select** e **Update** .  
  
 Per un altro esempio di codice, vedere [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 10  
  
SQLHSTMT     hstmtSelect,  
SQLHSTMT     hstmtUpdate;  
SQLRETURN    retcode;  
SQLHDBC      hdbc;  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   cbName, cbPhone;  
  
/* Allocate the statements and set the cursor name. */  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtSelect);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtUpdate);  
SQLSetCursorName(hstmtSelect, "C1", SQL_NTS);  
  
/* SELECT the result set and bind its columns to local buffers. */  
  
SQLExecDirect(hstmtSelect,  
      "SELECT NAME, PHONE FROM CUSTOMERS",  
      SQL_NTS);  
SQLBindCol(hstmtSelect, 1, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
SQLBindCol(hstmtSelect, 2, SQL_C_CHAR, szPhone, PHONE_LEN, &cbPhone);  
  
/* Read through the result set until the cursor is */  
/* positioned on the row for John Smith. */  
  
do  
 retcode = SQLFetch(hstmtSelect);  
while ((retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) &&  
   (strcmp(szName, "Smith, John") != 0));  
  
/* Perform a positioned update of John Smith's name. */  
  
if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
   SQLExecDirect(hstmtUpdate,  
   "UPDATE EMPLOYEE SET PHONE=\"2064890154\" WHERE CURRENT OF C1",  
   SQL_NTS);  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Restituzione di un nome di cursore|[Funzione SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Impostazione delle opzioni di scorrimento del cursore|[Funzione SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
