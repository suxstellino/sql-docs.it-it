---
description: SQLPutData Function
title: Funzione SQLPutData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPutData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPutData
helpviewer_keywords:
- SQLPutData function [ODBC]
ms.assetid: 9a60f004-1477-4c54-a20c-7378e1116713
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f76a3624222968aabf91f9aff012e9370aa8ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192562"
---
# <a name="sqlputdata-function"></a>SQLPutData Function
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLPutData** consente a un'applicazione di inviare i dati per un parametro o una colonna al driver al momento dell'esecuzione dell'istruzione. Questa funzione può essere utilizzata per inviare valori di dati di tipo carattere o binario in parti a una colonna con un tipo di dati carattere, binario o origine dati (ad esempio, i parametri del SQL_LONGVARBINARY o i tipi di SQL_LONGVARCHAR). **SQLPutData** supporta l'associazione a un tipo di dati C Unicode, anche se il driver sottostante non supporta i dati Unicode.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLPutData(  
      SQLHSTMT     StatementHandle,  
      SQLPOINTER   DataPtr,  
      SQLLEN       StrLen_or_Ind);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *DataPtr*  
 Input Puntatore a un buffer contenente i dati effettivi per il parametro o la colonna. I dati devono essere nel tipo di dati C specificato nell'argomento *ValueType* di **SQLBindParameter** (per i dati dei parametri) o nell'argomento *targetType* di **SQLBindCol** (per i dati della colonna).  
  
 *StrLen_or_Ind*  
 Input Lunghezza di \* *DataPtr*. Specifica la quantità di dati inviati in una chiamata a **SQLPutData**. La quantità di dati può variare a seconda della chiamata per un parametro o una colonna specificata. *StrLen_Or_Ind* viene ignorato a meno che non soddisfi una delle condizioni seguenti:  
  
-   *StrLen_Or_Ind* è SQL_NTS, SQL_NULL_DATA o SQL_DEFAULT_PARAM.  
  
-   Il tipo di dati C specificato in **SQLBindParameter** o **SQLBindCol** è SQL_C_CHAR o SQL_C_BINARY.  
  
-   Il tipo di dati C viene SQL_C_DEFAULT e il tipo di dati C predefinito per il tipo di dati SQL specificato è SQL_C_CHAR o SQL_C_BINARY.  
  
 Per tutti gli altri tipi di dati C, se *StrLen_Or_Ind* non è SQL_NULL_DATA o SQL_DEFAULT_PARAM, il driver presuppone che le dimensioni del \* buffer *DataPtr* siano le dimensioni del tipo di dati c specificato con *ValueType* o *targetType* e inviano l'intero valore di dati. Per ulteriori informazioni, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) in Appendice D: tipi di dati.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLPutData** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLPutData** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati di tipo stringa o binario restituiti per un parametro di output hanno causato il troncamento di dati di tipo carattere non vuoto o binari non NULL. Se si trattasse di un valore stringa, viene troncato a destra. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Non è stato possibile convertire il valore di dati identificato dall'argomento *ValueType* in **SQLBindParameter** per il parametro associato nel tipo di dati identificato dall'argomento *ParameterType* in **SQLBindParameter**.|  
|07S01|Uso non valido del parametro predefinito|Il valore di un parametro, impostato con **SQLBindParameter**, è stato SQL_DEFAULT_PARAM e il parametro corrispondente non ha un valore predefinito.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22001|Dati stringa, troncamento a destra|L'assegnazione di un valore di tipo carattere o binario a una colonna ha comportato il troncamento di caratteri non vuoti (carattere) o byte (binari) non null.<br /><br /> Il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y" e sono stati inviati più dati per un parametro Long (il tipo di dati era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati Long Data Source-Specific) rispetto a quello specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**.<br /><br /> Il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y" e sono stati inviati più dati per una colonna lunga (il tipo di dati era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati Long Data Source-Specific) rispetto a quello specificato nel buffer di lunghezza corrispondente a una colonna in una riga di dati che è stata aggiunta o aggiornata con **SQLBulkOperations** o aggiornata con **SQLSetPos**.|  
|22003|Valore numerico non compreso nell'intervallo|I dati inviati per una colonna o un parametro numerico associato hanno causato il troncamento della parte intera (anziché frazionaria) del numero quando viene assegnato alla colonna della tabella associata.<br /><br /> La restituzione di un valore numerico (come valore numerico o stringa) per uno o più parametri di input/output o di output avrebbe causato il troncamento dell'intero, anziché della parte frazionaria del numero.|  
|22007|Formato DateTime non valido|I dati inviati per un parametro o una colonna associati a una struttura di data, ora o timestamp erano rispettivamente una data, un'ora o un timestamp non valido.<br /><br /> Un parametro di input/output o di output è stato associato a una struttura di data, ora o timestamp C e un valore nel parametro restituito era, rispettivamente, una data, un'ora o un timestamp non valido. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|22008|Overflow del campo DateTime|Un'espressione datetime calcolata per un parametro di input/output o di output ha restituito una struttura di data, ora o timestamp non valida.|  
|22012|Divisione per zero|Un'espressione aritmetica calcolata per un parametro di input/output o di output ha restituito una divisione per zero.|  
|22015|Overflow del campo Interval|I dati inviati per una colonna o un parametro numerico esatto o di intervallo a un tipo di dati SQL intervallo hanno causato la perdita di cifre significative.<br /><br /> I dati sono stati inviati per una colonna intervallo o un parametro con più di un campo, è stato convertito in un tipo di dati numerico e non aveva alcuna rappresentazione nel tipo di dati numeric.<br /><br /> I dati inviati per i dati di colonna o di parametro sono stati assegnati a un tipo di intervallo SQL e non è presente alcuna rappresentazione del valore del tipo C nel tipo SQL intervallo.<br /><br /> I dati inviati per una colonna o un parametro numerico esatto o di intervallo C a un tipo intervallo C hanno causato la perdita di cifre significative.<br /><br /> I dati inviati per i dati di colonna o di parametro sono stati assegnati a una struttura di intervallo C e non era presente alcuna rappresentazione dei dati nella struttura dei dati intervallo.|  
|22018|Valore di carattere non valido per la specifica del cast|Il tipo C è un valore numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo SQL della colonna è un tipo di dati character. il valore della colonna o del parametro non è un valore letterale valido del tipo C associato.<br /><br /> Il tipo SQL è un tipo numerico esatto o approssimativo, un valore DateTime o un tipo di dati interval; il tipo C è stato SQL_C_CHAR; il valore della colonna o del parametro non è un valore letterale valido del tipo SQL associato.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** in *statementHandle*. La funzione è stata chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY009|Uso non valido del puntatore null|(DM) l'argomento *DataPtr* è un puntatore null e l'argomento *StrLen_Or_Ind* non è 0, SQL_DEFAULT_PARAM o SQL_NULL_DATA.|  
|HY010|Errore sequenza funzione|(DM) la chiamata di funzione precedente non era una chiamata a **SQLPutData** o **SQLParamData**.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione SQLPutData.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY019|Dati non di tipo carattere e non binario inviati in parti|**SQLPutData** è stato chiamato più di una volta per un parametro o una colonna e non è stato utilizzato per inviare dati di tipo c a una colonna con un tipo di dati character, Binary o data source specifico oppure per inviare dati binari c a una colonna con un tipo di dati character, Binary o data source specifico.|  
|HY020|Tentativo di concatenare un valore null|**SQLPutData** è stato chiamato più di una volta dalla chiamata che ha restituito SQL_NEED_DATA e, in una di queste chiamate, l'argomento *StrLen_Or_Ind* conteneva SQL_NULL_DATA o SQL_DEFAULT_PARAM.|  
|HY090|Lunghezza della stringa o del buffer non valida|L'argomento *DataPtr* non è un puntatore null e l'argomento *StrLen_Or_Ind* è minore di 0 ma non uguale a SQL_NTS o SQL_NULL_DATA.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
 Se **SQLPutData** viene chiamato durante l'invio di dati per un parametro in un'istruzione SQL, può restituire qualsiasi valore SQLSTATE che può essere restituito dalla funzione chiamata per eseguire l'istruzione (**SQLExecute** o **SQLExecDirect**). Se viene chiamato durante l'invio di dati per una colonna da aggiornare o aggiungere con **SQLBulkOperations** o da aggiornare con **SQLSetPos**, può restituire qualsiasi valore SQLSTATE che può essere restituito da **SQLBulkOperations** o **SQLSetPos**.  
  
## <a name="comments"></a>Commenti  
 È possibile chiamare **SQLPutData** per fornire i dati di data-at-execution per due utilizzi: i dati dei parametri da utilizzare in una chiamata a **SQLExecute** o **SQLExecDirect** oppure i dati di colonna da utilizzare quando una riga viene aggiornata o aggiunta da una chiamata a **SQLBulkOperations** o viene aggiornata da una chiamata a **SQLSetPos**.  
  
 Quando un'applicazione chiama **SQLParamData** per determinare quali dati devono essere inviati, il driver restituisce un indicatore che può essere utilizzato dall'applicazione per determinare i dati dei parametri da inviare o i dati della colonna in cui è possibile trovare i dati. Restituisce inoltre SQL_NEED_DATA, che è un indicatore dell'applicazione che deve chiamare **SQLPutData** per inviare i dati. Nell'argomento *DataPtr* per **SQLPutData**, l'applicazione passa un puntatore al buffer che contiene i dati effettivi per il parametro o la colonna.  
  
 Quando il driver restituisce SQL_SUCCESS per **SQLPutData**, l'applicazione chiama di nuovo **SQLParamData** . **SQLParamData** restituisce SQL_NEED_DATA se è necessario inviare più dati, nel qual caso l'applicazione chiama di nuovo **SQLPutData** . Restituisce SQL_SUCCESS se sono stati inviati tutti i dati di data-at-execution. L'applicazione chiama quindi di nuovo **SQLParamData** . Se il driver restituisce SQL_NEED_DATA e un altro indicatore in *\* ValuePtrPtr*, richiede i dati per un altro parametro o colonna e **SQLPutData** viene chiamato nuovamente. Se il driver restituisce SQL_SUCCESS, sono stati inviati tutti i dati di data-at-execution e l'istruzione SQL può essere eseguita oppure è possibile elaborare la chiamata **SQLBulkOperations** o **SQLSetPos** .  
  
 Per ulteriori informazioni sul modo in cui i dati dei parametri data-at-execution vengono passati in fase di esecuzione dell'istruzione, vedere "passaggio di valori dei parametri" in [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) e [invio di dati Long](../../../odbc/reference/develop-app/sending-long-data.md). Per ulteriori informazioni sull'aggiornamento o l'aggiunta dei dati della colonna data-at-execution, vedere la sezione relativa all'utilizzo di SQLSetPos in [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), "esecuzione di aggiornamenti bulk mediante segnalibri" in [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [Long Data e SQLSetPos e SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
> [!NOTE]  
>  Un'applicazione può utilizzare **SQLPutData** per inviare dati in parti solo quando si inviano dati di tipo carattere c a una colonna con un tipo di dati carattere, binario o specifico dell'origine dati oppure quando si inviano dati c binari a una colonna con un tipo di dati carattere, binario o specifico dell'origine dati. Se **SQLPutData** viene chiamato più di una volta in altre condizioni, restituisce SQL_ERROR e SQLSTATE HY019 (dati non di tipo carattere e non binario inviati in parti).  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente si presuppone un nome di origine dati denominato test. Il database associato deve disporre di una tabella che è possibile creare, come indicato di seguito:  
  
```sql  
CREATE TABLE emp4 (NAME char(30), AGE int, BIRTHDAY datetime, Memo1 text)  
```  
  
```cpp  
// SQLPutData.cpp  
// compile with: odbc32.lib user32.lib  
#include <stdio.h>  
#include <windows.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define TEXTSIZE  12000  
#define MAXBUFLEN 256  
  
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
}  
  
int main() {  
   RETCODE retcode;  
  
   // SQLBindParameter variables.  
   SQLLEN cbTextSize, lbytes;  
  
   // SQLParamData variable.  
   PTR pParmID;  
  
   // SQLPutData variables.  
   UCHAR  Data[] =   
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyz";  
  
   SDWORD cbBatch = (SDWORD)sizeof(Data) - 1;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
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
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"Test", SQL_NTS, (UCHAR*)"",SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Allocate statement handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLAllocHandle(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Set parameters based on total data to send.  
   lbytes = (SDWORD)TEXTSIZE;  
   cbTextSize = SQL_LEN_DATA_AT_EXEC(lbytes);  
  
   // Bind the parameter marker.  
   retcode = SQLBindParameter (hstmt1,           // hstmt  
                               1,                // ipar  
                               SQL_PARAM_INPUT,  // fParamType  
                               SQL_C_CHAR,       // fCType  
                               SQL_LONGVARCHAR,  // FSqlType  
                               lbytes,           // cbColDef  
                               0,                // ibScale  
                               (VOID *)1,        // rgbValue  
                               0,                // cbValueMax  
                               &cbTextSize);     // pcbValue  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLBindParameter Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the command.  
   retcode =   
      SQLExecDirect(hstmt1, (UCHAR*)"INSERT INTO emp4 VALUES('Paul Borm', 46,'1950-11-12 00:00:00', ?)", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_NEED_DATA) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLExecDirect Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Check to see if NEED_DATA; if yes, use SQLPutData.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if (retcode == SQL_NEED_DATA) {  
      while (lbytes > cbBatch) {  
         SQLPutData(hstmt1, Data, cbBatch);  
         lbytes -= cbBatch;  
      }  
      // Put final batch.  
      retcode = SQLPutData(hstmt1, Data, lbytes);   
   }  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Make final SQLParamData call.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("Final SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Clean up.  
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Restituzione del parametro successivo per l'invio dei dati|[Funzione SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
