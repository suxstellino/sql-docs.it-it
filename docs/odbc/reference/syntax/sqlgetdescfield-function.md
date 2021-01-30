---
description: Funzione SQLGetDescField
title: Funzione SQLGetDescField | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescField
helpviewer_keywords:
- SQLGetDescField function [ODBC]
ms.assetid: f09ff660-1e4a-4370-be85-90d4da0487d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0692c769921d8e0ee326b19f0b5d501367237c28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201952"
---
# <a name="sqlgetdescfield-function"></a>Funzione SQLGetDescField
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetDescField** restituisce l'impostazione corrente o il valore di un singolo campo di un record di descrittore.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *DescriptorHandle*  
 Input Handle descrittore.  
  
 *RecNumber*  
 Input Indica il record del descrittore da cui l'applicazione cerca le informazioni. I record del descrittore sono numerati da 0, con il numero di record 0 che è il record del segnalibro. Se l'argomento *FieldIdentifier* indica un campo di intestazione, *RecNumber* viene ignorato. Se *RecNumber* è minore o uguale a SQL_DESC_COUNT ma la riga non contiene dati per una colonna o un parametro, una chiamata a **SQLGetDescField** restituirà i valori predefiniti dei campi. Per ulteriori informazioni, vedere l'argomento relativo all'inizializzazione dei campi del descrittore in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 *FieldIdentifier*  
 Input Indica il campo del descrittore di cui deve essere restituito il valore. Per ulteriori informazioni, vedere la sezione "argomento *FieldIdentifier* " in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 *ValuePtr*  
 Output Puntatore a un buffer in cui restituire le informazioni sul descrittore. Il tipo di dati dipende dal valore di *FieldIdentifier*.  
  
 Se *ValuePtr* è di tipo Integer, le applicazioni devono usare un buffer di SQLULEN e inizializzare il valore su 0 prima di chiamare questa funzione perché alcuni driver possono scrivere solo la versione precedente a 32 bit o a 16 bit di un buffer e lasciare invariato il bit di ordine superiore.  
  
 Se *ValuePtr* è null, *StringLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *ValuePtr*.  
  
 *BufferLength*  
 Input Se *FieldIdentifier* è un campo definito da ODBC e *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di \* *ValuePtr*. Se *FieldIdentifier* è un campo definito da ODBC e \* *ValuePtr* è un numero intero, *bufferLength* viene ignorato. Se il valore in *\* ValuePtr* è di un tipo di dati Unicode (quando si chiama **SQLGetDescFieldW**), l'argomento *bufferLength* deve essere un numero pari.  
  
 Se *FieldIdentifier* è un campo definito dal driver, l'applicazione indica la natura del campo per Gestione driver impostando l'argomento *bufferLength* . *BufferLength* può avere i valori seguenti:  
  
-   Se *\* ValuePtr* è un puntatore a una stringa di caratteri, *bufferLength* è la lunghezza della stringa o del SQL_NTS.  
  
-   Se *\* ValuePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *bufferLength*. Questo inserisce un valore negativo in *bufferLength*.  
  
-   Se *\* ValuePtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *bufferLength* deve essere SQL_IS_POINTER.  
  
-   Se *\* ValuePtr* contiene un tipo di dati a lunghezza fissa, *BufferLength* è SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT, a seconda delle esigenze.  
  
 *StringLengthPtr*  
 Output Puntatore al buffer in cui restituire il numero totale di byte, escluso il numero di byte necessari per il carattere di terminazione null, disponibile per restituire in **ValuePtr*.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_NO_DATA o SQL_INVALID_HANDLE.  
  
 Viene restituito SQL_NO_DATA se *RecNumber* è maggiore del numero corrente di record del descrittore.  
  
 Viene restituito SQL_NO_DATA se *DescriptorHandle* è un handle IRD e l'istruzione è nello stato preparato o eseguito, ma non è stato associato alcun cursore aperto.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetDescField** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_STMT e un *handle* di *statementHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLGetDescField** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il buffer \* *ValuePtr* non era sufficientemente grande da restituire l'intero campo del descrittore, quindi il campo è stato troncato. La lunghezza del campo del descrittore untruncated viene restituita in **StringLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07009|Indice del descrittore non valido|(DM) l'argomento *RecNumber* è uguale a 0, l'attributo dell'istruzione SQL_ATTR_USE_BOOKMARK è stato SQL_UB_OFF e l'argomento *DescriptorHandle* è un handle IRD. Questo errore può essere restituito per un descrittore allocato in modo esplicito solo se il descrittore è associato a un handle di istruzione.<br /><br /> L'argomento *FieldIdentifier* è un campo record, l'argomento *RecNumber* è 0 e l'argomento *DescriptorHandle* è un handle DIP.<br /><br /> L'argomento *RecNumber* è minore di 0.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare la memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY007|L'istruzione associata non è preparata|*DescriptorHandle* è stato associato a un *statementHandle* come IRD e l'handle di istruzione associato non è stato preparato o eseguito.|  
|HY010|Errore sequenza funzione|(DM) *DescriptorHandle* è stato associato a un *statementHandle* per il quale è stata chiamata una funzione in esecuzione asincrona (non questa) ed è ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) *DescriptorHandle* è stato associato a un *statementHandle* per il quale **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *DescriptorHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLGetDescField** .|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY021|Informazioni descrittore incoerenti|I campi SQL_DESC_TYPE e SQL_DESC_DATETIME_INTERVAL_CODE non formano un tipo SQL ODBC valido, un tipo SQL valido specifico del driver (per IPD) o un tipo C ODBC valido (per APD o ARDs).|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) *\* ValuePtr* è una stringa di caratteri e *bufferLength* è minore di zero.|  
|HY091|Identificatore del campo del descrittore non valido|*FieldIdentifier* non è un campo definito da ODBC e non è un valore definito dall'implementazione.<br /><br /> *FieldIdentifier* non è stato definito per il *DescriptorHandle*.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *DescriptorHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Un'applicazione può chiamare **SQLGetDescField** per restituire il valore di un singolo campo di un record di descrittore. Una chiamata a **SQLGetDescField** può restituire l'impostazione di qualsiasi campo in qualsiasi tipo di descrittore, inclusi i campi di intestazione, i campi di record e i campi dei segnalibri. Un'applicazione può ottenere le impostazioni di più campi nello stesso o in descrittori diversi, in ordine arbitrario, effettuando chiamate ripetute a **SQLGetDescField**. **SQLGetDescField** può essere chiamato anche per restituire campi di descrizione definiti dal driver.  
  
 Per motivi di prestazioni, un'applicazione non deve chiamare **SQLGetDescField** per un IRD prima di eseguire un'istruzione.  
  
 Le impostazioni di più campi che descrivono il nome, il tipo di dati e l'archiviazione dei dati di colonna o di parametro possono anche essere recuperate in una singola chiamata a **SQLGetDescRec**. È possibile chiamare **SQLGetStmtAttr** per restituire l'impostazione di un singolo campo nell'intestazione del descrittore che è anche un attributo di istruzione. **SQLColAttribute**, **SQLDescribeCol** e **SQLDescribeParam** restituiscono campi di record o segnalibri.  
  
 Quando un'applicazione chiama **SQLGetDescField** per recuperare il valore di un campo che non è definito per un determinato tipo di descrittore, la funzione restituisce SQL_SUCCESS, ma il valore restituito per il campo non è definito. Ad esempio, se si chiama **SQLGetDescField** per il campo SQL_DESC_NAME o SQL_DESC_NULLABLE di un oggetto APD o ARD, viene restituito SQL_SUCCESS ma un valore non definito per il campo.  
  
 Quando un'applicazione chiama **SQLGetDescField** per recuperare il valore di un campo definito per un determinato tipo di descrittore ma che non dispone di un valore predefinito e non è ancora stato impostato, la funzione restituisce SQL_SUCCESS ma il valore restituito per il campo non è definito. Per ulteriori informazioni sull'inizializzazione dei campi del descrittore e delle descrizioni dei campi, vedere l'argomento relativo all'inizializzazione dei campi del descrittore in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Per ulteriori informazioni sui descrittori, vedere [descrittori](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Recupero di più campi di descrizione|[Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Impostazione di un singolo campo del descrittore|[Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Impostazione di più campi di descrizione|[Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
