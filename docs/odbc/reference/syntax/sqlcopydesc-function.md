---
description: Funzione SQLCopyDesc
title: Funzione SQLCopyDesc | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCopyDesc
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLCopyDesc
helpviewer_keywords:
- SQLCopyDesc function [ODBC]
ms.assetid: d5450895-3824-44c4-8aa4-d4f9752a9602
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab2ef18e777d433daf4ab93b73d249e62507ea3c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206311"
---
# <a name="sqlcopydesc-function"></a>Funzione SQLCopyDesc
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLCopyDesc** copia le informazioni sul descrittore da un handle descrittore a un altro.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLCopyDesc(  
     SQLHDESC     SourceDescHandle,  
     SQLHDESC     TargetDescHandle);  
```  
  
## <a name="arguments"></a>Argomenti  
 *SourceDescHandle*  
 Input Handle descrittore di origine.  
  
 *TargetDescHandle*  
 Input Handle descrittore di destinazione. L'argomento *TargetDescHandle* può essere un handle per un descrittore dell'applicazione o un oggetto dpi. Non è possibile impostare *TargetDescHandle* su un handle per un IRD oppure **SQLCOPYDESC** restituirà SQLSTATE HY016 (Impossibile modificare il descrittore di una riga di implementazione).  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLCopyDesc** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DESC e un *handle* di *TargetDescHandle*. Se nella chiamata è stato passato un *SourceDescHandle* non valido, verrà restituito SQL_INVALID_HANDLE, ma non verrà restituito alcun valore SQLSTATE. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLCopyDesc** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
 Quando viene restituito un errore, la chiamata a **SQLCopyDesc** viene immediatamente interrotta e il contenuto dei campi nel descrittore *TargetDescHandle* non è definito.  
  
 Poiché **SQLCopyDesc** può essere implementato chiamando **SQLGetDescField** e **SQLSetDescField**, **SQLCopyDesc** può restituire SQLSTATE restituito da **SQLGetDescField** o **SQLSetDescField**.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare la memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY007|L'istruzione associata non è preparata|*SourceDescHandle* è stato associato a un IRD e l'handle di istruzione associato non era nello stato preparato o eseguito.|  
|HY010|Errore sequenza funzione|(DM) l'handle del descrittore in *SourceDescHandle* o *TargetDescHandle* è stato associato a un *statementHandle* per cui è stata chiamata una funzione in esecuzione asincrona (non questa) ed è ancora in esecuzione quando è stata chiamata la funzione.<br /><br /> (DM) l'handle del descrittore in *SourceDescHandle* o *TargetDescHandle* è stato associato a un *statementHandle* per il quale **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** è stato chiamato e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima dell'invio dei dati per tutti i parametri o le colonne data-at-execution.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per l'handle di connessione associato a *SourceDescHandle* o *TargetDescHandle*. Questa funzione asincrona era ancora in esecuzione quando è stata chiamata la funzione **SQLCopyDesc** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per uno degli handle di istruzione associati a *SourceDescHandle* o *TargetDescHandle* e ha restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY016|Impossibile modificare il descrittore di una riga di implementazione|*TargetDescHandle* è stato associato a un IRD.|  
|HY021|Informazioni descrittore incoerenti|Le informazioni sul descrittore controllate durante una verifica di coerenza non erano coerenti. Per ulteriori informazioni, vedere l'argomento relativo alle verifiche di coerenza in **SQLSetDescField**.|  
|HY092|Identificatore di attributo/opzione non valido|La chiamata a **SQLCopyDesc** ha richiesto una chiamata a **SQLSetDescField**, ma *\* ValuePtr* non era valido per l'argomento *FieldIdentifier* in *TargetDescHandle*.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *SourceDescHandle* o *TargetDescHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Una chiamata a **SQLCopyDesc** copia i campi dell'handle del descrittore di origine nell'handle del descrittore di destinazione. I campi possono essere copiati solo in un descrittore dell'applicazione o in un oggetto dpi, ma non in un IRD. I campi possono essere copiati da un'applicazione o da un descrittore di implementazione.  
  
 I campi possono essere copiati da un IRD solo se l'handle dell'istruzione è nello stato preparato o eseguito. in caso contrario, la funzione restituisce SQLSTATE HY007 (l'istruzione associata non è preparata).  
  
 I campi possono essere copiati da un valore DPI indipendentemente dal fatto che un'istruzione sia stata preparata o meno. Se è stata preparata un'istruzione SQL con parametri dinamici e il popolamento automatico del dispositivo dpi è supportato e abilitato, il valore di dpi viene popolato dal driver. Quando **SQLCopyDesc** viene chiamato con il dpi come *SourceDescHandle*, vengono copiati i campi popolati. Se il DPI non viene popolato dal driver, viene copiato il contenuto dei campi originariamente in dpi.  
  
 Tutti i campi del descrittore, eccetto SQL_DESC_ALLOC_TYPE (che specifica se l'handle descrittore è stato allocato automaticamente o in modo esplicito), vengono copiati, indipendentemente dal fatto che il campo sia definito per il descrittore di destinazione. I campi copiati sovrascrivono i campi esistenti.  
  
 Il driver copia tutti i campi del descrittore se gli argomenti *SourceDescHandle* e *TargetDescHandle* sono associati allo stesso driver, anche se i driver si trovano in due diversi ambienti o connessioni. Se gli argomenti *SourceDescHandle* e *TargetDescHandle* sono associati a driver diversi, gestione driver copia i campi definiti da ODBC, ma non copia i campi o i campi definiti dal driver che non sono definiti da ODBC per il tipo di descrittore.  
  
 La chiamata a **SQLCopyDesc** viene interrotta immediatamente se si verifica un errore.  
  
 Quando viene copiato il campo SQL_DESC_DATA_PTR, viene eseguita una verifica di coerenza sul descrittore di destinazione. Se la verifica di coerenza ha esito negativo, viene restituito SQLSTATE HY021 (informazioni sul descrittore incoerenti) e la chiamata a **SQLCopyDesc** viene immediatamente interrotta. Per ulteriori informazioni sulle verifiche di coerenza, vedere "verifiche di coerenza" nella [funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 Gli handle descrittore possono essere copiati tra le connessioni anche se le connessioni si trovano in ambienti diversi. Se Gestione driver rileva che il descrittore di origine e quello di destinazione non appartengono alla stessa connessione e le due connessioni appartengono a driver distinti, implementa **SQLCopyDesc** eseguendo una copia Field-by-Field usando **SQLGetDescField** e **SQLSetDescField**.  
  
 Quando **SQLCopyDesc** viene chiamato con un *SourceDescHandle* su un driver e un *TargetDescHandle* in un altro driver, la coda degli errori di *SourceDescHandle* viene cancellata. Questo problema si verifica perché **SQLCopyDesc** in questo caso viene implementato dalle chiamate a **SQLGetDescField** e **SQLSetDescField**.  
  
> [!NOTE]  
>  Un'applicazione potrebbe essere in grado di associare un handle descrittore allocato in modo esplicito con un *statementHandle*, invece di chiamare **SQLCopyDesc** per copiare i campi da un descrittore a un altro. Un descrittore allocato in modo esplicito può essere associato a un altro *statementHandle* nello stesso *connectionHandle* impostando l'attributo dell'istruzione SQL_ATTR_APP_ROW_DESC o SQL_ATTR_APP_PARAM_DESC sull'handle del descrittore allocato in modo esplicito. Al termine di questa operazione, non è necessario chiamare **SQLCopyDesc** per copiare i valori dei campi del descrittore da un descrittore a un altro. Non è tuttavia possibile associare un handle del descrittore a un *statementHandle* in un altro *connectionHandle*. per usare gli stessi valori di campo del descrittore in *StatementHandles* in *ConnectionHandles* diversi, è necessario chiamare **SQLCopyDesc** .  
  
 Per una descrizione dei campi in un'intestazione o in un record del descrittore, vedere [funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Per ulteriori informazioni sui descrittori, vedere [descrittori](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="copying-rows-between-tables"></a>Copia di righe tra tabelle  
 Un'applicazione può copiare dati da una tabella all'altra senza copiare i dati a livello di applicazione. A tale scopo, l'applicazione associa gli stessi buffer di dati e le informazioni sul descrittore a un'istruzione che recupera i dati e l'istruzione che inserisce i dati in una copia. Questa operazione può essere eseguita condividendo un descrittore dell'applicazione (associazione di un descrittore allocato in modo esplicito come ARD a un'istruzione e APD in un altro) oppure usando **SQLCopyDesc** per copiare i binding tra ARD e l'APD delle due istruzioni. Se le istruzioni sono in connessioni diverse, è necessario usare **SQLCopyDesc** . Inoltre, **SQLCopyDesc** deve essere chiamato per copiare i binding tra IRD e il DPI delle due istruzioni. Quando si esegue la copia tra istruzioni nella stessa connessione, il SQL_ACTIVE_STATEMENTS tipo di informazioni restituito dal driver per una chiamata a **SQLGetInfo** deve essere maggiore di 1 affinché questa operazione abbia esito positivo. Questo non avviene quando si esegue la copia tra le connessioni.  
  
### <a name="code-example"></a>Esempio di codice  
 Nell'esempio seguente vengono usate le operazioni del descrittore per copiare i campi della tabella PartsSource nella tabella PartsCopy. Il contenuto della tabella PartsSource viene recuperato nei buffer del set di righe in *hstmt0*. Questi valori vengono usati come parametri di un'istruzione INSERT su *hstmt1* per popolare le colonne della tabella PartsCopy. A tale scopo, i campi di IRD di *hstmt0* vengono copiati nei campi del dispositivo dpi di *hstmt1* e i campi di ARD di *hstmt0* vengono copiati nei campi dell'APD di *hstmt1*. Usare **SQLSetDescField** per impostare l'attributo SQL_DESC_PARAMETER_TYPE di dpi su SQL_PARAM_INPUT quando si copiano i campi IRD da un'istruzione con parametri di output a campi dpi che devono essere parametri di input.  
  
```cpp  
#define ROWS 100  
#define DESC_LEN 50  
#define SQL_SUCCEEDED(rc) (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO)  
  
// Template for a row  
typedef struct {  
   SQLINTEGER   sPartID;  
   SQLINTEGER   cbPartID;  
   SQLUCHAR     szDescription[DESC_LENGTH];  
   SQLINTEGER   cbDescription;  
   REAL         sPrice;  
   SQLINTEGER   cbPrice;  
} PartsSource;  
  
PartsSource    rget[ROWS];          // rowset buffer  
SQLUSMALLINT   sts_ptr[ROWS];       // status pointer  
SQLHSTMT       hstmt0, hstmt1;  
SQLHDESC       hArd0, hIrd0, hApd1, hIpd1;  
  
// ARD and IRD of hstmt0  
SQLGetStmtAttr(hstmt0, SQL_ATTR_APP_ROW_DESC, &hArd0, 0, NULL);  
SQLGetStmtAttr(hstmt0, SQL_ATTR_IMP_ROW_DESC, &hIrd0, 0, NULL);  
  
// APD and IPD of hstmt1  
SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_PARAM_DESC, &hApd1, 0, NULL);  
SQLGetStmtAttr(hstmt1, SQL_ATTR_IMP_PARAM_DESC, &hIpd1, 0, NULL);  
  
// Use row-wise binding on hstmt0 to fetch rows  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER) sizeof(PartsSource), 0);  
  
// Set rowset size for hstmt0  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
  
// Execute a select statement  
SQLExecDirect(hstmt0, "SELECT PARTID, DESCRIPTION, PRICE FROM PARTS ORDER BY 3, 1, 2"",  
               SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt0, 1, SQL_C_SLONG, rget[0].sPartID, 0,   
   &rget[0].cbPartID);  
SQLBindCol(hstmt0, 2, SQL_C_CHAR, &rget[0].szDescription, DESC_LEN,   
   &rget[0].cbDescription);  
SQLBindCol(hstmt0, 3, SQL_C_FLOAT, rget[0].sPrice,   
   0, &rget[0].cbPrice);  
  
// Perform parameter bindings on hstmt1.   
SQLCopyDesc(hArd0, hApd1);  
SQLCopyDesc(hIrd0, hIpd1);  
  
// Set the array status pointer of IRD  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_STATUS_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the ARRAY_STATUS_PTR field of APD to be the same  
// as that in IRD.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_PARAM_OPERATION_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the hIpd1 records as input parameters  
rc = SQLSetDescField(hIpd1, 1, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 2, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 3, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
  
// Prepare an insert statement on hstmt1. PartsCopy is a copy of  
// PartsSource  
SQLPrepare(hstmt1, "INSERT INTO PARTS_COPY VALUES (?, ?, ?)", SQL_NTS);  
  
// In a loop, fetch a rowset, and copy the fetched rowset to PARTS_COPY  
  
rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
while (SQL_SUCCEEDED(rc)) {  
  
   // After the call to SQLFetchScroll, the status array has row   
   // statuses. This array is used as input status in the APD  
   // and hence determines which elements of the rowset buffer  
   // are inserted.  
   SQLExecute(hstmt1);  
  
   rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
} // while  
```  
  
### <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Recupero di più campi di descrizione|[Funzione SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Impostazione di un singolo campo del descrittore|[Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Impostazione di più campi di descrizione|[Funzione SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
