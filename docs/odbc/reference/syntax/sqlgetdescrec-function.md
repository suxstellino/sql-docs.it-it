---
description: Funzione SQLGetDescRec
title: Funzione SQLGetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescRec
helpviewer_keywords:
- SQLGetDescRec function [ODBC]
ms.assetid: 325e0907-8e87-44e8-a111-f39e636a9cbc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a340ecc75e7ae4f03c44051f594fda7305a2a00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205635"
---
# <a name="sqlgetdescrec-function"></a>Funzione SQLGetDescRec
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetDescRec** restituisce le impostazioni correnti o i valori di più campi di un record di descrittore. I campi restituiti descrivono il nome, il tipo di dati e l'archiviazione dei dati di colonna o di parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetDescRec(  
      SQLHDESC        DescriptorHandle,  
      SQLSMALLINT     RecNumber,  
      SQLCHAR *       Name,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLSMALLINT *   TypePtr,  
      SQLSMALLINT *   SubTypePtr,  
      SQLLEN *        LengthPtr,  
      SQLSMALLINT *   PrecisionPtr,  
      SQLSMALLINT *   ScalePtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *DescriptorHandle*  
 Input Handle descrittore.  
  
 *RecNumber*  
 Input Indica il record del descrittore da cui l'applicazione cerca le informazioni. I record del descrittore sono numerati da 1, con il numero di record 0 che è il record del segnalibro. L'argomento *RecNumber* deve essere minore o uguale al valore di SQL_DESC_COUNT. Se *RecNumber* è minore o uguale a SQL_DESC_COUNT ma la riga non contiene dati per una colonna o un parametro, una chiamata a **SQLGetDescRec** restituirà i valori predefiniti dei campi. Per ulteriori informazioni, vedere l'argomento relativo all'inizializzazione dei campi del descrittore in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 *Nome*  
 Output Puntatore a un buffer in cui restituire il campo SQL_DESC_NAME per il record del descrittore.  
  
 Se *Name* è null, *StringLengthPtr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta il *nome*.  
  
 *BufferLength*  
 Input Lunghezza del buffer dei *nomi* *, in caratteri.  
  
 *StringLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero di caratteri dei dati disponibili da restituire nel buffer dei \* *nomi* , escluso il carattere di terminazione null. Se il numero di caratteri è maggiore o uguale a *bufferLength*, i dati in \* *Name* vengono troncati in *bufferLength* meno la lunghezza di un carattere di terminazione null e con terminazione null dal driver.  
  
 *TypePtr*  
 Output Puntatore a un buffer in cui restituire il valore del campo SQL_DESC_TYPE per il record del descrittore.  
  
 *SubTypePtr*  
 Output Per i record il cui tipo è SQL_DATETIME o SQL_INTERVAL, si tratta di un puntatore a un buffer in cui restituire il valore del campo SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *LengthPtr*  
 Output Puntatore a un buffer in cui restituire il valore del campo SQL_DESC_OCTET_LENGTH per il record del descrittore.  
  
 *PrecisionPtr*  
 Output Puntatore a un buffer in cui restituire il valore del campo SQL_DESC_PRECISION per il record del descrittore.  
  
 *ScalePtr*  
 Output Puntatore a un buffer in cui restituire il valore del campo SQL_DESC_SCALE per il record del descrittore.  
  
 *NullablePtr*  
 Output Puntatore a un buffer in cui restituire il valore del campo SQL_DESC_NULLABLE per il record del descrittore.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_NO_DATA o SQL_INVALID_HANDLE.  
  
 Viene restituito SQL_NO_DATA se *RecNumber* è maggiore del numero corrente di record del descrittore.  
  
 Viene restituito SQL_NO_DATA se *DescriptorHandle* è un handle IRD e l'istruzione è nello stato preparato o eseguito, ma non è stato associato alcun cursore aperto.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetDescRec** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DESC e un *handle* di *DescriptorHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLGetDescRec** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il \* *nome* del buffer non è sufficientemente grande da restituire l'intero campo del descrittore. Pertanto, il campo è stato troncato. La lunghezza del campo del descrittore untruncated viene restituita in **StringLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07009|Indice del descrittore non valido|L'argomento *FieldIdentifier* è un campo record, l'argomento *RecNumber* è stato impostato su 0 e l'argomento *DescriptorHandle* è un handle DIP.<br /><br /> (DM) l'argomento *RecNumber* è stato impostato su 0 e l'attributo SQL_ATTR_USE_BOOKMARKS Statement è stato impostato su SQL_UB_OFF e l'argomento *DescriptorHandle* è un handle IRD.<br /><br /> L'argomento *RecNumber* è minore di 0.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare la memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY007|L'istruzione associata non è preparata|*DescriptorHandle* è stato associato a un IRD e l'handle di istruzione associato non era nello stato preparato o eseguito.|  
|HY010|Errore sequenza funzione|(DM) *DescriptorHandle* è stato associato a un *statementHandle* per il quale è stata chiamata una funzione in esecuzione asincrona (non questa) ed è ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) *DescriptorHandle* è stato associato a un *statementHandle* per il quale **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *DescriptorHandle*. Questa funzione asincrona era ancora in esecuzione quando è stato chiamato **SQLGetDescRec** .|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *DescriptorHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Un'applicazione può chiamare **SQLGetDescRec** per recuperare i valori dei campi di descrizione seguenti per un singolo parametro o colonna:  
  
-   SQL_DESC_NAME  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (per i record il cui tipo è SQL_DATETIME o SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_NULLABLE  
  
 **SQLGetDescRec** non recupera i valori per i campi di intestazione.  
  
 Un'applicazione può impedire la restituzione dell'impostazione di un campo impostando l'argomento che corrisponde al campo a un puntatore null.  
  
 Quando un'applicazione chiama **SQLGetDescRec** per recuperare il valore di un campo che non è definito per un determinato tipo di descrittore, la funzione restituisce SQL_SUCCESS, ma il valore restituito per il campo non è definito. Ad esempio, se si chiama **SQLGetDescRec** per il campo SQL_DESC_NAME o SQL_DESC_NULLABLE di un oggetto APD o ARD, viene restituito SQL_SUCCESS ma un valore non definito per il campo.  
  
 Quando un'applicazione chiama **SQLGetDescRec** per recuperare il valore di un campo definito per un determinato tipo di descrittore ma che non dispone di un valore predefinito e non è ancora stato impostato, la funzione restituisce SQL_SUCCESS ma il valore restituito per il campo non è definito. Per ulteriori informazioni, vedere l'argomento relativo all'inizializzazione dei campi del descrittore in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 I valori dei campi possono anche essere recuperati singolarmente da una chiamata a **SQLGetDescField**. Per una descrizione dei campi in un'intestazione o in un record del descrittore, vedere [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Per ulteriori informazioni sui descrittori, vedere [descrittori](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di una colonna|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Associazione di un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Recupero di un campo del descrittore|[Funzione SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Impostazione di più campi di descrizione|[Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
