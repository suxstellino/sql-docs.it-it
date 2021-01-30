---
description: Funzione SQLGetStmtAttr
title: Funzione SQLGetStmtAttr | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetStmtAttr
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC]
ms.assetid: e321d460-e997-4527-aee6-207cf5a498e9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b527d4a828ab1684977ced21a0509a7c75bf2041
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186186"
---
# <a name="sqlgetstmtattr-function"></a>Funzione SQLGetStmtAttr
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetStmtAttr** restituisce l'impostazione corrente di un attributo di istruzione.  
  
> [!NOTE]  
>  Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando ODBC 3. l'applicazione *x* funziona con ODBC 2. driver *x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetStmtAttr(  
     SQLHSTMT        StatementHandle,  
     SQLINTEGER      Attribute,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *StatementHandle*  
 Input Handle di istruzione.  
  
 *Attributo*  
 Input Attributo da recuperare.  
  
 *ValuePtr*  
 Output Puntatore a un buffer in cui restituire il valore dell'attributo specificato nell' *attributo*.  
  
 Se *ValuePtr* è null, *StringLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *ValuePtr*.  
  
 *BufferLength*  
 Input Se *attribute* è un attributo definito da ODBC e *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di \* *ValuePtr*. Se *attribute* è un attributo definito da ODBC e \* *ValuePtr* è un numero intero, *bufferLength* viene ignorato. Se il valore restituito in *\* ValuePtr* è una stringa Unicode (quando si chiama **SQLGetStmtAttrW**), l'argomento *bufferLength* deve essere un numero pari.  
  
 Se *attribute* è un attributo definito dal driver, l'applicazione indica la natura dell'attributo a gestione driver impostando l'argomento *bufferLength* . *BufferLength* può avere i valori seguenti:  
  
-   Se *\* ValuePtr* è un puntatore a una stringa di caratteri, *bufferLength* è la lunghezza della stringa o del SQL_NTS.  
  
-   Se *\* ValuePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *bufferLength*. Questo inserisce un valore negativo in *bufferLength*.  
  
-   Se *\* ValuePtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *bufferLength* deve essere SQL_IS_POINTER.  
  
-   Se *\* ValuePtr* contiene un tipo di dati a lunghezza fissa, *BufferLength* è SQL_IS_INTEGER o SQL_IS_UINTEGER, in base alle esigenze.  
  
 *StringLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di byte, escluso il carattere di terminazione null, disponibile per restituire in *\* ValuePtr*. Se il valore dell'attributo è una stringa di caratteri e il numero di byte disponibili per restituire è maggiore o uguale a *bufferLength*, i dati in *\* ValuePtr* vengono troncati a *bufferLength* meno la lunghezza di un carattere di terminazione null e con terminazione null dal driver.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetStmtAttr** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLGetStmtAttr** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati restituiti in *\* ValuePtr* sono stati troncati in modo da essere *bufferLength* meno la lunghezza di un carattere di terminazione null. La lunghezza del valore stringa untruncated viene restituita in **StringLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|24000|Stato del cursore non valido|L' *attributo* argument è stato SQL_ATTR_ROW_NUMBER e il cursore non è aperto oppure il cursore è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nell'argomento *MessageText* descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *statementHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLGetStmtAttr** .<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per *statementHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|*(DM) \* ValuePtr* è una stringa di caratteri e bufferLength è minore di zero, ma non uguale a SQL_NTS.|  
|HY092|Identificatore di attributo/opzione non valido|Il valore specificato per l' *attributo* argument non è valido per la versione di ODBC supportata dal driver.|  
|HY109|Posizione del cursore non valida|L'argomento dell' *attributo* è stato SQL_ATTR_ROW_NUMBER e la riga è stata eliminata o non è stato possibile recuperarla.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il valore specificato per l' *attributo* argument è un attributo di istruzione ODBC valido per la versione di ODBC supportata dal driver, ma non è supportato dal driver.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver corrispondente a *statementHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Per informazioni generali sugli attributi dell'istruzione, vedere [attributi](../../../odbc/reference/develop-app/statement-attributes.md)di istruzione.  
  
 Una chiamata a **SQLGetStmtAttr** restituisce in *\* ValuePtr* il valore dell'attributo dell'istruzione specificato nell' *attributo*. Tale valore può essere un valore SQLULEN o una stringa di caratteri con terminazione null. Se il valore è un valore SQLULEN, è possibile che alcuni driver scrivano solo la versione precedente a 32 bit o a 16 bit di un buffer e lascino invariati il bit di ordine superiore. Pertanto, le applicazioni devono utilizzare un buffer di SQLULEN e inizializzare il valore su 0 prima di chiamare questa funzione. Inoltre, gli argomenti *bufferLength* e *StringLengthPtr* non vengono utilizzati. Se il valore è una stringa con terminazione null, l'applicazione specifica la lunghezza massima di tale stringa nell'argomento *bufferLength* e il driver restituisce la lunghezza di tale stringa nel buffer *\* StringLengthPtr* .  
  
 Per consentire alle applicazioni che chiamano **SQLGetStmtAttr** di funzionare con ODBC 2. driver *x* , viene eseguito il mapping di una chiamata a **SQLGetStmtAttr** in Gestione driver a **SQLGetStmtOption**.  
  
 Gli attributi di istruzione seguenti sono di sola lettura, quindi possono essere recuperati da **SQLGetStmtAttr**, ma non impostati da **SQLSetStmtAttr**:  
  
-   SQL_ATTR_IMP_PARAM_DESC  
  
-   SQL_ATTR_IMP_ROW_DESC  
  
-   SQL_ATTR_ROW_NUMBER  
  
 Per un elenco di attributi che è possibile impostare e recuperare, vedere [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione dell'impostazione di un attributo di connessione|[Funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Impostazione di un attributo di connessione|[Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
