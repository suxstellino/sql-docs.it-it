---
description: Funzione SQLGetDiagRec
title: Funzione SQLGetDiagRec | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagRec
helpviewer_keywords:
- SQLGetDiagRec function [ODBC]
ms.assetid: ebdbac93-3d68-438f-8416-ef1f08e04269
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93b5b85d6c20a58d314dcb7f7c53f391c15a1491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194176"
---
# <a name="sqlgetdiagrec-function"></a>Funzione SQLGetDiagRec
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetDiagRec** restituisce i valori correnti di più campi di un record di diagnostica che contiene informazioni su errori, avvisi e stato. A differenza di **SQLGetDiagField**, che restituisce un campo di diagnostica per chiamata, **SQLGetDiagRec** restituisce diversi campi di uso comune di un record di diagnostica, inclusi SQLSTATE, il codice di errore nativo e il testo del messaggio di diagnostica.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetDiagRec(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLCHAR *       SQLState,  
     SQLINTEGER *    NativeErrorPtr,  
     SQLCHAR *       MessageText,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   TextLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HandleType*  
 Input Identificatore del tipo di handle che descrive il tipo di handle per cui sono necessarie le diagnostica. I possibili valori sono i seguenti:  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN handle viene utilizzato solo dal driver e dalla gestione driver. Le applicazioni non devono usare questo tipo di handle. Per ulteriori informazioni su SQL_HANDLE_DBC_INFO_TOKEN, vedere [sviluppo di Connection-Pool awareness in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *Handle*  
 Input Handle per la struttura dei dati di diagnostica, del tipo indicato da *HandleType*. Se *HandleType* è SQL_HANDLE_ENV, *handle* può essere un handle di ambiente condiviso o non condiviso.  
  
 *RecNumber*  
 Input Indica il record di stato da cui l'applicazione cerca le informazioni. I record di stato sono numerati da 1.  
  
 *SQLState*  
 Output Puntatore a un buffer in cui restituire un codice SQLSTATE di cinque caratteri (e terminando NULL) per il record di diagnostica *RecNumber*. I primi due caratteri indicano la classe; i successivi tre indicano la sottoclasse. Queste informazioni sono contenute nel campo di diagnostica SQL_DIAG_SQLSTATE. Per ulteriori informazioni, vedere [Sqlstates](../../../odbc/reference/develop-app/sqlstates.md).  
  
 *NativeErrorPtr*  
 Output Puntatore a un buffer in cui restituire il codice di errore nativo, specifico dell'origine dati. Queste informazioni sono contenute nel campo di diagnostica SQL_DIAG_NATIVE.  
  
 *MessageText*  
 Output Puntatore a un buffer in cui restituire la stringa di testo del messaggio di diagnostica. Queste informazioni sono contenute nel campo di diagnostica SQL_DIAG_MESSAGE_TEXT. Per il formato della stringa, vedere [messaggi di diagnostica](../../../odbc/reference/develop-app/diagnostic-messages.md).  
  
 Se *MessageText* è null, *TextLengthPtr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *MessageText*.  
  
 *BufferLength*  
 Input Lunghezza del buffer **MessageText* in caratteri. Non esiste una lunghezza massima del testo del messaggio di diagnostica.  
  
 *TextLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri, escluso il numero di caratteri necessari per il carattere di terminazione null, disponibile per restituire in *\* MessageText*. Se il numero di caratteri disponibili per la restituzione è maggiore di *bufferLength*, il testo del messaggio di diagnostica in *\* MessageText* viene troncato in *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 **SQLGetDiagRec** non pubblica i record di diagnostica per se stesso. USA i valori restituiti seguenti per segnalare il risultato della propria esecuzione:  
  
-   SQL_SUCCESS: la funzione ha restituito correttamente le informazioni di diagnostica.  
  
-   SQL_SUCCESS_WITH_INFO: il \* buffer *MessageText* è troppo piccolo per conservare il messaggio di diagnostica richiesto. Nessun record di diagnostica generato. Per determinare se si è verificato un troncamento, l'applicazione deve confrontare *bufferLength* con il numero effettivo di byte disponibili, scritto in **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE: l'handle indicato da *HandleType* e *handle* non è un handle valido.  
  
-   SQL_ERROR: si è verificata una delle condizioni seguenti:  
  
    -   *RecNumber* è negativo o 0.  
  
    -   *BufferLength* è minore di zero.  
  
    -   Quando si usa la notifica asincrona, l'operazione asincrona sull'handle non è stata completata.  
  
-   SQL_NO_DATA: *RecNumber* è maggiore del numero di record di diagnostica esistenti per l'handle specificato nell' *handle.* La funzione restituisce inoltre SQL_NO_DATA per qualsiasi *RecNumber* positivo se non sono presenti record di diagnostica per *handle*.  
  
## <a name="comments"></a>Commenti  
 Un'applicazione in genere chiama **SQLGetDiagRec** quando una precedente chiamata a una funzione ODBC ha restituito SQL_ERROR o SQL_SUCCESS_WITH_INFO. Tuttavia, poiché qualsiasi funzione ODBC può inviare zero o più record di diagnostica ogni volta che viene chiamato, un'applicazione può chiamare **SQLGetDiagRec** dopo qualsiasi chiamata di funzione ODBC. Un'applicazione può chiamare più volte **SQLGetDiagRec** per restituire alcuni o tutti i record nella struttura dei dati di diagnostica. ODBC non impone alcun limite al numero di record di diagnostica che possono essere archiviati in un momento qualsiasi.  
  
 Impossibile utilizzare **SQLGetDiagRec** per restituire campi dall'intestazione della struttura dei dati di diagnostica. L'argomento *RecNumber* deve essere maggiore di 0. Per questo scopo, l'applicazione deve chiamare **SQLGetDiagField** .  
  
 **SQLGetDiagRec** recupera solo le informazioni di diagnostica associate più di recente all'handle specificato nell'argomento *handle* . Se l'applicazione chiama un'altra funzione ODBC, ad eccezione di **SQLGetDiagRec**, **SQLGetDiagField** o **SQLError**, le informazioni di diagnostica delle chiamate precedenti sullo stesso handle andranno perse.  
  
 Un'applicazione può analizzare tutti i record di diagnostica eseguendo un ciclo, incrementando *RecNumber*, a condizione che **SQLGetDiagRec** restituisca SQL_SUCCESS. Le chiamate a **SQLGetDiagRec** non sono distruttive per i campi di intestazione e di record. L'applicazione può chiamare nuovamente **SQLGetDiagRec** in un secondo momento per recuperare un campo da un record, purché non siano state chiamate altre funzioni, ad eccezione di **SQLGetDiagRec**, **SQLGetDiagField** o **SQLError**, nel frattempo. L'applicazione può anche recuperare un conteggio del numero totale di record di diagnostica disponibili chiamando **SQLGetDiagField** per recuperare il valore del campo SQL_DIAG_NUMBER e quindi chiamando **SQLGetDiagRec** che molte volte.  
  
 Per una descrizione dei campi della struttura dei dati di diagnostica, vedere [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md). Per altre informazioni, vedere [uso di SQLGetDiagRec e SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) e [implementazione di SQLGetDiagRec e SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 La chiamata di un'API diversa da quella che viene eseguita in modo asincrono genererà HY010 "errore della sequenza di funzioni". Non è tuttavia possibile recuperare il record di errore prima del completamento dell'operazione asincrona.  
  
## <a name="handletype-argument"></a>Argomento HandleType  
 A ogni tipo di handle possono essere associate informazioni di diagnostica. L'argomento *HandleType* indica il tipo di handle dell'argomento *handle* .  
  
 Non è possibile restituire alcuni campi di intestazione e record per gli handle di ambiente, connessione, istruzione e descrittore. Gli handle per i quali non è applicabile un campo sono indicati nelle sezioni "campi di intestazione" e "campi dei record" di [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
 Una chiamata a **SQLGetDiagRec** restituirà SQL_INVALID_HANDLE se *HandleType* è SQL_HANDLE_SENV, che indica un handle di ambiente condiviso. Tuttavia, se *HandleType* è SQL_HANDLE_ENV, *handle* può essere un handle di ambiente condiviso o non condiviso.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Recupero di un campo di un record di diagnostica o di un campo dell'intestazione di diagnostica|[Funzione SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Programma di esempio ODBC](../../../odbc/reference/sample-odbc-program.md)
