---
description: 'Funzione SQLDescribeCol '
title: Funzione SQLDescribeCol | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeCol
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeCol
helpviewer_keywords:
- SQLDescribeCol function [ODBC]
ms.assetid: eddef353-83f3-4a3c-8f24-f9ed888890a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ceffc2b0db32ccce75c566c89ac3d69b535abe29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192074"
---
# <a name="sqldescribecol-function"></a>Funzione SQLDescribeCol 
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLDescribeCol** restituisce il nome della colonna, il tipo, le dimensioni della colonna, le cifre decimali e il supporto di valori null per una colonna del set di risultati. Queste informazioni sono disponibili anche nei campi del IRD.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLDescribeCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLCHAR *      ColumnName,  
      SQLSMALLINT    BufferLength,  
      SQLSMALLINT *  NameLengthPtr,  
      SQLSMALLINT *  DataTypePtr,  
      SQLULEN *      ColumnSizePtr,  
      SQLSMALLINT *  DecimalDigitsPtr,  
      SQLSMALLINT *  NullablePtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *ColumnNumber*  
 Input Numero di colonna dei dati dei risultati, ordinati in sequenza in ordine crescente di colonne, a partire da 1. L'argomento *ColumnNumber* può anche essere impostato su 0 per descrivere la colonna del segnalibro.  
  
 *ColumnName*  
 Output Puntatore a un buffer con terminazione null in cui restituire il nome della colonna. Questo valore viene letto dal campo SQL_DESC_NAME di IRD. Se la colonna è senza nome o non è possibile determinare il nome della colonna, il driver restituisce una stringa vuota.  
  
 Se *ColumnName* è null, *NameLengthPtr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *ColumnName*.  
  
 *BufferLength*  
 Input Lunghezza del buffer **ColumnName* , in caratteri.  
  
 *NameLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri (esclusa la terminazione null) disponibili per la restituzione in \* *ColumnName*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *bufferLength*, il nome della colonna in \* *ColumnName* viene troncato in *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
 *DataTypePtr*  
 Output Puntatore a un buffer in cui restituire il tipo di dati SQL della colonna. Questo valore viene letto dal campo SQL_DESC_CONCISE_TYPE di IRD. Si tratta di uno dei valori dei [tipi di dati SQL](../../../odbc/reference/appendixes/sql-data-types.md)o di un tipo di dati SQL specifico del driver. Se non è possibile determinare il tipo di dati, il driver restituisce SQL_UNKNOWN_TYPE.  
  
 In ODBC 3. *x*, SQL_TYPE_DATE, SQL_TYPE_TIME o SQL_TYPE_TIMESTAMP viene restituito in *\* DataTypePTR* per i dati di data, ora o timestamp, rispettivamente, in ODBC 2.** viene restituito x, SQL_DATE, SQL_TIME o SQL_TIMESTAMP. Gestione driver esegue i mapping necessari quando ODBC 2. l'applicazione *x* funziona con ODBC 3. driver *x* o quando ODBC 3. l'applicazione *x* funziona con ODBC 2. driver *x* .  
  
 Quando *ColumnNumber* è uguale a 0 (per una colonna di segnalibro), SQL_BINARY viene restituito in *\* DataTypePTR* per i segnalibri a lunghezza variabile. (SQL_INTEGER viene restituito se i segnalibri vengono utilizzati da ODBC 3. applicazione *x* che utilizza ODBC 2. driver *x* o ODBC 2. applicazione *x* che utilizza un ODBC 3. driver *x* .)  
  
 Per ulteriori informazioni su questi tipi di dati, vedere tipi di dati [SQL](../../../odbc/reference/appendixes/sql-data-types.md) in Appendice D: tipi di dati. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.  
  
 *ColumnSizePtr*  
 Output Puntatore a un buffer in cui restituire la dimensione (in caratteri) della colonna nell'origine dati. Se non è possibile determinare le dimensioni della colonna, il driver restituisce 0. Per ulteriori informazioni sulle dimensioni delle colonne, vedere [dimensioni delle colonne, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.  
  
 *DecimalDigitsPtr*  
 Output Puntatore a un buffer in cui restituire il numero di cifre decimali della colonna nell'origine dati. Se il numero di cifre decimali non può essere determinato o non è applicabile, il driver restituisce 0. Per ulteriori informazioni sulle cifre decimali, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.  
  
 *NullablePtr*  
 Output Puntatore a un buffer in cui restituire un valore che indica se la colonna ammette valori NULL. Questo valore viene letto dal campo SQL_DESC_NULLABLE di IRD. I possibili valori sono i seguenti:  
  
 SQL_NO_NULLS: la colonna non consente valori NULL.  
  
 SQL_NULLABLE: la colonna consente valori NULL.  
  
 SQL_NULLABLE_UNKNOWN: il driver non è in grado di determinare se la colonna supporta i valori NULL.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLDescribeCol** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLDescribeCol** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il nome della colonna del buffer \*  non è abbastanza grande per restituire l'intero nome della colonna, quindi il nome della colonna è stato troncato. La lunghezza del nome di colonna non troncato viene restituita in **NameLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07005|L'istruzione preparata non è una *specifica del cursore*|L'istruzione associata a *statementHandle* non ha restituito un set di risultati. Nessuna colonna da descrivere.|  
|07009|Indice del descrittore non valido|(DM) il valore specificato per l'argomento *ColumnNumber* è uguale a 0 e l'opzione SQL_ATTR_USE_BOOKMARKS istruzione è stata SQL_UB_OFF.<br /><br /> Il valore specificato per l'argomento *ColumnNumber* è maggiore del numero di colonne nel set di risultati.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stato chiamato **SQLDescribeCol** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) la funzione è stata chiamata prima di chiamare **SQLPrepare**, **SQLExecute** o una funzione di catalogo nell'handle di istruzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *bufferLength* è minore di 0.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
 **SQLDescribeCol** può restituire qualsiasi SQLSTATE che può essere restituito da **SQLPrepare** o **SQLExecute quando viene** chiamato dopo **SQLPrepare** e prima di **SQLExecute**, a seconda del momento in cui l'origine dati valuta l'istruzione SQL associata all'handle di istruzione.  
  
 Per motivi di prestazioni, un'applicazione non deve chiamare **SQLDescribeCol** prima di eseguire un'istruzione.  
  
## <a name="comments"></a>Commenti  
 Un'applicazione in genere chiama **SQLDescribeCol** dopo una chiamata a **SQLPrepare** e prima o dopo la chiamata associata a **SQLExecute**. Un'applicazione può anche chiamare **SQLDescribeCol** dopo una chiamata a **SQLExecDirect**. Per ulteriori informazioni, vedere [metadati del set di risultati](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLDescribeCol** Recupera il nome della colonna, il tipo e la lunghezza generati da un'istruzione **SELECT** . Se la colonna è un'espressione, **ColumnName* può essere una stringa vuota o un nome definito dal driver.  
  
> [!NOTE]  
>  ODBC supporta SQL_NULLABLE_UNKNOWN come estensione, anche se la specifica di interfaccia del gruppo di accesso Open Group e del gruppo di accesso SQL non specifica l'opzione per **SQLDescribeCol**.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a una colonna in un set di risultati|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su una colonna in un set di risultati|[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Recupero di più righe di dati|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Restituzione del numero di colonne del set di risultati|[SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Preparazione di un'istruzione per l'esecuzione|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
