---
description: Funzione SQLAllocHandle
title: Funzione SQLAllocHandle | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLAllocHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLAllocHandle
helpviewer_keywords:
- SQLAllocHandle function [ODBC]
ms.assetid: 6e7fe420-8cf4-4e72-8dad-212affaff317
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d546ddfa5f7e18365fec4bdff08032a8bba749f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184331"
---
# <a name="sqlallochandle-function"></a>Funzione SQLAllocHandle
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLAllocHandle** alloca un handle di ambiente, connessione, istruzione o descrittore.  
  
> [!NOTE]  
>  Questa funzione è una funzione generica per l'allocazione di handle che sostituisce le funzioni ODBC 2,0 **SQLAllocConnect**, **SQLAllocEnv** e **SQLAllocStmt**. Per consentire alle applicazioni che chiamano **SQLAllocHandle** di funzionare con ODBC 2. driver *x* , una chiamata a **SQLAllocHandle** viene mappata in Gestione driver a **SQLAllocConnect**, **SQLAllocEnv** o **SQLAllocStmt**, a seconda dei casi. Per ulteriori informazioni, vedere "Commenti". Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando ODBC 3. l'applicazione *x* funziona con ODBC 2. driver *x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLAllocHandle(  
      SQLSMALLINT   HandleType,  
      SQLHANDLE     InputHandle,  
      SQLHANDLE *   OutputHandlePtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HandleType*  
 Input Tipo di handle che deve essere allocato da **SQLAllocHandle**. Deve essere uno dei valori seguenti:   
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN handle viene utilizzato solo dal driver e dalla gestione driver. Le applicazioni non devono usare questo tipo di handle. Per ulteriori informazioni su SQL_HANDLE_DBC_INFO_TOKEN, vedere [sviluppo di Connection-Pool awareness in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *InputHandle puntare*  
 Input Handle di input nel cui contesto è necessario allocare il nuovo handle. Se *HandleType* è SQL_HANDLE_ENV, questo è SQL_NULL_HANDLE. Se *HandleType* è SQL_HANDLE_DBC, deve essere un handle di ambiente e, se è SQL_HANDLE_STMT o SQL_HANDLE_DESC, deve essere un handle di connessione.  
  
 *OutputHandlePtr*  
 Output Puntatore a un buffer in cui restituire l'handle per la struttura di dati appena allocata.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_INVALID_HANDLE o SQL_ERROR.  
  
 Quando si alloca un handle diverso da un handle di ambiente, se **SQLAllocHandle** restituisce SQL_ERROR, imposta *OutputHandlePtr* su SQL_NULL_HDBC, SQL_NULL_HSTMT o SQL_NULL_HDESC, a seconda del valore di *HandleType*, a meno che l'argomento di output non sia un puntatore null. L'applicazione può quindi ottenere informazioni aggiuntive dalla struttura dei dati di diagnostica associata all'handle nell'argomento *InputHandle puntare* .  
  
## <a name="environment-handle-allocation-errors"></a>Errori di allocazione dell'handle di ambiente  
 L'allocazione dell'ambiente si verifica sia all'interno di gestione driver che all'interno di ogni driver. L'errore restituito da **SQLAllocHandle** con *HandleType* SQL_HANDLE_ENV dipende dal livello in cui si è verificato l'errore.  
  
 Se Gestione driver non è in grado di allocare memoria per *\* OutputHandlePtr* quando viene chiamato **SQLAllocHandle** con *HandleType* di SQL_HANDLE_ENV oppure se l'applicazione fornisce un puntatore null per *OutputHandlePtr*, **SQLAllocHandle** restituisce SQL_ERROR. Gestione driver imposta **OutputHandlePtr* su SQL_NULL_HENV, a meno che l'applicazione non abbia fornito un puntatore null, che restituisce SQL_ERROR. Nessun handle con cui associare ulteriori informazioni di diagnostica.  
  
 Gestione driver non chiama la funzione di allocazione di handle di ambiente a livello di driver finché l'applicazione non chiama **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect**. Se si verifica un errore nella funzione **SQLAllocHandle** a livello di driver, la funzione **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect** a livello di gestione driver restituisce SQL_ERROR. La struttura dei dati di diagnostica contiene SQLSTATE IM004 ( **SQLAllocHandle** del driver non riuscita). L'errore viene restituito in un handle di connessione.  
  
 Per ulteriori informazioni sul flusso di chiamate di funzione tra Gestione driver e un driver, vedere [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLAllocHandle** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con la *HandleType* e l' *handle* appropriati impostati sul valore di *InputHandle puntare*. È possibile restituire SQL_SUCCESS_WITH_INFO (ma non SQL_ERROR) per l'argomento *OutputHandle* . Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLAllocHandle** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08003|Connessione non aperta|(DM) l'argomento *HandleType* è stato SQL_HANDLE_STMT o SQL_HANDLE_DESC, ma la connessione specificata dall'argomento *InputHandle puntare* non è aperta. È necessario completare correttamente il processo di connessione (e la connessione deve essere aperta) affinché il driver allochi un handle di istruzione o descrittore.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer **MessageText* descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|(DM) Gestione driver non è in grado di allocare memoria per l'handle specificato.<br /><br /> Il driver non è stato in grado di allocare memoria per l'handle specificato.|  
|HY009|Uso non valido del puntatore null|(DM) l'argomento *OutputHandlePtr* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) l'argomento *HandleType* è stato SQL_HANDLE_DBC e **SQLSetEnvAttr** non è stato chiamato per impostare l'attributo dell'ambiente SQL_ODBC_VERSION.<br /><br /> (DM) è stata chiamata una funzione in esecuzione asincrona per **InputHandle puntare** ed è stata ancora eseguita quando è stata chiamata la funzione **SQLAllocHandle** con **HandleType** impostato su SQL_HANDLE_STMT o SQL_HANDLE_DESC.|  
|HY013|Errore di gestione della memoria|L'argomento *HandleType* è SQL_HANDLE_DBC, SQL_HANDLE_STMT o SQL_HANDLE_DESC; non è stato possibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti di memoria sottostanti, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY014|È stato superato il limite per il numero di handle|È stato raggiunto il limite definito dal driver per il numero di handle che è possibile allocare per il tipo di handle indicato dall'argomento *HandleType* .|  
|HY092|Identificatore di attributo/opzione non valido|(DM) l'argomento *HandleType* non era: SQL_HANDLE_ENV, SQL_HANDLE_DBC, SQL_HANDLE_STMT o SQL_HANDLE_DESC.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|L'argomento *HandleType* è stato SQL_HANDLE_DESC e il driver era ODBC 2. driver *x* .|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) l'argomento *HandleType* è stato SQL_HANDLE_STMT e il driver non è un driver ODBC valido.<br /><br /> (DM) l'argomento *HandleType* è stato SQL_HANDLE_DESC e il driver non supporta l'allocazione di un handle di descrittore.|  
  
## <a name="comments"></a>Commenti  
 **SQLAllocHandle** viene utilizzato per allocare gli handle per gli ambienti, le connessioni, le istruzioni e i descrittori, come descritto nelle sezioni riportate di seguito. Per informazioni generali sugli handle, vedere [Handles](../../../odbc/reference/develop-app/handles.md).  
  
 Più di un ambiente, una connessione o un handle di istruzione può essere allocato da un'applicazione per volta se più allocazioni sono supportate dal driver. In ODBC non è definito alcun limite per il numero di handle di ambiente, connessione, istruzione o descrittore che possono essere allocati in un momento qualsiasi. I driver possono imporre un limite al numero di un determinato tipo di handle che può essere allocato alla volta. Per ulteriori informazioni, vedere la documentazione del driver.  
  
 Se l'applicazione chiama **SQLAllocHandle** con *\* OutputHandlePtr* impostato su un ambiente, una connessione, un'istruzione o un handle descrittore già esistente, il driver sovrascrive le informazioni associate all' *handle*, a meno che l'applicazione non usi il pool di connessioni (vedere "allocazione di un attributo di ambiente per il pool di connessioni" più avanti in questa sezione). Gestione driver non controlla se l' *handle* immesso in *\* OutputHandlePtr* è già in uso, né controlla il contenuto precedente di un handle prima di sovrascriverlo.  
  
> [!NOTE]  
>  La programmazione di applicazioni ODBC non è corretta per chiamare **SQLAllocHandle** due volte con la stessa variabile dell'applicazione definita per *\* OutputHandlePtr* senza chiamare **SQLFreeHandle** per liberare l'handle prima della riallocazione. La sovrascrittura degli handle ODBC in questo modo può causare un comportamento incoerente o errori della parte dei driver ODBC.  
  
 Nei sistemi operativi che supportano più thread, le applicazioni possono utilizzare lo stesso ambiente, connessione, istruzione o handle descrittore su thread diversi. I driver devono pertanto supportare l'accesso sicuro, multithread a queste informazioni; un modo per ottenere questo risultato, ad esempio, consiste nell'usare una sezione critica o un semaforo. Per ulteriori informazioni sul threading, vedere [multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
 **SQLAllocHandle** non imposta l'attributo di ambiente SQL_ATTR_ODBC_VERSION quando viene chiamato per allocare un handle di ambiente. l'attributo Environment deve essere impostato dall'applicazione. in caso di chiamata a **SQLAllocHandle** per l'allocazione di un handle di connessione, verrà restituito il valore SQLSTATE HY010 (errore della sequenza di funzioni).  
  
 Per le applicazioni conformi agli standard, **SQLAllocHandle** viene mappato a **SQLAllocHandleStd** in fase di compilazione. La differenza tra queste due funzioni è che **SQLAllocHandleStd** imposta l'attributo SQL_ATTR_ODBC_VERSION environment su SQL_OV_ODBC3 quando viene chiamato con l'argomento *HandleType* impostato su SQL_HANDLE_ENV. Questa operazione viene eseguita perché le applicazioni conformi agli standard sono sempre ODBC 3. applicazioni *x* . Inoltre, gli standard non richiedono la registrazione della versione dell'applicazione. Questa è l'unica differenza tra queste due funzioni; in caso contrario, sono identici. **SQLAllocHandleStd** viene mappato a **SQLAllocHandle** all'interno di gestione driver. Pertanto, i driver di terze parti non devono implementare **SQLAllocHandleStd**.  
  
 Le applicazioni ODBC 3,8 devono usare:  
  
-   **SQLAllocHandle e non SQLAllocHandleStd** per allocare un handle di ambiente.  
  
-   **SQLSetEnvAttr** per impostare l'attributo SQL_ATTR_ODBC_VERSION environment su SQL_OV_ODBC3_80.  
  
## <a name="allocating-an-environment-handle"></a>Allocazione di un handle di ambiente  
 Un handle di ambiente fornisce l'accesso a informazioni globali, ad esempio handle di connessione validi e handle di connessione attivi. Per informazioni generali sugli handle di ambiente, vedere [handle di ambiente](../../../odbc/reference/develop-app/environment-handles.md).  
  
 Per richiedere un handle di ambiente, un'applicazione chiama **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_ENV e un *InputHandle puntare* di SQL_NULL_HANDLE. Il driver alloca memoria per le informazioni sull'ambiente e passa di nuovo il valore dell'handle associato nell'argomento *\* OutputHandlePtr* . L'applicazione passa il valore *\* OutputHandle* in tutte le chiamate successive che richiedono un argomento dell'handle di ambiente. Per altre informazioni, vedere [allocazione dell'handle di ambiente](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 In un handle di ambiente di gestione driver, se esiste già un handle di ambiente di un driver, **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_ENV non viene chiamato in tale driver quando viene effettuata una connessione, solo **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_DBC. Se il punto di controllo dell'ambiente di un driver non esiste nell'handle di ambiente di gestione driver, entrambi SQLAllocHandle con HandleType SQL_HANDLE_ENV e SQLAllocHandle con HandleType di SQL_HANDLE_DBC vengono chiamati nel driver quando il primo handle di connessione dell'ambiente è connesso al driver.  
  
 Quando Gestione driver elabora la funzione **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_ENV, verifica la parola chiave **Trace** nella sezione [ODBC] delle informazioni sul sistema. Se è impostato su 1, gestione driver Abilita la traccia per l'applicazione corrente. Se il flag di traccia è impostato, la traccia viene avviata quando viene allocato il primo handle di ambiente e termina quando viene liberato l'ultimo handle di ambiente. Per altre informazioni, vedere [Configuring Data Sources](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Dopo l'allocazione di un handle di ambiente, un'applicazione deve chiamare **SQLSetEnvAttr** sull'handle di ambiente per impostare l'attributo di ambiente SQL_ATTR_ODBC_VERSION. Se questo attributo non viene impostato prima della chiamata di **SQLAllocHandle** per allocare un handle di connessione nell'ambiente, la chiamata per allocare la connessione restituirà SQLSTATE HY010 (errore della sequenza di funzioni). Per ulteriori informazioni, vedere [dichiarazione della versione ODBC dell'applicazione](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md).  
  
## <a name="allocating-shared-environments-for-connection-pooling"></a>Allocazione di ambienti condivisi per il pool di connessioni  
 Gli ambienti possono essere condivisi tra più componenti in un singolo processo. Un ambiente condiviso può essere utilizzato da più di un componente nello stesso momento. Quando un componente utilizza un ambiente condiviso, può utilizzare le connessioni in pool, in modo da consentire l'allocazione e l'utilizzo di una connessione esistente senza ricreare tale connessione.  
  
 Prima di allocare un ambiente condiviso che può essere usato per il pool di connessioni, un'applicazione deve chiamare **SQLSetEnvAttr** per impostare l'attributo di ambiente SQL_ATTR_CONNECTION_POOLING su SQL_CP_ONE_PER_DRIVER o SQL_CP_ONE_PER_HENV. **SQLSetEnvAttr** in questo caso viene chiamato con *EnvironmentHandle* impostato su null, che rende l'attributo un attributo a livello di processo.  
  
 Dopo l'abilitazione del pool di connessioni, un'applicazione chiama **SQLAllocHandle** con l'argomento *HandleType* impostato su SQL_HANDLE_ENV. L'ambiente allocato da questa chiamata sarà un ambiente condiviso implicito perché il pool di connessioni è stato abilitato.  
  
 Quando viene allocato un ambiente condiviso, l'ambiente che verrà utilizzato non viene determinato fino a quando non viene chiamato **SQLAllocHandle** con *HandleType* SQL_HANDLE_DBC. A questo punto, gestione driver tenta di trovare un ambiente esistente corrispondente agli attributi dell'ambiente richiesti dall'applicazione. Se non esiste alcun ambiente di questo tipo, ne viene creato uno come ambiente condiviso. Gestione driver mantiene un conteggio dei riferimenti per ogni ambiente condiviso; il conteggio viene impostato su 1 quando l'ambiente viene creato per la prima volta. Se viene trovato un ambiente corrispondente, l'handle di tale ambiente viene restituito all'applicazione e il conteggio dei riferimenti viene incrementato. Un handle di ambiente allocato in questo modo può essere utilizzato in qualsiasi funzione ODBC che accetta un handle di ambiente come argomento di input.  
  
## <a name="allocating-a-connection-handle"></a>Allocazione di un handle di connessione  
 Un handle di connessione fornisce l'accesso a informazioni quali gli handle di istruzione e descrittore validi per la connessione e se una transazione è attualmente aperta. Per informazioni generali sugli handle di connessione, vedere [handle di connessione](../../../odbc/reference/develop-app/connection-handles.md).  
  
 Per richiedere un handle di connessione, un'applicazione chiama **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_DBC. L'argomento *InputHandle puntare* è impostato sull'handle di ambiente restituito dalla chiamata a **SQLAllocHandle** che ha allocato tale handle. Il driver alloca memoria per le informazioni di connessione e passa di nuovo il valore dell'handle associato in *\* OutputHandlePtr*. L'applicazione passa il valore *\* OutputHandlePtr* in tutte le chiamate successive che richiedono un handle di connessione. Per ulteriori informazioni, vedere [allocazione di un handle di connessione](../../../odbc/reference/develop-app/allocating-a-connection-handle-odbc.md).  
  
 Gestione driver elabora la funzione **SQLAllocHandle** e chiama la funzione **SQLAllocHandle** del driver quando l'applicazione chiama **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect**. Per ulteriori informazioni, vedere [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Se l'attributo SQL_ATTR_ODBC_VERSION Environment non è impostato prima della chiamata di **SQLAllocHandle** per allocare un handle di connessione nell'ambiente, la chiamata per allocare la connessione restituirà SQLSTATE HY010 (errore della sequenza di funzioni).  
  
 Quando un'applicazione chiama **SQLAllocHandle** con l'argomento *inputhandle puntare* impostato su SQL_HANDLE_DBC e anche su un handle di ambiente condiviso, gestione driver tenta di trovare un ambiente condiviso esistente corrispondente agli attributi dell'ambiente impostati dall'applicazione. Se non esiste alcun ambiente di questo tipo, ne viene creato uno con un conteggio dei riferimenti (gestito da Gestione driver) di 1. Se viene trovato un ambiente condiviso corrispondente, tale handle viene restituito all'applicazione e il relativo conteggio dei riferimenti viene incrementato.  
  
 La connessione effettiva che verrà utilizzata non è determinata da Gestione driver fino a quando non viene chiamato **SQLConnect** o **SQLDriverConnect** . Gestione driver usa le opzioni di connessione nella chiamata a **SQLConnect** (o le parole chiave di connessione nella chiamata a **SQLDriverConnect**) e gli attributi di connessione impostati dopo l'allocazione della connessione per determinare la connessione nel pool da usare. Per altre informazioni, vedere [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="allocating-a-statement-handle"></a>Allocazione di un handle di istruzione  
 Un handle di istruzione fornisce l'accesso alle informazioni di istruzione, ad esempio i messaggi di errore, il nome del cursore e le informazioni sullo stato per l'elaborazione di istruzioni SQL. Per informazioni generali sugli handle di istruzione, vedere [handle di istruzione](../../../odbc/reference/develop-app/statement-handles.md).  
  
 Per richiedere un handle di istruzione, un'applicazione si connette a un'origine dati e quindi chiama **SQLAllocHandle** prima di inviare istruzioni SQL. In questa chiamata, *HandleType* deve essere impostato su SQL_HANDLE_STMT e *InputHandle puntare* deve essere impostato sull'handle di connessione restituito dalla chiamata a **SQLAllocHandle** che ha allocato tale handle. Il driver alloca memoria per le informazioni sull'istruzione, associa l'handle di istruzione alla connessione specificata e passa il valore dell'handle associato in *\* OutputHandlePtr*. L'applicazione passa il valore *\* OutputHandlePtr* in tutte le chiamate successive che richiedono un handle di istruzione. Per ulteriori informazioni, vedere [allocazione di un handle di istruzione](../../../odbc/reference/develop-app/allocating-a-statement-handle-odbc.md).  
  
 Quando viene allocato l'handle di istruzione, il driver alloca automaticamente un set di quattro descrittori e assegna gli handle per tali descrittori agli attributi dell'istruzione SQL_ATTR_APP_ROW_DESC, SQL_ATTR_APP_PARAM_DESC, SQL_ATTR_IMP_ROW_DESC e SQL_ATTR_IMP_PARAM_DESC. Questi vengono definiti descrittori allocati in *modo implicito* . Per allocare un descrittore dell'applicazione in modo esplicito, vedere la sezione seguente "allocazione di un handle di descrittore".  
  
## <a name="allocating-a-descriptor-handle"></a>Allocazione di un handle di descrittore  
 Quando un'applicazione chiama **SQLAllocHandle** con un *HandleType* di SQL_HANDLE_DESC, il driver alloca un descrittore dell'applicazione. Questi vengono definiti descrittori allocati in *modo esplicito* . L'applicazione indirizza un driver all'utilizzo di un descrittore dell'applicazione allocato in modo esplicito anziché di uno allocato automaticamente per un handle di istruzione specificato chiamando la funzione **SQLSetStmtAttr** con l'attributo SQL_ATTR_APP_ROW_DESC o SQL_ATTR_APP_PARAM_DESC. Non è possibile allocare un descrittore di implementazione in modo esplicito, né specificare un descrittore di implementazione in una chiamata di funzione **SQLSetStmtAttr** .  
  
 I descrittori allocati in modo esplicito sono associati a un handle di connessione anziché a un handle di istruzione (come descrittori allocati automaticamente). I descrittori rimangono allocati solo quando un'applicazione è effettivamente connessa al database. Poiché i descrittori allocati in modo esplicito sono associati a un handle di connessione, un'applicazione può associare un descrittore allocato in modo esplicito con più di un'istruzione all'interno di una connessione. Un descrittore di applicazione allocato in modo implicito, d'altra parte, non può essere associato a più di un handle di istruzione. Non può essere associato a un handle di istruzione diverso da quello per cui è stata allocata. Gli handle descrittore allocati in modo esplicito possono essere liberati in modo esplicito dall'applicazione o chiamando **SQLFreeHandle** con un *HandleType* di SQL_HANDLE_DESC o in modo implicito quando la connessione viene chiusa.  
  
 Quando il descrittore allocato in modo esplicito viene liberato, il descrittore allocato in modo implicito viene nuovamente associato all'istruzione. (L'attributo SQL_ATTR_APP_ROW_DESC o SQL_ATTR_APP_PARAM_DESC per l'istruzione viene nuovamente impostato sull'handle descrittore allocato in modo implicito). Questo vale per tutte le istruzioni associate al descrittore allocato in modo esplicito nella connessione.  
  
 Per ulteriori informazioni sui descrittori, vedere [descrittori](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Esempio di codice  
 Vedere il [programma ODBC di esempio](../../../odbc/reference/sample-odbc-program.md), la funzione [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md), la [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)e la [funzione SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Esecuzione di un'istruzione SQL|[Funzione SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Esecuzione di un'istruzione SQL preparata|[Funzione SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Liberare un ambiente, una connessione, un'istruzione o un handle descrittore|[SQLFreeHandle Function](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Preparazione di un'istruzione per l'esecuzione|[Pagina relativa alla funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Impostazione di un attributo di connessione|[Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Impostazione di un campo del descrittore|[Funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Impostazione di un attributo Environment|[SQLSetEnvAttr Function](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
