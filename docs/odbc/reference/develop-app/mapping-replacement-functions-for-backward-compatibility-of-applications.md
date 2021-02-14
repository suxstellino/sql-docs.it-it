---
description: Mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni
title: Mapping di funzioni di sostituzione per la compatibilità delle app-ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- mapping replacement functions [ODBC]
- upgrading applications [ODBC], mapping replacement functions
- compatibility [ODBC], mapping replacement functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], mapping replacement functions
- application upgrades [ODBC], mapping replacement functions
- backward compatibility [ODBC], mapping replacement functions
ms.assetid: f5e6d9da-76ef-42cb-b3f5-f640857df732
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4c31d523a9fb50dd45bf8ab56c551d3830b0861
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082162"
---
# <a name="mapping-replacement-functions-for-backward-compatibility-of-applications"></a>Mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni
Un'applicazione ODBC *3. x* che utilizza Gestione driver ODBC *3. x* funzionerà con un driver ODBC *2. x* , purché non vengano utilizzate nuove funzionalità. Le funzionalità duplicate e le modifiche comportamentali, tuttavia, influiscono sulla modalità di funzionamento dell'applicazione ODBC *3. x* su un driver ODBC *2. x* . Quando si utilizza un driver ODBC *2.* x, gestione driver esegue il mapping delle seguenti funzioni ODBC *3. x* , che hanno sostituito una o più funzioni ODBC *2. x* , nelle funzioni ODBC *2. x* corrispondenti.  
  
|Funzione ODBC *3. x*|Funzione ODBC *2. x*|  
|-------------------------|-------------------------|  
|**SQLAllocHandle**|**SQLAllocEnv**, **SQLAllocConnect** o **SQLAllocStmt**|  
|**SQLBulkOperations**|**SQLSetPos**|  
|**SQLColAttribute**|**SQLColAttributes**|  
|**SQLEndTran**|**SQLTransact**|  
|**SQLFetch**|**SQLExtendedFetch**|  
|**SQLFetchScroll**|**SQLExtendedFetch**|  
|**SQLFreeHandle**|**SQLFreeEnv**, **SQLFreeConnect** o **SQLFreeStmt**|  
|**SQLGetConnectAttr**|**SQLGetConnectOption**|  
|**SQLGetDiagRec**|**SQLError**|  
|**SQLGetStmtAttr**|**SQLGetStmtOption**[1]|  
|**SQLSetConnectAttr**|**SQLSetConnectOption**|  
|**SQLSetStmtAttr**|**SQLSetStmtOption**[1]|  
  
 [1] è possibile che vengano eseguite anche altre azioni, a seconda dell'attributo richiesto.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
 Gestione driver esegue il mapping a **SQLAllocEnv**, **SQLAllocConnect** o **SQLAllocStmt**, in base alle esigenze. La chiamata seguente a **SQLAllocHandle**:  
  
```  
SQLAllocHandle(HandleType, InputHandle, OutputHandlePtr);  
```  
  
 in Gestione driver verrà effettuato il mapping seguente (concettuale, nessun controllo degli errori):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLAllocEnv(OutputHandlePtr));  
   case SQL_HANDLE_DBC: return (SQLAllocConnect (InputHandle, OutputHandlePtr));  
   case SQL_HANDLE_STMT: return (SQLAllocStmt (InputHandle, OutputHandlePtr));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
 Gestione driver esegue il mapping a **SQLSetPos**. La chiamata seguente a **SQLBulkOperations**:  
  
```  
SQLBulkOperations(hstmt, Operation);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Se l'argomento Operation è SQL_ADD, gestione driver chiama **SQLSetPos** nel modo seguente:  
  
    ```  
    SQLSetPos (hstmt, 0, SQL_ADD, SQL_LOCK_NO_CHANGE);  
    ```  
  
2.  Se l'argomento Operation non è SQL_ADD, il driver restituisce SQLSTATE HY092 (identificatore di opzione/attributo non valido).  
  
3.  Se l'applicazione tenta di modificare il SQL_ATTR_ROW_STATUS_PTR tra le chiamate a **SQLFetch** o **SQLFetchScroll** e **SQLBulkOperations**, la gestione driver restituirà SQLState HY011 (non è possibile impostare l'attributo).  
  
4.  Se l'argomento Operation è SQL_ADD, l'applicazione deve chiamare **SQLBindCol** per associare i dati da inserire. Non è possibile chiamare **SQLSetDescField** o **SQLSetDescRec** per associare i dati da inserire.  
  
5.  Se l'argomento Operation è SQL_ADD e il numero di righe da inserire non corrisponde a quello delle dimensioni correnti del set di righe, è necessario chiamare **SQLSetStmtAttr** per impostare l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE sul numero di righe da inserire prima di chiamare **SQLBulkOperations**. Per ripristinare le dimensioni del set di righe precedenti, l'applicazione deve impostare l'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE prima della chiamata a **SQLFetch**, **SQLFetchScroll** o **SQLSetPos** .  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
 Gestione driver esegue il mapping a **SQLColAttributes**. La chiamata seguente a **SQLColAttribute**:  
  
```  
SQLColAttribute(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Se *FieldIdentifier* è uno dei seguenti:  
  
     SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH, SQL_DESC_OCTET_LENGTH, SQL_DESC_UNNAMED, SQL_DESC_BASE_COLUMN_NAME, SQL_DESC_LITERAL_PREFIX, SQL_DESC_LITERAL_SUFFIX o SQL_DESC_LOCAL_TYPE_NAME  
  
     Gestione driver restituisce SQL_ERROR con SQLSTATE HY091 (identificatore del campo del descrittore non valido). Non vengono applicate altre regole di questa sezione.  
  
2.  Gestione driver esegue il mapping di SQL_COLUMN_COUNT, SQL_COLUMN_NAME o SQL_COLUMN_NULLABLE SQL_DESC_COUNT, SQL_DESC_NAME o SQL_DESC_NULLABLE rispettivamente. (Un driver ODBC *2. x* deve supportare solo SQL_COLUMN_COUNT, SQL_COLUMN_NAME e SQL_COLUMN_NULLABLE, non SQL_DESC_COUNT, SQL_DESC_NAME e SQL_DESC_NULLABLE). Viene eseguito il mapping della chiamata a SQLColAttribute a:  
  
    ```  
    SQLColAttributes(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
    ```  
  
3.  Tutti gli altri valori di *FieldIdentifier* vengono passati al driver, con **SQLColAttribute** mappato a **SQLColAttributes** , come illustrato in precedenza.  
  
4.  Se *bufferLength* è minore di 0, Gestione Driver restituisce SQL_ERROR con SQLSTATE HY090 (lunghezza di stringa o di buffer non valida). Non vengono applicate altre regole di questa sezione.  
  
5.  Se *FieldIdentifier* è SQL_DESC_CONCISE_TYPE e il tipo restituito è un tipo di dati DateTime conciso, gestione driver esegue il mapping dei valori restituiti per i codici di data, ora e timestamp.  
  
6.  Gestione driver esegue le verifiche necessarie per verificare se deve essere generato il HY010 SQLSTATE (errore della sequenza di funzioni). In tal caso, gestione driver restituisce SQL_ERROR e SQLSTATE HY010 (errore della sequenza di funzioni). Non vengono applicate altre regole di questa sezione.  
  
## <a name="sqlendtran"></a>SQLEndTran  
 Gestione driver esegue il mapping di questo oggetto a **SQLTransact**. La chiamata seguente a **SQLEndTran**:  
  
```  
SQLEndTran(HandleType, Handle, CompletionType);  
```  
  
 in Gestione driver verrà effettuato il mapping seguente (concettuale, nessun controllo degli errori):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV:return(SQLTransact(Handle, SQL_NULL_HDBC, CompletionType));  
   case SQL_HANDLE_DBC:return(SQLTransact(SQL_NULL_HENV, Handle, CompletionType);  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlfetch"></a>SQLFetch  
 Gestione driver esegue il mapping di questo oggetto a **SQLExtendedFetch** con un argomento *FetchOrientation* di SQL_FETCH_NEXT. La chiamata seguente a **SQLFetch**:  
  
```  
SQLFetch (StatementHandle);  
```  
  
 la gestione driver chiamerà **SQLExtendedFetch**, come indicato di seguito:  
  
```  
rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, &RowCount, RowStatusArray);  
```  
  
 In questa chiamata, l'argomento *pcRow* è impostato sul valore che l'applicazione imposta l'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR su tramite una chiamata a **SQLSetStmtAttr**.  
  
> [!NOTE]  
>  Quando l'applicazione chiama **SQLSetStmtAttr** per impostare SQL_ATTR_ROW_STATUS_PTR in modo che punti a una matrice di stato, il puntatore viene memorizzato nella cache da Gestione driver. *RowStatusArray* può essere uguale a un puntatore null.  
  
 Se il driver non supporta **SQLExtendedFetch** e viene caricata la libreria di cursori, gestione driver usa il **SQLExtendedFetch** della libreria di cursori per eseguire il mapping di **SQLFetch** a **SQLExtendedFetch**. Se il driver non supporta **SQLExtendedFetch** e la libreria di cursori non è caricata, gestione driver passa la chiamata a **SQLFetch** tramite al driver. Se l'applicazione chiama **SQLSetStmtAttr** per impostare SQL_ATTR_ROW_STATUS_PTR, gestione driver garantisce che la matrice venga popolata. Se l'applicazione chiama **SQLSetStmtAttr** per impostare SQL_ATTR_ROWS_FETCHED_PTR, gestione driver imposta questo campo su 1.  
  
## <a name="sqlfetchscroll"></a>SQLFetchScroll  
 Gestione driver esegue il mapping a **SQLExtendedFetch**. La chiamata seguente a **SQLFetchScroll**:  
  
```  
SQLFetchScroll(StatementHandle, FetchOrientation, FetchOffset);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Quando l'applicazione chiama **SQLSetStmtAttr** per impostare SQL_ATTR_ROW_STATUS_PTR (che imposta il campo SQL_DESC_ARRAY_STATUS_PTR in IRD) in modo che punti a una matrice di stato, il puntatore viene memorizzato nella cache da Gestione driver. Lasciare che questo puntatore sia *RowStatusArray*; in caso contrario, lasciare che *RowStatusArray* sia uguale a un puntatore null. Se l'argomento *RowStatusArray* è impostato su un puntatore null, gestione driver genera una matrice di stato della riga.  
  
2.  Se *FetchOrientation* non è uno dei SQL_FETCH_NEXT, SQL_FETCH_PRIOR, SQL_FETCH_ABSOLUTE, SQL_FETCH_RELATIVE, SQL_FETCH_FIRST, SQL_FETCH_LAST o SQL_FETCH_BOOKMARK, gestione driver restituisce con SQL_ERROR e SQLSTATE HY106 (tipo di recupero non compreso nell'intervallo). Non vengono applicate altre regole di questa sezione.  
  
3.  Maiuscole/minuscole:  
  
-   Se *FetchOrientation* è uguale a SQL_FETCH_BOOKMARK,:  
  
    -   Se **SQLSetStmtAttr** è stato chiamato in precedenza per impostare il valore di SQL_ATTR_FETCH_BOOKMARK_PTR, lasciare che *BMK* sia il valore ottenuto dereferenziando il puntatore SQL_DESC_FETCH_BOOKMARK_PTR.  
  
    -   In caso contrario, restituire SQL_ERROR con SQLSTATE HY111 (valore di segnalibro non valido). Non vengono applicate altre regole di questa sezione.  
  
     Gestione driver chiama ora **SQLExtendedFetch**, come indicato di seguito:  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, Bmk, pcRow, RowStatusArray);  
    ```  
  
-   In caso contrario, gestione driver chiama **SQLExtendedFetch**, come indicato di seguito:  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, pcRow, RowStatusArray);  
    ```  
  
     In queste chiamate, l'argomento *pcRow* è impostato sul valore che l'applicazione imposta l'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR su tramite una chiamata a **SQLSetStmtAttr**.  
  
-   SQL_ATTR_ROW_ARRAY_SIZE è mappato a SQL_ROWSET_SIZE.  
  
-   Se *RC* è uguale a SQL_SUCCESS o SQL_SUCCESS_WITH_INFO e se *FetchOrientation* è uguale a SQL_FETCH_BOOKMARK e *FetchOffset* è diverso da 0, gestione driver invia un avviso, SQLSTATE 01S10 (tentativo di recupero tramite un offset di segnalibro, valore di offset ignorato) e restituisce SQL_SUCCESS_WITH_INFO.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 Gestione driver esegue il mapping a **SQLFreeEnv**, **SQLFreeConnect** o **SQLFreeStmt** in base alle esigenze. La chiamata seguente a **SQLFreeHandle**:  
  
```  
SQLFreeHandle(HandleType, Handle);  
```  
  
 in Gestione driver verrà effettuato il mapping seguente (concettuale, nessun controllo degli errori):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLFreeEnv(Handle));  
   case SQL_HANDLE_DBC: return (SQLFreeConnect(Handle));  
   case SQL_HANDLE_STMT: return (SQLFreeStmt(Handle, SQL_DROP));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
 Gestione driver esegue il mapping a **SQLGetConnectOption**. La chiamata seguente a **SQLGetConnectAttr**:  
  
```  
SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Se l' *attributo* non è un attributo di connessione o di istruzione definito dal driver e non è un attributo definito in ODBC *2. x*, gestione driver restituisce SQL_ERROR con SQLState HY092 (identificatore di opzione/attributo non valido). Non sono valide altre regole in questa sezione.  
  
2.  Se *attribute* è uguale a SQL_ATTR_AUTO_IPD o SQL_ATTR_METADATA_ID, Gestione Driver restituisce SQL_ERROR con SQLState HY092 (identificatore di opzione o attributo non valido).  
  
3.  Gestione driver esegue le verifiche necessarie per verificare se è necessario generare SQLSTATE 08003 (connessione non aperta) o SQLSTATE HY010 (errore della sequenza di funzioni). In tal caso, gestione driver restituisce SQL_ERROR e invia il messaggio di errore appropriato. Non vengono applicate altre regole di questa sezione.  
  
4.  Gestione driver chiama **SQLGetConnectOption** nel modo seguente:  
  
    ```  
    SQLGetConnectOption (ConnectionHandle, Attribute, ValuePtr);  
    ```  
  
     Si noti che *bufferLength* e *StringLengthPtr* vengono ignorati.  
  
## <a name="sqlgetdata"></a>SQLGetData  
 Quando un'applicazione ODBC *3. x* che utilizza un driver *ODBC 2. x* chiama **SQLGetData** con l'argomento *COLUMNNUMBER* uguale a 0, gestione driver ODBC *3. x* esegue il mapping di questo oggetto a una chiamata a **SQLGetStmtOption** con l'attributo *Option* impostato su SQL_GET_BOOKMARK.  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
 Gestione driver esegue il mapping a **SQLGetStmtOption**. La chiamata seguente a **SQLGetStmtAttr**:  
  
```  
SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Se l' *attributo* non è un attributo di connessione o di istruzione definito dal driver e non è un attributo definito in ODBC *2. x*, gestione driver restituisce SQL_ERROR con SQLState HY092 (identificatore di opzione/attributo non valido). Non sono valide altre regole in questa sezione.  
  
2.  Se *attribute* è uno dei seguenti:  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     Gestione driver restituisce SQL_ERROR con SQLSTATE HY092 (identificatore di opzione/attributo non valido). Non vengono applicate altre regole di questa sezione.  
  
3.  Gestione driver esegue le verifiche necessarie per verificare se deve essere generato il HY010 SQLSTATE (errore della sequenza di funzioni). In tal caso, gestione driver restituisce SQL_ERROR e SQLSTATE HY010 (errore della sequenza di funzioni). Non vengono applicate altre regole di questa sezione.  
  
4.  Se *attribute* è uguale a SQL_ATTR_ROWS_FETCHED_PTR, gestione driver restituisce un puntatore alla variabile di gestione driver interna *Crow*, che ha usato o userà in una chiamata a **SQLExtendedFetch**. Non vengono applicate altre regole di questa sezione.  
  
5.  Se *attribute* è uguale a SQL_DESC_FETCH_BOOKMARK_PTR, gestione driver restituisce il puntatore appropriato memorizzato nella cache durante una chiamata a **SQLSetStmtAttr**.  
  
6.  Se *attribute* è uguale a SQL_ATTR_ROW_STATUS_PTR, gestione driver restituisce il puntatore appropriato memorizzato nella cache durante una chiamata a **SQLSetStmtAttr**.  
  
7.  Gestione driver chiama **SQLGetStmtOption** nel modo seguente:  
  
    ```  
    SQLGetStmtOption (hstmt, fOption, pvParam);  
    ```  
  
     dove *HSTMT*, *fOption* e *pvParam* verranno impostati rispettivamente sui valori di *statementHandle*, *attribute* e *ValuePtr*. *BufferLength* e *StringLengthPtr* vengono ignorati.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 Gestione driver esegue il mapping a **SQLSetConnectOption**. La chiamata seguente a **SQLSetConnectAttr**:  
  
```  
SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, StringLength);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Se l' *attributo* non è un attributo di connessione o di istruzione definito dal driver e non è un attributo definito in ODBC *2. x*, gestione driver restituisce SQL_ERROR con SQLState HY092 (identificatore di opzione/attributo non valido). Non sono valide altre regole in questa sezione.  
  
2.  Se *attribute* è uguale a SQL_ATTR_AUTO_IPD, Gestione Driver restituisce SQL_ERROR con SQLState HY092 (identificatore di opzione/attributo non valido).  
  
3.  Gestione driver esegue le verifiche necessarie per verificare se è necessario generare SQLSTATE 08003 (connessione non aperta) o SQLSTATE HY010 (errore della sequenza di funzioni). Se è necessario generare uno di questi errori, gestione driver restituisce SQL_ERROR e invia il messaggio di errore appropriato. Non vengono applicate altre regole di questa sezione.  
  
4.  Gestione driver chiama **SQLSetConnectOption** nel modo seguente:  
  
    ```  
    SQLSetConnectOption (hdbc, fOption, vParam);  
    ```  
  
     dove *HDBC*, *fOption* e *parametro vParam* verranno impostati rispettivamente sui valori di *connectionHandle*, *attribute* e *ValuePtr*. *StringLengthPtr* viene ignorato.  
  
> [!NOTE]  
>  La possibilità di impostare gli attributi di istruzione a livello di connessione è stata deprecata. Gli attributi di istruzione non devono mai essere impostati a livello di connessione da un'applicazione ODBC *3. x* .  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Gestione driver esegue il mapping a **SQLSetStmtOption**. La chiamata seguente a **SQLSetStmtAttr**:  
  
```  
SQLSetStmtAttr(StatementHandle, Attribute, ValuePtr, StringLength);  
```  
  
 comporterà la sequenza di passaggi seguente:  
  
1.  Se l' *attributo* non è un attributo di connessione o di istruzione definito dal driver e non è un attributo definito in ODBC *2. x*, gestione driver restituisce SQL_ERROR con SQLState HY092 (identificatore di opzione/attributo non valido). Non sono valide altre regole in questa sezione.  
  
2.  Se *attribute* è uno dei seguenti:  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     Gestione driver restituisce SQL_ERROR con SQLSTATE HY092 (identificatore di opzione/attributo non valido). Non vengono applicate altre regole di questa sezione.  
  
3.  Gestione driver esegue i controlli necessari per verificare se devono essere generati HY010 SQLSTATE (errore della sequenza di funzioni). In tal caso, gestione driver restituisce SQL_ERROR e SQLSTATE HY010 (errore della sequenza di funzioni). Non vengono applicate altre regole di questa sezione.  
  
4.  Se *attribute* è uguale a SQL_ATTR_PARAMSET_SIZE o SQL_ATTR_PARAMS_PROCESSED_PTR, vedere la sezione "mapping per la gestione delle matrici di parametri" più avanti in questo argomento. Non vengono applicate altre regole di questa sezione.  
  
5.  Se *attribute* è uguale a SQL_ATTR_ROWS_FETCHED_PTR, gestione driver memorizza nella cache il valore del puntatore per un uso successivo con **SQLFetchScroll**.  
  
6.  Se *attribute* è uguale a SQL_ATTR_ROW_STATUS_PTR, gestione driver memorizza nella cache il valore del puntatore per un uso successivo con **SQLFetchScroll** o **SQLSetPos**. Non vengono applicate altre regole di questa sezione.  
  
7.  Se *attribute* è uguale a SQL_ATTR_FETCH_BOOKMARK_PTR, gestione driver memorizza nella cache *ValuePtr* e utilizzerà il valore memorizzato nella cache in un secondo momento in una chiamata a **SQLFetchScroll**. Non vengono applicate altre regole di questa sezione.  
  
8.  Gestione driver chiama **SQLSetStmtOption** nel modo seguente:  
  
    ```  
    SQLSetStmtOption (hstmt, fOption, vParam);  
    ```  
  
     dove *HSTMT*, *fOption* e *parametro vParam* verranno impostati rispettivamente sui valori di *statementHandle*, *attribute* e *ValuePtr*. L'argomento *StringLength* viene ignorato.  
  
     Se un driver ODBC *2. x* supporta le opzioni per le istruzioni specifiche del driver e delle stringhe di caratteri, un'applicazione ODBC *3. x* deve chiamare **SQLSetStmtOption** per impostare tali opzioni.  
  
## <a name="mappings-for-handling-parameter-arrays"></a>Mapping per la gestione delle matrici di parametri  
 Quando l'applicazione chiama:  
  
```  
SQLSetStmtAttr (StatementHandle, SQL_ATTR_PARAMSET_SIZE, Size, StringLength);  
```  
  
 Gestione driver chiama:  
  
```  
SQLParamOptions (StatementHandle, Size, &RowCount);  
```  
  
 Gestione driver restituisce successivamente un puntatore a questa variabile quando l'applicazione chiama **SQLGetStmtAttr** per recuperare SQL_ATTR_PARAMS_PROCESSED_PTR. Gestione driver non è in grado di modificare questa variabile interna finché l'handle di istruzione non viene restituito allo stato preparato o allocato.  
  
 Un'applicazione ODBC *3. x* può chiamare **SQLGetStmtAttr** per ottenere il valore di SQL_ATTR_PARAMS_PROCESSED_PTR anche se non ha impostato in modo esplicito il campo SQL_DESC_ARRAY_SIZE nell'APD. Questa situazione può verificarsi, ad esempio, se l'applicazione dispone di una routine generica che verifica la presenza della "riga" corrente dei parametri elaborati quando **SQLExecute** restituisce SQL_NEED_DATA. Questa routine viene richiamata indipendentemente dal fatto che il SQL_DESC_ARRAY_SIZE sia 1 o maggiore di 1. Per tenere conto di questo problema, gestione driver dovrà definire questa variabile interna indipendentemente dal fatto che l'applicazione abbia chiamato **SQLSetStmtAttr** per impostare il campo SQL_DESC_ARRAY_SIZE in APD. Se SQL_DESC_ARRAY_SIZE non è stato impostato, gestione driver deve assicurarsi che la variabile contenga il valore 1 prima di restituire da **SQLExecDirect** o **SQLExecute**.  
  
## <a name="error-handling"></a>Gestione degli errori  
 In ODBC *3. x*, la chiamata a **SQLFetch** o **SQLFETCHSCROLL** popola il SQL_DESC_ARRAY_STATUS_PTR in IRD e il campo SQL_DIAG_ROW_NUMBER di un record di diagnostica specificato contiene il numero della riga nel set di righe a cui si riferisce questo record. Utilizzando questa operazione, l'applicazione può correlare un messaggio di errore con una determinata posizione di riga.  
  
 Un driver ODBC *2. x* non sarà in grado di fornire questa funzionalità. Tuttavia, fornirà la delimitazione degli errori con SQLSTATE 01S01 (errore nella riga). Un'applicazione ODBC *3. x* che utilizza **SQLFetch** o **SQLFetchScroll** mentre è in corso un driver ODBC *2. x* deve essere a conoscenza di questo fatto. Si noti inoltre che tale applicazione non sarà in grado di chiamare **SQLGetDiagField** per ottenere effettivamente il campo SQL_DIAG_ROW_NUMBER. Un'applicazione ODBC *3. x* che utilizza un driver ODBC *2. x* sarà in grado di chiamare **SQLGetDiagField** solo con un argomento *DiagIdentifier* di SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_RETURNCODE o SQL_DIAG_SQLSTATE. Gestione driver ODBC *3. x* gestisce la struttura dei dati di diagnostica quando si utilizza un driver ODBC *2. x* , ma il driver ODBC *2. x* restituisce solo questi quattro campi.  
  
 Quando un'applicazione ODBC *2. x* utilizza un driver ODBC *2. x* , se un'operazione può causare la restituzione di più errori da parte di gestione driver, è possibile che vengano restituiti errori diversi da Gestione driver ODBC *3. x* rispetto a gestione driver ODBC *2. x* .  
  
## <a name="mappings-for-bookmark-operations"></a>Mapping per le operazioni di segnalibro  
 Gestione driver ODBC *3. x* esegue i mapping seguenti quando un'applicazione ODBC *3. x* che utilizza un driver ODBC *2. x* esegue operazioni sui segnalibri.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Quando un'applicazione *ODBC 3. x* che utilizza un driver ODBC *2. x* chiama **SQLBindCol** per l'associazione alla colonna 0 con *FCTYPE* uguale a SQL_C_VARBOOKMARK, gestione driver ODBC *3. x* verifica se l'argomento *bufferLength* è minore di 4 o maggiore di 4 e, in caso affermativo, restituisce SQLSTATE HY090 (lunghezza di stringa o di buffer non valida). Se l'argomento *bufferLength* è uguale a 4, gestione driver chiama **SQLBindCol** nel driver, dopo aver sostituito *fCType* con SQL_C_BOOKMARK.  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Quando un'applicazione ODBC *3. x* che utilizza un driver *ODBC 2. x* chiama **SQLColAttribute** con l'argomento *ColumnNumber* impostato su 0, gestione driver restituisce i valori *FieldIdentifier* elencati nella tabella seguente.  
  
|*FieldIdentifier*|Valore|  
|-----------------------|-----------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|  
|SQL_DESC_CATALOG_NAME|"" (stringa vuota)|  
|SQL_DESC_CONCISE_TYPE|SQL_BINARY|  
|SQL_DESC_COUNT|Lo stesso valore restituito da **SQLNumResultCols**|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|  
|SQL_DESC_DISPLAY_SIZE|8|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|  
|SQL_DESC_LABEL|"" (stringa vuota)|  
|SQL_DESC_LENGTH|0|  
|SQL_DESC_LITERAL_PREFIX|"" (stringa vuota)|  
|SQL_DESC_LITERAL_SUFFIX|"" (stringa vuota)|  
|SQL_DESC_LOCAL_TYPE_NAME|"" (stringa vuota)|  
|SQL_DESC_NAME|"" (stringa vuota)|  
|SQL_DESC_NULLABLE|SQL_NO_NULLS|  
|SQL_DESC_OCTET_LENGTH|4|  
|SQL_DESC_PRECISION|4|  
|SQL_DESC_SCALE|0|  
|SQL_DESC_SCHEMA_NAME|"" (stringa vuota)|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|  
|SQL_DESC_TABLE_NAME|"" (stringa vuota)|  
|SQL_DESC_TYPE|SQL_BINARY|  
|SQL_DESC_TYPE_NAME|"" (stringa vuota)|  
|SQL_DESC_UNNAMED|SQL_UNNAMED|  
|SQL_DESC_UNSIGNED|SQL_FALSE|  
|SQL_DESC_UPDATEABLE|SQL_ATTR_READ_ONLY|  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Quando un'applicazione ODBC *3. x* che utilizza un driver ODBC *2. x* chiama **SQLDescribeCol** con l'argomento *ColumnNumber* impostato su 0, gestione driver restituisce i valori elencati nella tabella seguente.  
  
|Buffer|Valore|  
|------------|-----------|  
|ColumnName|"" (stringa vuota)|  
|*NameLengthPtr|0|  
|*DataTypePtr|SQL_BINARY|  
|*ColumnSizePtr|4|  
|*DecimalDigitsPtr|0|  
|*NullablePtr|SQL_NO_NULLS|  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Quando un'applicazione ODBC *3. x* che utilizza un driver ODBC *2. x* effettua la chiamata seguente a **SQLGetData** per recuperare un segnalibro:  
  
```  
SQLGetData(StatementHandle, 0, SQL_C_VARBOOKMARK, TargetValuePtr, BufferLength, StrLen_or_IndPtr)  
```  
  
 viene eseguito il mapping della chiamata a **SQLGetStmtOption** con un *fOption* di SQL_GET_BOOKMARK, come indicato di seguito:  
  
```  
SQLGetStmtOption(hstmt, SQL_GET_BOOKMARK, TargetValuePtr)  
```  
  
 dove *HSTMT* e *pvParam* sono impostati rispettivamente sui valori in *statementHandle* e *TargetValuePtr*. Il segnalibro viene restituito nel buffer a cui punta l'argomento *pvParam* (*TargetValuePtr*). Il valore del buffer a cui punta l'argomento *StrLen_or_IndPtr* nella chiamata a **SQLGetData** è impostato su 4.  
  
 Questo mapping è necessario per tenere conto del caso in cui **SQLFetch** è stato chiamato prima della chiamata a **SQLGetData** e il driver ODBC *2. x* non supporta **SQLExtendedFetch**. In questo caso, **SQLFetch** viene passato al driver ODBC *2. x* , nel qual caso il recupero dei segnalibri non è supportato.  
  
 **SQLGetData** non può essere chiamato più volte in un driver ODBC *2. x* per recuperare un segnalibro in parti, quindi la chiamata di **SQLGetData** con l'argomento *bufferLength* impostato su un valore minore di 4 e l'argomento *ColumnNumber* impostato su 0 restituiranno SQLSTATE HY090 (lunghezza di stringa o di buffer non valida). **SQLGetData** può, tuttavia, essere chiamato più volte per recuperare lo stesso segnalibro.  
  
### <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Quando un'applicazione ODBC *3. x* che utilizza un driver ODBC *2. x* chiama **SQLSetStmtAttr** per impostare l'attributo SQL_ATTR_USE_BOOKMARKS su SQL_UB_VARIABLE, gestione driver imposta l'attributo su SQL_UB_ON nel driver ODBC *2. x* sottostante.
