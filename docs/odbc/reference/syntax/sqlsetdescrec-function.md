---
description: Funzione SQLSetDescRec
title: Funzione SQLSetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescRec
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescRec
helpviewer_keywords:
- SQLSetDescRec function [ODBC]
ms.assetid: bf55256c-7eb7-4e3f-97ef-b0fee09ba829
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 65e0a789f38ad9f0d1d4b624caca896d01356ce2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160186"
---
# <a name="sqlsetdescrec-function"></a>Funzione SQLSetDescRec
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 La funzione **SQLSetDescRec** imposta più campi di descrizione che influiscono sul tipo di dati e sul buffer associato a una colonna o a dati di parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLSetDescRec(  
      SQLHDESC      DescriptorHandle,  
      SQLSMALLINT   RecNumber,  
      SQLSMALLINT   Type,  
      SQLSMALLINT   SubType,  
      SQLLEN        Length,  
      SQLSMALLINT   Precision,  
      SQLSMALLINT   Scale,  
      SQLPOINTER    DataPtr,  
      SQLLEN *      StringLengthPtr,  
      SQLLEN *      IndicatorPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *DescriptorHandle*  
 Input Handle descrittore. Non deve trattarsi di un handle IRD.  
  
 *RecNumber*  
 Input Indica il record del descrittore che contiene i campi da impostare. I record del descrittore sono numerati da 0, con il numero di record 0 che è il record del segnalibro. Questo argomento deve essere maggiore o uguale a 0. Se *RecNumber* è maggiore del valore di SQL_DESC_COUNT, SQL_DESC_COUNTis è stato modificato nel valore di *RecNumber*.  
  
 *Tipo*  
 Input Valore in cui impostare il campo SQL_DESC_TYPE per il record del descrittore.  
  
 *Sottotipo*  
 Input Per i record il cui tipo è SQL_DATETIME o SQL_INTERVAL, questo è il valore al quale impostare il campo SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *Length*  
 Input Valore in cui impostare il campo SQL_DESC_OCTET_LENGTH per il record del descrittore.  
  
 *Precisione*  
 Input Valore in cui impostare il campo SQL_DESC_PRECISION per il record del descrittore.  
  
 *Ridimensiona*  
 Input Valore in cui impostare il campo SQL_DESC_SCALE per il record del descrittore.  
  
 *DataPtr*  
 [Input o output posticipato] Valore in cui impostare il campo SQL_DESC_DATA_PTR per il record del descrittore. *DataPtr* può essere impostato su un puntatore null.  
  
 L'argomento *DataPtr* può essere impostato su un puntatore null per impostare il campo SQL_DESC_DATA_PTR su un puntatore null. Se l'handle nell'argomento *DescriptorHandle* è associato a un ARD, questa operazione Annulla l'associazione della colonna.  
  
 *StringLengthPtr*  
 [Input o output posticipato] Valore in cui impostare il campo SQL_DESC_OCTET_LENGTH_PTR per il record del descrittore. *StringLengthPtr* può essere impostato su un puntatore null per impostare il campo SQL_DESC_OCTET_LENGTH_PTR su un puntatore null.  
  
 *IndicatorPtr*  
 [Input o output posticipato] Valore in cui impostare il campo SQL_DESC_INDICATOR_PTR per il record del descrittore. *IndicatorPtr* può essere impostato su un puntatore null per impostare il campo SQL_DESC_INDICATOR_PTR su un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetDescRec** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DESC e un *handle* di *DescriptorHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLSetDescRec** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|07009|Indice del descrittore non valido|L'argomento *RecNumber* è stato impostato su 0 e il *DescriptorHandle* fa riferimento a un handle dpi.<br /><br /> L'argomento *RecNumber* è minore di 0.<br /><br /> L'argomento *RecNumber* è maggiore del numero massimo di colonne o parametri che l'origine dati può supportare e l'argomento *DescriptorHandle* è un oggetto APD, dpi o ARD.<br /><br /> L'argomento *RecNumber* è uguale a 0 e l'argomento *DescriptorHandle* fa riferimento a un oggetto APD allocato in modo implicito. Questo errore non si verifica con un descrittore di applicazione allocato in modo esplicito perché non è noto se un descrittore di applicazione allocato in modo esplicito è un valore APD o ARD fino alla fase di esecuzione.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) *DescriptorHandle* è stato associato a un *statementHandle* per il quale è stata chiamata una funzione in esecuzione asincrona (non questa) ed è stata ancora eseguita quando è stata chiamata la funzione.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato per *statementHandle* a cui il *DescriptorHandle* è stato associato e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *DescriptorHandle*. Questa funzione Aynchronous era ancora in esecuzione quando è stata chiamata la funzione **SQLSetDescRec** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati a *DescriptorHandle* e restituiti SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY016|Impossibile modificare il descrittore di una riga di implementazione|L'argomento *DescriptorHandle* è stato associato a un IRD.|  
|HY021|Informazioni descrittore incoerenti|Il campo *tipo* o qualsiasi altro campo associato al campo SQL_DESC_TYPE nel descrittore non è valido o è coerente.<br /><br /> Le informazioni sul descrittore controllate durante una verifica di coerenza non erano coerenti. Vedere "verifiche di coerenza", più avanti in questa sezione.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il driver era un driver ODBC *2. x* , il descrittore era un ARD, l'argomento *ColumnNumber* è stato impostato su 0 e il valore specificato per l'argomento *bufferLength* non è uguale a 4.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *DescriptorHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Un'applicazione può chiamare **SQLSetDescRec** per impostare i campi seguenti per una colonna o un parametro singolo:  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (per i record il cui tipo è SQL_DATETIME o SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_DATA_PTR  
  
-   SQL_DESC_OCTET_LENGTH_PTR  
  
-   SQL_DESC_INDICATOR_PTR  
  
> [!NOTE]  
>  Se una chiamata a **SQLSetDescRec** ha esito negativo, il contenuto del record del descrittore identificato dall'argomento *RecNumber* non è definito.  
  
 Quando si associa una colonna o un parametro, **SQLSetDescRec** consente di modificare più campi che influiscono sull'associazione senza chiamare **SQLBindCol** o **SQLBindParameter** o effettuare più chiamate a **SQLSetDescField**. **SQLSetDescRec** può impostare campi in un descrittore non attualmente associato a un'istruzione. Si noti che **SQLBindParameter** imposta un numero di campi superiore a quello di **SQLSetDescRec**, può impostare i campi sia su un APD che su un dpi in una chiamata e non richiede un handle del descrittore.  
  
> [!NOTE]  
>  L'attributo Statement SQL_ATTR_USE_BOOKMARKS deve sempre essere impostato prima di chiamare **SQLSetDescRec** con un argomento *RecNumber* 0 per impostare i campi dei segnalibri. Sebbene non sia obbligatorio, si tratta di un'operazione fortemente consigliata.  
  
## <a name="consistency-checks"></a>Verifiche coerenza  
 Una verifica della coerenza viene eseguita automaticamente dal driver ogni volta che un'applicazione imposta il campo SQL_DESC_DATA_PTR di un oggetto APD, ARD o dpi. Se uno dei campi non è coerente con gli altri campi, **SQLSetDescRec** restituirà SQLSTATE HY021 (informazioni descrittore incoerenti).  
  
 Ogni volta che un'applicazione imposta il campo SQL_DESC_DATA_PTR di un oggetto APD, ARD o dpi, il driver verifica che il valore del campo SQL_DESC_TYPE e i valori applicabili a tale campo SQL_DESC_TYPE siano validi e coerenti. Questo controllo viene sempre eseguito quando viene chiamato **SQLBindParameter** o **SQLBindCol** o quando viene chiamato **SQLSetDescRec** per un oggetto APD, ARD o dip. Questa verifica della coerenza include i controlli seguenti nei campi del descrittore:  
  
-   Il campo SQL_DESC_TYPE deve essere uno dei tipi ODBC C o SQL validi o un tipo SQL specifico del driver. Il campo SQL_DESC_CONCISE_TYPE deve essere uno dei tipi ODBC C o SQL validi oppure un tipo C o SQL specifico del driver, inclusi i tipi di intervallo di valori DateTime e Interval concisi.  
  
-   Se il campo SQL_DESC_TYPE record è SQL_DATETIME o SQL_INTERVAL, il campo SQL_DESC_DATETIME_INTERVAL_CODE deve essere uno dei codici DateTime o intervallo validi. (Vedere la descrizione del campo SQL_DESC_DATETIME_INTERVAL_CODE in [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)).  
  
-   Se il SQL_DESC_TYPE campo indica un tipo numerico, i campi SQL_DESC_PRECISION e SQL_DESC_SCALE vengono verificati come validi.  
  
-   Se il campo SQL_DESC_CONCISE_TYPE è un tipo di dati time o timestamp, un tipo di intervallo con un componente relativo ai secondi oppure uno dei tipi di dati intervallo con un componente ora, il campo SQL_DESC_PRECISION viene verificato come una precisione in secondi valida.  
  
-   Se il SQL_DESC_CONCISE_TYPE è un tipo di dati interval, il campo SQL_DESC_DATETIME_INTERVAL_PRECISION viene verificato come un valore di precisione leader di intervallo valido.  
  
 Il campo SQL_DESC_DATA_PTR di un oggetto dpi non è impostato normalmente; Tuttavia, un'applicazione può eseguire questa operazione per forzare una verifica di coerenza dei campi di dpi. Non è possibile eseguire una verifica di coerenza su un IRD. Il valore che il campo SQL_DESC_DATA_PTR di dpi è impostato su non è effettivamente archiviato e non può essere recuperato da una chiamata a **SQLGetDescField** o **SQLGetDescRec**; l'impostazione viene eseguita solo per forzare la verifica della coerenza.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Associazione di una colonna|[Funzione SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Associazione di un parametro|[Pagina relativa alla funzione SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Recupero di un singolo campo del descrittore|[Funzione SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Recupero di più campi di descrizione|[Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Impostazione di singoli campi del descrittore|[Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
