---
description: Funzione SQLParamData
title: Funzione SQLParamData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLParamData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLParamData
helpviewer_keywords:
- SQLParamData function [ODBC]
ms.assetid: 68fe010d-9539-4e5b-a260-c8d32423b1db
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e050102612c7e5d370e730909522fa23e801a8f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195852"
---
# <a name="sqlparamdata-function"></a>Funzione SQLParamData
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLParamData** viene usato insieme a **SQLPutData** per fornire i dati dei parametri in fase di esecuzione dell'istruzione e con **SQLGetData** per recuperare i dati del parametro di output trasmesso.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLParamData(  
     SQLHSTMT       StatementHandle,  
     SQLPOINTER *   ValuePtrPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *ValuePtrPtr*  
 Output Puntatore a un buffer in cui restituire l'indirizzo del buffer *ParameterValuePtr* specificato in **SQLBindParameter** (per i dati dei parametri) o l'indirizzo del buffer *TargetValuePtr* specificato in **SQLBindCol** (per i dati della colonna), come contenuto nel campo del record del descrittore di SQL_DESC_DATA_PTR.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_INVALID_HANDLE o SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLParamData** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLParamData** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07006|Violazione dell'attributo del tipo di dati con restrizioni|Non è stato possibile convertire il valore di dati identificato dall'argomento *ValueType* in **SQLBindParameter** per il parametro associato nel tipo di dati identificato dall'argomento *ParameterType* in **SQLBindParameter**.<br /><br /> Il valore di dati restituito per un parametro associato come SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_OUTPUT non può essere convertito nel tipo di dati identificato dall'argomento *ValueType* in **SQLBindParameter**.<br /><br /> Se non è stato possibile convertire i valori dei dati per una o più righe, ma una o più righe sono state restituite correttamente, questa funzione restituisce SQL_SUCCESS_WITH_INFO.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22026|Lunghezza dei dati non corrispondente.|Il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y" e sono stati inviati meno dati per un parametro Long (il tipo di dati era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati Long Data Source-Specific) rispetto a quello specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**.<br /><br /> Il tipo di informazioni SQL_NEED_LONG_DATA_LEN in **SQLGetInfo** era "Y" e sono stati inviati meno dati per una colonna lunga (il tipo di dati era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo di dati Long Data Source-Specific) rispetto a quello specificato nel buffer di lunghezza corrispondente a una colonna in una riga di dati che è stata aggiunta o aggiornata con **SQLBulkOperations** o aggiornata con **SQLSetPos**.|  
|40001|Errore di serializzazione|È stato eseguito il rollback della transazione a causa di un deadlock delle risorse con un'altra transazione.|  
|40003|Completamento istruzione sconosciuto|La connessione associata non è riuscita durante l'esecuzione di questa funzione e non è possibile determinare lo stato della transazione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *statementHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stato chiamato **SQLCancel** o **SQLCancelHandle** su *statementHandle*; la funzione è stata quindi chiamata nuovamente in *statementHandle*.<br /><br /> La funzione è stata chiamata e prima del completamento dell'esecuzione, **SQLCancel** o **SQLCancelHandle** è stato chiamato su *statementHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) la chiamata di funzione precedente non era una chiamata a **SQLExecDirect**, **SQLExecute**, **SQLBulkOperations** o **SQLSetPos** in cui il codice restituito era SQL_NEED_DATA o la chiamata di funzione precedente era una chiamata a **SQLPutData**.<br /><br /> La chiamata di funzione precedente era una chiamata a **SQLParamData**.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLParamData** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona (non questa) per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e ha restituito SQL_NEED_DATA. **SQLCancel** è stato chiamato prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver che corrisponde a *statementHandle* non supporta la funzione.|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
  
 Se **SQLParamData** viene chiamato durante l'invio di dati per un parametro in un'istruzione SQL, può restituire qualsiasi valore SQLSTATE che può essere restituito dalla funzione chiamata per eseguire l'istruzione (**SQLExecute** o **SQLExecDirect**). Se viene chiamato durante l'invio di dati per una colonna da aggiornare o aggiungere con **SQLBulkOperations** o da aggiornare con **SQLSetPos**, può restituire qualsiasi valore SQLSTATE che può essere restituito da **SQLBulkOperations** o **SQLSetPos**.  
  
## <a name="comments"></a>Commenti  
 È possibile chiamare **SQLParamData** per fornire i dati di data-at-execution per due utilizzi: i dati dei parametri che verranno usati in una chiamata a **SQLExecute** o **SQLExecDirect** oppure i dati di colonna che verranno usati quando una riga viene aggiornata o aggiunta da una chiamata a **SQLBulkOperations** o aggiornata da una chiamata a **SQLSetPos**. In fase di esecuzione, **SQLParamData** restituisce all'applicazione un indicatore dei dati richiesti dal driver.  
  
 Quando un'applicazione chiama **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos**, il driver restituisce SQL_NEED_DATA se sono necessari dati data-at-execution. Un'applicazione chiama quindi **SQLParamData** per determinare i dati da inviare. Se il driver richiede i dati dei parametri, il driver restituisce nel buffer di output *\* ValuePtrPtr* il valore che l'applicazione inserisce nel buffer del set di righe. L'applicazione può utilizzare questo valore per determinare i dati dei parametri richiesti dal driver. Se il driver richiede dati della colonna, il driver restituisce nel buffer *\* ValuePtrPtr* l'indirizzo a cui la colonna è stata originariamente associata, come indicato di seguito:  
  
 *Indirizzo associato*  +  *Offset binding* + ((*numero di riga* -1) x *dimensioni elemento*)  
  
 dove le variabili vengono definite come indicato nella tabella seguente.  
  
|Variabile|Descrizione|  
|--------------|-----------------|  
|*Indirizzo associato*|Indirizzo specificato con l'argomento *TargetValuePtr* in **SQLBindCol**.|  
|*Offset binding*|Valore archiviato in corrispondenza dell'indirizzo specificato con l'attributo dell'istruzione SQL_ATTR_ROW_BIND_OFFSET_PTR.|  
|*Numero di riga*|Numero in base 1 della riga nel set di righe. Per i recuperi a riga singola, che corrispondono all'impostazione predefinita, questo valore è 1.|  
|*Dimensioni elemento*|Valore dell'attributo SQL_ATTR_ROW_BIND_TYPE Statement per i buffer di dati e di lunghezza/indicatore.|  
  
 Restituisce inoltre SQL_NEED_DATA, che è un indicatore dell'applicazione che deve chiamare **SQLPutData** per inviare i dati.  
  
 L'applicazione chiama **SQLPutData** il numero di volte necessario per inviare i dati di data-at-execution per la colonna o il parametro. Dopo che tutti i dati sono stati inviati per la colonna o il parametro, l'applicazione chiama di nuovo **SQLParamData** . Se **SQLParamData** restituisce di nuovo SQL_NEED_DATA, è necessario inviare i dati per un altro parametro o colonna. Quindi, l'applicazione chiama di nuovo **SQLPutData**. Se tutti i dati in fase di esecuzione sono stati inviati per tutti i parametri o le colonne, **SQLParamData** restituisce SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, il valore in *\* ValuePtrPtr* non è definito e l'istruzione SQL può essere eseguita oppure è possibile elaborare la chiamata **SQLBulkOperations** o **SQLSetPos** .  
  
 Se **SQLParamData** fornisce dati di parametro per un'istruzione di aggiornamento o eliminazione ricercata che non influisce su alcuna riga nell'origine dati, la chiamata a **SQLParamData** restituisce SQL_NO_DATA.  
  
 Per ulteriori informazioni sul modo in cui i dati dei parametri data-at-execution vengono passati in fase di esecuzione dell'istruzione, vedere "passaggio di valori dei parametri" in [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) e [invio di dati Long](../../../odbc/reference/develop-app/sending-long-data.md). Per ulteriori informazioni sull'aggiornamento o l'aggiunta dei dati della colonna data-at-execution, vedere la sezione relativa all'utilizzo di SQLSetPos in [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), "esecuzione di aggiornamenti bulk mediante segnalibri" in [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [Long Data e SQLSetPos e SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
 È possibile chiamare **SQLParamData** per recuperare i parametri di output trasmessi. Quando **SQLMoreResults**, **SQLExecute**, **sqlgetdata** o **SQLExecDirect** restituisce SQL_PARAM_DATA_AVAILABLE, chiamare **SQLParamData** per determinare in quale parametro è disponibile un valore. Per ulteriori informazioni sui parametri di output SQL_PARAM_DATA_AVAILABLE e trasmessi, vedere [recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di un buffer a un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annullamento dell'elaborazione di istruzioni|[Funzione SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Restituzione di informazioni su un parametro in un'istruzione|[Funzione SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Invio di dati dei parametri in fase di esecuzione|[SQLPutData Function](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recupero di parametri di output tramite SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
