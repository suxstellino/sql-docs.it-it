---
description: Funzione SQLSetConnectAttr
title: Funzione SQLGetConnectAttr | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectAttr
helpviewer_keywords:
- SQLGetConnectAttr function [ODBC]
ms.assetid: 2cb4ffa8-19d3-4664-8c2f-6682cdcc3f33
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5da83146d0c6167a08f36a648749470830795703
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171863"
---
# <a name="sqlgetconnectattr-function"></a>Funzione SQLSetConnectAttr
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetConnectAttr** restituisce l'impostazione corrente di un attributo di connessione.  
  
> [!NOTE]
>  Per ulteriori informazioni su ciò che Gestione driver esegue il mapping di questa funzione a quando un'applicazione ODBC 3 *. x* utilizza un driver ODBC 2 *. x* , vedere [mapping di funzioni di sostituzione per la compatibilità con le versioni precedenti delle applicazioni](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetConnectAttr(  
     SQLHDBC        ConnectionHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *Attributo*  
 Input Attributo da recuperare.  
  
 *ValuePtr*  
 Output Puntatore alla memoria in cui restituire il valore corrente dell'attributo specificato dall' *attributo*. Per gli attributi di tipo Integer, è possibile che alcuni driver scrivano solo la versione precedente a 32 bit o a 16 bit di un buffer e lascino invariati il bit di ordine superiore. Pertanto, le applicazioni devono utilizzare un buffer di SQLULEN e inizializzare il valore su 0 prima di chiamare questa funzione.  
  
 Se *ValuePtr* è null, *StringLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *ValuePtr*.  
  
 *BufferLength*  
 Input Se *attribute* è un attributo definito da ODBC e *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di \* *ValuePtr*. Se *attribute* è un attributo definito da ODBC e *\* ValuePtr* è un numero intero, *bufferLength* viene ignorato. Se il valore in *\* ValuePtr* è una stringa Unicode (quando si chiama **SQLGetConnectAttrW**), l'argomento *bufferLength* deve essere un numero pari.  
  
 Se *attribute* è un attributo definito dal driver, l'applicazione indica la natura dell'attributo a gestione driver impostando l'argomento *bufferLength* . *BufferLength* può avere i valori seguenti:  
  
-   Se *\* ValuePtr* è un puntatore a una stringa di caratteri, *bufferLength* è la lunghezza della stringa.  
  
-   Se *\* ValuePtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *bufferLength*. Questo inserisce un valore negativo in *bufferLength*.  
  
-   Se *\* ValuePtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *bufferLength* deve essere SQL_IS_POINTER.  
  
-   Se *\* ValuePtr* contiene un tipo di dati a lunghezza fissa, *BufferLength* è SQL_IS_INTEGER o SQL_IS_UINTEGER, in base alle esigenze.  
  
 *StringLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di byte, escluso il carattere di terminazione null, disponibile per restituire in \* *ValuePtr*. Se il valore dell'attributo è una stringa di caratteri e il numero di byte disponibili per la restituzione è maggiore di *bufferLength* , meno la lunghezza del carattere di terminazione null, i dati in *\* ValuePtr* vengono troncati a *bufferLength* meno la lunghezza del carattere di terminazione null e con terminazione null dal driver.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetConnectAttr** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato dalla struttura dei dati di diagnostica chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DBC e un *handle* di *connectionHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLGetConnectAttr** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati restituiti in \* *ValuePtr* sono stati troncati in modo da essere *bufferLength* meno la lunghezza di un carattere di terminazione null. La lunghezza del valore stringa non troncato viene restituita in *\* StringLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08003|Connessione non aperta|(DM) è stato specificato un valore di *attributo* che richiede una connessione aperta.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito dalla struttura dei dati di diagnostica dall'argomento *MessageText* in **SQLGetDiagField** descrive l'errore e la relativa origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) **SQLBrowseConnect** è stato chiamato per *ConnectionHandle* e restituito SQL_NEED_DATA. Questa funzione è stata chiamata prima che **SQLBrowseConnect** restituisse SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *connectionHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) *\* ValuePtr* è una stringa di caratteri e bufferLength è minore di zero ma non uguale a SQL_NTS.|  
|HY092|Identificatore di attributo/opzione non valido|Il valore specificato per l' *attributo* argument non è valido per la versione di ODBC supportata dal driver.|  
|HY114|Il driver non supporta l'esecuzione di funzioni asincrone a livello di connessione|(DM) un'applicazione ha tentato di abilitare l'esecuzione della funzione asincrona con SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE per un driver che non supporta le operazioni di connessione asincrona.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il valore specificato per l' *attributo* argument è un attributo di connessione ODBC valido per la versione di ODBC supportata dal driver, ma non è supportato dal driver.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver che corrisponde a *connectionHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Per informazioni generali sugli attributi di connessione, vedere [attributi di connessione](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Per un elenco di attributi che è possibile impostare, vedere [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md). Si noti che se *attribute* specifica un attributo che restituisce una stringa, *ValuePtr* deve essere un puntatore a un buffer per la stringa. La lunghezza massima della stringa restituita, incluso il carattere di terminazione null, sarà *bufferLength* byte.  
  
 A seconda dell'attributo, un'applicazione non deve stabilire una connessione prima di chiamare **SQLGetConnectAttr**. Tuttavia, se **SQLGetConnectAttr** viene chiamato e l'attributo specificato non dispone di un valore predefinito e non è stato impostato da una chiamata precedente a **SQLSetConnectAttr**, **SQLGetConnectAttr** restituirà SQL_NO_DATA.  
  
 Se l' *attributo* è SQL_ATTR_ TRACE o SQL_ATTR_ tracefile, non è necessario che *connectionHandle* sia valido e **SQLGetConnectAttr** restituirà SQL_ERROR o SQL_INVALID_HANDLE se *connectionHandle* non è valido. Questi attributi si applicano a tutte le connessioni. **SQLGetConnectAttr** restituirà SQL_ERROR o SQL_INVALID_HANDLE se un altro argomento non è valido.  
  
 Sebbene un'applicazione possa impostare gli attributi dell'istruzione utilizzando **SQLSetConnectAttr**, un'applicazione non può utilizzare **SQLGetConnectAttr** per recuperare i valori degli attributi dell'istruzione. deve chiamare **SQLGetStmtAttr** per recuperare l'impostazione degli attributi dell'istruzione.  
  
 Sia SQL_ATTR_AUTO_IPD che SQL_ATTR_CONNECTION_DEAD attributi di connessione possono essere restituiti da una chiamata a **SQLGetConnectAttr** , ma non possono essere impostate da una chiamata a **SQLSetConnectAttr**.  
  
> [!NOTE]  
>  Non è disponibile alcun supporto asincrono per **SQLGetConnectAttr**. Quando si implementa **SQLGetConnectAttr**, un driver può migliorare le prestazioni riducendo al minimo il numero di volte in cui le informazioni vengono inviate o richieste dal server.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione dell'impostazione di un attributo di istruzione|[Funzione SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Impostazione di un attributo di connessione|[Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Impostazione di un attributo Environment|[SQLSetEnvAttr Function](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
