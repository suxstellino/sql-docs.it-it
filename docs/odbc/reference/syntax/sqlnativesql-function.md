---
description: Funzione SQLNativeSql
title: Funzione SQLNativeSql | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLNativeSql
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLNativeSql
helpviewer_keywords:
- SQLNativeSql function [ODBC]
ms.assetid: b8efc247-27ab-4a00-92b6-1400785783fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7cc73e184b6b092e1de4eef40de8eaadd11d738b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160785"
---
# <a name="sqlnativesql-function"></a>Funzione SQLNativeSql
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLNativeSql** restituisce la stringa SQL modificata dal driver. **SQLNativeSql** non esegue l'istruzione SQL.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLNativeSql(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      InStatementText,  
     SQLINTEGER     TextLength1,  
     SQLCHAR *      OutStatementText,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   TextLength2Ptr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *InStatementText*  
 Input Stringa di testo SQL da tradurre.  
  
 *TextLength1*  
 Input Lunghezza in caratteri di **InStatementText* stringa di testo.  
  
 *OutStatementText*  
 Output Puntatore a un buffer in cui restituire la stringa SQL convertita.  
  
 Se *OutStatementText* è null, *TextLength2Ptr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *OutStatementText*.  
  
 *BufferLength*  
 Input Numero di caratteri nel buffer di \* *OutStatementText* . Se il valore restituito in *\* InStatementText* è una stringa Unicode (quando si chiama **SQLNativeSqlW**), l'argomento *bufferLength* deve essere un numero pari.  
  
 *TextLength2Ptr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri (esclusa la terminazione null) disponibili per restituire in \* *OutStatementText*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *bufferLength*, la stringa SQL tradotta in \* *OutStatementText* viene troncata a *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLNativeSql** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_DBC e un *handle* di *connectionHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLNativeSql** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|Il buffer \* *OutStatementText* non era sufficientemente grande da restituire l'intera stringa SQL, quindi la stringa SQL è stata troncata. La lunghezza della stringa SQL non troncata viene restituita in **TextLength2Ptr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08003|Connessione non aperta|Il *connectionHandle* non si trova in uno stato connesso.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato connesso il driver non è riuscito prima del completamento dell'elaborazione della funzione.|  
|22007|Formato DateTime non valido|**InStatementText* contiene una clausola escape con un valore di data, ora o timestamp non valido.|  
|24000|Stato del cursore non valido|Il cursore a cui si fa riferimento nell'istruzione è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati. Questo errore non può essere restituito da un driver con un'implementazione del cursore DBMS nativa.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY009|Uso non valido del puntatore null|(DM) **InStatementText* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stata chiamata una funzione in esecuzione asincrona per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) l'argomento *TextLength1* era minore di 0, ma diverso da SQL_NTS.|  
|||(DM) l'argomento *bufferLength* è minore di 0 e l'argomento *OutStatementText* non è un puntatore null.|  
|HY109|Posizione del cursore non valida|La riga corrente del cursore è stata eliminata o non è stata recuperata. Questo errore non può essere restituito da un driver con un'implementazione del cursore DBMS nativa.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver associato a *connectionHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Di seguito sono riportati alcuni esempi di ciò che **SQLNativeSql** può restituire per la stringa SQL di input seguente che contiene la funzione Scalar Convert. Si supponga che la colonna EmpID sia di tipo INTEGER nell'origine dati:  
  
```sql  
SELECT { fn CONVERT (empid, SQL_SMALLINT) } FROM employee  
```  
  
 Un driver per Microsoft SQL Server potrebbe restituire la stringa SQL tradotta seguente:  
  
```sql  
SELECT convert (smallint, empid) FROM employee  
```  
  
 Un driver per il server ORACLE può restituire la stringa SQL tradotta seguente:  
  
```sql  
SELECT to_number (empid) FROM employee  
```  
  
 Un driver per Ingres può restituire la stringa SQL tradotta seguente:  
  
```sql  
SELECT int2 (empid) FROM employee  
```  
  
 Per ulteriori informazioni, vedere [esecuzione diretta](../../../odbc/reference/develop-app/direct-execution-odbc.md) ed [esecuzione preparata](../../../odbc/reference/develop-app/prepared-execution-odbc.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
 No.  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
